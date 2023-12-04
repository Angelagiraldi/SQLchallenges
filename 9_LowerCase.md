Please modify the demographics table provided by converting all the entries in the 'race' column to lowercase letters while keeping the rest of the table unchanged.

Given:
| demogrpahics | | | |
| -- | -- | -- | -- |
| id | name | birthday | race |

***

A possible solution is

```
SELECT
  id,
  name,
  birthday,
  LOWER(race)
FROM
  demographics;
```

***LOWER built-in function converts all letters in a string to lower case.***