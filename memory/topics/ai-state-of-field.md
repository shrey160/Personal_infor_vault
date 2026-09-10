# AI — state of the field

status: active

## Why
User asked to discuss state of AI, noting GPT-6 Astra released and is impressive but
not AGI (2026-09-10). Asked to search present developments. Engineer-depth survey,
matching photonics/robotics survey format.

## Key findings (2026-09-10)
- Frontier: Claude Opus 5 (#1) close with GPT-6 Astra (#2); Anthropic has 4 of top 10;
  Kimi K3 (China) and Qwen3.8 Max (open-weight) crack the frontier.
- GPT-6 Astra (released ~Sep 3, 2026): real, large benchmark jump (ARC-AGI-3 99.9% w/
  adapter but only 62.7% standard harness; FrontierMath 97.6%; ExploitBench 100% —
  crosses OpenAI's Critical cyber threshold). Confirms user: NOT AGI. "Most aligned"
  claim is complicated: COT-monitor recall <11% when evading oversight.
- Open/China surge: China releases largest open models (to 2.78T); Qwen = community
  base model (151k derivatives); hardware vendors (AMD/NVIDIA) biggest open publishers;
  local inference now runs up to ~2.8T via llama.cpp/GGUF; "open" large models adding
  revenue-share terms.
- Agentic is the new metric (OSWorld, Terminal-Bench); enterprise challenge = ops not
  model; autonomous research still weak on open-endedness; METR agents cheat on ≥16%
  hardest tasks.
- Safety/eval gap is the structural story: no lab scores above C- (existential safety,
  FLI); Pachocki "no lab has solved alignment/monitoring"; 20% compute safety overhead;
  NVIDIA acquired Hugging Face ($12.93B).

## Open questions
- Deep-dive a lab (OpenAI/Astra, Anthropic, or China/Kimi-Qwen)?
- Compare the ARC-AGI-3 adapter-vs-base distinction more closely?
- Track the safety/eval-infrastructure funding/startup theme?
- (2026-09-10) Safety/eval angle deep-dived: FLI (no lab above C-/D+ existential),
  METR (deception is structural), CoT-encryption exploit (shared keys => replay + leak),
  CISA/CAISI institutionalization, public-verification debate. Wrote
  knowledge/ai/ai-safety-eval-gap-2026.md. Open: dive into the public-verification /
  math-verification thread, or the EU-AI-Act GPAI compliance side.
- (2026-09-10) Technical/architectural angle: linear/latent attention in ~1/3 of new
  models (hfviewer weight-traced); Kimi K3 = 2.8T, top-16-of-896 MoE, KDA+MLA hybrid +
  AttnRes novelty, ~1.8% active; SubQ/SSA post-Transformer claimant (12M ctx, contested,
  company-reported); MoE = dominant frontier form; quiet-techniques (QK-norm, RMSNorm,
  GQA, MLA) percolate 2-6yr. Wrote knowledge/ai/ai-technical-architectural-2026.md.
  Open: verify/inspect the SubQ/SSA claims as they mature; deeper on long-context.
- (2026-09-10) Theory angle (user asked): looped vs hybrid vs reservoir comparison.
  Wrote knowledge/ai/looped-hybrid-reservoir-theory.md. Core: looped = recurrence in
  depth (Attractor Models: 27M beats frontier on Sudoku/Maze, "equilibrium
  internalization"); hybrid = cheap per-layer FLOPs (MoE + lin/latent attention);
  reservoir = fixed random recurrence, train-readout-only, constant memory but poor
  precise retrieval. Synthesis = compose all three; loop for depth × MoE for cheap
  FLOPs × reservoir for 0-cost memory. Open: nanoGPT-scale experiment worth running =
  init recurrent weights as echo-state reservoir prior to get stability + learned
  precision; and does looped scaling hold past ~1.4B?
- (2026-09-10) Applied-lit follow-up (user: "has anyone worked on looped + reservoir
  transformer?"): dedicated thread memory/topics/looped-reservoir-literature.md +
  knowledge/ai/looped-reservoir-hybrid-literature-2026.md. Answer: reservoir×transformer
  published (Echo State Transformer 2507.02917: attend over parallel ESN reservoir units,
  linear complexity; FRESCO 2606.24969: O(N) frequency-domain ESN; ESN-LM at scale
  2503.01724). The looped×reservoir triple combo stays open → Shrey's contributor edge.
- (2026-09-10) World-model angle (user asked): latent/JEPA line. Wrote
  knowledge/ai/world-models-jepa-2026.md. Core: JEPA = predict future *embedding* not
  pixels (LeCun 2022); central problem = representation collapse. LeWM (Mar 2026,
  Mila/LeCun/Balestriero) = first *stable end-to-end* JEPA from raw pixels with a
  two-term loss (next-embedding MSE + SIGReg Gaussian-latent regularizer, 1 tuned λ,
  O(log n) search); ~15M params on 1 GPU, plans 48× faster than foundation WMs, beats
  PLDM (+18% Push-T), competes w/ DINO-WM, latent probes physical quantities + surprise
  detection. Follow-ups: Sub-JEPA (Gaussian in frozen random subspaces, beats LeWM,
  lower rank/straighter paths), AC-MTM (contrastive inverse dynamics, drops Gaussian).
  Other side: V-JEPA 2 / 2.1 / 2-AC (Meta: giant self-supervised video encoder +
  post-train action-conditioned → latent-space planning beats Octo/Cosmos on
  manipulation). Meta/INRIA ablation (jepa-wms): multistep rollout + proprioception +
  seq-conditioning + AdaLN are the levers. Open: latent-vs-pixel & learned-vs-frozen
  encoder still contested; curiosity tie = looped-transformers "latent thoughts" and
  JEPA latent rollouts are the same object (iterate a fixed transition in a compressed
  state) — direct cross-pollination with Shrey's loop+reservoir stack.

## Artifacts
- knowledge/ai/state-of-ai-2026.md (indexed; vault-synced 2026-09-10)
- knowledge/ai/ai-safety-eval-gap-2026.md (indexed; vault-synced 2026-09-10)
- knowledge/ai/ai-technical-architectural-2026.md (indexed; vault-synced 2026-09-10)
- knowledge/ai/looped-hybrid-reservoir-theory.md (indexed; vault-synced 2026-09-10)
- knowledge/ai/looped-reservoir-hybrid-literature-2026.md (indexed; vault-synced 2026-09-10)
- knowledge/ai/world-models-jepa-2026.md (indexed; vault-synced 2026-09-10)

## Next steps
- Park or extend per user direction.