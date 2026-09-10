# Looped × Reservoir — the published hybrids and the gap (2026)

_Companion to `ai/looped-hybrid-reservoir-theory.md` (the theory spine). This note
records the applied literature found 2026-09-10 for Shrey's loop+reservoir project:
what has actually been published, and the combination that hasn't._

_Context: `web_search` was out-of-balance (HTTP 402) this session, so the scan used
arXiv's own full-text search via direct fetch — the finds below are abstract-level,
verified against the arXiv pages._

---

## The one-line answer

**Reservoir × transformer** (Echo State Transformer; Frequency-domain ESN) and
**reservoir-as-language-model** (ESN-LM at scale) are published. The
**depth-looping × reservoir-memory composition** — loop a transformer/attractor stack *and*
use a reservoir as the constant-cost shared-state memory — has **no dedicated prior
art**. That unclaimed middle is the contribution surface for a nanoGPT-scale run.

---

## 1. Echo State Transformer (EST) — reservoir units you attend *over*

Bendi-Ouis & Hinaut (Inria / Mnemosyne), **arXiv:2507.02917** (v1 Jun 2025, v3 Feb 2026).

- **Idea**: several ESN reservoirs run in parallel as a **fixed-size working memory**
  (instead of an unbounded token history). Attention operates **over that fixed set of
  reservoir units**, not over input tokens.
- **Why it matters:** adds *linear* sequence complexity, breaking the transformer's
  quadratic scaling; each reservoir gets its **own adaptive leak rate**, so units learn
  distinct temporal sensitivities.
- **Results:** #1 overall in **2 of 5** Time Series Library categories (69 tasks) —
  classification and anomaly detection; competitive on short-term forecasting.
- **Scope caveat:** evaluated on **timeseries**, not language; and it is
  reservoir-as-memory-being-attended, **not** an iterative depth loop. So EST proves the
  "reservoir you selectively pull from" mechanism — a piece, not the whole.

## 2. FRESCO — scaling the reservoir itself to transformer scale

- Schertler, Runge, Ceni, **Kappel**, Gallicchio, **arXiv:2606.24969** (Jun 2026).
- **Idea:** an echo-state network run **entirely in the frequency domain** (novel
  zero-padding input embedding, packed readout, native frequency-domain nonlinearity).
- **Why it matters:** removes the ESN's **O(N²) state-update** bottleneck → **O(N)**
  dense non-linear recurrent updates, enabling reservoirs at recurrent-model scale;
  matches SOTA on memory / sequential-classification / multivariate forecasting.
- **Connections:** **David Kappel** co-authors this and *RC-as-Language-Model* (already
  cited in `looped-hybrid-reservoir-theory.md`) — so the "reservoir as a real LM" line
  and the "make ESNs scale" line are converging in the same community.

---

## 3. Reservoir-as-LM, at scale

- **Syntactic Learnability of Echo State Neural Language Models at Scale** — Ueda,
  Kuribayashi, Kando, Inui, arXiv:2503.01724 (Mar 2025). A plain, large-hidden-state
  ESN is **comparable-or-superior to a Transformer** on grammaticality-judgment when
  trained on ~100M words. Evidence for the theory-note claim that transformer-grade
  complexity is not always required for structure.

---

## 4. The gap (why your project is on unclaimed ground)

Direct arXiv scan for `"looped"+reservoir+language` returned **zero relevant results**
(only oil-reservoir / quantum-reservoir noise). The adjacent pieces each exist:

| Piece | Published |
|---|---|
| Transformer × reservoir (attend over reservoirs) | EST (2507.02917) |
| Reservoir state bottleneck fixed (scale) | FRESCO (2606.24969) |
| Reservoir alone as LM at scale | Ueda et al (2503.01724), RC-as-LM (Kappel) |
| Depth-looping / recursive transformer (iterative depth) | Looped-LM (Saunshi ICLR'25), Attractor Models — in theory note |
| **looping × reservoir (loop the stack, reservoir as shared memory)** | **— open —** |

The synthesis the theory note proposed — *loop for depth × sparse/MoE for cheap FLOPs ×
reservoir for constant-memory* — is exactly the triple no single paper covers.

---

## Sources
- EST: https://arxiv.org/abs/2507.02917
- FRESCO: https://arxiv.org/abs/2606.24969
- ESN-LM at scale: https://arxiv.org/abs/2503.01724
- (theory spine) `knowledge/ai/looped-hybrid-reservoir-theory.md` → looped-lm /
  Attractor / RC-as-LM refs there.

_Note: abstract-level, arrowed sources filtered to the primarily AI-relevant hits;
FRESCO/EST link-ids are from the arXiv pages read directly this session. Contact: treat
"#1 leaderboard" claims as self-reported baseline-relative results._