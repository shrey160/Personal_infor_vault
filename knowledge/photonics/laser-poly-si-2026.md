# Laser processing of poly-Si — verified research map (2026-09-10)

> **Status: verified.** Four claims-verification passes (one per front) ran 2026-09-10,
> each cross-checked against primary/registry sources (Crossref, DataCite, OpenAlex,
> Semantic Scholar, OSTI, PMC, publisher pages, pv magazine, lab press). Every lead is
> now marked **VERIFIED / PARTIALLY / NOT FOUND / CONTRADICTED** with a DOI and corrected
> numbers. All DOIs are canonical; the few bot-blocked PIIs are flagged.
> This file supersedes the earlier "direction map" version — leads are now resolved.

## Origin of interest
Shrey's dormant interest: laser-driven deposition of bulk polysilicon (Siemens-process
alternative) and laser CVD of poly-Si thin films. **Verdict on that:** the field's center
of gravity is laser *processing* of already-deposited poly-Si across four fronts below.
No meaningful 2024–26 work on bulk-deposition/silane conversion (laser CVD) surfaced —
re-checked 2026-09-10; only 1985-era laser-CVD-of-a-Si-from-silane, unrelated material-
deposition systems, and post-deposition laser *processing* (e.g. a Jan-2026 Zenodo
preprint on laser-ablation-patterned SiO2/poly-Si superlattices for 26% IBC cells,
zenodo.org/records/18862297). **Suggested next probe for this dormant line:** hunt 2025–26
*laser-assisted deposition* patents/startups directly (papers are silent; that's where any
pulse would surface first).

---

## Front A — Laser activation & crystallization of poly-Si passivating contacts

**VERIFIED.** Four leads, all real:

- **A1 · Stuttgart dissertation — VERIFIED (bibliography); content plausible-not-read.**
  Saman Sharbaf Kalaghichi, *"Laser activation of boron-doped polycrystalline silicon
  layers for highly efficient passivated contact solar cells"*, U. Stuttgart, Dissertation
  (2025; defense 2025-12-22, advisor Jürgen H. Werner). DataCite:
  https://doi.org/10.18419/opus-17859 — note the *extended* title. Content (epitaxial
  regrowth; SiO₂-interlayer damage threshold) not directly readable (PDF), but consistent
  with group's open abstracts (`Energies` 17 (2024) 1319, iVOC 715 mV; `Solar` 3 (2023) 21).
  Leads' bitstream URL resolves to a real PDF (title corrected).
- **A2 · P-doped UV-laser activation — VERIFIED, all three numbers exact.** Yijun Wang,
  Di Yan, J. Ibarra Michel, et al. (Melbourne/RMIT/ANU/NREL/Jinko), *"Ultraviolet Laser
  Activation of Phosphorus-Doped Polysilicon Layers…"*, **Adv. Mater. Interfaces 12(1),
  2400542 (Nov 2024)** — year is 2024, not 2025. Pulsed UV on LPCVD P-poly/SiOx/c-Si.
  **Peak iVoc 726 mV; trade-off 712 mV / 89 mΩ·cm² @ 0.78 J/cm².** https://doi.org/10.1002/admi.202400542
- **A3 · "first LECO on p⁺" — PARTIALLY VERIFIED, framing corrected.** Real paper is
  **Jan Hoß, S. Sharbaf Kalaghichi, J. Lossen, F. Buchholz, et al. (ISC Konstanz),
  *"Advanced TOPCon solar cells with patterned p-type poly-Si fingers on the front side
  and vanishing metal induced recombination losses"*, EPJ Photovoltaics 15, 43 (2024)** —
  year 2024, not 2025. https://doi.org/10.1051/epjpv/2024040 . Champion Voc 719 mV /
  23.4%; <2 mV implied-vs-external Voc gap = "vanishing metal-induced recombination."
  **Not a LECO-on-p⁺ proof** — it is *laser-ACTIVATION-based* patterning of p⁺-poly
  (alkaline etch-stop of laser-activated p⁺⁺). LECO-on-passivated-contacts is a separate
  Fraunhofer-CSP + ISC Konstanz + centrotherm project (**SelFi**, BMWK 03EE1138, 2021–2025;
  target 715–720 mV).
- **A4 · process-window review — VERIFIED, every number exact.** "Nano-Micro Letters" is
  a **real SJTU/Springer journal** (not a typo): Hao Liu, …, Liang Fang, Xixiang Xu (LONGi),
  Deyan He, *"Advanced Laser Technologies for Efficient Crystalline Silicon Solar Cells"*,
  Nano-Micro Letters 18, 348 (**2026**), https://doi.org/10.1007/s40820-026-02199-4 ,
  PMC13125567. Table-1 laser-crystallization row verbatim: CW/ns/fs; 532, 808, 1064 nm;
  ~40–150 mJ cm⁻²; 10–30 m s⁻¹; "cuts process time to milliseconds vs conventional
  furnace annealing hours." (Do not confuse with a similarly-titled 2026 Materials & Design
  review, DOI 10.1016/j.matdes.2026.116594.)

**Missed real work (worth citing):** Stuttgart/ISC laser-Activation precursors incl. the
"three-step" activation (Solar 3 (2023); Energies 17 (2024), iVOC 715 mV); Fraunhofer-ISC
SelFi; Georgia Tech Wilkes et al., "Laser Crystallization and Dopant Activation of a-Si:H
CSL in TOPCon", IEEE JPV 10 (2020); NREL/CSM Ga-hyperdoped poly-Si pulsed-laser anneal
(Energy Environ. Mater. 2023, 10.1002/eem2.12542); 2025 SEMSOLMAT LECO-on-p⁺-emitter studies.

## Front B — Poly-fingers & selective poly-Si patterning

**VERIFIED.** Six leads:

- **B1 · Yangzhou 26.08% rear poly-fingers — VERIFIED, all figures.** pv magazine
  (10 Jul 2026) + journal paper **Qinqin Wang et al. (Yangzhou; Jinko co-authors),
  *"Laser-assisted poly-finger rear patterning… n-TOPCon solar cells"*,
  Solar Energy Materials and Solar Cells 306, 114543 (2026)**,
  https://doi.org/10.1016/j.solmat.2026.114543 . ps green-laser + KOH (optimum 480 s),
  rear poly-fingers; **26.08% (ISFH-certified), Voc 746.1 mV, FF 83.6%.**
- **B2 · Ding et al. 2025 — VERIFIED; front, not rear.** Yu Ding, Jianning Ding, et al.
  (Yangzhou/Jiangsu + JA Solar), *"Green picosecond pulsed laser-induced **front** poly-Si
  finger patterning for rear-junction double-sided Poly-Si…"*, SEMSOLMAT 292, 113827 (2025),
  https://doi.org/10.1016/j.solmat.2025.113827 . Lead's sciencedirect PII exists ✓, but
  fingers are **front**; "n⁺ and p⁺ fingers" + pre-anneal map **not verifiable** (paywalled).
- **B3 · Wang et al. 2025 UV-ps — VERIFIED; URL slug malformed; citations ≠13.** Qinqin
  Wang, Jianning Ding, et al., *"26.35%-Efficiency and high-bifaciality n-TOPCon enabled by
  UV-ps laser-induced selective modification of double-layered SiOx/n+-poly-Si passivating
  contacts"*, Energy & Environmental Science 18(20), 9217–9229 (2025),
  https://doi.org/10.1039/d5ee02742j . Correct RSC URL: pubs.rsc.org/ee/article/18/20/9217-9229/891402.
  Citations: **9–10, not 13**.
- **B4 · Cai et al. 2026 localized poly-Si thinning — VERIFIED in substance; PII unconfirmed.**
  Yalun Cai, Ning Song, et al., *"Mitigating parasitic optical losses in bifacial TOPCon…
  localized thinning of polysilicon"*, SEMSOLMAT 298, 114147 (2026),
  https://doi.org/10.1016/j.solmat.2025.114147 (SSRN 10.2139/ssrn.5557546). PII S0927…
  007482 well-formed but unconfirmable (bot-403).
- **B5 · laser-oxidation KOH-etch mask — VERIFIED; attribution CORRECTED.** Original is
  **Georgia Tech** (not ISC Konstanz): Dasgupta, Ok, Upadhyaya, …, Rohatgi, *"Novel Process
  for Screen-Printed Selective Area Front Polysilicon Contacts for TOPCon Cells Using Laser
  Oxidation"*, IEEE J. Photovoltaics 12(6) (2022), https://doi.org/10.1109/JPHOTOV.2022.3196822 .
  Nanosecond **UV 355 nm** (~3 W @ 400 mm/s) grows **1–4 nm stoichiometric SiO₂** mask; KOH
  etches 200 nm poly-Si; full-area J0 36.8 fA/cm² @3 W, ~1.68 fA/cm² @ 4.48% wing coverage →
  **total front J0 ≈ 10 fA/cm²**. (ISC Konstanz conflation source = Hoß et al., B/A3.)
- **B6 · context numbers — PARTIALLY VERIFIED; labels corrected; records moved on.**
  LONGi 27.3%→27.81% is real but **heterojunction/hybrid back-contact, not pure poly-Si TBC**:
  27.3% HBC (Wu Hua et al., *Nature* 635:604–609, 2024, 10.1038/s41586-024-08110-8; NOT
  3rd-party certified); 27.81% HIBC ISFH (Nature, 12 Nov 2025, 10.1038/s41586-025-09681-w).
  **No certified pure-poly-Si TBC >27% found.** Since Apr 2026 the certified ceiling is
  **Trina THBC 28.00% (ISFH)** and **LONGi HIBC 28.13% (ISFH)**. "Laser patterning =
  dominant industrial BC method" is SUPPORTED (LONGi states it's the cheapest BC technique).

## Front C — LECO (laser-enhanced contact optimization)

**VERIFIED** (with one correction):

- **Origin CORRECTED — not ISC Konstanz/Fraunhofer.** LECO is a post-metallization step:
  screen-printed firing → IR-laser scan under reverse bias → current crowds at contacts
  ("current firing"), locally re-forming the Ag–Si interface (Ag crystallites through
  interfacial glass) → lower ρc (FF↑) and often metallization-recombination (Voc↑).
  Developed/patented/scaled by **CE Cell Engineering GmbH, Kabelsketal (founded 2017)**;
  first exposure MIW 2020; first paper Krassowski et al., AIP Conf. Proc. 2367, 020005 (2021),
  https://doi.org/10.1063/5.0056380. Fraunhofer/ISC Konstanz are **partners** (MIW venue).
  Adoption: PERC ~2021–22; TOPCon-standard ~2023–24 (+0.3% abs; >26% cells).
- **C1 · Wang et al. 2025 — VERIFIED; verbatim "migrating to mainstream."** Qinqin Wang,
  Jianning Ding, et al. (Yangzhou/Changzhou/NUAA/Yingkou Jinchen), *"Impact of LECO on
  n-TOPCon solar cells…"*, SOLMAT 285 (2025), https://doi.org/10.1016/j.solmat.2025.113526 .
  Abstract verbatim: LECO "instead of conventional high-temperature sintering… **is being
  migrated to mainstream technology**". Maps sinter-T/laser-P/reverse-V; COMSOL; optimum
  790 °C/18 W/16 V → 25.97%, Voc 731.5 mV, FF 84.42%. Citations ≈20–23, not 26.
- **C2 · Krassowski — PARTIALLY VERIFIED; affiliation CORRECTED.** Eve Krassowski is real,
  active 2024–25, but is at **CE Cell Engineering, not Fraunhofer**. 2025 IEEE 53rd PVSC
  *"LECO – on the evolution and potentials…"*, https://doi.org/10.1109/pvsc59419.2025.11132512 ;
  MIW2024 reliability talk "LECO induced Current Fired Contacts stable over lifetime?".
  Indirect Fraunhofer link = co-author on Lange et al., SOLMAT 2025, 10.1016/j.solmat.2025.113795.
- **C3a · Gao et al. 2026 coupled sim — VERIFIED.** Qianhong Gao, Zhenhai Yang, et al.,
  *"Mechanism insights… photo-electric-thermal coupled simulations"*, SOLMAT,
  https://doi.org/10.1016/j.solmat.2026.114177 . PII unverifiable (bot-403).
- **C3b · Chen et al. 2025 CEJ — VERIFIED; citations ≈14 not 16.** Sheshicheng Chen, …
  Jichun Ye (Ningbo CAS/Soochow), *"Mechanisms and strategies for LECO… ultra-high sheet
  resistance emitters"*, Chemical Engineering Journal, https://doi.org/10.1016/j.cej.2025.162979 .
  Companion: 10.1016/j.cej.2025.167397.
- **C4 · X. Wang thermal-stability — VERIFIED (bankability paper).** Xutao Wang et al.
  (UNSW SPREE + Jolywood), *"Thermal stability of laser-assisted fired TOPCon… module
  manufacturing, certification testing, and field conditions"*, SOLMAT,
  https://doi.org/10.1016/j.solmat.2025.114124 (CC-BY). **Hydrogen-related defects**,
  three-state defect model; lamination ~0.29% abs PCE loss; light-soaking self-healing;
  450 °C RTA cycling. pv magazine AU: +0.6% (5 May 2025).
- **C5a · Zuo et al. 2026 secondary LECO — VERIFIED.** Tongcheng Zuo, et al. (Zhejiang),
  *"…Extra Secondary LECO"*, Progress in Photovoltaics, https://doi.org/10.1002/pip.70096
  (2026-03-22). Champion **26.69%**; secondary LECO ≈ annealing step for Ag–Si contacts.
- **C5b · CONTRADICTED as merged (two distinct works).** (i) 27.27% back-contact,
  Jianchao Yang et al. (incl. Jianning Ding), ***Solar Energy* 314, 114727 (2026)**,
  https://doi.org/10.1016/j.solener.2026.114727 — laser shock-wave management.
  (ii) **27.62% certified** hybrid-BC is a **different Nature paper**: Hui Yan (BJUT) +
  Gold Stone Energy, *"Maximizing carrier extraction in hybrid back-contact solar cells"*,
  Nature 652:650–654 (2026). Note S0038092X = valid *Solar Energy* ISSN prefix (not SEMSC).

## Front D — Laser-crystallized poly-Si for photonics

**VERIFIED.** Four leads:

- **D1 · Li et al. 2025 — VERIFIED, exact.** Junying Li et al. (Zhejiang/Westlake/IMECAS),
  *"Local laser annealing for amorphous/polycrystalline silicon hybrid photonics on CMOS"*,
  Optics & Laser Technology 181, 111799 (Feb 2025; online 2024),
  https://doi.org/10.1016/j.optlastec.2024.111799 . Mask-assisted anneal of **only active
  regions** → poly-Si; excimer vs solid-state; **buffer layer under mask prevents metal
  contamination**; mitigates **~140 dB/cm** penalty in a-Si racetrack; **~8 dB/pair**
  grating-coupler loss. ("355 nm" only in lead, not abstract — plausible, unverified.)
- **D2 · Lei et al. 2025 — VERIFIED substance; URL wrong; "first" unsupported.** Kunhao Lei
  et al. (Zhejiang), *"Polysilicon Modulator Utilizing CMOS-Compatible Local Excimer Laser
  Annealing Technology"*, J. Lightwave Technol. 43(2), 684–689 (15 Jan 2025),
  https://doi.org/10.1109/JLT.2024.3451963 (IEEE 10659095). Racetrack modulator ~17 dB
  extinction, rise/fall 0.82/6.87 µs, Pπ 27.2 mW. **Lead's opg.optica.org URL is WRONG**
  (JLT is IEEE; URL bot-walled). "First" contestable — excimer-annealed Si GHz modulators
  predate (Lee, Thompson & Lipson, Opt. Express 21, 26688 (2013)); novel = *localized*
  anneal of the doped region.
- **D3 · Franz (Southampton) baseline — VERIFIED; numbers merged.** Franz et al., *"Laser
  crystallized low-loss polycrystalline silicon waveguides"*, Opt. Express 27(4), 4462–4470
  (2019), https://doi.org/10.1364/OE.27.004462 (CC-BY): **CW Ar⁺ 488 nm**, ≤~320 mW, 4.7 µm
  spot on pre-patterned a-Si; a-Si **HWCVD @ 320 °C**, budget <400 °C; quasi-single-crystal
  to mm, grains ~1.8 mm; **5.31 dB/cm @ 1550 nm (1.5 µm guide)**. The lead's **5.13 dB/cm**
  is from the same group's CLEO:2017 paper (10.1364/CLEO_SI.2017.SM3K.4). Both real, same
  group — no fabrication.
- **D4 · Ghosh VERIFIED; Xu venue CONTRADICTED.** Ghosh et al. (Southampton), *"Low-temper-
  ature polycrystalline silicon waveguides for low loss transmission in the near-to-mid-IRC
  region"*, Opt. Express 31(2), 1532 (2023), https://doi.org/10.1364/OE.473474 — **532 nm**
  laser heat treatment, <3 dB/cm @1.55 µm & 2–2.25 µm. Xu 2×2 MZI switch exists but is in
  **Optics Express, not Optics Letters**: X. Xu et al. (Zhejiang), Opt. Express 31(18),
  29695 (2023), https://doi.org/10.1364/OE.495983 — poly-Si @ 620 °C on IMECAS 180 nm pilot
  line, RTP 1050 °C, p-i-n switch, IL 5.9±0.4 dB, crosstalk <−20 dB; **NOT laser-annealed**
  (authors cite laser annealing as future route).

**Corrected state of the field:** not a ~2-yr-old field — enabling results are 2013–2019;
new since 2023–25 is the mask-based *local/selective* excimer-anneal flow + foundry framing.
Threads: (1) Southampton ORC (Peacock/Chong) CW 488 nm → ~5 dB/cm, <400 °C, extended to
<3 dB/cm near/mid-IR (Ghosh 2023); (2) excimer-annealed Si modulator lineage (Lee 2013);
(3) poly-Si-in-CMOS without laser anneal (MIT, IME A*STAR, CEA-Leti, NTT — many); (4) the
2025 China push (Zhejiang H. Lin group / Westlake / IMECAS / Hangzhou IAS): Xu 2023 switch,
Lei 2025 modulator, Li 2025 hybrid mask-anneal. **No commercial laser-crystal poly-Si
photonics product**; still academic (foundry interest from IMECAS pilot line).

---

## Cross-cutting verification outcomes
- **Confirmed repairable:** citations inflated in several leads (26→~20–23; 13→9–10; 16→14);
  years slipped (2024→claimed 2025); "rear vs front" finger labels wrong in B2; wrong
  attributions (Georgia Tech vs ISC Konstanz in B5; CE Cell Engineering vs Fraunhofer for
  LECO origin + Krassowski's affiliation).
- **Wrong/mismapped URLs (4):** opg.optica.org jlt URL (D2, IEEE); RSC article-abstract
  slug (B3); sciencedirect PIIs generally bot-blocked (must cite DOIs); no PII→DOI mapping
  exists publicly.
- **Technology labels corrected:** LONGi 27.3/27.81 are HBC/HIBC, not pure poly-Si TBC;
  no certified pure-TBC >27%; current ceiling Trina THBC 28.0% / LONGi HIBC 28.13% (ISFH).

## Open questions for the next round
- Read the Stuttgart dissertation PDF directly (epitaxial regrowth / SiO₂ damage-threshold
  claims) — highest-value confirmation left in Front A.
- Chase LIong 28.13% HIBC and Trina THBC 28.0% papers for the exact mechanisms (laser vs
  mask-based patterning at the record level).
- Deepen the photonics angle: the Zhejiang/Westlake/IMECAS cluster's near-term foundry
  plans (is this heading to a wafer-line demo?).
- Package a deep-research seed prompt for exhaustive expansion once the user picks a
  sub-front to go deeper on.

## Sources
Full URLs/DOIs embedded per lead above. Verification ran from Crossref, DataCite, OpenAlex,
Semantic Scholar, OSTI, PMC, publisher pages, pv magazine, KETEP, BJUT/English, CSP/ISC pages.
Bot-403s noted where only a DOI is safe to cite.