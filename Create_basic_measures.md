## Create basic measures


### Measure 1 - Customer Sales


![customer_sales_isblank](/DAX_pictures/customer_sales_isblank.png "customer sales isblank")


```
Customer Sales = 
SUMX(
    'Sales by Store',
    'Sales by Store'[quantity_sold] * 'Sales by Store'[unit_price]
)
```


### Measure 2 - Customer Sales (Last Year)

```
Customer Sales (Last Year) = 
calculate(
    [customer sales],
    dateadd(
        'Calendar'[Transaction_Date],
        -1,
        YEAR
    ))
```


### Measure 3 - Customer Sales LY (ISBLANK)


```
Customer Sales LY (ISBLANK) = 
IF(
    ISBLANK(
        [Customer Sales (Last Year)]),
        "No Sales",
        [Customer Sales (Last Year)]
)
```



