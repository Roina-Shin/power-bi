## TREATAS() function

- TREATAS() applies the result of a table expression to filter columns in an unrelated table (essentially creating a new, virtual relationship)


```
=TREATAS(TableExpression, ColumnName, [ColumnName], [...])
```

- In this demo, we will create a new table. Here, we created a store_id table using a table constructor inside a variable and then used TREATAS() to create a relationship that didn't exist between the two tables.


```
TREATAS Demo = 
VAR StoreID =
{
    "1",
    "2",
    "3",
    "4",
    "5",
    "6"
}
RETURN 
TREATAS(
    StoreID,  --Virtual table we created using table expression
    'Store Lookup'[store_id]
)

```


![TREATAS demo](/Relationship_pictures/TREATAS-demo.png "TREATAS demo")


- And the result that is returned is that all of the store_id values of the Store Lookup table that also exists within our virtual table are returned.


## ASSIGNMENT (TREATAS)

1. Based on the Target Sales table, use **TREATAS** and create measures for Bean/Tea, Beverage, Merchandise & Food sale goals.

Bonus: Use SUMMARIZE with TREATAS


2. Create % of goal measures that compare quantity sold to the goal amount.


3. Add the above measures to a matrix broken down by store & target months.


- Here's the Target Sales table created using DATATABLE().

```
Target Sales Table = 
datatable(
    "Store ID", INTEGER,
    "Year", integer,
    "Month", string,
    "Bean/Teas Goal", integer,
    "Beverage Goal", integer,
    "Food Goal", integer,
    "Merchandise Goal", integer,
    {
        {2, 2019, "April", 268, 15608, 1964, 80},
        {5, 2019, "April", 277, 14687, 2020, 91},
        {8, 2019, "April", 377, 15011, 1973, 34}
    }
)
```

![target sales table](/Relationship_pictures/target%20sales%20table.png "target sales table")


- The target Sales table is then connected to our Calendar table at the Year/Month level.


- Then create a measure for Bean/Teas Goal for Year/Month.


```
Bean Goal (TREATAS) = 
CALCULATE(
    SUM(
        'Target Sales Table'[Bean/Teas Goal]
    ),
    TREATAS(
        VALUES(
            'Calendar'[Year_ID]
        ),
        'Target Sales Table'[Year]
    )
)
```

![bean goal](/Relationship_pictures/bean%20goal.png "bean goal")


- We can repeat the same thing for our Month name. We are tying those Year and Month to our Calendar table **because both tables have different levels of granularity**.

```
Bean Goal (TREATAS) = 
CALCULATE(
    SUM(
        'Target Sales Table'[Bean/Teas Goal]
    ),
    TREATAS(
        VALUES(
            'Calendar'[Year_ID]
        ),
        'Target Sales Table'[Year]
    ),
    TREATAS(
        VALUES(
            'Calendar'[Month_Name]
        ),
        'Target Sales Table'[Month]
    )
)
```


![mapping month name](/Relationship_pictures/mapping%20month%20name.png "mapping month name")


- Now, do the same thing for Food, Beverages, and Merchandise.


```
Food Goal (TREATAS) = 
CALCULATE(
    SUM(
        'Target Sales Table'[Food Goal]
    ),
    TREATAS(
        VALUES(
            'Calendar'[Year_ID]
        ),
        'Target Sales Table'[Year]
    ),
    TREATAS(
        VALUES(
            'Calendar'[Month_Name]
        ),
        'Target Sales Table'[Month]
    )
)
```

```
Beverage Goal (TREATAS) = 
CALCULATE(
    SUM(
        'Target Sales Table'[Beverage Goal]
    ),
    TREATAS(
        VALUES(
            'Calendar'[Year_ID]
        ),
        'Target Sales Table'[Year]
    ),
    TREATAS(
        VALUES(
            'Calendar'[Month_Name]
        ),
        'Target Sales Table'[Month]
    )
)
```

```
Merchandise Goal (TREATAS) = 
CALCULATE(
    SUM(
        'Target Sales Table'[Merchandise Goal]
    ),
    TREATAS(
        VALUES(
            'Calendar'[Year_ID]
        ),
        'Target Sales Table'[Year]
    ),
    TREATAS(
        VALUES(
            'Calendar'[Month_Name]
        ),
        'Target Sales Table'[Month]
    )
)
```

- The second key objective is to create a percent of goal measures that compare quantity sold to goal amount.


```
Bean % to Goal = 
DIVIDE(
    CALCULATE(
        SUM(
            'Sales by Store'[quantity_sold]
        ),
        'Sales by Store'[Product Group] = "Whole Bean/Teas"
    ),
    [Bean Goal (TREATAS)]
)
```

![bean percent to goal](/Relationship_pictures/bean%20percent%20goal.png "bean percent to goal")


```
Beverage % to Goal = 
DIVIDE(
    CALCULATE(
        SUM(
            'Sales by Store'[quantity_sold]
        ),
        'Sales by Store'[Product Group] = "Beverage"
    ),
    [Beverage Goal (TREATAS)]
)
```

```
Food % to Goal = 
DIVIDE(
    CALCULATE(
        SUM(
            'Sales by Store'[quantity_sold]
        ),
        'Sales by Store'[Product Group] = "Food"
    ),
    [Food Goal (TREATAS)]
)
```

```
Merchandise % to Goal = 
DIVIDE(
    CALCULATE(
        SUM(
            'Sales by Store'[quantity_sold]
        ),
        'Sales by Store'[Product Group]  = "Merchandise"
    ),
    [Merchandise Goal (TREATAS)]
)
```

- When you create a matrix to map each measure we created above and add a proper filter that allows to see only the April 2019 values there, we get the proper percentage.


![matrix-complete](/Relationship_pictures/matrix-complete.png "matrix complete")