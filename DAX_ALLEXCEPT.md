## DAX ALLEXCEPT() function

- ALLEXCEPT() removes all report context filters in the table **except** the filters applied to the specified columns in the query.


```
=ALLEXCEPT(TableName, [ColumnName], [ColumnName], [...])
```


```
Customer Sales (ALLEXCEPT) = 
calculate(
    [Customer Sales],
    ALLEXCEPT(
        'Sales by Store',
        'Product Lookup (Updated)'[product_group],
        'Calendar'[Transaction_Date]
    ))
```

![ALLEXCEPT](/Table_pictures/ALLEXCEPT.png "ALLEXCEPT")


- When we visualize this with our customer sales table, our totals here are aligning. But the values for each of of the products are not retaining these filters or clearing them, so we see the same repeating totals here.


![same-repeating-totals](/Table_pictures/same-repeating-totals.png "same repeating totals")


