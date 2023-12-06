Given an integer or a floating-point number, find its opposite.

>Examples:
1: -1
14: -14
-34: 34

You will be given a table: *opposite*, with a column *number*. Return a table with a column: res.

***

A possible solution is 
```
SELECT
    -number as res
FROM opposite
```

**Always returns the opposite by  applying the negative**