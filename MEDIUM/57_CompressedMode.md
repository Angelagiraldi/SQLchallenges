You're given a table containing the item count for each order on Alibaba, along with the frequency of orders that have the same item count. 
>Write a query to retrieve the mode of the order occurrences. Additionally, if there are multiple item counts with the same mode, the results should be sorted in ascending order.

Clarifications:
*item_count: Represents the number of items sold in each order.
*order_occurrences: Represents the frequency of orders with the corresponding number of items sold per order.
For example, if there are 800 orders with 3 items sold in each order, the record would have an item_count of 3 and an order_occurrences of 800.

items_per_order Table:

|item_count	|order_occurrences|
|--|--|
1	|500
2	|1000
3	|800

Example Output:
mode|
|--|
2

Explanation:
Based on the example output, the order_occurrences value of 1000 corresponds to the highest frequency among all item counts. This means that item count of 2 has occurred 1000 times, making it the mode of order occurrences.

***


A possible solution is:
```
SELECT 
  item_count
FROM items_per_order
WHERE order_occurrences = 
  (SELECT MAX(order_occurrences) FROM items_per_order)
ORDER BY item_count
```


* **SELECT item_count FROM items_per_order**
Selects the item_count from the items_per_order table.
* **WHERE order_occurrences = (SELECT MAX(order_occurrences) FROM items_per_order)**
    * Filters the results to include only rows where the order_occurrences is equal to the maximum number of occurrences in the entire items_per_order table.
    * The subquery (SELECT MAX(order_occurrences) FROM items_per_order) calculates the maximum value of order_occurrences in the table.
* **ORDER BY item_count**
Orders the results by item_count in ascending order.