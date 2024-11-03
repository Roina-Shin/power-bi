## CROSSFILTER() function

- CROSSFILTER() function specifies cross filtering direction to be used for the duration of the DAX expression. The relationship is defined by naming the two columns that serve as endpoints.


```
=CROSSFILTER(LeftColumnName, RightColumnName2, CrossFilterType)
```

- The left one is typically the MANY side while the right column is typically the ONE side.


- The third parameter is what specifies the direction of the cross filter. And you can specify one way which is **unidirectional**, both of which is bidirectional or none, which actually turns off the relationship for the duration of the calculation.

- CROSSFILTER() is a great function to use instead of setting up bidirectional filters in your data model. With CROSSFILTER() you can actually enable two way filtering for only specific cases meaning for the duration of the calculation of the measure you are using in.


- Let's say that we wanted to look at our customer sales by the number of employees Maven Roasters had working on a specific day at a specific store.


![crossfilter-demo](/Relationship_pictures/CROSSFILTER-demo.png "CROSSFILTER demo")


- If we look at our data model here, we have our Employee Lookup table and Sales By Store table lined together. But if we wanted to add in our transaction date or anything from our Calendar or any detail about our Store, these tables won't be able to filter Employee Lookup table. The direction of the filters is a single direction. The filters flow down from the lookup tables into our fact tables. 


- So what we need to do is to create a measure that **allows filters to flow or to propagate from the Lookup Tables (e.g. Calendar table) into the Sales by Store table and then back up to filter Employee Lookup table**.


- Now, we've got this framework set up here.


![framework-setup](/Relationship_pictures/framework-setup.png "framework set up")


- Then I want to look at the number of employees who actually drove these sales here. Let's create a measure here.


```
Number of Employees (CROSSFILTER) = 
COUNTROWS(
    'Employee Lookup'
)
```

- If we map this value to the matrix, we see the same repeating number for the entire dates. Because there is no relationship that can be determined between our Employee Lookup table and our Calendar dates.


![number of employees](/Relationship_pictures/number%20of%20employees.png "number of employees")


- So what we need to do is to add CALCULATE statement and modify it with the CROSSFILTER. In CROSSFILTER, we want to use the MANY sie first, which is 'Sales by Store'[staff_id] and the next side is the primary key, which is 'Employee Lookup'[staff_id].

- And we want filters to flow both ways between the Sales table and the Employee table.


```
Number of Employees (CROSSFILTER) = 
CALCULATE(
    COUNTROWS(
        'Employee Lookup'
    ),
    CROSSFILTER(
        'Sales by Store'[staff_id],
        'Employee Lookup'[staff_id],
        Both
    )
)
```

- Now, we can see different number of employees working on certain days.


![employees on certain days](/Relationship_pictures/employees%20on%20certain%20days.png "employees on certain days")


- We can confirm this by going to the data pane and filter the table down to January 1st. (where it had 7 employees working)


![7 employees working](/Relationship_pictures/7%20employees%20working.png "7 employees working")


- We can even filter the data with store_id within the Sales by Store.


![filter by store](/Relationship_pictures/filtering%20by%20store.png "filter by store")


## ASSIGNMENT (CROSSFILTER)

- We want to figure our the average order value for customers who make purchases each month, broken down by different product groups.


1. Create a measure called "Customers who Purchased" that uses **COUNTROWS & CROSSFILTER** to calculate the number of customers who made a purchase in a given time period.


2. Create a measure that calculates the average order value for "Customers who Purchased".


3. Create a matrix that shows the previous two measures broken down by Darren's request.


```
Customers Who Purchased = 
CALCULATE(
    COUNTROWS(
        'Customer Lookup'
    ),
    CROSSFILTER(
        'Sales by Store'[customer_id],
        'Customer Lookup'[customer_id],
        Both
    )
)
```

![customers who purchased](/Relationship_pictures/customers%20who%20purchased.png "customers who purchased")


```
Avg Order Values (Customers who Purchased) = 
DIVIDE(
    [Customer Sales],
    [Customers Who Purchased],
    BLANK()
)
```

![average order values](/Relationship_pictures/avg%20order%20values.png "avg order values")


- Then create a matrix broken down by Year/Month and Product Group.


![matrix creation](/Relationship_pictures/matrix-creation.png "matrix creation")


