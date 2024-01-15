A Microsoft Azure Supercloud customer is defined as a company that purchases at least one product from each product category.

> Write a query that effectively identifies the company ID of such Supercloud customers.



customer_contracts Table:

customer_id	|product_id	|amount
|--|--|--|
1	|1	|1000
1	|3	|2000
1	|5	|1500
2	|2	|3000
2	|6	|2000

products Table:
product_id	|product_category	|product_name
|--|--|--|
1|	Analytics	|Azure Databricks
2|	Analytics	|Azure Stream Analytics
4|	Containers	|Azure Kubernetes Service
5|	Containers	|Azure Service Fabric
6|	Compute	|Virtual Machines
7|	Compute	|Azure Functions

|customer_id
|--|
1

Explanation:
Customer 1 bought from Analytics, Containers, and Compute categories of Azure, and thus is a Supercloud customer. 
Customer 2 isn't a Supercloud customer, since they don't buy any container services from Azure.

***

A solution is:
```
WITH supercloud AS (SELECT 
  customer_contracts.customer_id,
  COUNT(DISTINCT(products.product_category)) AS  count_category
FROM customer_contracts
LEFT JOIN products
ON customer_contracts.product_id = products.product_id
GROUP BY customer_contracts.customer_id)

SELECT customer_id 
FROM supercloud
WHERE count_category = (SELECT COUNT(DISTINCT(products.product_category))
  FROM products)
```

* CTE Definition - **WITH supercloud AS (...)**:
    * The CTE is defined to select the customer_id and count the distinct product categories for each customer from the customer_contracts and products tables.
    * The LEFT JOIN is used to include all rows from customer_contracts and match them with corresponding rows in products based on the product_id.
    * The **COUNT(DISTINCT(products.product_category))** calculates the number of distinct product categories for each customer.
    * The CTE is aliased as supercloud.
* Main Query:
    * **SELECT customer_id FROM supercloud**
    Selects the customer_id from the supercloud CTE.
    * **WHERE count_category = (SELECT COUNT(DISTINCT(products.product_category)) FROM products)**
    Filters the results to include only those customers whose count of distinct product categories (count_category) is equal to the total count of distinct product categories in the entire products table.
    * The subquery **(SELECT COUNT(DISTINCT(products.product_category)) FROM products)** calculates the total count of distinct product categories.