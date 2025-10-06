# Telecom Churn Prediction (Prepaid, High-Value Customers)

Exploratory Data Analysis (EDA) and modeling to predict churn among **prepaid, high-value** customers and identify the main indicators of churn so that retention teams can act before customers leave.

## Introduction

This case study applies **Exploratory Data Analysis** in a real-world **telecom churn** setting.  
The goal is to analyze multi-month usage and recharge behavior to distinguish **at-risk** customers from **healthy** ones, then build predictive and interpretable models that enable proactive retention.

When a telecom operator evaluates churn risk, two major business risks exist:
1. Not identifying a likely churner in time → **lost revenue / ARPU erosion**
2. Misclassifying healthy users as churners → **wasted retention offers / margin impact**

Data-driven scoring helps strike the right balance.

## Business Understanding

We focus on **prepaid** users (common in India & SE Asia), where churn is implicit (customers can simply stop recharging/using service).  
We work with four consecutive months of data encoded as **6, 7, 8, 9**:
- **Good phase:** months 6–7  
- **Action phase:** month 8 (behavior shifts before churn)  
- **Churn phase:** month 9 (used only to tag churn; then dropped from modeling)

Churn definition (usage-based for prepaid): in month 9, users with **no outgoing calls**, **no incoming calls**, and **no mobile data usage** are tagged as churners.

## Business Objectives

Identify **drivers of churn** and build a practical model that:
- Predicts **which high-value customers** are likely to churn next month  
- Surfaces **interpretable indicators** to guide retention strategy (offers, outreach)  
- Balances recall/precision to maximize **saves** within operational capacity

## Analytical Approach

1. **Data Understanding**
   - Consolidate customer, usage, and recharge information across months **6–9**.

2. **Data Preparation**
   - Derive domain-driven features (e.g., month-over-month deltas, on-net/off-net mix, voice/data ratios, recharge frequency/amount).
   - Define **high-value customers**: recharge amount ≥ 70th percentile of the **average recharge in months 6–7**.
   - **Tag churn** using month-9 usage: `total_ic_mou_9 = 0`, `total_og_mou_9 = 0`, `vol_2g_mb_9 = 0`, `vol_3g_mb_9 = 0`.
   - **Remove churn-phase attributes** (all `*_9` features) before modeling.

3. **EDA**
   - Univariate and segmented univariate (by churn) for usage, recharge, and network metrics.
   - Bivariate patterns across good vs action months to capture early warning signals.

4. **Modeling**
   - Handle class imbalance (e.g., `class_weight='balanced'` / resampling).
   - **Dimensionality reduction** (e.g., PCA) → build a predictive classifier (e.g., Logistic Regression / Tree-based).
   - **Interpretable model** (e.g., Logistic Regression or tree) to identify top churn drivers (check p-values/VIF or feature importances).

5. **Evaluation**
   - Prioritize metrics that reflect the business goal of catching churners: **Recall (churn class)**, **PR-AUC**, along with **ROC AUC**, **Precision**, **F1**.
   - Compare performance and calibration; choose an operating threshold aligned with retention capacity.

## Tools & Libraries

- **Python** (Jupyter Notebook)  
- **Pandas**, **NumPy**, **scikit-learn**, **Matplotlib**, **Seaborn**  
- EDA: univariate / segmented univariate / bivariate; PCA for reduction

## Key Insights

Patterns this workflow is designed to surface include (examples):
- **Decline in recharge amount/frequency** from good → action month
- **Drops in outgoing/incoming minutes**; changes in **on-net vs off-net** and **ISD** usage
- **Reduced mobile data volume** or shifts between **2G/3G** usage
- **Rising failed call ratios** or **customer-care interactions** (if available)
- Feature deltas (e.g., month-8 minus average of months-6/7) as strong early signals

*Exact drivers and magnitudes are reported in the notebook and should be referenced in decisioning playbooks.*

## Conclusion & Business Impact

A two-track approach - **predictive model** for timely saves and **interpretable model** for actionable drivers—enables:
- Prioritized **outreach lists** for the retention team
- **Offer targeting** (e.g., data top-up, tariff match) based on driver patterns
- Reduced **revenue leakage** from high-value churn

## Files in this Repository

| File Name | Description |
|------------|-------------|
| `Telecom_Churn_Case_Study.ipynb` | Full workflow (EDA → feature engineering → PCA/classification → interpretable model) |
| `customer_data.csv` | Customer-level monthly attributes |
| `internet_data.csv` | Data-usage metrics across months |
| `churn_data.csv` | Voice/SMS/recharge usage across months |
| `Telecom Churn Data Dictionary.csv` | Meanings of variables/abbreviations (loc, IC, OG, T2T, T2O, RECH, etc.) |

---

## Author

**Aakash Maskara**  
*M.S. Robotics & Autonomy, Drexel University*  
Data Science | Machine Learning | Quantitative Finance  

[LinkedIn](https://linkedin.com/in/aakashmaskara) • [GitHub](https://github.com/AakashMaskara)
