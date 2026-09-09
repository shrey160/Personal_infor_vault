# PsiQuantum — Status Deep-Dive, 2026

_Engineer-depth follow-up to `state-of-photonics-2026.md`. Compiled 2026-09 from
public sources (2025–2026). Scope: verify the site/tech-status gap flagged in the
original survey. Flags unverified claims. Latest-wins where the original survey
and this diverge._

---

## Bottom line (multi-factor)

The "mixed/quiet execution-churn" read from the Sept 2026 survey has **shifted
toward "progressing but pre-utility."** The company moved from design to
**groundbreaking on two sites** in 2026, finalized government funding, and is
executing a foundry-scale manufacturing + domestic supply-chain thesis. But
**no fault-tolerant system is operational anywhere, and no timeline for a
running machine is public** — the near-term tell remains *site/buildout
completion*, exactly as the 2026-09 survey predicted.

---

## What changed since the 2026-09 survey

| Dimension | Survey (2026-09) said | Current (2026) ground truth |
|---|---|---|
| Chicago site | "Alpha, Chicago-region" targeted | **Broke ground** on Chicago quantum site (2026); steel rising in South Chicago (IQMP) |
| Australia site | "Brisbane (Australia) targeted" | **Broke ground** at Moreton Bay, Queensland (2026-06-17) |
| Leadership | "Victor Peng interim CEO (churn)" | **Victor Peng now CEO** (former president of AMD); founder Jeremy O'Brien now Executive Chairman; Lip-Bu Tan (Intel CEO) joined board 2026-04 |
| Funding | ~$7B valuation, NVIDIA-backed Series E | $7B valuation confirmed (BlackRock + Temasek-led $1B Series E, Sep 2025); +A$940M (~US$620M) Australia; +US$100M CHIPS Act (definitive, finalized) +$125M DARPA |
| Manufacturing | Omega at GlobalFoundries | Omega/GlobalFoundries partnership continuing; +$100M to scale domestic supply chain (barium titanate, high-temp single-photon detectors, advanced packaging) |
| Test infra | n/a | Test & Validation Lab opened at Griffith University (May 2026); Linde Engineering cryoplant ordered, delivery ~H2 2027 |

---

## 1. Washington / Chicago site

- **Chicago groundbreaking confirmed** (IQMP; PsiQuantum news). South Chicago steel
  rising as of the report date. This is the "utility-scale" US build. *(Exact
  operational date not public.)*
- The 2026-09 survey's "Alpha, Chicago-region" phrase is now concrete: ground
  broken, steel up.

## 2. Australia / Moreton Bay site

- **Ground broken 2026-06-17** at Moreton Bay, Queensland — billed as the world's
  first utility-scale, fault-tolerant quantum computer.
- Site will hold **tens of thousands of photonic quantum chips**, networked over
  standard optical fibre, cooled by one of the largest cryogenic systems ever
  built for quantum (Linde Engineering cryoplant, ordered late 2024, delivery
  **H2 2027**).
- Funded by **A$940M (~US$620M)** in Australian Commonwealth + Queensland equity,
  grants, and loans — drew criticism for bypassing public tender (NDAs required).
- **Test & Validation Lab opened at Griffith University, May 2026** to refine chips
  and subsystems before they go into the Moreton Bay machine.

## 3. Manufacturing / Omega chipset / supply chain

- **GlobalFoundries partnership (from 2019) continuing** as the central scaling
  lever for the photonic chipset. *(CEO Tim Breen quoted reaffirming the
  partnership 2026-09.)*
- **CHIPS Act $100M (definitive, executed)** with US Dept. of Commerce — targeted:
  **barium titanate optical switches** (incl. Molecular Beam Epitaxy tool in Santa
  Clara for 300mm BTO wafers), **high-temperature single-photon detectors**, and
  **advanced packaging**. Purpose: build quantum-classical chip-manufacturing bridge
  and US domestic supply chain.
- **DARPA $125M** (expanded, announced **July 2026**) — puts PsiQuantum in the US
  government's evaluation of commercial pathways to utility-scale QC.
- PsiQuantum reports **~$200M spent with hundreds of US suppliers across 38 states
  in 2025** *(unverified vendor figure)*.
- Explictly positioned as a **silicon-photonics platform** play, not only a quantum
  processor — "opportunities across advanced computing infrastructure" (EVP
  Rob Soderbery) — i.e. the same silicon-photonics supply chain as the AI-interconnect
  story.

## 4. Technology & architecture (unchanged thesis, now de-risked by deals)

- **Photons as qubits** on **conventional foundry silicon** (vs. IBM/Google
  superconducting). Fault-tolerant / error-corrected approach rather than raw
  qubit fidelity.
- Still **no public demonstration of a useful, reproducible advantage** on real
  problems. No fault-tolerant QC has been shown at commercial scale by anyone.
- Caveat stands: slower logical clock / serialization vs. superconducting rivals
  is the open counterweight.

---

## Multi-factor read (drives the conclusion)

| Factor | Assessment |
|---|---|
| Manufacturing viability | Strong — foundry route + GlobalFoundries + now US-supply-chain money (most differentiated asset) |
| Funding depth | Very strong — ~$7B val, A$940M, $100M CHIPS, $125M DARPA, $1B Series E |
| Site progress | Moved from blueprint to groundbreaking (2 sites) in 2026 — the year's headline signal |
| Technical demo | Weak — still no utility-scale / advantage demonstration |
| Timeline clarity | Weak — no public operational date; criyoplant not delivered until H2 2027 |

**Conclusion:** the bet is real and well-financed, and 2026 produced real physical
progress (groundbreaking). But "utility-scale machine running" = years out; judge
by buildout milestones (cryoplant arrival, cabinet commissioning), not advantage demos.

---

## Sources

- TNW — PsiQuantum breaks ground in Australia (2026-06-17):
  https://thenextweb.com/news/psiquantum-aupsiquantum-australia-groundbreaking-quantum-computerstralia-groundbreaking-quantum-computer
- AFR — Billion-dollar supercomputer on the move after two years of stagnation (2026-05-20):
  https://www.afr.com/technology/billion-dollar-supercomputer-on-the-move-after-two-years-of-stagnation-20260520-p5zz01
- Quantum Computing Report — $100M CHIPS Act executed:
  https://quantumcomputingreport.com/psiquantum-executes-definitive-100-million-chips-act-award-with-u-s-department-of-commerce/
- Quantum Zeitgeist — PsiQuantum lands $100M US (2026-09-08):
  https://quantumzeitgeist.com/psiquantum-lands-100-million-quantum/
- IQMP — PsiQuantum breaks ground on Chicago quantum site; Steel rising in South Chicago:
  https://iqmp.org/news/psiquantum-breaks-ground-on-chicago-quantum-site/
  https://iqmp.org/news/steel-is-rising-again-in-south-chicago/
- PsiQuantum — Victor Peng interim CEO announcement; Niklas Zennström to board:
  https://www.psiquantum.com/news-import/psiquantum-appoints-victor-peng
  https://www.psiquantum.com/news-import/psiquantum-appoints-niklas-zennstrom-to-board-of-directors

---
_Status: snapshot at 2026-09. Chicago/Maryland planet-assembly and site-op dates, and
the ~$200M supplier spend figure, are the least-verified — re-check before citing hard
numbers._