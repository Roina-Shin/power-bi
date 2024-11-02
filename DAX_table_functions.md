## DAX Advanced Table functions

- My assignment is as follows:

1. Check if the Ginger Scone product exists in the data model.

2. Use counting functions plus **DISTINCT & VALUES** to create a view that shows the blank row.

3. Add a new visual to show the missing product id when the blank product is selected.

4. Update the data model to include the new product.


### Calculating the count of Product IDs using DISTINCT() and VALUES()

```
Count of Product ID (DISTINCT) = 
COUNTROWS(
    DISTINCT(
        'Product Lookup (Updated)'[product_id])
)
```

```
Count of Product ID (VALUES) = 
COUNTROWS(
    VALUES(
        'Product Lookup (Updated)'[product_id])
)
```

![DISTINCT-and-VALUES](/DAX_pictures(2)/DISTINCT_and_VALUES.PNG "DISTINCT and Values")

