# MS Lesion Detection from Brain MRI

## Abstract
This project explores AI-based multiple sclerosis (MS) detection from brain MRI using a research notebook that audits preprocessing, patient identity, and model behavior in a real multi-source clinical dataset. The workflow preserves the original investigation and keeps the private imaging data local-only. The repository is organized for GitHub publication without exposing any private dataset content or hard-coded Kaggle paths.

## Problem statement
Multiple sclerosis diagnosis from MRI is clinically important but can be confounded by acquisition-site differences, preprocessing artifacts, and metadata embedded in the image pixels. In this project, the notebook investigates whether a model is learning disease-related signal or shortcut patterns created by site-specific acquisition and export conditions.

## Objectives
- Build a reproducible MRI analysis workflow for MS vs. normal brain images.
- Audit class confounding and shortcut behavior before trusting classification metrics.
- Preserve the original experimental methodology while making the project portable to local machines.
- Prepare the repository for publication without publishing the private dataset.

## Dataset description
This project uses a private MRI dataset stored locally. The repository intentionally does not include the raw imaging data. The expected local directory structure is:

- `MS/`
  - `flair axial/`
  - `flair sagital/`
  - `normal axial/`
  - `normal sagital/`

The dataset is organized by acquisition type and orientation, with each class folder containing image files in the original exported naming convention. The code expects the dataset root to be set via the environment variable `MS_DATA_ROOT` or placed directly in `./MS`.

## Methodology
The notebook follows a staged research workflow:
1. Inventory the MRI files recursively and verify folder structure.
2. Audit preprocessing and burned-in metadata.
3. Use OCR-derived patient identity instead of filename-based assumptions.
4. Apply brain crop and masking procedures to reduce non-disease artifacts.
5. Build interpretable radiomics/biomarker and deep-learning models.
6. Evaluate via patient-aware validation and report shortcut-audit results honestly.

## Preprocessing
The notebook includes several preprocessing stages designed to reduce non-anatomical shortcuts while preserving the original research workflow:
- Recursive file scanning and dataset inventory
- Burned-in text masking and OCR-based patient identity extraction
- Brain localization and crop operations
- Corner masking strategies
- Contrast enhancement and image normalization where used by the experimental pipeline

The repository keeps the original preprocessing logic intact and only adapts the dataset path configuration for local execution.

## Models used
The codebase contains a combination of classical machine-learning and deep-learning components, including:
- Logistic regression baselines
- Random forest classifiers
- Scikit-learn pipelines and patient-grouped validation
- PyTorch-based image classification models
- ResNet-style architecture and EfficientNet-style backbone usage where applicable
- Grad-CAM / explainability visualization when included in the notebook workflow

## Training / validation approach
The project uses a patient-aware validation strategy and preserves the notebook's original evaluation framework. The code is structured to avoid leakage from duplicated or near-duplicate scans, and it evaluates performance under patient-aware splits rather than naive random image splitting.

The notebook explicitly reports that the central scientific finding is data confounding, not only model performance. This is important for correct interpretation of the reported numbers.

## Evaluation metrics
The notebook uses common classification metrics relevant to the experimental pipeline, including:
- Accuracy
- ROC-AUC
- Precision, recall, and F1-score
- Patient-grouped validation performance
- Shortcut-audit comparison under preprocessing changes

No numerical claims should be interpreted apart from the results preserved in the notebook itself.

## Results
The notebook preserves the original project results and reports the analysis in a transparent, audit-first manner. The project is not framed as a purely performance-optimized classifier benchmark; instead, it highlights the detection of strong acquisition-site confounding and the implications for model interpretation.

The documented findings include:
- Burned-in hospital metadata is strongly tied to the class label.
- The full dataset is confounded by acquisition site and image export conditions.
- Preprocessing changes reduce some shortcut signal, but do not eliminate the underlying confound.
- The notebook’s honest interpretation is that model performance must be read in the context of this confounding rather than treated as a clean disease-detection benchmark.

## Explainability / Grad-CAM
If the notebook includes Grad-CAM or saliency-based explanation steps, those results are preserved in the historical notebook workflow. This project is meant to retain the original interpretability analysis rather than replace it with a new architecture or different set of experiments.

## Limitations
- The private dataset is not included in this repository and cannot be uploaded.
- The dataset is subject to site-level confounding and acquisition bias.
- Model accuracy must be interpreted carefully because the notebook shows that the label is strongly associated with acquisition-site conditions.
- Deeper model runs and full training require a local GPU-capable environment and the private dataset.

## Future work
- Acquire a more balanced multi-site dataset with both classes present across sites.
- Reduce site confounding in data collection and annotation procedures.
- Re-evaluate the model on an external, domain-balanced cohort.
- Expand the explainability analysis and publication-ready reporting.

## Installation
Clone the repository and create a local Python environment:

```bash
git clone <your-repo-url>
cd MS-detection
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Dataset setup
Place the private dataset locally in one of the supported locations:

```bash
export MS_DATA_ROOT="/absolute/path/to/private/MS"
```

or place it directly under the repository root as:

```text
./MS/
├── flair axial/
├── flair sagital/
├── normal axial/
├── normal sagital/
```

Do not commit or upload this folder. It must remain local-only.

## How to run the project
1. Set `MS_DATA_ROOT` to the local dataset path, or ensure `./MS` exists.
2. Open the notebook in Jupyter or VS Code.
3. Run the cells in order.
4. Keep the private dataset outside the repository and ensure it is not added to version control.

```bash
export MS_DATA_ROOT="/absolute/path/to/private/MS"
jupyter notebook winc26.ipynb
```

## Repository structure

```text
MS-detection/
├── .gitignore
├── README.md
├── requirements.txt
├── winc26.ipynb
├── MS/                    # private local dataset; ignored by git
│   ├── flair axial/
│   ├── flair sagital/
│   ├── normal axial/
│   └── normal sagital/
└── .venv/                 # local virtual environment; ignored by git
```

## Technologies / libraries
- Python 3
- Jupyter Notebook
- NumPy
- pandas
- OpenCV
- scikit-learn
- PyTorch
- torchvision
- Matplotlib
- Pillow
- SciPy
- pytesseract

## References
The notebook itself contains the relevant methodological references and discussion of the site confounding and shortcut-learning findings. The README preserves that framing rather than replacing it with a generic performance summary.

For additional context, the project draws on MRI short-cut learning literature and confounding-aware clinical imaging evaluation practices, as noted in the notebook.

## Notes for publication
This repository is prepared for GitHub publication in a way that preserves the original scientific workflow and private data boundaries. The notebook remains the primary research artifact, while the data stay local-only and the repository is cleaned for reproducible local execution.
