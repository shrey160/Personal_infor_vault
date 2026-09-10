# Space AI and robotics — thread

status: active

## Why
User asked to create a new **Space** knowledge folder with sub-folders; the first
is the interdisciplinary **"Space AI and robotics"** sub-folder (spans space +
AI/robotics, per the AGENTS.md interdisciplinary convention). The first study is
ESA's **Embodied Intelligence for Autonomous Space Systems** campaign — the
institutional demand signal for space physical AI toward 2040 (2026-09-10).

## Key findings (2026-09-10)
- **(Study 1) ESA embodied-intelligence campaign** — see esa-embodied-intelligence-space-2026.md.
  The *roadmap* question: system-level autonomy for lunar/planetary surface robots.
  Idea phase already closed (12 Aug 2026), proposal eval Oct 2026, final day late 2027.
- **(Study 2) ESA Hera as flying software laboratory** — see esa-hera-flying-software-lab-2026.md.
  The *deployment-testbed* question: actual third-party onboard autonomy/edge/resilience
  software running in a real deep-space sandbox ~150 M km out.
  - **Hardware reality:** dual-core LEON OBC — Core 0 = flight software; **Core 1 = protected
    sandbox** running guest Experimental Software (ESW) under an Experimental Software
    Framework. Auto-kill on anomaly/Safe Mode, strict memory protection (any cross-region
    access ⇒ immediate termination), no direct hw access, exec windows ~2–3 h/day (≤~10 h/day).
  - **Timing:** still open for idea submission as of 2026-09-10 (OSIP: Submission→Discussion
    Sep 15→Evaluation Oct 15; feature article says select mid-Oct 2026, impl package + source
    due 31 May 2027, ~1-month campaign Aug 2027). ~40-min comm lag; ops ~5–30 km from Didymos.
  - **Scope:** autonomy (progress without ground), edge computing (downlink only what matters),
    resilience (safe coexistence with flight sw). Categories: onboard image processing/feature
    tracking, autonomous decision logic, advanced GNC (incl. convex-optimisation guidance,
    PQC for data security), compression/prioritisation, lightweight onboard AI inference for
    anomaly detection/science classification, new deep-space ops concepts.
  - **Precedent:** OPS-SAT famously ran standard software in space incl. a game of Doom;
    Hera "pushes that spirit further."
- **Interdisciplinary tie:** ESA's lever list (planetary FMs, world models, multi-agent) =
  the same stack studied in robotics/physical_ai; the **space-specific filter** is the
  constrained edge compute/power/radiation budget — compact world models (JEPA line) fit
  better than huge generative backbones.

## Open questions / next steps (offers to user)
- **Live opportunity:** Hera call (Ca-2026-00066) still open for idea submission as of
  2026-09-10 (~5 days left in Submission). Worth drafting an idea — a compact
  world-model or lightweight onboard anomaly-detection classifier fits the 2–3 h/kB-RAM
  envelope with a quantifiable claim.
- The venn-diagram of who else funds this: NASA (ARTEMIS/SSNext surface autonomy),
  China lunar programs, commercial (Vast/Robotic ISRU startups) — contrast against ESA.
- Deep-dive the shared-world-model coordination problem (ISRU heterogeneous agents).
- The edge-compute envelope: what planetary world-model class actually fits a
  flight-like power budget (model-size ceiling as a function of W/radiation).
- Track how ESA's roadmap money vs NVIDIA/PI frontier converge on architecture.

## Artifacts
- knowledge/space/_space.md + README.md — domain folder hub (folder-hub tag)
- knowledge/space/ai_robotics/_index.md — interdisciplinary sub-folder hub
- knowledge/space/ai_robotics/esa-embodied-intelligence-space-2026.md — embodied-intelligence roadmap study
- knowledge/space/ai_robotics/esa-hera-flying-software-lab-2026.md — Hera flying-software-lab study
- Cross-links to: robotics/physical_ai/, ai/world-models-jepa-2026.md,
  robotics/state-of-robotics-2026.md, photonics/state-of-photonics-2026.md

## Next steps
- Park or extend per user direction.