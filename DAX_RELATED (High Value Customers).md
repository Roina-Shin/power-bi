## DAX RELATED advanced function

### RELATED()

- RELATED() returns a value from a related table in the data model.

- Name of the column you want to retrieve values from (must reference a table on the "one" side of many-to-one relationship)

Examples:

- 'Product Lookup'[current_cost]

- 'Customer Lookup'[home_store]


```
=RELATED(COlumnName)
```

- For example, we can create a new calculated column using RELATED function:


```
Product Group =
RELATED(
    'Product Lookup (Updated)'[product_group]
)
```

![RELATED_calculated_column](/Relationship_pictures/RELATED_calculated_column.png "RELATED calculated column")


![resulting-new-column](/Relationship_pictures/resulting-new-column.png "resulting new column")


- Another example of RELATED function usage is to create a measure like this:


```
Wholesale Cost = 
SUMX(
    'Sales by Store',
    'Sales by Store'[quantity_sold] *
    RELATED(
        'Product Lookup (Updated)'[current_wholesale_price]
    )
)
```

![RELATED_measure](/Relationship_pictures/RELATED_measure.png "related measure")


## RELATEDTABLE()

- RELATEDTABLE() returns a related table, filtered so that it only includes the related rows.

- RELATEDTABLE() is a shortcut for CALCULATETABLE with no logical expression and performs a context transition from row context to filter context.


```
COUNTROWS(
        RELATEDTABLE('Food Inventory')
)
```

```
Number of Food Items Made = 
SUMX(
    RELATEDTABLE(
        'Food Inventory'
    ),
    'Food Inventory'[quantity_start_of_day]
)
```

![RELATEDTABLE_SUMX](/Relationship_pictures/RELATEDTABLE_SUMX.png "RELATEDTABLE SUMX")


## ASSIGNMENT: RELATEDTABLE (Calculating High Value Customers)

- I'm thinking we could flag customers as "high value" if they have purchased more than the average customer.

1. Add a calculated column to the Customer Lookup table and create a vriable for "Total Sales".

2. Add another variable called "AllCustomers" to  count total customers.

3. Add another variable that defines average sales.

4. Use RELATEDTABLE to create a variable that computes sales at the customer level.

5. Return "High" if customer-level sales are above the average. Otherwise, return "low".


```
High Value Customers = 
VAR TotalSales =
SUMX(
    'Sales by Store',
    'Sales by Store'[quantity_sold] *
    RELATED(
        'Product Lookup (Updated)'[current_retail_price]
    )
)
VAR AllCustomers =
COUNTA('Customer Lookup'[customer_id])
VAR AvgSales =
DIVIDE(TotalSales, AllCustomers)
VAR IndividualSales =
SUMX(
    RELATEDTABLE(
        'Sales by Store'
    ),
    'Sales by Store'[quantity_sold] *
    RELATED(
        'Product Lookup (Updated)'[current_retail_price]
    )
)
RETURN IF(
    IndividualSales > AvgSales,
    "High",
    "Low"
)
```

![high vlue customers](/Relationship_pictures/high%20value%20customers.png "high value customers")


