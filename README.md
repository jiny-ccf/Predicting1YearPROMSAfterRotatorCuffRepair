# Patient-Reported Outcomes Calculator for Rotator Cuff Repair

> **An interactive clinical prediction tool for estimating 1-year patient-reported outcomes after primary arthroscopic rotator cuff repair.**

[![R](https://img.shields.io/badge/R-276DC3?logo=r\&logoColor=white)](https://www.r-project.org/)
[![Shiny](https://img.shields.io/badge/Shiny-0088CC?logo=rstudio\&logoColor=white)](https://shiny.posit.co/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Live Demo](https://img.shields.io/badge/Live%20Demo-Risk%20Calculator-orange)](https://riskcalc.org/Predicting1YearPROMSAfterRotatorCuffRepair/)

## Overview

**Patient-Reported Outcomes Calculator for Rotator Cuff Repair** is an interactive R/Shiny implementation of a multivariable prognostic model for estimating **1-year Penn Shoulder Score (PSS)** outcomes following primary arthroscopic rotator cuff repair (RCR).

The application transforms a patient's preoperative demographic, clinical, disease-specific, and surgical characteristics into individualized predictions of:

* **1-year PSS Total**
* **1-year PSS Pain**
* **1-year PSS Function**
* **1-year PSS Satisfaction**

The calculator is publicly deployed through the **Cleveland Clinic Risk Calculator Library**.

### 🚀 Try it live

**[Open the Rotator Cuff Repair Risk Calculator →](https://riskcalc.org/Predicting1YearPROMSAfterRotatorCuffRepair/)**

---

## Why this project?

Outcomes following rotator cuff repair vary considerably between patients.

Population-level averages can describe what happens to a typical patient, but they do not directly answer a more useful question:

> **"Given this patient's current condition and clinical profile, what outcome might we expect one year after surgery?"**

This project translates a multivariable prognostic model into an interactive prediction system that allows clinicians and researchers to explore individualized 1-year patient-reported outcomes.

The application accepts patient-level characteristics, incorporates the patient's baseline PSS, and performs model inference in real time.

---

## ✨ Features

### Individualized prediction

The prediction models incorporate **23 prospectively identified patient, disease, and surgical factors**, including:

* Age
* Sex
* Race
* BMI
* Charlson Comorbidity Index
* Smoking status
* Education
* Area Deprivation Index (ADI)
* Insurance status
* VR-12 Mental Component Score
* Psychiatric diagnosis
* Prior-year opioid use
* Chronic pain
* Prior shoulder surgery
* Rotator cuff tear type
* Tear size
* Repair technique
* Subscapularis status
* Biceps treatment
* Glenohumeral cartilage status
* Acromioplasty
* Acromioclavicular joint status
* Preoperative PSS Total

The underlying study found that **preoperative PSS, VR-12-MCS, insurance status, and race** were consistently among the most important predictors across the 1-year PSS outcomes.

### 🧮 Integrated PSS assessment

If the patient's current PSS Total is already known, it can be entered directly.

If not, the application provides an integrated PSS questionnaire to calculate the current score before prediction. The Shiny interface explicitly supports both workflows.

### ⚡ Real-time model inference

The application converts the user-provided clinical profile into the model's expected feature representation and evaluates the fitted prediction models.

Separate prediction functions are implemented for:

```text
PSS Total
PSS Pain
PSS Function
PSS Satisfaction
```

The prediction functions return estimated outcomes together with model-based uncertainty intervals.

---

## 🧠 Model

The calculator implements the prognostic models developed in:

> **Sahoo S, Imrey PB, Jin Y, et al.**
> *Predictors of patient-reported outcomes after primary arthroscopic rotator cuff repair.*
> **Journal of Shoulder and Elbow Surgery.** 2026;35(1):143–154.
> DOI: [10.1016/j.jse.2025.05.002](https://doi.org/10.1016/j.jse.2025.05.002)

The study analyzed patients undergoing primary arthroscopic rotator cuff repair at Cleveland Clinic between February 2015 and February 2022.

After exclusions, **3,483 patients** with superior-posterior rotator cuff tears and complete preoperative PROM information comprised the analysis cohort. One-year PSS was available for **2,491 patients (72%)**, with a median 1-year PSS of 90.0.

The investigators used:

* **Identity-link beta regression** for PSS Total, Pain, and Function
* **Proportional-odds modeling** for PSS Satisfaction
* Multiple imputation for missing predictor data
* **R²**, Nagelkerke's pseudo-R², and changes in **Akaike Information Criterion (AIC)** to evaluate model performance and predictor importance

The primary PSS-Total model explained approximately **20% of outcome variability**. Sensitivity analyses showed that removing preoperative PSS reduced predictive capacity from 20% to 14%, and removing both preoperative PSS and VR-12-MCS reduced it further to 11%.

> **Important:** This is a statistical prognostic model, not a deep-learning or AI model. The repository contains the deployed implementation of the published prediction models.

---

## 🔬 Prediction Pipeline

The application can be viewed as a simple clinical inference pipeline:

```text
              Patient Profile
                    │
                    ▼
        ┌───────────────────────┐
        │   Feature Processing  │
        │                       │
        │ Demographics          │
        │ Clinical factors      │
        │ Disease characteristics│
        │ Surgical factors      │
        │ Baseline PSS          │
        └───────────┬───────────┘
                    │
                    ▼
        ┌───────────────────────┐
        │   Prognostic Models   │
        │                       │
        │ Beta regression       │
        │ Proportional odds     │
        └───────────┬───────────┘
                    │
                    ▼
        ┌───────────────────────┐
        │ Individual Prediction │
        │                       │
        │ PSS Total             │
        │ PSS Pain              │
        │ PSS Function          │
        │ PSS Satisfaction      │
        └───────────────────────┘
```

This separation between **input processing → model inference → prediction output** mirrors the structure of the deployed Shiny application.

---

## 🏗️ Repository Structure

```text
.
├── global.R       # Model objects and prediction functions
├── server.R       # Reactive application logic and inference
├── ui.R           # Interactive user interface and PSS questionnaire
├── LICENSE
└── README.md
```

### `global.R`

Contains the prediction layer used by the application.

Separate functions generate predictions for:

```r
pss_total_preds()
pss_pain_preds()
pss_func_preds()
pss_sat_preds()
```

The first three models evaluate fitted beta-regression model objects and derive mean predictions and prediction quantiles. The satisfaction model uses the corresponding fitted proportional-odds model.

### `server.R`

Implements the application logic, including:

* patient input processing
* BMI calculation
* PSS calculation
* reactive model inference
* navigation between application components
* generation of individualized prediction results

### `ui.R`

Defines the interactive Shiny interface, including the patient characteristics, disease and surgical characteristics, baseline PSS workflow, and embedded PSS questionnaire.

---

## 📊 What the model tells us

The published analysis provides an important perspective on individualized outcome prediction.

Patients generally experienced excellent outcomes following primary ARCR, but substantial variation remained between individuals. The strongest predictors of 1-year PSS were:

1. **Preoperative PSS**
2. **VR-12 Mental Component Score**
3. **Insurance status**
4. **Race**

Acromioplasty and chronic pain were also among the most important predictors for selected PSS outcomes.

Importantly, the model's R² of approximately 20% also demonstrates a limitation of prediction from routinely collected preoperative variables: a substantial portion of individual outcome variability remains unexplained. Factors such as muscle strength, structural healing, and individual biology may contribute to outcomes but are not captured by this model.

---

## 💻 Running locally

This project is implemented in **R** using **Shiny**.

Install the required packages:

```r
install.packages(c(
  "shiny",
  "shinythemes",
  "plyr",
  "dplyr",
  "ggplot2",
  "reshape2",
  "rms",
  "betareg"
))
```

Then launch the application:

```r
shiny::runApp()
```

The application expects the trained model objects required by `global.R` to be available in the application environment.

> **Note:** The clinical development dataset is not included in this repository.

---

## 🌐 Deployment

A production version of this application is publicly available through the Cleveland Clinic Risk Calculator Library:

**https://riskcalc.org/Predicting1YearPROMSAfterRotatorCuffRepair/**

The published article explicitly identifies this URL as the implementation of the risk calculator based on the primary 1-year PSS Total and subscore models.

---

## ⚠️ Intended Use & Limitations

This calculator is intended for **research, educational, and clinical decision-support purposes**.

It should not be interpreted as a definitive prediction of an individual patient's outcome.

Important considerations include:

* The model was developed using data from a single healthcare system.
* The development cohort consisted of patients undergoing primary ARCR at Cleveland Clinic.
* Patients without baseline PROM information were excluded from model development.
* One-year PROM availability was 72% in the analysis cohort.
* The model explains only a modest proportion of individual outcome variability.
* Important biological, functional, imaging, and postoperative factors are not captured.

Clinical decisions should not be based solely on the calculator output.

---

## 📄 Citation

If you use this software, calculator, or prediction model in your research, please cite:

```bibtex
@article{Sahoo2026RCRPROM,
  title   = {Predictors of patient-reported outcomes after primary
             arthroscopic rotator cuff repair},
  author  = {Sahoo, Sambit and Imrey, Peter B. and Jin, Yuxuan and
             Cogan, Charles J. and Entezari, Vahid and Ho, Jason C. and
             Iannotti, Joseph P. and Ricchetti, Eric T. and Derwin, Kathleen A.},
  journal = {Journal of Shoulder and Elbow Surgery},
  volume  = {35},
  number  = {1},
  pages   = {143--154},
  year    = {2026},
  doi     = {10.1016/j.jse.2025.05.002}
}
```

**Publication:**
https://doi.org/10.1016/j.jse.2025.05.002

**Live calculator:**
https://riskcalc.org/Predicting1YearPROMSAfterRotatorCuffRepair/

**Source code:**
https://github.com/jiny-ccf/Predicting1YearPROMSAfterRotatorCuffRepair

---

## 📚 Related Work

The calculator is part of a broader effort to translate clinical prediction models into accessible, interactive tools for patient-reported outcomes following shoulder surgery.

The corresponding publication reports the model development, predictor importance, sensitivity analyses, and clinical context in detail.

---

## Authors

Developed at Cleveland Clinic as part of the development and deployment of an
interactive clinical prediction tool for total shoulder arthroplasty outcomes.

**Primary developer:** Yuxuan Jin  
**Affiliation:** Cleveland Clinic

For software questions, please open an issue in this repository.
For questions about the clinical study or prediction model, please refer to the
original publication.

---

## License

This project is released under the **MIT License**. See [`LICENSE`](LICENSE) for details.

