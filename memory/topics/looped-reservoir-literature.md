# Looped × Reservoir — literature scan & the gap

status: active (research thread for Shrey's loop+reservoir nanoGPT project)

## Why
User asked (2026-09-10): "has anyone worked on looped + reservoir transformer
architecture?" This is the applied-literature view of the existing theory thread
`memory/topics/ai-state-of-field.md`'s theory angle (looped × hybrid × reservoir).

## Headline answer
Yes, people have done **reservoir × transformer** and **reservoir-as-LM**, but the
**depth-looping × reservoir-memory triple combo** is **unclaimed territory** — that
gap is the contributor edge for a nanoGPT-scale run.

## Key findings (2026-09-10)
- **Echo State Transformer (EST)** — Bendi-Ouis & Hinaut (Inria/Mnemosyne),
  arXiv:2507.02917 (v3 Feb 2026). Several parallel ESN reservoirs as a **fixed-size
  working memory**; attention runs **over the reservoir units, not tokens** → linear
  complexity; per-reservoir adaptive leak rate. #1 overall in 2/5 Time Series Library
  categories (classification, anomaly detection). Timeseries model, NOT an LM, and no
  depth-looping → reservoir-attended, not looped-depth.
- **Frequency Domain Reservoir Computing (FRESCO)** — Schertler/Runge/Ceni/**Kappel**/
  Gallicchio, arXiv:2606.24969 (Jun 2026). ESN fully in the frequency domain → **O(N)**
  dense nonlinear recurrent updates (vs O(N²)); closes the reservoir state-update
  bottleneck that limited ESN scale. Note: **Kappel** = same author as RC-as-LM (in
  theory note) → reservoir-as-LM + scalable-ESN are the same community.
- **Syntactic Learnability of ES Neural Language Models at Scale** — Ueda/Kuribayashi/
  Kando/Inui, arXiv:2503.01724 (Mar 2025). Plain large-hidden-state ESN ≥ Transformer
  on grammaticality judgment at ~100M words → empirical "complexity not always needed
  for syntax" support.
- **Negative result (the point):** combined arXiv search for `"looped"+reservoir+
  language` → **zero relevant hits** (only oil-reservoir/quantum noise). Adjacent
  pieces (EST, FRESCO, ESN-LM, + the looped-transformers/Attractor line from the
  theory note) each exist in isolation; the **composition is open**.

## What this means for the project
- Design implication: reservoir as a **light-weight shared-state compressor feeding an
  otherwise-differentiable (looped) model** = the "reservoir-in-the-loop" idea already
  flagged in `knowledge/ai/looped-hybrid-reservoir-theory.md` §5. EST proves "attend
  over reservoir units" is tractable; looped/Attractor proves each half. Landing them
  together is a contribution a nanoGPT-scale run can claim.
- Follow-up (open): pull EST's full method (reservoir↔attention interaction, adaptive
  leak, positional handling) into an implementation note for the project.

## Artifacts
- knowledge/ai/looped-reservoir-hybrid-literature-2026.md (new; indexed, vault-synced 2026-09-10)
- Cross-link: knowledge/ai/looped-hybrid-reservoir-theory.md (updated to point at this)

## Next steps
- Park or extend per user direction (e.g. EST full-method note, or an ablation
  design for the looped×reservoir experiment).