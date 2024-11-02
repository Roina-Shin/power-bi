## GENERATESERIES() function

- GENERATESERIES() function returns a one column table populated with sequential values.


```
=GENERATESERIES(StartValue, EndValue, [IncrementValue])
```

- GENERATESERIES() is a great way to build a custom range of data to be used as a parameter input.


```
GENERATESERIES Demo = 
GENERATESERIES(
    -50.5,
    50.5,
    10
)
```


![generateseries-demo](/Table_pictures/GENERATESERIES-demo.png "generateseries demo")


- See above, the last value in the gerated list is not 50.5 but 49.5. This happens based on the increment value. The sequence meaning these values doesn't automatically end at the final value in this case, 50.5.


## Create a DATATABLE

```
DATATABLE Assignment = 
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

![datatable-assignment](/Table_pictures/DATATABLE-assignment.png "DATATABLE assignment")

