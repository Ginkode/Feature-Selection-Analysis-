# Autism Feature Selection Study

Statistical-analysis project on an autism-screening dataset, focused on **how different feature-selection and dimensionality-reduction strategies change both prediction and interpretation**.

The original university report is in Italian; this README summarizes the main methodological choices and results in English for the portfolio.

## Research question

The analysis asks which clinical, demographic and family variables remain useful for classifying subjects with and without ASD traits **after removing variables that act as direct diagnostic proxies**.

A full logistic model including questionnaire and diagnostic-scale variables reached almost perfect performance, but this was treated as evidence of leakage rather than as a meaningful predictive result. The analysis therefore continues on a reduced dataset without the main screening/diagnostic variables.

## Methods compared

Three different strategies were applied to the reduced dataset:

### Boruta — all-relevant feature selection

Boruta uses Random Forest importance and shadow variables to identify predictors that carry information about the target.

In this analysis Boruta confirmed 10 variables, including sex, family history of ASD, speech/language delay, global developmental delay/intellectual disability, anxiety, depression, genetic disorders, learning disorder, social/behavioural issues and age.

Its main strength here is **retaining all potentially informative variables**, even when several of them are strongly correlated.

### LASSO — sparse feature selection

LASSO applies an L1 penalty and can shrink some coefficients exactly to zero. This makes it useful when the goal is to reduce multicollinearity and obtain a smaller, more parsimonious model.

The final LASSO-based model used 6 predictors and achieved essentially the same predictive performance as Boruta while using fewer variables.

### FAMD — dimensionality reduction

Factor Analysis of Mixed Data (FAMD) was used because the dataset contains both quantitative and qualitative variables.

Rather than selecting original variables, FAMD constructs orthogonal latent components. Three components were retained and interpreted as:

- a **neurodevelopmental / clinical** dimension;
- an **age-related** dimension;
- a **biological / family** dimension.

This approach reduces multicollinearity while preserving information in latent constructs rather than discarding variables directly.

## Model comparison

| Model | Accuracy | AUC | Predictors / components | Main role |
|---|---:|---:|---:|---|
| Full logistic model | 0.991 | 0.999 | 27 variables | diagnostic leakage / reference model |
| LASSO | 0.693 | 0.741 | 6 variables | sparse selection |
| Boruta | 0.693 | 0.742 | 10 variables | all-relevant selection |
| FAMD | 0.682 | 0.725 | 3 components | latent dimensionality reduction |

The predictive differences among the three reduced models are small. The important result is therefore not simply which model has the highest AUC, but **what kind of information each method preserves**.

## Methodological conclusion

The report reaches two complementary conclusions:

- **LASSO provides the best compromise between predictive performance, interpretability and parsimony**, because it removes several strongly correlated variables while preserving essentially the same predictive performance as Boruta.
- **FAMD is the most coherent choice for the latent-factor research objective**, because it summarizes the correlated clinical information into a small number of interpretable dimensions instead of eliminating it.

Boruta is useful when the aim is to identify all informative variables, but in this dataset that also means retaining several highly correlated predictors.

This comparison is the main contribution of the project: methods with similar predictive performance can lead to **substantially different scientific interpretations**.

## Additional findings

The exploratory analysis found strong redundancy among several clinical variables. For example, Speech Delay/Language Disorder and Learning Disorder were very highly correlated, as were Depression and Anxiety Disorder. This motivated the use of feature-selection and dimensionality-reduction methods rather than relying on a single full model.

## Repository structure

```text
Feature-Selection-Analysis-/
├── README.md
└── report/
    └── autism_feature_selection_report.pdf
```

The PDF contains the complete analysis, diagnostics, tables, plots and discussion.

## About the source code

The original analysis source code is not currently included in the repository. If it is recovered later, it can be added to make the workflow fully reproducible. Rewriting it from scratch solely to fill the repository is not necessary, because this project's portfolio value is primarily the **methodological comparison and statistical reasoning documented in the report**.
