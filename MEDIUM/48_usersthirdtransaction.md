>Assume you are given the table below on Uber transactions made by users. Write a query to obtain the third transaction of every user. Output the user id, spend and transaction date.

transactions Table:

Column Name	| Type
|--|--|
user_id|	integer
spend	|decimal
transaction_date|	timestamp

transactions Example Input:
user_id	|spend	|transaction_date
|--|--|--|
111	|100.50	|01/08/2022 12:00:00
111	|55.00	|01/10/2022 12:00:00
121	|36.00	|01/18/2022 12:00:00
145	|24.99	|01/26/2022 12:00:00
111	|89.60	|02/05/2022 12:00:00

Example Output:
user_id	|spend	|transaction_date
|--|--|--|
111	|89.60	|02/05/2022 12:00:00

***

A possible solution is:
```
SELECT 
  user_id, 
  spend, 
  transaction_date 
FROM (
  SELECT 
    user_id, 
    spend, 
    transaction_date, 
    RANK() OVER (
      PARTITION BY user_id 
      ORDER BY transaction_date) AS rank_num 
  FROM transactions
) AS trans_num
WHERE rank_num = 3
```

It's using a subquery with a window function to achieve this. Here's a breakdown of how the query works:

* Outer Query: **SELECT user_id, spend, transaction_date**
This part of the query selects three columns: user_id (the identifier for the user), spend (the amount spent in the transaction), and transaction_date (the date of the transaction).
* Subquery:
The subquery is used to assign a rank to each transaction for each user, based on the transaction date.
    * **SELECT user_id, spend, transaction_date, RANK() OVER (PARTITION BY user_id ORDER BY transaction_date) AS rank_num**
This part of the subquery selects user_id, spend, and transaction_date from the transactions table.
        * The **RANK() OVER (PARTITION BY user_id ORDER BY transaction_date)** function assigns a rank to each transaction for each user, ordered by the transaction date. Transactions for the same user are grouped together (PARTITION BY user_id), and then ordered by the date (ORDER BY transaction_date). The earliest transaction gets rank 1, the next gets rank 2, and so on.
        * **FROM transactions**
        Indicates that the data is sourced from the transactions table.
        * **AS trans_num**
        This aliases the subquery as trans_num.
* Outer Query Filter: **WHERE rank_num = 3**
After the ranks are assigned in the subquery, the outer query filters the results to only include those transactions where rank_num equals 3. This effectively selects the third transaction for each user.