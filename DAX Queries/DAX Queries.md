# Workforce Analytics Dashboard  : DAX Queries

###  Executive Overview  :


*     Manpower as on Key Date =

                VAR Key_Date =
                    LASTDATE ( DIM_Key_Date[Date] )
                RETURN
                    COUNTX (
                        FILTER (
                            'FACT_Employee_Master',
                            'FACT_Employee_Master'[DOJ] <= Key_Date
                                && 'FACT_Employee_Master'[DOR] > Key_Date
                        ),
                        'FACT_Employee_Master'[Emp ID]
                    )


###  Workforce Productivity  :



*      PAT as on Key date =
                SUMX (
                    FILTER (
                        'FACT_COMPANY_REVENUE',
                        'ACT_COMPANY_REVENUE'[Start Date] <= LASTDATE ( Key_Date[Date] )
                            && 'FACT_COMPANY_REVENUE'[End Date] >= LASTDATE ( Key_Date[Date] )
                    ),
                    'BHEL TO'[PAT]
                )


*      PAT per Employee =
                'FACT_COMPANY_REVENUE'[PAT as on Key_date] * 100 / 'Fact Emp Master'[Manpower as on Key Date]

*      Revenue as on Key date =
                SUMX (
                    FILTER (
                        'FACT_COMPANY_REVENUE',
                        'FACT_COMPANY_REVENUE'[Start Date] <= LASTDATE ( Key_Date[Date] )
                            && 'FACT_COMPANY_REVENUE'[End Date] >= LASTDATE ( Key_Date[Date] )
                    ),
                    'FACT_COMPANY_REVENUE'[Revenue]
                )



*      Revenue per Employee =
                'FACT_COMPANY_REVENUE'[Revenue as on Key date] * 100 / 'Fact Emp Master'[Manpower as on Key Date]



###  Workforce Cost Analytics  :

*      Monthly Gross Pay =
              FACT_EMP_SALARY[Basic Pay] + FACT_EMP_SALARY[DA] + FACT_EMP_SALARY[Cafetaria] + FACT_EMP_SALARY[HRA] + FACT_EMP_SALARY[PF/Pension] +    FACT_EMP_SALARY[Basic Misc] + FACT_EMP_SALARY[Dep Allw] + FACT_EMP_SALARY[Susp Allw] + FACT_EMP_SALARY[Other Ret. Benefits]

*      Annual Gross Pay =
                FACT_EMP_SALARY[Monthly Gross Pay] * 12


     
###  Workforce Diversity  :

*      Female Count =
                CALCULATE (
                    COUNT ( FACT_Employee_Master[Emp ID] ),
                    FACT_Employee_Master[Gender] = "F"
                )


*      Female % =

                [Female Count] / [Manpower as on Key Date]

*      Ethnic Count =
                  CALCULATE (
                      COUNT ( FACT_Employee_Master[Emp ID] ),
                      FACT_Employee_Master[Category] <> "UR"
                  )


*      Ethnicity % =
                   [Ethnic Count] / [Manpower as on Key Date]


*       Specially Abled =
                      CALCULATE (
                          COUNT ( FACT_Employee_Master[Emp ID] ),
                          NOT ( ISBLANK ( FACT_Employee_Master[Disability TYpe] ) )
                      )

*      Specially Abled % =
                 [Disabled Count]/ [Manpower as on Key Date]


*      Minority Count =
                      CALCULATE (
                          COUNT ( FACT_Employee_Master[Emp ID] ),
                          DIM_Religion_Master[Religion Code] <> 22
                      )

*      Religious Minority % =
                        [Minority Count]/ [Manpower as on Key Date]

###  Workforce Age  :

*      Age =
          DATEDIFF ( Fact Emp Master[DOB], TODAY (), YEAR )


###  Workforce Attrition  :

*        Total Turnover =
                    CALCULATE (
                        COUNT ( FACT_Employee_Attrition[To Unit Code] ),
                        ( FACT_Employee_Attrition[Action Type] = "BR" )
                            || ( FACT_Employee_Attrition[Action Type] = "BS" )
                            || ( FACT_Employee_Attrition[Action Type] = "BT" ),
                        FACT_Employee_Attrition[Attrition Date] >= MIN ( DIM_Key_Date[Date] ),
                        FACT_Employee_Attrition[Attrition Date]
                            <= MAX ( FACT_Employee_Attrition[Attrition Date] )
                    )
   
*        Voluntary Turnover =
                      CALCULATE (
                          COUNT ( FACT_Employee_Attrition[Emp ID] ),
                          FACT_Employee_Attrition[Action Type] = "DP"
                              || AND (
                                  FACT_Employee_Attrition[Action Type] = "BR",
                                  FACT_Employee_Attrition[Reason Code] = "N2"
                                      || FACT_Employee_Attrition[Reason Code] = "N4"
                              )
                              || AND (
                                  FACT_Employee_Attrition[Action Type] = "BS",
                                  FACT_Employee_Attrition[Reason Code] = "N0"
                                      || FACT_Employee_Attrition[Reason Code] = "N2"
                                      || FACT_Employee_Attrition[Reason Code] = "N3"
                              ),
                          FACT_Employee_Attrition[Attrition Date] >= MIN ( DIM_Key_Date[Date] ),
                          FACT_Employee_Attrition[Attrition Date] <= MAX ( DIM_Key_Date[Date] )
                      )
*        Avg Manpower =
                  VAR mpp_start =
                      COUNTX (
                          FILTER (
                              'FACT_Employee_Master',
                              'FACT_Employee_Master'[DOJ] <= FIRSTDATE ( 'DIM_Key_Date'[Date] )
                                  && 'FACT_Employee_Master'[DOR] >= FIRSTDATE ( 'DIM_Key_Date'[Date] )
                          ),
                          'FACT_Employee_Master'[Emp ID]
                      )
                  VAR mpp_end =
                      COUNTX (
                          FILTER (
                              'FACT_Employee_Master',
                              'FACT_Employee_Master'[DOJ] <= LASTDATE ( 'DIM_Key_Date'[Date] )
                                  && 'FACT_Employee_Master'[DOR] >= LASTDATE ( 'DIM_Key_Date'[Date] )
                          ),
                          'FACT_Employee_Master'[Emp ID]
                      )
                  VAR mpp_avg = ( mpp_start + mpp_end ) / 2
                  RETURN
                      mpp_avg
*        Turnover Rate =
                [Total Turnover] / [Avg Manpower]


*        Voluntary TO Rate = 
                        [Voluntary Turnover]/ [Avg Manpower]


*          Involuntary Turnover =
                                COUNT ( FACT_Employee_Attrition[Emp ID] )
                                    - COUNTX (
                                        FILTER (
                                            FACT_Employee_Attrition,
                                            FACT_Employee_Attrition[Action Type] = "DP"
                                                || AND (
                                                    FACT_Employee_Attrition[Action Type] = "BR",
                                                    FACT_Employee_Attrition[Reason Code] = "N2"
                                                        || FACT_Employee_Attrition[Reason Code] = "N4"
                                                )
                                                || AND (
                                                    FACT_Employee_Attrition[Action Type] = "BS",
                                                    FACT_Employee_Attrition[Reason Code] = "N0"
                                                        || FACT_Employee_Attrition[Reason Code] = "N2"
                                                        || FACT_Employee_Attrition[Reason Code] = "N3"
                                                )
                                        ),
                                        FACT_Employee_Attrition[Emp ID]
                                    )



*        Resignations =
                      COUNTX (
                          FILTER (
                              FACT_Employee_Attrition,
                              FACT_Employee_Attrition[Action Type] = "BS"
                                  && FACT_Employee_Attrition[Reason Code] = "N0"
                          ),
                          FACT_Employee_Attrition[Emp ID]
                      )
                      )

*        Retirements =
                      COUNTX (
                          FILTER ( FACT_Employee_Attrition, FACT_Employee_Attrition[Action Type] = "BR" ),
                          FACT_Employee_Attrition[Emp ID]
                      )


*        Terminations =
                      COUNTX (
                          FILTER ( FACT_Employee_Attrition, FACT_Employee_Attrition[Action Type] = "BT" ),
                          FACT_Employee_Attrition[Emp ID]
                      )

*         Deaths =
                  COUNTX (
                      FILTER (
                          FACT_Employee_Attrition,
                          FACT_Employee_Attrition[Action Type] = "BS"
                              && FACT_Employee_Attrition[Reason Code] = "N1"
                      ),
                      FACT_Employee_Attrition[Emp ID]
                  )







