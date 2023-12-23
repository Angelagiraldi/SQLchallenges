CVS Health is trying to better understand its pharmacy sales, and how well different products are selling. Each drug can only be produced by one manufacturer.

>Write a query to find the top 3 most profitable drugs sold, and how much profit they made. Assume that there are no ties in the profits. Display the result from the highest to the lowest total profit.

Definition:
* cogs stands for Cost of Goods Sold which is the direct cost associated with producing the drug.
* Total Profit = Total Sales - Cost of Goods Sold


pharmacy_sales Table:
Column Name	|Type
|--|--|
product_id	|integer
units_sold	|integer
total_sales	|decimal
cogs	|decimal
manufacturer	|varchar
drug	|varchar

pharmacy_sales Example Input:
product_id	|units_sold	|total_sales	|cogs	|manufacturer	|drug
|--|--|--|--|--|--|
9	|37410|	293452.54|	208876.01	|Eli Lilly	|Zyprexa
34	|94698|	600997.19	|521182.16	|AstraZeneca	|Surmontil
61	|77023|	500101.61	|419174.97	|Biogen	|Varicose Relief
136	|144814|	1084258|	1006447.73	|Biogen	|Burkhart

Example Output:

drug	|total_profit
|--|--|
Zyprexa	|84576.53
Varicose Relief	|80926.64
Surmontil	|79815.03

Explanation:
Zyprexa made the most profit (of $84,576.53) followed by Varicose Relief (of $80,926.64) and Surmontil (of $79,815.3).


***

A solution could be:
```
SELECT drug,
    (total_sales - cogs) AS total_profit
FROM pharmacy_sales
ORDER BY (total_sales - cogs) DESC
LIMIT 3
```

* **SELECT drug,**
This line selects the drug column, which represents different types of drugs.
* **(total_sales - cogs) AS total_profit**
For each drug, the query calculates the total profit by subtracting the Cost of Goods Sold (cogs) from the Total Sales (total_sales).
The result of this calculation is aliased as total_profit.
* **FROM pharmacy_sales**
Specifies that the data is being selected from the pharmacy_sales table.
* **ORDER BY (total_sales - cogs) DESC**
Orders the results by the calculated total profit in descending order. This means the drug with the highest profit will be listed first.
* **LIMIT 3**
Limits the results to the top three records. Since the results are ordered by total profit in descending order, these will be the three drugs with the highest profits.