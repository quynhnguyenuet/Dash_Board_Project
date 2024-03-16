# PBI-Tranportation-Analysis

![Dashboard Image](https://github.com/quynhnguyenuet/Dash_Board_Project/blob/main/Transportation%20Resources/Image/Report_1.PNG"Final Dashboard Image")
* 
![Dashboard Image](https://github.com/quynhnguyenuet/Dash_Board_Project/blob/main/Transportation%20Resources/Image/Report_2.PNG"Final Dashboard Image")

## Table of Contents
* [Introduction](#Introduction)
* [Dashboard Requirements](#Dashboard-Requirements)
* [Installation / Usage](#Installation--Usage)
* [DAX Formulas Used in Measures](#DAX-Formulas-Used-in-Measures)
## Introduction
* This project is aimed at developing a Power BI Dashboard for generating insights about Tranportation Resources data
* The dataset can be accessed from this link: [Tranportation Data](https://github.com/quynhnguyenuet/Dash_Board_Project/tree/main/Transportation%20Resources/Data)
## Dashboard Requirements
* 1 Total Passenger Count

* 2 Average Passenger Count per Journey

* 3 Trend Analysist of Trips Over Time

* 4 Most Active Operating Hour

* 5 Least Active Operating Hour

* 6 Distribution of Trip Across Diffrent Time Ranges

* 7 Annual Rider Distribution Analysis

* 8 Monthly Rider Trends

* 9 Weeday Patterns in Rider Numbers

* 10 Classification of Bus Usage

* 11 Route with Highest Passenger Traffic

* 12 Route with Lowest Passenger Traffic

* 13 Total Revenue

* 14 Passenger by Gender

* 15 Route Distribution by Riders

* 16 Occupation Distribution by Riders

* 17 Age-Group Distribution by Riders
## Some DAX Formulas Used in Measures
* Total Passenger Count
```dax
Total Riders (Passengers) = SUM(FactTable_ridership[NumberOfRiders])
```
*  Average Passenger Count per Journey
```dax
Avg. Rider Per Trip = 
    DIVIDE([Total Riders (Passengers)],COUNTROWS(FactTable_ridership))
```
*  Trend Analysist of Trips Over Time
Create new colunm TimeGroup
```dax
TimeGroup = 
    VAR _Time = 
        MOD(HOUR(FactTable_ridership[Time]),24)
    RETURN
        SWITCH(TRUE(),_Time<3,"12:00 AM - 2:59 AM",
                      _Time<6,"3:00 AM - 5:59 AM",
                      _Time<9,"6:00 AM - 8:59 AM",
                      _Time<12,"9:00 AM - 11:59 AM",
                      _Time<15,"12:00 PM - 2:59 PM",
                      _Time<18,"3:00 PM - 5:59 PM",
                      _Time<21,"6:00 PM - 8:59 PM",
                      "9:00 PM - 11:59 PM"
                      )
    
```
* Most Active Operating Hour
```dax
Peak Hour of Operation = 
    VAR _Total_Passengers_Per_Hour =
    SUMMARIZE(FactTable_ridership,FactTable_ridership[Time],"TotalPassengers",SUM(FactTable_ridership[NumberOfRiders])
    )
VAR _PeakTime = TOPN(1,_Total_Passengers_Per_Hour,[TotalPassengers],DESC)
RETURN MAXX(_PeakTime,FactTable_ridership[Time])
```
* Least Active Operating Hour
```dax
Down Hour of Operation = 
    VAR _Total_Passengers_Per_Hour =
    SUMMARIZE(FactTable_ridership,FactTable_ridership[Time],"TotalPassengers",SUM(FactTable_ridership[NumberOfRiders])
    )
VAR _PeakTime = TOPN(1,_Total_Passengers_Per_Hour,[TotalPassengers],ASC)
RETURN MAXX(_PeakTime,FactTable_ridership[Time])
```
* Route with Highest Passenger Traffic
```dax
Busiest Route = 
VAR _Total_Passengers_Per_Route =
    SUMMARIZE(Lookup_routes,Lookup_routes[RouteName],"TotalPassengers",SUM(FactTable_ridership[NumberOfRiders])
    )
VAR _TopRoute = TOPN(1,_Total_Passengers_Per_Route,[TotalPassengers],DESC)
RETURN MAXX(_TopRoute,Lookup_routes[RouteName])
```
* Route with Lowest Passenger Traffic
```dax
Least Busy Route = 
VAR _Total_Passengers_Per_Route =
    SUMMARIZE(Lookup_routes,Lookup_routes[RouteName],"TotalPassengers",SUM(FactTable_ridership[NumberOfRiders])
    )
VAR _TopRoute = TOPN(1,_Total_Passengers_Per_Route,[TotalPassengers],ASC)
RETURN MAXX(_TopRoute,Lookup_routes[RouteName])
```
* Total Revenue 
```dax
Total Revenue = 
    SUMX(FactTable_ridership,RELATED(Lookup_routes[TripFee])*FactTable_ridership[NumberOfRiders])
```
* Age-Group Distribution by Riders
```dax
   Age-Bucke = 
    VAR _Age = Lookup_demographics[Age]
    VAR _BucketStart = FLOOR(Lookup_demographics[Age],10)
    VAR _BucketEnd = IF(_BucketStart>=70,"+",_BucketStart+9)
    RETURN
        IF(_BucketStart>=70,"70+",CONCATENATE(_BucketStart,CONCATENATE("-",_BucketEnd))
        )
```
