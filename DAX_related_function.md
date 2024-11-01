## Joining data with Related

- Related() function is bringing over the values like a vlookup function does in Excel from a table to another table.


```
Retail Price =
RELATED(
    'Product Lookup'[ProductPrice])
```


![related-function](/pictures/related-function.png "related function")


- Related function is used to reach into another table and grab the required column based on the relationship between those tables.