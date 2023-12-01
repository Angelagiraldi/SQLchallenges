Arrange the data within the *companies* table by the number of employees in descending order. The resulting table should retain the same format and columns as the original *companies* table.

| companies | | | |
| -- | -- | -- | -- |
| id | ceo | motto | employees |


The objective is to use SQL exclusively. The resulting table should display the companies with the highest number of employees at the top.

*** 

A possible solution is

```
SELECT *
FROM companies
ORDER by employees DESC;
```

**The SQL query uses the SELECT statement to retrieve all columns (*) from the 'companies' table.
The ORDER BY clause is used to sort the data based on the 'employees' column in descending order (DESC). This arranges the companies with the highest number of employees at the top of the result set.**
