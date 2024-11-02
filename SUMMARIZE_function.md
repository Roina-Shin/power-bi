## SUMMARIZE function

- SUMMARIZE functionc creates a summary of the input table grouped by the specified columns.

```
=SUMMARIZE(Table, ColumnName)
```

- One thing to call out here is that you acutally have to use actual, physical column name as its second parameter to be the output of the summarized table. But for the table as the first parameter, you can use a virtual one using FILTER().


- First, you need table you want to call as a reference. And then you need a column name. 

```
SUMMARIZE Demo = 
summarize(
    'Sales by Store',
    'Sales by Store'[customer_id]
)
```

![summarize-function-with-single-column](/Table_pictures/summarize-function-with-single-column.png "summarize function with single column")


- We have a single column table here. You can see 2,250 rows here. The number of rows equals the the number of distinct values. 

- One way that you can think about SUMMARIZE is that it's kind of like **SELECT DISTINCT** in SQL if you are familiar with SQL. So, that may be a way to start thinking about this SUMMARIZE function.


```
SUMMARIZE Demo = 
summarize(
    'Sales by Store',
    'Sales by Store'[customer_id],
    'Sales by Store'[quantity_sold]
)
```

![summarize-function-with-two-columns](/Table_pictures/summarize-function-with-two-columns.png "summarize function with two columns")


- If you sort by the column customer_id, you see that the number of distinct customer_id is 2,250 while the unique combinations of these two columns are 4,742.


![two-columns-summarized](/Table_pictures/two-columns-summarized.png "two columns summarized")


```
SUMMARIZE Demo = 
summarize(
    filter(
        'Sales by Store',
        'Sales by Store'[quantity_sold] >= 3
    ),
    'Sales by Store'[customer_id],
    'Sales by Store'[quantity_sold]
)
```

![using-virtual-table-in-SUMMARIZE](/Table_pictures/using-virtual-table-in-SUMMARIZE.png "Using virtual table in SUMMARIZE")


- It's handy function that can come in useful when you return a table that only contains unique combinations of those columns that you've specified.


## ASIGNMENT

1. Use SUMMARIZE to create a table that only shows the days with unsold pastries and columns needed to calculate sold, unsold, and lost revenue.


2. Build a matrix that shows the results by product & store.


- This assignment is to create a matrix table that shows Sold Food vs. Unsold Food together with the Lost Revenue.

- First, create a summarized table: 


```
Unsold Pastries = 
SUMMARIZE(
    'Food Inventory',
    'Food Inventory'[transaction_date],
    'Food Inventory'[store_id],
    'Food Inventory'[product_id],
    'Food Inventory'[quantity_start_of_day],
    'Food Inventory'[quantity_sold],
    'Product Lookup (Updated)'[current_retail_price],
    'Product Lookup (Updated)'[product]
)
```

![assignment-create-summarized-table](/Table_pictures/assignment-create-summarize-table.png "assignment - create summarized table")


- Then head over to the Report section, and create Food Sold, Food Unsold, and Lost Revenue measures respectively:


```
Food Sold (SUMMARIZE Table) = 
SUM(
    'Unsold Pastries'[quantity_sold]
)
```

```
Food Unsold (SUMMARIZE Table) = 
SUMX(
    'Unsold Pastries',
    'Unsold Pastries'[quantity_start_of_day] - 'Unsold Pastries'[quantity_sold]
)
```

```
Lost Revenue (SUMMARIZE Table) = 
SUMX(
    'Unsold Pastries',
    [Food Unsold (SUMMARIZE Table)] * 
    RELATED('Product Lookup (Updated)'[current_retail_price])
)
```

![assignment-summarized-table-complete](/Table_pictures/assignment-summarized-table-complete.png "assignment summarized table complete")