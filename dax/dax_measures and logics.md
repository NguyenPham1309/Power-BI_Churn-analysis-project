// This file contains the DAX logic for key measures and calculated columns
// It serves as a reference and documentation for the DAX code.
// -----------------------------------------------------------------------------
// Measure: Counting dynamic churning ratio
//This 1st idea is a static count of churning customer
% Churn_alldata = 
DIVIDE(
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset',
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    ),
    DISTINCTCOUNT(
        '01 Churn-Dataset'[customerID]
        ),
    "0"
)

//The dynamic count consists of 2 steps:
//Create the new table which contains only customer "Yes" in the churning column
//I used SUMMARIZECOLUMNS (similar to the GROUP BY function of SQL)
Churn_table = 
SUMMARIZECOLUMNS(
    '01 Churn-Dataset'[customerID],
    '01 Churn-Dataset'[Churn],
    FILTER(
        ALL('01 Churn-Dataset'),
        '01 Churn-Dataset'[Churn] = "Yes")
)
//Calculate the ratio by using DIVIDE
//Be aware that the “churn” customer I count is in a different table from the total number of customers.
% Churn = 
DIVIDE(
    COUNT(
        'Churn_table'[customerID]),
    DISTINCTCOUNTNOBLANK('01 Churn-Dataset'[customerID]),
    "0"
)
//-----------------------------------------------------------------------------
//Static segmentation approach: how to label the price within each static range

//Step 1: Create the table with the defined range for the needed values

//Step 2: Create the foreign key in the original table to connect with the table of defined range

//1. When scanning each charge record in the column Monthly Charges/Total Charges, it needs to be compared with the Min Charge/Max Charge column in the new range table. 
//2. The charge must be >= Min charge and <= Max charge.
//3. Output the final list of labels, where the filter from the range in step 2 will choose the only suitable title for that range value. Be careful with the ‘=’ to avoid confusion when labelling a particular charge.

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
	
//Finally, we can connect the range tables to the original fact table