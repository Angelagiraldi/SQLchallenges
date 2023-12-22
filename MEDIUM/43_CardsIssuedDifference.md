Your team at JPMorgan Chase is preparing to launch a new credit card, and to gain some insights, you're analyzing how many credit cards were issued each month.

> Write a query that outputs the name of each credit card and the difference in the number of issued cards between the month with the highest issuance cards and the lowest issuance. Arrange the results based on the largest disparity.

monthly_cards_issued Table:

Column Name	| Type
| -- | --|
issue_month	|integer
issue_year	|integer
card_name	|string
issued_amount	|integer

monthly_cards_issued Example Input:
card_name	|issued_amount	|issue_month	|issue_year
|--|--|--|--|
Chase Freedom Flex	|55000	|1	|2021
Chase Freedom Flex	|60000	|2	|2021
Chase Freedom Flex	|65000	|3	|2021
Chase Freedom Flex	|70000	|4	|2021
Chase Sapphire Reserve|	170000	|1	|2021
Chase Sapphire Reserve	|175000	|2	|2021
Chase Sapphire Reserve	|180000	|3	|2021

Example Output:

card_name	|difference
|--|--|
Chase Freedom Flex	|15000
Chase Sapphire Reserve	|10000

Explanation:

Chase Freedom Flex's best month was 70k cards issued and the worst month was 55k cards, so the difference is 15k cards.

Chase Sapphire Reserve’s best month was 180k cards issued and the worst month was 170k cards, so the difference is 10k cards.

***

A possible solution is:
```
SELECT  
card_name,
MAX(issued_amount) - MIN(issued_amount) AS difference
FROM monthly_cards_issued
GROUP BY card_name
ORDER BY difference DESC
```

* **SELECT card_name,**
This line selects the card_name column, which likely represents different types of cards (e.g., credit cards, debit cards, etc.).
* **MAX(issued_amount) - MIN(issued_amount) AS difference**
For each type of card (card_name), the query calculates the difference between the maximum (MAX) and minimum (MIN) issued amounts. The issued amount probably represents the value or number of cards issued.
The result of this calculation is aliased as difference.
* **FROM monthly_cards_issued**
Specifies that the data is being selected from the monthly_cards_issued table.
* **GROUP BY card_name**
Groups the results by card_name. This is necessary because the MAX and MIN functions are aggregate functions that operate on a group of rows. Grouping by card_name means the calculation of the difference in issued amounts is done separately for each card type.
* **ORDER BY difference DESC**
Orders the results by the calculated difference in descending order. This means the card type with the largest difference between the maximum and minimum issued amounts will be listed first.