## AVERAGEX() function

- AVERAGEX() is a function that calculates the average (arithmetic mean) of a set of expressions evaluated over a table.


```
=AVERAGEX(Table, Expression)
```

- Let's say we want to calculate the daily average sales.

- We will use the AVERAGEX() and **the table we want to evaluate every row for** is Calendar table. Calendar table is at the daily level and we want to iterate our expression, the customer sales for every day.


```
Average Daily Sales (AVERAGEX) = 
AVERAGEX(
    'Calendar',
    [Customer Sales]
)
```

![average-daily sales](/Iterator_pictures.md/average-daily-sales.png "average daily sales")


## Moving Average

```
Moving Average (AVERAGEX) =
VAR LastTransactionDate = MAX('Calendar'[Transaction_Date])
VAR AverageDays = 30
VAR PeriodInVisual =
FILTER(
    ALL(
        'Calendar'[Transaction_Date]
    ),
    AND(
        'Calendar'[Transaction_Date] > LastTransactionDate - AverageDays,
        'Calendar'[Transaction_Date] <= LastTransactionDate
    )
)
VAR Output =
CALCULATE(
    AVERAGEX(
        'Calendar',
        [Customer Sales]
    ),
    PeriodInVisual
)
RETURN Output
```

![moving average using AVERAGEX](/Iterator_pictures.md/moving%20average%20using%20averagex.png "Moving average using AVERAGEX")


- You can map this moving average for the last 30 days with the transaction date from Calendar and customer sales measure.


![clustered column chart](/Iterator_pictures.md/clustered-column-chart.png "clustered column chart")


## ASSIGNMENT (Moving Averages)

- Let's create a visual showing our total and average profit, trended monthly from Jan 2018 through Apr 2019.

1. Create a matrix to show total and daily average profit by month, for Jan 2018 - Apr 2019.

2. Use GENERATESERIES & SELECTEDVALUE to create a parameter with increments of 7 days over a 9 week period.

3. Create a measure using AVERAGEX and the parameter you defined to calculate the moving average profit.


![average profit](/Iterator_pictures.md/average%20profit.png "average profit")


![GENERATESERIES](/Iterator_pictures.md/GENERATESERIES.png "GENERATE SERIES")


![average day value](/Iterator_pictures.md/average%20day%20value.png "average day value")


- And here, we can put our average day value as a parameter to our Moving Avg calculation for profit.


```
Moving Average Profit (AVERAGEX) =
VAR LastTransactionDate = MAX('Calendar'[Transaction_Date])
VAR AverageDays = 'Average Days'[Average Day Value]
VAR PeriodInVisual =
FILTER(
    ALL(
        'Calendar'[Transaction_Date]
    ),
    AND(
        'Calendar'[Transaction_Date] > LastTransactionDate - AverageDays,
        'Calendar'[Transaction_Date] <= LastTransactionDate
    )
)
VAR Output =
CALCULATE(
    AVERAGEX(
        'Calendar',
        [Profit]
    ),
    PeriodInVisual
)
RETURN Output
```

![put parameter to moving average](/Iterator_pictures.md/putting%20parameter%20into%20moving%20average.png "putting parameter to moving average calc")


- We can create a slicer to add average days values to select from to control the view of our Moving Average Profit.


![add average days values](/Iterator_pictures.md/map%20average%20days%20value.png "map average days value")


