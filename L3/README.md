# Layer 3 — DOOIT2 Nested Optimization and Final ESOL Results

This directory contains the principal outputs from **Layer 3 (L3)** of the 4L-QSPR workflow.

L3 performs the final supervised model optimization, recursive descriptor pruning, and leakage-aware performance evaluation using nested `StratifiedGroupKFold` validation.

The deposited files correspond to the final locked ESOL configuration:

```text
Descriptor representation: COSMO–PaDEL
Regressor: LightGBM
Structural grouping: hier_level_100
Outer validation folds: 5
Inner validation folds: 4
Optuna trials per pruning step: 30
Minimum descriptor count: 4
Random seeds: 7, 42, and 123
````

## Directory contents

The directory contains the following workbooks:

```text
seed7_dooit2_fold_summary.xlsx
seed42_dooit2_feature_stability.xlsx
seed42_dooit2_fold_summary.xlsx
seed123_dooit2_fold_summary.xlsx
```

## Fold-summary workbooks

The following three files contain the results of complete independent L3 runs:

```text
seed7_dooit2_fold_summary.xlsx
seed42_dooit2_fold_summary.xlsx
seed123_dooit2_fold_summary.xlsx
```

Each workbook summarizes the five outer validation folds for one random seed.

The reported information includes, depending on the worksheet:

* outer-fold number;
* outer-fold MAE;
* outer-fold R²;
* number of descriptors retained after recursive pruning;
* selected descriptor names;
* optimized model configuration;
* fold-level results generated after completion of inner optimization;
* summary statistics across the five outer folds.

The seed number affects stochastic elements of the L3 procedure, including:

* outer and inner fold shuffling;
* Optuna trial generation;
* LightGBM initialization;
* feature-importance estimation;
* recursive descriptor-pruning trajectories.

The L1 structural assignments themselves remain fixed and identical across all three runs.

## Representative seed-42 stability workbook

The file:

```text
seed42_dooit2_feature_stability.xlsx
```

contains descriptor-selection stability information for the representative seed-42 run.

It summarizes how frequently individual descriptors were retained across the five seed-42 outer-fold models.

The workbook can be used to identify:

* descriptors selected in all five outer folds;
* descriptors selected in most outer folds;
* descriptor families showing fold-dependent selection;
* the relative recurrence of COSMO-derived and PaDEL descriptors.

The seed-42 run was used as the representative run for the fold-wise performance table, descriptor-recurrence table, and parity plot presented in the manuscript.

## Nested validation design

The final model was evaluated using nested `StratifiedGroupKFold` validation.

### Outer loop

The five-fold outer loop was used exclusively for final performance estimation.

Molecules belonging to the same `hier_level_100` L1 group were assigned to the same outer fold. Consequently, molecules from a held-out structural group could not occur simultaneously in the outer-training and outer-test subsets.

The held-out outer fold was not used for:

* descriptor selection;
* descriptor removal;
* hyperparameter optimization;
* pruning-depth selection;
* model-complexity selection.

### Inner loop

Within each outer-training subset, four-fold inner `StratifiedGroupKFold` validation was used for:

* LightGBM hyperparameter optimization;
* recursive descriptor pruning;
* evaluation of candidate descriptor subsets;
* selection of the final model for the corresponding outer fold.

The continuous `logS` endpoint was transformed into quantile-based strata to improve response-distribution balance among folds.

The target strata did not replace or modify the L1 groups. L1 groups controlled structural separation, whereas the target strata were used only for fold balancing.

## DOOIT2 optimization

The DOOIT2 procedure combines:

1. nested group-aware validation;
2. multi-objective hyperparameter optimization;
3. recursive descriptor pruning;
4. model-complexity control;
5. final held-out outer-fold evaluation.

At each descriptor-pruning step:

1. LightGBM hyperparameters were optimized within the inner folds;
2. candidate models were evaluated using inner-fold MAE;
3. model complexity was included as an additional optimization objective;
4. descriptor importance was calculated;
5. the least useful descriptor was removed;
6. optimization was repeated using the reduced descriptor set.

The descriptor subset and hyperparameters selected for each outer fold were determined entirely from the corresponding outer-training data.

## Final performance

The final seed-wise results were:

|                   Seed |    MAE, mean ± SD |     R², mean ± SD | Selected descriptors, mean ± SD |
| ---------------------: | ----------------: | ----------------: | ------------------------------: |
|                      7 |     0.454 ± 0.042 |     0.905 ± 0.013 |                      21.8 ± 5.1 |
|                     42 |     0.460 ± 0.046 |     0.895 ± 0.031 |                      22.2 ± 5.8 |
|                    123 |     0.461 ± 0.030 |     0.891 ± 0.018 |                      25.0 ± 2.9 |
| Across seed-wise means | **0.459 ± 0.004** | **0.897 ± 0.007** |                  **23.0 ± 1.7** |

The values in the final row represent the standard deviation among the three seed-wise mean results, rather than pooled variability among all 15 outer folds.

The small between-seed variability indicates that the final performance was not dependent on one favorable stochastic realization of the L3 procedure.

## Descriptor recurrence

Across the 15 outer-fold models generated for seeds 7, 42, and 123, nine descriptors were retained in every model:

```text
ln_gamma_water
ln_gamma_wet_octanol
DeltaH_int_wet_octanol_minus_water
XLogP
piPC4
solute_pure_2_Chemical_potential
SpMax3_Bhm
SpMax4_Bhm
SpMin4_Bhs
```

These descriptors represent both principal components of the combined descriptor space.

### COSMO-derived descriptors

```text
ln_gamma_water
ln_gamma_wet_octanol
DeltaH_int_wet_octanol_minus_water
solute_pure_2_Chemical_potential
```

These variables describe aqueous or reference-environment activity, solvation preference, interaction-energy contrasts, and chemical-potential-related behavior.

### PaDEL descriptors

```text
XLogP
piPC4
SpMax3_Bhm
SpMax4_Bhm
SpMin4_Bhs
```

These variables describe lipophilicity, molecular graph complexity, and graph-spectral or topological characteristics.

The recurrence of descriptors from both sources supports the complementarity of the combined COSMO–PaDEL representation.

## Relationship to the manuscript

The deposited files provide the numerical basis for:

* the fold-wise performance table for the representative seed-42 run;
* the seed-42 descriptor-recurrence table;
* the three-seed robustness table;
* the discussion of descriptor-selection stability;
* the reported final MAE and R² values;
* the conclusion that the final model was not a single-run artifact.

The complete three-seed analysis includes 15 independently optimized outer-fold models:

```text
3 random seeds × 5 outer folds = 15 final outer-fold models
```

Each model has its own descriptor subset and optimized LightGBM configuration.

## Important interpretation

The fold-summary workbooks contain results of independently optimized outer-fold models. They should not be interpreted as repeated evaluations of one globally optimized model.

For each outer fold:

* descriptor pruning was repeated;
* hyperparameters were re-optimized;
* the final descriptor subset could differ;
* the held-out structural groups remained unseen during model selection.

Therefore, variability in selected descriptors is expected and forms part of the nested-validation result.

## Reproducing the L3 analysis

To reproduce the final calculations:

1. use `L3_COSMOPaDEL_lgbm.xlsx` from the L2 directory;
2. use `logS` as the target variable;
3. use `hier_level_100` as the structural grouping variable;
4. exclude identifiers, target values, and L1 labels from the descriptor matrix;
5. set the outer loop to five grouped folds;
6. set the inner loop to four grouped folds;
7. use 30 Optuna trials at each pruning step;
8. use four descriptors as the minimum pruning limit;
9. repeat the complete calculation for seeds 7, 42, and 123;
10. store separate checkpoints and output directories for each seed.

The resulting fold-wise summaries should be compared with the deposited workbooks.

## Files not represented by these summaries

The workbooks in this directory contain tabular L3 results, but they are not trained-model binaries.

Where available, additional reproducibility material may include:

* molecule-level outer-fold predictions;
* fold assignments;
* optimized LightGBM parameters;
* Optuna study outputs;
* checkpoint files;
* trained model objects;
* parity-plot source data;
* environment and package metadata.

## Methodological boundary

L3 is endpoint-dependent, but all optimization must remain confined to the appropriate training data.

The following operations must never use the held-out outer fold:

* descriptor ranking;
* descriptor removal;
* hyperparameter selection;
* pruning-depth selection;
* model-complexity selection;
* stopping decisions.

The outer folds are used only for final evaluation after the model-development decisions for that fold have been completed.

## Citation

When using the L3 output files or the DOOIT2 workflow, please cite the associated manuscript:

> P. Cysewski,
> **Chemically Informed and Leakage-Aware QSPR Modeling: The 4L-QSPR Framework Applied to Aqueous Solubility Prediction**

Full bibliographic details will be added after publication.

## Contact

**Piotr Cysewski**
Department of Physical Chemistry
Faculty of Pharmacy, Collegium Medicum in Bydgoszcz
Nicolaus Copernicus University in Toruń
Bydgoszcz, Poland

Email: [piotr.cysewski@cm.umk.pl](mailto:piotr.cysewski@cm.umk.pl)

