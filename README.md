In Silico Morphological Phenotyping of Hippocampal Regions

Proof-of-concept pipeline for in-silico mouse morphological phenotyping.
This repository demonstrates a minimal, reproducible workflow to load public histology images, preprocess them, extract interpretable features, train a classifier, and produce simple evaluation figures. It is intended as a learning and portfolio project for computational image analysis applied to morphological phenotyping.

Summary

This project implements an end-to-end teaching pipeline that shows the core steps used in bioimage analysis:

organize image data by biological class (here: hippocampal subregions)

image preprocessing (resize, optional normalization / crop)

feature extraction (baseline: flattened pixels; recommended: texture features such as GLCM)

classical machine learning classification (Random Forest baseline)

evaluation (confusion matrix, classification report, simple visualizations)

The image sources used for examples are public atlas images (for example from the Allen Mouse Brain Atlas). Please see DATA below for provenance and licensing notes.

Author / Contact

Developed by Shahriyar Tonmoy — email: tonmoy.pharma.ju@gmail.com

Project repository (this repo): in_silico_morphological_phenotyping on GitHub.

Intended audience & purpose

Students learning computational phenotyping and bioimage pipelines

Researchers who want a reproducible, minimal example to adapt for teaching or further development

Applicants (e.g., to MorphoPHEN) who want a demonstrable project showing both wet-lab understanding and computational practice

If you are preparing an application to the MorphoPHEN Erasmus Mundus, this repo provides a small practical demonstration of relevant skills (data handling, reproducible code, and basic image-based machine learning).

Repository structure
in_silico_morphological_phenotyping/
│
├── data/                 # small example images (organized by region)
│   ├── CA1/
│   ├── CA3/
│   └── Dentate_Gyrus/
├── notebooks/
│   └── 01_pipeline_demo.ipynb   # runnable pipeline and figures
├── src/
│   ├── preprocess.py    # helper functions for resizing/cropping/normalizing
│   ├── features.py      # feature extraction helpers (GLCM etc.)
│   └── classify.py      # training / evaluation utilities
├── environment.yml      # conda environment spec
├── README.md
└── LICENSE
Quickstart — run locally

Clone the repository (replace the URL with your fork if needed):

git clone https://github.com/YOUR_USERNAME/in_silico_morphological_phenotyping.git
cd in_silico_morphological_phenotyping

Create and activate the conda environment:

conda env create -f environment.yml
conda activate in_silico_morphological_phenotyping

Launch the demo notebook:

jupyter notebook notebooks/01_pipeline_demo.ipynb
# or
jupyter lab

In the notebook, run cells in order (Load → Preprocess → Features → Train → Evaluate).
Tip: use Kernel → Restart & Run All to ensure a clean run.

Example commands (core snippets)

Load images (conceptual):

import os, cv2
images, labels = [], []
for label in os.listdir('data'):
    for fname in os.listdir(os.path.join('data', label)):
        img = cv2.imread(os.path.join('data', label, fname))
        images.append(img); labels.append(label)

Compute GLCM texture features (conceptual):

from skimage.feature import graycomatrix, graycoprops
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
glcm = graycomatrix(gray, distances=[1], angles=[0], levels=256, symmetric=True, normed=True)
contrast = graycoprops(glcm, 'contrast')[0,0]

Train a RandomForest:

from sklearn.ensemble import RandomForestClassifier
clf = RandomForestClassifier()
clf.fit(X_train, y_train)
print(clf.score(X_test, y_test))
Data & provenance

Example images used in this project were taken from public atlas resources (for example, the Allen Mouse Brain Atlas). When using third-party images in any public or publication context, document exact experiment IDs, probe/orientation (e.g., coronal), and cite the original source.

If you reuse these files beyond a demo, check the original dataset license and provide attribution. The README and a DATA_LICENSES.md file should be extended with exact dataset IDs when the repo grows.

Important: This repo contains only a small, demonstrative subset of images to keep the example lightweight. For full research, collect larger datasets and follow ethical and licensing rules.

(Reference data source example: Allen Institute for Brain Science.)

Results & visualizations

The demo notebook produces a few example outputs:

grid of sample images per class

intensity histograms per region

PCA visualization of chosen features

confusion matrix and classification report (precision/recall/F1)
