# **🛡️ Final Project — Track A / Issuer / Card Identity Fraud**

---

## **📌 Overview**

This KNIME workflow detects card-not-present fraud in the IEEE-CIS transaction dataset. It prepares the data, engineers fraud-risk features, and evaluates multiple detection approaches to help card issuers identify suspicious transactions while limiting false positives.   
---

**📁 File** 

KNIME\_Final\_Project.knwf 

**Description**

Complete KNIME workflow containing data preprocessing, feature engineering, chronological data splitting, Logistic Regression, XGBoost Tree, rule-based scoring and H2O Isolation Forest pipelines, together with model evaluation and decision-layer components.   
---

**👥 Pipelines & Ownership** 

| Pipeline | Node range / location in workflow | Owner |  
|---|---|---|  
| Logistic Regression | Root514 (Learner) → Root510 (Predictor) | Chen Jian |  
| XGBoost (engineered features) | Root533 (Learner) → Root536/546 (Predictor) | Yuan Ruotian |  
| Rule-based scorecard | Root447 (Column Aggregator) → Root587 (combo\_review\_decision) | Zhang Xiaohan|  
| H2O Isolation Forest | Root590 (Learner) → Root591 (Predictor) | Zhang Yang |  
| Decision layer (stacked meta-model) | Root700 – Root719 | Zhang Yang |  
| Marginal contribution / ablation study | Root720 – Root731 | Zhang Yang |  
---

**▶️ Run Order**

1\. Data Reading and Split into training set and testing set — Root517 (CSV Reader) → Root21 (Table Partitioner)  
2\. Shared data prep / feature engineering steps — Root21 (Table Partitioner) → Root320/138 (Math Formula)  
3\. Each pipeline can be run independently after step 2  
4\. Decision layer (Root700 – Root719) — requires all four pipelines' predictors to have executed first  
5\. Ablation study (Root720 – Root731) — requires Root710 (Table Partitioner) to have executed first  
---

**⚙️ Setup / Dependencies**

\- KNIME version: 5.11.0 / 5.12.0  
\- Required extensions: KNIME H2O Machine Learning Integration / XGBoost Integration  
\- 42 for all random seeds: e.g. Root88&297 (XGboost Tree Ensemble Learner) / Root 590 (H2O Isolation Forest Learner) / Root514&711&730&734&739&741 (Logistic Regression Learner)

---

**🗂️ Data** 

\- Expected input file(s): ieee\_cis\_joined\_full.csv  
\- Expected settings: Advanced settings: TransactionID → String / isFraud → String  
---

**⚠️ Known Issues**

**1\. GroupBy configuration**

Root153/155/145/567/549 must be resetted: GroupBy → Manual Aggregation → Column: TransactionID → Aggregation: Count

**2\. Yellow warning icons** 

Some nodes (e.g. Root514 / Root469) might show a yellow warning icon. The yellow warning icon can be safely ignored; the node has executed successfully and will run normally.  
---

**📬 Contributors / Contact** 

| Name |  
|---|  
| Chen Jian |   
| Yuan Ruotian |   
| Zhang Xiaohan |   
| Zhang Yang | 
