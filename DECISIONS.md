# DECISIONS.md — dated, with reasons. Append only.

- 2026-08  Λ_QPM = 4.108 µm, M = 77, R = 50.344 µm. Reason: mesh-converged femwell n_eff; M = 77 is the smallest integer order keeping R ≥ 50 µm (DRC min bend).
- 2026-08  Pulley wrap default 12° (was 30°). Reason: EIM-FDFD showed oscillatory κ vs wrap and gross overcoupling at 30°; 12° reduces κ² ~3.5× but still overcoupled → DOE required.
- 2026-08  Dual-path split η_A/η_B = 0.62/0.38, reference-arm bias φ_B = π − 0.35 rad. Reason: synthesizes x·Φ(x) non-monotonic shape from monotonic χ(2) depletion + asymmetric interference.
- 2026-08  Clements 4×4 mesh, cols 2-1-2-1, 130 µm rails, 750 µm node pitch. Reason: fits 5.41 × 2.86 mm, heater pads need ≥100 µm standoff.
- 2026-08  Claim-1 limitations kept narrow (doubly-resonant poled ring, CW sole input, amplitude encoding, pulley dual-λ coupler, integer M). Reason: structural proximity to US 12,332,539 and Marandi family.
- 2026-10-13  Commercial strategy: license IP first → small chip company → acquisition. Reason: solo founder, no lab, no US/EU ties, Bangladesh capital constraints.
- 2026-10-14  Patent applicant will be a company incorporated in a PCT state, not Riad personally. Reason: Bangladesh is not a PCT contracting state; a company applicant restores PCT access. Jurisdiction (Delaware vs Singapore) pending ASK-004/006.
- 2026-10-14  Repo = source of truth; project knowledge = curated snapshot; STATE.md read first every session.

- 2026-10-14  Reference-arm bias and node trims are DC electro-optic, never thermo-optic. Reason: energy model shows a 5 mW TO bias costs ~49 pJ/activation vs ~0.01 pJ for EO; TO would erase the energy advantage.
- 2026-10-14  Claim language: "tens of fJ per activation at the device (sim), 0.5–3 pJ at the system" replaces "sub-fJ/activation". Reason: knee 4.9 µW / ~100 MS/s linewidth-limited rate = 35–80 fJ; sub-fJ requires g/2π ≳ 10 MHz, unverified.

- 2026-10-14  Ring waveguide top width is 1.0 µm (600/300 stack), matching GDS and Λ = 4.108 µm. Reason: C-015 shows Λ shifts to 4.425 µm at 1.4 µm width; the 1.4–1.5 µm figure in older notes was stale.
- 2026-10-14  g/2π = 2.36 MHz (overlap-computed, sim) replaces the literature-scaled 1.415 MHz; quote conservative range 1.8–2.4 MHz. Knee 1.75 µW (range 1.75–3 µW). Device energy ≈ 17–30 fJ/activation (sim).

- 2026-10-14  GeLU operating point (sim, C-005): η_B ≈ 0.67, φ_B ≈ 4.33 rad, δ_a ≈ 0.28 κ_a, δ_b ≈ 1.0 κ_b, P_in(x=0) ≈ 1.5 µW. Reason: fit to x·Φ(x) gives RMSE 0.040; the earlier η_B = 0.38 / φ_B = π − 0.35 / δ = 0 point gives RMSE 1.27 (no dip). EO trims need ≥2π on φ_B and ≥ ±1 linewidth on δ_a, δ_b.

- 2026-10-14  Public read-only mirror SarderLabs/tfln-ppu-state carries STATE/TASKS/DECISIONS and non-sensitive result JSONs so Claude chat sessions can read live state without uploads. Allow-list enforced in scripts/sync_state.py; ip/, layout/, biz/, control/ never mirrored.

- 2026-10-15  Foundry route: CCRAFT (CSEM spin-off) is primary test-chip candidate, Lightium secondary, QCi only under a competitor NDA; target MPW 2701/2702 (design-in 2027-03-21), not 2604. Reason: only published TFLN MPW schedule with poling as a service; 2604 unreachable before filing/entity.
- 2026-10-15  Foundries get non-exclusive hosting rights only; licensee options are field-limited (Field A = ML activation/nonlinear processing); Fields B/C reserved. Reason: foundry-neutral IP keeps second-sourcing and other fields alive.
- 2026-10-15  Royalty base is the photonic die/module, never the system. Reason: 3 % of die passes; any % of a server is rejected.
- 2026-10-15  Outreach gates G1–G4 (biz/outreach/foundry_outreach_brief.md §1) are binding on all sends. Reason: EPC Art. 54 absolute novelty; applicant must be the entity.