Remove the spaces from the string, then return the resultant string.

You are given a table *nospace* with column *x*,  return table with column *x* and your result in a column named *res*.


***

A possible solution is:
```
SELECT
    x, 
    REPLACE(x, ' ','') as res
FROM nospace
```

**Remove a space in x and replace it with no space.**

