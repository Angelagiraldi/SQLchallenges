Assume you're given a table containing information on Facebook user actions. 
>Write a query to obtain number of monthly active users (MAUs) in July 2022, including the month in numerical format "1, 2, 3".

An active user is defined as a user who has performed actions such as 'sign-in', 'like', or 'comment' in both the current month and the previous month.

user_actions Table:
|user_id	|event_id	|event_type|	event_date
|--|--|--|--|
445	|7765	|sign-in	|05/31/2022 12:00:00
742	|6458	|sign-in	|06/03/2022 12:00:00
445	|3634	|like	|06/05/2022 12:00:00
742	|1374	|comment	|06/05/2022 12:00:00
648	|3124	|like	|06/18/2022 12:00:00

Example Output for June 2022:

month	|monthly_active_users
|--|--|
6	|1

In June 2022, there was only one monthly active user (MAU) with the user_id 445.

***

A possible solution is:
```
SELECT 
  EXTRACT(MONTH FROM curr_month.event_date) as month,
  COUNT(DISTINCT curr_month.user_id) AS monthly_active_users
FROM user_actions AS curr_month
WHERE EXISTS (
  SELECT last_month.user_id
  FROM user_actions AS last_month
  WHERE last_month.user_id = curr_month.user_id
    AND EXTRACT(MONTH FROM last_month.event_date) = EXTRACT(MONTH FROM curr_month.event_date - interval '1 month')
    
)
  AND EXTRACT(MONTH FROM curr_month.event_date) = 7
  AND EXTRACT(YEAR FROM curr_month.event_date) = 2022
GROUP BY EXTRACT(MONTH FROM curr_month.event_date);
```

* **SELECT EXTRACT(MONTH FROM curr_month.event_date) as month, COUNT(DISTINCT curr_month.user_id) AS monthly_active_users** Selects the month extracted from the event_date and the count of distinct user IDs from the current month (curr_month). The result is aliased as month and monthly_active_users.
* **FROM user_actions AS curr_month** Specifies that the data is sourced from the user_actions table, aliased as curr_month.
* **WHERE EXISTS (...)** Utilizes the EXISTS clause to filter rows where there exists a user action in the previous month (last_month) for the same user. The subquery checks for a user action in the previous month based on the condition:
    * **last_month.user_id = curr_month.user_id**: The user IDs must match.
    * **EXTRACT(MONTH FROM last_month.event_date) = EXTRACT(MONTH FROM curr_month.event_date - interval '1 month')**: The event date of the user action in the previous month should match the month before the current month.
* **AND EXTRACT(MONTH FROM curr_month.event_date) = 7 AND EXTRACT(YEAR FROM curr_month.event_date) = 2022** Further filters the results to include only those rows where the month extracted from curr_month.event_date is July (7) and the year is 2022.
* **GROUP BY EXTRACT(MONTH FROM curr_month.event_date)** Groups the results by the month extracted from curr_month.event_date.