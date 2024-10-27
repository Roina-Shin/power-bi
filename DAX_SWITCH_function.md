## DAX SWITCH function

- Switch function comes in pretty handy when you are trying to replace **multiple nested if statements**. 


```
Month Number (DAX) = 
SWITCH('Calendar Lookup'[Month Name], 
"January", "1",
"February", "2",
"March", "3",
"April", "4",
"May", "5",
"June", "6",
"July", "7",
"August", "8",
"September", "9",
"October", "10",
"November", "11",
"December", "12"
)
```


![SWITCH-function](/pictures/SWITCH_function.png "SWITCH function")


- But Switch doesn't support other operators other than "equal to". So, greater than, less than operators are not acceptable to use as part of the Switch statement. However, we can use the following workaround.


- By adding "TRUE()" in, we are still testing for equivalence using SWITCH, but we are actually just trying to understand if the result of the test, and we are going to return the value if it is true.


```
Price Point =
SWITCH(
    TRUE(),
    'Product Lookup'[ProductPrice] > 500, "High",
    'Product Lookup'[ProductPrice] > 100, "Mid-Range",
    "Low")
```


![SWITCH-comparison-workaround](/pictures/SWITCH-comparison-workaround.png "SWITCH comparison workaround")


- So there you have it. SWITCH and SWITCH True. SWITCH True can come in super handy when you're trying to evaluate columns of data and return a value.