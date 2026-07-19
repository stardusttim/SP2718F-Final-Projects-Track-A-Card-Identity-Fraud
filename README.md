# **🛡️ Final Project — Track A / Issuer / Card Identity Fraud**

---

## **📌 Overview**

This KNIME workflow detects card-not-present fraud in the IEEE-CIS transaction dataset. It prepares the data, engineers fraud-risk features, and evaluates multiple detection approaches to help card issuers identify suspicious transactions while limiting false positives.   
---

**📁 File** 

Combined final KNIME.knwf  
Pipeline 1 \- Chen Jian.knwf  
Pipeline 2 \- Yuan Ruotian.knwf  
Pipeline 3 \- Zhang Xiaohan.knwf  
Pipeline 4 \- Zhang Yang.knwf  
Group 1 demo video.mov  
Sweetviz for ieee cis.ipynb

**Description**

*Combined final KNIME.knwf:* Complete KNIME workflow containing data preprocessing, feature engineering, chronological data splitting, Logistic Regression, XGBoost Tree, rule-based scoring and H2O Isolation Forest pipelines, together with model evaluation and decision-layer components.   
*Pipeline 1 \- Chen Jian.knwf:* Complete KNIME pipeline workflow containing data preprocessing, feature engineering, chronological data splitting, and Logistic Regression.  
*Pipeline 2 \- Yuan Ruotian.knwf*: Complete KNIME pipeline workflow containing data preprocessing, feature engineering, chronological data splitting, and XGBoost Tree.  
*Pipeline 3 \- Zhang Xiaohan.knwf:* Complete KNIME pipeline workflow containing data preprocessing, feature engineering, chronological data splitting, and rule-based scoring.  
*Pipeline 4 \- Zhang Yang.knwf:* Complete KNIME pipeline workflow containing data preprocessing, feature engineering, chronological data splitting, and H2O Isolation Forest.  
*weetviz for ieee cis.ipynb:* Colab notebook for EDA of the first 70% data in ascending time order.

The instructions below is the guidance for running the *Combined final KNIME.knwf* and the weetviz for ieee cis.ipynb. As each individual pipeline is already included in the full combined workflow, no specific instructions for running the separate pipelines. A video walkthrough demo is also available in the folder.

---

**🗂️ Data** 

\- Expected input file(s): ieee\_cis\_joined\_full.csv  (KAGGLE)
---

**⚓ Colab Run Order**

weetviz for ieee cis.ipynb:

1. Data Reading: save the dataset file to your google drive’s SP2718F \- GSSP2026 (folder name) → datasets (folder name) (if you don’t have SP2718F \- GSSP2026 folder or datasets folder, just create them)  
2. Prepare an empty html file called ieee\_cis\_report.html inside your google drive’s SP2718F \- GSSP2026 (folder name)  → outputs (folder name) (if you don’t have the output folder, just create one)  
3. Run the cells one by one.

---

**👥 KNIME Pipelines & Ownership** 

| Pipeline | Node range / location in workflow | Owner |  
|---|---|---|  
| Logistic Regression | Root514 (Learner) → Root510 (Predictor) | Chen Jian |  
| XGBoost (engineered features) | Root533 (Learner) → Root536/546 (Predictor) | Yuan Ruotian |  
| Rule-based scorecard | Root447 (Column Aggregator) → Root587 (combo\_review\_decision) | Zhang Xiaohan|  
| H2O Isolation Forest | Root590 (Learner) → Root591 (Predictor) | Zhang Yang |  
| Decision layer (stacked meta-model) | Root700 – Root719 | Zhang Yang |  
| Marginal contribution / ablation study | Root720 – Root731 | Zhang Yang |  
---

**▶️ KNIME Run Order**

*Combined final KNIME.knwf:*

1. Data Reading and Split into training set and testing set  
    — Root517 (CSV Reader:  save the dataset file to local, then copy file path and paste it into the csv reader.Expected settings: Advanced settings: TransactionID → String / isFraud → String)   
   → Root21 (Table Partitioner)  
2. Shared data prep / feature engineering steps — Root21 (Table Partitioner) → Root320/138 (Math Formula)  
3. Each pipeline can be run independently after step 2  
4. Decision layer (Root700 → Root719) — requires all four pipelines' predictors to have executed first  
5. Ablation study (Root720 → Root731) — requires Root710 (Table Partitioner) to have executed first

---

**⚙️ Setup / Dependencies**

\- KNIME version: 5.12.0  
\- Required extensions: KNIME H2O Machine Learning Integration & XGBoost Integration  
\- 42 for all random seeds: e.g. Root88 & 297 (XGboost Tree Ensemble Learner) / Root 590 (H2O Isolation Forest Learner) / Root514 & 711 & 730 & 734 & 739 & 741 (Logistic Regression Learner).

---

**⚠️ Known Issues**

**1\. GroupBy configuration**

Root153 / 155 / 145 / 567 / 549 need to be resetted manually: GroupBy → Manual Aggregation: Column \- TransactionID \- Aggregation: Count

**2\. Yellow warning icons** 

Some nodes (e.g. Root514 / Root469) might show a yellow warning icon. The yellow warning icon can be safely ignored; the node has executed successfully and will run normally.

**3\. In case TransactionID cannot be loaded**

If TransactionID doesn’t appear in the advanced settings’ list in the Csv Reader, Root153 / 155 / 145 / 567 / 549 need to be resetted manually: GroupBy → Manual Aggregation: Column \- isFraud \- Aggregation: Count

---

**📬 Contributors**

| Name |  
|---|  
| Chen Jian |   
| Yuan Ruotian |   
| Zhang Xiaohan |   
| Zhang Yang | 

For demo video, contact contributers above.
