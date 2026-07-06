# superconductors-ml

ML pipeline for predicting superconducting critical temperature (`Tc`) from composition
and crystal structure. Work from a summer '23 research project with the Harvard Hoffman Lab.

Presentation: [presentation.pdf](presentation.pdf) (also served via GitHub Pages).

## Pipeline

1. **Source data** — SuperCon (MDR `220808_MDR_OAndM`) as the labeled `Tc` set; candidate
   structures/properties queried from Materials Project (`MPRester`), OQMD, and ICSD; 3DSC
   for doped structural matches; an insulators set (~76k) as negatives for classification.
2. **Matching** — SuperCon compositions matched to stable Materials Project entries with
   structures (`only_stable_mp_matches_withStructure`).
3. **Featurization** — `matminer` composition featurizers: Magpie/`ElementProperty`,
   `Meredig`, plus stoichiometric and structural descriptors (min relative distance,
   structural heterogeneity). Reproduces Hamidieh (2018) element-property features.
4. **Modeling** — regression (`XGBRegressor`, `LinearRegression`) and classification of
   superconductor vs. insulator; parity plots, feature importances, error analysis.

## Layout

- `data-prep/` — notebooks for data acquisition, matching, and featurization
  (`MaterialsProject-*`, `OQMD-*`, `ICSD-*`, `MatMiner-*`, `MatMiner2-Composition-*`,
  `Dries-ClassificationModel-*`, `full-SC-exploration`). Intermediate feature tables
  (`*14k.csv`, `MM-*.csv/.xlsx`). `3DSC/` is the vendored 3DSC repo.
- `datasets/` — raw and processed data: SuperCon MDR dump, `element_data*`, MP match
  tables, feature-augmented material tables (`df_unique_materials_wfeatures*`).
- `machine-learning/` — model notebooks (`element-data-model`, `initial_regressions`).
- `external-files/` — Hamidieh R sources (`R-from-Hamidieh/`) and MP query reference.

## Stack

Python: `pandas`, `numpy`, `scikit-learn`, `xgboost`, `matminer`, `pymatgen`/`mp-api`,
`matplotlib`, `seaborn`. Notebook-driven; data in CSV/XLSX/JSON.
