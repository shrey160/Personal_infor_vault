# World Models — the latent/JEPA line, 2026 state of the field

_Compiled 2026-09-10 for Shrey. Present date confirmed 2026-09-10. This is the
world-models angle of the AI survey, and it sits directly on Shrey's research stack
(looped transformers + reservoir hybrids, KV-cache/context-window memory math). It
continues the theory thread in
[`knowledge/ai/looped-hybrid-reservoir-theory.md`](looped-hybrid-reservoir-theory.md)
and the robotics survey in
[`knowledge/robotics/state-of-robotics-2026.md`](../robotics/state-of-robotics-2026.md).
Engineer-depth; primary sources cited inline.

---

## What a world model is, and the two competing bets

**"World model" (WM)** = a model that builds an internal representation of observed
dynamics and can *simulate the future conditioned on actions*, so an agent can plan /
decide / generate synthetic experience *in imagination* rather than in the real
environment. The 2024–2026 narrative generalizes WM beyond the classic Ha &
Schmidhuber latent-MBRL notion: it is now the hub connecting video generation, VLA
policy learning, autonomous-driving simulation, and 3D scene generation.

The field splits on **where the future is predicted**:

- **Pixel-space (generative) WMs** — predict future *frames* (diffusion / next-frame).
  Workhorse: Genie, DIAMOND, GameNGen, Cosmos, HunyuanWorld, DreamGen. Expensive to
  run, but directly renderable and usable as "general-purpose interactive simulators."
- **Latent-space (JEPA-style, non-generative) WMs** — predict a *compact embedding* of
  the future, not pixels. This is the Jožica/LeCun bet, and the focus here. Much
  cheaper (a few orders of magnitude fewer tokens to simulate), but the latent must be
  learned so that *relevant* structure survives the compression.

> **The core JEPA reframe (LeCun 2022):** don't reconstruct the world; predict the
> *embedding* of a future observation from the embedding of the current one. This
> "predict v1's encoding from v0's encoding" is the Joint-Embedding Predictive
> Architecture. The abstract claim: predicting future *latch* features is enough to
> build a world model and plan, without modeling irrelevant pixel detail. → [AMI
> position paper](https://openreview.net/pdf?id=BZ5a1r-kVsf)

_Both_ bets are being pursued hard through 2026; the key contested question is whether
latent WMs can match or beat pixel WMs on real control once you control for compute.

---

## The central technical problem: **representation collapse**

Train a JEPA with *only* the prediction loss and the encoder can collapse: map every
input to (nearly) the same vector, making the prediction loss trivially zero *and*
the representation useless. Collapse-avoidance is *the* stability problem of the
latent line, and it is what distinguishes the methods:

| Method family | How it avoids collapse | Cost / fragility |
|---|---|---|
| PLDM (end-to-end) | VICReg + multi-term loss (≈6–7 terms) | **Unstable**, hard hyperparameter search O(n⁶) |
| DINO-WM / V-JEPA-2 (foundation) | **Freeze** a pretrained vision encoder + EMA; train the predictor on top | Stable, but representation limited to the frozen encoder; heavy |
| **LeWM (2026)** | **Single** anti-collapse regularizer (SIGReg → Gaussian latents) | Stable, principled, 1 tuned hyperparameter O(log n) |

The trade the field is circling: **do you learn the encoder (and risk collapse /
instability) or reuse a foundation encoder (and give up end-to-end expressivity)?**
LeWorldModel (Mar 2026) is the first to make the *learned-encoder* route stable with a
principled two-term loss.

---

## LeWorldModel (LeWM) — the 2026 end-to-end landmark

**[LeWorldModel: Stable End-to-End Joint-Embedding Predictive Architecture from
Pixels](https://arxiv.org/abs/2603.19312)** — Maes, Le Lidec, Scieur, LeCun,
Balestriero (Mila / Montréal, NYU, Samsung SAIL, Brown), Mar 2026. Code
[`flyingGH/le-wm`](https://github.com/flyingGH/le-wm), checkpoints/data on
[HuggingFace](https://huggingface.co/collections/quentinll/lewm).

### The two-term loss (the whole contribution in one equation)

```
Ł = Ł_pred + λ · SIGReg(Z)

Ł_pred      = ‖ ẑ(t+1) − z(t+1) ‖²        (next-embedding prediction, teacher-forced)
SIGReg(Z)   = (1/M) Σ_m  T( h^(m) )        (sketched isotropic Gaussian regularizer)
              h^(m) = Z · u^(m), u ∈ S^(d−1) random projections, T = Epps–Pulley normality stat
```

- **SIGReg** projects the latent batch onto `M = 1024` random directions and drives each
  1-D marginal toward Gaussian via the Epps–Pulley test; by Cramér–Wold, matching all
  marginals ≈ matching the full distribution → isotropic Gaussian latents. This is the
  *sole* anti-collapse term; only `λ = 0.1` is tuned (bisection, O(log n)).
- Versions PLDM's **6–7 loss terms → 1** and its polynomial search → O(log n).
- **No EMA, no stop-gradient, no pretrained encoder, no reconstruction, no reward.**
  Fully end-to-end, reward-free, offline.

### Architecture & economics (the part that matters to an inference-engine researcher)

- **~15M params total** (ViT-tiny encoder ~5M + prediction transformer ~10M), trainable
  on a **single GPU in a few hours**.
- Latent is a single `[CLS]`-token + 1-layer MLP (with **BatchNorm**, deliberately — the
  final ViT LayerNorm would defeat the anti-collapse objective). Predictor uses
  **AdaLN action conditioning** (zero-initialized so actions ramp in progressively).
- Planning via **MPC + CEM**: encode initial+goal frames, roll the predictor forward in
  latent space, minimize ‖ẑ_H − z_goal‖² over action sequences, re-plan after K steps.
- **Planning up to 48× faster** than foundation-based WMs (single latent token ≈ ~200×
  fewer tokens than DINO-WM); full plan in <1 s — narrowing toward real-time control.

### Results & physical-understanding claims

- Beats **PLDM** (end-to-end rival) by up to **+18% success on Push-T**; **competitive
  with DINO-WM**, and *exceeds DINO-WM-with-proprioception on Push-T using pixels-only*.
- Weaker on the trivial **Two-Room** env — the isotropic-Gaussian prior fights very
  low-intrinsic-dimensionality data (the honest weakness that Sub-JEPA targets).
- **Probing**: the latent linear-decodes physical quantities (block position, joint
  angle). **Surprise/violation-of-expectation**: the model flags physically-impossible
  trajectories — evidence the latent actually encodes intuitive causal/physical
  structure, not just "a compressible future."

---

## The 2026 architecture-follow-up lineage (these build directly on LeWM)

The Gaussian-latent regularization — LeWM's key move — became a research front itself
in months:

- **Sub-JEPA** ([arXiv:2605.09241](https://arxiv.org/abs/2605.09241), May 2026):
  LeWM's *global* isotropic-Gaussian prior is "too strong" — real latents live on a
  low-dim manifold inside a high-dim ambient space. Sub-JEPA applies the Gaussian
  constraint inside `K=32` frozen **random subspaces** instead of the whole ambient
  space → relaxes the prior, keeps anti-collapse. **Consistently beats LeWM** across
  four control envs (e.g. Two-Room 84.3→95.0%, OGB-Cube 67.3→76.3%) with **lower
  effective rank, straighter latent trajectories, less open-loop drift**. Code
  [`intcomp/Sub-JEPA`](https://github.com/intcomp/Sub-JEPA).
- **No Gaussian Required / AC-MTM** (Boylan & Hokamp,
  [arXiv:2608.17542](https://arxiv.org/abs/2608.17542), 18 Aug 2026): attacks the
  *premise* that anti-collapse needs a distributional prior at all. Thesis: LeWM's
  SIGReg prevents collapse by *prescribing* what the latent must look like
  (isotropic Gaussian), independent of the environment — instead, the pressure should
  come from the **transition data itself**. Mechanism: (1) keeps LeWM's forward
  next-embedding MSE objective + CEM planning untouched; (2) adds a **training-only
  inverse-dynamics head** trained with **Action-NCE** — each latent transition
  `(z_t→z_{t+1})` must identify which action in the batch produced it, a
  discrimination task a **collapsed encoder provably fails** (constant latents ⇒ no
  action-specific structure to discriminate); (3) **discards the inverse head after
  training**, so test-time encoding/prediction/planning/compute are *identical to
  LeWM*. Results: matches SIGReg on average across four standard pixel-control tasks
  with stable from-scratch training; on the harder multi-object **OGBench Visual Scene**
  the *prescribed geometry becomes a bottleneck* — **80.0±2.0% vs 58.0±2.0% success
  (20–24 pt/seed), 52% random baseline**. Distribution-free anti-collapse with **no
  target network, no stop-gradient, no pretrained encoder, no reconstruction**; paper
  also **characterizes the action-space and observability assumptions** under which the
  contrastive signal is sound. Code
  [`jackboyla/action-contrastive-jepa`](https://github.com/jackboyla/action-contrastive-jepa).
- **Rectified LpJEPA** (sparse, nonnegative, maximum-entropy representations) — pushes
  the same "what structural prior prevents collapse *best*" question.

**Engineer takeaway:** the collapse-prevention mechanism became a first-class research
axis. Shrey's reservoir/looped framing maps cleanly: LeWM ≈ predictor-in-latent-space
(learned recurrence over latents); Sub-JEPA ≈ *structuring* the state space the
recurrence lives in; both are the same "cheap latent dynamics + anti-collapse prior"
recipe Shrey's hybrid stack explores at nanoGPT scale.

---

## The Meta/INRIA ablation — what *actually* makes JEPA-WMs work

**[What drives success in physical planning with JEPA world models?](https://arxiv.org/abs/2512.24497)**
(Bardes/Terver/Yang/Ponce/LeCun, Dec 2025; code
[`facebookresearch/jepa-wms`](https://github.com/facebookresearch/jepa-wms)) — a
systematic ablation of the *JEPA-with-pretrained-encoder* line (DINO-WM, V-JEPA-2-AC).
Their recipe that beats both baselines on Metaworld / Push-T / Robocasa:

- **Multi-step rollout loss** (predict k steps, backprop truncated, not just 1-step
  teacher-forcing) — the single biggest lever.
- **Proprioception helps** for manipulation, hurts or neutral elsewhere; worth
  *including as a learned token + loss term*, not just input.
- **Action conditioning**: sequence-conditioning (actions as tokens, RoPE) beats
  feature-concatenation for the predictor; **AdaLN** (action affects all layers) is the
  cleanest.
- **Longer training context (W=3→7)** helps the predictor unroll farther.
- **Frozen DINOv2/DINOv3 local features** remain the strongest frozen-encoder base vs
  trained-from-scratch encoders — at the cost of the end-to-end expressivity LeWM
  buys back.

> Note the tension this exposes: the two traction routes (LeWM's *learned* encoder +
> Gaussian prior vs the jepa-wms *frozen* foundation encoder + heavier training) are
> both converging on the same recipe (autoregressive latent prediction, multi-step,
> good action conditioning, MPC+CEM planning). Which encoder wins is still open.

---

## V-JEPA 2 / V-JEPA 2.1 / V-JEPA 2-AC — the foundation-scale latent WM

**[V-JEPA 2](https://arxiv.org/abs/2506.09985)** (Meta FAIR, Jun 2025) + **V-JEPA 2.1**
([arXiv:2603.14482](https://arxiv.org/abs/2603.14482), Mar 2026) + **V-JEPA 2-AC**
(action-conditioned). This is the *other* side of the latent line: scale up the encoder
rather than learn it from scratch.

- **V-JEPA 2**: self-supervised *video* model (masked latent-feature prediction),
  decoder-free; ViT-L/H/g (0.3B–1B). SOTA on motion understanding & action
  anticipation without any task labels (EK100 39.7%, SSv2 77.3% probe, Diving48 90.2%).
- **V-JEPA 2-AC**: *post-trained* from V-JEPA 2 with a small amount of robot-trajectory
  data (DROID) → latent action-conditioned world model. Solves Franka-arm manipulation
  (reach/grasp/pick-and-place) from monocular RGB **by latent-space planning** —
  beating Octo and Cosmos on grasp/pick-and-place without env-specific training.
- **V-JEPA 2.1**: a better recipe for **temporally consistent dense features** (Dense
  Predictive Loss over *all* tokens incl. masked, deep self-supervision, multi-modal
  tokenizers, scaling).

**Where LeWM and V-JEPA sit:** V-JEPA 2-AC is the "use a giant pretrained video encoder
+ tiny post-train" exemplar; LeWM is the "train a tiny encoder end-to-end on the data
you actually have" exemplar. The Meta/INRIA ablation treats DINO-WM / V-JEPA-2-AC as
the frozen-encoder baselines that LeWM competes *against* on the same control suites.

---

## Where the field actually is (2024 → 2026 trajectory, per survey)

Per the 2024-26 world-model survey ([World Model — A 2024–2026
Survey](https://liqing.io/mindflow/Topics/WorldModel-Survey), Jun 2026):

- **Timeline**: Genie (2024-02) → DIAMOND (2024-05) → GameNGen (2024-08) → Cosmos
  (2025-01) → UWM (2025-04) → **V-JEPA 2 (2025-06)** → DreamGen (2025-05) → Motus/
  DreamZero (2025-12/2026-02) → World-VLA-Loop/GigaBrain (2026-02) → **OpenWorldLib &
  HY-World 2.0 (2026-04, open-source ecosystem matures)**.
- **Industrial map**: NVIDIA (Cosmos/CosmosReason/DreamGen/DreamZero), Google DeepMind
  (Genie), Meta FAIR (V-JEPA); CN: Tencent Hunyuan, GigaAI (GigaBrain), AgiBot; driving:
  OpenDriveLab/Wayve (Vista, GAIA-1, Drive-WM); robotics: ETH (RWM), HKUST/ByteDance
  (IRASim), UW/TRI (UWM).
- **The five WM×VLA couplings — all now empirically validated**: offline data engine →
  inference-time latent conditioning → joint model → RL simulator → evaluator.
- **Latent vs pixel entered a comparable era**: V-JEPA 2 claims a ~15× compute advantage
  over pixel (Cosmos) plus success-rate superiority on zero-shot planning — the first
  time latent genuinely threatens pixel at matched compute.
- **Redefinition/convergence (OpenWorldLib, 2026-04)**: text-to-video (Sora) is *excluded*
  from "world model"; the core objective is the actionable
  "perceive → maintain state → simulate-action-consequences → decide" loop. WM's goal
  migrated from "realistic video" to "controllable dynamics usable as an agent
  backend" — with DreamGen-Bench / WorldSimBench / Physics-IQ giving the video
  community robotics-useful proxy metrics.

**Still-open problems (endorsed across the survey and the papers above):** long-horizon
imagination drift, physics alignment, and generalizing across the latent/pixel and
learned/frozen-encoder divides. No single method solves all three.

---

## How this connects to Shrey's research stack

- **Latent prediction = the recurrence Shrey already studies.** LeWM's predictor over
  latent states is a *learned recurrent dynamical system in embedding space*. The
  "latent thoughts" of looped transformers (CoT-in-depth) and LeWM's latent rollouts are
  the same object: iterating a fixed transition in a compressed state space. Looped
  transformers *reason* in depth over token-streams; JEPA-WMs *predict* in depth over
  latent states. Cross-pollination is direct: a looped predictor block (tied weights)
  is precisely what a cheap latent dynamics model wants — Shrey's loop + reservoir prior
  could anti-collapse-init the way Sub-JEPA structures the latent.
- **Reservoir tie-in.** A reservoir's *fixed recurrent state = cheap latent dynamics*,
  and Sub-JEPA's "Gaussian in subspaces" is a cousin of restricting the recurrent state
  to low intrinsic dimension. The "capacity-0-cost memory compressor" idea from the
  theory thread is exactly the kind of latent that a JEPA-WM wants to predict and that
  a reservoir provides for free.
- **KV/context math.** JEPA-WM latent rollouts are *constant-state* (one latent per
  frame, no growing KV per token) — the "cheapest long-horizon" case in Shrey's
  memory-math taxonomy, directly comparable to reservoir remembering and opposite a
  growing KV cache.

---

## Sources (primary first)
- LeWorldModel (arXiv:2603.19312, Maes/Le Lidec/Scieur/LeCun/Balestriero, 2026-03):
  https://arxiv.org/abs/2603.19312 — full text used for loss/arch/results above.
- LeWM code: https://github.com/flyingGH/le-wm ; checkpoints/data:
  https://huggingface.co/collections/quentinll/lewm
- Sub-JEPA (arXiv:2605.09241, May 2026): https://arxiv.org/abs/2605.09241 ; code
  https://github.com/intcomp/Sub-JEPA
- No Gaussian Required / AC-MTM (arXiv:2608.17542, Aug 2026):
  https://arxiv.org/abs/2608.17542 ; code https://github.com/jackboyla/action-contrastive-jepa
- JEPA-WM recipe ablation (arXiv:2512.24497, Dec 2025):
  https://ar5iv.labs.arxiv.org/html/2512.24497 ; code
  https://github.com/facebookresearch/jepa-wms
- V-JEPA 2 (arXiv:2506.09985, Jun 2025): https://arxiv.org/abs/2506.09985 ; code/README
  https://github.com/facebookresearch/vjepa2 ; V-JEPA 2.1 (arXiv:2603.14482)
- LeCun, "A Path Towards Autonomous Machine Intelligence" (AMI, 2022):
  https://openreview.net/pdf?id=BZ5a1r-kVsf
- World Model — A 2024–2026 Survey (Jun 2026):
  https://liqing.io/mindflow/Topics/WorldModel-Survey

_Unverified/caveat: performance numbers (48×, +18%, Sub-JEPA deltas, V-JEPA-2-AC vs Octo/
Cosmos) are from the authors' own papers/ABIs at time of writing; no independent frontier
reproduction is available for the LeWM-line control results. The "surprise = physical
understanding" claim is the authors' interpretation of a violation-of-expectation probe,
not ground truth. LeWM's Two-Room weakness is acknowledged in-paper. Treat cross-family
competitive claims (latent-vs-pixel, learned-vs-frozen encoder) as directionally
informative, contested.