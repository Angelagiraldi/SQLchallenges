Given a table named 'numbers' with a single column 'number' of integer values, the goal is to generate an output table named 'output' with a column 'is_even' containing the labels "Even" or "Odd" based on the values in the 'number' column.

***

A possible solution is:

```
INSERT INTO output (is_even)
SELECT
  CASE WHEN number%2=0 THEN 'Even' ELSE 'Odd' END AS is_even
FROM
  numbers
```

***We create a new table named 'output' with a column 'is_even'. With the SELECT statement, we populate the 'output' table by employing a CASE statement. It checks each 'number' in the 'numbers' table.
The CASE statement evaluates if the number is even or odd based on the remainder of the number divided by 2 (number % 2). If the remainder is 0, it's even; otherwise, it's odd.
The result is stored in the 'is_even' column of the 'output' table.
