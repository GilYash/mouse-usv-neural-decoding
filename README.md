# Predicting Mouse Ultrasonic Vocalization Type from Pre-Call Neural Population Activity

Neural Data Science final project investigating whether neural population activity **before** a mouse ultrasonic vocalization (USV) contains information about the acoustic type of the upcoming call.

**Authors:** Itay Oucherenko, Elad Joseph, Gil Yashayev  
**GitHub:** `@ItayOucherenko`, `@EladJoseph`, `@GilYash`  
**Course:** Neural Data Science (00970405)  
**Institution:** Faculty of Data and Decision Sciences, Technion – Israel Institute of Technology

## Technical highlights

This project demonstrates an end-to-end **machine learning, statistical inference, and scientific data analysis** workflow on multimodal neural and acoustic recordings.

**Core tools:** Python, NumPy, pandas, SciPy, scikit-learn, Matplotlib, Joblib, UMAP

**Methods and capabilities:**

- **Signal processing:** high-frequency audio spectrograms, session-specific background estimation, frequency-ridge tracking, acoustic feature extraction, and neural spike-count processing
- **Unsupervised learning:** K-Means and Gaussian Mixture Models (GMMs), cluster validation, stability analysis, and feature ablation
- **Dimensionality reduction & visualization:** PCA, t-SNE, and UMAP in 2-D/3-D
- **Supervised machine learning:** logistic regression, random forest, and RBF-SVM classification
- **Robust model evaluation:** nested cross-validation, temporally blocked folds, bout-aware grouping, and leakage diagnostics
- **Statistical inference:** circular-shift permutation/null testing, Mann-Whitney tests, effect sizes, Fisher/Stouffer evidence combination, and Benjamini-Hochberg FDR correction
- **Reproducible research:** fixed random seeds, cached expensive computations, explicit data validation, robustness analyses, and a complete scientific report

## Overview

The dataset does not contain ground-truth syllable labels, so the analysis is split into two stages:

1. **Acoustic target construction** — raw USV recordings are aligned to neural timestamps, cleaned using session-specific background estimates, filtered by acoustic quality, represented with interpretable acoustic features, and clustered into data-driven call categories.
2. **Neural decoding** — population spike-count activity before each call is used to predict the upcoming acoustic category with temporally blocked, bout-grouped cross-validation and circular-shift null testing.

The final report and notebook contain the complete analysis, robustness checks, positive controls, and supplementary diagnostics.

## Main findings

- The analysis uses **16 female-encounter sessions** containing **3,984 detected vocalizations**; **2,692 calls** remain after acoustic quality control.
- K-Means identifies a stable **K = 3** acoustic partition using duration, FM intercept, and FM slope.
- The selected K = 3 solution has **Silhouette = 0.394** and **mean subsampling ARI = 0.98**.
- Only **1 of 21** session-by-K pre-call decoding tests survives BH-FDR correction: **M7_F2 at K = 3**, with Macro-F1 = **0.392**, p = **0.002**, q = **0.042**.
- The dataset therefore supports **limited, session-dependent evidence**, not a dataset-wide conclusion that upcoming call type is reliably decodable from pre-call population activity.
- A pooled **calling-vs-quiet** positive control is significant after correction (**q = 0.0047**), showing that the same neural-analysis pipeline can detect broader vocalization-related activity.

## Authors and collaborators

- **Gil Yashayev** — `@GilYash` (repository owner)
- **Elad Joseph** — `@EladJoseph`
- **Itay Oucherenko** — `@ItayOucherenko`

All three authors contributed to the Neural Data Science final project.

## Repository structure

```text
mouse-usv-neural-decoding/
├── README.md
├── requirements.txt
├── CITATION.cff
├── AUTHORS.md
├── .gitignore
├── notebooks/
│   └── usv_neural_decoding.ipynb
├── report/
│   └── final_report.pdf
├── data/
│   └── README.md
└── vocal_data/
    └── README.md
```

The experimental `.mat` and `.wav` files are **not included** in this repository.

## Analysis pipeline

The notebook is organized as a complete end-to-end workflow:

1. Setup and configuration
2. Session loading and inventory
3. Audio/neural clock alignment
4. Bout segmentation
5. Session-specific background estimation
6. Acoustic feature extraction and quality control
7. Acoustic feature-set selection
8. K-Means/GMM comparison and K selection
9. Acoustic cluster characterization
10. Neural design-matrix construction
11. Nested model selection and grouped temporal cross-validation
12. Positive controls
13. Circular-shift significance testing and robustness analyses
14. Summary and limitations
15. Additional ablations and supplementary checks

## Setup

The notebook was developed with Python 3.11.

```bash
python -m venv .venv
source .venv/bin/activate        # Linux/macOS
# .venv\Scripts\activate       # Windows
pip install -r requirements.txt
```

Then start Jupyter:

```bash
jupyter lab
```

and open:

```text
notebooks/usv_neural_decoding.ipynb
```

## Required data layout

Place the neural/session `.mat` files under:

```text
data/
```

and the raw audio recordings under:

```text
vocal_data/
```

The notebook contains an explicit session-to-file mapping and checks that every required pair is present before analysis begins. See [`data/README.md`](data/README.md) and [`vocal_data/README.md`](vocal_data/README.md) for the expected filenames.

## Reproducing the analysis

Run the notebook from top to bottom. It automatically creates:

```text
outputs/
├── cache/
└── figures/
```

Two expensive stages are cached: acoustic feature extraction and permutation/null analyses. In the configuration cell:

```python
RECOMPUTE_FEATURES = True
RECOMPUTE_ANALYSIS = True
```

forces a clean recomputation. After a successful run, these flags can be changed to `False` to reuse cached intermediate results.

The main analysis uses a fixed random seed (`RANDOM_SEED = 0`) for reproducibility.

## Statistical design

Several choices in the notebook are specifically intended to reduce optimistic bias:

- Calls from the same bout are kept in the same cross-validation fold.
- Folds are blocked in time rather than randomly shuffled.
- Standardization, PCA, and hyperparameter selection are performed using training data only.
- Logistic regression, random forest, and RBF-SVM are compared in nested cross-validation.
- Significance is assessed using circular shifts of the temporally ordered labels rather than unrestricted label permutations.
- Families of tests are corrected using Benjamini-Hochberg FDR.

## Data availability

The raw experimental recordings are not redistributed in this repository. The notebook expects session `.mat` files containing neural/session metadata and matching high-sampling-rate `.wav` recordings. Access to those files should follow the original dataset/lab permissions.

## Report

The complete scientific report, including methods, results, figures, references, robustness analyses, and supplementary material, is available at:

[`report/final_report.pdf`](report/final_report.pdf)

## Citation

If you use or reference this project, please cite the project authors listed in [`CITATION.cff`](CITATION.cff).

## License

No open-source license is included by default because reuse permissions for the project code and underlying research materials have not been specified. Add an appropriate license before public redistribution if desired.
