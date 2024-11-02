## DAX SELECTEDVALUE()

- SELECTEDVALUE() is a function that returns a value when there's only one value in a specified column, otherwise returns an (optional) alternate result.


```
=SELECTEDVALUE(ColumnName, [AlternateResult])
```


```
Retail Price (SELECTEDVALUE) = 
SELECTEDVALUE(
    'Product Lookup (Updated)'[current_retail_price]
)
```


![SELECTEDVALUE](/Table_pictures/SELECTEDVALUE.png "SELECTEDVALUE()")


- There isn't a current retail price at the product group level above, because it **can't evaluate to a unique value**, so it doesn't return anything.


- But when we drill down, each individual product has its own unique value as these all can evaluate to a single retail price. 


- Now, what if you use SELECTEDVALUE() function as part of another function?


```
Quantity Sold (SELECTEDVALUE) =
DIVIDE(
    [Customer Sales],
    SELECTEDVALUE(
        'Product Lookup (Updated)'[current_retail_price])
)
```

![SELECTEDVALUE_within_another_function](/Table_pictures/SELECTEDVALUE_within_another_function.png "SELECTEDVALUE within another function")


