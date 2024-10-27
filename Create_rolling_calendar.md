## Create Rolling Calendar in Power BI


- First, create a blank query. We are going to use it **as a recipe to create a list of dates**.


![create-blank-query](/pictures/crreate_new_blank_query.png "Create blank query")


- Come up to the formular bar and enter in your literal as follows:


```
=#date(2024,1,1)
```

- Then you get this single result:


![entering-literal](/pictures/entering-literal.png "entering literal")


- Then click on the function button (fx) beside the formula bar.


![function-source](/pictures/function-source.png "function source")


- And the following will generate a list of dates.


```
=List.Dates(
    Source,
    Number.From(DateTime.LocalNow()) - Number.From(Source),
    #duration(1,0,0,0)
)
```


- What we are asking it to do is to calculate the current date and then compare it against the literal that we just created, which is our starting date.


- And based off of the difference between the two dates, I want you to create a list of all the individual values where the duration is one day.


- The cool thing here is that if you come back tomorrow or next week, or in a month, and you refresh this query, all of those new dates will be added or appended to the end of this list.


![convert-to-table](/pictures/convert-to-table.png "convert to a table")


- Now, we have a List Tools that has popped up, and click on the **To Table**.


- Then click ok and then the query is now converted to a table:


![query-to-table](/pictures/table-crated.png "table created")


- With a little custom M code, we created a scalable rolling calendar that is gonna update based on the current date.