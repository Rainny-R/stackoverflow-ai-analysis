# README

## Overview

This project conducts statistical analysis and machine learning modeling on the 2025 Stack Overflow Developer Survey data to explore developers' acceptance of AI tools.

**Developers and AI: Understanding the Factors That Shape Acceptance**

While exploring the data, I discovered interesting patterns that shaped the direction of this analysis. Based on the provided dataset, I developed and analyzed relationships around three main questions:

1. **What factors best predict AI acceptance?**
2. **How does work experience affect AI acceptance?**
3. **How does AI acceptance differ by technology preferences?**

### Success Criteria

Build a machine learning model to accurately predict developers' AI acceptance level (Low/Medium), identify key influencing factors through feature importance analysis, and provide data-driven insights for companies developing AI developer tools.

## Project Structure
Machine_Learning_FinalProject/
├── StackOverFlow_Survey_Analysis.ipynb
├── README.md
├── requirements.txt
└── stack-overflow-developer-survey-2025.zip
	├── survey_results_public.csv
	├── survey_results_schema.csv
	└── 2025_Developer_Survey_Tool.pdf


## Data Understanding

**Data Source:** Stack Overflow Developer Survey 2025 (official release)

The downloaded data package includes three files:

- `2025_Developer_Survey_Tool.pdf` – Survey questionnaire (reference)
- `survey_results_public.csv` – Survey responses from 49,191 developers
- `survey_results_schema.csv` – Data dictionary explaining question meanings

### Initial Exploration
- Reviewed data shape, column types, and missing value ratio (overall missing rate: 52.45%)
- Analyzed distributions of target-related columns (AI usage, attitude, trust)
- Used schema to understand core features such as `TechEndorse_1` through `TechEndorse_9`

## Data Preparation

### Selecting Relevant Columns
Selected 5 target columns related to AI acceptance (`AISelect`, `AISent`, `AIAcc`, `LearnCodeAI`, `AIComplex`) and feature columns (9 technology preference columns + `WorkExp`, `Age`, `EdLevel`).

### Cleaning Data
- Dropped rows with critical missing values in target columns
- Encoded `Age` and `EdLevel` numerically (age mapped 1–7, education mapped 1–7)
- `TechEndorse` columns had no missing values; used original rankings (lower values indicate higher priority)

### Creating Target Variable
Combined 5 AI-related columns with weighted scoring to create an AI acceptance score (0–100), then classified into two categories: **Low** (<40) and **Medium** (≥40). (Note: No `High` class was present in the data.)

### Final Dataset
33,084 rows × 18 columns (including encoded features), with 28,943 complete rows used for modeling.

## Modeling

### Algorithm Selection
Random Forest Classifier was chosen for its ability to handle non-linear relationships, provide feature importance, and support class imbalance via the `class_weight` parameter.

### Handling Imbalance
Set `class_weight='balanced'` to give more focus to the minority class (`Medium`).

### Hyperparameter Tuning
Used `GridSearchCV` with 3-fold cross-validation to tune parameters such as `n_estimators`, `max_depth`, and `min_samples_split`. The best model achieved **72.39%** accuracy.

### Model Comparison

| Model | Accuracy |
|-------|----------|
| Original Random Forest | 65.6% |
| Tuned Random Forest | 67.4% |
| Grid Search Best Model | **72.39%** |

The best model was selected for final analysis.

## Evaluation

### Model Performance
- **Accuracy:** 72.39%
- **Classification Report:**  
  - Low class: precision 0.82, recall 0.68  
  - Medium class: precision 0.40, recall 0.59
- **Confusion Matrix:** Most errors occurred when Medium class was misclassified as Low.

### Feature Importance
- **Work Experience (`WorkExp`)** – 13.9%
- **AI Integration (`TechEndorse_1`)** – 11.6%
- All 9 technology preference features combined contributed **80.9%** of prediction weight

### Business Questions Answered
The three business questions were answered using visualizations and statistics (see Notebook sections: Question 1–3).

## Deployment

### Results Sharing
This analysis is organized into a Jupyter Notebook and published on GitHub.

## Acknowledgements

- **Data Source**: [Stack Overflow Annual Developer Survey 2025](https://insights.stackoverflow.com/survey)
- **Inspiration**: Understanding developer behavior to build better AI tools
- **Libraries Used**: pandas, numpy, matplotlib, seaborn, scikit-learn
