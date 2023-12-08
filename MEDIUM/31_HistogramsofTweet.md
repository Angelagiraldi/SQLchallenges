Assume you're given a table with Twitter tweet data, write a query to obtain a histogram of tweets posted per user in 2022. Output the tweet count per user as the bucket and the number of Twitter users who fall into that bucket.

In other words, group the users by the number of tweets they posted in 2022 and count the number of users in each group.

|tweets | | 
| --| -- | 
|Column Name |	Type
|tweet_id|	integer
|user_id|	integer
|msg|	string
|tweet_date|	timestamp

As example:

| tweet_id	| user_id| 	msg	| tweet_date|
| -- | -- | -- | --|
 214252	| 111	| Am considering taking Tesla private at $420. Funding secured.| 	12/30/2021 00:00:00
739252	| 111	| Despite the constant negative press covfefe | 	01/01/2022 00:00:00
846402	| 111	| Following @NickSinghTech on Twitter changed my life!	| 02/14/2022 00:00:00
241425	| 254 | 	If the salary is so competitive why won’t you tell me what it is?	| 03/01/2022 00:00:00
231574	| 148 |	I no longer have a manager. I can't be managed	| 03/23/2022 00:00:00


Example Output:
| tweet_bucket	| users_num |
| -- | -- |
| 1	| 2 |
| 2	| 1 |

Explanation:
Based on the example output, there are two users who posted only one tweet in 2022, and one user who posted two tweets in 2022. The query groups the users by the number of tweets they posted and displays the number of users in each group.

The dataset you are querying against may have different input & output - this is just an example!

***

A possible solution:
```
SELECT 
  tweet_count_per_user AS tweet_bucket, 
  COUNT(user_id) AS users_num 
FROM (
  SELECT 
    user_id, 
    COUNT(tweet_id) AS tweet_count_per_user 
  FROM tweets 
  WHERE tweet_date BETWEEN '2022-01-01' AND '2022-12-31' 
  GROUP BY user_id) AS total_tweets 
GROUP BY tweet_count_per_user;
```

This SQL code is used to retrieve information about the number of users falling into different "tweet_count" buckets based on the total count of tweets made by each user within a specified date range (from January 1, 2022, to December 31, 2022).

Here's a breakdown of the code:
* **Inner Query (Total Tweets per User)**:
The inner query starts with the *tweets* table and filters the data for tweets made between January 1, 2022, and December 31, 2022, using the WHERE clause (tweet_date BETWEEN '2022-01-01' AND '2022-12-31').
It then groups the tweets by *user_id* using the GROUP BY clause.
For each user, it calculates the total count of tweets made by that user (COUNT(tweet_id) AS tweet_count_per_user).
* **Outer Query (Bucketing Users by Tweet Count)**:
The outer query uses the results from the inner query (*total_tweets*) as a derived table.
It takes the *tweet_count_per_user* column (representing the total tweet count per user) and groups the users based on this count using the GROUP BY clause.
It then counts the number of users falling into each tweet count bucket using COUNT(user_id) AS *users_num*.