# Workforce Analytics Dashboard  : DAX Queries

###  Executive Overview  :


*     Manpower as on Key Date =
                VAR key_date =
                    LASTDATE ( Key_Date[Date] )
                RETURN
                    IF (
                        key_date == TODAY (),
                        COUNTX (
                            FILTER (
                                'Fact Emp Master',
                                'Fact Emp Master'[DOJ] <= key_date
                                    && 'Fact Emp Master'[DOR] >= key_date
                                    && 'Fact Emp Master'[EMP_DEFUNCT] == "N"
                            ),
                            'Fact Emp Master'[Staff No.]
                        ),
                        COUNTX (
                            FILTER (
                                'Fact Emp Master',
                                'Fact Emp Master'[DOJ] <= key_date
                                    && 'Fact Emp Master'[DOR] >= key_date
                            ),
                            'Fact Emp Master'[Staff No.]
                        )
                    )


###  Workforce Productivity  :



*      PAT as on Key date =
                SUMX (
                    FILTER (
                        'BHEL TO',
                        'BHEL TO'[Start Date] <= LASTDATE ( Key_Date[Date] )
                            && 'BHEL TO'[End Date] >= LASTDATE ( Key_Date[Date] )
                    ),
                    'BHEL TO'[PAT]
                )


*      PAT per Employee =
                'BHEL TO'[PAT as on Key_date] * 100 / 'Fact Emp Master'[Manpower as on Key Date]

*      Revenue as on Key date =
                SUMX (
                    FILTER (
                        'BHEL TO',
                        'BHEL TO'[Start Date] <= LASTDATE ( Key_Date[Date] )
                            && 'BHEL TO'[End Date] >= LASTDATE ( Key_Date[Date] )
                    ),
                    'BHEL TO'[Revenue]
                )



*      Revenue per Employee =
                'BHEL TO'[Revenue as on Key date] * 100 / 'Fact Emp Master'[Manpower as on Key Date]



###  Workforce Cost Analytics  :

*      Monthly Gross Pay =
              PIS_EMP_BASIC_PAY[Basic Pay] + PIS_EMP_BASIC_PAY[DA] + PIS_EMP_BASIC_PAY[Cafetaria] + PIS_EMP_BASIC_PAY[HRA] + PIS_EMP_BASIC_PAY[PF/Pension] +    PIS_EMP_BASIC_PAY[Basic Misc] + PIS_EMP_BASIC_PAY[Dep Allw] + PIS_EMP_BASIC_PAY[Susp Allw] + PIS_EMP_BASIC_PAY[Other Ret. Benefits]

*      Annual Gross Pay =
                PIS_EMP_BASIC_PAY[Monthly Gross Pay] * 12


     
###  Workforce Diversity  :

*      Female % =
              DIVIDE (
                  CALCULATE (
                      COUNT ( Fact Emp Master[Staff No.] ),
                      KEEPFILTERS ( Fact Emp Master[Gender] = "Female" )
                  ),
                  CALCULATE (
                      COUNT ( Fact Emp Master[Staff No.] ),
                      REMOVEFILTERS ( Fact Emp Master[Gender] )
                  ),
                  0
              )

*      Ethnicity % =
              DIVIDE (
                  CALCULATE (
                      COUNT ( Fact Emp Master[Staff No.] ),
                      KEEPFILTERS ( Fact Emp Master[Category] <> "Gen" )
                  ),
                  CALCULATE (
                      COUNT ( Fact Emp Master[Staff No.] ),
                      REMOVEFILTERS ( Fact Emp Master[Category] )
                  ),
                  0
              )


*      Specially Abled % =
              DIVIDE (
                  CALCULATE (
                      COUNT ( Fact Emp Master[Staff No.] ),
                      KEEPFILTERS ( Fact Emp Master[PHYSICALLY_CHALLENGE] = "Yes" )
                  ),
                  CALCULATE (
                      COUNT ( Fact Emp Master[Staff No.] ),
                      REMOVEFILTERS ( Fact Emp Master[Special Ability] )
                  ),
                  0
              )


*      Religious Minority % =
              DIVIDE (
                  CALCULATE (
                      COUNT ( Fact Emp Master[Staff No.] ),
                      KEEPFILTERS ( Fact Emp Master[Religion] <> "Hinduism" )
                  ),
                  CALCULATE (
                      COUNT ( Fact Emp Master[Staff No.] ),
                      REMOVEFILTERS ( Fact Emp Master[Religion] )
                  ),
                  0
              )

###  Workforce Age  :

*      Age =
          DATEDIFF ( Fact Emp Master[DOB], TODAY (), YEAR )


###  Workforce Attrition  :

*      Turnover Rate =
                    VAR mpp_start =
                        COUNTX (
                            FILTER (
                                'Emp Master for Name',
                                'Emp Master for Name'[DOJ] <= FIRSTDATE ( 'Start Date Table'[Start Date] )
                                    && 'Emp Master for Name'[DOR] >= FIRSTDATE ( 'Start Date Table'[Start Date] )
                            ),
                            'Emp Master for Name'[Staff No.]
                        )
                    VAR mpp_end =
                        COUNTX (
                            FILTER (
                                'Emp Master for Name',
                                'Emp Master for Name'[DOJ] <= LASTDATE ( 'End Date Table'[End Date] )
                                    && 'Emp Master for Name'[DOR] >= LASTDATE ( 'End Date Table'[End Date] )
                            ),
                            'Emp Master for Name'[Staff No.]
                        )
                    VAR mpp_avg = ( mpp_start + mpp_end ) / 2
                    RETURN
                        COUNT ( PIS_EMP_ACTIONS_DET[Staff No.] ) / mpp_avg


*      Voluntary TO Rate =
                    VAR mpp_start =
                        COUNTX (
                            FILTER (
                                'Emp Master for Name',
                                'Emp Master for Name'[DOJ] <= FIRSTDATE ( 'Start Date Table'[Start Date] )
                                    && 'Emp Master for Name'[DOR] >= FIRSTDATE ( 'Start Date Table'[Start Date] )
                            ),
                            'Emp Master for Name'[Staff No.]
                        )
                    VAR mpp_end =
                        COUNTX (
                            FILTER (
                                'Emp Master for Name',
                                'Emp Master for Name'[DOJ] <= LASTDATE ( 'End Date Table'[End Date] )
                                    && 'Emp Master for Name'[DOR] >= LASTDATE ( 'End Date Table'[End Date] )
                            ),
                            'Emp Master for Name'[Staff No.]
                        )
                    VAR mpp_avg = ( mpp_start + mpp_end ) / 2
                    VAR vol_attrition =
                        COUNTX (
                            FILTER (
                                PIS_EMP_ACTIONS_DET,
                                PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "DP"
                                    || AND (
                                        PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "BR",
                                        PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N2"
                                            || PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N4"
                                    )
                                    || AND (
                                        PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "BS",
                                        PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N0"
                                            || PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N2"
                                            || PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N3"
                                    )
                            ),
                            PIS_EMP_ACTIONS_DET[Staff No.]
                        )
                    RETURN
                        vol_attrition / mpp_avg

  *        Voluntary Turnover =
                      COUNTX (
                          FILTER (
                              PIS_EMP_ACTIONS_DET,
                              PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "DP"
                                  || AND (
                                      PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "BR",
                                      PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N2"
                                          || PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N4"
                                  )
                                  || AND (
                                      PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "BS",
                                      PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N0"
                                          || PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N2"
                                          || PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N3"
                                  )
                          ),
                          PIS_EMP_ACTIONS_DET[Staff No.]
                      )

*          Involuntary Turnover =
                            COUNT ( PIS_EMP_ACTIONS_DET[Staff No.] )
                                - COUNTX (
                                    FILTER (
                                        PIS_EMP_ACTIONS_DET,
                                        PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "DP"
                                            || AND (
                                                PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "BR",
                                                PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N2"
                                                    || PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N4"
                                            )
                                            || AND (
                                                PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "BS",
                                                PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N0"
                                                    || PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N2"
                                                    || PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N3"
                                            )
                                    ),
                                    PIS_EMP_ACTIONS_DET[Staff No.]
                                )


*      Total Turnover =
                      CALCULATE (
                          COUNT ( PIS_EMP_ACTIONS_DET[CURR_UNIT] ),
                          ( PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "BR" )
                              || ( PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "BS" )
                              || ( PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "BT" )
                      )


*       Resignations =
                      COUNTX (
                          FILTER (
                              PIS_EMP_ACTIONS_DET,
                              PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "BS"
                                  && PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N0"
                          ),
                          PIS_EMP_ACTIONS_DET[Staff No.]
                      )

*      Retirements =
                      COUNTX (
                          FILTER ( PIS_EMP_ACTIONS_DET, PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "BR" ),
                          PIS_EMP_ACTIONS_DET[Staff No.]
                      )


*    Terminations =
                      COUNTX (
                          FILTER ( PIS_EMP_ACTIONS_DET, PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "BT" ),
                          PIS_EMP_ACTIONS_DET[Staff No.]
                      )

*        Deaths =
                  COUNTX (
                      FILTER (
                          PIS_EMP_ACTIONS_DET,
                          PIS_EMP_ACTIONS_DET[ACTION_TYPE] = "BS"
                              && PIS_EMP_ACTIONS_DET[ACTION_REASON] = "N1"
                      ),
                      PIS_EMP_ACTIONS_DET[Staff No.]
                  )







