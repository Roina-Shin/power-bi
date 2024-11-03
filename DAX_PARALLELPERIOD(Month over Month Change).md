## PARALLELPERIOD() function

- The PARALLELPERIOD() function returns a column of dates from a parallel period, by shifting the dates specified forward or backward in time based on a given interval (month/quarter/year)


- PARALLEPERIOD() lets you choose the interval, being month, quarter, and year. As we want to look at the previous quarter sales, we will choose quarter here.


![PARALLELPERIOD options](/Time_pictures/PARALLELPERIOD%20options.png "PARALLELPERIOD options")


![parallelperiod complete](/Time_pictures/PARALLELPERIOD%20complete.png "parallelperiod complete")


![parallel comparison](/Time_pictures/parallel%20comparison.png "parallel comparison")


## PREVIOUSQUARTER() function

- PREVIOUSQUARTER() function returns a table containing a column of all dates from the previous quarter, based on the first date in the date range specified.


```
Last Quarter Sales (PREVIOUSQUARTER) = 
CALCULATE(
    [Customer Sales],
    PREVIOUSQUARTER(
        'Calendar'[Transaction_Date]
    )
)
```


![previousquarter demo](/Time_pictures/PREVIOUSQUARTER%20Demo.png "previousquarter demo")


- If you see, the PREVIOUSQUARTER() returns the same repeating customer sales as PARALLELPERIOD() does, but the totals are very different.


![previousquarter mapping](/Time_pictures/mapping%20previousquarter.png "mapping previousquarter")


- In totals, the PREVIOUSQUARTER() returns the last quarter's sales instead of the total sales for the year.


## ASSIGNMENT (Time Periods)

- Share the month over month change in sales from Mar - Apr 2019. Also, look at April 2019 compared to April 2018 for YoY sales comparison.


1. Using time intelligence functions to calculate total sales for the previous month, as well as the percent change month over month.


2. Calculate the year over year change in sales from April 2018 vs. April 2019.


```
Customer Sales Last Month (PARALLELPERIOD) = 
CALCULATE(
    [Customer Sales],
    PARALLELPERIOD(
        'Calendar'[Transaction_Date],
        -1,
        MONTH
    )
)
```

![last month sales using parallelperiod](/Time_pictures/last%20month%20sales%20using%20parallelperiod.png "last month sales using parallelperiod")


![bring that in](/Time_pictures/bring%20that%20in.png "bring that in")


- Now, we will create a Month-over-Month % change measure.


```
Customer Sales MoM % Change =
DIVIDE(
    ([Customer Sales] - [Customer Sales Last Month (PARALLELPERIOD)]),
    [Customer Sales Last Month (PARALLELPERIOD)],
    BLANK()
)
```

![month over month percent change](/Time_pictures/month%20over%20month%20percent%20change.png "month over month percent change")


![MoM Change complete](/Time_pictures/MoM%20change%20complete.png "MoM Change complete")


- Next, we will calculate YoY change for customer sales.


```
Customer Sales YoY % Change =
VAR LastYearSales =
CALCULATE(
    [Customer Sales],
    SAMEPERIODLASTYEAR(
        'Calendar'[Transaction_Date]
    )
)
VAR Ratio =
DIVIDE(
    ([Customer Sales] - LastYearSales),
    LastYearSales
)
RETURN Ratio
```


![year over year change percent](/Time_pictures/year%20over%20year%20sales%20change%20percent.png "year over year percent change")


![YoY wrap up](/Time_pictures/YoY%20wrap%20up.png "YoY wrap up")


