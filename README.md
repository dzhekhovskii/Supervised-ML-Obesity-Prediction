# Obesity Prediction – Supervised Learning Final Project

## Contributors
Danila Zhekhovskii  
Naye Kanoute  
Marianne Akobe  
Nicolas Dollet  

## 1. Business Challenge

### Context
Obesity has become one of the most critical public health challenges of the 21st century.
Since 1975, global obesity rates have tripled. In 2024:
- 1.9 billion adults were overweight
- 650 million adults were obese
- 340 million children and adolescents (5–19) were overweight or obese

Obesity increases the risk of diabetes, cardiovascular diseases, cancer, and premature mortality.
Beyond health risks, the economic impact is staggering:
- 2023: ~$2 trillion (2.8% of global GDP)
- 2035: projected ~$4.3 trillion
- 2060: may exceed $18 trillion

### The Problem
How can we use Machine Learning to help individuals prevent obesity with personalized, accessible, and effective recommendations?

### Nomore – Our Vision
Nomore is a health-tech initiative aimed at empowering individuals to adopt healthier lifestyles using:
- AI-powered personalized recommendations
- Behavior and habit tracking
- Predictive insights using ML
- Community, coaching, and long-term support

Predicting obesity levels allows us to:
- Identify at-risk users early
- Provide tailored nutrition & activity recommendations
- Improve long-term health and retention
- Support clinicians and nutrition specialists with explainable predictions

This ML model will be a core component of Nomore’s health engine.

## 2. Dataset Description

### Source
Dataset combining real data and SMOTE-balanced synthetic samples.
- 23% real user data (Mexico, Peru, Colombia)
- 77% synthetic (Weka + SMOTE)
- Total records: 2,111
- Features: 17 + target

### Target Variable
**NObeyesdad — 7 classes:**
- Insufficient Weight
- Normal Weight
- Overweight I
- Overweight II
- Obesity I
- Obesity II
- Obesity III

### Features

| Feature | Type | Description |
|--------|------|-------------|
| Gender | Categorical | Male / Female |
| Age | Continuous | Age |
| Height, Weight | Continuous | Physical measures |
| family_history_with_overweight | Binary | Genetic predisposition |
| FAVC | Binary | High-calorie food consumption |
| FCVC | Integer | Vegetable eating frequency |
| NCP | Continuous | Number of daily meals |
| CAEC | Categorical | Eating between meals |
| SMOKE | Binary | Smoking |
| CH2O | Continuous | Water intake |
| SCC | Binary | Calorie monitoring |
| FAF | Continuous | Physical activity |
| TUE | Integer | Tech usage |
| CALC | Categorical | Alcohol consumption |
| MTRANS | Categorical | Transport mode |

## 3. How to Reproduce

### Python version
Python 3.11

### Install Dependencies
```bash
pip install -r requirements.txt
```

### EDA Dataset Placement
```
/data/ObesityDataSet.csv 
```

### Preprocessed Dataset Placement
```
/data/ObesityDataSet_preprocessed.csv 
```

### Run EDA
```
jupyter notebook eda.ipynb
```

### Run Training Pipeline
```
jypyter notebook main.ipynb
```

## 4. Exploratory Data Analysis
Explained more in details inside the notebook `eda.ipynb`

### Numerical Insights
- Strong correlation between Height & Weight (~0.46)
- Weak correlations elsewhere → good for ML
- Computed mean, std, percentiles
- Low-variance features identified

### Categorical Insights
- SMOKE & SCC extremely low variance → removed
- CAEC, CALC, MTRANS are unbalanced

### Target Simplification
Original 7 classes → **4 simplified classes:**
- Insufficient weight
- Normal weight
- Overweight
- Obesity

### Data Cleaning
Removed:
- 24 duplicates
- SMOKE, SCC
- FCVC, TUE (weak signal)
- Height, Weight (leakage)

Converted and standardized numeric types.

## 5. Feature Engineering
Explained more in details inside the notebook `main.ipynb`

### One-Hot Encoding
- Gender
- family_history_with_overweight
- FAVC

### Ordinal Encoding
NCP:
- 1–2 → 0
- 3–4 → 1

CAEC:
- no → 2
- sometimes/frequently → 1
- always → 0

CALC:
- no → 2
- sometimes → 1
- frequently/always → 0

MTRANS:
- walking/bike → 2
- public transport → 1
- motorbike/car → 0

FAF kept as numeric

### Target Encoding
7 original → 4 simplified classes.

## 6. Baseline Model
Explained more in details inside the notebook `main.ipynb`

### Model Used
Logistic Regression

### Baseline Performance
- Accuracy: 0.55
- Weighted F1-score: 0.53

## 7. Modeling Workflow

- Train-test split (80/20, stratified)
- 5-fold stratified CV

Models:
- Logistic Regression
- Random Forest
- kNN
- Naive Bayes

Hyperparameter tuning: GridSearchCV

Metrics: Accuracy, F1-weighted, Confusion matrix

## 8. Best Model: Random Forest
Explained more in details inside the notebook `main.ipynb`

### Final Test Metrics
- Accuracy: 0.7464
- Weighted F1-score: 0.7480

### Confusion Matrix Summary
- Overweight & Obesity predicted well
- Insufficient weight: ~71% correct
- Normal weight → most confusion

### ROC Curves
- AUC > 0.90 for all classes

### Precision–Recall
- Best: Overweight
- Hardest: Normal weight

## 9. Feature Importance
Explained more in details inside the notebook `main.ipynb`

Top predictors:
1. Age
2. CH2O
3. FAF
4. CALC
5. family_history_with_overweight
6. Gender
7. MTRANS
8. NCP

## 10. SHAP Explainability
Explained more in details inside the notebook `main.ipynb`

Key insights:
- Age ↑ → Obesity ↑
- Low water intake → obese prediction
- High physical activity → normal weight
- Family history → strong influence

SHAP aligns with feature importance.

## Clinical Value
- Individual-level explanations
- Actionable habits: water intake, activity, alcohol, transport

## 11. Experiment Tracking

| Experiment | Change | Result | Notes |
|-----------|--------|--------|-------|
| 1 | Baseline LR | Acc 0.55 | Weak |
| 2 | Feature engineering | +10% | Strong |
| 3 | Model comparison | RF best | Captures interactions |
| 4 | RF tuning | Acc 0.74 | Best generalization |
| 5 | SHAP | — | Interpretability validated |

## 12. Project Structure
```
project/
│
├── ObesityDataSet.csv
├── ObesutyDataSet_preprocessed.csv
├── README.md
├── eda.ipynb
├── main.ipynb
├── requirements.txt
```

## 14. Conclusion
Machine Learning can:
- Identify obesity risk early
- Provide personalized insights
- Power a preventive health platform
- Improve long-term habits
- Produce interpretable predictions

Random Forest is accurate, explainable, aligned with real-world health patterns.
