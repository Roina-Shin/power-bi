## ALLSELECTED() function

- ALLSELECTED() function returns all rows in a table or values in a column, ignoring filters specified in the query but keeping any other existing filter context.

```
=ALLSELECTED(TableNameOrColumnName, [ColumnName], [ColumnName], [...])
```

- And oftentimes this function can be used to obtain visual totals or subtotals within a query.


```
Customer Sales (ALLSELECTED) = 
CALCULATE(
    [Customer Sales],
    ALLSELECTED(
    )
)
```

![ALLSELECTED-in-CALCULATE](/Table_pictures/ALLSELECTED-in-CALCULATE.png "ALLSELECTED in CALUCLATE")


![ALLSELECTED-example](/Table_pictures/ALLSELECTED-example.png "ALLSELECTED example")


- If you drag in the ALLSELECTED customer sales here, we have the same repeating total. Because there's no other filter context. We know that it invalidates or doesn't pay attention to the row or any sort of column filter. 


- But an important thing to note here is that it does take into account the external filter context, like slicers, for example. So, it only ignores the filter contexts within the same visual.


![external-filter-accepted](/Table_pictures/external-filter-accepted.png "external filter accepted")


## ASSIGNMENT

- To create a total baked:


```
Total Baked =
SUMX(
    'Food Inventory',
    'Food Inventory'[quantity_start_of_day]
)
```


- To calculate a total sold:

```
Total Sold (Food) = 
SUMX(
    'Food Inventory',
    'Food Inventory'[quantity_sold]
)
```

- So to calculate the ratio between the total baked where the filters are applied and the total baked where all the filters are cleared:


```
% of Total Baked (ALLSELECTED) = 
VAR SelectedQuantityBaked =
CALCULATE(
    [Total Baked],
    ALLSELECTED(
    )
)
VAR Ratio =
DIVIDE(
    [Total Baked],
    SelectedQuantityBaked,
    "0"
)
RETURN Ratio
```

![quantity-baked-allselected](/Table_pictures/quantity-baked-allselected.png "quantity baked allselected")


```
% of Food Quantity Sold (ALLSELECTED) = 
VAR SelectedFoodSold =
CALCULATE(
    [Total Sold (Food)],
    ALLSELECTED(
    )
)
VAR Ratio =
divide(
    [Total Sold (Food)],
    SelectedFoodSold
)
RETURN Ratio
```

![food-quantity-sold-allselected](/Table_pictures/food-quantity-sold-allselected.png "food quantity sold allselected")


- Finally, we need another % of all baked that is with REMOVEFILTERS() for food inventory table.

```
% of All Baked = 
VAR AllBaked =
CALCULATE(
    [Total Baked],
    REMOVEFILTERS(
        'Food Inventory'
    )
)
VAR Ratio =
DIVIDE(
    [Total Baked],
    AllBaked
)
RETURN Ratio
```

![complete-table](/Table_pictures/complete-table.png "complete table")


![second-slicer-filtering](/Table_pictures/second-slicer-filtering.png "second slicer filtering")


