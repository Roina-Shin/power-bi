## CALENDAR() function

- CALENDAR() function returns a table with one column of all datew between start and end date.


```
=CALENDAR(StartDate, EndDate)
```

- When we create our own Calendar table, we might want to base this with our transaction_date from our sales table.


```
CALENDAR Demo =
CALENDAR(
    DATE(
        YEAR(
            MIN('Calendar'[Transaction_Date])), 1, 1),
    DATE(
        YEAR(
            MAX('Calendar'[Transaction_Date])), 12, 31)
)

```

![create custom calendar](/Time_pictures/create%20custom%20calendar.png "create custom calendar")


## CALENDARAUTO() function

- CALENDARAUTO() function returns a table with one column of dates based on a fiscal year end month. The range of dates is calculated automatically based on data in the model.


```
Date Table (CALENDARAUTO) = 
VAR MinYear = YEAR(MIN('Calendar'[Transaction_Date]))
VAR MaxYear = YEAR(MAX('Calendar'[Transaction_Date]))

RETURN 
ADDCOLUMNS(
    FILTER(
        CALENDARAUTO(),
        AND(
        YEAR([Date]) >= MinYear,
        YEAR([Date]) <= MaxYear)
        ),
        "Year", YEAR([Date]),
        "Quarter Number", INT(FORMAT([Date], "q")),
        "Quarter", "Q" & INT(FORMAT([Date], "q")),
        "Month Number", MONTH([Date]),
        "Month Short", FORMAT([Date], "mmm"),
        "Week Number", WEEKNUM([Date])
)
```


![calendarauto demo](/Time_pictures/CALENDARAUTO%20demo.png "CALENDARAUTO demo")


