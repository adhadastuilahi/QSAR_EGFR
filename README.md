# EGFR Inhibitor Discovery using Machine Learning and Virtual Screening

This repository contains the KNIME workflow and related resources for a computational pipeline aimed at discovering novel EGFR (Epidermal Growth Factor Receptor) inhibitors. The study employs a comprehensive virtual screening approach through the KNIME Analytics Platform.

## 🧪 Objective

We aimed to identify novel EGFR inhibitors through a comprehensive computational approach. Virtual screening was performed using the KNIME application.

## 🚀 Workflow Overview

This study involves the development of QSAR models using machine learning in KNIME. The workflow consists of six key stages:

### 🧷 W1: Data Set Preparation

The data used in this study was obtained from the **ChEMBL** database (CHEMBL203, corresponding to EGFR/erbB1). Data were selected based on Single Protein Assay format focusing on *Homo sapiens*, and cleaned by:
- Removing missing IC50 values
- Eliminating duplicate ChEMBL IDs and SMILES
- Selecting IC50 from the most recent year
- Removing salts using **RDKit Salt Stripper**

#### Compound Standardization and Labeling
- IC50 values were converted to pIC50: `pIC50 = -log(IC50 * 10^-9)`
- Compounds were labeled as:
  - **Inactive**: pIC50 < 5
  - **Semi-active**: 5 ≤ pIC50 ≤ 7.5
  - **Active**: pIC50 > 7.5
- SMOTE oversampling (k=10, rate=2) was applied to balance the minority class.

### 🧬 W2: Calculation of Descriptors and Fingerprints

Molecular fingerprints were calculated using **RDKit** and **CDK** in KNIME, including:
- Atom Pair, Morgan (ECFP), FeatMorgan (FCFP), Torsion
- RDKit, MACCS, Pattern, Avalon, Standard, Extended, Estate, PubChem
- A **combined fingerprint set**

**Feature selection** was applied using **Tree Ensemble** to reduce overfitting and enhance model quality.

### 🤖 W3: Machine Learning Optimization

ML model hyperparameters were optimized using **Bayesian optimization** (100 iterations):
- Warm-up: 50–75 rounds
- 50 candidates per round

ML models used:
- **Classification**: XGBoost (XGB), MLP, Random Forest (RF), Gradient Boosted Trees (GBT), Support Vector Machine (SVM)
- **Regression**: XGB, RF, GBT, SVM, Multiple Linear Regression (MLR)

The goal was to maximize classification accuracy and R² for regression.

### ⚙️ W4: Model Determination

Optimization was performed using:
- **14 fingerprint types**
- **3 data partitions**: 70:30, 80:20, and 90:10 (linear sampling)
- ML algorithms:
  - **Classification**: SVM, XGB, RF, GBT, Decision Tree (DT), MLP
  - **Regression**: RF, XGB, SVR, GBT, MLR

### 📊 W5: Evaluation of Model Performance

Model validation included:
- **Internal (training set)** and **external (test set)** validation
- **Classification QSAR**: Evaluated by external accuracy, top 5 models tested for sensitivity, F-score, and ROC curve
- **Regression QSAR**: Evaluated using R², MAE, and Model Acceptable Criteria

Sub-fingerprint correlation analysis was conducted using the **Linear Correlation** node to assess relationships between substructures and predicted pIC50 values.

### 🧩 W6: Fragment-Based Drug Design (FBDD)

FBDD analysis was conducted using the **MoSS** node:
- Selected compounds: pIC50 > 7.5
- Fragment criteria:
  - Focus support ≥ 10%
  - Complement support ≤ 1%
  - Fragment size: 6–100 atoms

Fragments were then tested using the best QSAR regression model and filtered with **Rule of Three (RO3)**:
- LogP < 3
- MW < 300
- HBD < 3
- TPSA < 60 Å²

## 🛠️ Technology Used

- [KNIME Analytics Platform](https://www.knime.com/)
- RDKit and CDK nodes
- MoSS and SMOTE nodes
- ML algorithms: XGBoost, MLP, RF, GBT, SVM, MLR

## 📥 How to Use

1. Open the **KNIME Analytics Platform**.
2. Click **File > Import KNIME Workflow...**
3. In the window:
   - Click **Browse...** and select the `.knwf` file.
   - Choose destination folder (e.g., **LOCAL**).
4. Click **Finish**.

## 👥 Authors

- Adha Dastu Illahi  
- Gatot Fatwanto Hertono  
- Arry Yanuar  
- Tomoyuki Miyao  
- Rezi Riadhi Syahdi

## 📌 Notes

- This repository does not yet include example results or sample data.
- Future updates may include molecular docking, MD simulation integration, and additional validation studies.
