# Autonomous in-orbit docking — thread

status: active

## Why
User asked to look into **autonomous in-orbit docking**, naming applications like
in-orbit repair and efficient space-base construction, wanting the definition,
applications and current state (2026-09-10). New `space/on-orbit-docking/` sub-folder
(tagged `sub-folder`) — the first non-interdisciplinary Space topic sub-folder.

## Key findings (2026-09-10)
- **Definition:** autonomous docking = one spacecraft approaching + physically connecting
  to another / to infrastructure without continuous human control. Sits at the end of
  **Rendezvous & Proximity Operations (RPO)**. Splits into **docking** (active closing
  velocity into a port), **berthing** (robotic arm places vehicle), **capture** (grab of a
  non-cooperative target — no port).
- **Regimes:** **cooperative** (known port + nav aids; the ISS/Soyuz/Dragon world) vs
  **non-cooperative** (legacy/derelict/debris — tumbling, no port, no comms) — the hard frontier.
- **The stack:** GPS/cGPS for Earth-bound, relative-state sensing (RF/laser/cameras → now
  **vision-based relative nav / VisNav** + learned pose estimators), closed-loop GNC, and a
  **safety architecture** (control-tolerance zones, autonomous collision-avoidance, collision-
  probability budget). Light-time latency forces autonomy at GEO and beyond.
- **Application:** in-orbit repair; life-extension / refueling (the proven business); debris
  removal; in-orbit assembly & construction (the "space base" ask); station/base ops;
  orbit-transfer servicing / space tugs; failed-launch recovery.
- **Current state 2026:** **Northrop MEV-1/MEV-2** = paying cooperative-GEO life-extension;
  **MRV** launched ~Jul 2026 (robotic-arm servicing). **Astroscale ADRAS-J** (25 Mar 2026) =
  world's first commercial debris inspection, non-cooperative RPO to 15 m, autonomous
  collision-avoidance; **ADRAS-J2** FY2027 removal. **NASA OSAM-1 CANCELLED** (cost/schedule) —
  the big-robotic-flagship cautionary tale. Hardware/standards mature (**IBDM / IDSS**);
  VisNav + safety-critical deep-learning certification is the active R&D frontier (ESA funding
  Blackswan, academic critical surveys).

## Open questions / next steps (offers to user)
- ~~Deep-dive a program (MEV/MRV vs ADRAS-J)~~ — **DONE** → see `mev-vs-adrasj-docking-approaches.md`.
- The non-cooperative target problem: tumbling + no-port + no-comms; VisNav/pose + world models.
- Why OSAM-1 died — cost/schedule drivers, commercial vs government route.
- Docking mechanism taxonomy + standards (hard vs soft capture, androgynous, IBDM/IDSS, NDS).
- ISAM market model — $ forecast, buyers, price per operation.

## Artifacts
- knowledge/space/on-orbit-docking/_index.md — sub-folder hub (tagged sub-folder)
- knowledge/space/on-orbit-docking/autonomous-in-orbit-docking-2026.md — the survey
- knowledge/space/on-orbit-docking/mev-vs-adrasj-docking-approaches.md — MEV/MRV vs ADRAS-J deep-dive
- Cross-links to: space/ai_robotics/ (GNC/vision/autonomy layer), ai/world-models-jepa-2026.md
  (pose prediction), ai/ai-safety-eval-gap-2026.md (certifying learned rel-nav).
- Vault mirroring complete (2026-09-10): deep-dive mirrored to
  Chat Workspace/Knowledge/space/on-orbit-docking/, hub + Index.md + graph + sessions note updated,
  `(vault-synced 2026-09-10)` marked in knowledge/_index.md.

## Next steps
- Park or extend per user direction.