![image](https://github.com/user-attachments/assets/10933c6e-3ad9-439a-9f44-90ebc7aedf4d)

# Credit-Card-Dashboard
This dashboard will contain the data visualization of a credit card datasets as well as some insights gained from the dashboard
The datasets is obtained through kaggle and can be accessed from this link:
https://www.kaggle.com/datasets/rikdifos/credit-card-approval-prediction
# Data Pre-Processing 
## 1. Data Cleaning
In this dataset, in the OCCUPATION_TYPE column there are some missing values. Therefore, we need to fill it, in this case since the column contain the occupation type of the credit card holder and based on some consideration like the income, the education level, the marrital status, I decided to fill the missing value with Low Income Unspecified using these following line of code in M Query in Power BI
```
= Table.ReplaceValue(#"Previous Step","","Low Income Unspecified",Replacer.ReplaceValue,{"OCCUPATION_TYPE"})
```

## 1. Data Transformation
During this step the data will be transformed into standardize format for the columns, columns with numerical value will use whole number data type and for the other columns it will use text data type.The next thing that I do is to transform the DAYS_BIRTH column to year format, since the original value of the data is in days format. After that I transform the data type into whole number. This step is done using this following M Query Code
```
= Table.TransformColumns(#"Previous Step", {{"DAYS_BIRTH", each _ / 365, type number}})
= Table.TransformColumnTypes(#"Previous Step",{{"DAYS_BIRTH", Int64.Type}})
Next I also transform the DAYS_EMPLOYED column since it's in the same format as the DAYS_BIRTH column
= Table.TransformColumns(#"Changed Type2", 
    {{"DAYS_EMPLOYED", each 
        if _ > 0 then "Currently Unemployed" 
        else Text.From(Number.Round(Number.Abs(_) / 365, 1)) & " years"
    , type text}})
```
### Feature Engineering
Some of the data contain large range such as the Income Group and the Age Group therefore it is needed to group those data into several group. 
#### Income Group
```
= Table.AddColumn(#"Filtered Rows1", "INCOME_GROUP", each
    if [AMT_INCOME_TOTAL] <= 2700000 then "Very Low income"
    else if [AMT_INCOME_TOTAL] <= (2700000 + 13500000) / 2 then "Low income"
    else if [AMT_INCOME_TOTAL] <= 13500000 then "Lower-middle income"
    else if [AMT_INCOME_TOTAL] <= (13500000 + 18000000) / 2 then "Middle income"
    else if [AMT_INCOME_TOTAL] <= 18000000 then "Upper-middle income"
    else if [AMT_INCOME_TOTAL] <= (18000000 + 22950000) / 2 then "Moderate-high income"
    else if [AMT_INCOME_TOTAL] <= 22950000 then "High income"
    else "Very High income", type text)
```
#### Age Group 
```
= Table.AddColumn(#"Added Custom", "AGE_GROUP", each Table.AddColumn(#"Added Custom", "AGE_GROUP", each
    if [DAYS_BIRTH] <= 30 then "< 30"
    else if [DAYS_BIRTH] <= 40 then "31 - 40"
    else if [DAYS_BIRTH] <= 50 then "41 - 50"
    else if [DAYS_BIRTH] <= 60 then "51 - 60"
    else if [DAYS_BIRTH] <= 70 then "61 - 70"
    else "> 71", type text))
```
To increase readability, I also decided to replace the value of STATUS column and MONTHS_BALANCE column in the credit_record table 
#### Status
```
= Table.ReplaceValue(#"Previous Step",each [STATUS], each 
if [STATUS] = "0" then "1 - 29 days due" 
else if [STATUS] = "1" then "30 - 59 days due"
else if [STATUS] = "2" then "60 - 89 days due"
else if [STATUS] = "3" then "90 - 119 days due"
else if [STATUS] = "4" then "120 - 149 days due" 
else if [STATUS] = "5" then "150+ days/write off"
else if [STATUS] = "X" then "No Loan"
else if [STATUS] = "C" then "Paid Off"
else [STATUS], Replacer.ReplaceText,{"STATUS"})
```
#### MONTHS_BALANCE
```
= Table.ReplaceValue(#"Previous Step", each [MONTHS_BALANCE], each 
    if [MONTHS_BALANCE] = "0" then "Current Month"
    else if Number.FromText([MONTHS_BALANCE]) > -12 then 
        Text.From(Number.Abs(Number.FromText([MONTHS_BALANCE]))) & " Month" & (if Number.Abs(Number.FromText([MONTHS_BALANCE])) > 1 then "s" else "") & " Ago"
    else 
        Text.From(Number.RoundDown(Number.Abs(Number.FromText([MONTHS_BALANCE])) / 12)) & " Year" & (if Number.RoundDown(Number.Abs(Number.FromText([MONTHS_BALANCE])) / 12) > 1 then "s" else "") & " Ago",
    Replacer.ReplaceText, {"MONTHS_BALANCE"}
)
```

  




