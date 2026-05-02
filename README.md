# Final submission checklist - 3 dashboard version

## Required deliverables

- [ ] Tableau packaged workbook: `.twbx`
- [ ] Final report PDF
- [ ] Python script

## Tableau workbook checks

- [ ] Dashboard 1 title: `Health inequality across England and Wales`
- [ ] Dashboard 2 title: `Structural profiles of health, housing and economic inequality`
- [ ] Dashboard 3 title: `Bayesian model diagnostics and residual geography`
- [ ] Dashboard 1 has no unknown map locations
- [ ] Dashboard 2 cluster map has no unknown map locations
- [ ] Dashboard 3 residual map has no unknown map locations
- [ ] Cluster colours match across cluster map, PCA and Isomap
- [ ] Change map colour scale is centred at 0
- [ ] Residual map colour scale is centred at 0
- [ ] Profile chart uses AVG, not SUM
- [ ] No accidental selected marks remain before saving
- [ ] Tooltips include local authority name and relevant percentages
- [ ] Extra legends are hidden; only necessary legends remain

## Data source checks

- [ ] `tableau_master.csv` is used for PCA, Isomap, Bayesian scatter and profile views
- [ ] `tableau_map_hybrid_unknown_fix.csv` is used for map views that need robust geocoding
- [ ] Do not globally replace `tableau_master` with the hybrid map source

## Report checks

- [ ] Name and student ID are added
- [ ] Report describes three dashboards, not two
- [ ] Report mentions cluster map and residual map
- [ ] Report explains 2011 raw percentage vs 2021 age-standardised percentage limitation
- [ ] Report explains residual = observed minus predicted
- [ ] Report remains within the required page range

## Final packaging check

- [ ] Save as `.twbx`, not `.twb`
- [ ] Close Tableau
- [ ] Reopen the `.twbx` from a different folder
- [ ] Confirm dashboards, maps and tooltips still work
