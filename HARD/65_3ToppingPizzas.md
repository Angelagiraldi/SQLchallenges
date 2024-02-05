You’re a consultant for a major pizza chain that will be running a promotion where all 3-topping pizzas will be sold for a fixed price, and are trying to understand the costs involved.

>Given a list of pizza toppings, consider all the possible 3-topping pizzas, and print out the total cost of those 3 toppings. Sort the results with the highest total cost on the top followed by pizza toppings in ascending order.

Break ties by listing the ingredients in alphabetical order, starting from the first ingredient, followed by the second and third.

P.S. Be careful with the spacing (or lack of) between each ingredient. Refer to our Example Output.

Notes:
Do not display pizzas where a topping is repeated. For example, ‘Pepperoni,Pepperoni,Onion Pizza’.
Ingredients must be listed in alphabetical order. For example, 'Chicken,Onions,Sausage'. 'Onion,Sausage,Chicken' is not acceptable.

pizza_toppings Table:

topping_name	| ingredient_cost
|--|--|
Pepperoni|	0.50
Sausage|	0.70
Chicken|	0.55
Extra Cheese	|0.40

Example Output:

pizza	|total_cost
|--|--|
Chicken,Pepperoni,Sausage	|1.75
Chicken,Extra Cheese,Sausage	|1.65
Extra Cheese,Pepperoni,Sausage	|1.60
Chicken,Extra Cheese,Pepperoni	|1.45

Explanation
* There are four different combinations of the three toppings. Cost of the pizza with toppings Chicken, Pepperoni and Sausage is $0.55 + $0.50 + $0.70 = $1.75.
* Additionally, they are arranged alphabetically; in the dictionary, the chicken comes before pepperoni and pepperoni comes before sausage.


***

A possible solution is:
```
SELECT
  CONCAT(p1.topping_name,',', p2.topping_name,',', p3.topping_name ) AS pizza,
  (p1.ingredient_cost + p2.ingredient_cost + p3.ingredient_cost) AS total_cost
FROM pizza_toppings AS p1
CROSS JOIN
  pizza_toppings AS p2,
  pizza_toppings AS p3
WHERE p1.topping_name < p2.topping_name
  AND p2.topping_name < p3.topping_name
ORDER BY total_cost DESC, pizza;
```


* **SELECT CONCAT(p1.topping_name,',', p2.topping_name,',', p3.topping_name ) AS pizza, ...**
    * The SELECT clause constructs a string representing a pizza with three toppings by concatenating topping_name from p1, p2, and p3. This string is aliased as pizza.
    * It also calculates the total_cost by summing the ingredient_cost from p1, p2, and p3.
* **FROM pizza_toppings AS p1 CROSS JOIN pizza_toppings AS p2, pizza_toppings AS p3**
    The FROM clause performs a CROSS JOIN between three instances of the pizza_toppings table, aliased as p1, p2, and p3. This generates combinations of three toppings.
* **WHERE p1.topping_name < p2.topping_name AND p2.topping_name < p3.topping_name**
    The WHERE clause filters the combinations to ensure that the topping_name in each instance (p1, p2, p3) is in ascending order, preventing duplicate combinations.
* **ORDER BY total_cost DESC, pizza**   
    The result is ordered in descending order of total_cost (highest cost first) and then by the pizza string to alphabetically order pizzas with the same cost.