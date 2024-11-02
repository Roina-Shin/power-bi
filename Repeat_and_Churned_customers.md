## Calculated Table Joins

### Common Use Cases 

1. Blending and combining data across multiple tables.

2. Creating advanced calculations like **new vs. returning users** or **repeat purchase** behavior.

3. Querying tables to troubleshoot errors or better understand relationships in a data model.



### CROSSJOIN

- CROSSJOIN() is a function that returns a table that contains the cartesian product of the specified tables.

- **Cartesian** means it's the product of two sets, forming a set containing all ordered pairs. Saying that a little differently, **CROSSJOIN** returns a table that contains all of the combinations of the input tables. 

- So, CROSSJOIN has two parameters which are both tables, and these tables can either be table expresssions or physical tables contained within your data model.


```
=CROSSJOIN(Table, Table, [...])
```

- CROSSJOIN usually works well with tables with a small amount of columns and a low cardinality.


```
CROSSJOIN Demo = 
CROSSJOIN(
    'Product Lookup (Updated)',
    'Store Lookup'
)
```

![crossjoin-demo](/JOIN_pictures/CROSSJOIN_demo.PNG "crossjoin demo")


- In many cases, it may not be beneficial to create a table like this. But you may have use cases where you'd like to return a table that only contains a couple of values.


```
CROSSJOIN Demo = 
CROSSJOIN(
    VALUES(
        'Product Lookup (Updated)'[product_category]
    ),
    VALUES(
        'Product Lookup (Updated)'[product_group]
    )
)
```

![crossjoin-in-small-dataset](/JOIN_pictures/small-dataset-in-crossjoin.png "small dataset and crossjoin")


```
CROSSJOIN Demo = 
CROSSJOIN(
    VALUES(
        'Product Lookup (Updated)'[product_category]
    ),
    VALUES(
        'Product Lookup (Updated)'[product_group]
    ),
    FILTER(
        VALUES(
            'Store Lookup'[store_id]
        ),
        'Store Lookup'[store_id] = 3)
)
```

![cartesian-product-of-table-expressions](/JOIN_pictures/cartesian-product-of-table-expressions.png "cartesian product of table expressions")


- So, CROSSJOIN() function comes in handy when you do need to return the **cartesian product** of two or more tables.


### UNION

- UNION() function combines or "stacks" rows from two or more tables sharing the same column structure.


```
=UNION(Table, Table, [...])
```

```
Union Demo = 
UNION(
    FILTER(
        VALUES(
            'Calendar'[Month_ID]
        ),
        'Calendar'[Month_ID] = 3
    ),
    FILTER(
        VALUES(
            'Calendar'[Month_ID]
        ),
        'Calendar'[Month_ID] = 6
    )
)
```

![UNION-demo](/JOIN_pictures/UNION-demo.png "UNION demo")


### EXCEPT

- EXCEPT() function that returns all rows from the left table which do not appear in the right table.


```
=EXCEPT(LeftTable, RightTable)
```

- For example, if you want to look at your **churned customers**, and you would use **EXCEPT** to do that.


```
=EXCEPT('Customers 2019', 'Customers 2020')
```

- Important Notes:

1. Both tables must contain **the same number of columns**.

2. Columns are compared based on positioning in their respective tables.

3. The column names are determined by the **left table**.

4. The resulting table does NOT retain **relationships** to other tables (can't be used as an expanded table)


- As we can't reference a table expression as our first table parameter for the EXCEPT() function, as we can only reference a physical table that exists in our data model, we can create a table like below:


```
EXCEPT Demo = 
EXCEPT(
    'Customer Lookup',
    FILTER(
        VALUES(
            'Customer Lookup'
        ),
        'Customer Lookup'[customer_since] > DATE(2017,02,01)
    )
)
```


![EXCEPT-function-demo](/JOIN_pictures/except-function-demo.PNG "EXCEPT function demo")


### INTERSECT

- INTERSECT() returns all the rows from the left table which also appear in the right table.


```
=INTERSSECT(LeftTable, RightTable)
```

- Similar to EXCEPT(), INTERSECT accepts only two tables as parameters. And the first table must be a table that physically resides within the data model.


- Typically, INTERSECT() is used to determine **active customers** during a time period or **repeat purchases** in subsequent weeks, or any specific action that is happening across a time period.


- Also, INTERSECT() isn't **commutative**, meaning that combining table 1 and table 2 may have a different result than combining table 2 and table 1. The column structures should be the same.


- In this example, we want to look at employees who have started in a certain period.


```
New Employees (INTERSECT) = 
INTERSECT(
    'Employee Lookup',
    FILTER(
        'Employee Lookup',
        'Employee Lookup'[start_date] > DATE(2016, 12, 31)
    )
)
```


![new-employees](/JOIN_pictures/new-employees.PNG "new employees")


```
New Employees (INTERSECT) = 
INTERSECT(
    ADDCOLUMNS(
    'Employee Lookup',
    "Revenue",
    [Customer Sales]
    ),
    ADDCOLUMNS(
    FILTER(
        'Employee Lookup',
        'Employee Lookup'[start_date] > DATE(2016,12,31)
    ),
    "Revenue",
    [Customer Sales]
    )
)
```


![new-employees-with-revenue](/JOIN_pictures/new-employees-with-revenue.PNG "new employees with revenue")


- Maybe I could have just used **FILTER()** and **ADDCOLUMNS()** to get the same result, but understing basic syntax of joining tables can help you quickly plug in multiple different functions to your other use cases.


## ASSIGNMENT

- First we need to know the repeat customers between week 45 and week 46 in 2018.


```
Returning Customers Assignment = 
VAR PurchaseWeek45 =
CALCULATETABLE(
    VALUES(
        'Sales by Store'[customer_id]
    ),
    'Calendar'[Week_ID] = 45,
    'Calendar'[Year_ID] = 2018
)
VAR PurchaseWeek46 =
CALCULATETABLE(
    VALUES(
        'Sales by Store'[customer_id]
    ),
    'Calendar'[Week_ID] = 46,
    'Calendar'[Year_ID] = 2018
)
RETURN INTERSECT(
    PurchaseWeek45,
    PurchaseWeek46
)
```


![repeat-customers-within-a-period](/JOIN_pictures/repeat-customers-within-a-period.PNG "repeat customers within a period")


```
Returning Customers Assignment = 
VAR PurchaseWeek45 =
CALCULATETABLE(
    VALUES(
        'Sales by Store'[customer_id]
    ),
    'Calendar'[Week_ID] = 45,
    'Calendar'[Year_ID] = 2018
)
VAR PurchaseWeek46 =
CALCULATETABLE(
    VALUES(
        'Sales by Store'[customer_id]
    ),
    'Calendar'[Week_ID] = 46,
    'Calendar'[Year_ID] = 2018
)
RETURN INTERSECT(
    ADDCOLUMNS(
    PurchaseWeek45,
    "Revenue",
    [Customer Sales]
    ),
    ADDCOLUMNS(
    PurchaseWeek46,
    "Revenue",
    [Customer Sales]
    )
)
```


![repeat-customers-with-revenue](/JOIN_pictures/repeat-customers-with-revenue.PNG "repeat customers with revenue")
