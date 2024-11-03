## Date Formatting

- Use the FORMAT function to specify date/time formatting. Common examples are:


![date formatting](/Time_pictures/date%20formatting.png "date formatting")


- One thing to note here, this is by no means an exhaustive list.



## ASSIGNMENT (Date Formatting)

- I'm wondering what portion of sales come in on weekends vs. weekdays. I would love to see a breakdown of weekend vs. weekday sales, shown as a percent of total.


1. Add columns to the calendar table to capture the weekday number and name.

2. Add a binary Y/N column to flag weekend dates.

3. Use a Matrix visual to show the percent of total sales for weekdays vs. weekends.


- WEEKDAY() function returns a number from 1 to 7 identifying the the day of the week of a date. If you choose 2 as the second paramter, it will be from **Monday = 1 through Sunday = 7**.


```
Week Day Number = 
WEEKDAY(
    'Calendar'[Transaction_Date],
    2)
```


![parameter 2](/Time_pictures/parameter-2.png "parameter 2")


```
Week Day Name = 
FORMAT(
    'Calendar'[Transaction_Date],
    "dddd"
)
```


```
Weekends =
IF(
    WEEKDAY(
        'Calendar'[Transaction_Date],
        2) IN {6, 7},
        "Y",
        "N"
)
```

![weekends validator](/Time_pictures/weekends%20validator.png "weekends validator")


- Lastly, we will create a percentage of Y/N by customer sales.


```
% of Total Sales =
VAR AllSales =
CALCULATE(
    [Customer Sales],
    ALL(
        'Sales by Store'
    )
)
VAR Ratio =
DIVIDE(
    [Customer Sales],
    AllSales
)
RETURN Ratio
```


![percent of total sales](/Time_pictures/percent%20of%20total%20sales.png "percent of total sales")


![visual](/Time_pictures/visual.png "visual")


