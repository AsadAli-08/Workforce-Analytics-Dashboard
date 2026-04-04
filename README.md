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
    *   Unit Code
    *   Unit Name   

*   DIM_Location_Master
    *   Location Code
    *   Location
    *   Site/Non Site/NCR   

*   DIM_Cadre_Master
    *   Cadre Code
    *   Cadre

*   DIM_Grade_Master
      *   Grade Code
      *   Grade

*   DIM_Function_Master
      *   Function Code
      *   Function   

*   DIM_Unit_Master
      *   Unit Code
      *   Unit

*   DIM_Key_Date
      *   Date
      *   Year
      *   Month
      *   Day

*   DIM_From_Date
      *   Date
      *   Year
      *   Month
      *   Day


*   DIM_To_Date
      * Date
      * Year
      * Month
      * Day
        
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
  *   Observations :
      *   Workforce composition is heavily skewed towards Workers, followed by Executives and Supervisors
      *   A few major units (HPBP, HEP, HEEP, HPEP, BAP, EDN, TP) together account for ~74% of total manpower, indicating concentration risk
      *   Production function dominates workforce allocation, contributing ~47% of total manpower
      *   Worker strength is largely concentrated in key units (HPBP, HPEP, HEEP, HEP), forming ~75% of total worker population
      *   Marketing (96%) and Law (90%) functions are predominantly executive-driven, reflecting role specialization
      *   Within Production, Workers form ~75% of manpower, highlighting operational intensity
                 
  ###  Workforce Productivity  :
  *  Filters :
          Period/ Unit/ Location/ Function/ Cadre/ Grade
  *  Visuals :
          Revenue per Employee-Profit After Tax per Employee Trend (Line Chart)/ Revenue-PAT-Manpower Trend (Line Chart)/ Financial Year wise PAT per Employee & Revenue per Employee (Matrix)/ Financial Year wise PAT per Employee & Revenue per Employee (Matrix)
  * Observations   :
      *   Revenue per Employee showed steady growth until 2010 (peak ~103 Cr), followed by fluctuations, with a renewed upward trend post-2020
      *   PAT per Employee remained stable until 2003, surged to a peak (~14.66 Cr), then declined sharply to a low (~8.36 Cr in 2020), and has since stabilized at lower levels (~1.83 Cr)
      *   Manpower trend declined until 2007, increased until 2012, and has been consistently declining thereafter
      *   Overall Revenue trend closely mirrors Revenue per Employee, indicating productivity-driven performance
      *   Similarly, PAT trend aligns with PAT per Employee, reinforcing the linkage between profitability and workforce efficiency
    
  ###  Workforce Cost Analytics  :
  *  Filters :
          Unit/ Location/ Function/ Cadre/ Grade
  *  Metrics :
          Real time Manpower Count/ Total Annual Manpower Cost(Cr)/ Average Annual Manpower Cost(Lacs)
  *  Buttons  :
          Totals/ Averages
  *  Visuals :
          Unit-Location-Cadre-Grade wise Manpower Cost (Matrix)/ Function-Cadre-Grade wise Manpower Cost (Matrix) / Cadre-Grade wise Manpower Cost (Column Chart) / Employee wise Manpower Cost Details (Table)
  *  Observations   :
       *    

  ###  Workforce Diversity  :
  *  Filters :
          Unit/ Location/ Function/ Cadre/ Grade/ Gender/ Category/ Special Ability/ Religion
  *  Metrics :
          Gender wise Manpower Breakup/ Category wise Manpower Breakup/ Special Ability wise Manpower Breakup/ Religion wise Manpower Breakup
  *  Buttons  :
          Counts/ Averages
  *  Visuals :
          Unit-Cadre-Grade wise Breakup (Matrix)/ Function-Cadre-Grade wise Breakup (Matrix) / Employee wise Manpower Details (Table)
  *  Observations   :
       *    

 ###  Workforce Age  :
  *  Filters :
          Unit/ Location/ Function/ Cadre/ Grade
  *  Metrics :
          Real time Manpower Count/ Average Age
  *  Visuals :
          Unit-Location wise Average Age (Matrix)/ Function wise Average Age (Matrix)/ Age Group wise Manpower Breakup (Histogram)/ Employee wise Age Details (Table)
  *  Observations   :
       *    
    
  ###  Workforce Mobility  :
  *  Filters :
          From Date/ To Date/ Unit/ Cadre/ Grade
  *  Metrics :
          Transfer Ins/ Transfer Outs
  *  Visuals :
          Unit-Year wise Transfer Ins & Transfer Outs (Matrix)/ Cadre-Grade-Year wise Transfer Ins & Transfer Outs (Matrix)/ Transfer Ins & Transfer Outs Trend (Line Chart)/ Employee wise Transfer Details (Table)
  *  Observations   :
       *    
    
  ###  Workforce Attrition  :
  *  Filters :
          From Date/ To Date/ Unit/ Location/ Cadre/ Grade/ Reason for Attrition
  *  Metrics :
          Turnover Rate/ Voluntary TO Rate/ Total Turnover/ Involuntary Turnover/ Voluntary Turnover/ Retirements/ Resignations/ Deaths/ Terminations
  *  Visuals :
          Turnover & Involuntary Turnover Rate Trend (Line Chart)/ Unit-Cadre-Grade-Year wise Turnover & Involuntary Turnover Rate (Matrix)/ Employee wise Attrition Details (Table)
  *  Observations   :
       *    

##  Key Insights  :

## Business Impact  :

*  Enabled comprehensive workforce visibility across composition, diversity, cost, and risk dimensions
*  Supported diversity and inclusion monitoring aligned with organizational and compliance requirements
*  Provided early signals for workforce aging and succession planning risks
*  Improved decision-making by linking workforce structure with productivity and cost metrics
*  Reduced manual HR reporting effort by delivering a centralized analytics solution

## Author  :
Asad Ali



