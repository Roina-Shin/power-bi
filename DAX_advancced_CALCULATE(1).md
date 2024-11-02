## DAX Advanced CALCULATE

![advanced-calculate](/DAX_pictures/advanced-calculate.png "advanced calculate")

```
Store 3 Sales of Whole Beans & Teas (CALCULATE) = 
CALCULATE(
    [Customer Sales],
    'Store Lookup'[store_id] = 3,
    'Product Lookup (Updated)'[product_group] = "Whole Bean/Teas"
)
```


- Here's our assignment for advanced CALCULATE:

1. First, create a matrix containing **product group** & **store id** on rows.

2. Use **KEEPFILTERS** to create measures that show sales for each of the 3 stores.

3. Use **REMOVEFILTERS** with variables to create a single measure for % of Store Sales.


- First we are going to create 3 measure for each of the 3 stores:


```
Store 3 Sales = 
calculate(
    [Customer Sales],
    KEEPFILTERS(
        'Store Lookup'[store_id] = 3)
)
```


```
Store 5 Sales = 
CALCULATE(
    [Customer Sales],
    KEEPFILTERS(
        'Store Lookup'[store_id] = 5
    )
)
```


```
Store 8 Sales = 
calculate(
    [Customer Sales],
    KEEPFILTERS(
        'Store Lookup'[store_id] = 8
    )
)
```


- Then we are going to create a **% of Store Sales (REMOVEFILTERS)** measure.


```
% of Store Sales (REMOVEFILTERS) = 
VAR AllStoreSales =
CALCULATE(
    [Customer Sales],
    REMOVEFILTERS(
        'Store Lookup'[store_id]
    )
)
VAR Ratio =
DIVIDE(
    [Customer Sales],
    AllStoreSales
)
RETURN Ratio
```

![removefilters-with-variables](/DAX_pictures/removefilters-with-variables.png "REMOVEFILTERS with variables")


- We removed the filter context from the store, so when we drill in, we have the % context of each store within each category.


![percent-of-store-sales](/DAX_pictures/percent-of-store-sales.png "percent of store sales")


