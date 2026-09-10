# Short-term Memory — session staging area

_Budget ~100 lines. Consolidate at session end; then reset to this header._

## This session (2026-09-10)
- obsidian-mcp: on (`mcp__obsidian__*` present) — vault duty ON.
- **New domain: system_design (2026-09-10)** — user asked for a **RAG system-design interview-prep
  package** saved to a new folder. Created `knowledge/system_design/` (hub `README.md`, `folder-hub`
  in vault) + the complete note `rag-system-design-2026.md`: two-phase architecture (ingestion +
  retrieve/generate), component deep-dives (chunking/embedding/vector DB/ hybrid search+RRF/
  reranking/query rewrite/prompt+citations), Naive/Advanced/Modular taxonomy (Gao et al. survey +
  original Lewis et al. NeurIPS 2020), RAG-vs-fine-tuning scoring helper, scaling/cost, failure
  modes, 45-min interview walk-through, RAGAS eval metrics, 22-question bank. Grounded in PracHub
  RAG-at-scale guide + FAANG-style RAG codex. Mirrored to vault
  `Chat Workspace/Knowledge/system_design/` (hub `_system_design.md` + note) + added
  `system_design/` row to `Chat Workspace/Index.md` + session note appended. Local
  `knowledge/_index.md` marked `(vault-synced 2026-09-10)`.
- **Agent/coding harness system-design package (2026-09-10)** — follow-up to the RAG package:
  wrote `knowledge/system_design/agent-harness-system-design-2026.md`, the complete **agent
  harness** note (vault mirror in `Chat Workspace/Knowledge/system_design/`). Covers: control
  loop + Codex Thread/Turn/Item state model (steer/interrupt/approval), tool interface +
  registry (function calling / MCP), sandbox + safety-gate approval policy, memory tiers
  (scratchpad/episodic/long-term), context compaction (Codex ARC-AGI-3 13.3%→38.3% @ 6× fewer
  tokens — harness config beat model), harness-vs-workflow decision helper (Anthropic "start
  simple"), cost/budget control, 45-min interview walk-through, SWE-bench Verified eval, question
  bank. Grounded in OpenAI/Codex harness architecture, Anthropic building-effective-agents,
  handbook-academy agent-architectures (ReAct/Reflection/planner-executor), MCP, harness.io
  design philosophy. Updated `system_design/README.md` + `knowledge/_index.md` (both list RAG +
  harness) + `Chat Workspace/Index.md` row + `_system_design.md` hub (links AI + RAG-RAG edge).
- **Reconfirmed MCP quirk (2026-09-10):** heading-target `vault_patch` flakiness is persistent —
  nested and parenthesised heading paths (`["9. Question answer bank (practice)"]`,
  `["...", "Scale & production"]`) fail "could not resolve heading target" even though the same
  path works in `vault_get_document_map` / `vault_read`. Workaround that worked every time: full
  `vault_write` of the file. Recorded for dshtools/obsidian-mcp-notes.

- **Index + graph restructure (2026-09-10)** — user clarified the target was the vault
  `Chat Workspace/Index.md` (same folder as the graph), NOT the local `knowledge/_index.md`.
  Made **Index.md** folder-level only: points at the five top-level folder hubs
  (`_ai`/`_robotics`/`_space`/`_photonics`/`_dshtools`, each `folder-hub`) with a one-line
  domain summary; sub-folder hubs listed only for orientation; per-file detail removed (lives
  in each folder README/hub). Made **Topic Connection Graph.md** fully self-contained: links
  only to Index, references no other notes; cross-domain edges now shown as descriptive plain
  text (not `[[...]]` wikilinks) so the graph node creates no edges to other notes
  (verified `links: [Index.md]`, `unresolvedLinks: []`). Mirrored the folder-level structure
  back to local `knowledge/_index.md` to keep both in sync.
- **(continued) on-orbit-docking deep-dive (2026-09-10):** answered the survey's open question —
  wrote `knowledge/space/on-orbit-docking/mev-vs-adrasj-docking-approaches.md`.
  Core: MEV/MRV = cooperative GEO docking (machined client interface, ground go/no-go at ~20 m
  via downlinked IR+visible imagery, MRV adds DARPA robotic arm + MEPs); ADRAS-J = non-cooperative
  LEO debris RPO (Angles-Only → Model-Matching nav, autonomous collision-avoidance, 50 m→20 m→15 m
  fly-arounds, no contact; H-IIA upper stage). Key synthesis: MEV = autonomy via procedure+human
  gate; ADRAS-J = autonomy via perception+onboard safety; first truly non-cooperative *capture*
  of unprepared debris still unproven (watch ADRAS-J2 FY2027). Updated hub/_index/topic/sessions.
  **Vault mirror COMPLETE (2026-09-10)**: deep-dive mirrored to
  `Chat Workspace/Knowledge/space/on-orbit-docking/`; hub + Index.md + graph (node + edge) +
  sessions note updated; `(vault-synced 2026-09-10)` marked in `knowledge/_index.md`.
- **New Space sub-folder: on-orbit-docking (2026-09-10):** user asked about autonomous
  in-orbit docking (applications: in-orbit repair, space-base construction). Created
  `knowledge/space/on-orbit-docking/` (first **non-interdisciplinary** Space sub-folder,
  hub tagged `sub-folder`, links up to `_space`) + survey
  `autonomous-in-orbit-docking-2026.md`. Core: docking/RPO/berthing/capture mechanics;
  cooperative vs non-cooperative regimes; vision-based rel-nav stack; applications table;
  state 2026 = Northrop MEV(mv)-1/2 life-extension paying + MRV launch ~Jul 2026,
  Astroscale ADRAS-J (world's-first commercial debris inspection, non-coop to 15 m,
  autonomous collision-avoidance; ADRAS-J2 FY2027), **NASA OSAM-1 CANCELLED**, IBDM/IDSS
  standard, VisNav+safety-critical-ML cert frontier. Opened
  `memory/topics/space-on-orbit-docking.md`; updated `_space` hub + README + `knowledge/_index.md`.
  Cross-links to space/ai_robotics (GNC/vision layer), world-models (pose pred), safety-eval.
  Vault mirror pending.
- **(continued) Space AI and robotics (2026-09-10):** second study added to
  `knowledge/space/ai_robotics/` = **ESA Hera as a flying software laboratory**
  (`esa-hera-flying-software-lab-2026.md`, OSIP Ca-2026-00066). Read it as the
  **deployment-testbed counterpart** to the embodied-intelligence roadmap study:
  dual-core LEON OBC sends Core 0 = flight sw, **Core 1 = protected sandbox** running
  third-party onboard autonomy/edge-compute/resilience experiments ~150 M km out
  (auto-kill on anomaly, strict memory protection, 2–3 h/≤10 h day windows; ~40-min
  comm lag; ops ~5–30 km from Didymos). Still **open for idea submission** as of
  2026-09-10 (Discussion Sep 15 → Evaluation Oct 15; impl + source due 31 May 2027;
  ~4-week campaign Aug 2027). OPS-SAT/Doom precedent. Updated sub-folder hub +
  `knowledge/_index.md` + topic file + sessions; mirroring to vault next.
- **New domain: Space (2026-09-10)** — user asked to create a new `knowledge/space/`
  domain folder with sub-folders; first sub-folder = interdisciplinary **Space AI and robotics**
  (spans space + AI/robotics), tagged `interdisciplinary`. First study = **ESA's embodied
  intelligence for space robotics** (Discovery Element OSIP campaign: target scenarios =
  autonomous surface/subsurface exploration, autonomous ISRU, robotic support to long-term
  human presence; embodied co-design OR physical-intelligence approach; NOT isolated AI/hw,
  NOT orbital). Wrote `knowledge/space/ai_robotics/*`, hubs `_space` + `ai_robotics`, opened
  `memory/topics/space-ai-robotics.md`, indexed; vault duty ON → mirrored to
  `Chat Workspace/Knowledge/space/` + hubs + Index + sessions note.
- **New research: Physical AI (2026-09-10)** — user asked to gather info on physical AI in the
  context of robotics and AI. Created interdisciplinary sub-folder `knowledge/robotics/physical_ai/`
  (tagged `interdisciplinary`, per AGENTS.md convention), hub `physical_ai/_index.md` linking to
  BOTH `robotics/_robotics` and `ai/_ai`. Wrote `knowledge/robotics/physical_ai/state-of-physical-ai-2026.md`,
  indexed it, opened `memory/topics/physical-ai-state-of-field.md`. Key findings: Physical AI =
  AI that senses/decides/acts in the physical world (NVIDIA framing); >$75B capital 2025; $430B→2030,
  ~$1T 2035–40, ~$1.6T 2040 (Future Markets analyst); "compute is data" data factory (Cosmos+OSMO)
  attacks the 50–200 demo bottleneck; **safety certification (NVIDIA Halos, BlackBerry QNX) = binding
  deployment gate**; value moving metal → models → orchestration → safety-cert; open race (US
  intelligence / China cost / Japan $65B density / EU physics wedge Mistral-Emmi & INSAIT SPEAR-1).
  NVIDIA GTC 2026: Cosmos 3 (unified world model), GR00T N1.7 + GR00T N2 (DreamZero world-motion,
  "2× VLA" claim), Isaac Lab 3.0/Newton, Physical AI Data Factory + Omniverse DSX/Mega blueprints.
  Peers: Physical Intelligence π0.7 (VLA "GPT-3 moment"), Google Gemini Robotics, Qwen-Robot.
  **Vault duty ON & bouncing**: artifacts mirrored to `Chat Workspace/Knowledge/robotics/physical_ai/`
  (hub `physical_ai.md` tagged interdisciplinary), Index + `_robotics` + `_ai` hubs updated, sessions
  note appended; `(vault-synced 2026-09-10)` markers written locally.
- **Looped×reservoir literature scan (2026-09-10)** — user asked if anyone built looped +
  reservoir transformer. Worked around the out-of-balance `web_search` endpoint (HTTP 402) via
  arXiv full-text search through fetch. Findings in `knowledge/ai/looped-reservoir-hybrid-literature-2026.md`
  + thread `memory/topics/looped-reservoir-literature.md`, cross-linked from the theory spine:
  - Reservoir×transformer IS published: **Echo State Transformer** (arXiv:2507.02917,
    Bendi-Ouis/Hinaut) = parallel ESN reservoirs as fixed-size working memory, attention over
    reservoir units → linear complexity, #1 in 2/5 Time Series Library cats; **FRESCO**
    (arXiv:2606.24969, incl. Kappel) = O(N) frequency-domain ESN; **ESN-LM at scale**
    (arXiv:2503.01724, Ueda et al) — ESN ≥ Transformer on grammar at ~100M words.
  - **The looped×reservoir triple combo (loop-for-depth × reservoir-memory) is UNCLAIMED** —
    direct arXiv search `"looped"+reservoir+language` = zero relevant hits. That is Shrey's
    contributor edge for the nanoGPT project.

## Prior session (2026-09-10)
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
- **Obsidian MCP end-to-end test (2026-09-10)**: exercised every `mcp__obsidian__*`
  tool against the live vault (`Chat Workspace/`). All worked: vault_list/read/write/
  append/copy/move/patch/delete, get_document_map, search_simple, search_query
  (JsonLogic), tag_list, open_file, active_file_get_path, command_list/execute.
  **Two quirks found (worth remembering):**
   1. **Heading targeting by array path is flaky** — only the document root /
      top-level heading resolves (e.g. `["Note Title"]`). Nested heading targets like
      `["Section One"]` or `["A","B"]` consistently returned **"Target not found"**,
      even on real files (session note). Workaround: target the root heading and use
      relative `#` levels in `content`. Likely a server-side resolver quirk.
   2. **Block-scope list append splices with no newline** — appending `- item three`
      to a 2-item list gave `- item two- item three` (one line). Functional but
      cosmetically broken for lists; you must supply your own leading `\n` to continue
      a list via block `append`.
  Frontmatter list-append w/ createTargetIfMissing, scalar replace, tag_list live
  reflection, and `vault_delete` (to-trash) all behaved correctly. Test files were
  created under `Chat Workspace/_tests/` then fully removed.
- **Vault bridge now UP (2026-09-10)** — the earlier `Session not found` failures
  are resolved; bridge authenticated in this session. So the pending local-only
  artifacts are being synced (ai/*, robotics, laser-poly-si) and an MCP-testing note
  + topic connection-graph note are being appended to the vault — core structure
  retained (+ `Chat Workspace/Index.md` refresh).
- **Folder-hub colour + sub-folder conventions (2026-09-10)**: user wanted the four
  vault folder hubs (`_ai`, `_robotics`, `_photonics`, `_dshtools`) red in the graph
  view. Added `tags: [folder-hub]` frontmatter to all four (verified via tag_list:
  count 4). Actual red colour is a one-off graph-view colour group on `tag:#folder-hub`
  (user applied manually — `.obsidian/graph.json` not writable via the REST API).
  **Standing rule added to AGENTS.md**: any future sub-folder → tag its hub
  `sub-folder`; an interdisciplinary sub-folder (e.g. `robotics/physical_ai`, spans
  AI + robotics) is **kept in its parent folder**, tagged `interdisciplinary`, and
  its hub links to BOTH parent + spanned domain hubs. Replicate in local `knowledge/`
  with a README/_index per folder. To do when sub-folders appear.

## Open loops
- Review `personality.md` and `user.md` with the user before first real use.