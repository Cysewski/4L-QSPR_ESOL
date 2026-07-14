Optimized conformers and COSMO files for the curated ESOL dataset
## Molecular dataset
This directory contains the optimized conformer structures, corresponding COSMO files, and conformer-energy information generated for all **1,111 unique molecular identities** included in the curated ESOL dataset used in the 4L-QSPR study.
The files are provided to support transparency, reproducibility, reuse of the quantum-chemical calculations, and independent generation of COSMO-RS-derived molecular descriptors.

## Contents
For each ESOL molecule, the deposited files include:
- optimized three-dimensional conformer structures;
- TURBOMOLE/COSMO files for the retained conformers;
- conformer-energy information used for conformer ranking and subsequent COSMO-RS calculations;
- molecular identifiers allowing the files to be linked to the curated ESOL dataset via InChIKey.
- a molecule may be represented by up to 10 conformers. 

The calculations correspond to the final curated ESOL dataset containing 1,111 unique molecular identities.
The curation procedure included:
•	molecular-identity verification; 
•	deduplication at the InChIKey level; 
•	removal of one unresolved molecular structure; 
•	retention of distinct stereochemical identities represented by distinct stereochemical InChIKeys. 
The mapping between molecular identifiers, SMILES, InChIKeys, ESOL records, and deposited conformer directories is provided elsewhere in this repository.
## Computational protocol
Conformational analysis was performed using COSMOconf 2024, with TURBOMOLE as the quantum-chemical engine.
The computational protocol was consistent with the COSMOtherm parameterization:
BP_TZVPD_FINE_24.ctd / BP_TZVP_24.ctd
The quantum-chemical workflow can be summarized as:
ridft-BP,fine/def2-TZVPD//ridft-BP/def-TZVP
Molecular geometries were first optimized using the BP density functional with the def-TZVP basis set. Single-point COSMO calculations were subsequently performed using the fine cavity setting and the def2-TZVPD basis set.
The resulting conformer ensembles and COSMO files were used for the calculation of COSMO-RS-derived descriptors employed in the combined COSMO–PaDEL representation described in the manuscript.
## Intended use
The deposited files may be used to:
•	reproduce the COSMO-RS-derived descriptor calculations; 
•	inspect the conformational ensembles used in the study; 
•	derive additional COSMO-RS or quantum-chemical descriptors; 
•	compare alternative conformer-weighting procedures; 
•	support future aqueous-solubility and molecular-property modeling studies. 
The original experimental logS values were not used during conformer generation, geometry optimization, COSMO calculation, or conformer-energy evaluation.
Software requirements
Reading or reprocessing some of the deposited files may require licensed installations of:
•	COSMOconf; 
•	COSMOtherm; 
•	TURBOMOLE. 
Proprietary executable files and COSMOtherm parameter files are not redistributed in this repository. Users should obtain the appropriate software and parameterization directly from the respective developers or authorized distributors.
## Reproducibility note
The files deposited here represent the conformers and quantum-chemical results used in the final 4L-QSPR ESOL workflow. They should be used together with the curated molecular table and molecular-identifier mapping supplied in this repository.
When reprocessing the files, differences in software version, parameter files, numerical settings, conformer-selection criteria, or conformer-weighting procedures may lead to differences in the resulting COSMO-RS descriptors.
## Citation
When using these files, please cite the associated manuscript:
P. Cysewski, Chemically Informed and Leakage-Aware QSPR Modeling: The 4L-QSPR Framework Applied to Aqueous Solubility Prediction.
Full bibliographic information will be added after publication.
## Contact
Piotr Cysewski
Department of Physical Chemistry
Faculty of Pharmacy, Collegium Medicum in Bydgoszcz
Nicolaus Copernicus University in Toruń
Email: piotr.cysewski@cm.umk.pl

