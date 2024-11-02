## How to Calculate TOP 10 Customers by Sales


![how-to-create-top-10-customers](/Table_pictures/how-to-create-top-10-customers.png "How to create TOP 10 customers")


- And click **Apply filter**.


![apply-filter-button](/Table_pictures/apply-filter-button.png "apply filter button")


- And now we have filters set up for store_id (to 8) and transaction_date (only for 2018).


![filters-set-up](/Table_pictures/filters-set-up.png "filters set up")


- The last thing we need to do here is to exclude non-members. Add another filter named customer_first-name, and select all but "Non-Member" from the list.


![add-another-filter](/Table_pictures/add-another-filter.png "add another filter")


- Perfect. Let's visualize the progress we made so far. We created additional measure as follows:


```
Customers Sales (ALLEXCEPT Assignment) = 
CALCULATE(
    [Customer Sales],
    ALLEXCEPT(
        'Sales by Store',
        'Calendar'[Transaction_Date],
        'Store Lookup'[store_id],
        'Customer Lookup'[customer_first-name],
        'Product Lookup (Updated)'[product_group]
)
)
```

![customer-sales-ALLEXCEPT](/Table_pictures/customer-sales-allexcept.png "customer sales ALLEXCEPT")


![measures-mapping](/Table_pictures/measures-mapping.png "measures mapping")


- Now, we want to create a measure that basically takes all of the sales for store 8 for the current time period that's selected, and we want to ignore the filters for things like customer names, using REMOVEFILTERS and KEEPFILTERS.


```
% of Store Level Sales = 
VAR StoreLevelSales =
CALCULATE(
    [Customer Sales],
    REMOVEFILTERS(
        'Customer Lookup'
    ),
    KEEPFILTERS(
        'Calendar'
    )
)
VAR Ratio =
DIVIDE([Customers Sales (ALLEXCEPT Assignment)], StoreLevelSales)
RETURN Ratio
```


![REMOVEFILTERS-and-KEEPFILTERS](/Table_pictures/REMOVEFILTERS-and-KEEPFILTERS.png "REMOVEFILTERS and KEEPFILTERS")


![result](/Table_pictures/result.png "result")


