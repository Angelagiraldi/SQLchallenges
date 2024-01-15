New TikTok users sign up with their emails. They confirmed their signup by replying to the text confirmation to activate their accounts. Users may receive multiple text messages for account confirmation until they have confirmed their new account.

>A senior analyst is interested to know the activation rate of specified users in the emails table. Write a query to find the activation rate. Round the percentage to 2 decimal places.

Definitions:
* *emails* table contain the information of user signup details.
* *texts* table contains the users' activation information.

Assumptions:
The analyst is interested in the activation rate of specific users in the emails table, which may not include all users that could potentially be found in the texts table.
For example, user 123 in the emails table may not be in the texts table and vice versa.


emails Table:

email_id	| user_id	|signup_date
| --| --|--|
125	|7771	|06/14/2022 00:00:00
236	|6950	|07/01/2022 00:00:00
433	|1052	|07/09/2022 00:00:00

texts Table:

text_id|	email_id|	signup_action
|--|--|--|
6878	|125|	Confirmed
6920	|236|	Not Confirmed
6994	|236|	Confirmed

'Confirmed' in signup_action means the user has activated their account and successfully completed the signup process.

Example Output:
confirm_rate|
|--|
0.67

Explanation:
67% of users have successfully completed their signup and activated their accounts. The remaining 33% have not yet replied to the text to confirm their signup.


***

A possible solution is:

```
SELECT 
  ROUND(COUNT(texts.email_id)::DECIMAL
    /COUNT(DISTINCT emails.email_id),2) AS activation_rate
FROM emails
LEFT JOIN texts
  ON emails.email_id = texts.email_id
  AND texts.signup_action = 'Confirmed';
```

* **SELECT ROUND(COUNT(texts.email_id)::DECIMAL / COUNT(DISTINCT emails.email_id), 2) AS activation_rate**
    * Counts the number of distinct email IDs in the emails table and counts the number of email IDs in the texts table where the signup_action is 'Confirmed'.
    * Calculates the activation rate by dividing the count of confirmed signups by the count of distinct emails.
    * ROUND(..., 2): Rounds the result to two decimal places.
    * The result is aliased as activation_rate.
* **FROM emails LEFT JOIN texts ON emails.email_id = texts.email_id AND texts.signup_action = 'Confirmed';**
    * Uses a LEFT JOIN to combine rows from the emails table with matching rows in the texts table based on the email_id.
    * The condition texts.signup_action = 'Confirmed' ensures that only confirmed signups are considered in the count.
    * emails.email_id is used as the key for the join.


The query calculates the activation rate by determining the proportion of confirmed signups (from texts) relative to the total number of distinct emails (from emails). The result is a percentage rounded to two decimal places, representing the activation rate.