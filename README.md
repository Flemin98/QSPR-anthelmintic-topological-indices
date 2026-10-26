# QSPR-anthelmintic-topological-indices
Python implementation of QSPR models using topological indices, with regression analysis and computational results.

Code, data and results for: *Degree-based Topological indices for QSPR Modeling of Anthelmintic drug Physicochemical Properties: A Cross-Validated Comparison of Linear and Nonlinear Regression*, Flemin Sajeev, S.Roy, ACS Omega (2026, under review).

Ten degree-based topological indices (Randić, ABC, GA, harmonic, hyper-Zagreb, M1, M2, ISI, SDD, AZI)
are computed on the hydrogen-suppressed graphs of 25 anthelmintic drugs and related to six
ACD/Labs-predicted properties. Simple linear regression (SLR) is the baseline; forward stepwise
regression and RBF support vector regression (SVR) are compared with it by leave-one-out
cross-validation. Two compounds (Bunamidine, Oxyclozanide) serve as an external set.

## Repository layout
| Path | Contents |
|---|---|
| `QSPR_Anthelmintic_Analysis.ipynb` | Complete analysis, cells 1-11, run in order |
| `data/` | Input data (properties from ChemSpider, external-set values) and computed index tables |
| `results/` | All tables used in the paper and SI (CSV) and the consolidated workbook `QSPR_Anthelmintic_Complete_Results.xlsx` |
| `figures/` | All figures (PNG, 300 dpi) |
| `requirements.txt` | Pinned package versions |

## How to reproduce
1. Open the notebook in Google Colab and place the repository folder in Google Drive
   (the notebook expects `/content/drive/MyDrive/QSPR_Anthelmintic_repo`; change `PROJECT_DIR` in Cell 1 if you use another path).
2. Run the cells in order. Cell 7 (nested SVR) takes several minutes.
3. Random seed: 42 (inner CV of the SVR grid search). Versions are written to `results/environment.txt`.

## Data provenance
- Structures: PubChem connectivity SMILES (CIDs listed in Cell 2 and in `data/topological_indices_25drugs.csv`, with retrieval date).
- Properties: ACD/Labs Percepta **predicted** values as displayed on ChemSpider (retrieved March 2026); they are not experimental measurements.
  Eight drugs have no boiling point, enthalpy of vaporization or flash point in the source, so n = 17 for those three properties and n = 25 for the others.
**Timeline.** Property values (ACD/Labs predictions on ChemSpider) were retrieved in March 2026 and the
original analysis was completed in April 2026. For this repository the full pipeline was re-run in
October 2026 (structures re-fetched from PubChem; all tables reproduced the values in the paper).
File timestamps and the "Generated" lines in `results/` therefore show the October run.

## Mapping to the paper
| Paper | File |
|---|---|
| Table 4 (indices) | `data/topological_indices_25drugs.csv` |
| Table 6 (SLR) | `results/SLR_best_TI_summary.csv` (all indices: `SLR_R2_matrix.csv`, `SLR_Q2_matrix.csv`, `SLR_index_ranking.csv`) |
| Table 7 (stepwise) | `results/stepwise_LR_summary.csv` (diagnostics: `stepwise_vif_summary.csv`, `stepwise_all_candidates.csv`) |
| Table 8 (SVR) | `results/SVR_summary.csv`, `results/SLR_vs_SVR_comparison.csv` |
| Table 9 (best performing model per property) | `results/model_comparison_summary.csv` (highest Q² per property; also `results/SLR_vs_SVR_comparison.csv`) |
| Table 10 (external set) | `results/external_validation_table9.csv` |
| Table 11 (Applicability domain) | `results/applicability_domain_external.csv`, `figures/williams_plot_external_validation.png` |

## Notes
- For a single standardized feature, SVR `gamma='scale'` and `'auto'` are identical (both 1.0); the label reported by the grid search can flip between runs.
- Flash point fails the R² / Q² criteria for all models and is not interpreted.

## License
Code: MIT. Data derived from ChemSpider/PubChem are subject to the terms of those services.
