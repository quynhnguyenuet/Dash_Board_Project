# PBI-Crime-Analysis

![Dashboard Image](https://github.com/quynhnguyenuet/Dash_Board_Project/blob/main/Crime%20Dashboard/Edit_background/Report.png "Final Dashboard Image")
## Detail
![Dashboard Detail](https://github.com/quynhnguyenuet/Dash_Board_Project/blob/main/Crime%20Dashboard/Edit_background/Details.png)
## Table of Contents
* [Introduction](#Introduction)
* [Dashboard Requirements](#Dashboard-Requirements)
* [Installation / Usage](#Installation--Usage)
* [DAX Formulas Used in Measures](#DAX-Formulas-Used-in-Measures)
## Introduction
* This project is aimed at developing a Power BI Dashboard for generating insights about Crime data
* The dataset can be accessed from this link: [Crime Data](https://github.com/quynhnguyenuet/Dash_Board_Project/tree/main/Crime%20Dashboard/Data)
## Dashboard Requirements
* 1 Total Crimes:
   - Sum of all reported crimes in the dataset.

* 2 Crime Distribution by Year and Yearly Changes:
   - Analysis of crimes categorized by year, including insights into the year-over-year changes.

* 3 Crimes by Time Range (e.g., 3:00 AM to 5:59 AM):
   - Exploration of crime occurrences within specific time intervals, providing a detailed breakdown.

* 4 Hitmap Showing Crime Distribution by Weekdays and Months:
   - Visualization using a hitmap to illustrate how crimes are distributed across weekdays and months.

* 5 Crimes by Country:
   - Examination of crimes categorized by the country where they occurred.

* 6 Total Resolved and Unresolved Crimes:
   - Distinction between resolved and unresolved crimes, offering an overview of the overall resolution rate.

* 7 Monthly Crime Trend with Percentage Variance:
   - Analysis of the monthly crime trend, accompanied by the percentage variance to highlight fluctuations.

* 8 Identification of the Most Dangerous Time of the Day:
   - Exploration to pinpoint the specific time periods during the day associated with a higher frequency of crimes
## Some DAX Formulas Used in Measures
**Total Crimes**
```dax
Total Crimes = COUNTROWS('Crimes Data')
```
**Crime Distribution by Year, Month and Yearly, Monthly Changes**
* Label Yealy Changes
```dax
Lebel (Year) = 
 VAR _PrevYer =
CALCULATE([Total Crimes],SAMEPERIODLASTYEAR(DateTable[Date])
)
VAR _YoYChange = IF(_PrevYer<> BLANK(), [Total Crimes] - _PrevYer,BLANK())
RETURN SWITCH(TRUE(),_YoYChange = 0,_YoYChange&"-",_YoYChange>=1,"▲ "&_YoYChange,_YoYChange <1,"▼ "&_YoYChange)
```
* Label Monthly Changes
```dax
Label (Month) = 
    VAR _PctMoMChange = IF (DIVIDE([Total Crimes] - [Crime PreMonth], [Crime PreMonth]) <> BLANK(),DIVIDE([Total Crimes] - [Crime PreMonth], [Crime PreMonth]))
    VAR _NegativePct = -0.01
    RETURN SWITCH(
        TRUE(),
        _PctMoMChange = 0, _PctMoMChange & "-",
        _PctMoMChange >= _NegativePct, "▲ " & FORMAT(_PctMoMChange, "0%"),
        _PctMoMChange < _NegativePct, "▼ " & FORMAT(_PctMoMChange, "0%")
    )
```
**Crimes by Time Range**
```dax
Time Group = SWITCH(TRUE(),
MOD(HOUR('Crimes Data'[Crime Time]),24)>=0 &&MOD(HOUR('Crimes Data'[Crime Time]),24)<=3,"12:00 AM - 2:59 AM",
MOD(HOUR('Crimes Data'[Crime Time]),24)>=3 &&MOD(HOUR('Crimes Data'[Crime Time]),24)<=6,"3:00 AM - 5:59 AM",
MOD(HOUR('Crimes Data'[Crime Time]),24)>=6 &&MOD(HOUR('Crimes Data'[Crime Time]),24)<=9,"6:00 AM - 8:59 AM",
MOD(HOUR('Crimes Data'[Crime Time]),24)>=9 &&MOD(HOUR('Crimes Data'[Crime Time]),24)<=12,"9:00 AM - 11:59 AM",
MOD(HOUR('Crimes Data'[Crime Time]),24)>=12 &&MOD(HOUR('Crimes Data'[Crime Time]),24)<=15,"12:00 PM - 2:59 PM",
MOD(HOUR('Crimes Data'[Crime Time]),24)>=15 &&MOD(HOUR('Crimes Data'[Crime Time]),18)<=18,"3:00 PM - 5:59 PM",
MOD(HOUR('Crimes Data'[Crime Time]),24)>=18 &&MOD(HOUR('Crimes Data'[Crime Time]),24)<=21,"6:00 PM - 8:59 PM",
MOD(HOUR('Crimes Data'[Crime Time]),24)>=21 &&MOD(HOUR('Crimes Data'[Crime Time]),24)<=24,"9:00 PM - 11:59 PM"
)
```
**Total Resolved and Unresolved Crimes**
* Resolved 
```dax
Crimes Resolved = 
        DIVIDE(CALCULATE([Total Crimes],'Crimes Data'[Resolved]="1"),[Total Crimes])
```
* Unresolved 
```dax
Crimes UnResolved = 
        DIVIDE(CALCULATE([Total Crimes],'Crimes Data'[Resolved]="0"),[Total Crimes])
```

