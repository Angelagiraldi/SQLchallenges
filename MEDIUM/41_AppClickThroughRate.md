> Assume you have an events table on Facebook app analytics. Write a query to calculate the click-through rate (CTR) for the app in 2022 and round the results to 2 decimal places.

Definition and note:
Percentage of click-through rate (CTR) = 100.0 * Number of clicks / Number of impressions
To avoid integer division, multiply the CTR by 100.0, not 100.


events Table:

Column Name	| Type
| -- | -- |
app_id	| integer
event_type	| string
timestamp	| datetime

events Example Input:
app_id	| event_type| 	timestamp
|  -- |  -- | -- | 
123	| impression| 	07/18/2022 11:36:12
123	| impression	| 07/18/2022 11:37:12
123	| click	| 07/18/2022 11:37:42
234	| impression| 	07/18/2022 14:15:12
234	| click| 	07/18/2022 14:16:12

Example Output:

app_id	| ctr
| -- | -- | 
123	| 50.00
234	| 100.00

Explanation
Let's consider an example of App 123. This app has a click-through rate (CTR) of 50.00% because out of the 2 impressions it received, it got 1 click.

To calculate the CTR, we divide the number of clicks by the number of impressions, and then multiply the result by 100.0 to express it as a percentage. In this case, 1 divided by 2 equals 0.5, and when multiplied by 100.0, it becomes 50.00%. So, the CTR of App 123 is 50.00%.

***
A possible solution is:
```
SELECT 
  app_id,
  ROUND(100.0 * SUM(CASE 
    WHEN event_type = 'click' THEN 1 ELSE 0 END) 
  /SUM(CASE 
    WHEN event_type = 'impression' THEN 1 ELSE 0 END),2) AS ctr_rate
FROM events
WHERE timestamp >= '2022-01-01' 
  AND timestamp < '2023-01-01'
GROUP BY app_id
```

* **SELECT app_id**,
    This line selects the app_id column from the events table. Each app_id represents a different application.
* **ROUND(100.0 * SUM(CASE WHEN event_type = 'click' THEN 1 ELSE 0 END) / SUM(CASE WHEN event_type = 'impression' THEN 1 ELSE 0 END), 2) AS ctr_rate**
This complex expression calculates the CTR (Click-Through Rate) for each application:
    * **SUM(CASE WHEN event_type = 'click' THEN 1 ELSE 0 END)**: This part counts the number of 'click' events for each application. It uses a CASE statement within a SUM to increment the count by 1 whenever event_type is 'click'.
    * **SUM(CASE WHEN event_type = 'impression' THEN 1 ELSE 0 END)**: Similarly, this part counts the number of 'impression' events.
    * The formula **(Number of Clicks / Number of Impressions) * 100.0 **is used to calculate the CTR, which is then rounded to two decimal places. Multiplying by 100.0 converts the ratio to a percentage.
    The result of this calculation is aliased as ctr_rate.
* **FROM events**
Indicates that the data is being selected from the events table.
* **WHERE timestamp >= '2022-01-01' AND timestamp < '2023-01-01'**
This WHERE clause filters the data to include events that occurred in the year 2022. It includes records where timestamp is on or after January 1, 2022, and before January 1, 2023.
* **GROUP BY app_id**
Groups the results by app_id. This is necessary for the SUM functions to calculate the number of clicks and impressions per application.
Order and Output

The query does not explicitly specify an ORDER BY clause, so the results will be grouped by app_id but not ordered in any specific way.
