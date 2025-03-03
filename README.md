# Credit-Card-Dashboard
This dashboard will contain the data visualization of a credit card datasets as well as some insights gained from the dashboard

<iframe title="CreditCardData_Dashboard" width="600" height="373.5" src="https://app.powerbi.com/view?r=eyJrIjoiNmIwZWZjZTMtNTYyZS00ZGM2LTk4MjAtMmMwMzA3YzUxN2UwIiwidCI6ImM2NmUxODMzLWY2M2UtNGI5Zi05NDc5LWZhMDdiY2NhMTAzMyIsImMiOjEwfQ%3D%3D" frameborder="0" allowFullScreen="true"></iframe>

# Data Pre-Processing
## 1. Data Transformation
During this step, the data type will be adapted based on the value of the data istself. For this step this following code using the M Language (Language that is used in Power Query) is used
= Table.TransformColumnTypes(#"Previous Step",{{"ID", Int64.Type}, {"CODE_GENDER", type text}, {"FLAG_OWN_CAR", type text}, {"FLAG_OWN_REALTY", type text}, {"CNT_CHILDREN", Int64.Type}, {"AMT_INCOME_TOTAL", Int64.Type}, {"NAME_INCOME_TYPE", type text}, {"NAME_EDUCATION_TYPE", type text}, {"NAME_FAMILY_STATUS", type text}, {"NAME_HOUSING_TYPE", type text}, {"DAYS_BIRTH", Int64.Type}, {"DAYS_EMPLOYED", Int64.Type}, {"FLAG_MOBIL", Int64.Type}, {"FLAG_WORK_PHONE", Int64.Type}, {"FLAG_PHONE", Int64.Type}, {"FLAG_EMAIL", Int64.Type}, {"OCCUPATION_TYPE", type text}, {"CNT_FAM_MEMBERS", Int64.Type}})



