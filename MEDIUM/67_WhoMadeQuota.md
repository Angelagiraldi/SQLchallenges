As a data analyst on the Oracle Sales Operations team, you are given a list of salespeople’s deals, and the annual quota they need to hit.

>Write a query that outputs each employee id and whether they hit the quota or not ('yes' or 'no'). Order the results by employee id in ascending order.

Definitions:
* deal_size: Deals acquired by a salesperson in the year. Each salesperson may have more than 1 deal.
* quota: Total annual quota for each salesperson.

deals Table:
employee_id |deal_size
|--|--|
101 |400000
101 |300000
201 |500000
301 |500000

sales_quotas Table:

employee_id |quota
|--|--|
101| 500000
201 |400000
301 |600000

Example Output:

employee_id| made_quota
|--|--|
101 |yes
201 |yes
301 |no

Explanation:
User 101 had 700k in sales, beating their 500k quota. User 201 had 500k in sales, beating their 400k quota. User 301 had 500k in sales, but had a 600k quota, so they didn't hit their goal.


***

A possible solution is:
```
WITH total_deals AS (
  SELECT 
    employee_id, 
    sum(deal_size) as deal_size
  FROM deals
  GROUP BY employee_id
)
SELECT 
  td.employee_id,
  CASE WHEN td.deal_size < sq.quota THEN 'no' ELSE 'yes' END AS made_quota
FROM total_deals AS td
JOIN sales_quotas AS sq
ON td.employee_id = sq.employee_id
ORDER BY td.employee_id
```

* CTE - total_deals:
    * This Common Table Expression (CTE) calculates the total deal size for each employee by summing up the deal sizes from the deals table, grouped by employee_id.
* Main Query:
    * **SELECT td.employee_id, ...**
        * The SELECT clause retrieves the employee_id from the total_deals CTE.
        * It also includes a CASE statement to determine whether each employee has met their sales quota based on the comparison between their total deal size (td.deal_size) and the quota (sq.quota).
    * **FROM total_deals AS td JOIN sales_quotas AS sq ON td.employee_id = sq.employee_id**
        * The FROM clause performs an inner join between the total_deals CTE (td) and the sales_quotas table (sq) based on the employee_id.
        * This join links the total deal sizes of employees with their respective sales quotas.
    * **ORDER BY td.employee_id**
        * The results are ordered by employee_id in ascending order.
