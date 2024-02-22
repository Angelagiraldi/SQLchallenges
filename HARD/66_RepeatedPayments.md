Sometimes, payment transactions are repeated by accident; it could be due to user error, API failure or a retry error that causes a credit card to be charged twice.

> Using the transactions table, identify any payments made at the same merchant with the same credit card for the same amount within 10 minutes of each other. Count such repeated payments.

Assumptions:
* The first transaction of such payments should not be counted as a repeated payment. This means, if there are two transactions performed by a merchant with the same credit card and for the same amount within 10 minutes, there will only be 1 repeated payment.

transactions Table:

transaction_id |	merchant_id |	credit_card_id |	amount	| transaction_timestamp
|--|--|--|--|--|
1|	101	|1|	100	|09/25/2022 |12:00:00
2|	101	|1|	100|	09/25/2022 |12:08:00
3|	101	|1|	100|	09/25/2022 |12:28:00
4|	102	|2|	300|	09/25/2022 |12:00:00
6|	102	|2|	400|	09/25/2022 |14:00:00

Example Output:
|payment_count
|--|
1|

Explanation:
* Within 10 minutes after Transaction 1, Transaction 2 is conducted at Merchant 1 using the same credit card for the same amount. This is the only instance of repeated payment in the given sample data.
* Since Transaction 3 is completed after Transactions 2 and 1, each of which occurs after 20 and 28 minutes, respectively hence it does not meet the repeated payments' conditions. 
* Whereas, Transactions 4 and 6 have different amounts.

***

A possible solution is:
```
 WITH payments AS (
  SELECT 
    merchant_id, 
    EXTRACT(
      EPOCH 
      FROM 
        transaction_timestamp - LAG(transaction_timestamp) OVER(
          PARTITION BY merchant_id, 
          credit_card_id, 
          amount 
          ORDER BY 
            transaction_timestamp
        )
    )/60 AS minute_difference 
  FROM 
    transactions
) 
SELECT 
  COUNT(merchant_id) AS payment_count
FROM 
  payments 
WHERE minute_difference<=10
```

* CTE - payments:
    * This Common Table Expression (CTE) calculates the time difference (in minutes) between consecutive transactions for each combination of merchant_id, credit_card_id, and amount.
    * It selects the merchant_id, and calculates the time difference using the EXTRACT(EPOCH FROM ...) function to compute the difference between the current transaction timestamp and the timestamp of the previous transaction (using LAG window function).
    * The time difference is divided by 60 to convert it from seconds to minutes.
* Main Query:
    * **SELECT COUNT(merchant_id) AS payment_count**
        * Counts the number of transactions for each merchant_id. liasing this count as payment_count.
    * **FROM payments**
        * Specifies that the data is sourced from the payments CTE.
    * **WHERE minute_difference <= 10**
        * Filters the results to include only transactions where the time difference between consecutive transactions is less than or equal to 10 minutes.
        * This condition ensures that only transactions occurring within a 10-minute window are counted together as a single payment.
