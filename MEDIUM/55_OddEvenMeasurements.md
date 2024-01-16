Assume you're given a table with measurement values obtained from a Google sensor over multiple days with measurements taken multiple times within each day.

>Write a query to calculate the sum of odd-numbered and even-numbered measurements separately for a particular day and display the results in two different columns. Refer to the Example Output below for the desired format.

measurements Table:

|measurement_id|	measurement_value	|measurement_time
|--|--|--|
131233	|1109.51	|07/10/2022 09:00:00
135211	|1662.74	|07/10/2022 11:00:00
523542	|1246.24	|07/10/2022 13:15:00
143562	|1124.50	|07/11/2022 15:00:00
346462	|1234.14	|07/11/2022 16:45:00

Example Output:

measurement_day	|odd_sum	|even_sum
|--|--|--|
07/10/2022 00:00:00|	2355.75	|1662.74
07/11/2022 00:00:00	|1124.50	|1234.14

Based on the results,
* On 07/10/2022, the sum of the odd-numbered measurements is 2355.75, while the sum of the even-numbered measurements is 1662.74.
* On 07/11/2022, there are only two measurements available. The sum of the odd-numbered measurements is 1124.50, and the sum of the even-numbered measurements is 1234.14.

***

A possible solution is:
```
WITH ranked_measurements AS (
    SELECT  
    CAST(measurement_time AS DATE) AS measurement_day,
    measurement_value,
    ROW_NUMBER() OVER(
        PARTITION BY CAST(measurement_time AS DATE) 
        ORDER BY measurement_time) AS measurement_num
    FROM measurements
)
SELECT
  measurement_day, 
  SUM(measurement_value) FILTER (WHERE measurement_num % 2 != 0) AS odd_sum, 
  SUM(measurement_value) FILTER (WHERE measurement_num % 2 = 0) AS even_sum 
FROM ranked_measurements
GROUP BY measurement_day;
```

* CTE Definition - **WITH ranked_measurements AS (...)**:
    * The CTE selects the measurement_day (casting measurement_time as DATE), measurement_value, and assigns a rank (measurement_num) to each measurement within each day.
    * **ROW_NUMBER() OVER(PARTITION BY CAST(measurement_time AS DATE) ORDER BY measurement_time)** assigns a unique rank to each measurement within each day based on the measurement time.
* Main Query:
    * **SELECT measurement_day, SUM(measurement_value) FILTER (WHERE measurement_num % 2 != 0) AS odd_sum, SUM(measurement_value) FILTER (WHERE measurement_num % 2 = 0) AS even_sum**
    Selects the measurement_day along with the sum of measurement values for odd ranks (odd_sum) and even ranks (even_sum).
    **FILTER (WHERE measurement_num % 2 != 0)** calculates the sum for measurements with odd ranks.
    **FILTER (WHERE measurement_num % 2 = 0)** calculates the sum for measurements with even ranks.
    * **FROM ranked_measurements**
    Indicates that the data is being selected from the CTE ranked_measurements.
    * **GROUP BY measurement_day**
    Groups the results by measurement_day to calculate the sums for odd and even ranks separately for each day.