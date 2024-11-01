## Time Grouping Function

![time_column](/DAX_pictures/time_column.PNG "time column")


- I want to create bins of data or groups of data. By that, I want to take all of the transactions that happened within a certain time frame and group that into one time. 


- For example, we could take all transactions that happen in the 6'o clock hour, and we can group those around to 6:00 AM, so we can do an analysis later by hour of the day.


- So, let's create a new calculated column and see what we can create for some bins here.


```
Time Group =
MROUND(
    'Sales by Store'[transaction_time],
    "1:00" --This is the multiple by which you want to group the time column
)
```


![mround-function](/DAX_pictures/mround-function.PNG "mround function")


- To change this decimal numbers into proper time format, click on Data Type and select Time from the menu.


![change-data-type](/DAX_pictures/change-data-type.png "change data type")


![data-type-changed](/DAX_pictures/data-type-changed.PNG "data type changed")


- But if you review the result, you will know it is not what we wanted to do. For example 6:45:00 rounded up to 7:00:00 am. What we would really like to do take everything that happens within that hour and round it down.


- So, let's update this with **floor** function. 


```
Time Group = 
FLOOR(
    'Sales by Store'[transaction_time],
    "1:00")
```

![floor-function-for-time-data](/DAX_pictures/floor-function-for-time-data.PNG "floor function for time data")


- Now, you see the time group is properly updated.


![properly-updated](/DAX_pictures/properly-updated.PNG "properly updated")


