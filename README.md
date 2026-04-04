# Workforce-Analytics-Dashboard

##  Project Overview :

This project presents an end-to-end Workforce Analytics Dashboard built using Power BI, designed to enable data-driven decision-making across HR, finance, and leadership functions.

The solution integrates workforce data across multiple dimensions such as manpower, cost, productivity, mobility, and employee risk indicators (attrition, long leave, disciplinary actions).

The dashboard is structured to move beyond descriptive reporting and provide a decision-support system for workforce planning, cost optimization, and organizational effectiveness.

## Objective :

The objective of this project is to transform fragmented HR data into actionable insights by:

*  Providing a unified view of workforce composition, movement, and cost
*  Enabling monitoring of workforce productivity (Revenue/PAT per employee)
*  Identifying workforce risks such as attrition, long leave, and disciplinary trends
*  Supporting leadership in strategic workforce planning and optimization

##  Tools & Technologies :

*  Power BI (Data Modeling, DAX, Visualization)
*  SQL (Data extraction and transformation)
*  Excel (Data preprocessing and validation)
*  HRIS / ERP Data (Employee master, movement, payroll, disciplinary records)
*  Data Modeling: Star schema with fact tables for manpower, movement, cost, and events

## Data Model  :

###  Fact Tables  :

*  FACT_Employee_Master
    *  Emp ID
    *  Full Name
    *  Gender
    *  DOB
    *  Age
    *  Age Group
    *  DOJ
    *  DOR
    *  Grade Date
    *  Designation
    *  Category
    *  Religion
    *  Special Ability
    *  Inactive (Y/N)
    *  Unit Code
    *  Location Code
    *  Cadre Code
    *  Grade Code
    *  Function Code
    *  Department Code
 
*   FACT_Employee_Attrition
    *   Emp ID
    *   Attrition Date
    *   Attrition Type 
    *   Attrition Reason
    *   Cadre at Attrition
    *   Grade at Attrition
    *   Unit at Attrition
    *   Location at Attrition
 
*   FACT_Employee_Mobility
    *   Emp ID
    *   Transfer Date
    *   Transfer Type 
    *   Transfer Reason
    *   Cadre at Transfer
    *   Grade at Transfer
    *   From Unit
    *   To Unit
    *   From Location
    *   To Location

*   FACT_Employee_Salary
    *   Emp ID
    *   From Date
    *   To Date
    *   Basic Pay
    *   DA
    *   Stagnation
    *   Personal Pay
    *   Family Planning Inc
    *   Special Perf Allw
    *   Special Pers Pay
    *   Service Weightage
    *   Susp Allw
    *   HRA
    *   Lease
    *   Comp Qtr
    *   Non Prac Allw
    *   Cafetaria
    *   PF/Pension
    *   Deputation Allw
 

###  Dimension Tables  :

*   DIM_Unit_Master

*   DIM_Location_Master

*   DIM_Cadre_Master

*   DIM_Grade_Master

*   DIM_Function_Master

*   DIM_Unit_Master

*   DIM_Key_Date

*   DIM_From_Date

*   DIM_To_Date

##  Power BI Dashboard  :

  ###  Executive Overview  :
  *  Filters :
          Key Date/ Unit/ Location/ Function/ Cadre/ Grade
  *  Metrics :
          Real time Manpower Count
  *  Buttons  :
          Counts / Percentage %
  *  Visuals :
          Cadre-Grade wise Manpower Distribution (Column Chart)/ Unit wise Manpower Distribution (Column Chart)/ Function wise Manpower Distribution (Bar Chart)/ Unit-Location-Cadre-Grade wise Manpower Distribution (Matrix)/ Function-Cadre-Grade wise Manpower Distribution (Matrix) / Employee wise Master Date (Table)
    
  ###  Workforce Productivity  :
  *  Filters :
          Period/ Unit/ Location/ Function/ Cadre/ Grade
  *  Visuals :
          Revenue per Employee-Profit After Tax per Employee Trend (Line Chart)/ Revenue-PAT-Manpower Trend (Line Chart)/ Financial Year wise PAT per Employee & Revenue per Employee (Matrix)/ Financial Year wise PAT per Employee & Revenue per Employee (Matrix)
    
  ###  Workforce Cost Analytics  :
  *  Filters :
          Unit/ Location/ Function/ Cadre/ Grade
  *  Metrics :
          Real time Manpower Count/ Total Annual Manpower Cost(Cr)/ Average Annual Manpower Cost(Lacs)
  *  Buttons  :
          Totals/ Averages
  *  Visuals :
          Unit-Location-Cadre-Grade wise Manpower Cost (Matrix)/ Function-Cadre-Grade wise Manpower Cost (Matrix) / Cadre-Grade wise Manpower Cost (Column Chart) / Employee wise Manpower Cost Details (Table)

  ###  Workforce Diversity  :
  *  Filters :
          Unit/ Location/ Function/ Cadre/ Grade/ Gender/ Category/ Special Ability/ Religion
  *  Metrics :
          Gender wise Manpower Breakup/ Category wise Manpower Breakup/ Special Ability wise Manpower Breakup/ Religion wise Manpower Breakup
  *  Buttons  :
          Counts/ Averages
  *  Visuals :
          Unit-Cadre-Grade wise Breakup (Matrix)/ Function-Cadre-Grade wise Breakup (Matrix) / Employee wise Manpower Details (Table)

 ###  Workforce Age  :
  *  Filters :
          Unit/ Location/ Function/ Cadre/ Grade
  *  Metrics :
          Real time Manpower Count/ Average Age
  *  Visuals :
          Unit-Location wise Average Age (Matrix)/ Function wise Average Age (Matrix)/ Age Group wise Manpower Breakup (Histogram)/ Employee wise Age Details (Table)
    
  ###  Workforce Mobility  :
  *  Filters :
          From Date/ To Date/ Unit/ Cadre/ Grade
  *  Metrics :
          Transfer Ins/ Transfer Outs
  *  Visuals :
          Unit-Year wise Transfer Ins & Transfer Outs (Matrix)/ Cadre-Grade-Year wise Transfer Ins & Transfer Outs (Matrix)/ Transfer Ins & Transfer Outs Trend (Line Chart)/ Employee wise Transfer Details (Table)
    
  ###  Workforce Attrition  :
  *  Filters :
          From Date/ To Date/ Unit/ Location/ Cadre/ Grade/ Reason for Attrition
  *  Metrics :
          Turnover Rate/ Voluntary TO Rate/ Total Turnover/ Involuntary Turnover/ Voluntary Turnover/ Retirements/ Resignations/ Deaths/ Terminations
  *  Visuals :
          Turnover & Involuntary Turnover Rate Trend (Line Chart)/ Unit-Cadre-Grade-Year wise Turnover & Involuntary Turnover Rate (Matrix)/ Employee wise Attrition Details (Table)

##  Key Insights  :

## Business Impact  :

*  Enabled comprehensive workforce visibility across composition, diversity, cost, and risk dimensions
*  Supported diversity and inclusion monitoring aligned with organizational and compliance requirements
*  Provided early signals for workforce aging and succession planning risks
*  Improved decision-making by linking workforce structure with productivity and cost metrics
*  Reduced manual HR reporting effort by delivering a centralized analytics solution

## Author  :
Asad Ali



