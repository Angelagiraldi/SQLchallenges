Assume you're given the table on user viewership categorised by device type where the three types are laptop, tablet, and phone.

> Write a query that calculates the total viewership for laptops and mobile devices where mobile is defined as the sum of tablet and phone viewership. Output the total viewership for laptops as laptop_reviews and the total viewership for mobile devices as mobile_views.


viewership Table

Column Name	Type
user_id	integer
device_type	string ('laptop', 'tablet', 'phone')
view_time	timestamp
viewership Example Input

user_id	device_type	view_time
123	tablet	01/02/2022 00:00:00
125	laptop	01/07/2022 00:00:00
128	laptop	02/09/2022 00:00:00
129	phone	02/09/2022 00:00:00
145	tablet	02/24/2022 00:00:00

***

A possible solution is:

```
SELECT 
  SUM(CASE WHEN device_type = 'laptop' THEN 1 ELSE 0 END) AS laptop_views, 
  SUM(CASE WHEN device_type IN ('tablet', 'phone') THEN 1 ELSE 0 END) AS mobile_views 
FROM viewership;
```

or 

```
SELECT 
  COUNT(*) FILTER (WHERE device_type = 'laptop') AS laptop_views,
  COUNT(*) FILTER (WHERE device_type IN ('tablet', 'phone'))  AS mobile_views 
FROM viewership;
```

To calculate the viewership on different devices (laptops vs. mobile devices), we can utilize the aggregate function COUNT() along with the FILTER clause to apply conditional expressions.
In the given example, the device types 'tablet' and 'phone' are considered as 'mobile' devices, while 'laptop' is treated as a In the first column laptop_views, COUNT(*) FILTER (WHERE device_type = 'laptop') calculates the count of rows where the device type is labeled as 'laptop'.

In the second column mobile_views, COUNT(*) FILTER (WHERE device_type IN ('tablet', 'phone')) counts the number of rows where the device type is a tablet or a phone.

The result would have two columns, laptop_views and mobile_views displaying the respective counts of views for each device type.separate device type.
