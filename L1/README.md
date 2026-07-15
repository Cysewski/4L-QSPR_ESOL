# Layer 1 — Curated Molecular Universe and Structural Grouping

This directory contains the curated molecular reference universe and the code, configuration, and outputs used to construct **Layer 1 (L1)** of the 4L-QSPR framework.

L1 provides endpoint-independent structural groups for leakage-aware molecular validation. The groups are generated without using aqueous-solubility values, descriptor rankings, model residuals, or downstream predictive-performance results.

## Directory contents

The principal input file is:

```text
L1_mol_curated.xlsx
It contains the final curated molecular universe used to construct the L1 hierarchy.
The dataset comprises:
342,526 unique molecular identities
Each record contains, at minimum:
•	a canonical molecular SMILES representation;
•	a validated InChIKey used as the principal molecular identifier.
Depending on the deposited version, the workbook may also contain source or auxiliary identity fields retained during curation.

Origin of the molecular universe
The reference universe was assembled from public chemical information sources, including:
•	PubChem;
•	ChemSpider;
•	ChEMBL.
The combined records were subjected to a uniform identity and structure curation procedure before L1 construction.
Molecular curation
The final molecular universe was prepared using structure- and identity-based criteria only.
The curation procedure included:
•	removal of records with missing molecular structures;
•	removal of invalid or non-parsable SMILES;
•	removal of inorganic and metal-containing structures;
•	removal of multi-component, dot-separated molecular records;
•	validation of consistency between molecular structure and InChIKey;
•	removal of records with inconsistent molecular identifiers;
•	deduplication at the InChIKey level;
•	retention of distinct stereochemical identities when represented by distinct stereochemical InChIKeys.
No experimental endpoint values were used during curation.
L1 molecular representation
Each molecule was represented by two complementary components.
Morgan fingerprint representation
•	fingerprint type: Morgan circular fingerprint;
•	radius: 2;
•	vector length: 2048 bits;
•	similarity measure: Tanimoto coefficient;
•	fingerprint distance:
D_FP = 1 - Tanimoto
RDKit8 physicochemical representation
Eight endpoint-independent RDKit descriptors were used:
1.	NumHBD
2.	NumHBA
3.	TPSA
4.	NumRotatableBonds
5.	NumAromaticRings
6.	FractionCSP3
7.	MolWt
8.	MolLogP
The descriptors were standardized using parameters calculated from the complete curated reference universe.
The RDKit8 component was compared using cosine distance.
Hybrid molecular distance
The final L1 distance was defined as:
D_L1 = 0.80 × D_FP + 0.20 × D_RDKit8
The weighting was fixed before the ESOL modeling analysis and was not tuned using logS, model residuals, descriptor importance, or predictive performance.
Deterministic L1 implementation
The final production implementation is based on:
L1B_hybrid_rdkit8_v55_4
The implementation uses:
•	stable internal ordering by InChIKey and SMILES;
•	identity-based tie resolution;
•	double-precision distance calculations;
•	deterministic MaxMin representative selection;
•	deterministic nearest-representative assignment;
•	deterministic capacity-constrained fine partitioning;
•	deterministic average-linkage hierarchical clustering;
•	canonical group numbering;
•	stage-specific SHA-256 hashes;
•	a final run signature for reproducibility verification.
Two independent executions on the complete 342,526-molecule universe produced the identical final run signature:
df6846b276a573019e9ba66d5fa26e6a8f3cb0df812974e7985d0228959effa2
This signature verifies that the final molecular hierarchy was reproduced exactly under the recorded computational environment and configuration.
L1 hierarchy
The reference universe was first represented by 500 deterministic coarse representatives. Average-linkage hierarchical clustering of these representatives was then used to generate several structural resolutions:
•	hier_level_10
•	hier_level_20
•	hier_level_50
•	hier_level_100
•	hier_level_200
•	hier_level_500
The hierarchy levels are intended for different validation resolutions. They are not named chemical classes and should not be interpreted as a universal chemical ontology.
For the curated ESOL dataset, hier_level_100 was selected as the structural grouping variable used in nested StratifiedGroupKFold validation.
The level was selected from group-size and fragmentation statistics only. Downstream solubility-model performance was not used to choose the hierarchy level.
Typical output files
The L1 workflow produces files such as:
L1_v55_4_L1B_hierarchical_mapping.xlsx
L1_v55_4_L1B_rdkit8_scaler.json
L1_v55_4_L1B_determinism_audit.json
L1_v55_4_L1B_run_signature.txt
L1_v55_4_L1B_projection.png
The hierarchical mapping contains the molecular identifiers together with the generated L1 assignments, including coarse, fine, and hierarchical group labels.
The determinism-audit file records configuration details and hashes of important intermediate results.
Reproducing the calculation
To reproduce the L1 hierarchy:
1.	place L1_mol_curated.xlsx in the input location expected by the notebook;
2.	open the final L1 notebook;
3.	restart the Python kernel;
4.	execute all cells from the beginning;
5.	verify the reported feature hashes and final run signature;
6.	compare the resulting signature with the reference value given above.
A fresh kernel is recommended to prevent stale notebook definitions or cached variables from influencing execution.
Computational requirements
The complete L1 calculation processes more than 342,000 molecules and therefore requires substantial memory and execution time.
The workflow uses, among others:
•	Python;
•	pandas;
•	NumPy;
•	SciPy;
•	scikit-learn;
•	RDKit;
•	joblib;
•	openpyxl.
Exact package versions should be taken from the deposited environment or reproducibility metadata files.
Intended use
The deposited molecular universe and L1 assignments may be used to:
•	reproduce the structural grouping employed in the 4L-QSPR ESOL study;
•	assign validation groups to subsets of the reference universe;
•	investigate different hierarchy resolutions;
•	support leakage-aware QSPR or QSAR validation;
•	compare alternative endpoint-independent molecular grouping strategies;
•	evaluate structural coverage of external molecular datasets.
The L1 groups are validation constructs. They should not be interpreted as experimentally established chemical classes.
Important methodological boundary
L1 must remain independent of the modeled endpoint.
The following information must not be used to construct, merge, split, or select L1 groups:
•	target-property values;
•	descriptor rankings derived from the target;
•	model residuals;
•	cross-validation results;
•	predictive-performance metrics.
Choosing or modifying an L1 partition after examining downstream model performance would introduce endpoint-dependent validation bias.
Data reuse and licensing
The curated workbook contains molecular structures and identifiers assembled from public chemical sources. Users should respect the terms of use and attribution requirements of the original databases.
Repository code and derived files should be reused according to the license supplied with this project.
Citation
When using the curated molecular universe, L1 implementation, or generated group assignments, please cite the associated manuscript:
P. Cysewski,
Chemically Informed and Leakage-Aware QSPR Modeling: The 4L-QSPR Framework Applied to Aqueous Solubility Prediction
(submitted). Full bibliographic details will be added after publication.

Contact
Piotr Cysewski
Department of Physical Chemistry
Faculty of Pharmacy, Collegium Medicum in Bydgoszcz
Nicolaus Copernicus University in Toruń
Bydgoszcz, Poland
Email: piotr.cysewski@cm.umk.pl


