>Given a table of Facebook posts, for each user who posted at least twice in 2021, write a query to find the number of days between each user’s first post of the year and last post of the year in the year 2021. Output the user and number of the days between each user's first and last post.

*posts* Table:

| Column  Name	| Type
| --  | -- |
user_id	| integer
post_id	| integer
post_date |	timestamp
post_content |	text

posts Example Input:

user_id	| post_id |	post_date	| post_content
| -- | -- | -- | -- |
151652|599415	|07/10/2021 12:00:00	|Need a hug
661093	|624356	|07/29/2021 13:00:00	|Bed. Class 8-12. Work 12-3. Gym 3-5 or 6. Then class 6-10. Another day that's gonna fly by. I miss my girlfriend
004239	|784254	|07/04/2021 11:00:00	|Happy 4th of July!
661093	|442560	|07/08/2021 14:00:00	|Just going to cry myself to sleep after watching Marley and Me.
151652	|111766	|07/12/2021 19:00:00	|I'm so done with covid - need travelling ASAP!

Example Output:

user_id	|days_between
| -- | -- |
151652	|2
661093	|21

***

A possible solution is:

```
SELECT 
    user_id,
    MAX(post_date::DATE) - MIN(post_date::DATE) AS days_between
FROM posts
WHERE DATE_PART('year', post_date::date) = 2021
GROUP BY user_id
HAVING COUNT(post_id) > 1;
```


* SELECT Clause:
    * user_id: This selects the user_id column from the posts table.
    * MAX(post_date::DATE) - MIN(post_date::DATE) AS days_between: This calculates the number of days between the earliest (MIN) and the latest (MAX) post_date for each user. The post_date::DATE converts post_date to a date format if it's not already in that format. The result is aliased as days_between.
* FROM Clause:
    * This specifies that the data is being retrieved from the posts table.
* WHERE Clause:
    * This filters the data to include only those rows where the year part of post_date is 2021. 
* GROUP BY Clause:
    * This clause groups the results based on user_id. It's essential for aggregate functions like MAX, MIN, and COUNT to work correctly in this context. Each group corresponds to a unique user_id, and the aggregate functions (like calculating the days between the earliest and latest post) are applied within each group.
* HAVING Clause:
    * The HAVING clause is used to filter groups created by the GROUP BY clause. It comes after GROUP BY.
    * COUNT(post_id) > 1: This part of the query counts the number of posts (post_id) for each user and includes only those users who have more than one post. Essentially, this filters out any user who only posted once in 2021.


In summary, the entire query is designed to:
1) Select each user (user_id) from the posts table.
2) Calculate the number of days between their first and last post in 2021.
3) Include only those users who have posted more than once in the year 2021.