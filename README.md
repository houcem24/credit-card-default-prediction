# Credit Card Default Prediction

Predicting whether a credit card client will default on next month's payment, using the
[UCI "Default of Credit Card Clients"](https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients)
dataset (30,000 clients, Taiwan, 2005). The notebook covers cleaning, exploratory analysis, class rebalancing,
a comparison of seven classifiers and hyperparameter tuning.

## Results

About 22% of clients default, so the minority class was oversampled to a 50/50 balance before modelling.
Scores on the 20% test split:

| Model | Accuracy | Recall (default) | F1 (default) |
|---|---|---|---|
| **Random Forest** | **0.90** | **0.95** | **0.90** |
| Decision Tree | 0.86 | 0.95 | 0.87 |
| KNN | 0.75 | 0.84 | 0.77 |
| SVM | 0.70 | 0.61 | 0.67 |
| AdaBoost | 0.70 | 0.58 | 0.66 |
| Logistic Regression | 0.69 | 0.57 | 0.65 |
| Bernoulli Naive Bayes | 0.68 | 0.53 | 0.63 |

**Caveat:** oversampling was applied before the train/test split, so duplicated default cases appear in both
sets. That inflates the tree-based scores in particular, which can memorise duplicates. A sound evaluation would
oversample only the training data (for example with an `imblearn` pipeline inside cross-validation); the
linear and distance-based models are less affected.

## Approach

1. **Cleaning:** renamed columns, converted types, removed 35 duplicate rows, checked for missing values.
2. **Exploration:** default rate by sex, age, education and marital status; correlation matrix (the six monthly
   bill amounts are strongly correlated with each other).
3. **Preprocessing:** standardisation of numeric features, then selection of the 10 most informative features
   (`SelectKBest`, ANOVA F-test) inside a scikit-learn pipeline.
4. **Modelling:** seven classifiers compared with classification reports, ROC curves and F1.
5. **Tuning:** `RandomizedSearchCV` (4- to 5-fold) for Logistic Regression, AdaBoost, Random Forest and SVM.

## Repository structure

```
├── credit_default_prediction.ipynb   # Full workflow (comments in French)
├── data/
│   └── default_of_credit_card_clients.xls
└── requirements.txt
```

## Getting started

```bash
git clone https://github.com/houcem24/credit-card-default-prediction.git
cd credit-card-default-prediction
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook credit_default_prediction.ipynb
```

## Context

Machine learning course project at ESPRIT (class 4DS1, 2022), built in a team of six: Houaida Fatma Karoui,
Aicha Chaaba, Molka Mrad, Yassine Kochat, Firas Kahia and Houssem Ouerdiane.

## License

Code: [MIT](LICENSE). Dataset: Yeh, I-Cheng (2009), UCI Machine Learning Repository, CC BY 4.0.
