# PWC-Call-Center-Analysis

Dashboard Link - [https://app.powerbi.com/groups/me/reports/2fb98590-42f4-4b51-be44-6c55404ed16f/b6e7faa788c2079b6b27?experience=power-bi&clientSideAuth=0](https://app.powerbi.com/groups/me/reports/b498e24f-5fd4-4cc0-b1b8-cb0c448d1d99/817f1e0990175abe7fcc?experience=power-bi)

## Problem Statement

The call center is trying to under stand the trend of DSATS and CSATS and take action to reduce DSAT score


### Steps followed 

- Step 1 : Load data into Power BI Desktop, dataset is a excel file.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset" to check the empty and error percentage.
- Step 4 : It was observed that Columns "Speed of calls answered in seconds" and "Satisfaction Rating" had emplty cells
- Step 5 : Visual filters (Slicers) were added for Agent, Topic, Month and Day field.
- Step 6 : Metrics table was used as a table to show the Total Calls, Call Abandoned %, Call Resolved %, Avg Speed Answered, CSAT % for each agent
- Step 7 : Bar graph is used to show the ratings
- Step 8 : Line Graph is used to show the call volume trend by hour and day of the week

- Step 9 : For creating new measures below DAX query was used

Average Call handling Time 
```DAX
Average Call handling Time = 
VAR total_duration= SUM(Call_Center[talk duration])
var number_of_calls_answered = CALCULATE(COUNT(Call_Center[call id]) ,(Call_Center[answered (Y/N)] = "Y"))
RETURN DIVIDE(total_duration,number_of_calls_answered,0)
```

Average CSAT
```DAX
Avg CSAT = 
var nume = SUM(Call_Center[satisfaction rating])
var deno = COUNT(Call_Center[satisfaction rating])
var sat=DIVIDE(nume,deno)
RETURN sat
```

Avg_Speed_Answered
```DAX
Avg_Speed_Answered = 
var toal_speed_answered = calculate(SUM(Call_Center[speed of answer in seconds]))
var number_of_calls = CALCULATE(COUNT(Call_Center[call id]) ,(Call_Center[answered (Y/N)] = "Y"))
Return DIVIDE(toal_speed_answered,number_of_calls)
```

Call Abandoned %
```DAX
Call Abandoned % = 
var Total_calls_abandoned = CALCULATE(COUNT(Call_Center[call id]),Call_Center[answered (Y/N)]="N")
var total_calls=CALCULATE(COUNT(Call_Center[call id]))
RETURN
    DIVIDE(Total_calls_abandoned,total_calls,0)
```

Call Resolved % = 
```DAX
Call Resolved % = 
var Total_resolved_call = CALCULATE(COUNT(Call_Center[call id]),Call_Center[resolved]="Y")
var total_calls=CALCULATE(COUNT(Call_Center[call id]),Call_Center[answered (Y/N)]="Y")
RETURN
    DIVIDE(Total_resolved_call,total_calls,0)
```

CSAT
```DAX
CSAT = 
VAR total_rating = divide((SUM(Call_Center[satisfaction rating])),(5*(COUNT(Call_Center[satisfaction rating]))))
RETURN total_rating
```

Last Call Time
```DAX
Last Call Time = MAX(Call_Center[Date Time])
```

Total Calls
```DAX
Total Calls = DISTINCTCOUNT(Call_Center[call id])
```
 
# Snapshot of Dashboard (Power BI Service)

![image](https://github.com/user-attachments/assets/096b4775-3c2b-4eb3-853e-59976523f046)



 
 # Report Snapshot (Power BI DESKTOP)

![image](https://github.com/user-attachments/assets/dbe9e48f-8c4e-42e4-ad54-dba9b86671a3)


# Insights

A single page report was created on Power BI Desktop with 2 tooltips & it was then published to Power BI Service.

Following inferences can be drawn from the dashboard;

### [1] Total Calls Analysed 

   Total for Q1 = 5000

### [2] Calls Abandoned

   Total Abandoned Calls = 946
   
    We notice that close to 950 (19%) of the calls were abandoned 
   
### [3] Average time taken to answer the call

   Average time taken to answer the call = 67.52
   
   We can observe that even though the talk time of these calls are significantly lower and their issue was resolved, customers were still Very dissatisfied and can suspect that the Avg time to answer the call could be the reason

           
###  [4] Call volume by hour

There is high call inflow during 11 AM, 1 PM and 5 PM  
  
### [5] Top 3 Call Volume by days

  Monday - 770
  Saturday - 768
  Sunday - 716

### [6] Bottom 3 Call Volume by days

  Tuesday - 675
  Wednesday - 679
  Friday - 680

### Staff Assignment

From the call volume by days we can note that
* Saturday, Sunday and Monday is the busiest at the call center and need full support staff
* Tuesday, Webnesday and Friday has a lower call inflow and can plan for agents weekoff and leaves
  
 With this data the manager can allocate more employee between Saturday and Monday to reduce Average time to answer thus leading to customer happiness

 ## Analysis

1. With the above dashboard we can determine that Saturday, Sunday and Monday is the busiest and Tuesday, Webnesday and Friday has a lower call inflo and manager can use this data to plan shift schedule thus reducing the average handle time
 
2. Manager/Leadership can also determine the agents with low CSAT score and high Call handling time and take necessary action

3. As the Average time to answer is over a minute it is recommended to hire more employees, to reduce both tIme taken to answer and abandoned calls.

## Author - Venkatesh Varma V

This project is part of my portfolio, showcasing my Power BI skills essential for Data Analyst roles.
