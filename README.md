# MSCS-634 Project Deliverable 1: Data Collection, Cleaning, and Exploration

## Dataset Summary

**Dataset:** Telco Customer Churn (IBM sample dataset)
**Size:** 7,043 customer records, 21 attributes
**Domain:** Customer behavior / telecom subscription churn

The dataset captures demographic attributes (gender, senior citizen status, partner/dependents), account attributes (tenure, contract type, payment method), subscribed services (phone, internet, streaming, security add-ons), and billing figures (monthly and total charges), with `Churn` (Yes/No) as the target of business interest.

This dataset was chosen because its mix of categorical and continuous attributes supports every later stage of the project: regression on billing amounts, classification of churn, clustering of customer segments, and association rule mining across service subscriptions.

## Key Insights from Analysis

- **Class imbalance:** ~26.5% of customers churned vs. ~73.5% retained — future classification work should rely on F1/ROC-AUC rather than accuracy alone.
- **Contract type is the strongest visible churn signal:** month-to-month customers churn at a much higher rate than one- or two-year contract holders.
- **Fiber-optic internet and electronic-check payment** are also associated with higher churn rates.
- **Tenure is bimodal:** a large group of very new customers and a large group of long-tenured (near 72-month) customers — a useful signal for customer segmentation/clustering.
- **TotalCharges is highly collinear** with tenure and MonthlyCharges (mechanically, since it accumulates from both), which will need to be accounted for in regression modeling.

## Data Cleaning Steps Performed

1. **Type correction:** `TotalCharges` was stored as text; converted to numeric.
2. **Missing values:** 11 records had blank `TotalCharges`, all corresponding to customers with `tenure == 0` (new customers not yet billed) — filled with 0 rather than imputed, since the cause is structural, not random.
3. **Duplicates:** Checked for duplicate `customerID`s and fully duplicate rows — none found.
4. **Consistency:** Standardized `SeniorCitizen` from 0/1 to No/Yes to match the rest of the binary attributes; scanned all categorical columns for inconsistent labels — none found.
5. **Outlier check:** Boxplots of tenure, MonthlyCharges, and TotalCharges showed no implausible values requiring removal.

The cleaned dataset is saved as `data/telco_cleaned.csv` for use in Deliverables 2-4.

## Challenges Encountered

- The blank `TotalCharges` values were initially indistinguishable from ordinary missing data until cross-referencing with `tenure` revealed they were all zero-tenure customers — a reminder to investigate the *cause* of missingness before choosing an imputation strategy.
- `SeniorCitizen`'s inconsistent 0/1 encoding (versus Yes/No elsewhere) was easy to miss without explicitly listing unique values per column.

## Files in This Deliverable

- `Deliverable1_Data_Cleaning_EDA.ipynb` — full notebook with code, comments, and visualizations
- `README.md` — this file
- `churn_distribution.png`, `numeric_distributions.png`, `boxplots_outliers.png`, `churn_by_category.png`, `correlation_heatmap.png` — exported EDA visualizations
