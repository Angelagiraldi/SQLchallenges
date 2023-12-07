In the application, a section designated for adults requires retrieving a list of user names and their respective ages from the `users`` table. The goal is to gather information exclusively for users who are 18 years old or older.

Table Schema:
| users| |
| --| --|
| name | age |

The task is to formulate a SQL query that retrieves the names and ages of users from the `users` table, specifically targeting individuals aged 18 years or above.

______________________

A possible solution is:


```
SELECT * 
FROM users
WHERE age >= 18
```

**Using a conditions to only include rows where the age column has a value greater than or equal to 18, fulfilling the requirement of obtaining information solely for users aged 18 or older.**