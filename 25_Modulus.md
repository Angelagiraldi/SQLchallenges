Given the following table *decimals*:

| decimals | | |
| -- | -- | -- |
| id | number1 | number 2|


Return a table with one column (mod) which is the output of *number1* modulus *number2*.

***

A possible solution:

```
SELECT
  number1%number2 as mod
FROM
  decimals;
```

**The % modulus simply returns the remainder of two numbers/ variables in two columns**