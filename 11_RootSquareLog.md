Given the following table 'decimals':

|decimals| | |
| -- | -- | -- |
| id | number1 | number 2 |

The objective is to transform data from the 'decimals' table, computing the square root of the values in the 'number1' column and obtaining the base 10 logarithm of the values in the 'number2' column.


***
A possible solution is 
```
SELECT 
    SQRT(number1) AS root,
    LOG10(number2) AS log
FROM decimals;
```

***We populate the new table by computing the square root of 'number1' using the SQRT() function and calculating the base 10 logarithm of 'number2' using the LOG10() function, selecting values from the 'decimals' table.***