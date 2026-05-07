# Alzheimer's Disease Classification

<img width="1891" height="885" alt="image" src="https://github.com/user-attachments/assets/51a5220b-71b3-4f6e-8921-39c7404c6642" />


A machine learning web application that predicts Alzheimer's disease progression — classifying patients as **Dementia**, **Mild Cognitive Impairment (MCI)**, or **Normal (NL)** — using clinical, neuroimaging, genetic, and biomarker data.

## Overview

This project trains and evaluates three supervised learning models on longitudinal patient data from the [Alzheimer's Disease Neuroimaging Initiative (ADNI)](https://adni.loni.usc.edu/). A Flask web app exposes the models through an interactive interface with visualizations and model comparisons.

## Features

- Multi-class classification (Dementia / MCI / Normal)
- Three model choices: Support Vector Machine, Random Forest, Logistic Regression
- Configurable pipeline: optional feature scaling (MinMax or Standard) and class-imbalance handling (SMOTE or random oversampling)
- 5-fold cross-validation with accuracy, precision, recall, F1, and ROC curve metrics
- Interactive web interface with demographics, treatment, and methodology pages
- Decision tree visualization endpoint

## Tech Stack

| Layer | Tools |
|---|---|
| Web framework | Flask, Flask-CORS |
| ML / Data | scikit-learn, imbalanced-learn, Pandas, NumPy |
| Deep learning (experimental) | Keras, TensorFlow 1.14 |
| Visualization | Plotly |
| Deployment | Gunicorn, Heroku (Procfile) |

## Data

The `/Data` directory contains progressively enriched CSV datasets built from ADNI records:

| Feature group | Variables |
|---|---|
| Demographics | Age, sex, race, ethnicity, education |
| Genetics | APOE4 status |
| Cognitive tests | MMSE, ADAS-Cog, RAVLT |
| Neuroimaging (MRI/PCT) | Hippocampus, ventricles, whole brain, entorhinal, fusiform, mid-temporal volumes |
| PET imaging | FDG-PET, AV45 |
| Cerebrospinal fluid | CSF biomarkers |

Final combined datasets (e.g., `final_DX_demographic_apoe4_cogtest_mripct_pet_csf.csv`) integrate all feature groups.

## Installation

```bash
git clone https://github.com/jasminajovanovic/Alzheimer-s_Disease.git
cd Alzheimer-s_Disease
pip install -r requirements.txt
```

> **Note:** `requirements.txt` pins TensorFlow 1.14 and Keras 2.2. A Python 3.6–3.7 environment is recommended for compatibility.

## Running the App

```bash
python app.py
# or
bash run.sh
```

Then open `http://localhost:5000` in your browser.

## API

**`GET /getdata/<dict>`**

Trains (or loads cached) models and returns evaluation metrics as JSON.

| Parameter | Type | Description |
|---|---|---|
| `prediction` | string | Target feature set to use |
| `oversampling` | bool | Apply SMOTE / random oversampling |
| `scaling` | string | `minmax`, `standard`, or `none` |
| `cross_validate` | bool | Run 5-fold cross-validation |

## Pages

| Route | Description |
|---|---|
| `/` | Home / model runner |
| `/methodology` | Pipeline and methodology documentation |
| `/models` | Model performance comparison |
| `/demographics` | Patient demographic breakdowns |
| `/treatment` | Treatment information |
| `/treeimage` | Decision tree visualization |
| `/resources` | Data sources and references |

## Project Structure

```
├── app.py               # Flask application and routes
├── ad_tools.py          # ML pipeline (preprocessing, training, evaluation)
├── Data/                # Raw and processed CSV datasets
├── Models/              # Cached trained model files
├── templates/           # Jinja2 HTML templates
├── static/              # CSS, JS, images
├── requirements.txt
├── Procfile             # Heroku deployment
└── runtime.txt          # Python version spec
```

## ML Pipeline

1. Load and clean ADNI data (drop rare diagnoses, handle missing values)
2. Encode categorical variables with `LabelEncoder`
3. 80/20 train-test split
4. Optional scaling (MinMax or Standard)
5. Optional oversampling (SMOTE or random)
6. Train SVM (linear), Random Forest (200 trees), or Logistic Regression
7. Evaluate: accuracy, precision, recall, F1, ROC-AUC (one-vs-rest)
8. Cache trained models with `cloudpickle`

## License

This project is forked from [parismoscow/Alzheimer-s_Disease](https://github.com/parismoscow/Alzheimer-s_Disease). Please refer to the original repository for licensing details.
