A phone call is considered an international call when the person calling is in a different country than the person receiving the call.

>What percentage of phone calls are international? Round the result to 1 decimal.

Assumption:
The caller_id in phone_info table refers to both the caller and receiver.

phone_calls Table:
caller_id	| receiver_id	| call_time
|--|--|--|
1	|2	|2022-07-04 10:13:49
1	|5	|2022-08-21 23:54:56
5	|1	|2022-05-13 17:24:06
5	|6	|2022-03-18 12:11:49

phone_info Table:
caller_id|	country_id	|network|	phone_number
|--|--|--|--|
1	|US|	Verizon	|+1-212-897-1964
2	|US	|Verizon	|+1-703-346-9529
3	|US	|Verizon	|+1-650-828-4774
4	|US	|Verizon	|+1-415-224-6663
5	|IN	|Vodafone	|+91 7503-907302
6	|IN	|Vodafone	|+91 2287-664895

Example Output:
international_calls_pct|
|--|
50.0

Explanation
There is a total of 4 calls with 2 of them being international calls (from caller_id 1 => receiver_id 5, and caller_id 5 => receiver_id 1). Thus, 2/4 = 50.0%

***

A possible solution is:
```
SELECT  
  ROUND(100.0 * SUM(CASE 
    WHEN caller.country_id <> receiver.country_id THEN 1 ELSE NULL END)
    /COUNT(*),1) as international_calls_pct
FROM phone_calls AS calls
LEFT JOIN phone_info AS caller
  ON calls.caller_id = caller.caller_id
LEFT JOIN phone_info AS receiver
  ON calls.receiver_id = receiver.caller_id
```


* **SELECT ROUND(100.0 * SUM(CASE WHEN caller.country_id <> receiver.country_id THEN 1 ELSE NULL END) / COUNT(*), 1) as international_calls_pct**
Calculates the percentage of international calls using the formula: international_calls_pct = (Sum of international calls / Total number of calls) ×100
    * The **CASE WHEN caller.country_id <> receiver.country_id THEN 1 ELSE NULL END** checks if the country IDs of the caller and receiver are different. If they are, it counts as an international call (1), otherwise, it is NULL.
    * **SUM(...)** calculates the total count of international calls.
    * **COUNT(*)** calculates the total number of calls.
    * **ROUND(..., 1)** rounds the result to one decimal place.
    * The result is aliased **as international_calls_pct**.
**FROM phone_calls AS calls** Specifies that the data is sourced from the phone_calls table.
* **LEFT JOIN phone_info AS caller ON calls.caller_id = caller.caller_id**
Joins the phone_calls table with the phone_info table (aliased as caller) based on the caller_id. This join is used to retrieve information about the caller.
* **LEFT JOIN phone_info AS receiver ON calls.receiver_id = receiver.caller_id**
Joins the phone_calls table again with the phone_info table (aliased as receiver) based on the receiver_id. This join is used to retrieve information about the receiver.