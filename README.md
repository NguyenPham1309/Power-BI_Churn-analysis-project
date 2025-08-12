# Project: Power BI Customer Churn Analysis Dashboard

**Author:** NGUYEN PHAM
**Date:** May 14, 2025
**Contact:** nguyen.pham961309@gmail.com | [https://www.linkedin.com/in/khoinguyenpham/]
You can find the detail work and analysis [here in my blog](https://nguyenphamdp1309.com/customer-analysis-how-can-we-target-right/)

---
## 📌 Table of Contents

This document outlines the structure of the Churn analysis project. **Click the links below to jump directly to the corresponding section in the main notebook.**
1.  [**Introduction & Objectives**](#1-introduction--objectives)
2.  [**Data Sources**](#2-data-sources)
3.  [**Tools & Technologies Used**](#3-tools--technologies-used)
4.  [**Dashboard creation images**](#4-dashboard-creation-images-from-power-bi)
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
## 4. Dashboard creation images (from Power BI)
* **Combined dynamic filters for Categorical values**
![Dashboard](https://github.com/NguyenPham1309/Power-BI_Churn-analysis-project/blob/main/images/PwC%20Swiss_Data%20Analysis%20by%20PBI_Nguyen%20Khoi%20Nguyen%20Pham-08.png)

* **Dynamic dashboard with filters for Numeric values**
![Dashboard](https://github.com/NguyenPham1309/Power-BI_Churn-analysis-project/blob/main/images/PwC%20Swiss_Data%20Analysis%20by%20PBI_Nguyen%20Khoi%20Nguyen%20Pham-09.png)

* **Create the dynamic churning ratio with the data modeling view**
![Modeling](https://github.com/NguyenPham1309/Power-BI_Churn-analysis-project/blob/main/images/Create%20the%20dynamic%20Churning%20ratio.png)
💡 **Insight**
1. The first instinct is to create a ratio that counts every customer terminating their contract/or not terminating, over the total number of customers. However, the idea creates a static measure, which is tightly knitted to the condition inside the DAX syntax and not to the visualization filter.
2. Thus, this should be used as the benchmark showcasing the average of the churn ratio among the dataset. Any customer segment with a higher ratio than the average will have a higher possibility of terminating their contract soon. So, after trying many techniques, I was inspired by the idea of using CTE (Common Table Expressions) from SQL to calculate this metric. The idea is to create a table with a set condition (containing all the customers who terminated their contracts – “Yes” in the churn column). By connecting it with the original table, it will be influenced by the filter of any categorical and numeric variables, and make the churn metric dynamic.
```dax
Churn_table = 
SUMMARIZECOLUMNS(
    '01 Churn-Dataset'[customerID],
    '01 Churn-Dataset'[Churn],
    FILTER(
        ALL('01 Churn-Dataset'),
        '01 Churn-Dataset'[Churn] = "Yes")
)
```
3. Then I used a usual DIVIDE and COUNT syntax to calculate the dynamic churn ratio. Be aware that the “churn” customer I count is in a different table from the total number of customers. 
```dax
% Churn = 
DIVIDE(
    COUNT(
        'Churn_table'[customerID]),
    DISTINCTCOUNTNOBLANK('01 Churn-Dataset'[customerID]),
    "0"
)
```

* **Transform selected numeric columns (the Monthly Charge and Total Charges) for better analysis**
I came up with the idea to transform them into a range of charge fees. It is usually called the Static Segmentation. We can define the benchmark of the range by using a distribution plot, for example, a boxplot with average, median, and IQR(25%-75%), or by combining with a density plot(to detect outliers or any meaningful "hump" in the plot)

![Transform](https://github.com/NguyenPham1309/Power-BI_Churn-analysis-project/blob/main/images/Create%20the%20tables%20for%20static%20segmentation.png)

* **Step 1**: Create the table with the defined range for the needed values
![Monthly charge](https://github.com/NguyenPham1309/Power-BI_Churn-analysis-project/blob/main/images/Table%20of%20monthly%20charge.png)
![Total charges](https://github.com/NguyenPham1309/Power-BI_Churn-analysis-project/blob/main/images/Table%20of%20total%20charge.png)

* **Step 2**: Create the foreign key in the original table to connect with the table of defined range. I break through small steps.
* When scanning each charge record in the column Monthly Charges/Total Charges, it needs to be compared with the Min Charge/Max Charge column in the new range table.
* The charge must be >= Min charge and <= Max charge.
* Output the final list of labels, where the filter from the range in step 2 will choose the only suitable title for that range value. Be careful with the ‘=’ to avoid confusion when labelling a particular charge.
```dax
MonthlyRangeKey = 
VAR CURRENT_MONTHLY = '01 Churn-Dataset'[MonthlyCharges]#An alias name for charge variable 

#Create the filter comparing the scanned value of charge to the range in the new range table 
VAR FILTERSEGMENT =
    FILTER(
        'MonthlyChargeRange',
        AND(
            MonthlyChargeRange[Min Charge] < CURRENT_MONTHLY,
            MonthlyChargeRange[Max Charge] >= CURRENT_MONTHLY
        )
    )
#Output is a list where there is only one label, filtered by the condition in step 2
VAR RESULT =
    CALCULATE(
        DISTINCT('MonthlyChargeRange'[Range Key]),
        FILTERSEGMENT
    )
RETURN
    RESULT
```

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
