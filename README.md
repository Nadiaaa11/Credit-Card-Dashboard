# Credit-Card-Dashboard
This dashboard will contain the data visualization of a credit card datasets as well as some insights gained from the dashboard
The datasets is obtained through kaggle and can be accessed from this link:
https://www.kaggle.com/datasets/rikdifos/credit-card-approval-prediction
# Data Pre-Processing 
## 1. Data Cleaning
In this dataset, in the OCCUPATION_TYPE column there are some missing values. Therefore, we need to fill it, in this case since the column contain the occupation type of the credit card holder and based on some consideration like the income, the education level, the marrital status, I decided to fill the missing value with Low Income Unspecified
= Table.ReplaceValue(#"Previous Step","","Low Income Unspecified",Replacer.ReplaceValue,{"OCCUPATION_TYPE"})
## 1. Data Transformation
During this step, the data type will be adapted based on the value of the data istself. For this step this following code using the M Language (Language that is used in Power Query) is used
= Table.TransformColumnTypes(#"Previous Step",{{"ID", Int64.Type}, {"CODE_GENDER", type text}, {"FLAG_OWN_CAR", type text}, {"FLAG_OWN_REALTY", type text}, {"CNT_CHILDREN", Int64.Type}, {"AMT_INCOME_TOTAL", Int64.Type}, {"NAME_INCOME_TYPE", type text}, {"NAME_EDUCATION_TYPE", type text}, {"NAME_FAMILY_STATUS", type text}, {"NAME_HOUSING_TYPE", type text}, {"DAYS_BIRTH", Int64.Type}, {"DAYS_EMPLOYED", Int64.Type}, {"FLAG_MOBIL", Int64.Type}, {"FLAG_WORK_PHONE", Int64.Type}, {"FLAG_PHONE", Int64.Type}, {"FLAG_EMAIL", Int64.Type}, {"OCCUPATION_TYPE", type text}, {"CNT_FAM_MEMBERS", Int64.Type}})
However, when i try to use this code the CNT_FAM_MEMBERS does not get transformed properly so i decided to keep the original data type and add this code after executing those code
= Table.AddColumn(AGE_GROUP, "First Characters", each Text.Start([CNT_FAM_MEMBERS], 1), type text)
The next thing that I do is to transform the DAYS_BIRTH column to year format, since the original value of the data is in days format. After that I transform the data type into whole number 
= Table.TransformColumns(#"Previous Step", {{"DAYS_BIRTH", each _ / 365, type number}})
= Table.TransformColumnTypes(#"Previous Step",{{"DAYS_BIRTH", Int64.Type}})
Next I also transform the DAYS_EMPLOYED column since it's in the same format as the DAYS_BIRTH column
= Table.TransformColumns(#"Changed Type2", 
    {{"DAYS_EMPLOYED", each 
        if _ > 0 then "Currently Unemployed" 
        else Text.From(Number.Round(Number.Abs(_) / 365, 1)) & " years"
    , type text}})
### Feature Engineering
Some of the data contain large range such as the Income Group and the Age Group therefore it is needed to group those data into several group. 
Income Group
= Table.AddColumn(#"Filtered Rows1", "INCOME_GROUP", each
    if [AMT_INCOME_TOTAL] <= 2700000 then "Very Low income"
    else if [AMT_INCOME_TOTAL] <= (2700000 + 13500000) / 2 then "Low income"
    else if [AMT_INCOME_TOTAL] <= 13500000 then "Lower-middle income"
    else if [AMT_INCOME_TOTAL] <= (13500000 + 18000000) / 2 then "Middle income"
    else if [AMT_INCOME_TOTAL] <= 18000000 then "Upper-middle income"
    else if [AMT_INCOME_TOTAL] <= (18000000 + 22950000) / 2 then "Moderate-high income"
    else if [AMT_INCOME_TOTAL] <= 22950000 then "High income"
    else "Very High income", type text)

Age Group 
= Table.AddColumn(#"Added Custom", "AGE_GROUP", each Table.AddColumn(#"Added Custom", "AGE_GROUP", each
    if [DAYS_BIRTH] <= 30 then "< 30"
    else if [DAYS_BIRTH] <= 40 then "31 - 40"
    else if [DAYS_BIRTH] <= 50 then "41 - 50"
    else if [DAYS_BIRTH] <= 60 then "51 - 60"
    else if [DAYS_BIRTH] <= 70 then "61 - 70"
    else "> 71", type text))

  




