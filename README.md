# MoA Prediction with Multimodal Data

## Overview

This project investigates the prediction of mechanisms of action (MoAs) for chemical compounds by integrating multimodal data:
- Morphological profiles from Cell Painting (high-content imaging)
- Chemical descriptors and binary flags (e.g. drug approval, PAINS)
- Molecular fingerprints (Morgan/ECFP from SMILES)
- SMILES-derived embeddings via contrastive graph learning (MolCLR)

The objective is to build a robust, interpretable pipeline capable of generalizing to rare classes, leveraging both classical machine learning and modern molecular representation learning techniques.

## Data Sources
- Cell Painting data from 5 different datasets (e.g. FMP, MEDINA, etc.)
- Probes & Drugs (ECBD) database, including:
	•	MoA labels (single- and multi-label)
	•	Compound-level metadata and flags
	•	Target annotations
	•	SMILES strings and activity values

Careful preprocessing was applied to handle overlapping compounds, normalization, label imbalance, and missing values.

## Methods & Pipeline

- **MultimodalMoAPipeline** — modular training workflow with:
	•	Feature scaling and modality selection
	•	Stratified train/test splitting
	•	GridSearchCV and nested CV
	•	Support for various classifiers (CatBoost, GBDT, MLP, etc.)
	•	Visualization of PR/ROC curves
	•	Grouped feature importances by modality

- **Aggregation strategies** for replicates:
	•	Arithmetic mean (baseline)
	•	Geometric mean (sign-adjusted)
	•	Arithmetic–geometric mean (AGM)
	•	Closest real compound to each aggregate

- **SMILES representations:**
	•	Traditional: Morgan fingerprints (ECFP)
	•	Learned: MolCLR graph-based embeddings

## Results Summary

## Key Findings

- **Morphology remains the dominant modality** in terms of feature importance, though it was not the focus of this comparison.
- **SMILES embeddings from MolCLR outperform Morgan fingerprints** on rare-class generalization. Despite similar accuracy, MolCLR achieves higher macro F1, indicating better sensitivity to underrepresented MoAs.
- **AGM-based aggregation** slightly outperforms arithmetic and geometric means in macro F1 score, while preserving accuracy.
- **Using the closest real replicate** instead of synthetic aggregation generally leads to lower macro F1 and weaker detection of minority classes.
- **CatBoostClassifier with balanced class weights** demonstrates the most stable performance across all classes and is recommended as a baseline model for further development.
- **Class imbalance remains a major bottleneck**, with `moa_inhibitor` dominating performance. Minor classes like `moa_agonist` and `moa_antagonist` remain challenging to predict.

## Next Steps

- Further improve MolCLR embeddings via fine-tuning or contrastive pretraining on MoA-specific compounds.
- Integrate attention-based or learned aggregation methods for better replicate fusion.
- Package the pipeline into a reproducible Python module for downstream tasks.
- Expand to multi-label targets and integrate biochemical activity predictions.
- Apply interpretability tools such as SHAP and LIME to investigate compound-specific decisions.

## Citation & Acknowledgments
- [Cell Painting Images](https://cellpainting-gallery.s3.amazonaws.com/index.html#cpg0036-EU-OS-bioactives/)
- [Morphological Profiling Datase of Cell Painting Images](https://zenodo.org/records/13309566)
- [Process and analyse aggregated Cell Painting data](https://github.com/schmiedc/EU-OS_bioactives)
- [ECDB data](https://ecbd.eu/download)
- [Probes & Drugs data](https://www.probes-drugs.org/download)
- [MolCLR](https://github.com/yuyangw/MolCLR)
