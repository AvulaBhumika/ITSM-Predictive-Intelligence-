# ABC Tech - ITSM Predictive Intelligence Project

![image](https://github.com/user-attachments/assets/bd1b4836-22bf-42a1-961e-247020e7d819)



## 📊 Project Overview

ABC Tech, a mid-sized IT-enabled services company, initiated this project to improve the operational intelligence of its ITSM ecosystem. Despite having matured ITIL processes (Incident, Problem, Change, Configuration Management), ABC Tech had hit a plateau in customer satisfaction, prompting a shift from process maturity to predictive automation.

This project leverages machine learning to deliver four key predictive capabilities across the ITSM lifecycle.

---

## 🌐 Business Objectives

| Module | Goal                          | ML Objective               | Outcome                                                                         |
| ------ | ----------------------------- | -------------------------- | ------------------------------------------------------------------------------- |
| **1**  | Predict High Priority Tickets | Binary Classification      | Early identification of P1/P2 tickets to reduce escalation risk                 |
| **2**  | Forecast Incident Volume      | Time Series Forecasting    | Resource and capacity planning based on historical patterns                     |
| **3**  | Auto-tag Tickets              | Multi-class Classification | Automatically assign correct Priority and Department (CI\_Cat) to reduce delays |
| **4**  | Predict RFC Failures          | Binary Classification      | Prevent change-related failures via risk-based modeling                         |

---

## 🌐 Dataset Description

* Total Records: **46,606**
* Key Fields: `Incident_ID`, `Open_Time`, `CI_Name`, `CI_Cat`, `CI_Subcat`, `Status`, `Impact`, `Urgency`, `Priority`, `Closure_Code`, `Related_Change`, `Handle_Time_hrs`, `No_of_Reassignments`, `Category`, etc.
* Timestamps: From 2012 to 2014

---

## 📊 Module 1: High Priority Ticket Prediction

**Goal:** Predict if a ticket will be classified as Priority 1 or 2 (high priority)

### Model

* **Model:** LightGBM with class imbalance handling (class\_weight='balanced')
* **Oversampling:** SMOTE
* **Features Used:** Only pre-resolution metadata (e.g., `CI_Name`, `CI_Subcat`, `Category`, `Alert_Status`, `WBS`, `No_of_Reassignments`)

### Final Results

* **Accuracy:** 97%
* **Recall (P1/P2):** 77%
* **Precision (P1/P2):** 28%
* **F1-Score (P1/P2):** 0.41
* **AUC Score:** 0.95

### Notes:

* High recall ensures most critical tickets are flagged.
* SHAP used for interpretability.

---

## 🔄 Module 2: Incident Volume Forecasting

**Goal:** Predict volume of incidents quarterly/annually to aid in capacity planning.

### Model

* **Model:** Facebook Prophet
* **Input:** Monthly aggregation on `Open_Time`
* **Output:** Incident volume forecast for 12 months ahead

### Deliverables

* Time series plot with confidence intervals
* Forecast exported to Excel

### Insight

* Identified seasonal spikes in Q4 annually.
* Supports staffing decisions.

---

## 🔹 Module 3: Auto-tagging Tickets

**Goal:** Predict ticket `Priority` and `CI_Cat` for accurate assignment.

### Models

* **Priority:** LightGBM (Multi-class)
* **CI\_Cat:** LightGBM (Multi-class)

### Priority Model Results

| Class | Recall | F1-Score |
| ----- | ------ | -------- |
| 2     | 0.91   | 0.33     |
| 3     | 0.78   | 0.67     |
| 4     | 0.78   | 0.83     |
| 5     | 0.82   | 0.85     |

### CI\_Cat Model Results

* **Accuracy:** Nearly 100% across all categories
* **Classes:** 10+ departments (`application`, `subapplication`, `hardware`, etc.)

### Notes:

* SHAP plots show strong department-level signals.
* Clean auto-assignment can cut TAT drastically.

---

## 🔧 Module 4: RFC Failure Prediction

**Goal:** Identify RFCs likely to fail due to misconfiguration or error.

### Modeling

* **Filtered RFC rows:** Where `Related_Change` is present
* **Target:** Closure\_Code ∈ \['Other', 'Failed', 'Misconfiguration', 'Rollback'] ➔ label as 1 (failure)
* **Model:** LightGBM + SMOTE

### Final Metrics

* **Accuracy:** 84%
* **Recall (Failures):** 50%
* **Precision (Failures):** 38%
* **F1 (Failures):** 0.43
* **AUC:** 0.82

### Notes:

* Very strong performance on rare class (only 6 failures in test set)
* SHAP confirms useful signals from metadata like `CI_Cat`, `Status`, and `Reassignments`

---

## 📂 Deliverables

| File                            | Description                               |
| ------------------------------- | ----------------------------------------- |
| `model_lgbm_priority.pkl`       | Final priority classifier                 |
| `model_lgbm_cicat.pkl`          | Final department classifier               |
| `incident_volume_forecast.xlsx` | Forecast results from Module 2            |
| `rfc_failure_predictions.xlsx`  | Predicted RFC failures with probabilities |
| `auto_tagging_predictions.xlsx` | Auto-tagged Priority and CI\_Cat values   |
| `shap_summary_*.png`            | SHAP explainability plots                 |
| `README.md`                     | Project summary and instructions          |
| `summary_slides.pptx`           | Presentation-ready insights (optional)    |

---

## 🚀 Final Notes

* This solution is ready for API integration or dashboard deployment.
* Each module is modular and reusable.
* Business can prioritize next steps like SLA violation prediction, agent assignment models, or real-time feedback loops.


**~AvB**
