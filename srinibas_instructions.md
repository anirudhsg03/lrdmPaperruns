# Srinibas Nandi's instructions (Slack, 2026-11-24), transcribed from screenshots

## Message 1 (12:35 PM)
- Use mesh size 7, 1.2, and atom ball radius = 4
- density quad rule = 18
- electrostatic polynomial degree default
- higher quad psp (lpsp and nlpsp = 20) (in the new pgd set the parameters accordingly.)
- set the other quadrature rules to be the default before they were parameters in the recent commit.
- at least 25 Bohr of vacuum in each side (for the non periodic or semi periodic cases you can use base mesh false with the domain vectors to be multiple of base mesh size. and base mesh size can be 4.8)
- number of kohn-sham states, chebyshev tolerance, polynomial degree, mixing history default
- verbosity = 4
- sg15 psp for all the runs . make sure you use the correct number of valence electrons in the coordinates.inp (since the psp is changing, that might change as well.)
- use the proper number of k point (refer to the attached figure). Use time reversal symmetry only.
- Turn off multipole boundary conditions.

## Message 2
Also in all the runs use single prec cheby, commun prec cheby = bf16 (if you are running in aurora, use TF32 tensor OP type)

## Message 3 (12:47 PM)
For Pt555/333/666 use mesh size 7, 1.4 and density quad rule 20 and lpsp and nlpsp to be 24 both

Follow-up (edited): use 7, 1.5 (base mesh = 6) for the above only

## Attached figure: k-point table

| system | n_atoms | periodicity | k-grid | a·n_k (bohr), x × y × z | irreducible k |
|---|---|---|---|---|---|
| Pt_333 | 172 | non-periodic | Γ | – | 1 |
| Pt_555 | 666 | non-periodic | Γ | – | 1 |
| Pt_666 | 1099 | non-periodic | Γ | – | 1 |
| LGPS | 400 | semi-periodic (z not periodic) | 2×2×1 | 65.4 × 66.7 × – | 2 |
| HfO2_Alstripes | 864 | periodic (slab, vacuum along z) | 2×2×1 | 57.6 × 57.6 × 153.8 | 2 |
| SiO2_20 | 90 | periodic (slab, vacuum along z) | 4×6×1 | 65.3 × 62.1 × 97.9 | 12 |
| SiO2H_20 | 98 | periodic (slab, vacuum along z) | 4×6×1 | 65.3 × 62.1 × 97.9 | 12 |
| PtSiO2H_10 | 93 | periodic (stack) | 4×6×1 | 65.3 × 62.1 × 67.8 | 12 |
| AlGaAsSiO2H_10 | 133 | periodic (stack) | 4×6×1 | 60.4 × 64.1 × 105.6 | 12 |
| AuGaAsSiO2H_10 | 133 | periodic (stack) | 4×6×1 | 60.4 × 64.1 × 105.6 | 12 |
| PtGaAsSiO2H_10 | 133 | periodic (stack) | 4×6×1 | 60.4 × 64.1 × 105.6 | 12 |
| AlGaAsSiO2H_20 | 258 | periodic (stack) | 4×6×1 | 60.4 × 64.1 × 205.5 | 12 |
| AuGaAsSiO2H_20 | 258 | periodic (stack) | 4×6×1 | 60.4 × 64.1 × 205.5 | 12 |
| PtGaAsSiO2H_20 | 258 | periodic (stack) | 4×6×1 | 60.4 × 64.1 × 205.5 | 12 |
| AlGaAsSiO2H_40 | 508 | periodic (stack) | 4×6×1 | 60.4 × 64.1 × 414.1 | 12 |
| AuGaAsSiO2H_40 | 508 | periodic (stack) | 4×6×1 | 60.4 × 64.1 × 414.1 | 12 |
| PtGaAsSiO2H_40 | 508 | periodic (stack) | 4×6×1 | 60.4 × 64.1 × 414.1 | 12 |

(The a·n_k column was computed for the original cells. Cells that were later enlarged for vacuum will differ along z.)

## Not yet available
Two more systems (CePd and the Li_Li3NO3 interface) are in Archive.zip on Slack. It has not been downloaded yet.
