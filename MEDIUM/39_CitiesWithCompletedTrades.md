> Write a query to retrieve the top three cities that have the highest number of completed trade orders listed in descending order. Output the city name and the corresponding number of completed trade orders.

trades Table:
Column Name	| Type
| -- | -- |
order_id	| integer
user_id	|integer
price	|decimal
quantity	|integer
status	|string('Completed' ,'Cancelled')
timestamp	|datetime

trades Example Input:
order_id	|user_id	|price	|quantity	|status	|timestamp
| -- | -- | -- | -- | -- | -- |
100101	|111	|9.80	|10	|Cancelled	|08/17/2022 |12:00:00
100102	|111	|10.00	|10	|Completed	|08/17/2022 |12:00:00
100259	|148	|5.10	|35	|Completed	|08/25/2022 |12:00:00
100264	|148	|4.80	|40	|Completed	|08/26/2022 |12:00:00
100305	|300	|10.00	|15	|Completed	|09/05/2022 |12:00:00
100400	|178	|9.90	|15	|Completed	|09/09/2022 |12:00:00
100565	|265	|25.60	|5	|Completed	|12/19/2022 |12:00:00


users Table:

Column Name	|Type
| -- | --|
user_id	|integer
city	|string
email	|string
signup_date|	datetime


users Example Input:

user_id	|city	|email	|signup_date
| -- | -- | -- |--|
111	|San Francisco|	rrok10@gmail.com	|08/03/2021 12:00:00
148	|Boston	|sailor9820@gmail.com	|08/20/2021 12:00:00
178	|San Francisco	|harrypotterfan182@gmail.com	|01/05/2022 12:00:00
265	|Denver	|shadower_@hotmail.com	|02/26/2022 12:00:00
300	|San Francisco	|houstoncowboy1122@hotmail.com	|06/30/2022 12:00:00


Example Output:
city	|total_orders
| -- | --|
San Francisco	|3
Boston	|2
Denver	|1

Explanation
In the given dataset, San Francisco has the highest number of completed trade orders with 3 orders. Boston holds the second position with 2 orders, and Denver ranks third with 1 order.

***
A possible solution is:
```
SELECT users.city,
  COUNT(trades.order_id) AS total_orders
FROM trades
JOIN users
  ON trades.user_id = users.user_id
WHERE trades.status = 'Completed'
GROUP BY users.city
ORDER BY COUNT(trades.order_id) DESC
LIMIT 3
```

* **SELECT users.city, COUNT(trades.order_id) AS total_orders**
This line selects two pieces of data:
    * **users.city**: The city of the user from the users table.
    * **COUNT(trades.order_id) AS total_orders**: The total number of orders (trade transactions) for each city. COUNT(trades.order_id) counts the number of order_ids (which represent unique trade orders) associated with users from each city, and it is aliased as total_orders.
* **FROM trades**
Specifies that the primary table from which data is being selected is the trades table.
* **JOIN users ON trades.user_id = users.user_id**
This line performs an INNER JOIN between the trades and users tables. The join is based on the user_id field, meaning it links records from trades and users that have matching user_ids. This join is essential to associate each trade with a user's city.
* **WHERE trades.status = 'Completed'**
This WHERE clause filters the records to include only those trades whose status is 'Completed'. It ensures that only successfully completed trades are counted.
* **GROUP BY users.city**
Groups the results by users.city. This is necessary for the COUNT(trades.order_id) function to calculate the number of completed trades per city.
* **ORDER BY COUNT(trades.order_id) DESC**
Orders the grouped results by the count of trade orders in descending order. This means the city with the most completed trades will appear first.
* **LIMIT 3**
Limits the result to the top three records. Since the results are ordered by the count of completed trades in descending order, these will be the three cities with the highest number of completed trades.