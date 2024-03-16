# PBI-Customer-Performance-Analysis
* Light Background
![Dashboard Image_1](https://github.com/quynhnguyenuet/Dash_Board_Project/blob/main/Power%20BI%20AW/Image/Light_BG.png "Final Dashboard Image")
* Dark Background
![Dashboard Image_2]https://github.com/quynhnguyenuet/Dash_Board_Project/blob/main/Power%20BI%20AW/Image/Dark_BG.png "Final Dashboard Image")
## Table of Contents
* [Introduction](#Introduction)
* [Dashboard Requirements](#Dashboard-Requirements)
* [Installation / Usage](#Installation--Usage)
* [DAX Formulas Used in Measures](#DAX-Formulas-Used-in-Measures)
## Introduction
* This project is aimed at developing a Power BI Dashboard for generating insights about AdventureWorks data
* The dataset can be accessed from this link: [AdventureWork Data](https://github.com/quynhnguyenuet/Dash_Board_Project/blob/main/Power%20BI%20AW/AdventureWorksDW.xlsx)
## Dashboard Requirements
* 1 Calculate the average age of customers.
* 2 Determine the total count of customer base.
* 3 Identify VIP Customers, Loyal Customers, and Periodic Buyers.
* 4 Explore how to analyze revenue based on whether customers have children or not, gaining critical * insights into their preferences.
* 5 Implement a dynamic ranking system to identify and celebrate top-performing customers, boosting your business strategies.
* 6 Gain valuable insights into revenue trends by examining customer gender.
## Some DAX Formulas Used in Measures
* average age of your customers
```dax
Avg Customer Age = AVERAGE(DimCustomer[Customer Age])
```
* total count of customer
```dax
#Customer = DISTINCTCOUNT(FactTable[CustomerKey])
```
* Total Revenue
```dax
Total Revenue = SUMX(FactTable,FactTable[OrderQuantity]*RELATED(DimProduct[Price]))
```
* dynamic ranking system to identify and celebrate top-performing customers
```dax
Dynamic customer = 
    VAR _TopCustomers =
        RANKX(All(DimCustomer[Full Name]),[Total Revenue],
        ,DESC
        )
        
        RETURN
        IF(_TopCustomers<='Dynamic Customers'[Dynamic Customers Value],
            [Total Revenue]
        )
```
* Revenue based on whether customers have children or not
```dax
C Without Children Revenue2 = -- This calculate total revenue from customers with children
    SUMX(
        FILTER(
            FactTable,
            RELATED(DimCustomer[TotalChildren]) = 0),
            [Total Revenue]
        )
        
```