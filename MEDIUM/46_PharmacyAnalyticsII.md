CVS Health is analyzing its pharmacy sales data, and how well different products are selling in the market. Each drug is exclusively manufactured by a single manufacturer.

> Write a query to identify the manufacturers associated with the drugs that resulted in losses for CVS Health and calculate the total amount of losses incurred.

Output the manufacturer's name, the number of drugs associated with losses, and the total losses in absolute value. Display the results sorted in descending order with the highest losses displayed at the top.

pharmacy_sales Table:

|Column Name|	Type
|--|--|
product_id|	integer
units_sold|	integer
total_sales	|decimal
cogs	|decimal
manufacturer|	varchar
drug	|varchar


pharmacy_sales Example Input:
product_id	|units_sold	|total_sales|	cogs	|manufacturer|	drug
|--|--|--|--|--|--|
156	|89514	|3130097.00|	3427421.73	|Biogen|	Acyclovir
25	|222331	|2753546.00	|2974975.36	|AbbVie	|Lamivudine and Zidovudine
50	|90484	|2521023.73	|2742445.90	|Eli Lilly	|Dermasorb TA Complete Kit
98	|110746	|813188.82	|140422.87	|Biogen	|Medi-Chord


Example Output:
manufacturer	|drug_count	|total_loss
|--|--|--|
Biogen	|1	|297324.73
AbbVie	|1	|221429.36
Eli Lilly	|1	|221422.17

Explanation:
The first three rows indicate that some drugs resulted in losses. Among these, Biogen had the highest losses, followed by AbbVie and Eli Lilly. However, the Medi-Chord drug manufactured by Biogen reported a profit and was excluded from the result.


***

```
SELECT manufacturer,
       COUNT(product_id) as drug_count,
       ABS(SUM(total_sales - cogs)) AS total_loss
FROM pharmacy_sales
WHERE (total_sales - cogs)<=0
GROUP BY manufacturer
ORDER BY total_loss DESC
```



ChatGPT
ChatGPT
This SQL query is designed to calculate and display the total loss and the count of drugs (products) for each manufacturer in a pharmacy_sales table, focusing specifically on those drugs that incurred losses. Here's a breakdown of the query:

* **SELECT manufacturer,**
This line selects the manufacturer column, representing different drug manufacturers.
* **COUNT(product_id) as drug_count,**
Counts the number of products (product_id) associated with each manufacturer that have incurred a loss. The result is aliased as drug_count.
* **ABS(SUM(total_sales - cogs)) AS total_loss**
    * Calculates the total loss for each manufacturer. The loss for each product is determined by subtracting the Cost of Goods Sold (cogs) from the Total Sales (total_sales). If this value is negative, it indicates a loss.
    * **SUM(total_sales - cogs)** sums up these losses for each manufacturer's products.
    * **ABS(...)** is used to ensure the total loss is a positive number. The result of this calculation is aliased as total_loss.
* **FROM pharmacy_sales**
Specifies that the data is being selected from the pharmacy_sales table.
* **WHERE (total_sales - cogs) <= 0**
This WHERE clause filters the records to include only those where the difference between total_sales and cogs is less than or equal to zero, indicating a loss or no profit.
* **GROUP BY manufacturer**
Groups the results by manufacturer. This is essential for the COUNT and SUM functions to calculate the drug count and total loss per manufacturer.
* **ORDER BY total_loss DESC**
Orders the results by the calculated total loss in descending order. This means the manufacturer with the highest total loss will be listed first.