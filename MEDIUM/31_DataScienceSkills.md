Given a table of candidates and their skills, you're tasked with finding the candidates best suited for an open Data Science job. You want to find candidates who are proficient in Python, Tableau, and PostgreSQL.

>Write a query to list the candidates who possess all of the required skills for the job. Sort the output by candidate ID in ascending order.

Assumption: There are no duplicates in the candidates table. 

The candidates Table looks like:
| Column Name	| Type |
| -- | -- |
| candidate_id |	integer
skill	|varchar

A *candidates* example Input:

|candidates | |
| -- | -- |
| candidate_id	| skill |
123	|Python
123	|Tableau
123	|PostgreSQL
234	|R
234	|PowerBI
234	|SQL Server
345	|Python
345	|Tableau

Example Output:

|candidate_id |
| -- |
| 123 |

Explanation:
Candidate 123 is displayed because they have Python, Tableau, and PostgreSQL skills. 345 isn't included in the output because they're missing one of the required skills: PostgreSQL.

***

A possible solution is:

```
SELECT
    candidate_id
FROM 
    candidates
WHERE 
    skill IN ('Python', 'Tableau', 'PostgreSQL')
GROUP BY 
    candidate_id
HAVING 
    COUNT(skill)=3
ORDER BY 
    candidate_id ASC;
```


* **Selection of Candidate IDs**:
The SELECT statement retrieves the candidate_id column from the candidates table.
* **Filtering Candidates with Specific Skills**:
The WHERE clause filters candidates based on their skills. It looks for candidates who have at least one of the skills mentioned in the IN clause: Python, Tableau, and PostgreSQL.
* **Grouping Candidates by ID**:
The GROUP BY clause groups the candidates based on their candidate_id. This step ensures that each unique candidate ID is considered as a separate group.
* **Counting Skills per Candidate**:
The HAVING clause applies a condition after the grouping operation. It checks for groups (candidates) where the count of distinct skills (COUNT(skill)) is equal to 3. This ensures that only candidates possessing all three specified skills are included in the results.
* **Sorting Candidate IDs**:
The ORDER BY clause arranges the resulting candidate_ids in ascending order for clearer presentation.