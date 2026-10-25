# Network Intrusion Detection with Machine Learning (NSL-KDD)

Project submission for the **IBM SkillsBuild Cybersecurity Bootcamp 2026**, conducted by **BharatCares** (5 to 19 October 2026).

## Overview

This project builds a machine-learning **intrusion detector** that looks at a network connection and decides whether it is **normal traffic or an attack**. It also names the attack family (DoS, Probe, R2L or U2R). It is based on project idea 6, *ML Network Intrusion Detector*, from the bootcamp resources.

The main finding: models score almost perfectly on attacks they have seen (cross-validation F1 0.999 for Random Forest) but much lower on the official test set, which contains attack types that never appear in training (F1 0.754 for the same model). Rare attack families such as R2L and U2R are the hardest to catch.

## Dataset

**NSL-KDD**, an improved version of the KDD Cup 1999 intrusion detection data from the Canadian Institute for Cybersecurity, University of New Brunswick.

* Official page: <https://www.unb.ca/cic/datasets/nsl.html>
* Files used (GitHub mirror): <https://github.com/defcom17/NSL_KDD>
  * `KDDTrain+.txt`: 125,973 labelled connections (training)
  * `KDDTest+.txt`: 22,544 labelled connections (testing, includes 17 attack types unseen in training)

The notebook downloads both files automatically into a `data/` folder the first time it runs, so you do not need to download anything by hand.

## Project description

1. **Load and label:** group the specific attack names into four families (DoS, Probe, R2L, U2R) plus a binary normal/attack label.
2. **Explore:** class balance, protocol and TCP-flag patterns, missing values and duplicates.
3. **Preprocess:** one-hot encode `protocol_type`, `service` and `flag`; standardise numeric features; all inside one scikit-learn `Pipeline` to avoid data leakage.
4. **Model:** compare Logistic Regression, Decision Tree, Random Forest and Hist Gradient Boosting on the official test set.
5. **Analyse:** confusion matrix, ROC curves, detection rate per attack family, feature importance and a precision/recall threshold study.
6. **Multi-class:** a 5-class Random Forest that names the attack family.

### Key results (official test set, `random_state=42`)

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
| --- | --- | --- | --- | --- | --- |
| Decision Tree | 0.810 | 0.966 | 0.691 | 0.806 | 0.830 |
| Hist Gradient Boosting | 0.809 | 0.970 | 0.686 | 0.804 | 0.959 |
| Random Forest | 0.770 | 0.968 | 0.617 | 0.754 | 0.962 |
| Logistic Regression | 0.754 | 0.918 | 0.623 | 0.742 | 0.791 |

Detection rate of the best binary model by attack family: DoS 82.7%, Probe 80.6%, R2L 25.1%, U2R 28.4%, with 3.2% false alarms on normal traffic.

## Technologies used

* Python 3.10 or newer (developed and tested on Python 3.13)
* pandas, NumPy: data handling
* scikit-learn: preprocessing pipelines, models and metrics
* matplotlib, seaborn: visualisation
* Jupyter Notebook

## Project structure

```text
.
├── YourName_NetworkIntrusionDetector.ipynb   # main code (run this)
├── requirements.txt                          # Python dependencies
├── YourName_ProjectReport.docx               # full project report
├── README.md                                 # this file
├── data/                                     # created automatically (downloaded dataset)
└── outputs/                                  # created automatically (figures and result tables)
```

## Setup and run instructions

1. Open a terminal in the project folder.
2. (Recommended) create and activate a virtual environment:
   ```bash
   python -m venv .venv
   source .venv/bin/activate        # Windows: .venv\Scripts\activate
   ```
3. Install the dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. Start Jupyter and open the notebook:
   ```bash
   jupyter notebook YourName_NetworkIntrusionDetector.ipynb
   ```
5. Choose **Run > Run All Cells**. An internet connection is needed on the first run to download the dataset (about 22 MB). The whole notebook runs in roughly one minute on a laptop.

Figures and result tables are saved to `outputs/`.

**Google Colab:** upload the notebook, run all cells, and the data downloads automatically. The required libraries are already installed there.

## Key information

* **Ethics:** only a public, pre-recorded dataset is used. No live network, system or account is scanned or attacked. Never test a system without written authorisation.
* **Limitations:** NSL-KDD is an older simulated dataset with pre-computed connection features, so it does not represent modern encrypted traffic. R2L and U2R attacks are rare in training and mostly missed. Evaluation is offline with a single random seed.
* **Reproducibility:** `random_state=42` is used everywhere; small differences in the last digit can occur with different library versions.

## Author

Name: YourName
IBM SkillsBuild Cybersecurity Bootcamp 2026, BharatCares
