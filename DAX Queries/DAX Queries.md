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
                CALCULATE(COUNT(FACT_Employee_Attrition[Emp ID]), FACT_Employee_Master[Category] <> "UR")


*      Ethnicity % =
                   [Ethnic Count] / [Manpower as on Key Date]


* Specially Abled =
                  CALCULATE (
                  COUNT ( FACT_Employee_Attrition[Emp ID] ),
                  NOT ( ISBLANK ( FACT_Employee_Master[Disability TYpe] ) )
              )

*      Specially Abled % =
                 [Disabled Count]/ [Manpower as on Key Date]


*      Minority Count =
                                    CALCULATE (
                          COUNT ( FACT_Employee_Attrition[Emp ID] ),
                          DIM_Religion_Master[Religion Code] <> 22
                      )

*      Religious Minority % =
                        [Minority Count]/ [Manpower as on Key Date]

###  Workforce Age  :

*      Age =
          DATEDIFF ( Fact Emp Master[DOB], TODAY (), YEAR )


###  Workforce Attrition  :

*      Turnover Rate =
                    VAR mpp_start =
                        COUNTX (
                            FILTER (
                                'Fact Emp Master',
                                'Fact Emp Master'[DOJ] <= FIRSTDATE ( 'Start Date Table'[Start Date] )
                                    && 'Fact Emp Master'[DOR] >= FIRSTDATE ( 'Start Date Table'[Start Date] )
                            ),
                            'Emp Master for Name'[Staff No.]
                        )
                    VAR mpp_end =
                        COUNTX (
                            FILTER (
                                'Fact Emp Master',
                                'Fact Emp Master'[DOJ] <= LASTDATE ( 'End Date Table'[End Date] )
                                    && 'Fact Emp Master'[DOR] >= LASTDATE ( 'End Date Table'[End Date] )
                            ),
                            'Fact Emp Master'[Staff No.]
                        )
                    VAR mpp_avg = ( mpp_start + mpp_end ) / 2
                    RETURN
                        COUNT ( FACT_EMP_ACTIONS[Staff No.] ) / mpp_avg


*      Voluntary TO Rate =
                    VAR mpp_start =
                        COUNTX (
                            FILTER (
                                'Fact Emp Master',
                                'EFact Emp Master'[DOJ] <= FIRSTDATE ( 'Start Date Table'[Start Date] )
                                    && 'Fact Emp Master'[DOR] >= FIRSTDATE ( 'Start Date Table'[Start Date] )
                            ),
                            'Fact Emp Master'[Staff No.]
                        )
                    VAR mpp_end =
                        COUNTX (
                            FILTER (
                                'Fact Emp Master',
                                'Fact Emp Master'[DOJ] <= LASTDATE ( 'End Date Table'[End Date] )
                                    && 'Fact Emp Master'[DOR] >= LASTDATE ( 'End Date Table'[End Date] )
                            ),
                            'Emp Master for Name'[Staff No.]
                        )
                    VAR mpp_avg = ( mpp_start + mpp_end ) / 2
                    VAR vol_attrition =
                        COUNTX (
                            FILTER (
                                FACT_EMP_ACTIONS,
                                FACT_EMP_ACTIONS[ACTION_TYPE] = "DP"
                                    || AND (
                                        FACT_EMP_ACTIONS[ACTION_TYPE] = "BR",
                                        FACT_EMP_ACTIONS[ACTION_REASON] = "N2"
                                            || FACT_EMP_ACTIONS[ACTION_REASON] = "N4"
                                    )
                                    || AND (
                                        FACT_EMP_ACTIONS[ACTION_TYPE] = "BS",
                                        FACT_EMP_ACTIONS[ACTION_REASON] = "N0"
                                            || FACT_EMP_ACTIONS[ACTION_REASON] = "N2"
                                            || FACT_EMP_ACTIONS[ACTION_REASON] = "N3"
                                    )
                            ),
                            FACT_EMP_ACTIONS[Staff No.]
                        )
                    RETURN
                        vol_attrition / mpp_avg

*        Voluntary Turnover =
                      COUNTX (
                          FILTER (
                              FACT_EMP_ACTIONS,
                              FACT_EMP_ACTIONS[ACTION_TYPE] = "DP"
                                  || AND (
                                      FACT_EMP_ACTIONS[ACTION_TYPE] = "BR",
                                      FACT_EMP_ACTIONS[ACTION_REASON] = "N2"
                                          || FACT_EMP_ACTIONS[ACTION_REASON] = "N4"
                                  )
                                  || AND (
                                      FACT_EMP_ACTIONS[ACTION_TYPE] = "BS",
                                      FACT_EMP_ACTIONS[ACTION_REASON] = "N0"
                                          || FACT_EMP_ACTIONS[ACTION_REASON] = "N2"
                                          || FACT_EMP_ACTIONS[ACTION_REASON] = "N3"
                                  )
                          ),
                          FACT_EMP_ACTIONS[Staff No.]
                      )

*          Involuntary Turnover =
                            COUNT ( FACT_EMP_ACTIONS[Staff No.] )
                                - COUNTX (
                                    FILTER (
                                        FACT_EMP_ACTIONS,
                                        FACT_EMP_ACTIONS[ACTION_TYPE] = "DP"
                                            || AND (
                                                FACT_EMP_ACTIONS[ACTION_TYPE] = "BR",
                                                FACT_EMP_ACTIONS[ACTION_REASON] = "N2"
                                                    || FACT_EMP_ACTIONS[ACTION_REASON] = "N4"
                                            )
                                            || AND (
                                                FACT_EMP_ACTIONS[ACTION_TYPE] = "BS",
                                                FACT_EMP_ACTIONS[ACTION_REASON] = "N0"
                                                    || FACT_EMP_ACTIONS[ACTION_REASON] = "N2"
                                                    || FACT_EMP_ACTIONS[ACTION_REASON] = "N3"
                                            )
                                    ),
                                    FACT_EMP_ACTIONS[Staff No.]
                                )


*        Total Turnover =
                      CALCULATE (
                          COUNT ( FACT_EMP_ACTIONS[CURR_UNIT] ),
                          ( FACT_EMP_ACTIONS[ACTION_TYPE] = "BR" )
                              || ( FACT_EMP_ACTIONS[ACTION_TYPE] = "BS" )
                              || ( FACT_EMP_ACTIONS[ACTION_TYPE] = "BT" )
                      )


*        Resignations =
                      COUNTX (
                          FILTER (
                              FACT_EMP_ACTIONS,
                              FACT_EMP_ACTIONS[ACTION_TYPE] = "BS"
                                  && FACT_EMP_ACTIONS[ACTION_REASON] = "N0"
                          ),
                          FACT_EMP_ACTIONS[Staff No.]
                      )

*        Retirements =
                      COUNTX (
                          FILTER ( FACT_EMP_ACTIONS, FACT_EMP_ACTIONS[ACTION_TYPE] = "BR" ),
                          FACT_EMP_ACTIONS[Staff No.]
                      )



*        Terminations =
                      COUNTX (
                          FILTER ( FACT_EMP_ACTIONS, FACT_EMP_ACTIONS[ACTION_TYPE] = "BT" ),
                          FACT_EMP_ACTIONS[Staff No.]
                      )

*         Deaths =
                  COUNTX (
                      FILTER (
                          FACT_EMP_ACTIONS,
                          FACT_EMP_ACTIONS[ACTION_TYPE] = "BS"
                              && FACT_EMP_ACTIONS[ACTION_REASON] = "N1"
                      ),
                      FACT_EMP_ACTIONS[Staff No.]
                  )







