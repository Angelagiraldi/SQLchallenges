Google's marketing team is making a Superbowl commercial and needs a simple statistic to put on their TV ad: the median number of searches a person made last year.

However, at Google scale, querying the 2 trillion searches is too costly. Luckily, you have access to the summary table which tells you the number of searches made last year and how many Google users fall into that bucket.

>Write a query to report the median of searches made by a user. Round the median to one decimal point.

search_frequency Example Input:

searches	|num_users
|--|--|
1|	2
2|	2
3|	3
4|	1

Example Output:

|median
|--|
2.5|

By expanding the search_frequency table, we get [1, 1, 2, 2, 3, 3, 3, 4] which has a median of 2.5 searches per user.

***

A possible solution is:
```
WITH searches_expanded AS (
SELECT searches
FROM search_frequency
GROUP BY searches,
  GENERATE_SERIES(1, num_users)
)
SELECT 
  PERCENTILE_CONT(0.5) 
    WITHIN GROUP (
      ORDER BY searches) AS median
FROM searches_expanded;
```

* **CTE - searches_expanded** (Expanded Search Frequency):
    * The CTE selects the searches column from the search_frequency table.
    * It uses the **GENERATE_SERIES** function to create a series of numbers from 1 to num_users. This effectively replicates each searches value multiple times based on the number of users.
    * The purpose of this CTE is to expand the dataset by repeating each search frequency value for the specified number of users.
* Main Query:
    * **SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY searches) AS median**
        * Calculates the median search frequency using the PERCENTILE_CONT function.
        * **PERCENTILE_CONT(0.5)** calculates the median, which represents the value that separates the dataset into two equal halves.
        * **WITHIN GROUP (ORDER BY searches)** specifies that the calculation should be based on the searches column and ordered in ascending order.
    * **FROM searches_expanded** 
    Indicates that the data is sourced from the CTE searches_expanded, which contains the expanded search frequency values.