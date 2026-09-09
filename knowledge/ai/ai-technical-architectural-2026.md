# Technical & Architectural Advances in AI/ML — Sept 2026 survey

_Compiled 2026-09-10 for Shrey. Present date confirmed 2026-09-10. This covers the
*under-the-hood* side of the state-of-AI survey: model architecture, scaling
efficiency, attention innovation, MoE sparsity, and the compute/hardware layer.
Bias note: several strong sources (hfviewer, Kimi K3) are weight-traced data — high
trust for *adoption curves*; vendor benchmark claims (SubQ) carry the usual
unverified asterisk._

---

## One-paragraph answer

The Transformer is no longer the default recipe — it's being **edited from the
inside**, layer by layer, rather than replaced wholesale. Three parallel shifts
define 2026: (1) **attention is going sub-quadratic** — linear + latent-attention
layers are now in roughly a third of new models (and a whole post-Transformer
architecture, SubQ/SSA at 12M-token context, made a contested splash); (2) **sparse
Mixture-of-Experts is the dominant frontier-scale form** — Kimi K3 ships top-16 of
**896 experts** at 2.8T params, and an emerging "sparsity race" keeps shrinking the
active fraction; and (3) **scaling efficiency, not raw scale, is the new currency** —
Kimi K3 claims ~2.5× better scaling efficiency than its predecessor via architectural
bets rather than just more params. Underneath it all, small "quiet" innovations
(QK-norm, RMSNorm, GQA, MLA) that percolate from paper to default over 2–6 years.

---

## 1. The headline attention shifts (weight-traced data)

Hannes von Essen's hfviewer traces 2,400+ models from *actual weights* (not model
cards) and reports adoption curves per concept. The five-story overview
([post](https://huggingface.co/blog/embedl/architecture-trends-over-5-years)):

| Concept | Origin → default | Adoption now |
|---|---|---|
| **Multi-head Latent Attention (MLA)** | DeepSeek-V2, May 2024; DeepSeek specialty ~1.5 yr | ~23% of new models (V4, GLM-5.2, Kimi K2.7, LongCat 2.0, Mistral MoE) |
| **QK-Norm** | proposed Oct 2020, forgotten; revived for scaling stability (Qwen3, Gemma3) | >50% of new models |
| **Grouped-Query Attention (GQA)** | May 2023 → Llama 2 & Mistral within months (fastest sprint) | ~2/3 of new models |
| **RMSNorm** | 2019 simplification; Llama made it the OS default 2023; curves crossed 2024 | ~9 of 10 new models |
| **Linear attention** | 2020 ("Transformers are RNNs"); on shelf 5 yrs | ~1/3 of new models via hybrid interleave (Qwen3.5/3.6) |

**The meta-pattern:** the biggest ideas take *years* to percolate, then go from a
singular lab to default near-simultaneously. MLA sat as a DeepSeek specialty for 18
months before jumping to 23% in a single quarter. The modern frontier stack is a
*layered accretion* of these — the "architecture of record" is increasingly
**hybrid**: a few full-attention layers for lossless global retrieval + a majority
of cheaper linear/latent layers for the bulk.

→ [architecture trends over 5 years](https://huggingface.co/blog/embedl/architecture-trends-over-5-years),
[hfviewer glossary](https://hfviewer.com/glossary/)

---

## 2. The concrete embodiment: Kimi K3 (2.8T, Moonshot, Jul 2026)

The best case study of *where* frontier architecture is going — and the first open
3T-class model ([Kimi K3 preview](https://huggingface.co/blog/embedl/kimi-k3-preview)
= weight-grounded *before* the Jul 27 release; specs from the launch post).
Moonshot credits **~2.5× better scaling efficiency than K2 — via architecture, not
just param count.** Four ideas carry it:

1. **Kimi Delta Attention (KDA) — the workhorse.** A delta-rule / linear-attention
   variant: short convolutions, L2-normalized keys, gated state updates instead of a
   quadratic matrix. **3 of every 4 attention layers are KDA.** (Echo of the
   linear-attention trend above, pushed far harder.)
2. **Gated MLA — the precise quarter.** Every **4th** layer is full multi-head latent
   attention with output gating — keeps *global, lossless retrieval* in the stack
   while KDA does the cheap bulk. The "hybrid" answer to the linear-vs-full tradeoff.
3. **Attention Residuals (AttnRes) — genuinely novel.** Blocks *selectively retrieve*
   representations from earlier depths (learned α operators) instead of each block
   stacking onto one residual stream. "Attention over depth, not just sequence."
   No open model traced so far does this — a genuinely new shape on the graph.
4. **Stable LatentMoE — the sparsity bet.** Feed-forward routes **top-16 of 896
   experts** (largest open pool shipped), with *Quantile Balancing* (router-score
   quantiles instead of an auxiliary loss), a *SiTU* activation, *Per-Head Muon*
   optimizer. Active share ≈ **1.8%**.

Context: MLA had its breakout quarter (23% of new models); K3 pairs it *with* linear
attention rather than choosing. The "sparsity race" — expert pools grow while active
fraction falls — is unmistakable in the trace data (recent frontier MoEs ~few-% active;
K3 at 1.8%, pool 896). Moonshot candidly says K3 "still trails the most powerful
proprietary models" overall while leading all other open models.

---

## 3. The "overthrow the Transformer" claimant: SubQ / SSA (contested)

The most dramatic 2026 architecture story — and the one to treat most skeptically
([36kr, May 6 2026](https://eu.36kr.com/en/p/3797755244157959)):

- **Subquadratic** (Miami; 13 employees, $29M seed @ $500M val) shipped **SubQ**, the
  first model on a **Subquadratic Sparse Attention (SSA)** architecture:
  - **Content-dependent sparse routing** — select the meaningful positions per query,
    skip ~99% of the attention computations (vs dense token-to-token).
  - Claims **12M-token context**; FLOPs reduced ~1000× at 12M tokens (62.5× at 1M);
    52.5× faster than FlashAttention-2 at 1M tokens.
  - Cost: $8 on RULER-128K vs Opus's $2,600 (~300×); claims parity-or-better accuracy
    (RULER 95.0 vs Opus 94.8; SWE-Bench 81.8 vs 80.8).
- **Reception:** split — "biggest breakthrough since Transformer" vs "the Theranos of
  AI." Bindu Reddy's hyperbole ("valuations of Anthropic/OpenAI go to zero") is on
  record. **Critical caveat:** these are the *company's own* claims, not independently
  reproduced; long-context retrieval (MRCR-v2 65.9%) still trails Opus (78%). Treat
  SSA as an important *direction* (sparse content-dependent attention is real and
  aligns with the linear-attention trend) but the specific numbers are unverified and
  possibly marketing-inflated.

Framing: SSA is a *radical* version of the same "stop computing all pairs" impulse
that linear-attention/latent-attention hybrids implement *incrementally*. The field
is hedging: conservatively via hybrid transformers (proven, adopted), and
aggressively via post-Transformer bets like SSA.

---

## 4. Frontier model scale & the systems layer

- **MoE is the dominant frontier-scale form** — the automated ARC survey notes
  "the mixture-of-experts architecture has become the overwhelmingly dominant paradigm
  for frontier-scale models." Kimi K3 (896 experts, 1.8% active) and Qwen3.8 2.4T /
  DeepSeek-V4 / GLM-5.2 track it. → [ARC survey](https://arxiv.org/html/2306.02781v4)
- **Context is being attacked two ways:** (a) by pushing sparse/linear attention to
  make long context *affordable* (SubQ 12M; Kimi K3 1M native; Inkling 1M), and (b) by
  the agentic/OS-use workloads that actually *need* it. Long-context retrieval (MRCR)
  remains the honest bottleneck.
- **Hardware/rack-scale is part of "architecture"** — the AMD/Schneider Helios
  reference design and NVIDIA's engine push (with HugoFace acquisition) suggest the
  datacenter/rack is now a co-designed layer with the model. → [ABI Research](https://www.abiresearch.com/market-research/insight/7788238-advancing-ai-2026-amd-and-schneider-electr?hsLang=en),
  [AMD Helios](https://newsroom.amd.com/news/aai-2026-helios-update/)
- **Test-time compute is its own architectural axis:** parallel coordinated reasoning /
  PaCoRe and implicit test-time scaling (Recurrent-Depth VLA) are active areas — compute
  spent *at inference* across multiple coordinated reasoning paths is increasingly
  treated as a first-class architectural variable, not a post-hoc trick.
- **Architecture search is becoming agentic:** HARMONY (large-scale hybrid search) and
  AIRA-Compose/AIRA-Design (agentic discovery of neural architectures) point at
  automating the very design decisions above. → [HARMONY](https://ieeexplore.ieee.org/document/11520487),
  [AIRA](https://ar5iv.labs.arxiv.org/html/2605.15871)

---

## 5. The quiet-convergence thesis (what an engineer should internalize)

Reading across the trends, the architectural "best practice" of 2026 is a **hybrid,
nominal-transformer** that:

- uses **RMSNorm** + **QK-norm** for stable scaling,
- interleaves **~25–33% full-attention layers** (MLA-based for KV savings) with
  **~66–75% linear/low-rank attention** for cost,
- routes through a **sparse MoE** with a large expert pool and tiny active fraction
  (KDA/MLA + 896-expert MoE in K3 = the extreme composite),
- adds **selective residual/retrieval** (AttnRes) to break the "residual stream as
  the only highway" bottleneck,
- and increasingly spends **test-time compute** (parallel/recurrent reasoning) as a
  scaling lever rather than only more weights.

The structural themes: **sparsity everywhere** (sparse attention, sparse experts),
**mixing cheap + precise** (linear + full), and **measuring scaling efficiency, not
raw size**. Whoever *reduces cost per capability* — whether via SSA-claims, K3-style
composites, or MoE sparsity — owns the margin, which is the same
model-layer-economics thesis from the rest of this survey.

---

## Sources
- hfviewer — Architecture trends over 5 years: https://huggingface.co/blog/embedl/architecture-trends-over-5-years
- hfviewer — Kimi K3 preview: https://huggingface.co/blog/embedl/kimi-k3-preview (official: https://www.kimi.com/blog/kimi-k3)
- 36kr — SubQ / SSA (company-reported, contested): https://eu.36kr.com/en/p/3797755244157959
- ARC survey — MoE dominant frontier paradigm: https://arxiv.org/html/2306.02781v4
- HARMONY (hybrid architecture search): https://ieeexplore.ieee.org/document/11520487
- AIRA-Compose/AIRA-Design: https://ar5iv.labs.arxiv.org/html/2605.15871
- PaCoRe (parallel coordinated reasoning): https://aclanthology.org/2026.acl-long.1253/
- Recurrent-Depth VLA (implicit test-time compute): https://www.semanticscholar.org/paper/Recurrent-Depth-VLA-a8e59afe9d/18c56e2b9e54518db0f5ad2cd2873269813e7fcc
- AMD Helios rack-scale: https://newsroom.amd.com/news/aai-2026-helios-update/

_Unverified: SubQ/SSA performance numbers are the company's own claims, not
independently reproduced. Kimi K3 specs/benchmarks are Moonshot-reported (some 3rd-
party cited); hfviewer adoption-curves are weight-traced (high confidence on trend
direction). Dense-vs-sparse speedups (SubQ 52.5×) not audited._