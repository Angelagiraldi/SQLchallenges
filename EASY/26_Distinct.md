For this challenge you need to create a simple DISTINCT statement, you want to find all the unique ages.

Given this table schema:

| people | | |
| -- | -- | -- |
| id | name | age |

*** 

A possible solution is:

```
SELECT
    DISTINCT(age)
FROM
    people
```

**Distinct is a function that return unique values in a column.**
