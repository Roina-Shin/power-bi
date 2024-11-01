## COALESCE function

- Let's talk about the COALESCE function. The COALESCE function is pretty straightforward.

- It returns the first argument that does not evaluate to blank. If all arguments evaluate to blank, blank is returned.


```
=COALESCE(Expression1, Expresssion2, [...])
```

- Example usages are like below:


```
=COALESCE(
    SUM('Sales by Store'[quantity_sold],
    0)
)


=COALESCE(
    [Yesterday Customer Revenue],
    "-"
)
```

- The COALESCE function actually replaces the **IF + ISBLANK** pattern, makes your code more readable, and can be optimized by the DAX engines.


```
Customer Sales (Last Year) = 
VAR CustomerSalesLY =
CALCULATE(
    SUMX(
        'Sales by Store',
        'Sales by Store'[quantity_sold] *
        RELATED(
            'Product Lookup (Updated)'[current_retail_price])
    ),
    DATEADD(
        'Calendar'[Transaction_Date],
        -1,
        YEAR)
)
RETURN COALESCE(
    CustomerSalesLY,
    "-")
```


![using-COALESCE-with-variable](/DAX_pictures/using-coalesce-with-variable.PNG "using coalesce with variable")


