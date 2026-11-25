# LDOS vs Anderson: LRDM-paper systems (Table I, arXiv:2211.07894), Aurora inputs

Settings follow Srinibas Nandi's checklist (Slack, 2026-11-24) and are written for DFT-FE `publicGithubDevelop` at `23d6d02f6` (2026-11-15).

Each system has `GGA/` (PBE) and `MGGA_SCAN/`. Each of those holds `anderson.prm`, `adaptive_anderson.prm`, `ldos.prm`,
empty `job_anderson.sub` / `job_adaptive_anderson.sub` / `job_ldos.sub`, and the inputs (coordinates, domain vectors, pseudo.inp, SG15 UPFs).
Within a folder the three prms differ only in the mixing lines; GGA and SCAN differ only in `EXCHANGE CORRELATION TYPE`.

## Common settings (all 102 prms)

| setting | value |
|---|---|
| VERBOSITY | 4 |
| POLYNOMIAL ORDER | 7 (electrostatics order left at default = 7) |
| MULTIPOLE BOUNDARY CONDITIONS | false |
| k-point symmetry | USE TIME REVERSAL SYMMETRY = true, USE GROUP SYMMETRY = false |
| NUCLEAR CHARGE QUADRATURE ORDER / COPY | 10 / 2 (the hard-coded rule before commit f100d7ca5; identical to today's default) |
| LOCAL / NON LOCAL PSP QUADRATURE COPY | 1 |
| Eigen-solver | USE SINGLE PREC CHEBY = true, COMMUN PREC CHEBY = BF16, TENSOR OP TYPE SINGLE PREC CHEBY = TF32 (Aurora) |
| left at DFT-FE defaults | number of Kohn-Sham states, Chebyshev filter tolerance and degree, mixing history (10), block sizes (AUTO GPU BLOCK SIZES = true) |
| SCF | T = 500 K, tol 1e-5, max 250 iterations, Poisson tol 1e-10 |
| mixing | anderson: ANDERSON at the paper's α; adaptive_anderson: same with ADAPT ANDERSON MIXING PARAMETER = true; ldos: ANDERSON_WITH_LDOS, α 0.5, LDOS TEMPERATURE 10000 |

## Per-system settings

| system | BCs (x y z) | mesh around atom / ball | base mesh | density quad | lpsp / nlpsp quad | k-grid (shift 1 1 0) | Anderson α | cell / vacuum |
|---|---|---|---|---|---|---|---|---|
| Pt_333 | F F F | 1.5 / 4.0 | 6.0, auto-adapt off | 20 | 24 / 24 | none (cluster) | 0.015 | 78.0³ = 13 × 6, vacuum ≥ 27.9 |
| Pt_555 | F F F | 1.5 / 4.0 | 6.0, auto-adapt off | 20 | 24 / 24 | none | 0.015 | 90.0³ = 15 × 6, vacuum ≥ 26.5 |
| Pt_666 | F F F | 1.5 / 4.0 | 6.0, auto-adapt off | 20 | 24 / 24 | none | 0.015 | 96.0³ = 16 × 6, vacuum ≥ 25.8 |
| LGPS | T T F | 1.2 / 4.0 | 4.8, auto-adapt off | 18 | 20 / 20 | 2x2x1 | 0.015 | z = 100.8 = 21 × 4.8, vacuum 27.15 each side |
| SiO2H_20 | T T T | 1.2 / 4.0 | auto | 18 | 20 / 20 | 4x6x1 | 0.2 | z 97.85 -> 99.0, gap 50.07 (25.04 per side) |
| SiO2_20 | T T T | 1.2 / 4.0 | auto | 18 | 20 / 20 | 4x6x1 | 0.05 | unchanged, gap 51.57 |
| PtSiO2H_10 | T T T | 1.2 / 4.0 | auto | 18 | 20 / 20 | 4x6x1 | 0.05 | unchanged (stack) |
| Pt/Au/Al GaAsSiO2H_10/20/40 | T T T | 1.2 / 4.0 | auto | 18 | 20 / 20 | 4x6x1 | Pt 0.015, Au 0.025 (10) / 0.015, Al 0.05 (10) / 0.015 | unchanged (stack) |
| HfO2_Alstripes | T T T | 1.2 / 4.0 | auto | 18 | 20 / 20 | 2x2x1 | 0.015 | unchanged, gap 75.59 |

k-grids are from Srinibas's table. With shift 1 1 0 and time reversal, DFT-FE reports 12 irreducible k-points for 4x6x1 and 2 for 2x2x1, matching the table.
The LGPS row (2x2x1, 2 irreducible) is confirmed by the full table. The verbatim instructions are in srinibas_instructions.md.

Geometry changes only add vacuum: interatomic distances are unchanged (checked to 1e-8 Bohr). Non-periodic cell lengths are exact multiples of the base mesh.
In periodic cells DFT-FE's auto-adapted base mesh is used; BASE MESH SIZE would be ignored there.

## Pseudopotentials: SG15 ONCV (sg15_oncv_upf_2020-02-06)

Version 1.2 for every element except Hf, which uses 1.1 (12 e, as in the paper; Hf 1.2 has 4f in the valence, 26 e).
Both GGA and SCAN use these PBE-generated files. The valence column of every coordinates.inp matches the UPF z_valence:
H 1, Li 3, O 6, Al 11, Si 4, P 5, S 6, Ga 13, Ge 14, As 5, Hf 12, Pt 18, Au 19.
Compared with the paper's PseudoDojo files, As changes 15 -> 5 and Al 3 -> 11.

| system | electrons |
|---|---|
| Pt/Au/Al GaAsSiO2H_10 | 1328 / 1368 / 1048 |
| Pt/Au/Al GaAsSiO2H_20 | 2648 / 2728 / 2088 |
| Pt/Au/Al GaAsSiO2H_40 | 5288 / 5448 / 4168 |
| HfO2_Alstripes | 6894 |
| Pt_333 / 555 / 666 | 3096 / 11988 / 19782 |
| LGPS, SiO2H_20, SiO2_20, PtSiO2H_10 | 2016, 488, 480, 968 |

## Source runs and paper numbers

| system | source (lrdm_paper_runs_sambit/fromStampede/) | paper α / hist | paper Anderson | existing output |
|---|---|---|---|---|
| Pt_333 | dftfeRunsPerlmutter/PtNP3x3x3 | 0.015 / 50 | 53 | 53 |
| Pt_555 | dftfeRunsPerlmutter/PtNP5x5x5 | 0.015 / 50 | 73 | 73 |
| Pt_666 | dftfeRunsPerlmutter/PtNP6x6x6 | 0.015 / 50 | 90 | 90 |
| SiO2H_20 | dftfeRuns/SiO2H_20 | 0.2 / 20 | 16 | 17 |
| SiO2_20 | responseRuns/danglingBonds/SiO2_20 | 0.05 / 20 | 87 | 87 |
| LGPS | dftfeRuns/LGPS2x2x2 | 0.015 / 50 | DNC | stopped at 118 |
| PtSiO2H_10 | dftfeRuns/PtSiO2H_repeat_10 | 0.05 / 20 | 112 | none |
| PtGaAsSiO2H_10 | dftfeRuns/PtGaAsSiO2H_repeat_10 | 0.015 / 20 | 92 | 92 was Γ with α 0.025 |
| AuGaAsSiO2H_10 | dftfeRuns/AuGaAsSiO2H_repeat_10 | 0.025 / 20 | 93 | 93 came from a run on the Pt structure |
| AlGaAsSiO2H_10 | dftfeRuns/AlGaAsSiO2H_repeat_10 | 0.05 / 20 | 55 | 55 was Γ only |
| Pt/Au/Al GaAsSiO2H_20 | dftfeRuns/*GaAsSiO2H_repeat_20 | 0.015 / 20 | DNC | Au output is actually Pt (3048 e) |
| Pt/Au/Al GaAsSiO2H_40 | dftfeRuns/*GaAsSiO2H_repeat_40 | 0.015 / 20 | DNC | Au had no Anderson prm; Al prm had NATOMS = 258 (fixed: 508) |
| HfO2_Alstripes | newAccumulatedRunsPerlmutter/HfO2_P42nmc_t_ONCV_slab001_19Alstripes | 0.015 / 20 | 125 | 125 |

Because the pseudopotentials, mesh, quadrature and k-grids all differ from the paper, iteration counts are not expected to reproduce Table I.

Checked with the local DFT-FE build (ldos_cases/build/release/complex, same branch) on CPU:
Pt_333 (SCAN, LDOS), AlGaAsSiO2H_10 (GGA, Anderson), HfO2_Alstripes (GGA, adaptive) and LGPS (SCAN, LDOS) all reached SCF set-up.
