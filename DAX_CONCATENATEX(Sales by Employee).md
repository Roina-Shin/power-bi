## CONCATENATEX() function

- CONCATENATEX() evaluates an expression for each row of the table and returns the concatenation of those values in a single string, separated by a delimiter.


```
=CONCATENATEX(Table, Expression, [Delimiter], [OrderBy_Expression], [Order])
```

```
Employee Full Name (CONCATENATEX) =
CONCATENATEX(
    'Employee Lookup',
    'Employee Lookup'[first_name] & " " & 'Employee Lookup'[last_name],
    ", ",
    'Employee Lookup'[first_name],
    ASC
)
```

![concatenatex-demo](/Iterator_pictures.md/concatenatex-demo.png "concatenatex demo")


![employee full name](/Iterator_pictures.md/employee%20full%20name.png "employee full name")


- It would be more beneficial to look at each employee level.

![broken by staff id](/Iterator_pictures.md/broken%20by%20staff%20id.png "broken by staff id")


- You can do something like this, like creating a card visual that shows which item you selected.


```
Selected Product Category (CONCATENATEX) = 
"Showing Sales For: " &
CONCATENATEX(
    VALUES(
        'Product Lookup (Updated)'[product_category]
    ),
    'Product Lookup (Updated)'[product_category],
    ", ",
    'Product Lookup (Updated)'[product_category],
    ASC
)
```

![selectedvalues](/Iterator_pictures.md/selectedvalues.png "selected values")


![card visual](/Iterator_pictures.md/card%20visual.png "card visual")


## ASSIGNMENT (CONCATENATEX)

- I want to look into Store 5 sales by employee, to see which employees account for largest percentage of total sales. Create a visual that shows Sales and % Total Sales with the ability to filter for any combination of Employee ID.


1. Create a visual for Store 5 that shows customer sales by store and employee id. 

2. Create a measure to show % of Total Sales for Store 5.

3. Use CONCATENATEX to define a measure that shows the employee name(s) selected and the % of Total Sales for Store 5.


```
% of Customer Sales (CONCATENATEX Assignment) = 
VAR AllExceptSales =
CALCULATE(
    [Customer Sales],
    ALLEXCEPT(
        'Sales by Store',
        'Store Lookup'[store_id]
    )
)
VAR Ratio =
DIVIDE(
    [Customer Sales],
    AllExceptSales,
    BLANK()
)
Return Ratio
```

![allexceptsales](/Iterator_pictures.md/allexceptsales.png "allexcept sales")


```
Sales by Employee Name (CONCATENATEX) = 
"Employee: " &
CONCATENATEX(
    VALUES(
        'Employee Lookup'[first_name]
    ),
    'Employee Lookup'[first_name] &
    "-" &
    FORMAT(
        [% of Customer Sales (CONCATENATEX Assignment)], "Percent"),
        ", ",
        'Employee Lookup'[first_name],
        ASC
)
```

![salesbyemployeename](/Iterator_pictures.md/salesbyemployeename.png "sales by employee name")


![complete-result](/Iterator_pictures.md/complete%20result.png "complete result")


- You can make this report more neat by using HASONEVALUE() function.


```
Sales by Employee Name (CONCATENATEX) = 
IF(
HASONEVALUE('Employee Lookup'[first_name]
),
"Employee: " &
CONCATENATEX(
    VALUES(
        'Employee Lookup'[first_name]
    ),
    'Employee Lookup'[first_name] &
    "-" &
    FORMAT(
        [% of Customer Sales (CONCATENATEX Assignment)], "Percent"),
        ", ",
        'Employee Lookup'[first_name],
        ASC
),
"Select a Single Employee"
)
```

![HASONEVALUE function](/Iterator_pictures.md/hasone%20function.png "hasonevalue function")



