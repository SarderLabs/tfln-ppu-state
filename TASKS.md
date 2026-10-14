# TASKS.md

Format for Riad-owned items (ASK): what / why / exact steps / cost / bring back / blocks.
Status: open | doing | done | blocked

## OWNER = Riad

### ASK-001  Private repo + file tree — DONE
Bring back: confirmed. Keep repo PRIVATE until provisional_01 is filed.

### ASK-002  Workstation environment — open
Why: 3D FDTD, torch training and gdsfactory sweeps run there, not in the chat sandbox.
Steps:
  1. conda create -n ppu python=3.11 && conda activate ppu
  2. conda install -c conda-forge pymeep gdsfactory femwell
  3. pip install torch torchvision ceviche
  4. git clone <repo>; cd tfln-ppu
  5. Smoke test: python sim/meep/pymeep_pulley_validation.py --quick   (2 grid points)
Cost/time: free, ~1–2 h incl. downloads
Bring back: results/meep_smoke.json + `conda list > results/env_<date>.txt`
Blocks: Phase 1 coupling closure

### ASK-003  Claude Code on the workstation — open
Why: lets Claude run long jobs and commit results directly.
Steps: install Claude Code (desktop), open the repo, first command:
  "Read STATE.md. Run the full sweep in sim/meep/pymeep_pulley_validation.py and write results/meep_sweep_<date>.json"
Bring back: nothing — results land in results/
Blocks: Phase 1 speed

### ASK-004  Dhaka IP / corporate lawyer consult — open
Why: decides who the patent applicant can be and whether you may hold foreign company shares.
Steps: book one paid consult with a firm that handles foreign patent filings; ask every question in biz/lawyer_questions.md; get answers in writing (email is fine).
Cost/time: ~USD 100–300, one week
Bring back: biz/lawyer_answers.md
Blocks: incorporation, provisional filing

### ASK-005  USPTO.gov account + identity verification — open
Why: you file provisional_01 yourself; verification takes days.
Steps: create account at uspto.gov → complete ID verification → read micro-entity certification (37 CFR 1.29) and confirm you qualify (gross income < 3× US median household income; < 5 prior applications)
Cost/time: free, 2–5 days
Bring back: "verified" + micro/small entity decision
Blocks: Phase 0 filing

### ASK-006  Choose entity jurisdiction — open (after ASK-004)
Options: Delaware C-corp (Clerky / Stripe Atlas, ~USD 500–1,000) or Singapore Pte Ltd (needs a resident director, ~USD 1,000–2,000/yr).
Bring back: decision → DECISIONS.md
Blocks: IP assignment, filing

### ASK-007  Drop the Overleaf provisional package into ip/provisional_01/ — DONE
Steps: copy ppu_provisional_overleaf.zip contents (main.tex, figures, README) into ip/provisional_01/src/ and the compiled PDF into ip/provisional_01/
Done 2026-10-14: src/main.tex + patent_fig1-4.png + README.txt + compiled ppu_provisional_01.pdf (31 pp) committed. Not in the package (open): numeral_legend.json; ppu_uspto_specification.md.
Blocks: attorney review, filing

### ASK-010  Line endings — DONE
Why: Python/LaTeX/JSON produced on Claude's side are LF; without a repo rule every commit churned whitespace (22 CRLF warnings).
Steps: .gitattributes at root (`* text=auto eol=lf`, plus `*.gds *.png *.pdf binary`); git add --renormalize .; commit "chore: normalize line endings".
Bring back: nothing — index verified all-LF via `git ls-files --eol`.

### ASK-011  Add the two GDS files — DONE
Why: the GDS is the artifact a foundry or partner opens; the .py is just how we make it.
Steps: python -m venv .venv; pip install "gdsfactory>=9,<10" (got 9.51.0); from layout\: python cw_gelu_activation_node.py; python ppu_matrix_4x4.py; commit "layout: v1 GDS".
Result 2026-10-14: layout/cw_gelu_activation_node.gds (168 KB), layout/ppu_matrix_4x4.gds (473 KB). DRC CLEAN both; LVS-lite PASS (METAL 40/40, POLING 8/8 islands). .venv/ gitignored.

ASK-012  Confirm LaTeX source is committed
  Steps: git ls-files ip/provisional_01   → should list src/main.tex, src/patent_fig1..4.png, the PDF, README.md
         If main.tex is missing, copy it from the Overleaf zip and commit "ip: provisional_01 LaTeX source"
  Bring back: the ls-files output

ASK-013  Check provisional_01 against the fitted operating point — DONE
  Why: finding 2 above — a fixed η_B/φ_B/δ=0 recitation would describe a non-working point
  Steps: grep ip/provisional_01/src/main.tex for "0.38", "0.62", "pi - 0.35", "0.35", "detun"; paste me every hit with its claim/paragraph number
  Bring back: the hits
  Result 2026-10-14: ip/provisional_01/ASK-013_hits.md — 22 hits on 13 lines, all in the specification (¶[0009], [0010], [0018], [0019], [0028], [0032], [0039], FIG. 2 caption, numeral legend). None in the claims; claim 10 is the only claim reciting the old ranges (55:45–70:30, π − 0.2…0.5 rad) and the fitted point (η_B ≈ 0.67, φ_B ≈ 4.33 rad, δ ≠ 0) lies outside both. No "detun" anywhere.
  Blocks: filing of provisional_01 (fix is a small edit; Claude drafts it)

ASK-014  Workstation run of the torch half of C-006 — DONE
  Steps: cd tfln-ppu; pip install torch pytest; pytest twin/tests -q;
         python twin/benchmarks/tiny_transformer.py --steps 3000   (≈10–20 min CPU, faster on GPU)
  Bring back: twin/benchmarks/transformer_results.json + pytest output (commit both)
  Result 2026-10-15: pytest 2/2 passed on workstation (.venv-ppu, torch 2.14.1 CPU), Kaggle T4 and Colab T4. Transformer run completed on Colab T4 (results/transformer_run_20261015_colab.txt): val_loss GeLU 1.7631 vs OpticalGeLU 1.7630 (2.544 vs 2.544 bits/char), i.e. parity. Workstation CPU run infeasible: OpticalGeLU measured 74 s/step (62 h) because autograd unrolls the 20-iteration complex-float64 Newton solve; the same unrolled graph OOMs a 16 GB T4, so the GPU runs used gradient checkpointing around OpticalGeLU.forward (twin/benchmarks/{kaggle,colab}_c006b.ipynb) — numerically identical, module files untouched. Follow-up: C-019.
  Blocks: nothing; completes the benchmark set for the brief

### ASK-015  Request TFLN design manuals — open
Why: C-012 blocked on a foundry design manual; CCRAFT 2027 runs need it by mid-Dec 2026.
Steps: send E1 (biz/outreach/email_templates.md) to foundry@ccraft.com and via Lightium's contact form; sign their NDA personally if NewCo does not exist yet (info flows foundry → us); ask Europractice and CMC whether they aggregate CCRAFT/Lightium.
Cost/time: free; 1–3 weeks for replies.
Bring back: design manuals → biz/foundry/<name>/ (NOT mirrored), answers to brief §4 Q1–Q11 → biz/foundry/answers.md; especially etch depth and poling-on-MPW.
Blocks: C-012, C-018.

### ASK-016  Confirm term-sheet economics — open
Why: all bracketed values in biz/licensing/option_to_license_term_sheet.md are Claude defaults.
Steps: read §4 and Exhibit A; accept or edit option fee, period, upfront, royalty %, MAR ladder, sublicence share; pick Field A wording.
Cost/time: 30 min.
Bring back: edited file committed.
Blocks: sending E3/E5.

### ASK-017  Retrieve verbatim claim 1 of US 2024/0419951 A1 — open
Why: FTO paragraph and C-002 need the as-published claim text; research found only a summary.
Steps: patents.google.com/patent/US20240419951A1 → copy all independent claims; also Patent Center appl. 17/973,118 → note any office action / allowance.
Cost/time: free, 15 min.
Bring back: ip/prior_art/US20240419951A1_claims.md + status line.
Blocks: C-002.

## OWNER = Claude

| ID | Task | Phase | Status | Needs |
|---|---|---|---|---|
| C-001 | Repo scaffold (STATE/TASKS/DECISIONS/lawyer questions) | 0 | done | — |
| C-002 | Claims-to-feature map + trade-secret register | 0 | open | — |
| C-003 | Inventor→company assignment deed (draft for lawyer) | 0 | open | ASK-006 |
| C-004 | Energy/heat table device→node→system | 1 | done | — |
| C-005 | twin/optical_gelu PyTorch package + energy counter | 1 | done | — |
| C-006a | MLP benchmarks (MNIST, Fashion-MNIST) | 1 | done | — |
| C-006b | Tiny transformer benchmark (torch, workstation) | 1 | done | — |
| C-007 | Fit 3D FDTD results → coupling DOE map → update design point | 1 | blocked | ASK-002/003 |
| C-008 | arXiv manuscript (LaTeX) | 1 | open | after filing |
| C-009 | Provisionals 02/03/04 (spec + 20 claims each) | 2 | open | — |
| C-010 | Option-to-license term sheet + foundry outreach brief + email templates | 2 | draft v0.1 | ASK-016 |
| C-011 | Lock-loop PID: Python sim → Verilog / MCU firmware | 2 | open | — |
| C-012 | Test-chip GDS (DOE, Q rings, Λ-offset poled rings, 2 nodes, 2×2) | 3 | blocked | foundry design manual |
| C-013 | Measurement protocol + spectrum-fitting scripts | 3 | open | — |
| C-014 | Replace TO compensation heaters with DC-EO trim sections in layout (node + 4×4) | 1 | open | — |
| C-015 | Overlap-computed g (not literature-scaled) from femwell mode fields → knee sensitivity | 1 | done | — |
| C-016 | Power-normalized overlap cross-check (Poynting normalization) to close the 10–40% η_norm uncertainty | 1 | open | — |
| C-017 | Per-node calibration routine (recover the 1 pt lost in condition C) | 1 | open | — |
| C-018 | 200 nm-etch contingency: re-run femwell/CME at 600/200 nm, recompute Λ, M, R, pulley κ; decide 300-vs-200 | 1 | blocked | ASK-015 |
| C-019 | OpticalGeLU torch twin: implicit-gradient backward (custom autograd.Function through the converged Newton fixed point) — removes the 20-iteration unrolled graph (74 s/step CPU, OOM on 16 GB T4); keep 1e-6 match test | 1 | open | — |
