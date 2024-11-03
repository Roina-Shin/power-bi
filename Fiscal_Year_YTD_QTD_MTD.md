## WEEK Based Calculations

### 4-4-5 & 4-5-4 Fiscal Calendars

![2020 fiscal calendar](/Time_pictures/2020%20fiscal%20calendar.PNG "2020 fiscal calendar")


- January here is actually January 2021 which is a part of Fiscal Year 2020. November, December, and January represent Q4 2020.

- Within each month, there's 4 weeks in November and December and 5 weeks in January. The whole point of this type of calendars are standardizing the number of months in a quarter and then the number of weeks within each months.


### Previous Fiscal Week


```
Last Week's Sales 4-5-4 (DATEADD) = 
CALCULATE(
    [Customer Sales],
    DATEADD(
        '4-5-4 Calendar'[Date],
        -7,
        DAY
    )
)
```

![fiscal year last week calc](/Time_pictures/fiscal%20year%20last%20week%20calc.PNG "fiscal year last week calc")


![fiscal year comparison](/Time_pictures/fiscal%20year%20comparison.PNG "fiscal year comparison")


- To calculate the QTD sales,

```
QTD Sales (4-5-4) = 
VAR MaxDate = MAX('4-5-4 Calendar'[Date])
VAR MaxPeriod = MAX('4-5-4 Calendar'[FiscalQuarterYear])
VAR Output =
IF(
    HASONEVALUE(
        '4-5-4 Calendar'[FiscalQuarterYear]
    ),
    CALCULATE(
        [Customer Sales],
        '4-5-4 Calendar'[Date] <= MaxDate,
        '4-5-4 Calendar'[FiscalQuarterYear] = MaxPeriod
    ),
    "-"
)
RETURN Output
```

![QTD Sales](/Time_pictures/QTD%20sales.PNG "qtd sales")


## How to change the alphabetical order of Months

- It looks like that we need to sort our month name.

![alphabetical order](/Time_pictures/alphabetical%20order.PNG "alphabetical order")


- We need to go to the Data view, select the FiscalMonthName.


![fiscal month name](/Time_pictures/fiscal%20month%20name.PNG "fiscal month name")


- Then choose **Sort by Column** and select **Sort by Month Number**.


![sort by month number](/Time_pictures/sort%20by%20month%20number.png "sort by month number")


- Then go back to the visual pane and now you see that these are in an appropriate order.


![proper order](/Time_pictures/sorted%20properly.png "sorted properly")


## Fiscal Previous Period

```
Last QTD Sales (4-5-4) = 
VAR LastPeriod =
CALCULATE(
    [Customer Sales],
    FILTER(
        ALL(
            '4-5-4 Calendar'
        ),
        IF(
            SELECTEDVALUE(
                '4-5-4 Calendar'[FiscalQuarter]
            ) = 1,
            '4-5-4 Calendar'[FiscalQuarter] = 4 && 
            '4-5-4 Calendar'[FiscalYear] = SELECTEDVALUE('4-5-4 Calendar'[FiscalYear]) - 1,
            '4-5-4 Calendar'[FiscalYear] = SELECTEDVALUE('4-5-4 Calendar'[FiscalYear]) && 
            '4-5-4 Calendar'[FiscalQuarter] = SELECTEDVALUE('4-5-4 Calendar'[FiscalQuarter]) - 1
        )
    )
)
RETURN  LastPeriod
```

![fiscal previous period](/Time_pictures/fiscal%20previous%20period.png "fiscal previous period")


![last qtd sales](/Time_pictures/last%20qtd%20sales.PNG "last qtd sales")



## ASSIGNMENT (YTD-QTD-MTD)

1. uSING THE 4-5-4 Calendar and [Customer Sales] create measures for YTD and MTD sales.

2. Create a measure to compute week-over-week % change.

3. Create a matrix to visualize your results.


```
MTD Sales (4-5-4) =
VAR MaxDate = MAX('4-5-4 Calendar'[Date])
VAR MaxPeriod = MAX('4-5-4 Calendar'[FiscalMonthYear])
VAR Output =
IF(
    HASONEVALUE(
        '4-5-4 Calendar'[FiscalMonthYear]
    ),
    CALCULATE(
        [Customer Sales],
        '4-5-4 Calendar'[Date] <= MaxDate,
        '4-5-4 Calendar'[FiscalMonthYear] = MaxPeriod
    ),
    "-"
)
RETURN Output
```

![MTD sales](/Time_pictures/MTD%20sales.PNG "MTD sales")


```
YTD Sales (4-5-4) = 
VAR MaxDate = MAX('4-5-4 Calendar'[Date])
VAR MaxPeriod = MAX('4-5-4 Calendar'[FiscalYear])
VAR Output =
IF(
    HASONEVALUE(
        '4-5-4 Calendar'[FiscalYear]),
    CALCULATE(
        [Customer Sales],
        '4-5-4 Calendar'[Date] <= MaxDate,
        '4-5-4 Calendar'[FiscalYear] = MaxPeriod
    ),
    "-"
)
RETURN Output
```

![YTD sales](/Time_pictures/YTD%20sales.PNG "YTD sales")


![output](/Time_pictures/output.PNG "output")


- The last objective is to create a Week Over Week % change.

```
Week Over Week % Change (4-5-4) = 
DIVIDE(
    [Customer Sales] - [Last Week's Sales 4-5-4 (DATEADD)],
    [Last Week's Sales 4-5-4 (DATEADD)],
    BLANK()
)
```

![wow percent change](/Time_pictures/WOW%20percent%20change.PNG "wow percent change")

