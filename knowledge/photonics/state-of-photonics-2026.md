# State of Photonics — 2026

_Engineer-depth survey. Compiled 2026-09-10 from public sources (2025–2026).
Scope: the present state of photonics as it collides with AI, communications, and
quantum tech. Each section notes what's real, what's hardware-in-the-lab, and the
multi-factor tradeoffs that actually drive the conclusion._ Some claims are vendor
headline numbers — flagged where unverified.

---

## The through-line: it's no longer "replace electronics"

The current story is **not** "photonic transistors replace transistors." It's three
distinct plays that all happen to be photon-based, each with a different maturity:

1. **Photonics for AI** = the *interconnect/near-term* (winning) play: move data
   between accelerators and inside the rack with light (bandwidth, energy, reach).
   Analog photonic *compute* is real but early and niche.
2. **Photonics for communications** = blue-ocean **deploying** today: silicon
   photonics / co-packaged optics (CPO) in data centers, plus hollow-core fiber in
   the field. This is where photonics is already a P&L line item.
3. **Photonics for quantum** = longest-horizon: photonic quantum computing
   (mixed-readiness, two very different architectures) and quantum *networks* /
   QKD, which are in early-landmark deployment.

The single most important 2025–2026 event is that **the AI industry adopted
photonic interconnects as a scaling necessity**, mainstreaming a silicon-photonics
supply chain — and that same supply chain is the one quantum/photonic hardware
leverages.

---

## 1. Photonics & AI — two sub-fields, very different maturities

### 1a. Photonic interconnects (scale-up/scale-out) — near-term, dominant

**Why now:** AI scale-up fabrics (hundreds/thousands of XPUs per domain) are
bandwidth- and power-limited, and the bottleneck is **electrical bandwidth
density**, not the fiber. Long-reach electrical SerDes deliver ~1–2 Tbps per mm of
chip "shoreline"; UCIe over advanced packaging reaches ~8–20 Tbps/mm but stays
inside the package. Photonics breaks that shoreline bound by putting the
electrical-to-optical interface across the whole die / near the package.

**Key 2026 markers:**
- **NVIDIA NVLink Fusion going photonic (GTC Taipei 2026).** NVIDIA nominated
  photonics for its scale-up fabric and co-authored the **OCI MSA** optical layer:
  bidirectional fibers, **DWDM in the O-band**, **micro-ring modulation**, external
  laser sources. This is the single biggest validation event.
- **Lightmatter** (NVIDIA-aligned): Passage platform across form factors — EVK50
  (800 Gbps/fiber), EVK100 (1.6 Tbps/fiber), and **M1000 reference: 114 Tbps
  aggregate bidirectional at ~2.3 pJ/bit incl. laser**. The M-series is a 3D
  photonic interposer pushing I/O to ~1 Tbps/mm² areal density. Their Guide 1
  external laser does 51.2 Tbps on a 16-λ DWDM grid (1310 nm), replacing ~9
  conventional ELSFP modules, with self-healing.
- **NVIDIA CPO switches** with ecosystem partners (co-developed DGX photonic
  hardware, announced ~Mar 2025, escalating since) as part of a "gigawatt AI
  factory" power agenda; NVIDIA reported spend on photonics around **US$6.5B** to
  fix the copper bottleneck.
- **Optical circuit switching (OCS)** scaling — driven by AI inference workloads;
  market forecast >**US$8B by 2030**.

**Factor that drives the conclusion:** energy + bandwidth-density, not raw fiber
capacity. Vendors' pJ/bit numbers (2.3 pJ/bit at Lightmatter) are marketing
figures; the credible direction is a large real step down from electrical SerDes
per-bit energy.

### 1b. Analog photonic compute — real, early, energy-argument-backed

The pitch: do the **linear algebra in light** (optical matrix multiply), cut energy
at the source. Concrete 2026 artifacts:
- **Lightelligence PACE 3** — 256×256 photonic matrix; billed as "first
  real-world deployed photonic computing," shown at **WAIC 2026.**
- **Q.ANT** (Stuttgart, TFLN / thin-film-lithium-niobate photonic PICs): second-gen
  Native Processing Unit ran a **diffusion model** and an **xLSTM time-series model
  at ISC HPC 2026**; claims ~30× energy efficiency vs. classical on equivalent
  matrix ops at the photonic-circuit level; first commercial orders via IONOS;
  deployed at Leibniz Rechenzentrum and Jülich HPC; a PyTorch→photonic **compiler**
  (Daisytuner) exists for object detection.
- **Company valuations/funding confirm the thesis is being financed broadly** (see
  quantum section — some quantum/photonic players are NVIDIA-backed at $7B+).

**Honest caveat (engineer's take):** analog optical compute is excellent at the
arithmetic step but must solve the **conversion tax** (ADC/DAC, opto-electronic
boundaries dominate energy and latency in a real pipeline), precision limits, and
lack of a mainstream toolchain/warp. The near-term commercial winners are the
interconnect plays; analog compute is a bet on a future where matrix ops dominate
enough to outweigh conversion overhead. This is the part of "photonic AI" to
treat with most skepticism.

---

## 2. Photonics & communications — deploying today

This is the established, revenue-yielding domain, now super-charged by the AI
buildout.

- **Hollow-core fiber (HCF) hits records.** YOFC claimed the *world's first
  1.2 Tb/s-per-wavelength* transmission over hollow-core fiber (Jun 2026);
  FiberHome (烽火通信) showed three HCF results at IEEE SUM 2026; there is active
  research on **hybrid SMF/HCF networks** with selective, bidirectional HCF
  deployment. HCF's draw: ~1.5× lower latency (closer to light-in-vacuum speed,
  ~1.0–1.5× vs silica) and far lower nonlinearity — tail latency matters for HFT
  and long-haul AI/compute links. Vendor records below are unverified numbers; treat
  as direction not spec.
- **Record single-fiber capacities** around **430–450 Tb/s** over multi-band /
  standard fiber (Aston University; multi-band C+L+S experiments) — research-class,
  signaling a long runway before fiber capacity truly bottlenecks.
- **Silicon photonics + co-packaged optics (CPO) mainstreaming.** 1.6T-class optics
  for AI clusters; **TSMC, NVIDIA and the Taiwanese supply chain treating
  silicon photonics as the next strategic metal; ** IO devices (micro-ring,
  MZM) all shipping to rack-scale. OCS (circuit) market forecast >$8B/2030.
- **Industry-wide copper→photonics migration** for AI racks reported (Broadcom,
  NVIDIA going furthest differently); photonic interconnect now a board-level
  decision, not a science project.

**Factor that drives adoption:** power per bit + latency + rack density. CPO cuts
the pluggable-transceiver energy and footprint; HCF cuts latency. This is
photonics' "most real" commercial frontier.

---

## 3. Photonics & quantum

Two very different things both called "photonics + quantum."

### 3a. Photonic quantum computing — two architectures, both pre-general-purpose

- **PsiQuantum** (~$7B valuation; NVIDIA-backed Series E, ~2025): the
  **fault-tolerant, million-qubit, waveguide/SPAD** architecture built on a
  **foundry-viable photonic chipset ("Omega")** manufactured at GlobalFoundries —
  a fundamentally manufacturable, error-corrected approach rather than brute
  advantage. Building Washington (Alpha, Chicago-region) and Brisbane (Australia)
  sites targeted at utility-scale, room-temperature operation. 2026 status is
  mixed/quiet (execution churn: named **Victor Peng interim CEO**); expect-site
  completion, not advantage-demos, as the near-term tell. *(Site/valuation details
  widely reported; interim-CEO change surfaced in 2026 sources.)*
- **Xanadu** (now public — earnings/Nasdaq filings exist): **modular, networked
  photonic QPUs** (84-photonic-qubit "Aurora" line), room-temperature, with a
  different sales thesis (gradual scale-up, networked cluster). 80–90% of their
  model was photonic-field unclamping vs coherence-focused competitors. Public-listing
  capital + U.S. expansion (NY state office, 2026) but no utility-scale advantage
  claim yet.
- **Academic/landmark angle:** "Toward scalable fault-tolerant photonic quantum
  computers" (Springer, 2025) is the intellectual backbone for the fault-tolerant
  photonic approach; comparative analyses note commercial systems are increasingly
  **calibration/overhead-limited, not coherence-limited** — which is exactly the
  problem photonics (large-scale device fabrication + active error correction)
  attacks.

**Reality check (multi-factor):** photonic QC's advantage thesis = manufacturability
(foundry scale), room-temp operation (no dilution fridges), and error correction
instead of qubit fidelity. The counterweight = slower logical clock / serialization,
and needing fault-tolerant schemes to be *good enough*. No independent photonic QC
has demonstrated a useful, reproducible advantage on real problems as of 2026-09.

### 3b. Quantum networks / QKD — early landmark deployments

Readiness is higher than QC; QKD is a backhaul crypto transport, not compute.

- **EuroQCI rolling out:** IBERIANQCI (Spain+Portugal) live; **TransEuroOGS**
  quantum-secure ground stations (Luxembourg among member states); **Eagle-1**
  QKD satellite (Europe's first) — **delayed again**, with backers defending its
  value *against* post-quantum crypto (the honest debate: do we need QKD at all, or
  is PQC enough?).
- **National/metro QKD:**
  - Ireland's first QKD network (Walton Institute @ SETU + Q*Bird) with
    expandable architecture.
  - **Qunnect demonstrated a metro-scale quantum-entanglement network** (with
    Cisco) — entanglement distribution over deployed fiber, a prerequisite for
    quantum repeaters/memory/networked QC.
  - **Three-node polarization entanglement swapping over NYC dark fiber** (APS
    Global Physics Summit 2026) — a real distributed-entanglement landmark.
  - China: integrated space-ground quantum backbone progress (post-Micius:
    quantum backbone to metro/hundred-node scale; vendor/state write-ups).
- **Satellite QKD (research):** end-to-end LEO satellite QKD, inter-satellite
  twin-field QKD simulation frameworks (IEEE/arXiv 2026) — still R&D/sim.

**Factor that drives the conclusion:** for most real use, **post-quantum
cryptography is the default answer**; **QKD is pursued where information-theoretic
security / immunity to PQC side-channels is demanded** (financial, defense,
government hubs, dual-use hybrid authenticated key exchange — noted in EPJ
Quantum Tech literature). So quantum networking is a *security vertical*, not yet a
compute infrastructure.

---

## Multi-factor read (what actually drives each conclusion)

| Domain | Winning factor | Caution / counterweight | Maturity |
|---|---|---|---|
| AI interconnect (CPO, NVLink photonics) | bandwidth density, pJ/bit energy, form factor | vendor pJ/bit numbers unverified; supply chain ramp | Deploying/ramping |
| Analog photonic compute | potential energy at scale | ADC/DAC conversion tax, precision, no toolchain | Lab→early commercial |
| Comms (silicon photonics, HCF) | power/bit, latency, density | vendor record numbers unverified | Shipping today |
| Photonic quantum computing | manufacturability, room-temp, error-corrected | no demonstrated advantage yet; slower clocks | Buildout to utility-scale (PsiQuantum) / modular (Xanadu) |
| Quantum networking/QKD | info-theoretic security | PQC may suffice for most; few deployments | Early landmark rollout |

---

## Sources (primary-first)

**AI/photonics:**
- Lightmatter — "Scale-Up is a Problem Made for Photonics": https://lightmatter.co/blog/scale-up-is-a-problem-made-for-photonics/
- Lightmatter — 1.6 Tbps/fiber press release: https://lightmatter.co/press-release/lightmatter-achieves-record-1-6-tbps-per-fiber-to-accelerate-ai-optical-interconnect/
- Q.ANT — Generative AI on photonic hardware: https://qant.com/press-releases/q-ant-runs-generative-ai-on-photonic-hardware/
- Lightelligence PACE 3 / WAIC 2026: https://pandaily.com/lightelligence-optical-computing-commercialization-jul2026
- NVIDIA CPO for gigawatt AI factories (webinar): https://quartr.com/events/nvidia-corporation-nvda-co-packaged-silicon-photonics-switches-for-gigawatt-ai-factories-we_33dRo6D6
- NVIDIA photonics spend to fix copper bottleneck: https://thenextweb.com/news/nvidia-photonics-investment-copper-bottleneck-ai-data-centre

**Comms:**
- YOFC 1.2 Tb/s/wavelength over hollow-core fiber: https://www.trendforce.com/news/2026/06/26/news-yofc-achieved-worlds-first-1-2tbs-per-wavelength-transmission-over-hollow-core-fiber/
- FiberHome HCF results @ IEEE SUM 2026: https://www.fiberhome.com/xwzx/20260716/48174.html
- Hybrid SMF/HCF networks (arXiv 2606.30260): https://arxiv-org.ezproxy.obspm.fr/html/2606.30260v1
- Aston University fiber record: https://digiworld.news/news/71341/
- OCS market >$8B by 2030: http://www.ic-ceca.org.cn/shichang/1776041.html

**Quantum:**
- Toward scalable fault-tolerant photonic quantum computers (Springer): https://link.springer.com/article/10.1007/s11227-025-08132-7
- PsiQuantum interim CEO: https://convergedigest.com/psiquantum-appoints-victor-peng-as-interim-ceo/
- Xanadu public filings / U.S. expansion: https://investors.xanadu.ai/news-releases/news-release-details/xanadu-accelerates-us-growth-new-york-state-office
- EuroQCI IBERIANQCI: https://www.qtict.com/english/frontier/detail?id=608
- Eagle-1 delay / PQC-vs-QKD debate: https://altagrove.com/grovewire-news/europes-eagle-1-quantum-key-distribution-satellite-delayed-again-backers-defend-its-value-vs-post-quantum-cryptography
- Ireland first QKD network (Q*Bird): https://www.q-bird.com/press/irelands-quantum-leap-walton-institute-at-setu-and-qbird-deploy-irelands-first-qkd-network-with-expandable-architecture/
- Qunnect metro-scale entanglement network: https://quantumzeitgeist.com/qunnect-cisco-quantum-entanglement-network/
- 3-node entanglement swapping over NYC dark fiber (APS 2026): https://meetings-archive.aps.org/smt/2026/mar-u09/6/
- China quantum backbone progress: https://www.ccidcom.com/wuxian/20260901/v6uTx5CyEMozeEiMY1cwre9tx5mss.html

---
_Status: survey snapshot at 2026-09-10. Photonic QC site/valuation and record-transmission
details are the least-verified and should be re-checked before citing hard numbers._