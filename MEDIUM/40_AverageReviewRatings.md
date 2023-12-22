> Given the reviews table, write a query to retrieve the average star rating for each product, grouped by month. The output should display the month as a numerical value, product ID, and average star rating rounded to two decimal places. Sort the output first by month and then by product ID.


reviews Table:

Column Name	| Type
| -- | --|
review_id	| integer
user_id	| integer
submit_date	| datetime
product_id	| integer
stars	| integer (1-5)

reviews Example Input:
review_id	| user_id| 	submit_date	| product_id	| stars
|  -- | --| --| --| --| 
6171	| 123	| 06/08/2022 | 00:00:00| 	50001| 	4
7802	| 265	| 06/10/2022 | 00:00:00| 	69852| 	4
5293	| 362	| 06/18/2022 | 00:00:00| 	50001| 	3
6352	| 192	| 07/26/2022 | 00:00:00| 	69852| 	3
4517	| 981	| 07/05/2022 | 00:00:00| 	69852| 	2

Example Output:

mth	| product	| avg_stars
| --| --| --| 
6	| 50001	| 3.50
6	| 69852	| 4.00
7	| 69852	| 2.50

Explanation
Product 50001 received two ratings of 4 and 3 in the month of June (6th month), resulting in an average star rating of 3.5.


***

A possible solution is:
```
SELECT 
  EXTRACT(MONTH FROM submit_date) AS mth,
  product_id,
  ROUND(AVG(stars), 2) AS avg_stars
FROM reviews
GROUP BY  EXTRACT(MONTH FROM submit_date), product_id
ORDER BY EXTRACT(MONTH FROM submit_date), product_id
```

* **SELECT EXTRACT(MONTH FROM submit_date) AS mth, product_id, ROUND(AVG(stars),  AS avg_stars**
    * EXTRACT(MONTH FROM submit_date) AS mth: This extracts the month part from the submit_date column (which presumably represents the date when the review was submitted) and aliases it as mth.
    * product_id: Selects the product_id associated with each review.
    * ROUND(AVG(stars), 2) AS avg_stars: Calculates the average star rating (stars) for each group of reviews, rounds it to 2 decimal places, and aliases this value as avg_stars.
* **FROM reviews**
Specifies that the data is being selected from the reviews table.
* **GROUP BY EXTRACT(MONTH FROM submit_date), product_id**
Groups the data by the month of the submit_date and product_id. This is essential for calculating the average star rating per product per month. The grouping ensures that the average is computed for each product separately within each month.
* **ORDER BY EXTRACT(MONTH FROM submit_date), product_id**
Orders the results first by the month (extracted from submit_date) and then by product_id. This sorting means the output will be organized in a chronological monthly order, and within each month, the products will be ordered by their product_id.
