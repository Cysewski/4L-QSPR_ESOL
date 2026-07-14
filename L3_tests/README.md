# L3 Tests — Synthetic Validation Summary Figures

This directory contains the summary figures generated during synthetic validation of the **DOOIT2 Layer 3 procedure** used in the 4L-QSPR framework.

The tests were designed to examine how the nested optimization and validation workflow behaves under controlled data-generating conditions representing common challenges in QSPR and cheminformatics.

## Directory contents

The directory contains six summary figures:

```text
test1_summary.png
test2_summary.png
test3_summary.png
test4_summary.png
test5_summary.png
test6_summary.png
````

Each file corresponds to the original output figure named:

```text
fig1_summary.png
```

generated for the respective synthetic test.

For repository clarity, the files were renamed according to the test number:

```text
fig1_summary.png from Test 1 → test1_summary.png
fig1_summary.png from Test 2 → test2_summary.png
fig1_summary.png from Test 3 → test3_summary.png
fig1_summary.png from Test 4 → test4_summary.png
fig1_summary.png from Test 5 → test5_summary.png
fig1_summary.png from Test 6 → test6_summary.png
```

The renaming changes only the file names. The figure content is unchanged.

## Synthetic validation tests

### Test 1 — Baseline linear signal

```text
test1_summary.png
```

Purpose:

* verify that DOOIT2 can recover a clear predictive relationship under relatively favorable conditions;
* confirm that the optimization and feature-selection procedure preserves informative variables;
* establish a baseline for comparison with more difficult scenarios.

Expected interpretation:

* high predictive performance;
* recovery of the principal informative descriptors;
* limited selection of irrelevant noise variables.

### Test 2 — Strong group effect

```text
test2_summary.png
```

Purpose:

* examine generalization when observations are organized into strongly differentiated groups;
* test whether grouped validation prevents information leakage between related observations;
* evaluate the expected loss of performance when complete groups are held out.

Expected interpretation:

* lower performance than under random or weakly grouped conditions;
* exposure of genuine group-extrapolation difficulty;
* absence of artificially inflated performance caused by group leakage.

### Test 3 — Nonlinear signal

```text
test3_summary.png
```

Purpose:

* test whether the L3 workflow can recover nonlinear structure–property relationships;
* compare the behavior of regressors with different capacities for nonlinear modeling;
* verify that feature pruning does not remove descriptors involved in nonlinear signal.

Expected interpretation:

* moderate-to-high predictive performance for nonlinear-capable regressors;
* preservation of informative variables despite nonlinear relationships;
* poorer performance for algorithms that cannot adequately represent the data-generating function.

### Test 4 — Correlated descriptors

```text
test4_summary.png
```

Purpose:

* evaluate robustness to descriptor redundancy and multicollinearity;
* test whether the feature-pruning procedure can retain predictive information while reducing unnecessary correlated variables;
* examine the stability of descriptor selection when several variables carry overlapping information.

Expected interpretation:

* predictive performance remains high;
* redundant descriptors may be selected interchangeably;
* descriptor counts are reduced without substantial loss of accuracy.

### Test 5 — No-signal control

```text
test5_summary.png
```

Purpose:

* determine whether the workflow produces artificial predictive structure when no real signal is present;
* test the behavior of hyperparameter optimization and descriptor pruning under pure-noise conditions;
* provide a negative control for false-positive model development.

Expected interpretation:

* R² values close to zero or negative;
* no stable recovery of informative descriptors;
* absence of convincing predictive performance.

This test is especially important because it checks whether the optimization procedure can distinguish genuine signal from random correlations.

### Test 6 — Interaction effects

```text
test6_summary.png
```

Purpose:

* evaluate the ability of the workflow to model relationships driven by interactions among descriptors;
* test whether nonlinear and tree-based models recover higher-order structure;
* assess whether recursive feature pruning retains variables that are weak individually but informative through interactions.

Expected interpretation:

* strong performance for regressors capable of modeling interactions;
* lower performance for models limited to simpler additive relationships;
* retention of descriptors contributing jointly to the target.

## Role of the synthetic tests

The synthetic tests were not designed to rank regression algorithms universally.

Their purpose was to verify that DOOIT2 behaves sensibly under known conditions, including:

* straightforward predictive signal;
* strong group structure;
* nonlinear relationships;
* correlated descriptors;
* complete absence of signal;
* higher-order descriptor interactions.

Together, the figures provide a compact visual summary of the methodological validation described in the manuscript and Supporting Information.

## Relationship to the manuscript

The results of these tests support the use of DOOIT2 as the optimization and validation engine in Layer 3 of the 4L-QSPR framework.

The main conclusions are:

* predictive signal is recovered when present;
* performance decreases appropriately under difficult group extrapolation;
* nonlinear and interaction effects can be modeled;
* descriptor redundancy does not prevent useful model construction;
* no-signal data do not produce artificial positive predictive performance.

The figures correspond to the synthetic validation results summarized in the manuscript section describing DOOIT2 benchmark behavior.

## File naming note

The original computational workflow generated a file named:

```text
fig1_summary.png
```

inside the output directory of each test.

Because all six files had the same original name, they were renamed before deposition in this repository to avoid ambiguity.

The repository names preserve the test identity:

```text
test1_summary.png
...
test6_summary.png
```

## Reuse

These figures may be used to:

* inspect the behavior of the L3 validation procedure;
* compare model responses across controlled synthetic scenarios;
* reproduce the Supporting Information;
* verify consistency with the manuscript discussion;
* support further development of DOOIT2.

The figures are summary outputs and do not contain the complete underlying fold-level data, model objects, or optimization histories.

## Citation

When using these figures or the synthetic validation results, please cite the associated manuscript:

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

```
```
