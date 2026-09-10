# Autonomous In-Orbit Docking — what it is, what it's for, where it stands

_Survey compiled 2026-09-10 for Shrey at engineer-depth. Opens a new `space/on-orbit-docking/`
sub-folder (tagged `sub-folder`) under the Space domain. Companion to the `space/ai_robotics/`
thread where the GNC/vision/autonomy layer lives._

_Working context: today is 2026-09-10. Figures are from primary sources or vendor-disclosed
estimates; I flag anything estimated or forward-looking. Where a program has been cancelled or
restructured (OSAM-1), I say so plainly — this field has as many failures as successes._

---

## One-paragraph answer

**Autonomous in-orbit docking is the capability of one spacecraft to approach and physically
connect to another — or to a piece of orbital infrastructure — without continuous human
control.** It is the enabling mechanism of the entire **on-orbit servicing (OOS) / in-orbit
servicing, assembly and manufacturing (ISAM)** economy: you cannot refuel, repair, inspect,
de-orbit, or assemble anything in space until you can reliably and safely touch it. The field
splits into **automated** docking (a spacecraft executes a pre-planned approach with human
supervision / go-no-go at gates — the mature, heavily-flown approach) and **full autonomy**
(the vehicle senses, decides and closes its own approach under uncertainty — the emerging,
rapidly-advancing frontier). The hard problems are not about *how to attach two objects* (that
hardware is decades old) but about **relative navigation and safety against a non-cooperative
target, with light-time latency and no room for error.** Right now (2026) we're at the
inflection: the first commercial RPO demonstrations have succeeded (Astroscale ADRAS-J), life-
extension servicing is a paying business (Northrop Grumman MEV), and the market is forecast to
grow roughly an order of magnitude by 2030 — even as a flagship US auto-servicing
mission (OSAM-1) was cancelled. The capability is real; the economics and trust are still being
proven.

---

## 1. What "autonomous docking" actually means (the mechanics)

Docking lives at the end of a longer chain called **Rendezvous and Proximity Operations (RPO)**.
It's useful to hold the whole chain, because "docking" is only the last, hardest few metres:

**Rendezvous (km → hundreds of metres):** the chaser acquires the target and flies from "far"
(where orbit mechanics dominate) down to "near". Includes phasing, far-field approaching along
the V-bar / R-bar corridors, and absolute-position navigation (GPS for Earth-orbiting targets,
onboard orbit propagation else).

**Proximity operations / close approach (hundreds down to ~1 m):** the chaser holds and
manoeuvres in close formation, building up a fine relative state solution. This is where latency
forces autonomy — at ~400 km LEO light-time is milliseconds, but at **geostationary (GEO)** and
beyond, one-way light time (~120-250 ms GEO, seconds-minutes for cislunar) makes closed-loop
joy-sticking from the ground impossible.

**Docking / berthing / capture (cm-scale contact):**
- **Docking** = the chaser actively propels itself into a **docking mechanism** on the target
  (hard or soft capture), using relative GNC to null velocity at contact. E.g. Apollo, Soyuz/Progress, Dragon to ISS.
- **Berthing** = a third party (often a robotic arm, e.g. the ISS Canadarm2) *grabs* the incoming
  vehicle and *places* it into a port — no active closing velocity by the vehicle itself.
- **Capture** (the non-cooperative / debris case) = grabbing a target with a robotic arm, net,
  harpoon, or magnet when there is **no compatible docking port** — the servicing-vehicle's job.

**Two target regimes that shape everything:**
| Regime | Target | Key difficulty |
|---|---|---|
| **Cooperative** | Target has a known docking port + navigation aids (reflectors, IDSS port, chaser-visible features) | Secure, well-trodden; the ISS/Soyuz/Dragon world. |
| **Non-cooperative** | Legacy/derelict satellite or debris — no port, tumbling, no comms, unknown state | The hard frontier: vision-based relative pose estimation from a tumbling, uncooperative body. |

### The sensing & decision stack (what "autonomous" is really made of)
- **Navigation:** GPS/carrier-phase differential GPS (cGPS) for cooperative Earth-bound; onboard
  propagation + ground-estimated ephemerides for the rest.
- **Relative-state sensing:** rendezvous sensors (RF ranging / laser ranging / wide+fine cameras),
  and increasingly **vision-based relative navigation (VisNav)** — LiDAR + monocular/stereo
  cameras + feature/pose-estimation models to solve "where am I relative to that tumbling body"
  in ~cm/mrad. This is the machine-learning-heavy slice that links directly to the
  `space/ai_robotics/` thread (vision, and modern learned pose estimators).
- **Guidance, Navigation & Control (GNC):** the closed-loop controller that computes and
  executes the burns. **Autonomy** here = the onboard system decides *what* to do under
  uncertainty — replanning approaches, aborting, or choosing a capture point — rather than a
  human commanding each burn.
- **Safety architecture:** the shielding that stops a servicing satellite from becoming another
  piece of debris. Typically layered: keep-out / control-tolerance zones, **autonomous
  collision-avoidance** (abort triggers), and a **sanctioned collision probability** budget.
  ADRAS-J demonstrated autonomous collision-avoidance in real operations (see §3).

---

## 2. Applications — why the world cares

Autonomous docking/berthing/capture is the *enabler*; the applications are where the value is:

| Application | What it is | Why it matters / status signal |
|---|---|---|
| **In-orbit repair** | Fix a failed/degraded subsystem (solar array, antenna, optics, propulsion) on a live satellite | Extends life; avoids premature deorbit of a $100M-$1B+ asset. |
| **Life-extension / refueling** | Dock a servicer, transfer propellant (or host a thruster pod) to a GEO satellite nearing fuel end-of-life | **The best-proven commercial case** (Northrop MEV, see §3). GEO fuel is the constraint; refueling is the recurring-revenue play. |
| **Debris removal / deorbit** | Approach, capture, then deorbit a non-cooperative upper stage or derelict satellite | Regulatory + sustainability driver; **Astroscale ADRAS-J is the first commercial demo** (see §3). |
| **In-orbit assembly & construction** | Dock/berth modules and trusses to build large structures too big for any single launch | The route to big telescopes, antenna arrays, and the "space base" the user named — avoids launch-volume limits. |
| **Space-station / base operations** | Routine cargo/crew docking (ISS today), plus future commercial stations (Starlab, Axiom), on-orbit propellant depots, and large crewed-basis construction | Needs safe, frequent, autonomous docking at scale. |
| **Satellite constellation / orbit transfer servicing** | A "space tug" docks to move a sat (orbit raising/relocation, end-of-life parking) | Logistics layer for large constellations; the "roadside services in space" framing. |
| **Spacecraft recovery of failed launch / payload** | Inspect or capture a payload stranded in a wrong orbit after a launch anomaly | Relates to a Chinese 2026 rocket-stage recovery demo (see §3). |

**The user's two named applications map cleanly:** *in-orbit repair* = the top row (and the
MEV/MRV commercial line); *efficient space-base construction* = the assembly/station rows (the
"heavy-lift-substitute" argument — an orbital assembly line can build things no single launcher
fairing can carry).

---

## 3. Current state (as of 2026-09-10) — what has actually flown vs. forecast

### The commercial successes (operational or just flown)
- **Northrop Grumman — SpaceLogistics.** The current commercial anchor.
  - **Mission Extension Vehicle (MEV-1, MEV-2):** the proven life-extension product. Launched
    2019/2020, MEV-1 docked to a **Intelsat 901** GEO sat in LEO first then relocated it; MEV-2
    docked to Intelsat 10-02/Palau. They rendezvoused and docked with *cooperative* GEO
    satellites, hosted a propulsion system to extend life. **This is the base of the OOS
    business that is actually making money.**
  - **Mission Robotic Vehicle (MRV):** first MRV launch news ~**July 2026** (SpaceX from Cape
    Canaveral) — brings *robotic* arm-mediated servicing: inspecting, and (with Mission
    Extension Pods) offering small thruster pods to many customers, plus capture of
    non-cooperative clients. Higher ambition than MEV (which needs a built-in port).
- **Astroscale — ADRAS-J (Japan).** Completed 25 Mar 2026 (after 293 days), then began deorbit.
  **World's first commercial mission to approach & image a large piece of actual debris** (an
  11 m × 4 m, ~3 t rocket upper stage). Demonstrated full-range RPO with a **non-cooperative**
  target: long-range approach, fly-around, closed to **15 m**, and **autonomous
  collision-avoidance** (even safe recovery after an abort). Phase I of JAXA's **CRD2**. Phase II
  (**ADRAS-J2**, planned launch FY2027) aims to actually *remove* the debris. Astroscale also has
  **ELSA-M** (LEO in-orbit demo, Isar launch), **LEXI** (GEO life extension), and a commercial
  **Docking Plate** product.
- **China — active RPO/service.** Multiple China on-orbit servicing/assembly demos and a 2026
  report of a rocket **stage-recovery** during launch.

### The flagship that was cancelled (important honest data point)
- **NASA OSAM-1 (On-orbit Servicing, Assembly, and Manufacturing 1)** — the marquee NASA
  mission to demonstrate servicing/assembly on orbit with a robotic arm. After years of cost
  growth and schedule slip, **NASA cancelled OSAM-1** (decision reported 2024, effective; NASA
  evaluated restructuring first). This is the clearest recent sign that **the hardware/robotics
  route to autonomous servicing proved harder and costlier than hoped** — the commercial,
  docking-first route (MEV) and the small-demo route (ADRAS-J) have outrun the big government
  flagship.

### Technology & standards landscape
- **Docking hardware is mature:** the **International Berthing and Docking Mechanism (IBDM)** /
  **International Docking System Standard (IDSS)** is the low-orbital interoperable standard;
  NASA Docking System (NDS), androgynous ports, soft/hard capture mechanisms are flown tech.
  The *mechanism* isn't the bottleneck — the **relative GNC + safety case** is.
- **Vision-based navigation (VisNav) is the active research/startup space:** ESA is funding
  start-ups like **Blackswan Space** (€600k ESA contract, Jan 2026) and European university work
  on VisNav for rendezvous/metrology and on GTO autonomous docking; there's a notable 2026
  **critical survey on uncertainty-aware, safety-critical deep learning for spacecraft relative
  pose estimation** — i.e. the field is now formally worrying about *certifying* learned pose
  estimators. This ties to the `ai_robotics/` safety-eval thread.
- **Market analysis:** ISAM/OOS market reports (e.g. Space Investments ISAM 2025 analysis) frame
  a transition from an emerging to a scaled market over the next 3-5 years, with GEO life-
  extension as near-term revenue and debris removal/assembly as the bigger long-term prize.

---

## 4. Engineering synthesis / what I'd actually flag

Multi-dimensional read, honest about the failure modes:

- **Docking-as-mechanism is solved; docking-as-autonomy is not.** Nearly every hard problem in
  this field is *relative navigation + trust under uncertainty*, not the latch. The "wow these
  two spacecraft touched" is decades old; the frontier is a **non-cooperative, tumbling target
  under light-time latency, with a certified collision-avoidance case.** That's why the
  machine-learning/vision slice (`space/ai_robotics/`) and the safety-eval thread matter here:
  learned pose estimators are powerful but hard to certify — exactly the physics-AI tension
  ESA's Hera sandbox and the embodied-intelligence campaign are probing.
- **Autonomy ≠ uncrewed.** The maturity skews *automated* (pre-planned + human go-no-go gates),
  which is what ISS/commercial crew/cargo actually flies today. **Full autonomy** — sensing +
  deciding with no ground — is only just being proven in the *non-cooperative* case (ADRAS-J's
  autonomous collision-avoidance is a landmark). The economic sweet spot is "autonomy where
  light-time makes ground control impossible," i.e. **GEO and beyond**, and **non-cooperative
  capture** where humans can't see/touch.
- **The commercial order is inverted from the R&D order.** The *paying, proven* business is
  **cooperative GEO life-extension** (MEV). The *flashy, less-proven* frontier — de-orbiting and
  autonomous non-cooperative capture — is where ADRAS-J is clearing the path. Anyone tracking
  this should keep the two buckets separate: one is a business, the other is a capability race.
- **The OSAM-1 cancellation is the honest caution.** It says government flagship robotic-
  servicing was over-ambitioned and over-budget. The field is pivoting to (a) simpler docking-
  first commercial servicers and (b) small, fast demos. That's a classic "prototype small,
  evaluate, then formalize" signal — and it validates the approach of incremental capability.
- **Natural cross-links for this workspace:** the **vision/relative-pose ML** layer connects to
  `space/ai_robotics/` (and the `ai/world-models` thread — a world model is a natural fit for
  predicting a tumbling target's pose over the approach); the **safety/certification** of learned
  GNC connects to `ai/ai-safety-eval-gap-2026`; and the whole ISAM economy is the concrete
  "application layer" that ESA's embodied-intelligence/robotics Roadmap and Hera's sandbox are
  building toward. This sub-folder belongs in `space/`, cross-linking to `ai_robotics/`.

---

## 5. Open questions worth pulling next (offers)

- **Deep-dive one program** for the mechanism-level detail you mentioned — **DONE** → see
  `mev-vs-adrasj-docking-approaches.md` (MEV/MRV vs ADRAS-J comparative).
- **The non-cooperative target problem** in depth: why tumbling + no-port + no-comms breaks
  classic RPO, and how VisNav/pose-estimation + world models attack it.
- **Why OSAM-1 died** (cost/schedule drivers) — the cautionary tale, and what it implies for
  commercial vs government routes.
- **Docking/berthing mechanism taxonomy + standards** (hard vs soft capture, androgynous, IBDM/
  IDSS, NDS) with a comparison table.
- **The ISAM market model** — $ forecast, who's buying, at what price per operation.

---

## 6. Sources (primary / near-primary first)

- **Astroscale ADRAS-J mission completion + deorbit** (25 Mar 2026): [astroscale.com](https://www.astroscale.com/en/news/astroscales-adras-j-mission-completes-operations-begins-deorbit) — 293-day RPO demo, 15 m approach, autonomous collision-avoidance, CRD2/JAXA Phase I, ADRAS-J2 FY2027.
- **Northrop Grumman — MRV launch (in-space servicing era)** [news.northropgrumman.com](https://news.northropgrumman.com/launch/northrop-grummans-mission-robotic-vehicle-launches-ushering-in-a-new-era-of-in-space-servicing) + [SpaceLogistics / MRV media kit](https://www.northropgrumman.com/what-we-do/events/mission-robotic-vehicle-mrv-media-kit) + [satellitetoday MRV summer launch](https://www.satellitetoday.com/technology/2026/05/19/northrop-grummans-first-mrv-readies-for-summer-launch-to-expand-the-space-servicing-toolkit/?itm_source=parsely-api).
- **OSAM-1 cancellation / restructuring:** [spacenews — NASA evaluating restructure](https://spacenews.com/nasa-evaluating-plan-to-restructure-osam-1-satellite-servicing-mission/), [azertag — NASA cancels OSAM-1](https://azertag.az/en/xeber/nasa_cancels_osam_1_satellite_servicing_technology_mission-2939211), [METI report context](https://www.meti.go.jp/meti_lib/report/2025FY/report_202605260205_0.pdf).
- **IBDM / IDSS docking standard:** [IAC paper — IBDM + International Docking System Standard](http://iafastro.directory/iac/archive/browse/IAC-15/B3/7/30720/); [ScienceDirect — IBDM design/qualification](https://www.sciencedirect.com/science/article/abs/pii/S2468896725001089).
- **Vision-based navigation / safety-critical deep learning:** [Blackswan Space ESA €600k VisNav contract](https://blackswanspace.com/2026/01/12/blackswan-space-secures-e600k-esa-contract-to-advance-vision-based-navigation-product/); [Critical survey — uncertainty-aware safety-critical deep learning for spacecraft relative pose estimation](https://www.sciencedirect.com/science/article/abs/pii/S0094576526004807).
- **Market / ISAM:** [Space Investments — ISAM Technologies & Logistics 2025 analysis](https://www.spaceinvestments.io/space-investment-reports/p/in-orbit-servicing-assembly-and-manufacturing-isam-technologies-and-logistics-for-sustainable-orbital-operations); [spaceinsider — ISAM/ISOM phase](https://spaceinsider.tech/2025/09/19/isam-and-isom-drive-forward-the-next-phase-of-space-persistence-not-disposability/).

_Unverified/estimated: fossil-market $ figures (I cite the analysis, not exact numbers), some
China program specifics (secondary press). MEV docking targets are from public Northrop/press
reporting._