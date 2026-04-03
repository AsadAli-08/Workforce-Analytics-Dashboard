# Workforce Analytics Dashboard  : DAX Queries

###  Executive Overview  :


    *  Count_key_date =
                VAR key_date1 =
                    LASTDATE ( Key_Date[Date] )
                RETURN
                    IF (
                        key_date1 == TODAY (),
                        COUNTX (
                            FILTER (
                                'Emp Master for Name',
                                'Emp Master for Name'[DOJ] <= key_date1
                                    && 'Emp Master for Name'[DOR] >= key_date1
                                    && 'Emp Master for Name'[EMP_DEFUNCT] == "N"
                            ),
                            'Emp Master for Name'[Staff No.]
                        ),
                        COUNTX (
                            FILTER (
                                'Emp Master for Name',
                                'Emp Master for Name'[DOJ] <= key_date1
                                    && 'Emp Master for Name'[DOR] >= key_date1
                            ),
                            'Emp Master for Name'[Staff No.]
                        )
                    )


###  Workforce Productivity  :

###  Workforce Cost Analytics  :

###  Workforce Diversity  :

###  Workforce Age  :

###  Workforce Mobility  :

###  Workforce Attrition  :
