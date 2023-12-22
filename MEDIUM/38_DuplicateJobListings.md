Assume you're given a table containing job postings from various companies on the LinkedIn platform. 
> Write a query to retrieve the count of companies that have posted duplicate job listings.

Definition:
Duplicate job listings are defined as two job listings within the same company that share identical titles and descriptions.

job_listings Table:
Column Name	| Type
| -- | -- |
job_id	|integer
company_id	|integer
title	|string
description	|string


job_listings Example Input:
job_id|	company_id	|title	|description
| -- | -- |-- | -- |
248	|827	|Business Analyst	|Business analyst evaluates past and current business data with the primary goal of improving decision-making processes within organizations.
149	|845	|Business Analyst	|Business analyst evaluates past and current business data with the primary goal of improving decision-making processes within organizations.
945|	345	|Data Analyst|	Data analyst reviews data to identify key insights into a business's customers and ways the data can be used to solve problems.
164	|345	|Data Analyst	|Data analyst reviews data to identify key insights into a business's customers and ways the data can be used to solve problems.
172|	244	|Data Engineer	|Data engineer works in a variety of settings to build systems that collect, manage, and convert raw data into usable information for data scientists and business analysts to interpret.

Example Output:

|duplicate_companies|
|--|
1

Explanation:

There is one company ID 345 that posted duplicate job listings. The duplicate listings, IDs 945 and 164 have identical titles and descriptions.

*** 

A possible solution is:

```
WITH job_count_cte AS (
SELECT 
  company_id, 
  title, 
  description, 
  COUNT(job_id) AS job_count
FROM job_listings
GROUP BY title, description, company_id
)
SELECT COUNT(DISTINCT company_id) AS duplicate_companies
FROM job_count_cte
WHERE job_count>1
```

* **WITH job_count_cte AS (** Begins a Common Table Expression (CTE) named job_count_cte. A CTE creates a temporary result set that can be referred to within the SQL statement.
    * **SELECT company_id, title, description, COUNT(job_id) AS job_count** Within the CTE, this SELECT statement chooses the company_id, title, and description columns from the job_listings table.
    * **COUNT(job_id) AS job_count** computes the number of job listings (job_id) for each unique combination of company_id, title, and description. The alias job_count is used for this count.
    * **FROM job_listings** Indicates that the data is sourced from the job_listings table.
    * **GROUP BY title, description, company_id** The data is grouped by title, description, and company_id, which is necessary for the COUNT function to accurately calculate the number of job listings for each group.
* **)** Ends the CTE definition.
* **SELECT COUNT(DISTINCT company_id) AS duplicate_companies** After defining the CTE, this line counts the distinct company_ids. The DISTINCT keyword ensures that each company_id is counted only once, regardless of how many duplicate job listings it might have.
The result of this count is aliased as duplicate_companies.
* **FROM job_count_cte** Specifies that the data for this portion of the query is sourced from the CTE job_count_cte.
* **WHERE job_count>1** This WHERE clause filters the results to include only those records where job_count is greater than 1, i.e., where a company has listed the same job (same title and description) more than once.