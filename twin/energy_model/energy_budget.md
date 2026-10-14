# Energy & heat budget — CW χ(2) GeLU node (device → node → system)

Source: **sim + assumed**. No measured values. Regenerate with `python twin/energy_model/energy_budget.py`.

## Design point (sim)

- λ = 1.55 µm, Q_i = 2.0e+06 → κ₀/2π = 96.7 MHz; κ_ext/2π = 48.4 MHz (κ_ext/κ₀ = 0.50)
- Loaded linewidth = 145.1 MHz → Q_loaded ≈ 1.33e+06
- Per-node symbol rate from the loaded linewidth: optimistic 145 MS/s, nominal 102 MS/s, conservative 73 MS/s
- Depletion knee on-chip: 4.88 µW (CME) / 7.87 µW (wrap-12° design)
- Dual-path split η_A = 0.62 → node optical input at the knee = 7.87 µW

## Budget per activation

| Scenario | Rate (MS/s) | Device optical (fJ) | Node optical (fJ) | Laser electrical (fJ) | Bias (fJ) | Control (fJ) | **System (pJ)** | Heat W per 10¹² act/s |
|---|---|---|---|---|---|---|---|---|
| nominal | 101.5 | 48.1 | 77.5 | 1291.9 | 9.8 | 1969.6 | **3.349** | 3.349 |
| optimistic | 145.1 | 33.6 | 54.3 | 287.1 | 6.9 | 137.9 | **0.486** | 0.486 |
| overlap_g_C015 | 101.5 | 17.2 | 27.8 | 463.3 | 9.8 | 1969.6 | **2.471** | 2.471 |
| wrap12_design | 101.5 | 77.5 | 125.0 | 2082.7 | 9.8 | 1969.6 | **4.187** | 4.187 |
| TO_bias_case | 101.5 | 48.1 | 77.5 | 1291.9 | 49240.4 | 1969.6 | **52.579** | 52.579 |

Digital baselines (assumed, system-level incl. operand movement):

- GPU FP16 GeLU: 10.0–20.0 pJ/act
- 7 nm ASIC SRAM-LUT GeLU: 0.5–2.0 pJ/act
- GPU: ~0.7 pJ/FLOP system-level (≈700 W / 1e15 FLOP/s) × 8–16 FLOP for a tanh-approx GeLU + register/SRAM traffic. ASIC: SRAM-LUT GeLU in a 7 nm inference accelerator. Both are assumptions, not measurements.

## Scenario assumptions

- **nominal**: facet 0.5, routing 0.8, laser WPE 0.15, bias 0.001 mW, control 0.2 mW — EO DC bias; 2 tap PDs + low-BW TIAs + shared ADC/PID; 3 dB facet, 1 dB routing; DFB WPE 15 %
- **optimistic**: facet 0.7, routing 0.9, laser WPE 0.3, bias 0.001 mW, control 0.02 mW — EO bias; duty-cycled, multiplexed lock loop (drift is kHz); good edge coupler; WPE 30 %
- **overlap_g_C015**: facet 0.5, routing 0.8, laser WPE 0.15, bias 0.001 mW, control 0.2 mW — as nominal but knee from the overlap-computed g (C-015), 1.0 µm rib, 600/300 stack
- **wrap12_design**: facet 0.5, routing 0.8, laser WPE 0.15, bias 0.001 mW, control 0.2 mW — as nominal but knee from the wrap-12° FDFD design point
- **TO_bias_case**: facet 0.5, routing 0.8, laser WPE 0.15, bias 5.0 mW, control 0.2 mW — reference-arm bias held with a thermo-optic heater (P_π ≈ 20 mW on TFLN) — shows why EO bias is mandatory
