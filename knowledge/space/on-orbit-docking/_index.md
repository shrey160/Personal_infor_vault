# space/on-orbit-docking/ — sub-folder hub

> Sub-folder hub: tagged **`sub-folder`** in the vault graph. A dedicated Space
> topic sub-folder (not tagged `interdisciplinary` — this is core spacecraft
> operations/RPO within the Space domain, though it cross-links to the
> `space/ai_robotics/` thread for the GNC/vision/autonomy layer). Hubs link up
> to the parent folder hub (`space/_space`).

## Role
Autonomous in-orbit docking / rendezvous & proximity operations (RPO) — the enabling
mechanism of on-orbit servicing, assembly and manufacturing (ISAM): repair, life-extension
& refueling, debris removal, in-orbit construction, and station/base operations.

## Contents
- autonomous-in-orbit-docking-2026.md — survey: what docking/RPO/berthing/capture is,
  cooperative vs non-cooperative regimes, the sensing+decision stack, applications table
  (repair, refueling, debris removal, assembly, base ops), current state 2026 (MEV/MRV,
  ADRAS-J, OSAM-1 cancellation, IBDM/IDSS, VisNav), engineering synthesis + open questions.
- mev-vs-adrasj-docking-approaches.md — deep-dive: Northrop **MEV/MRV** (cooperative GEO
  docking: machined interface, ground go/no-go at ~20 m, robotic-arm MEP via DARPA) vs
  Astroscale **ADRAS-J** (non-cooperative LEO debris RPO: Angles-Only → Model-Matching nav,
  autonomous collision-avoidance, 50 m → 20 m → 15 m fly-arounds, no contact).

## Cross-links (part of the Space domain, shares the autonomy/GNC layer with ai_robotics)
- `space/ai_robotics/` — the GNC/vision/onboard-autonomy layer (ESA embodied-intelligence
  roadmap, Hera sandbox) that autonomous docking depends on.
- `ai/world-models-jepa-2026.md` — world models as the natural fit for predicting a tumbling
  target's pose over the approach.
- `ai/ai-safety-eval-gap-2026.md` — certifiability of learned relative-pose estimators /
  collision-avoidance (the "trust under uncertainty" gate).
- `robotics/` — robotic-arm capture / in-orbit robotic manipulation.

_Structural note (AGENTS.md convention): a dedicated Space topic sub-folder gets its own hub
tagged `sub-folder`, linking up to `space/_space`; an *interdisciplinary* sub-folder (spans a
second domain, e.g. `ai_robotics`) would be tagged `interdisciplinary` instead. Vault-
equivalent hub: `Chat Workspace/Knowledge/space/on-orbit-docking/on-orbit-docking.md`._