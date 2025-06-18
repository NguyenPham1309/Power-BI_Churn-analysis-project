# Project: Power BI Customer Churn Analysis Dashboard

**Author:** NGUYEN PHAM
**Date:** May 14, 2025
**Contact:** nguyen.pham961309@gmail.com | [https://www.linkedin.com/in/khoinguyenpham/]
You can find the detail work and analysis [here in my blog](https://nguyenphamdp1309.com/customer-analysis-how-can-we-target-right/)

---

## 1. Introduction & Objectives

* This project presents an interactive Power BI dashboard designed to analyze the customer churn 
* The primary objective is to improve the customer retention by identifying different segment's profiles of multi features. From a dynamic group of features selected, the business users can understand the exact customer target - where they can put their best efforts to proactively encourage the customer loyalty
* This builds upon experience in creating impactful dashboards to track performance and drive customer-centric decisions, similar to my work done at AIA Vietnam

**The core problem** of this project addresses is the need for a clear, dynamic, and easily interpretable visualization of churn metrics, customer segmentation based on churn risk, and the factors influencing a customer's decision to leave.

**Key Analytical Goals:**
* To identify and visualize key churn indicators
* To segment customers based on their churn risk profile (categorical (demographic, contract) or numerical (discrete or continuous value))
* To provide a user-friendly interface for stakeholders to explore churn data and trends

---

## 2. Data Sources
This dataset is highly structured and standardized, but there are rooms for further exploration to have the analysis efficient
* Each row represents a unique customer, with the Churn (Leave) decision (Yes/No)
* There are columns with binary values (Yes / No). The column names suggest that those are services that the customers have already subscribed to or not during their time with this telecom company
* There are columns with categorical values (for instance, different type of Bank transfer – Payment, Credit card, or others)
* There are quantitative variables, including monthly charge, total historical charges, contract tenure, number of technical tickets sent

---

## 3. Tools & Technologies Used
*   **Data Visualization & Dashboarding:** Microsoft Power BI (including DAX for complex measures and calculations)
*   **Data Preparation:** Microsoft Excel (Power Query)
*   **Version Control:** Git & GitHub
*   **IDE/Editor (for this README):** Visual Studio Code

---

#### Project Structure

The repository is organized as follows:

```text
PwC Swiss_Nguyen Khoi Nguyen Pham_Churn rate analysis.pbix # The main Power BI file
├── data/                                                 # For the datasets
│   └── 02 Churn-Dataset.csv
├── dax/                                                  # For dax script including measures
│   └── dax_measures and logics.md
├── images/                                               # For screenshots of my dashboard
│   ├── PwC Swiss_Data Analysis by PBI_Nguyen Khoi Nguyen Pham-08.png
│   ├── PwC Swiss_Data Analysis by PBI_Nguyen Khoi Nguyen Pham-09.png
│   ├── Create the dynamic Churning ratio.png
│   ├── Create the tables for static segmentation.png
│   ├── Table of monthly charge.png
│   └── Table of total charge.png
├── .gitignore                                            # Specifies intentionally untracked files
└── README.md                                             # This file!