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

![DISTINCT_and_VALUES](/DAX_pictures/DISTINCT_and_VALUES.png "DISTINCT and VALUES")


- We create another table here showing product id, and because of the visual interactions that has been set up, we can actually click any product category from the product category table and actually filter this down to the relevant product id list.


![visual-interaction-for-filtering](/DAX_pictures/visual-interaction-for-filtering.png "visual interaction for filtering")


- That is a quick way to be able to use a couple of visuals to filter and identify what product is missing, etc.
