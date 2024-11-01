## DAX SWITCH function (2)

- In our Calendar table, I actually want to create a calculated column.

- I want to be able to conduct some sort of analysis to look at the first half of the year versus the second half of the year.

- Let's start doing this using the SWITCH function.


```
Year Half = 
SWITCH(
    'Calendar'[Month_ID],
    1, "1H",
    3, "1H",
    4, "1H",
    5, "1H",
    6, "1H",
    7, "2H",
    8, "2H",
    9, "2H",
    10, "2H",
    11, "2H",
    12, "2H")
```

- To add in a bunch more lines there, you can use **alt + shift + down arrow** shortcut key.


![year-half-switch-function](/DAX_pictures/year-half-switch-function.PNG "year half switch function")


![usage-of-year-half](/DAX_pictures/usage-of-year-half.PNG "usage of year half")


- This is a great way to add a little of context to your table. The great thing about SWITCH function is that it's super easy to use and it's really useful function for a couple of reasons:

1. It cuts down the amount of time needed to write a similar code.

2. Using SWITCH in your DAX makes it a lot more readable.
