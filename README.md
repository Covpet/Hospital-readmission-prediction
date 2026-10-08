# Hospital Readmission Prediction

## Introduction

When a patient is discharged from hospital and returns within 30 days, it is called a
**30-day readmission**. Many of these return visits can be prevented with better discharge
planning, follow-up appointments and medication support. Readmissions are hard on patients
and expensive for the healthcare system. In the US, the Centers for Medicare & Medicaid
Services (CMS) also reduces payments to hospitals with higher-than-expected readmission
rates through the Hospital Readmissions Reduction Program.

A related problem is **frequent emergency room use**. A small group of patients visits the ER
again and again, often for issues that could be managed earlier through primary care or
care coordination.

## Problem Statement

Care teams have limited time and cannot give intensive follow-up to every discharged patient.
They need a way to identify, at the point of discharge, **which patients are most likely to
return to hospital within 30 days**, so that support can go where it will have the most impact.

## Objectives

1. Explore the data to understand which patient and hospital-stay factors are linked to readmission
2. Profile frequent ER users and check whether they are also more likely to be readmitted
3. Build and compare machine learning models that predict 30-day readmission risk
4. Rank patients into risk groups a care team could use to prioritise follow-up

# Predicting 30-Day Hospital Readmissions

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Covpet/hospital-readmission-prediction/blob/main/hospital_readmission_analysis.ipynb)

## Data

- **Source:** Diabetes 130-US Hospitals for Years 1999–2008, UCI Machine Learning Repository
- **Size:** 101,766 hospital encounters from 130 US hospitals; 69,987 unique patients after cleaning
- **Target:** readmitted within 30 days of discharge (9.0% of patients)
- The notebook downloads the data automatically, so no manual download is needed.

## Approach

1. **Cleaning:** removed patients who died or went to hospice, kept one encounter per patient
   to avoid leakage, and treated unrecorded values as their own category
2. **EDA:** readmission rates by age, prior visits, discharge destination, payer and diagnosis
3. **Feature engineering:** ICD-9 codes grouped into clinical categories, medication counts,
   frequent-ER-user flag
4. **Preprocessing:** log transform on skewed counts, scaling, one-hot encoding in a scikit-learn Pipeline
5. **Modelling:** logistic regression, random forest and gradient boosting with balanced class weights
6. **Evaluation:** ROC-AUC, PR-AUC, recall and precision, plus threshold tuning and risk deciles

## Key findings

![Readmission by discharge destination](Read.png)

- Patients with 2+ inpatient stays in the prior year were readmitted at **21.5%**, against **8.1%** with none
- Patients discharged to skilled nursing, rehab or long-term care were readmitted at **15.2%**, against **6.9%** for those sent home
- Frequent ER users (2+ visits) were readmitted at **14.4%**, against **8.9%** for everyone else
- [Add one finding that surprised you, and why]

## Model results

| Model | ROC-AUC | Recall | Precision |
|---|---|---|---|
| Logistic Regression | 0.644 | 55.1% | 13.7% |
| Random Forest | 0.656 | 45.9% | 15.6% |
| Gradient Boosting | 0.649 | 53.9% | 13.9% |

![Risk deciles](risk_deciles.png)

The 20% of patients with the highest predicted risk accounted for **39%** of all actual readmissions.
[In your own words: how a care team could use this ranking.]

## Limitations

- Data is from 1999–2008 and covers diabetic inpatients only
- ROC-AUC around 0.65 is modest, so the model is best used to rank patients, not as a yes/no decision
- The findings are associations, not causes

## Next steps

- XGBoost with tuning, SHAP explanations, a separate model for frequent ER use.

## Tools

Python, pandas, NumPy, scikit-learn, matplotlib, seaborn, Google Colab

## Citation

Dataset:
> Clore, J., Cios, K., DeShazo, J., & Strack, B. (2014). *Diabetes 130-US Hospitals for Years 1999–2008* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5230J

Original study:
> Strack, B., DeShazo, J. P., Gennings, C., Olmo, J. L., Ventura, S., Cios, K. J., & Clore, J. N. (2014). Impact of HbA1c Measurement on Hospital Readmission Rates: Analysis of 70,000 Clinical Database Patient Records. *BioMed Research International*, 2014, Article ID 781670.

The dataset is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

## License

Code is released under the MIT License (see `LICENSE`).

## Author

**Covenant Ojo** · [GitHub](https://github.com/Covpet) 

Built with AI assistance; analysis and interpretation are my own.
