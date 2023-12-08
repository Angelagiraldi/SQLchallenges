Assume you're given two tables containing data about Facebook Pages and their respective likes (as in "Like a Facebook Page").

> Write a query to return the IDs of the Facebook pages that have zero likes. The output should be sorted in ascending order based on the page IDs.

*pages* Table:

Column Name	| Type
| -- | -- |
page_id	| integer
page_name	| varchar

And an example Input:

| pages | | 
| -- | -- |
page_id	| page_name 
20001	| SQL Solutions
20045	| Brain Exercises
20701	| Tips for Data Analysts


*page_likes* Table:

Column Name	| Type
| -- | -- |
user_id	| integer
page_id	| integer
liked_date |	datetime

And an example Input:

| page_likes  | | |
| -- | --| -- | 
user_id	| page_id	| liked_date
111	| 20001	| 04/08/2022 00:00:00
121| 	20045	 | 03/12/2022 00:00:00
156	|20001	| 07/25/2022 00:00:00


Example Output:
|page_id|
| -- |
20701

The dataset you are querying against may have different input & output - this is just an example!


***

A possible solution is:

```
SELECT pages.page_id
FROM pages
LEFT JOIN page_likes 
    ON pages.page_id = page_likes.page_id
WHERE page_likes.page_id is NULL
```


This SQL query is designed to retrieve the *page_ids* from the *pages* table where there are no corresponding entries in the *page_likes* table. Here's a breakdown of the query:

* **Selection of Page IDs**:
The SELECT statement retrieves the *page_id* column from the *pages* table.
**Joining Tables**:
The query uses a LEFT JOIN operation to combine the *pages* table with the *page_likes* table based on the condition *pages.page_id = page_likes.page_id*.
This type of join ensures that all records from the pages table are included in the result set, regardless of whether there are matching entries in the page_likes table.
* **Filtering Non-Matching Entries**:
The WHERE clause filters the joined results to only include rows where there is no match found in the *page_likes* table for the *page_id*. It specifically looks for cases where *page_likes.page_id* is NULL, indicating that there are no corresponding entries in page_likes for those page_ids from the pages table.

> Uses a LEFT JOIN to ensure inclusion of all page_ids from the pages table, regardless of their presence in the page_likes table.