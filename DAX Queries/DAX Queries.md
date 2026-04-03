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



*  PAT as on Key_date =
                SUMX (
                    FILTER (
                        'BHEL TO',
                        'BHEL TO'[Start Date] <= LASTDATE ( Key_Date[Date] )
                            && 'BHEL TO'[End Date] >= LASTDATE ( Key_Date[Date] )
                    ),
                    'BHEL TO'[PAT]
                )


*  PAT_per_Employee =
                'BHEL TO'[PAT_key_date] * 100 / 'Emp Master for Name'[Count_key_date]

*  TO_key_date =
                SUMX (
                    FILTER (
                        'BHEL TO',
                        'BHEL TO'[Start Date] <= LASTDATE ( Key_Date[Date] )
                            && 'BHEL TO'[End Date] >= LASTDATE ( Key_Date[Date] )
                    ),
                    'BHEL TO'[Revenue]
                )



*  Revenue_per_employee =
                'BHEL TO'[TO_key_date] * 100 / 'Emp Master for Name'[Count_key_date]



###  Workforce Cost Analytics  :

###  Workforce Diversity  :

###  Workforce Age  :

###  Workforce Mobility  :

###  Workforce Attrition  :
