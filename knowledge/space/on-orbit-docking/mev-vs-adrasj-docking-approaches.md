# MEV/MRV docking architecture vs ADRAS-J's non-cooperative approach

_Deep-dive compiled 2026-09-10 for Shrey. Follows the survey
`autonomous-in-orbit-docking-2026.md` in `knowledge/space/on-orbit-docking/`. This is the
"one program vs another" drill-down: **Northrop Grumman MEV/MRV** (cooperative GEO docking)
versus **Astroscale ADRAS-J** (non-cooperative LEO debris RPO). Primary sources + vendor-disclosed
detail; I flag where a number is an inference._

_Working context: today is 2026-09-10. Both programs are real and flown (MEV-1/2 operational;
ADRAS-J completed Phase I and began deorbit 25 Mar 2026). This is a *comparison of two design
philosophies* — cooperative-docking-for-revenue vs non-cooperative-RPO-for-capability — not a
scorecard of "who's winning."_

---

## One-line framing

**MEV/MRV solve "how to latch onto a satellite built with a servicing interface and keep it
alive in GEO"; ADRAS-J solves "how to get centimeters-near a dead, tumbling rocket body that
was never meant to be touched."** Same underlying RPO physics; opposite constraints. MEV is the
*assured, revenue-generating* design; ADRAS-J is the *capability-proving, high-risk* design.

---

## Side-by-side: the core difference

| Dimension | **MEV / MRV (Northrop, GEO)** | **ADRAS-J (Astroscale, LEO)** |
|---|---|---|
| **Target** | Cooperative GEO comsat (Intelsat 901, Intelsat 1002) with a compatible docking interface / MEP-compatible features | Non-cooperative defunct H-IIA **upper stage** (11 m × 4 m, ~3 t, launched 2009) — no port, no comms, unprepared |
| **Orbit regime** | **GEO** (~35,786 km; ~120–250 ms one-way light time) | **LEO** (~500 km; ms light time, but ground passes are short) |
| **Mission** | **Dock & stay** — act as proxy propulsion to extend life (MEV hosts its own thrusters) | **Approach, inspect, characterize** — fly-around photography; *no contact* (capture is ADRAS-J2's job) |
| **Navigation** | Uses target's **known interface + measured relative range/angle**; ground-in-the-loop for go/no-go at close range | **Angles-Only nav → Model-Matching nav** against an unknown body; progressively self-determines the relative state |
| **Autonomy level** | **Automated with human go/no-go gates** (e.g. hold at 20 m, downlink imagery, ground decides docking) | **Autonomous approach with onboard collision-avoidance** + ground-supervised phasing |
| **Revenue model** | Paying service (life extension as a product; MRV expands to inspection/repair/MEP install) | JAXA-funded **CRD2 demo** (Phase I inspection; Phase II removal) — capability then service |
| **Contact hardware** | Docking mechanism gripped to the client's interface; MRV adds **DARPA-developed robotic arm** + Mission Extension Pods (MEPs) | No capture hardware (inspection-only); **Payload Attach Fitting** on the stage identified as the future ADRAS-J2 capture point |

---

## 1. MEV/MRV — the cooperative, revenue-proven architecture

### What it is
The **Mission Extension Vehicle (MEV)** is Northrop Grumman SpaceLogistics' life-extension
servicer: it docks to a GEO satellite running low on fuel and hosts its own propulsion to keep
the client operational. Launched MEV-1 (2019, on-orbit 2020) is a three-axis stabilised GEO
servicer; MEV-2 followed; Northrop reports **a third docking operation completed** and "nearly a
decade of combined in-space service" with no reported client disruptions.

### How the docking works (the load-bearing detail)
- **Targets are cooperative and prepared (pre-selected):** Intelsat 901 / Intelsat 1002 had a
  **compatible docking interface** — a capture fixture Northrop could grip. This is the critical,
  load-bearing assumption of the whole MEV design: it only "just works" because the client has a
  known, machined interface with navigation features.
- **Phasing + approach uses the target's known state:** the MEV does long-range rendezvous in
  GEO using the client's well-known ephemeris, then closes in using relative range/angle sensing
  against the known interface.
- **The close-range go/no-go loop is ground-in-the-loop (automated, not autonomous):** a Chinese
  technical write-up of the 901 docking captures the pattern — **at ~20 m MEV held position and
  downlinked both infrared and visible-light images of the client so ground controllers could
  decide whether to proceed to dock.** That single datapoint is the whole MEV philosophy: the
  mechanism executes precisely, but the *decision* to touch is ratified by a human with imagery.
- **Contact/capture:** MEV drives into and grips the client interface (a capture-ring-type latch),
  then becomes a "space tug + RCS host" for the client's rest of life. No fuel transfer — it's
  *hosting thrust*, not refueling.

### MRV — the next generation (and where it departs from MEV)
The **Mission Robotic Vehicle (MRV)** is the follow-on, launching ~Jul 2026 (SpaceX from Cape
Canaveral). The key departure: **MRV adds a robotic arm developed by DARPA** and the ability to
**install Mission Extension Pods (MEPs)** — small "jet-pack" propulsion units — onto clients,
rather than docking the whole servicer. This is significant for the docking story because:
- Installation of an MEP still requires **close-proximity RPO and a controlled, forced contact with
  a client surface** — but now it's *robotic-arm placement* onto a variety of interfaces, not a
  single pre-built docking port. It generalises MEV's "one interface" into "many interfaces via a
  manipulator."
- MRV also performs inspection, relocation, inclination reduction, repair and **debris removal**,
  and MEPs could later be stored in an on-orbit cache for rapid call-up (resilience play). This
  edges MRV toward the non-cooperative capture space ADRAS-J is opening — but from the commercial,
  cooperative side.
- Northrop's refueling plan (Elixir program, USSF contract Jan 2025) extends RPO+docking+refuel+
  undock; they frame ISAM (in-space servicing/assembly/manufacturing) as the ~2030 payoff.

---

## 2. ADRAS-J — the non-cooperative, capability-proving approach

### What it is
**ADRAS-J** (Active Debris Removal by Astroscale-Japan) is Phase I of **JAXA's CRD2**
(Commercial Removal of Debris Demonstration). Launched 18 Feb 2024 (Rocket Lab Electron), it
became the world's first mission to safely approach and characterise a large, *unprepared* piece
of debris: a defunct **H-IIA upper stage** (11 m long, 4 m diameter, ~3 t, launched 2009). It was
photography/inspection-only; capture/removal is the follow-on.

### How it flies against something with no features (the load-bearing detail)
The **Conops and navigation phases** (from Astroscale's mission page timeline) are the heart of
the non-cooperative problem:

1. **Launch + initial rendezvous** (Feb 2024): phasing toward the stage's orbit.
2. **Angles-Only navigation** (from 9 Apr 2024): from *several hundred kilometres*, and then from
   several hundred km down — the chaser uses **angular measurements only** (angles-only nav) to
   converge on the target. This is hard because angles-only is poorly observable in range early on.
3. **Model-Matching navigation** (16 Apr 2024): once close enough for cameras to resolve the body,
   the system switches to **model-matching** — it builds/uses a 3D model of the object and matches
   imagery against it to determine relative pose. This replaces the absent cooperative interface.
4. **Close approach** (Apr–May 2024): to **several hundred metres**, then **50 m** (23 May 2024),
   with fixed-point observations.
5. **Fly-around + collision-avoidance validation** (Jun–Jul 2024): three+ controlled fly-arounds
   under varying lighting, capturing multi-angle high-res imagery — and validating the **autonomous
   collision-avoidance** function.
6. **Final approach** (Nov–Dec 2024): first to 20 m (Jul), then **historic 15 m** approach
   (30 Nov 2024) — the closest a commercial spacecraft has come to a debris object via RPO.
7. **Characterisation:** imaged the stage's **Payload Attach Fitting** — the intended ADRAS-J2
   capture point — confirming it's intact for future removal.
8. **Deorbit** (25 Mar 2026): after 293 days, began controlled deorbit (re-entry within ~5 years).

### The hard-won lessons (from Astroscale's project manager, Eijiro Atarashi, and the IAC 2025 lecture)
- **"Far more difficult than expected"** to design full-range RPO against a non-cooperative
  object — yet **every navigation phase exceeded predictions.**
- An **abort manoeuvre occurred during approach** ("even when the satellite initiated an abort
  during approach") and the team's "calm decision-making and robust engineering enabled safe
  recovery and multiple successful approaches." This is the honest cost of non-cooperative RPO:
  things go wrong, and the system + ops team must be robust.
- The mission **verified no docking/capture occurred** — it deliberately stayed at standoff and
  handed the contact-hardware problem to ADRAS-J2 (planned FY2027), which will *remove* the same
  debris using the intact Payload Attach Fitting as the capture point.

---

## 3. The engineering comparison that actually matters

| Axis | MEV/MRV (cooperative) | ADRAS-J (non-cooperative) |
|---|---|---|
| **Relative-state sensing** | Easy-ish: known interface + reflectors/geometry → range/angle | Hard: unknown body, Angles-Only → Model-Matching, no nav aids |
| **Light-time / ops latency** | GEO ~120–250 ms forces *some* automation, but ground can still gate via imagery (~120 ms is borderline; the 20 m image-downlink gate is the workaround) | LEO ms latency, but ground passes are short — the *onboard* loop must close on its own timing |
| **Autonomy requirement** | Moderate: execute precise profile + hold, human ratifies dock | High: self-determine pose and approach an unprepared body with autonomous collision-avoidance |
| **Risk tolerance** | Low — it's a revenue service protecting a paying client; **no-disruption guarantee** (nearly a decade without client disruption) | High — a demo against a dead rocket; failure costs a test, not a customer |
| **Contact mechanism** | Deterministic latch onto a machined interface (MEV); robotic-arm MEP placement (MRV) | None (inspection) → capture fixture targeted (ADRAS-J2 with the Payload Attach Fitting) |
| **Economic role** | Recurring revenue now | Capability milestone now, revenue later (debris-removal service) |

### The synthesis
- **You cannot "automate your way" from MEV to ADRAS-J — it's a different sensing problem.**
  The hard part of ADRAS-J is that there is **no known interface to measure against**, so the
  sensing chain (angles-only → model-matching) has to *construct* the relative navigation from a
  body that was never designed to be observed. MEV's interface collapses that problem to
  precision mechanics.
- **MEV = autonomy via procedure + human gate; ADRAS-J = autonomy via perception + onboard
  safety.** MEV nails the *execute-a-well-defined-profile* problem; ADRAS-J demonstrates the
  *sense-and-decide-under-uncertainty* problem. These are the "automated vs autonomous" and
  "cooperative vs non-cooperative" axes from the survey, made concrete.
- **MRV is the bridge.** By adding a DARPA robotic arm + MEPs, Northrop moves from "one machined
  interface" toward "manipulate a range of client bodies" — the first step from the MEV world
  toward the capture problem ADRAS-J2 will face. The non-cooperative *capture* (robotic grab of an
  unprepared, possibly tumbling body) is exactly the frontier where ADRAS-J2, MRV's future, and the
  vision-based-relnav R&D (Blackswan, ESA) all meet — and where the `space/ai_robotics/` thread's
  GNC/vision/world-model layers become decisive.
- **The honest caveats:** MEV's "nearly a decade" and "no reported disruptions" are vendor claims;
  ADRAS-J's success is well-documented but it never had to *touch* the target. The literal act of
  docking a non-cooperative, unprepared body remains unproven by any commercial actor as of
  2026-09-10 — that first successful *capture-and-control* of unprepared debris will be the real
  milestone to watch (ADRAS-J2, FY2027).

---

## 4. Sources (primary / near-primary first)

- **Astroscale ADRAS-J mission page** (Conops + full timeline incl. Angles-Only, Model-Matching,
  50 m → 20 m → 15 m, fly-arounds, collision-avoidance validation): [astroscale.com/missions/adras-j](https://www.astroscale.com/en/missions/adras-j)
- **IAC 2025 highlight lecture — "ADRAS-J: First Encounter with Space Debris"** (Chris Blackerby/
  Astroscale COO, Toru Yamamoto/JAXA): [iafastro.org](https://www.iafastro.org/events/iac/international-astronautical-congress-2025/plenary-programme/highlight-lectures/adras-j-first-encounter-with-space-debris.html)
- **Astroscale ADRAS-J completion + deorbit** (25 Mar 2026), incl. project-manager quote on the
  abort + recovery: [astroscale.com](https://www.astroscale.com/en/news/astroscales-adras-j-mission-completes-operations-begins-deorbit)
- **Northrop Grumman — Space Industrial Revolution** (MEV-1/901, MEV-2/1002, third docking, MRV +
  DARPA robotics + MEPs, refueling/Elixir, ISAM ~2030): [northropgrumman.com](https://northrop-grumman-7yrpnjh7o-agencyq-ngc.vercel.app/what-we-do/space/space-logistics-services/space-industrial-revolution)
- **Chinese technical write-up of Intelsat 901 MEV-1 docking** (the 20 m hold + IR/visible-image
  ground go/no-go detail): [huxiu.com](https://pro.huxiu.com/article/321386.html) *(English condensed; treat the 20 m gate as reported detail)*
- **Northrop MRV release/launch context:** [MRV media kit](https://www.northropgrumman.com/what-we-do/events/mission-robotic-vehicle-mrv-media-kit), [satellitetoday MRV summer launch](https://www.satellitetoday.com/technology/2026/05/19/northrop-grummans-first-mrv-readies-for-summer-launch-to-expand-the-space-servicing-toolkit/)

_Unverified/estimated: "nearly a decade" and "no client disruption" are Northrop claims; the exact
MEV capture-mechanism latching detail and the 20 m gate are reported rather than primary-source
mechanical specs. ADRAS-J operational phases are from Astroscale's own timeline (well-documented)._