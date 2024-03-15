# PBI-Patient-Healthcare-Analysis

![Dashboard Image](https://github.com/quynhnguyenuet/Dash_Board_Project/blob/main/Patients%20Healthcare%20dataset/Patient%20healthcare.PNG "Final Dashboard Image")

## Table of Contents
* [Introduction](#Introduction)
* [Dashboard Requirements](#Dashboard-Requirements)
* [Installation / Usage](#Installation--Usage)
* [DAX Formulas Used in Measures](#DAX-Formulas-Used-in-Measures)
## Introduction
* This project is aimed at developing a Power BI Dashboard for generating insights about Patients Healthcare data
* The dataset can be accessed from this link: [Hospital Data](https://github.com/quynhnguyenuet/Dash_Board_Project/blob/main/Patients%20Healthcare%20dataset/Data%20and%20Background/Hospital%20ER.csv)
## Dashboard Requirements
* 1.Average wait time: The amount of time patients typically wait before their appointment. Explore patterns and trends that shed light on the effectiveness of the healthcare system

* 2.Patient satisfaction: explore the average satisfaction scores given by patients. Learn about the factors that contribute to a positive patient experience and how it can be enhanced.

* 3.Total monthly patient visits: Get an overview of the ebb and flow of patients each month. Understand health care needs over time.

* 4.Administrative and non-administrative appointments: Drill into the data to differentiate between appointments related to administrative processes and those not related to administrative processes. Explore the impact on wait times and patient satisfaction.

* 5.Referrals and Attending Patients: Explore the balance between patients referred to specific departments and those arriving without prior referral. How does this affect the overall patient experience?

* 6.Number of patient visits by age group and race: Explore the distribution of patient visits across different age groups and races. Gain insight into the diversity of healthcare needs and preferences.
## Installation / Usage
* Install Power BI Desktop from Official [Power BI Download Site](https://powerbi.microsoft.com/en-us/downloads/)
* Download data files from link given in Introduction
* Clone/download this repository to your local machine
* Open Dashboard report file (Patient Healthcare Dashboard.pbix) in Power BI Desktop, to access the dashboard's interactivity 
## Some DAX Formulas Used in Measures
**1. Average wait time**

* Average Wait Time = AVERAGE('Patient Dataset'[patient_waittime])

**2. Patient satisfaction**

* Average Satisfaction score = CALCULATE(AVERAGE('Patient Dataset'[patient_sat_score]),'Patient Dataset'[patient_sat_score]<>BLANK())

**3. Administrative and non-administrative appointments**
(a) % Administrative

* % Administrative Schedule = DIVIDE(COUNTROWS(FILTER('Patient Dataset','Patient Dataset'[patient_admin_flag]=TRUE())),[Total Patients])

(b) % Non-Administrative

* % None - Administrative Schedule = DIVIDE(COUNTROWS(FILTER('Patient Dataset','Patient Dataset'[patient_admin_flag]=FALSE())),[Total Patients])

**4. Total monthly patient visits**

* Total Patients = COUNTROWS('Patient Dataset')

**Create Table Date**
 Date = 
 ADDCOLUMNS(
    CALENDARAUTO(),
    "Year", YEAR([Date]),
   "Month", FORMAT([Date], "mmm"),
  "WeekType", IF(WEEKDAY([Date]) = 1, "Weekend", IF(WEEKDAY([Date]) = 7, "Weekend", "Weekday")),
   "Weekday", FORMAT([Date], "ddd"),"MonthNum",MONTH([Date])
 )

**5. Referrals and Attending Patients**
* Referred Patients 

% Referred Patients = VAR _FillterPatients = CALCULATE([Total Patients],'Patient Dataset'[department_referral]<>"none") RETURN DIVIDE(_FillterPatients,[Total Patients])

* Un Referred Patients

% Un Referred Patients = VAR _FillterPatients = CALCULATE([Total Patients],'Patient Dataset'[department_referral]="none") RETURN DIVIDE(_FillterPatients,[Total Patients])

**6. Number of patient visits by age group and race**

* Create Age-Group

Age Group = VAR _PatientAge = 'Patient Dataset'[patient_age] RETURN IF(_PatientAge<=2,"Infancy",IF(_PatientAge<=6,"Early Childhood",IF(_PatientAge<=12,"Middle Childhood",IF(_PatientAge<=18,"Teenager","Adult"))))

* Create table Age-Buckets

Age Buckets = SWITCH(TRUE(),'Patient Dataset'[patient_age]<=10,"0-10",'Patient Dataset'[patient_age]<=10,"0-10",'Patient Dataset'[patient_age]<=20,"11-20",'Patient Dataset'[patient_age]<=30,"21-30",'Patient Dataset'[patient_age]<=40,"31-40",'Patient Dataset'[patient_age]<=50,"41-50",'Patient Dataset'[patient_age]<=60,"51-60",'Patient Dataset'[patient_age]<=71,"61-70","70+")
