## Convert date format into yyyy-mm-dd

![format-date](/DAX_pictures/format-date.PNG "format date")

```
Date Format (YYYY-MM-DD) = 
FORMAT(
    'Calendar'[Transaction_Date],
    "yyyy-mm-dd")
```

![format-date-column](/DAX_pictures/date-format-column.PNG "date format column")


- To change a number to a currency, you can use the following function:


```
Cost = 
CURRENCY(
    SUMX(
        'Sales by Store',
        'Sales by Store'[quantity_sold] *
        RELATED(
            'Product Lookup (Updated)'[current_cost]
        )
    )
)
```

![currency-format](/DAX_pictures/currency-format.PNG "currency format")


