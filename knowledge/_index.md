# Knowledge Index

_Points only at the **top-level domain folders** in `knowledge/`, one short summary per
folder. Per-file detail lives inside each folder's README / hub note
(`knowledge/<x>/README.md`, sub-folder `_index.md`). Never scan whole folders at session
start — open the folder README to find a file._

_Vault sync: folder entries are in the Obsidian vault under
`Chat Workspace/Knowledge/<domain>/` (marked `vault-synced 2026-09-10`). The vault
`Index.md` mirrors this folder-level structure._

_Organisation: each domain folder carries a `README.md` (folder role + contents +
cross-links; vault equivalent is the `folder-hub` note). Sub-folders get an `_index.md`
tagged `sub-folder`; interdisciplinary sub-folders stay in their parent, tagged
`interdisciplinary`, linking to both spanned domains._

## ai/ — AI research
Field survey (Sep 2026) plus deep-dives: safety/eval gap, technical architecture, theory
(looped × hybrid × reservoir), and world models / latent-JEPA. README → `ai/README.md`.
_(vault-synced 2026-09-10)_

## robotics/ — robotics
Mainstream intralogistics adoption, humanoid deployment, and the shift of value to the
robot-foundation-model layer. Sub-folder `physical_ai/` (interdisciplinary — AI that
senses/decides/acts in the world). README → `robotics/README.md`.
_(vault-synced 2026-09-10)_

## space/ — space
Space missions and the autonomy-and-robotics layer (embodied intelligence, planetary
world models, ISRU robotics, long-duration human presence). Sub-folder `ai_robotics/`
(interdisciplinary, spans space + AI/robotics: ESA embodied-intelligence + Hera flying-lab)
and `on-orbit-docking/` (sub-folder: autonomous RPO / ISAM). README → `space/README.md`.
_(vault-synced 2026-09-10)_

## system_design/ — system design
Interview-prep packages for AI-adjacent system design. **RAG** — complete package (two-phase
architecture, component deep-dives, Naive/Advanced/Modular taxonomy, RAG-vs-fine-tuning,
scaling/cost, failure modes, 45-min walk-through, question bank). **Agent/coding harness** —
complete package (control loop Thread/Turn/Item, tool interface + MCP, sandbox + safety gate,
context compaction, cost/budget control, harness-vs-workflow helper, SWE-bench evaluation).
README → `system_design/README.md`. _(vault-synced 2026-09-10)_

## photonics/ — photonics
Present state across AI/compute, communications, quantum; PsiQuantum; laser processing of
poly-Si. README → `photonics/README.md`. _(vault-synced 2026-09-10)_

## dshtools/ — DSH/Obsidian tooling
Operational notes on this tooling (Obsidian MCP bridge: what works + its quirks). README →
`dshtools/README.md`. _(vault-synced 2026-09-10)_