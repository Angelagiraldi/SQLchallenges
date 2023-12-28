> Given a table of tweet data over a specified time period, calculate the 3-day rolling average of tweets for each user. Output the user ID, tweet date, and rolling averages rounded to 2 decimal places.

Notes:
* A rolling average, also known as a moving average or running mean is a time-series technique that examines trends in data over a specified period of time.
In this case, we want to determine how the tweet count for each user changes over a 3-day period.

tweets Table:
Column Name	| Type
| -- | -- |
user_id |	integer
tweet_date	|timestamp
tweet_count	|integer

tweets Example Input:
user_id	| tweet_date |	tweet_count
| -- | -- | -- |
111	|06/01/2022 00:00:00	|2
111	|06/02/2022 00:00:00	|1
111	|06/03/2022 00:00:00	|3
111	|06/04/2022 00:00:00	|4
111	|06/05/2022 00:00:00	|5

Example Output:
user_id	|tweet_date	|rolling_avg_3d
|--|--|--|
111	|06/01/2022 00:00:00	|2.00
111	|06/02/2022 00:00:00	|1.50
111	|06/03/2022 00:00:00	|2.00
111	|06/04/2022 00:00:00	|2.67
111	|06/05/2022 00:00:00	|4.00

*** 

A solution is:
```
SELECT 
  user_id,
  tweet_date,
  ROUND(AVG(tweet_count) OVER(
    PARTITION BY user_id
    ORDER BY tweet_date
    ROWS BETWEEN 2 PRECEDING AND CURRENT ROW),2) AS rolling_avg
FROM tweets
```

* **SELECT user_id, tweet_date,**
This line selects the user_id and tweet_date from the tweets table. user_id identifies the user, and tweet_date indicates when the tweet was posted.
* **ROUND(AVG(tweet_count) OVER(
AVG(tweet_count) OVER(...)** is a window function that calculates the average tweet_count.
    * **ROUND(...,2)** rounds this average to two decimal places.
    * **PARTITION BY user_id**
    The PARTITION BY clause divides the data into partitions based on user_id. The window function (rolling average calculation) is applied within each partition independently. In other words, each user’s data is treated separately.
    * **ORDER BY tweet_date**
    The ORDER BY clause within the OVER clause specifies the order in which the rows are considered for the rolling average. In this case, it's ordered by tweet_date, meaning the calculation considers the chronological order of tweets.
    * **ROWS BETWEEN 2 PRECEDING AND CURRENT ROW**
    This clause defines the window frame for the rolling average calculation. For each row, the window includes the current row and the two preceding rows. If fewer than two preceding rows exist (like at the start of the data set), it includes as many as are available.
    * **AS rolling_avg**
    The result of the window function is given an alias rolling_avg.
* **FROM tweets**
Indicates that the data is sourced from the tweets table.