Assume you're given tables with information on Snapchat users, including their ages and time spent sending and opening snaps.

>Write a query to obtain a breakdown of the time spent sending vs. opening snaps as a percentage of total time spent on these activities grouped by age group. Round the percentage to 2 decimal places in the output.

Calculate the following percentages:
* time spent sending / (Time spent sending + Time spent opening)
* Time spent opening / (Time spent sending + Time spent opening)


activities Table
activity_id	|user_id |	activity_type |	time_spent |	activity_date
| -- | -- | -- | -- | -- |
7274	| 123	| open |	4.50	|06/22/2022 12:00:00
2425	| 123	| send	| 3.50	| 06/22/2022 12:00:00
1413	|456	|send	|5.67	|06/23/2022 12:00:00
1414	|789	|chat	|11.00	|06/25/2022 12:00:00
2536	|456	|open	|3.00	|06/25/2022 12:00:00


user_id|	age_bucket
|--|--|
123	|31-35
456	|26-30
789	|21-25

Example Output
age_bucket	|send_perc	|open_perc
|--|--|--|
26-30	|65.40	|34.60
31-35	|43.75	|56.25

Explanation
Using the age bucket 26-30 as example, the time spent sending snaps was 5.67 and the time spent opening snaps was 3.
To calculate the percentage of time spent sending snaps, we divide the time spent sending snaps by the total time spent on sending and opening snaps, which is 5.67 + 3 = 8.67.
So, the percentage of time spent sending snaps is 5.67 / (5.67 + 3) = 65.4%, and the percentage of time spent opening snaps is 3 / (5.67 + 3) = 34.6%.

***

A possible solution is:

```
SELECT age_breakdown.age_bucket,
    ROUND(100.0 * SUM(activities.time_spent) FILTER (WHERE activities.activity_type = 'send')/
      SUM(activities.time_spent),2) AS send_perc,   
    ROUND(100.0 * SUM(activities.time_spent) FILTER (WHERE activities.activity_type = 'open')/
      SUM(activities.time_spent),2) AS open_perc
FROM activities
INNER JOIN age_breakdown
  ON activities.user_id = age_breakdown.user_id
WHERE activities.activity_type IN ('send', 'open')
GROUP BY age_breakdown.age_bucket
```

* **SELECT age_breakdown.age_bucket**,
    This line selects the age_bucket column from the age_breakdown table, indicating different age groups or ranges.
**ROUND(100.0 * SUM(activities.time_spent) FILTER (WHERE activities.activity_type = 'send')/ SUM(activities.time_spent),2) AS send_perc**,
This part calculates the percentage of time spent on the 'send' activity within each age bucket.
    * **SUM(activities.time_spent) FILTER (WHERE activities.activity_type = 'send')**: Sums up the time spent on activities where the activity_type is 'send'.
    * *SUM(activities.time_spent)***: Calculates the total time spent on all activities (but since the WHERE clause filters for 'send' and 'open' only, it effectively sums up time for these two activities).
    * The ratio of these two sums is multiplied by 100.0 to convert it into a percentage and then rounded to two decimal places.
    * The result is aliased as send_perc.
* **ROUND(100.0 * SUM(activities.time_spent) FILTER (WHERE activities.activity_type = 'open')/ SUM(activities.time_spent),2) AS open_perc** 
    Similar to the previous calculation, but this time for the 'open' activity.
    The ratio of time spent on 'open' activities to the total time spent on 'send' and 'open' activities is calculated and expressed as a percentage, aliased as open_perc.
* **FROM activities INNER JOIN age_breakdown ON activities.user_id = age_breakdown.user_id**
The activities and age_breakdown tables are joined on user_id. This joins each activity with the corresponding user's age group.
* **WHERE activities.activity_type IN ('send', 'open')**
This WHERE clause filters the data to include only rows where the activity_type is either 'send' or 'open'.
* **GROUP BY age_breakdown.age_bucket**
Groups the results by the age_bucket. This is essential for the SUM functions to calculate the percentages for each age group.
* The query does not explicitly specify an ORDER BY clause, so the results will be grouped by age_bucket but not ordered in any specific way.