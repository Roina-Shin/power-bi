## The Filter Function

- The Filter function returns a table that represents a subset of another table or expression.


- The Filter scans and iterates row by row by row by row across the entire table and it chops out or cuts out any of the rows that don't meet the Filter Expression criteria.


- Because a Filter is an iterator function, it can be slow and it can be processor intensive.

- The following query is saying to produce a table based on the Product Lookup table and keep the rows where product price is greater than my overall average price. And then, once you have that table, evaluate the total orders against this virtual table.


```
High Ticket Orders =
calculate(
    [Total Orders],
    FILTER(
        'Product Lookup',
        'Product Lookup'[ProductPrice] > [Overall Average Price]))
```


- Create a matrix table based on this query.

- As the bike is the only product category where the price is higher than the overall average product price, the High Ticket Orders measure only shows the value for the bikes.


![high-ticket-orders](/pictures/high-ticket-orders.png "High ticket orders")


## Iterator Function (X function)

- Iterator (or "x") functions allow you to loop through the same exxpression on each row of a table, then apply some sort of aggregation to the results (SUM, MAX, etc.).



```
=SUMX(Table, Expression)
```


```
Total Revenue =
SUMX(
    'Sales Data',
    'Sales Data'[OrderQuantity] * 'Sales Data'[Retail Price]
)
```

![sumx-calc](/pictures/sumx-calc.png "sumx calc")


![total-revenue-calc](/pictures/total-revenue-calc.png "total revenue calc")


- We can also update the same query, now with the Related function.


```
Total Revenue = 
sumx(
    'Sales Data',
    'Sales Data'[OrderQuantity] *
    RELATED('Product Lookup'[ProductPrice]))
```



- Then we should see that our values still hold the exact same here.


![related-nested-within-iterator](/pictures/related-nested-within-sumx.png ""related function nested within the iterator")


![total-revenue-calc](/pictures/total-revenue-calc.png "total revenue calc")


- Create a new measure named Total Cost that muliplies the order quantities in the Sales Data table by the product cost in the Product Lookup table, then calculates the sum.


```
Total Cost = 
sumx('Sales Data',
'Sales Data'[OrderQuantity] * 
related('Product Lookup'[ProductCost])
)
```


