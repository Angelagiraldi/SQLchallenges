The task involves modifying the *decimals* table by rounding down the *number1* column and rounding up the *number2* column.

Given 

|decimals| | |
| -- | -- | -- |
| id | number1 | number 2 |

Return a table with two columns (number1, number2), 
the value in *number1* should be rounded down and the value in *number2* should be rounded up.

***

A possible solution is:

```
SELECT
  FLOOR(number1) as number1,
  CEILING(number2) as number2
FROM
  decimals;
```

***The new table is populated by rounding 'number1' down using the FLOOR() function and rounding 'number2' up using the CEILING() function, selecting values from the 'decimals' table.***
