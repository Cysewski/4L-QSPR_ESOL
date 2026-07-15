# Layer 2 — Descriptor Preselection and L3 Input Files

This directory contains the descriptor tables produced by **Layer 2 (L2)** of the 4L-QSPR workflow and prepared as inputs for the subsequent Layer 3 nested modeling procedure.

L2 is the supervised descriptor-processing layer. It operates after the endpoint-independent L1 structural grouping has been fixed and before the final nested optimization performed in L3.

## Directory contents

The directory contains three LightGBM input workbooks corresponding to three descriptor representations:

```text
L2_COSMO_lgbm.xlsx
L2_COSMOPaDEL_lgbm.xlsx
L2_PaDEL_lgbm.xlsx
````

These files contain the curated ESOL records together with the molecular identifiers, target values, L1 grouping assignments, and L2-retained descriptor subsets required for L3 modeling.

## Descriptor spaces

### `L2_COSMO_lgbm.xlsx`

This workbook contains descriptors derived from COSMO-RS-related calculations.

The COSMO-derived descriptor space includes physicochemical quantities such as:

* activity coefficients in selected reference environments;
* chemical potentials;
* interaction-energy terms;
* hydrogen-bonding contributions;
* sigma-profile- and sigma-potential-related descriptors;
* solvation-related quantities;
* reference-environment contrasts;
* water-, hexane-, and wet-octanol-related descriptors.

Representative descriptors include:

```text
ln_gamma_water
ln_gamma_hexane
ln_gamma_wet_octanol
H_int_water
H_int_hexane
H_int_wet_octanol
solute_wet_octanol_Chemical_potential
DeltaH_int_wet_octanol_minus_water
DeltaLn_activity_water_minus_hexane
```

The target `logS` values were not used during conformer generation, quantum-chemical calculations, COSMOtherm calculations, or extraction of the original COSMO-derived descriptors.

### `L2_PaDEL_lgbm.xlsx`

This workbook contains conventional two-dimensional molecular descriptors calculated using PaDEL-Descriptor.

The PaDEL representation includes descriptor families related to:

* molecular composition;
* lipophilicity;
* topology;
* graph structure;
* autocorrelation;
* electronic properties;
* atom-type and path-based information;
* molecular complexity.

Representative PaDEL descriptors retained during the modeling workflow include:

```text
XLogP
CrippenLogP
LipoaffinityIndex
MLFER_E
BCUTp-1h
piPC4
SpMax3_Bhm
SpMax4_Bhm
SpMin4_Bhs
```

### `L2_COSMOPaDEL_lgbm.xlsx`

This workbook contains the combined COSMO–PaDEL descriptor representation.

The file was constructed by concatenating the curated COSMO-derived and PaDEL descriptor tables at the molecule level after verifying compound alignment.

The combined representation was designed to provide complementary information:

* PaDEL descriptors encode molecular structure, topology, lipophilicity, and graph properties;
* COSMO-derived descriptors encode solvation, polarity, interaction energies, and reference-environment behavior.

The combined COSMO–PaDEL representation was used in the final locked LightGBM demonstration reported in the manuscript.

## Molecular identifiers and grouping information

Each workbook contains the information required to connect the descriptor matrices with the curated ESOL dataset and the L1 hierarchy.

Depending on the exact deposited workbook, the non-descriptor columns include:

* SMILES;
* InChIKey, where available;
* experimental aqueous solubility expressed as `logS`;
* `hier_level_50`;
* `hier_level_100`;
* other molecular or grouping identifiers retained for data alignment.

For the final ESOL analysis, the L3 grouping variable was:

```text
hier_level_100
```

The L1 hierarchy was generated independently of `logS`, descriptor rankings, model residuals, and predictive-performance results.

## L2 descriptor-processing procedure

L2 transformed the initial descriptor matrices into compact candidate descriptor pools suitable for nested L3 optimization.

The processing workflow included:

* identification and conversion of numerical columns;
* removal of invalid or non-numeric descriptor fields;
* removal of descriptors containing unusable missing or infinite values;
* removal of constant and near-constant descriptors;
* reduction of highly correlated and redundant variables;
* imputation of remaining missing numerical values;
* supervised univariate filtering;
* regressor-specific descriptor preselection.

The supervised filtering and ranking steps use the target property and therefore belong to L2 rather than L1.

L2 does not define structural groups and does not alter the L1 hierarchy.

## Relationship between L2 and L3

The workbooks in this directory are **inputs to L3**, not final trained models.

L2 provides a candidate descriptor pool. L3 subsequently performs:

* nested StratifiedGroupKFold validation;
* inner-fold hyperparameter optimization;
* recursive descriptor pruning;
* model-complexity optimization;
* selection of the final descriptor subset;
* evaluation on held-out outer L1 groups.

Therefore, the presence of a descriptor in one of these workbooks does not imply that it was selected in every final model.

Final descriptor subsets may differ among outer folds because descriptor pruning and hyperparameter selection are repeated independently within each outer-training set.

## Final manuscript configuration

The final confirmatory model reported in the manuscript used:

```text
Descriptor representation: COSMO–PaDEL
Regressor: LightGBM
Grouping variable: hier_level_100
Outer folds: 5
Inner folds: 4
Optuna trials per pruning step: 30
Minimum descriptor count: 4
Random seeds: 7, 42, 123
```

The final three-seed performance was:

```text
MAE = 0.459 ± 0.004 logS units
R²  = 0.897 ± 0.007
```

These values represent variability across the three seed-wise mean results.

## Endpoint dependence and leakage boundary

L2 is intentionally endpoint-dependent.

The `logS` values may be used for:

* univariate descriptor filtering;
* regressor-specific descriptor ranking;
* preparation of candidate descriptor pools.

However, L2 results were not used to construct or modify the L1 structural hierarchy.

During L3, descriptor pruning, hyperparameter selection, and model selection were confined to the corresponding outer-training data through nested validation.

The held-out outer folds were not used to choose descriptors or optimize model settings.

## Workbook use

To use one of the workbooks in L3:

1. select the workbook corresponding to the desired descriptor representation;
2. verify that molecular identifiers and row order match the curated ESOL table;
3. use `logS` as the target column;
4. use `hier_level_100` as the structural grouping column;
5. exclude molecular identifiers, target values, and L1 labels from the model descriptor matrix;
6. perform all final descriptor pruning and hyperparameter optimization within the nested L3 procedure.

## Important interpretation

The three workbooks should not be interpreted as three directly comparable final models unless they are evaluated under the same complete nested L3 protocol.

In particular, performance calculated directly from an L2-selected descriptor table without nested outer-fold evaluation would not represent the final leakage-aware performance reported in the manuscript.

## Reproducibility

For reproducible use, the deposited workbooks should be retained together with:

* the curated ESOL molecular table;
* the final L1 group assignments;
* descriptor-generation metadata;
* L2 preprocessing code;
* the final L3 notebook or script;
* fold-level outputs;
* selected-descriptor lists;
* environment and package information.

## Citation

When using these descriptor tables or the L2 workflow, please cite the associated manuscript:

> P. Cysewski,
> **Chemically Informed and Leakage-Aware QSPR Modeling: The 4L-QSPR Framework Applied to Aqueous Solubility Prediction**

Full bibliographic information will be added after publication.

## Contact

**Piotr Cysewski**
Department of Physical Chemistry
Faculty of Pharmacy, Collegium Medicum in Bydgoszcz
Nicolaus Copernicus University in Toruń
Bydgoszcz, Poland

Email: [piotr.cysewski@cm.umk.pl](mailto:piotr.cysewski@cm.umk.pl)


