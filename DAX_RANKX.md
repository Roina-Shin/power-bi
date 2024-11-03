## RANKX() function

- RANKX() function returns the ranking of a number in a list of numbers for each row in the table argument.


```
=RANKX(Table, Expression, [Value], [Order], [Ties])
```

- Only the first two parameters are mandatory.


- In Ties parameter, you can use two options:

1) **SKIP**: Skips ranks after ties. (1, 2, 3, 4, 4, 4, 7, 8, 9 ...)

2) **DENSE**: Shows the next rank, regardless of ties. (1, 2, 3, 3, 3, 4, 5, 6 ....)


- A pro tip is that if your Total row is showing a rank of 1, use **IF & HASONEVALUE** with **RANKX** to exclude it from the rank.


![table setup](/Iterator_pictures.md/table%20setup.png "table setup")


- Here in the above, I'd like to create a rank for my customer sales.

- To create a mesure, first, remove all filters by using ALL() within the RANKX() and look to Customer Sales.

```
Rank of Customer Sales (RANKX) = 
RANKX(
    ALL(
        'Product Lookup (Updated)'[product_category]
    ),
    [Customer Sales]
)
```

- If you map this out to our matrix:


![mapping rankx](/Iterator_pictures.md/mapping%20rankx.png "mapping rankx")


- But the total above is irrelevant for analysis. What we can do here is to use **HASONEVALUE()** AND **IF**.

```
Rank of Customer Sales (RANKX) = 
IF(
    HASONEVALUE(
        'Product Lookup (Updated)'[product_category]
    ),
    RANKX(
        ALL(
            'Product Lookup (Updated)'[product_category]
        ),
        [Customer Sales]
    )
)
```

![hasonevalue with rankx](/Iterator_pictures.md/HASONEVALUE-with-rankx.png "HASONEVALUE with RANKX()")


- Now, let's see how we can handle ties here. 

```
Rounded Customer Sales = 
MROUND(
    [Customer Sales],
    100000
)
```

```
Rounded Rank (RANKX) = 
IF(
    HASONEVALUE(
        'Product Lookup (Updated)'[product_category]
    ),
    RANKX(
        ALL(
            'Product Lookup (Updated)'[product_category]
        ),
        [Rounded Customer Sales],
        ,DESC
        ,Dense
    )
)
```

![rankx-dense](/Iterator_pictures.md/RANKX-dense.png "rankx dense")


## ASSIGNMENT (RANKX)

- I'm curious to see our profits for the best-selling products in each category. Show me a visual at the product category level, only including profit for the top 5 products within each category.


1. Use variables and RANKX to create a single measure for Top 5 Products.

2. Create a slicer for Product Category to be able to analyze the top 5 across different categories.


```
Top 5 Products by Profit (RANKX) = 
VAR ProfitRank =
IF(
    HASONEVALUE(
        'Product Lookup (Updated)'[product_category]
    ),
    RANKX(
        ALL(
            'Product Lookup (Updated)'[product]
        ),
        [Profit]
    )
)
VAR Top5Products =
IF(
    ProfitRank <= 5,
    [Profit]
)
RETURN Top5Products
```

![top 5 products by profit](/Iterator_pictures.md/top%205%20products%20by%20profit.png "top 5 products by profit")


![mapping top 5 products](/Iterator_pictures.md/mapping%20top%205%20products.png "mapping top 5 products")


- You can add a slicer to the Top 5 products to see the top 5 products based on the product category selection made here.


![add slicer to top 5](/Iterator_pictures.md/add%20slicer%20to%20top%205.png "add slicer to top 5")






