>Assume you're given a table containing data on Amazon customers and their spending on products in different category, write a query to identify the top two highest-grossing products within each category in the year 2022. The output should include the category, product, and total spend.

product_spend Table:
category	|product	|user_id	|spend	|transaction_date
|--|--|--|--|--|
appliance	|refrigerator	|165	|246.00	|12/26/2021 12:00:00
appliance	|refrigerator	|123|	299.99	|03/02/2022 12:00:00
appliance	|washing machine	|123	|219.80	|03/02/2022 12:00:00
electronics	|vacuum	|178|	152.00	|04/05/2022 12:00:00
electronics	|wireless headset	|156	|249.90	|07/08/2022 12:00:00
electronics	|vacuum	|145	|189.00	|07/15/2022 12:00:00

Example Output:

category	|product	|total_spend
|--|--|--|
appliance	|refrigerator|	299.99
appliance	|washing machine|	219.80
electronics|	vacuum	|341.00
electronics	|wireless headset	|249.90

Explanation:
Within the "appliance" category, the top two highest-grossing products are "refrigerator" and "washing machine."
In the "electronics" category, the top two highest-grossing products are "vacuum" and "wireless headset."

***

A solution is:
```
WITH ranked_spending_cte AS (
  SELECT 
    category, 
    product, 
    SUM(spend) AS total_spend,
    RANK() OVER (
      PARTITION BY category 
      ORDER BY SUM(spend) DESC) AS ranking 
  FROM product_spend
  WHERE EXTRACT(YEAR FROM transaction_date) = 2022
  GROUP BY category, product
)

SELECT 
  category, 
  product, 
  total_spend 
FROM ranked_spending_cte 
WHERE ranking <3
ORDER BY category, ranking;
```

* CTE Definition - **WITH ranked_spending_cte AS (...)**:
The query starts with a CTE named ranked_spending_cte. CTEs are temporary result sets that can be referred to within a SQL statement.
Inside the CTE:
    * **SELECT category, product, SUM(spend) AS total_spend**,
    The query selects category, product, and calculates total_spend for each product as the sum of spend.
    * **RANK() OVER (PARTITION BY category ORDER BY SUM(spend) DESC) AS ranking**
    A window function RANK() assigns a rank to each product within each category based on its total spend. Products are partitioned by category and ordered by total_spend in descending order.
    ranking is the alias for the rank assigned to each product.
    * **FROM product_spend**
    Specifies that the data is sourced from the product_spend table.
    * **WHERE EXTRACT(YEAR FROM transaction_date) = 2022**
    Filters the data to include only transactions from the year 2022.
    * **GROUP BY category, product**    
    Groups the data by both category and product for the SUM aggregation and ranking.

*   Main Query:
    * **SELECT category, product, total_spend**
    Selects category, product, and total_spend from the CTE.
    * **FROM ranked_spending_cte**
    Indicates that the data is being selected from the CTE ranked_spending_cte.
    * **WHERE ranking < 3**
    Filters to include only those rows where the product's ranking within its category is less than 3, meaning it includes only the top two products in each category.
    * **ORDER BY category, ranking;**
    Orders the results first by category and then by ranking. This ensures that the output is organized by category, and within each category, products are listed in order of their rank (i.e., the top two products in each category).