# Predicting Mouse Ultrasonic Vocalization Type from Pre-Call Neural Population Activity

An end-to-end **Neural Data Science** project combining acoustic signal processing, unsupervised learning, dimensionality reduction, supervised classification, cross-validation, and statistical inference to study whether neural population activity before a mouse ultrasonic vocalization (USV) contains information about the acoustic type of the upcoming call.

**Authors:** Itay Oucherenko, Elad Joseph, Gil Yashayev  
**Course:** Neural Data Science (00970405)  
**Institution:** Faculty of Data and Decision Sciences, Technion – Israel Institute of Technology

## Quick links

- [Analysis notebook](notebooks/Neural%20Data%20Science%20Final%20Project%20Code.ipynb)
- [Final report](report/Neural%20Data%20Science%20Final%20Project%20Report.pdf)
- [Neural/session data information](data/README.md)
- [USV audio data information](vocal_data/README.md)

## Project overview

The project asks a two-stage question:

1. **Can mouse vocalizations be organized into stable, data-driven acoustic categories?**
2. **Can pre-call neural population activity predict which acoustic category will occur next?**

Because the dataset does not contain manual ground-truth syllable labels, call types are first constructed from the audio. Raw USV recordings are aligned to neural timestamps, processed with session-specific background estimation and contour tracking, filtered by acoustic quality, and represented using interpretable acoustic features. K-Means and Gaussian Mixture Models are then evaluated to define acoustic categories.

The resulting labels are used as targets for neural decoding. Population spike-count activity in the second preceding each call is evaluated using temporally blocked, bout-grouped cross-validation and circular-shift null testing designed to reduce temporal leakage and optimistic bias.

## Technical highlights

**Core stack:** Python, NumPy, pandas, SciPy, scikit-learn, Matplotlib, Joblib, UMAP

**Methods:**

- **Signal processing:** high-frequency spectrograms, session-specific background estimation, frequency-ridge tracking, acoustic feature extraction, neural spike-count processing
- **Unsupervised learning:** K-Means, Gaussian Mixture Models, cluster validation, stability analysis, feature ablation
- **Dimensionality reduction:** PCA, t-SNE, UMAP in 2-D and 3-D
- **Supervised learning:** logistic regression, random forest, RBF-SVM
- **Model evaluation:** nested cross-validation, temporally blocked folds, bout-aware grouping, leakage diagnostics
- **Statistical inference:** circular-shift null testing, Mann-Whitney tests, effect sizes, Fisher/Stouffer evidence combination, Benjamini-Hochberg FDR correction
- **Reproducibility:** fixed random seeds, explicit data validation, cached expensive computations, robustness analyses, and a complete scientific report

## Main findings

- **16 female-encounter sessions** contained **3,984 detected vocalizations**; **2,692 calls** remained after acoustic quality control.
- K-Means identified a stable **K = 3** acoustic partition using duration, FM intercept, and FM slope.
- The selected partition achieved **Silhouette = 0.394** and **mean 80% subsampling ARI = 0.98**.
- Only **1 of 21** session-by-K pre-call decoding tests survived BH-FDR correction: **M7_F2 at K = 3**, with Macro-F1 = **0.392**, p = **0.002**, q = **0.042**.
- The overall result therefore supports **limited, session-dependent evidence**, rather than a dataset-wide conclusion that upcoming call type is reliably decodable from pre-call population activity.
- A pooled **calling-vs-quiet** control was significant after correction (**q = 0.0047**), showing that the same neural-analysis pipeline can detect broader vocalization-related activity.

## Data provenance and availability

The neural/session recordings and raw USV audio used in this project were **provided by Dr. Shai Netser from the Lab for Neurobiology of Social Behavior, Sagol Department of Neurobiology, University of Haifa**.

The original `.mat` and `.wav` research files are **not included in this repository and are not redistributed by the project authors**. They remain subject to the permissions of the originating research group and data owners. This repository therefore contains the **analysis code, documentation, and final report only**.

Full end-to-end reproduction requires authorized access to the original research data. See [`data/README.md`](data/README.md) and [`vocal_data/README.md`](vocal_data/README.md) for the expected local directory structure.

Lab website: https://shlomowagner-lab.haifa.ac.il/

## Analysis pipeline

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

## Repository structure

```text
mouse-usv-neural-decoding/
├── README.md
├── requirements.txt
├── CITATION.cff
├── AUTHORS.md
├── .gitignore
├── notebooks/
│   └── Neural Data Science Final Project Code.ipynb
├── report/
│   └── Neural Data Science Final Project Report.pdf
├── data/
│   └── README.md
└── vocal_data/
    └── README.md
```

The `data/` and `vocal_data/` directories contain documentation only; the underlying research recordings are intentionally excluded.

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
notebooks/Neural Data Science Final Project Code.ipynb
```

## Reproducing the analysis

If you have authorized access to the source data, place the neural/session `.mat` files under `data/` and the matching raw `.wav` recordings under `vocal_data/`. The notebook contains an explicit session-to-file mapping and validates the required inputs before analysis begins.

Running the notebook from top to bottom creates:

```text
outputs/
├── cache/
└── figures/
```

Two computationally expensive stages are cached: acoustic feature extraction and permutation/null analyses. In the configuration cell:

```python
RECOMPUTE_FEATURES = True
RECOMPUTE_ANALYSIS = True
```

forces a clean recomputation. After a successful run, these flags can be changed to `False` to reuse cached intermediate results.

The main analysis uses a fixed random seed (`RANDOM_SEED = 0`) for reproducibility.

## Statistical design

Several choices are specifically intended to reduce optimistic bias:

- Calls from the same bout remain in the same cross-validation fold.
- Folds are blocked in time rather than randomly shuffled.
- Standardization, PCA, and hyperparameter selection are fitted using training data only.
- Logistic regression, random forest, and RBF-SVM are compared with nested cross-validation.
- Significance is assessed using circular shifts of temporally ordered labels rather than unrestricted label permutations.
- Families of tests are corrected using Benjamini-Hochberg FDR.

## Authors and collaborators

- **Gil Yashayev** — [`@GilYash`](https://github.com/GilYash) (repository owner)
- **Elad Joseph** — [`@EladJoseph`](https://github.com/EladJoseph)
- **Itay Oucherenko** — [`@ItayOucherenko`](https://github.com/ItayOucherenko)

All three authors contributed to the Neural Data Science final project.

## Citation

If you use or reference this project, please cite the project authors listed in [`CITATION.cff`](CITATION.cff).

## License

No open-source license is included by default because reuse permissions for the project code and underlying research materials have not been specified.