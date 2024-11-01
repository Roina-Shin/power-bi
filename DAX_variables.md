## DAX Variables

- Variables (VAR) are DAX expressions which can be reused multiple times within a query, and are commonly used for two key purposes:


- Readability: Variables make complex code more human readable.

- Performance: Variables are only evaluated once no matter how often they are used within a query.


- To use variables within DAX, we must include 2 key components:

1) declaration expression - starts off with **VAR** 

2) return expression - starts off with **return**


```
Order by Female = 
var TotalOrderbyFemale =
calculate(
    sum('Sales by Store'[quantity_sold]),
    filter(
        'Customer Lookup',
        'Customer Lookup'[gender] = "F"))
RETURN TotalOrderbyFemale
```

![defining-a-variable-within-a-query](/DAX_pictures/defining-variable-within-a-query.png "defining a variable within a query")


- Variables can be a helpful tool for testing or debugging your DAX code.


```
% Quantity Sold to Female (VAR) = 
VAR OrdersByFemale = 
calculate(
    sum('Sales by Store'[quantity_sold]),
    filter(
        'Customer Lookup',
        'Customer Lookup'[gender] = "F")
)
var OverallOrders =
sum('Sales by Store'[quantity_sold])
VAR PercentofFemale =
DIVIDE(OrdersByFemale,OverallOrders,"-")
RETURN PercentofFemale
```

![percent-quantity-sold-to-female](/DAX_pictures/percent_quantity_sold_to_female.PNG "percent of quantity sold to female")


- In this example, we use variables to define individual components of larger, more complex measure. And the **divide** function really allows me to use clean divide on numerator and denominator in case the denominator is zero. The 3rd parameter in the divide function lets you replace the error from the zero division with something else. (in this case hyphen)


- We can more easily identify which specific component is the root cause in the case of an error.


![quantity-sold-to-female-ratio-in-visual](/DAX_pictures/quantity-sold-to-female-ratio-in-visual.PNG "quantity sold to female ratio in visual")


## Using Variables and SWITCH function together

```
Quarter and Year = 
VAR Q1 =
'Calendar'[Month_ID] IN {1, 2, 3}
VAR Q2 =
'Calendar'[Month_ID] IN {4, 5, 6}
VAR Q3 =
'Calendar'[Month_ID] IN {7, 8, 9}
VAR Q4 =
'Calendar'[Month_ID] IN {10, 11, 12}

RETURN
SWITCH(
    TRUE(),
    Q1, "Q1" & "-" & 'Calendar'[Year_ID],
    Q2, "Q2" & "-" & 'Calendar'[Year_ID],
    Q3, "Q3" & "-" & 'Calendar'[Year_ID],
    Q4, "Q4" & "-" & 'Calendar'[Year_ID]
)
```

![variables-and-SWITCH](/DAX_pictures/variables-and-SWITCH.PNG "variables and SWITCH")


![quarter-year-column](/DAX_pictures/quarter-year-column.PNG "quarter year column")


