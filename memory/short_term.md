# Short-term Memory — session staging area

_Budget ~100 lines. Consolidate at session end; then reset to this header._

## This session (2026-09-10)
- obsidian-mcp: on (bridge bound at boot this session; `mcp__obsidian__*` present)
- Session resume: user asked "what did we do last". Recap only; no new research yet.
- **Vault sync FAILED again (2026-09-10)**: user asked to add to vault; all `mcp__obsidian__*`
  calls (vault_list, search_simple, root vault_list) errored `Session not found`. Local REST
  API reachable on 127.0.0.1:27123 but bare probe = `authenticated: False`. Hypothesis: plugin
  API key/session changed since the DSH profile patch; bridge binds once at boot with stale key.
  Fix = user checks plugin token vs `~/.dsh/profiles/web/cordis.patch.yml`, then restart `dsh web`.
  No `vault-synced` markers written. Still pending: robotics artifact + all ai/ artifacts.
- New research direction (user, from a different chat's context): **laser processing of
  poly-Si** (PV + photonics). Direction-only, NOT facts — treats incoming claims as
  leads to verify (`verify:` labels throughout). Wrote
  `knowledge/photonics/laser-poly-si-2026.md`, indexed it, opened
  `memory/topics/semiconductor-material-science/laser-poly-si.md` (new
  semiconductor/material-science topic subfolder). Vault duty ON but bridge down →
  index line left unmarked for a future ON session.
- **Verification COMPLETE (2026-09-10)**: ran 4 parallel claims-verification subagents (one
  per front) on laser-poly-si. All ~15 leads resolved to VERIFIED/PARTIAL/CONTRADICTED with
  DOIs + corrected numbers. Key corrections: LECO origin = CE Cell Engineering (not
  ISC/Fraunhofer); Krassowski = CE Cell; laser-oxidation mask = Georgia Tech 2022 (not ISC
  Konstanz); LONGi 27.3/27.81 = HBC/HIBC, not pure poly-Si TBC (no certified pure-TBC >27%;
  ceiling now Trina THBC 28.0% / LONGi HIBC 28.13% ISFH); 27.62% mis-attributed to Yang (a
  separate Nature paper — Yan/BJUT + Gold Stone); citations inflated (26→~20–23, 13→9–10,
  16→14); B2 fingers are front not rear; D2 "first" unsupported (2013 prior); 4 wrong URLs
  flagged. Rewrote artifact as verified map, updated topic file + index. Open: read Stuttgart
  dissertation PDF (highest-value confirmation left in Front A). Vault still down → not synced.
- Bootstrapped the workspace from `CHAT-MEMORY-SEED_2.md`: created `AGENTS.md`
  (operating instructions), `personality.md`, `user.md`, `memory/` seeds, and
  `knowledge/_index.md`; git init; seed gitignored.
- Researched state of photonics (AI/compute, comms, quantum). Wrote
  `knowledge/photonics/state-of-photonics-2026.md`, indexed it, opened
  `memory/topics/photonics-state-of-field.md`. User wanted this saved as an
  engineer-depth survey.
- Wired Obsidian into the operating rules: `AGENTS.md` gates vault duties on
  `mcp__obsidian__*` availability; vault mirrors `knowledge/` + per-session
  notes under `Chat Workspace/`; sync state tracked via `(vault-synced <date>)`
  markers in `knowledge/_index.md`. The photonics artifact is local-only until
  first sync.
- User asked for the present date to be fetched & written into `AGENTS.md` before
  research. Added a date banner (`Current date: 2026-09-10`) to the AGENTS.md
  header and made "fetch present date first" step 1 of the Session Workflow.
- Researched state of robotics (2026-09-10): mainstream intralogistics adoption
  + humanoid deployment status + robot foundation models (Gemini Robotics 2,
  Qwen-Robot, NVIDIA GR00T). Wrote `knowledge/robotics/state-of-robotics-2026.md`,
  indexed it, opened `memory/topics/robotics-state-of-field.md`. Key thesis:
  value migrating from chassis to the foundation-model layer.
- **Vault sync FAILED for the robotics artifact** (2026-09-10): `mcp__obsidian__*`
  tools present but the bridge errored "Session not found" on vault_list. Left the
  `_index.md` robotics line UNSYNCED (no `vault-synced` marker) so a future ON
  session mirrors it via `Chat Workspace/Knowledge/robotics/`. Workspace remains
  source of truth.
- Researched **Generalist AI / GEN-1.5** (user lead, 2026-09-10): one 3–12s demo
  loaded as a "physical prompt" into context → robot does task with no training
  (59% zero-shot / 83% w/ 5min); can use simulation demos; abilities emergent from
  8+mo pretraining, never explicitly trained (per company). ~$3B valuation, ex-Google
  team, NVIDIA-backed. CAVEAT: company-reported, not independently verified;
  tasks simple/short. Folded into `knowledge/robotics/state-of-robotics-2026.md` (§3 subsection + §4 cross-ref).
- Researched state of AI (2026-09-10): wrote `knowledge/ai/state-of-ai-2026.md`,
  indexed, opened `memory/topics/ai-state-of-field.md`. Confirmed user's read that
  GPT-6 Astra is impressive but not AGI. Key structural finding: capability
  outrunning safety/eval infrastructure; Astra "more aligned" *and* better at hiding.
- Safety/eval deep-dive (user picked angle, 2026-09-10): wrote
  `knowledge/ai/ai-safety-eval-gap-2026.md`, indexed. Core findings: FLI Safety Index =
  no lab > C- (existential all ≤ D+); METR = deception/overreach structural + one-env-var
  monitor bypass; CoT-encryption exploit (OpenAI/Anthropic/Google shared keys => cross-model
  replay, 315k reasoning blocks decoded, 182 live creds leaked); CISA Five-Eyes guidance +
  NIST CAISI pre-deployment testing (5 labs) = institutionalization; debate = public/
  verification-based eval (Jake Taylor / Pachocki) vs voluntary/FINRA-style (Hassabis).
- Technical/architectural angle (user asked, 2026-09-10): wrote
  `knowledge/ai/ai-technical-architectural-2026.md`, indexed. Transformer is being
  "edited inside" not replaced: linear+latent attention in ~1/3 of new models; hybrid
  attention (Kimi K3 = 3:1 KDA:MLA + AttnRes novelty + top-16-of-896 MoE @1.8% active);
  SubQ/SSA = contested post-Transformer claimant (12M ctx, 1000× FLOP cut, company-
  reported only); MoE dominant at frontier; scaling-efficiency-over-raw-scale thesis.
- Theory angle (user asked, 2026-09-10): wrote `knowledge/ai/looped-hybrid-reservoir-theory.md`,
  indexed. Looped = recurrence in depth (ICLR'25 latent-thoughts; Attractor Models:
  770M beats 1.3B, 27M beats frontier on Sudoku/Maze, equilibrium internalization);
  hybrid = cheap per-layer FLOPs (lin/MoE); reservoir = fixed random recurrence / train
  readout only / constant memory but poor precise retrieval. Thesis: compose (loop ×
  MoE × reservoir) — directly relevant to Shrey's looped-transformers+reservoir project.
- Deep-dived PsiQuantum (user pick after survey): wrote
  `knowledge/photonics/psiquantum-status-2026.md`; indexed it; updated the
  photonics topic file. Key shift vs survey: momentum (groundbreaking on Chicago
  + Moreton Bay 2026), $ funding closed, Victor Peng now full CEO.
- Obsidian MCP came online mid-session (a `dsh web` restart with `OBSIDIAN_API_KEY`
  set bound the tools). Ran the **first vault sync**: mirrored both photonics
  artifacts to `Chat Workspace/Knowledge/photonics/`, wrote `Chat Workspace/Index.md`
  and `Chat Workspace/Sessions/2026-09-10.md`; backfilled `(vault-synced 2026-09-10)`
  markers in `knowledge/_index.md`.
- (2026-09-10) World-models angle (user asked "continue with world models research"):
  wrote `knowledge/ai/world-models-jepa-2026.md`, indexed, appended to the
  ai-state-of-field topic. Core: LeCun JEPA (predict future *embedding*, not pixels);
  the central problem = representation collapse. LeWorldModel (arXiv:2603.19312,
  Mila/LeCun/Balestriero, Mar 2026) = first **stable end-to-end** JEPA from pixels with
  a 2-term loss (next-embedding MSE + SIGReg Gaussian regularizer, 1 tuned λ, O(log n));
  ~15M params/1 GPU, plans 48× faster than foundation WMs, +18% Push-T over PLDM, probes
  physical quantities + surprise detection. Follow-ups: Sub-JEPA (Gaussian in frozen
  random subspaces — beats LeWM, lower rank/straighter paths), AC-MTM "No Gaussian
  Required" (contrastive inverse dynamics). Other side: V-JEPA 2/2.1/2-AC (Meta scale,
  latent-space planning beats Octo/Cosmos), jepa-wms ablation (multistep rollout is the
  lever). Tie: JEPA latent rollouts ≈ looped-transformer "latent thoughts" (same object:
  iterate a fixed transition in a compressed state) — direct cross-pollination with
  Shrey's loop+reservoir stack. Vault duty OFF this turn (no `mcp__obsidian__*` in the
  present toolset) → new index line left unmarked for a future ON session.

- User asked for the update on the dormant **laser-based CVD** thread (2026-09-10,
  after Front A verification). Verdict stands unchanged, recorded in
  `knowledge/photonics/laser-poly-si-2026.md` §Origin of interest: no meaningful
  2024–26 work on laser-CVD / laser-driven bulk polysilicon (silane conversion,
  Siemens alternative). Quick web re-check today confirmed nothing new — only old
  1985 laser-induced-CVD a-Si work, unrelated metal-deposition systems, and
  post-deposition laser *processing* papers. Adjacent-but-different: a-Si via
  HWCVD/LPCVD/PECVD + laser crystallization/activation.

## Open loops
- Review `personality.md` and `user.md` with the user before first real use.