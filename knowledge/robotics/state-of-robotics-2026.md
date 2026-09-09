# State of Robotics 2026 — where it is and where it's going

_Survey compiled 2026-09-10 for Shrey (engineer-depth, not a survey). Sits alongside
the photonics survey in style: multi-dimension, trend-forward, honest about what's
demo vs production._

_Working context: this workspace follows the "get present date first" rule. Article
dates below are what the sources report; treat all unit figures as vendor-disclosed
estimates. I flagged the handful of (unverified) figures._

---

## One-paragraph answer

Robotics is at the exact moment the photonics world was pleading for: the software
eye of AI has finally been pointed at the physical body. 2026 is the first year
humanoid robots have genuinely left the lab — on the order of 7,000–8,000
commercial humanoid units are deployed or shipped worldwide (mostly in structured
industrial runs), and warehouse/fulfillment robotics has crossed from novelty into
mainstream budgeting. But the *value* has migrated from the chassis to the brain:
the competitive moat is no longer "who builds the best arm" but "who trains the
best robot **foundation model**." Every frontier AI lab (Google DeepMind, OpenAI,
Alibaba/Qwen, NVIDIA) is now shipping or funding models designed to be the
"operating system" for any humanoid. The big, unresolved problems are not
mechanical — they are **data collection cost**, **autonomous failure recovery**,
and **safety certification**.

---

## 1. Two distinct industries wearing the same word "robotics"

Separating these is the single most useful mental model. They move on different
timelines and economics.

### A. Deployed, value-generating robotics (the mainstream)
The unsung story. Warehouses, logistics, and intralogistics are quietly automating
at scale. The **2026 Intralogistics Robotics Survey** (MHI / Peerless, n=166, Mar–Apr
2026) reports:

- **52%** of respondents already use robots, up from 48% — and the share with *no*
  plans fell from 9% to 3%. Essentially every company is at least considering it.
- **74%** say robotics **met** their business goals; 94% met/beat expectations on
  speed-to-results; 89% on process performance, safety, and investment risk.
- Typical project timeline is **6–12 months** inception→go-live — markedly faster
  than conventional automation.
- Adoption drivers ranked: reduced labor cost first (67% call it the single most
  important factor), then productivity/throughput. **Space** has emerged as a harder
  bottleneck than labor (you can't easily manufacture well-located square footage).
- Funding models maturing: hybrid CapEx/OpEx (36%), pure CapEx (36%), RaaS margin
  (29%); 53% of users buy hardware + subscribe to software.
- → [2026 Intralogistics Robotics Survey](https://www.materialhandling247.com/article/2026_intralogistics_robotics_survey_robotics_moves_into_the_mainstream/DK_Advascent)

This is the "tool" robotics market — autonomous mobile robots, sortation, piece
picking, palletizing. It is real, getting cheaper, and already paying for itself.
**Whole-warehouse "tote handling" robots are the proven entry** (see Agility Digit
below).

### B. Humanoid robots (the hype + the frontier)
The one in the news. Commercial reality as of early 2026, per Presenc AI's tracker:

| Player | Flagship | Deployed/shipped | Sites / notes |
|---|---|---|---|
| Tesla | Optimus Gen 3 | 1,000+ (in own factories) | Fremont, Austin, Berlin; internal use only in 2026 |
| Figure | Figure 03 | ~150 | BMW Spartanburg (30k+ cycles), BMW Leipzig (2026) |
| Apptronik | Apollo | ~80 | Mercedes, NASA, Jabil |
| Agility | Digit V6 | ~250 | Amazon, GXO Logistics, Schaeffler (100k+ tote moves) |
| 1X | Neo (Gamma) | ~120 pilots | Home-targeted, safety-focused |
| Sanctuary | Phoenix Gen 8 | ~40 pilots | Canada |
| Boston Dynamics | Atlas (electric) | ~50 pilots | — |
| Unitree | G1 / H1 | ~5,500 shipped | China; most third-party sales volume |
| Fourier / XPENG / UBTech | — | ~600 / 150 / 200 | China pilots |

Bottom line: roughly **7,000–8,000 commercial humanoids** operational worldwide
(vendor-disclosed estimates). Production deployments — robots running unattended
across shifts in real facilities — are still limited to a **handful** of enterprise
partners. The loudest commercial deals in 2026: **Mercado Libre** (Latin America's
largest marketplace) adopting Agility **Digit** for fulfillment at San Antonio, TX —
its first humanoid bet, mostly tote moving, an explicit pilot with options to scale
into Brazil — and Figure/BMW's multi-plant expansion.

Sources: [Presenc AI Humanoid Market Tracker 2026](https://presenc.ai/research/humanoid-robot-market-tracker-2026),
[Mercado Libre + Agility (Cointelegraph, 2026-01-10)](https://cointelegraph.es/news/mercado-libre-goes-beyond-bitcoin-and-announces-the-first-army-of-humanoid-robots-in-latin-america),
[Agility-NVIDIA partnership, Digit warehouse scale-up](https://www.tipranks.com/news/private-companies/agility-robotics-deepens-nvidia-partnership-as-digit-warehouse-deployments-scale)

---

## 2. The money: an unprecedented, lopsided investment wave

Humanoid companies combined raised **$12B+** between 2023 and early 2026 — VC that
in three years rivals the annual revenue of the *entire* established industrial-robot
market (~$16B/yr). Headline rounds:

- **Figure AI** ~$2.6B raised (Series B $675M Feb-2024; investors incl. Microsoft,
  OpenAI, NVIDIA, Bezos) — richest/strongest AI team.
- **Apptronik** closed a **$5.3B Series A** at **$6.2B** post-money (Feb 2026).
- **Physical Intelligence** — foundation-model specialist — ~$400M+ to ~$2B valuation,
  later reported raising at **$5.6B**. **Skild AI** at **$4.5B**. Both "general-purpose
  robot brain" plays.
- **1X** ~$500M (OpenAI-backed), **Agility** ~$600M (Amazon's Industrial Innovation
  Fund), **Unitree** ~$200M at ~$1B.

The **frame that matters:** capital has moved decisively toward the **model layer**,
not the metal. Presenc's own framing — "the model layer is where the long-term
economics are likely to concentrate" — matches the photonics-suite thesis that
software + margin-concentration beats commodity hardware. Sources:
[Robotics Center Humanoid Market Overview](https://www.roboticscenter.ai/blog/humanoid-robot-2026),
[Presenc tracker](https://presenc.ai/research/humanoid-robot-market-tracker-2026).

---

## 3. Where the action actually is: robot foundation models ("Physical AI")

The single biggest development of 2025–26. Frontier labs are racing to be the
**"operating system" for any robot**, not a chassis vendor. Three competitive
archetypes:

1. **Integrated manufacturers** (Tesla, Figure, 1X) — train proprietary foundation
   models exclusively on their own fleet data. Data = moat, but closed.
2. **Independent model labs** (Physical Intelligence, Skild AI, **NVIDIA Project
   GR00T**) — positioning as the "Android of robotics": one model any OEM can
   integrate. Cross-hardware is the pitch.
3. **Frontier-lab spin-outs / partnerships** — Google DeepMind, OpenAI (figure
   equity + renewed robotics effort), and Alibaba's **Qwen-Robot**.

Concrete, dated 2026 releases that make this real:

- **Google DeepMind — Gemini Robotics 2 (Jul 30, 2026):** a *three-model suite*
  rather than one model:
  - **Gemini Robotics 2** — core Vision-Language-Action (VLA) model for motor control.
  - **ER 2** — "the brain": multi-step task planning + **multi-robot fleet
    collaboration**.
  - **On-Device 2** — efficient local VLA that adapts to a new robot embodiment in
    **<200 examples / a few hours of data**, cutting cloud dependency.
  - Integrated on **Apptronik Apollo 2** (with 5-fingered, 22-DoF SharpaWave hands);
    Boston Dynamics and Agile Robots also partnering. Ships an **ASIMOV-Agentic**
    safety benchmark measuring refusal of unsafe actions.
  - This is the clearest statement that **the model, not the body, is the product.**
  - → [Forkast analysis](https://forkast.news/google-ships-the-first-ai-model-suite-that-controls-a-full-humanoid-from-feet-to-fingertips/), [eWeek](https://www.eweek.com/news/google-gemini-ai-controls-humanoid-robots/)

- **Alibaba — Qwen-Robot Suite:** open robot foundation models pitched as an
  "operating system for the robot economy" (vision-action world model + action model
  for a wide robot range). → [The Elec (orig. page 403)](https://www.thelec.net/news/articleView.html?idxno=11412),
  [Alizila intro (blocked) / Yahoo Tech](https://tech.yahoo.com/ai/meta-ai/articles/alibaba-building-qwen-robot-operating-223221358.html)

- **Europe / SPEAR-1** (INSAIT): first open robot foundation model trained on 3D
  data — a non-US/China counterweight. → [INSAIT](https://insait.ai/insait-unveils-spear-1-europes-first-open-robotic-foundation-model-trained-on-3d-data/)

### Generalist AI (GEN-1.5) — the one-demo "physical prompt" attack on the data bottleneck

The sharpest single example of foundation models eliminating the per-task data cost.
Reported 2026-09-10 (user lead), company-reported only:

- A **3–12 second demonstration** is loaded into the model's context window as a
  **"physical prompt"** (short-term memory). The robot then does the task with
  **zero training** — in-context learning, same family as feeding a VLM an image of
  an unseen task, but for physical control.
- **59%** average success across 10 tasks zero-shot (opening a jar, pulling money
  from a wallet); **83%** after 10 training steps / 5 minutes of data.
- Chains **two prompts** into longer sequences, **uses demos sourced from simulation**,
  and partly imitates human hand movement.
- **Why it matters for this survey:** it's the "simulation as a prompt" mechanism the
  user flagged — the model executes *novel* actions (including from a sim demo) that
  were plausibly **not directly in the sim data**. Generalist says these abilities
  **emerged** during **8+ months pretraining on interaction data** and were *never
  explicitly trained to do it* — i.e. out-of-distribution generalization, not replay.
- Funding: **~$3B valuation**, ex-Google founders, **NVIDIA-backed** (Jensen Huang; a
  report also names Fei-Fei Li and Bin Lin as investors) — a strong capital vote for
  one-shot in-context generalization as the data-bottleneck fix.
- **Caveat (important):** all *numbers and the "emergent, never-trained" claim come
  from Generalist's own blog* — **not independently verified**. Tasks are simple and
  short; prior academic work showed in-context learning only for a few task types;
  Generalist claims breadth. Treat the "action not in the sim data" as a
  *company-reported emergent-capability claim*, plausible and directionally big but
  unreplicated.
- → [The Decoder (EN)](https://the-decoder.com/gen-1-5-generalist-ai-teaches-robots-new-tasks-from-a-single-demo/),
  [The Decoder (DE)](https://the-decoder.de/gen-1-5-roboter-von-generalist-ai-lernen-neue-aufgaben-aus-einer-einzigen-vorfuehrung/),
  [yahoo finance — $3B valuation](https://uk.finance.yahoo.com/news/robotics-startup-generalist-reaches-3b-004059300.html),
  [electronicsforu](https://www.electronicsforu.com/news/new-robot-learns-physical-skills-from-brief-demonstrations),
  backup [edgen.tech](https://www.edgen.tech/zh-tw/news/post/generalist-gen-15-learns-robot-tasks-from-one-demo-59-success-rate)

> This is a textbook platform shift. Whoever owns the model that generalizes across
> bodies owns the industry the way Android owns phones. Hardware will commoditize;
> the 22-DoF hand is already the new GPU spec sheet, and it won't be the differentiator
> for long.

---

## 4. Reality check: the honest Technology Readiness picture

A TRL-style assessment (Robotics Center, Mar 2026) — what actually works today:

| Capability | Readiness | Verdict |
|---|---|---|
| Indoor flat-floor walking | TRL 8 (production) | All major platforms |
| Stair climbing | TRL 7 | Standard stairs only |
| Pick-and-place, known objects | TRL 7 | High reliability in structure |
| Pick-and-place, **novel** objects | TRL 5 | Foundation models enabling; still FLT |
| Dexterous in-hand manipulation | TRL 4 (lab demo) | Impressive, not production |
| Human-robot physical collaboration | TRL 3 | Safety cert is the bottleneck |
| General-purpose task learning | TRL 3 | Foundation models promising, not deployed |

**The hidden killer is data, not actuators.** Imitation learning (ACT, Diffusion
Policy) still needs **50–200 teleoperated demos per task**; a 20-task factory =
1,000–4,000 demos = 50–200 operator-hours. This is the real reason deployments
stall — and the exact thing foundation models (On-Device 2's "few hours of data")
are trying to eliminate. The 2027 projection is that the per-task demo burden drops
to **10–20 examples**, which would unlock multi-task fleets. **Generalist's GEN-1.5
pushes this to the limit — one demo as an in-context "physical prompt," zero
training** (see §3) — though unverified so far.

**Second killer: no autonomous failure recovery.** When the robot drops an object
or a sensor degrades, a human must reset it. This limits unsupervised operation to
tasks with >95% success, which is far harder in the wild than in a demo.

**Economic sketch** (mid-range, US): a $50K robot + $50K year-1 integration breaks
even against a single fully-loaded $45/hr worker after ~14 months *at parity
throughput* — but at 40–70% of human throughput, realistic break-even is 20–30
months. Economics flip strongly in favor of the robot with multi-shift (16hr/day)
or hazardous/labor-scarce work. Source:
[Robotics Center](https://www.roboticscenter.ai/blog/humanoid-robot-2026).

---

## 5. Price ladder (2026)

| Platform | Price | Status |
|---|---|---|
| Unitree G1 | ~$16,000 | Buying now; research/light-industrial |
| 1X Neo | ~$20,000 (announced) | Pre-order, late-2026 home target |
| Tesla Optimus Gen 3 | $20–30K (target) | Internal only in 2026 |
| Booster K1 (mid-tier) | $45–65K | Ordering now |
| Figure 03 / Apptronik Apollo | $200K+ | Enterprise contract only |
| Sanctuary Phoenix | ~$250K (est) | Enterprise contract only |

→ [Presenc pricing table](https://presenc.ai/research/humanoid-robot-market-tracker-2026)

---

## 6. Where it's going — 2–5 year outlook (consolidated)

Cross-referencing the three sources (Robotics Center timeline, Presenc deployments,
Forkast/DeepMind trajectory):

- **2026 (now):** single-task, structured deployments are viable. 10–50 units per
  major customer (BMW/Figure, Amazon/Agility, Tesla-internal at 1,000+). Foundation
  models begin cutting per-task demo cost.
- **2027:** multi-task in structured environments as foundation models drop the
  demo burden to ~10–20/task. Fleets of 50–200/facility become economical. Tesla
  projects consumer Optimus; 1X home units start shipping.
- **2028–29:** semi-structured retail/healthcare/hospitality begins as safety cert
  matures; entry humanoids drop below ~$20K. **Homes: 2027–28 at small volume,
  2030+ at meaningful penetration** (independent consensus).
- **Structural prediction with high confidence:** the winner is decided on the
  **model layer + safety certification + data flywheel** — not the arm. Hardware
  becomes interchangeable; the Android-of-robotics play (model sells to any OEM)
  is the durable margin.

---

## Sources (primary when possible; some pages 403-blocked, noted)

- [Robotics Center — Humanoid Robots in 2026: Complete Market Overview](https://www.roboticscenter.ai/blog/humanoid-robot-2026) — TRL, economics, players, deployment patterns
- [Presenc AI — Humanoid Robot Market Tracker 2026](https://presenc.ai/research/humanoid-robot-market-tracker-2026) — unit counts, deployments, pricing, foundation-model framing
- [MHI/Peerless — 2026 Intralogistics Robotics Survey](https://www.materialhandling247.com/article/2026_intralogistics_robotics_survey_robotics_moves_into_the_mainstream/DK_Advascent) — mainstream warehouse adoption data
- [Forkast — Google Gemini Robotics 2 (whole-body model suite)](https://forkast.news/google-ships-the-first-ai-model-suite-that-controls-a-full-humanoid-from-feet-to-fingertips/)
- [Cointelegraph — Mercado Libre adopts Agility Digit](https://cointelegraph.es/news/mercado-libre-goes-beyond-bitcoin-and-announces-the-first-army-of-humanoid-robots-in-latin-america)
- [INSAIT — SPEAR-1 open robot foundation model](https://insait.ai/insait-unveils-spear-1-europes-first-open-robotic-foundation-model-trained-on-3d-data/)
- Backup/blocked: [Yahoo Tech — Alibaba Qwen-Robot as OS for robot economy](https://tech.yahoo.com/ai/meta-ai/articles/alibaba-building-qwen-robot-operating-223221358.html); eWeek Gemini Robotics 2 (403): [eweek.com](https://www.eweek.com/news/google-gemini-ai-controls-humanoid-robots/)

_Unverified/estimated figures are marked inline or are vendor-disclosed estimates
(Tesla 1M/yr capacity target, Physical Intelligence $5.6B, Figure $200K+ pricing)._