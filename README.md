# Analyzing Data Patterns Through Effective Visualization Techniques Power BI


### Table of Contents


</br>
### Tool Selection


In this assignment we were required to visualize data provided by Halifax Regional Municipality. To complete the required task effectively, we used Microsoft Power BI which is a business analytics service. We chose Power BI because it provides interactive visualizations with self-service business intelligence capabilities. Power Bi supports Data Analysis Expression (DAX) which is a library of functions and operators that can be combined to build formulas [1]. We utilized DAX for data cleaning and data manipulation. Power BI also supports real-time filtering of data which we utilized to provide better insight on the given dataset.

</br>

### Data Loading

Step 1: From the top panel click on Enter Data and select Text/CSV



Step 2: From Windows Explorer pop-up select the Building_Permits.csv [2] file.


Step 3: The loaded data is automatically formatted into Tab Delimiter CSV file.



Alternatively, user can load data using following query in query editor



    Csv.Document(File.Contents("C:\Users\amant\Downloads\Building_Permits.csv"),[Delimiter="	", Columns=30, Encoding=1252, QuoteStyle=QuoteStyle.None])

</br>

### Data Cleaning

Step 1: Data is formatted and given its data type using following query.



    Table.TransformColumnTypes(#"Promoted Headers",{{"X", type number}, {"Y", type number}, {"BP_ID", Int64.Type}, {"PERMIT_NUMBER", Int64.Type}, {"DATE_OF_APPLICATION", type datetime}, {"DATE_OF_PERMIT_ISSUANCE", type datetime}, {"PERMIT_TYPE", type text}, {"SHORT_PROJECT_DESCRIPTION", type text}, {"LONG_PROJECT_DESCRIPTION", type text}, {"CIVIC_ID", Int64.Type}, {"CIVIC_NUMBER", Int64.Type}, {"STREET_NAME", type text}, {"STREET_TYPE", type text}, {"COMMUNITY", type text}, {"PID", Int64.Type}, {"ELECTORAL_DISTRICT", Int64.Type}, {"MOST_RECENT_INSPECTION", type text}, {"PERMIT_STATUS", type text}, {"ESTIMATED_VALUE_OF_PROJECT", Int64.Type}, {"ALTERNATE_BUILDING_TYPE", type text}, {"TOTAL_SQ_FOOTAGE", Int64.Type}, {"SQFT10", Int64.Type}, {"SQFT25", Int64.Type}, {"SQFM30", Int64.Type}, {"SQFO30", Int64.Type}, {"EXIST_RES_UNITS", Int64.Type}, {"NEW_RES_UNITS", Int64.Type}, {"NUMBER_OF_STORIES", Int64.Type}, {"INTERNAL_AREA", type text}, {"FID", Int64.Type}})



Step 2: After loading and formatting data, Power BI gave an error in following rows because the data type of Permit_Number column is whole number, however, these three rows contained string values.




To clean the data we replaced the “Error” with random Permit_Number using following query. (It should noted that to avoid any issue with visualization, these three rows were filtered out while plotting.)

    Table.ReplaceValue(#"Sorted Rows","Error","64468",Replacer.ReplaceValue,{"PERMIT_NUMBER"})


Step 3: The ALTERNATE_BUILDING_TYPE column contained 71942 blank fields. To rectify this we replaced blank fields with “Unknown” as no concrete data was available to verify the content of the field. (It should noted that to avoid any issue with visualization, “Unknown” building type was filtered out.)

    
    Table.ReplaceValue(#"Sorted Rows","","Unknown",Replacer.ReplaceValue,{"ALTERNATE_BUILDING_TYPE"})


Step 4: As we were required to plot the difference in permit application date and permit issuance date, we created a column with difference in days using following query.

    
    Diff_Permit_Application_Date_(DAY) = DATEDIFF(Building_Permits[DATE_OF_APPLICATION],Building_Permits[DATE_OF_PERMIT_ISSUANCE],DAY)
    

</br>

### Dashboard


**URL:** 

[Click here](!https://app.powerbi.com/view?r=eyJrIjoiZGQzNGFhZjktOTQxNS00OTdhLTlkZDMtMTBhYTRhYzBlMmU4IiwidCI6ImQ3OTA5NTVjLTc5MDMtNDc1NC04NDJiLTMyNTAzZDliNmVkYiIsImMiOjEwfQ%3D%3D)

</br>

### Output

#### 1. GeoSpatial Visualization

**Analysis**

Total number of permit is 84,777 across all 192 communities. The distribution ranges from one (Beaver Dam, Chaplin and Ecum Secum) to 25,576 (Halifax), a difference of 25,575, averaging 441.55. The distribution is positively skewed as the average of 441.55 is much greater than the median 45. Total number of permits is highly concentrated with just 19 of the 192 communities (9.9%) representing 83% of the total. Halifax accounts for 30% of overall number of permits. Halifax (25,576) is almost 58 times bigger than the average across the 192 communities. Dartmouth and Bedford stood out with very high number of permits.


#### 2. Count of Permit per Type

**Analysis**

There are total 84,777 permits across all six permit types. The minimum value is 374 (Blasting) and the maximum is 65,304 (Standardap), a difference of 64,930, averaging 14,130. The count of permits is highly concentrated with just one of the six types (17%) representing 77% of the total. Standardap accounts for 77% of total number of permits. Standardap (65,304) is almost five times bigger than the average across the six types.


#### 3. Maximum Time Gap For Various Permit Type

**Analysis:**

We plotted the maximum days difference in permit application date and permit issuance date  by Community and by Permit Type. We filtered out communities with less than 5 days difference. The sum of maximum of difference in permit application date and permit is 101608 days across all 177 communities. The distribution ranges from 12 days  (Brookvale) to 7365 days(Dartmouth), a difference of 7 7353 days, averaging 574 days. The top six communities represent over a quarter (26%) of overall value. The top 3 communities based on total maximum days difference across all permit types are discussed in details below:




