Assume you're given tables with information about TikTok user sign-ups and confirmations through email and text. New users on TikTok sign up using their email addresses, and upon sign-up, each user receives a text message confirmation to activate their account.

> Write a query to display the user IDs of those who did not confirm their sign-up on the first day, but confirmed on the second day.

Definition:
action_date refers to the date when users activated their accounts and confirmed their sign-up through text messages.

emails Table:
Column Name	|Type
| --|--|
email_id	|integer
user_id	|integer
signup_date	|datetime


emails Example Input:
email_id	|user_id|	signup_date
| --|--|--|
125	|7771	|06/14/2022 00:00:00
433	|1052	|07/09/2022 00:00:00


texts Table:

Column Name	|Type
| --|--|
text_id	|integer
email_id	|integer
signup_action	|string ('Confirmed', 'Not confirmed')
action_date	|datetime

texts Example Input:
text_id	|email_id	|signup_action	|action_date
|--|--|--|--|
6878	|125	|Confirmed	|06/14/2022 00:00:00
6997	|433	|Not Confirmed	|07/09/2022 00:00:00
7000	|433	|Confirmed	|07/10/2022 00:00:00

Example Output:
user_id
1052
Explanation:

Only User 1052 confirmed their sign-up on the second day.

***

A possible solution:
```
SELECT user_id 
FROM emails
INNER JOIN texts
ON emails.email_id = texts.email_id
WHERE texts.action_date = emails.signup_date + INTERVAL '1 day'
  AND texts.signup_action = 'Confirmed'
```

* **SELECT user_id**
This statement indicates that the query will return the user_id. However, it's not specified which table the user_id is coming from. If both tables contain a user_id column, you would need to specify which one you want (e.g., emails.user_id or texts.user_id).
* **FROM emails**
The data is being selected from the emails table.
* **INNER JOIN texts ON emails.email_id = texts.email_id**
This line performs an INNER JOIN between the emails and texts tables. The join is based on the condition that the email_id fields in both tables must match. An INNER JOIN will only return rows where there is a match in both tables.
* **WHERE texts.action_date = emails.signup_date + INTERVAL '1 day'**
This WHERE clause filters the records to include only those where the action_date in the texts table is exactly one day after the signup_date in the emails table. The + INTERVAL '1 day' part adds one day to the signup_date.
* **AND texts.signup_action = 'Confirmed'**
Further filters the results to include only rows where the signup_action in the texts table is equal to 'Confirmed'.