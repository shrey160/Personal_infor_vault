# ESA Hera — flying software laboratory for deep-space onboard autonomy

_Study compiled 2026-09-10 for Shrey from primary sources. Second study in the
interdisciplinary **Space AI and robotics** sub-folder (`knowledge/space/ai_robotics/`),
pairing with the ESA embodied-intelligence campaign study
(`esa-embodied-intelligence-space-2026.md`). Treat all figures as campaign-disclosed;
I flag estimated/forward-looking items._

_Working context: per the "get present date first" rule, today is 2026-09-10. **Unlike**
_the embodied-intelligence call (already in Evaluation), this Hera call (Ca-2026-00066)_
_is still **open for idea submission*** as of today — OSIP shows ~5 days left in the
_Submission phase (Discussion from Sep 15, Evaluation from Oct 15). A live opportunity.*_

---

## Why this pairs with the embodied-intelligence study

The embodied-intelligence campaign (Ca-2026-00061) is ESA **asking Europe what the
*architecture*** of space physical AI should be — a roadmap+demonstrator play. The Hera
call (Ca-2026-00066) is ESA **opening a real deep-space spacecraft to run guest
*software** in a protected sandbox — an *operational testbed* play. Read together:

> **Embodied-intelligence = the "what should we build" question.**
> **Hera = the "how do we safely test experimental onboard autonomy in flight" answer.**

Both are the institutional side of the same pivot the physical-AI thread tracks
(`robotics/physical_ai/`): value moving from isolated functions to integrated,
autonomous systems. Hera is the **edge-deployment reality check** the roadmap needs —
any credible embodied-intelligence idea must eventually run on hardware like Hera's
dual-core LEON OBC, ~150 M km from Earth, under 2–3-hour execution windows.

**One-line thesis:** ESA is turning its asteroid-deflection flagship into the world's
most remote software sandbox — a once-in-a-generation chance to validate onboard
autonomy, edge compute and resilience in *real* deep-space conditions, not a sim.

---

## 1. What the mission is (facts)

- **Hera** is the ESA spacecraft built as Europe's contribution to **AIDA** (Asteroid
  Impact & Deflection Assessment) alongside NASA's **DART**. Launched **2024**; in 2022,
  NASA's DART deliberately impacted **Dimorphos**, the first Solar System object
  transformed by human action (shifted its orbit around parent **Didymos**).
- Hera arrives at the Didymos/Dimorphos system **this autumn (2026)** to perform a
  close-up **"crash scene investigation"**: weigh Dimorphos, dissect the crater/reshaping
  DART left, resolve how asteroids respond to intentional impact — questions at the heart
  of **planetary defence**.
- **Extended Mission Phase:** in **mid-2027**, once the primary planetary-defence goals
  are complete, Hera becomes a **flying software laboratory 150 million km from Earth**.
- **Vantage reality:** communication delays reach **tens of minutes (up to ~40 min)**;
  during the experiment campaign the spacecraft is expected to operate at distances from
  the Didymos system of ~**5–30 km**. Ground support cannot be taken for granted —
  exactly the "can't phone home" regime the embodied-intelligence call describes.

---

## 2. The technical setup — how guest software runs safely

The nub of the design, and the reason this is both exciting and genuinely hard:

- **Dual-core LEON-based On-Board Computer (OBC):**
  - **Core 0** runs the **mission-critical flight software** — spacecraft control,
    nominal operations. Untouchable.
  - **Core 1** is the **secure "sandbox"** dedicated to **third-party experimental
    software** (ESW = Experimental Software).
- **Software engineer Jorge Lopez Trescastro** on the mechanism: "One core will operate
  the actual spacecraft, while the other has a safe sandbox environment that has been
  optimised to host guest software. Access to spacecraft instruments and subsystems
  will be fully facilitated, while the hosted software will only run for two to three
  hours at a time, and be shut off immediately if any problems are identified."
  (Note: the ESA article says **LEON3**; the OSIP call says "LEON-based" — the LEON family
  is a European-developed SPARC-compatible processor lineage. I've flagged the exact
  variant as the article's claim.)
- **Execution model:** ESW runs as a **guest application** under an **Experimental
  Software Framework**, intentionally constrained for strict separation:
  - **No direct access to spacecraft hardware** — all I/O through platform interfaces.
  - **Automatic stop on anomaly/Safe Mode** — ESW is killed immediately by flight
    software if the spacecraft detects an anomaly; experiments must tolerate unexpected
    interruption (no graceful shutdown).
  - **Strict memory protection** — ESW lives in a dedicated memory-protected region;
    any access outside it (incl. Core 0 memory or hardware registers) ⇒ **immediate
    termination**.
  - **Limited execution time** — not continuous; **up to ~10 h/day total**, individual
    experiments typically **2–3 h/day windows**.
- Inputs must be realistic about the **resource envelope**: execution time per run, CPU
  utilisation %, RAM in kB, data-pool categories, and expected telemetry generation
  (housekeeping/events/science) per window.

---

## 3. What ESA is hunting for (capabilities + idea types)

Three capabilities are the explicit target:

| Capability | Meaning in Hera terms |
|---|---|
| **Autonomy** | Keep progressing toward mission objectives with no ground support — onboard GNC support, FDIR decision logic that selects actions/data products despite ~40-min delays. |
| **Edge computing** | Useful onboard processing of data (e.g. asteroid imagery) so only the **most relevant info** is downlinked — features, event detections, compressed/prioritised products. Maximise value per bit of bandwidth. |
| **Resilience** | Experimental algorithms operating safely *alongside* mission-critical software without compromising spacecraft safety (the Core-1 sandbox itself is the resilience demo). |

Idea categories ESA lists:
- **Onboard image processing & feature tracking** — landmarks, target detection, feature
  tracking across image sequences, compact descriptors for efficient downlink, visual
  navigation support, autonomous image selection/prioritisation.
- **Autonomous decision logic** — selecting observation opportunities, health monitoring,
  condition detection, auto-generation of reports/recommendations.
- **Advanced GNC** — robust sensor-data fusion incl. vision-based sensors; globally
  optimal guidance (convex optimisation) relevant to rendezvous, proximity ops, sample return.
- **Data compression, security & prioritisation** — reduce downlink, confidentiality/
  integrity, **post-quantum cryptography**, intelligent prioritisation.
- **Inference for anomaly detection / science classification** — lightweight onboard AI/ML
  anomaly detection, scene classification, science-event detection.
- **New deep-space operational concepts** — cut continuous ground intervention.

---

## 4. The process, timeline & what the idea needs

### Two-phase process
1. **Idea (Phase 1)** — via OSIP, **max 10 pages**. Must cover: the problem; the
   solution (algorithm/functionality); **technical feasibility** (confirm runs on a LEON3
   architecture, operates asynchronously, fits a 2–3 h daily slot); **compliance** with the
   technical/operational requirements + Annex A interface API; **benefits quantified**
   (e.g. "reduces downlink volume by 50%"); **maturity** (tested in sim/drone/CubeSat?);
   **operational concept**; **resource estimates** (time/CPU/RAM/telemetry); management &
   team (2–3 pages).
   - Evaluated on: novelty, scientific+operational value, technical credibility, **compliance
     with sandbox/spacecraft safety constraints**, feasibility within operational windows.
2. **Experiment Implementation Package (Phase 2)** — for retained ideas → flight-ready
   implementation + docs: **Design Definition File (DDF)**, **Software User Manual (SUM)**,
   **Interface Control Document (ICD)**, **V&V Test Plan + Report**, and **full C source
   code** with build instructions.

### Timeline
- Call launched; **idea submission open now** (~5 days left; OSIP: Discussion from **Sep 15**,
  Evaluation from **Oct 15 2026**). The ESA feature article adds: ESA selects winning ideas
  **mid-October 2026**; full implementation package + source code due **31 May 2027**;
  planned operation for a **one-month period during August 2027** (campaign page says
  **nominal 4 weeks in August 2027**).

### Recognition (not automatic funding)
ESA does **not** guarantee a financial award — it "recognises the most successful"
experiments after the campaign: a jury assesses results (quality/relevance, scientific/
operational value, innovation, technical robustness, use of the environment, clarity/
reproducibility, relevance to future missions). Recognition may include a **financial
award**, ESA-channel communication, and a **dedicated Hera trophy**. All teams selected for
implementation get invited to present results (ESTEC and/or remote). Limited to teams from
**ESA Member States and collaborating agencies**. Open to ideas from **all ESA member states**.

---

## 5. Engineering synthesis / what I'd flag

Multi-dimensional take, honest about the tradeoffs:

- **The precedent that frames expectations: OPS-SAT.** ESA's earlier CubeSat flying
  laboratory famously ran *standard software* in space — including a **game of Doom**.
  Hera "pushes that same spirit further" (ESA). So the cultural read is: some payloads will
  be headline/gimmick experiments, but the *capability* being proven is the **safe
  third-party-execution architecture** — which is the real product. The Doom precedent is
  precisely why the **memory-protection + auto-kill** sandbox matters: ESA has to let people
  run arbitrary code without ever letting it touch Core 0.
- **The "quantify the benefit" bar is the filter.** The call explicitly wants numbers
  ("reduces downlink by 50%"). Combined with the strict feasibility demands (LEON3,
  asynchronous, 2–3 h window, kBs of RAM), this strongly favours **small, measurable,
  well-scoped experiments** over grandiose autonomy demos. The resource-tightness is the
  same edge-compute constraint the embodied-intelligence study flagged as space's
  differentiator from terrestrial physical AI.
- **Natural tie to existing knowledge:** the "edge computing / downlink-only-what-matters"
  ask maps directly to the compact-**world-model** direction (`ai/world-models-jepa-2026` —
  small/prior-constrained latent transitions, the onboard-fit candidate) and to the
  **post-quantum cryptography** angle for data security. A proposer would be well served by
  an edge-deployable world model or a lightweight anomaly-detection classifier — both fit the
  2–3 h, kB-RAM envelope and have a crisp, quantifiable claim.
- **The resilience experiment is meta:** proving an experimental process can run 2–3 h/day,
  survive arbitrary auto-kill, and never corrupt Core 0 *is itself* the resilience demo — the
  architecture is the deliverable. That's a genuinely rare, once-in-a-generation testbed.

---

## 6. Sources (primary first)

- **ESA feature article:** [Your chance to run software in deep space on ESA's asteroid mission](https://www.esa.int/Enabling_Support/Space_Engineering_Technology/Your_chance_to_run_software_in_deep_space_on_ESA_s_asteroid_mission?t=3) (05/08/2026) — includes the quoted passages (Carnelli, Trescastro, Pilz), LEON3 dual-core, OPS-SAT/Doom precedent, mid-2027 primary-mission-complete timing.
- **OSIP call page:** [Call for Ideas: Autonomous Software Experiments on Hera — Ca-2026-00066](https://ideas.esa.int/core/servlet/hype/IMT?documentTableId=8527279057944083272&userAction=Browse&templateName=&documentId=76590fb19b5e6424d8862a329c2884b1) — full brief: capabilities, idea categories, constraints/safety model, execution model, two-phase process, timeline, recognition.
- **Hera mission page:** [ESA — Hera Space Safety](https://www.esa.int/Space_Safety/Hera)
- **OPS-SAT precedent:** [Mission complete for ESA's OPS-SAT flying laboratory](https://www.esa.int/Enabling_Support/Operations/Mission_complete_for_ESA_s_OPS-SAT_flying_laboratory)
- **DART impact:** [First kinetic impact test succeeds in shifting asteroid orbit](https://www.esa.int/Space_Safety/Hera/First_kinetic_impact_test_succeeds_in_shifting_asteroid_orbit)

_Ties into existing knowledge — `knowledge/space/ai_robotics/esa-embodied-intelligence-space-2026.md`
(the roadmap counterpart in the same sub-folder), `robotics/physical_ai/state-of-physical-ai-2026.md`
(edge-deployment reality of physical AI), `ai/world-models-jepa-2026.md` (compact world models as the
onboard-fit candidate), `photonics/state-of-photonics-2026.md` (constrained low-power edge compute)._

_Cross-thread observation: the embodied-intelligence roadmap (Ca-2026-00061) and the Hera
testbed (Ca-2026-00066) are two halves of the same institutional story — ESA is not just
"asking for" space physical AI, it is simultaneously building the *safe test path* to
deploy it. Any serious Space AI and robotics thread should track the convergence: will the
winning embodied-intelligence architectures end up validated on Hera's Core-1 sandbox?_