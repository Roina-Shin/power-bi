## How to calculate customer age

- We are going to use DATEDIFF(Date1, Date2, Interval) to calculate the current age of our customers using their birthdates.


```
Customer Age = 
DATEDIFF(
    'Customer Lookup'[birthdate],
    TODAY(),
    DAY) / 365.25
```

- We are ging to create a new column using the above query.


![calculate-customer-age](/DAX_pictures/calculate-customer-age.PNG "calculate customer age")


- TODAY() is one of the most volatile function that constantly updates based on whatever the date or time is for your system.


- We get number of days between the two dates and divide them by **365.25**. And we are doing that to make sure that we account for leap years.


- Now, to show the data in proper age format, we need use **FLOOR** function to round down the decimal numbers to the whole numbers. Also, you could have used another function such as **ROUNDDOWN**.


```
Customer Age = 
FLOOR(
    DATEDIFF(
        'Customer Lookup'[birthdate],
        TODAY(),
        DAY) / 365.25,
        1)
```


![floor-function](/DAX_pictures/floor-function.PNG "floor function")


- Remember, calculated columns are stored in your data models, so they take up space. So, it may make more sense to create measure instead of calculated columns.