# Short-term Memory — session staging area

_Budget ~100 lines. Consolidate at session end; then reset to this header._

## This session (2026-09-10)
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

## Open loops
- Review `personality.md` and `user.md` with the user before first real use.