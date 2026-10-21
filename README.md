# Interpretable Machine Learning for Photovoltaic Defect Screening

Reproducible code for the peer-reviewed article:

> Hoque, K. (2026). Interpretable Machine Learning for Photovoltaic Defect Screening: A Reproducible Electroluminescence Benchmark. *International Journal of Science and Research Archive*, 20(2), 371–378. https://doi.org/10.30574/ijsra.2026.20.2.1601

## What it does
The code screens solar-cell electroluminescence (EL) images for defects with transparent models, so a reviewer can see why a cell was flagged.
- **Data:** the public ELPV dataset, 2,624 cell images from 44 modules.
- **Features:** 62 interpretable intensity and gradient features, 324 histogram-of-oriented-gradient (HOG) features, and the 386 combined.
- **Evaluation:** class-balanced logistic models under stratified five-fold cross-validation (seed 42).
- **Targets:** any annotated defect, and high-confidence defect.

| Task | Features | Accuracy | F1 | ROC-AUC | PR-AUC |
|---|---|---:|---:|---:|---:|
| Any annotated defect | Combined | 0.721 | 0.670 | 0.778 | 0.751 |
| High-confidence defect | Interpretable | 0.753 | 0.638 | 0.816 | 0.732 |

Performance differs by cell technology: out-of-fold F1 is **0.712 for monocrystalline** and **0.639 for polycrystalline** cells. Tools used across a mixed solar fleet need to account for this gap.

## Reproduce
```bash
pip install -r requirements.txt
git clone https://github.com/zae-bayern/elpv-dataset
export ELPV_DATA=elpv-dataset/src/elpv_dataset/data   # Windows: set ELPV_DATA=...
python src/run_pv_experiment.py
```
Outputs go to `results/run/`. Two folders hold reference copies of the outputs:
- `results/published/` holds the values reported in the article.
- `results/reproduction_2026-10-15/` holds an independent rerun.

The rerun matched the published summary exactly: the maximum absolute difference across all reported metrics was 0.0.

## Limits (stated in the article)
- **Screening only.** This is a cell-level screening benchmark, not a safety, certification, or energy-yield decision.
- **Cell-level splits.** Public ELPV labels do not include module IDs, so the folds are not module-independent.
- **Labels are not power loss.** Annotation probabilities are expert labels, not measured electrical loss.
- **Next steps** are module-grouped and cross-dataset validation, and correlation with calibrated current-voltage measurements. These are the goals of the ongoing project.

## Data license
ELPV images are licensed CC BY-NC-SA 4.0 by their authors and are **not** redistributed here. Download them from the official repository.

## Citation
See `CITATION.cff`. Author: Khandoker Samiul Hoque (ORCID 0009-0008-0756-2395). Code license: MIT.
