
##  Project Overview

This repository contains the code, workflow, and documentation for a **mproject** focused on the classification of **Prompt**, **Non-Prompt**, and **Background** heavy-flavor candidates using **Boosted Decision Trees (BDTs)**.

The study is performed in **exclusive transverse momentum (pT) bins** to ensure pT-dependent modeling and to avoid kinematic bias. Only **topological variables** are used for training, while **kinematic variables** are reserved for validation and diagnostic studies.

The project follows best practices commonly adopted in **high-energy physics (HEP)** machine learning analyses.


##  Dataset Description

The analysis uses three Monte Carlo ROOT files:

| File                         | Class                    |
| ---------------------------- | ------------------------ |
| `Prompt_DstarToD0Pi.root`    | Prompt signal            |
| `Nonprompt_DstarToD0Pi.root` | Non-prompt signal        |
| `Bkg_DstarToD0Pi.root`       | Combinatorial background |

Each file contains reconstructed **D* → D⁰π** candidates stored in a common TTree structure.
ROOT files are accessed using **`uproot`**, enabling ROOT-independent analysis.

---

##  Feature Selection

###  Topological Variables (Used for Training)

Only topology-related observables are used as inputs to the BDT:

* `fCpaD0`
* `FCpaXYD0`
* `fDecayLengthXYD0`
* `fImpactParameterProductD0`
* `fImpactSoftPi`
* `fMaxNormalisedDeltaIPD0`

These variables encode decay geometry and impact parameter information and provide strong discrimination power.

---

### Kinematic Variables (Used for Plotting Only)

The following variables are **excluded from training** and used only for validation:

* `fPt`
* `fM`
* `fMD0`
* `fInvDeltaMass`

---

##  pT Binning Strategy

The analysis is performed in exclusive pT bins:

```text
[1, 1.5, 2, 3, 4, 6, 8, 10, 12, 16, 24] GeV/c
```

A **separate BDT model is trained and evaluated in each pT bin**, ensuring pT-differential modeling and preventing the classifier from learning trivial kinematic dependencies.

---

##  Train–Test Split

* **Training fraction:** 70%
* **Testing fraction:** 30%

The split is performed **after pT binning** and training set is oversampled over classes to ensure unbiased evaluation.

---

##  Model Description

* **Algorithm:** Boosted Decision Trees (BDT)
* **Implementation:** XGBoost
* **Task:** Multi-class classification

  * Background (0)
  * Prompt (1)
  * Non-Prompt (2)

BDTs are chosen due to their robustness, interpretability, and widespread validation in heavy-flavor analyses.

---

##  Hyperparameter Tuning

Hyperparameter optimization is performed using a **random search** strategy over a physics-motivated grid, focusing on model stability and avoiding overtraining.

Key parameters include:

* Tree depth
* Learning rate
* Number of estimators
* Event and feature subsampling
* Regularization terms

The final model is selected based on **ROC-AUC performance** and stability between training and test datasets.

---

##  Performance Evaluation

Model performance is evaluated using:

* **BDT score distributions** (per class, per pT bin)
* **ROC curves** (one-vs-rest)
* **Area Under the Curve (AUC)**
* **Correlation matrix** of training variables

All evaluations are performed using the **test dataset only**.

---


##  Requirements

Main Python dependencies:

```text
numpy
pandas
uproot
matplotlib
seaborn
scikit-learn
xgboost
```

---

##  Report

The detailed methodology, physics background, and mid-term results are documented in:

```
report/mid_project_report.pdf
```



---

##  Author

**Ankit Kumar**
M.Sc Physics
25N0292
