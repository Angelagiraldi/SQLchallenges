Need to create a simple MIN / MAX statement that will return the Minimum and Maximum ages out of all the people.

Given this table schema:

| people | | |
| -- | -- | -- |
| id | name | age |

***

A solution is:

```
SELECT
    MIN(age) as age_min,
    MAX(age) as age_max
FROM 
    people;
```

**Using built in functions to find min and max age in a column.**
