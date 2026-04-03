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

###  Workforce Diversity  :

###  Workforce Age  :

###  Workforce Mobility  :

###  Workforce Attrition  :
