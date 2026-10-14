> Public mirror for chat sessions: https://raw.githubusercontent.com/SarderLabs/tfln-ppu-state/main/STATE.md — run `python scripts/sync_state.py` after every commit that changes STATE/TASKS/DECISIONS or twin results.

# STATE.md — single source of truth (Claude reads this first, every session)

Last updated: 2026-10-14 (by Claude, ASK-007/010/011 closed)
Current phase: **0 — Lock IP and entity**
Phase gate to 1: provisional filed with company as applicant; repo + workstation env live

## Where things stand
- Design: CW χ(2) GeLU node (poled TFLN microring, pulley dual-λ coupler) + 4×4 Clements MVM mesh. DRC clean, LVS-lite PASS. v1 GDS committed in layout/ (gdsfactory 9.51.0, repo .venv).
- Verified (2D): n_eff 1.9026 / 2.0913, Λ_QPM = 4.108 µm (M = 77, R = 50.344 µm), GVM 203 fs/mm.
- CME design point: g/2π = 1.415 MHz (literature-scaled), knee ≈ 4.9 µW on-chip, η_SH peak 33 %.
- OPEN RISK: EIM-FDFD shows gross overcoupling at 1550 (κ² ≈ 5–19 %/pass vs 0.07 % target). Absolute κ unverified until 3D FDTD. Coupling DOE columns are mandatory before any tapeout.
- IP: provisional_01 spec + 20 claims written and compile-verified; TeX + figs in ip/provisional_01/src/, compiled PDF at ip/provisional_01/ppu_provisional_01.pdf — NOT YET FILED. Closest art: US 12,332,539; Marandi family US2024/0419951A1 (as-published claims not yet retrieved).
- Entity: none yet. Riad is in Bangladesh (not a PCT state) → applicant must be a company in a PCT state.
- Strategy: license IP first → small chip company → acquisition.

## In flight
| Owner | Item | Status |
|---|---|---|
| Riad | ASK-002 workstation conda env | open |
| Riad | ASK-003 Claude Code on workstation | DONE (used for ASK-012, mirror setup) |
| Riad | ASK-004 Dhaka lawyer consult | open — blocks filing |
| Riad | ASK-005 USPTO.gov account | open — blocks filing |
| Riad | ASK-006 entity jurisdiction | open (after ASK-004) |
| Riad | ASK-013 provisional_01 grep for old operating point | open — blocks filing |
| Riad | ASK-014 torch twin test + transformer benchmark | open |
| Claude | C-002 claims-to-feature map + trade-secret register | open |
| Claude | C-006b transformer benchmark | blocked on ASK-014 |
| Claude | C-009 provisionals 02–04 | open |
| Claude | C-010 option-to-license term sheet + outreach brief | open |
| Claude | C-011 lock-loop PID sim → Verilog | open (promoted to Phase 1) |
| Claude | C-014 TO heaters → DC-EO trims in layout | open |
| Claude | C-017 per-node calibration routine | open |

## Next session should start with
"Phase 1 / Task: ASK-013 claim review / New files: ASK-013_hits.md pasted"

## Last result
C-004 energy/heat budget: device 48 fJ/act (sim), system 3.3 pJ nominal / 0.49 pJ optimistic / 53 pJ if TO-biased. Overhead (laser + control) = 97% of system energy. Consequences: EO bias mandatory; lock-loop power is on the critical path; sub-fJ claim retired.

C-015: overlap-computed g/2π = 2.36 MHz (1.67× literature scaling), knee 1.75 µW, device 17 fJ/act, system 2.5 pJ nominal. Stack choice moves g ±10% but Λ ±25%. Width pinned at 1.0 µm.

C-005/C-006: OpticalGeLU twin fitted (RMSE 0.040 vs x·Φ(x)); MNIST 97.4 % vs 97.5 % ideal, Fashion-MNIST 86.9 vs 87.1; noise+variation costs ~1 pt. Operating point differs from the white-space doc → ASK-013 before filing.

## Next session should start with
"Phase 0 / continue STATE.md / new files: <lawyer answers or conda list if available>"
