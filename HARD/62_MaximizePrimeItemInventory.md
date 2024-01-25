Amazon wants to maximize the number of items it can stock in a 500,000 square feet warehouse. It wants to stock as many prime items as possible, and afterwards use the remaining square footage to stock the most number of non-prime items.

>Write a query to find the number of prime and non-prime items that can be stored in the 500,000 square feet warehouse. Output the item type with prime_eligible followed by not_prime and the maximum number of items that can be stocked.

Assumptions:
* Prime and non-prime items have to be stored in equal amounts, regardless of their size or square footage. This implies that prime items will be stored separately from non-prime items in their respective containers, but within each container, all items must be in the same amount.
* Non-prime items must always be available in stock to meet customer demand, so the non-prime item count should never be zero.
* Item count should be whole numbers (integers).

inventory table:
|item_id	|item_type	|item_category	|square_footage|
|--|--|--|--|
1374	|prime_eligible	|mini refrigerator|	68.00
4245	|not_prime	|standing lamp	|26.40
2452	|prime_eligible	|television	|85.00
3255	|not_prime	|side table	|22.60
1672|prime_eligible	|laptop|	8.50

Example Output:

item_type|	item_count
|--|--|
prime_eligible	|9285
not_prime	|6


***

A possible solution is:
```
WITH summary AS (  
  SELECT  
    item_type,  
    SUM(square_footage) AS total_sqft,  
    COUNT(*) AS item_count  
  FROM inventory  
  GROUP BY item_type
),
prime_items AS (  
  SELECT  
    item_type,
    total_sqft,
    FLOOR(500000/total_sqft) AS prime_item_combination_count,
    (FLOOR(500000/total_sqft) * item_count) AS prime_item_count
  FROM summary  
  WHERE item_type = 'prime_eligible'
),
non_prime_items AS (
SELECT
  item_type,
  total_sqft,  
  FLOOR(
    (500000 - 
      (SELECT prime_item_combination_count * total_sqft FROM prime_items))  
      / total_sqft)  
      * item_count AS non_prime_item_count  
FROM summary
WHERE item_type = 'not_prime')

SELECT 
  item_type, 
  prime_item_count AS item_count
FROM prime_items

UNION ALL

SELECT 
  item_type, 
  non_prime_item_count AS item_count
FROM non_prime_items;
```


* **CTE - summary** (Summary of Inventory):
    * The CTE calculates the total square footage (total_sqft) and item count (item_count) for each item_type from the inventory table.
    * It uses the GROUP BY clause to group items by item_type.
* **CTE - prime_items** (Prime Eligible Items):
    * This CTE is designed to calculate the item count for "prime eligible" items based on specific conditions.
    * It selects item_type, total_sqft, calculates prime_item_combination_count as the floor division of 500,000 by total_sqft, and calculates prime_item_count as the product of prime_item_combination_count and item_count.
    * It filters the results to include only rows with item_type equal to 'prime_eligible'.
* **CTE - non_prime_items** (Non-Prime Eligible Items):
    * Similar to the prime_items CTE, this CTE calculates the item count for "not prime" items based on specific conditions.
    * It selects item_type, total_sqft, and calculates non_prime_item_count based on the available square footage and the calculated values from the prime_items CTE.
    * It filters the results to include only rows with item_type equal to 'not_prime'.
* Main Query - **Union of Prime and Non-Prime Items**:
    * The main query combines the results of prime_items and non_prime_items using the UNION ALL operator.
    * It selects item_type and item_count from both CTEs.
    * This union operation provides the final result, which includes the item count for both "prime eligible" and "not prime" items.