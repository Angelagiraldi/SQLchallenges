For this challenge you need to create a simple SUM statement that will sum all the ages.

| people | | |
| -- | -- | -- |
| id | name | age |

***

A solution  is:

```
SELECT
  SUM(age) as age_sum
FROM
  people;
```

**Using the builtin SUM function and the ALIAS for creating age_sum**