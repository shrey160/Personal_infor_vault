# ESA's Embodied Intelligence for Autonomous Space Systems — campaign study

_Named "Space AI and robotics" thread opener. Study compiled 2026-09-10 for Shrey_
_from primary sources. Treat all figures as campaign-disclosed; I flag anything_
_estimated. Sits in `knowledge/space/ai_robotics/` as the first interdisciplinary_
_space × AI/robotics artifact._

_Working context: per the workspace "get present date first" rule, today is
2026-09-10. The campaign's idea phase has already **closed** (deadline 10 Aug
2026, rescheduled to 12 Aug 2026) — this study is a backward-looking analysis of
the call, not a submission. [OSIP confirms campaign status = "Evaluation", Closed Oct 30.]

---

## Why this is the right first "Space AI and robotics" study

This is the sharpest possible anchor for a Space × AI/robotics knowledge thread,
and not only because it's ESA's flagship call. It is the institutional statement
of a definitional shift we've been tracking separately in AI and robotics land:
**the value is migrating from the isolated component to the tightly-integrated
system.** ESA is not asking Europe to build "a better rover" or "a smarter DNN" —
it is asking for the *integration* of body + brain so that the robot closes its
own sense→decide→act loop. That is precisely the "physical AI" framing already in
`robotics/physical_ai/`, applied to the harshest operating environment that exists.

**The one-line thesis:** ESA wants to move space robotics from *autonomation of
functions* to *autonomy of the system* — and it is spending Discovery/Preparation
money to seed the roadmap and demonstrators for lunar-surface embodied
intelligence, aligned to its Technology & Strategy 2040.

---

## 1. What the campaign is (facts)

- **Name / id:** Embodied Intelligence for Autonomous Space Systems,
  OSIP campaign **Ca-2026-00061** (Open Space Innovation Platform).
- **Launch:** 30 June 2026, through ESA's **Discovery Element** on OSIP.
- **Closes / status now:** idea phase closed **10 Aug 2026** (rescheduled to
  **12 Aug 2026** end-of-day due to an OSIP technical issue). OSIP lists the
  campaign status as **Evaluation**, with the close milestone on **Oct 30**.
  As of today (2026-09-10) ideas are being evaluated.
- **What's invited:** early-stage concepts, feasibility studies, enabling
  technology developments, and **mission concepts** that bring embodied
  intelligence to autonomous space robotics.
- **The definition ESA is pushing:** embodied intelligence = systems that
  *tightly integrate perception, cognition, learning, and physical interaction*
  to act autonomously in complex, unpredictable environments. The explicit target
  is to move "beyond today's space autonomy, which largely automates isolated
  functions," toward **system-level autonomy** where perception, decision-making,
  control, and adaptation operate as one.

### Why now (the stated driver)
The environments are ones where **teleoperation is impossible**: permanently
shadowed lunar craters, lava tubes, unstructured planetary terrain. Communication
latency, blackout windows, and GNSS-denied conditions mean the robot itself must
close the loop between what it senses and what it does. Today's robots "are not
built for that level of independence" (ESA). Key voices quoted in the ESA article:
**Lisa Denzer** (AI-Lab Lead), **Luis Mansilla** (AI System Engineer),
**Jai Grover** (Scientific Coordinator, Advanced Concepts Team).

---

## 2. Three target scenarios

| # | Scenario | What it means on the surface |
|---|---|---|
| 1 | **Autonomous surface & subsurface exploration** | Long-traverse science, surveys of permanently shadowed regions (PSRs), lava tubes, GNSS-denied cryogenic zones, mobility on unstructured terrain (regolith, boulders, slopes, ice), resilient operation under partial failures, and **opportunistic science** — the robot spots something interesting and acts on it with no human in the loop. |
| 2 | **Fully autonomous ISRU** | End-to-end lunar/planetary resource utilisation: prospecting, excavating, beneficiation of regolith for water/oxygen/construction feedstock; site preparation (levelling, berms, landing-pad assembly); autonomous construction of habitats/power relays/storage tanks; **coordination of heterogeneous specialised agents** (excavator ↔ hauler ↔ processor) sharing a common world model. |
| 3 | **Robotic support to long-term human presence** | Robots working *alongside* crews: habitat inspection/maintenance, ECLSS + power-system upkeep during crewed *and* uncrewed periods; and close-proximity human-robot teamwork — reading/predicting human intent, accepting natural-language + gestural instruction, explaining actions. |

---

## 3. Two approaches (how ESA wants ideas to come in)

| Approach | Core idea | What it includes |
|---|---|---|
| **1. Embodied co-design** | Design the robot's **body** (morphology, sensors, actuators) and **brain** (perception, control, decision-making, learning) *jointly, as one system* — a paradigm beyond traditional hardware/software co-design. | **Morphological computation** (co-design morphology+control+decision for task/environment, e.g. per-planetary-gravity); **adaptive & bio-inspired platforms** (soft, morphologically adaptive designs for unstructured terrain). |
| **2. Physical intelligence** | Pair emerging robotic platforms with **advanced AI** for perception, reasoning, learning, adaptation. | **Planetary foundation models** (adapt large multimodal FMs to lunar/planetary conditions as reusable backbones); **planetary world models** (specialise WMs or couple with physics-informed/causal models for onboard state estimation + multi-step prediction); **online adaptation**; **swarm / multi-agent cooperative systems**; **multimodal perception & sensor fusion** (vision, LiDAR, stereo, RADAR, haptic, thermal) incl. tightly-coupled perception–action loops beyond classical sense–plan–act. |

**What's explicitly OUT of scope** (this is the guardrail that defines the "system"
bar): isolated AI components (single DNNs) unless demonstrably embedded in a
broader system-level architecture; function-level automation (perception/control/
planning alone without tight integration); and **orbital applications** — the
campaign targets *lunar or planetary surface* environments (ground or
surface-proximal/aerial-drone OK, orbits not).

> Direct tie to existing knowledge: ESA's "physical intelligence" lever list —
> *planetary foundation models, world models, multi-agent cooperation* — is the
> same stack already studied here in `robotics/physical_ai/state-of-physical-ai-2026.md`
> (sense-decide-act physical AI, NVIDIA Cosmos/GR00T, PI π0.7) and in
> `ai/world-models-jepa-2026.md`. ESA is the institutional demand signal that this
> stack must survive **gravity, latency, power and radiation constraints** — the
> space-specific filter nothing in terrestrial physical AI has to face.

---

## 4. From idea to activity — the mechanics

### Two-step process
1. **Idea** (via OSIP) → evaluated on *innovation potential*: how clearly a new
   capability is described and how convincingly it goes beyond published/
   demonstrated work. Idea phase closed 12 Aug 2026; evaluation followed later in
   August 2026.
2. **Full proposal** (via **esa-star**) → authors of retained ideas submit a full
   proposal; **proposal evaluation expected October 2026**. Second-round criteria
   are stated in the CFP data pack (foreseen: relevant background/experience;
   innovation/quality/suitability; adequacy of planning, organisation, admin
   tender compliance).

### Implementation paths / funding
- **Discovery element** (open to all ESA Member & Associate Member States, no
  national support letters; funded under ESA Basic Activities):
  - **Co-sponsored research** — PhD / postdoc support (mature idea developed on OSIP).
  - **Studies** of early-stage concepts.
  - **Early technology development** up to **TRL 4**.
- **Preparation element** — **small system studies**: pre-Phase A analysis of a
  mission concept leveraging embodied intelligence. **Up to two parallel
  activities**, **max €150,000 each**, **max six months** duration, with a final
  review ~1 month after the report draft.

### Timeline (campaign + activities)
- 30 Jun 2026 — launch ·
- **12 Aug 2026 — idea close (rescheduled from 10 Aug)** ·
- Late Aug 2026 — idea evaluation ·
- **Oct 2026 — proposal evaluation** ·
- Common Kick-Off (parallel activity kick-off) ·
- **End of studies = Nov/Dec 2027 (Common Final Presentation day)**.

### ESA facilities that can support selected activities (self-managed by industry)
- **LUNA** analog lunar facility (Cologne, DE) — [luna-analog-facility.de](https://luna-analog-facility.de/en/)
- **Automation and Robotics Laboratories** (Noordwijk, NL) — [technology.esa.int](https://technology.esa.int/lab/automation-and-robotics-laboratories)

### Sourced prior research ESA points to (useful as background anchors)
- Space gaits · novelty search for soft-robotics co-design · a competition on
  **morphing rovers** · memetic co-design of 2D locomotion — these give the
  flavour of ESA's Advanced Concepts Team (ACT) body-brain co-design lineage.
- References include a citation on analysis of the **permanently shadowed region
  of Cabeus crater** (lunar south pole) and **cryogenic robotic swarms for PSRs** —
  i.e. the exploration scenario is backed by genuine scientific targets.

---

## 5. Engineering synthesis / what I'd actually flag

Reading this as an engineer (multi-dimensional, honest about hard constraints):

- **The binding constraint is the *edge* power/compute/radiation budget** — this
  is the differentiator from terrestrial physical AI. The ESA pitch leans on
  "limited power and computing to restricted communications"; any credible idea
  must show the sense→decide→act loop closing *within* a flight-like compute
  envelope, not at datacenter scale. World models that are small/prior-constrained
  (cf. the JEPA line — compact latent transitions) fit this better than huge
  generative backbones.
- **The "system, not component" bar is the hard filter.** The single most
  useful mental model here: a proposal is in-scope only if removing the tight
  integration between perception–decision–control–adaptation would break its
  core claim. That mirrors the prior finding that the moat has moved to the
  *integrated model layer* — here applied at the whole-robot level.
- **Coordination of heterogeneous agents sharing a common world model** (ISRU
  scenario) is the most architecturally ambitious ask — it combines the world-model
  thread with multi-agent cooperation and is likely where the roadmap money goes
  for infrastructure.
- **Validation path exists in Europe**: LUNA + the Noordwijk robotics labs give a
  grounded test route from idea → TRL 4, which is exactly the "prototype small,
  evaluate, then formalize" cadence Earth-based physical AI is also converging on.

---

## 6. Sources (primary first)

- **ESA news article:** [ESA calls for ideas to give space robots embodied intelligence](https://www.esa.int/Enabling_Support/Preparing_for_the_Future/Discovery_and_Preparation/ESA_calls_for_ideas_to_give_space_robots_embodied_intelligence) (10 Jul 2026)
- **OSIP campaign page (full brief):** [Embodied Intelligence for Autonomous Space Systems — Ca-2026-00061](https://ideas.esa.int/core/servlet/hype/IMT?userAction=Browse&templateName=&documentId=b076d14298d4e389a910ecd628361849)
- **OSIP banner image:** [Embodied Intelligence for Autonomous Space Systems](https://www.esa.int/ESA_Multimedia/Images/2026/07/Embodied_Intelligence_for_Autonomous_Space_Systems)
- **ESA Technology Vision 2040** (referenced anchor): [esamultimedia.esa.int/docs/technology/Technology_2040.pdf](https://esamultimedia.esa.int/docs/technology/Technology_2040.pdf)
- **LUNA analog facility:** [luna-analog-facility.de](https://luna-analog-facility.de/en/)
- **Automation and Robotics Laboratories (ESTEC):** [technology.esa.int/lab/automation-and-robotics-laboratories](https://technology.esa.int/lab/automation-and-robotics-laboratories)

_Ties into existing knowledge — `robotics/physical_ai/state-of-physical-ai-2026.md` (the terrestrial
physical-AI stack ESA now wants for space), `ai/world-models-jepa-2026.md` (compact world models for
onboard use), `robotics/state-of-robotics-2026.md` (robot foundation models), `photonics/state-of-photonics-2026.md`
(edge-vs-datacenter compute)._

_Cross-thread observation: this is the **institutional (ESA) counterpart of the commercial
physical-AI race** — same definitional pivot, opposite side of the funding fence. Worth
tracking whether ESA's roadmap money and the NVIDIA/Physical-Intelligence frontier
converge on the same architecture (integrated model + world-model-driven autonomy)._