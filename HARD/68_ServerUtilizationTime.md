Amazon Web Services (AWS) is powered by fleets of servers. Senior management has requested data-driven solutions to optimize server usage.

> Write a query that calculates the total time that the fleet of servers was running. The output should be in units of full days.

Assumptions:
* Each server might start and stop several times.
* The total time in which the server fleet is running can be calculated as the sum of each server's uptime.

server_utilization Table:
server_id | status_time | session_status
| --|--|--|
1 |08/02/2022 10:00:00 |start
1 |08/04/2022 10:00:00 |stop
2 |08/17/2022 10:00:00 |start
2 |08/24/2022 10:00:00 |stop

Example Output:
total_uptime_days|
|--|
21|

Explanation
In the example output, the combined uptime of all the servers (from each start time to stop time) totals 21 full days.


***

A possible solution is:

```
With running_time AS (
  SELECT
  server_id,
  status_time as start_time,
  session_status,
  LEAD(status_time) OVER (
   PARTITION BY server_id
    ORDER BY status_time)
  AS stop_time
FROM server_utilization
)
SELECT DATE_PART('days', JUSTIFY_HOURS(SUM(stop_time-start_time))) AS total_uptime_days
FROM running_time
WHERE session_status = 'start' AND stop_time IS NOT NULL;
```

* CTE - running_time:
    This Common Table Expression (CTE) retrieves data from the server_utilization table and prepares it for further analysis.
        * It selects the server_id, status_time as start_time, session_status, and uses the LEAD window function to find the next status_time for the same server_id, ordered by status_time.
        * Essentially, it pairs each 'start' status with the subsequent 'stop' status for each server, defining the start and stop times of each server session.
* Main Query:
    * **SELECT DATE_PART('days', JUSTIFY_HOURS(SUM(stop_time-start_time))) AS total_uptime_days**
        * Calculates the total uptime of servers by summing up the differences between stop_time and start_time for each server and then converting the total time to days.
    * **DATE_PART('days', ...)** calculates the number of days from the time difference.
    * **JUSTIFY_HOURS** function adjusts the time difference to account for any hours that may exceed 24 hours, ensuring accurate day calculations.
    * **FROM running_time**
    Specifies that the data is sourced from the running_time CTE, which contains the paired start and stop times for each server session.
    * **WHERE session_status = 'start' AND stop_time IS NOT NULL**
    Filters the results to include only records where the session_status is 'start' (indicating the beginning of a session) and where there is a valid stop_time.
    This ensures that only complete server sessions are considered for uptime calculation.
