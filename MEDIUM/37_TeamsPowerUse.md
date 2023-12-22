> Write a query to identify the top 2 Power Users who sent the highest number of messages on Microsoft Teams in August 2022. Display the IDs of these 2 users along with the total number of messages they sent. Output the results in descending order based on the count of the messages.

Assumption:
* No two users have sent the same number of messages in August 2022.

*messages* Table:

Column Name	| Type
| -- | -- |
message_id	| integer
sender_id	| integer
receiver_id	| integer
content	| varchar
sent_date	| datetime


*messages* Example Input:

message_id	| sender_id	| receiver_id	| content	| sent_date
| -- | -- | -- | -- | --|
901 | 	3601	| 4500	| You up?	| 08/03/2022 | 00:00:00
902	| 4500	| 3601	| Only if you're buying	| 08/03/2022 | 00:00:00
743	| 3601	| 8752	| Let's take this offline	| 06/14/2022 | 00:00:00
922	| 3601	| 4500	|  Get on the call	| 08/10/2022 | 00:00:00

Example Output:

sender_id	| message_count
| -- | -- | 
3601| 2
4500	| 1


***

A possible solution is:
```
SELECT sender_id,
      COUNT(message_id) AS count_messages
FROM messages
WHERE EXTRACT(MONTH FROM sent_date) = '8'
  AND EXTRACT(YEAR FROM sent_date) = '2022'
GROUP BY sender_id
ORDER BY count_messages DESC 
LIMIT 2
```

* **SELECT sender_id**
This line selects the sender_id column from the messages table. The sender_id presumably represents the identifier of the sender of a message.
* **COUNT(message_id)** AS count_messages
This line counts the number of messages sent by each sender_id. The COUNT(message_id) function counts the number of message_ids (which likely represents the unique identifier for each message) for each sender. The result of this count is given an alias count_messages for easy reference.
* **FROM messages**
Specifies that the data is being selected from the messages table.
* **WHERE EXTRACT(MONTH FROM sent_date) = '8'**
This line filters the data to include only those records where the month part of the sent_date (the date when the message was sent) is August (month 8). The EXTRACT function is used to get the month part from the sent_date column.
* **AND EXTRACT(YEAR FROM sent_date) = '2022'**
Further filters the records to include only those where the year part of the sent_date is 2022. Again, the EXTRACT function is used, this time to get the year part.
* **GROUP BY sender_id**
This line groups the records by sender_id. This is necessary for the COUNT(message_id) function to calculate the number of messages per sender.
* **ORDER BY count_messages DESC**
Orders the results by the count of messages (count_messages) in descending order (DESC). This means the sender with the most messages will be at the top.
* **LIMIT 2**
Limits the result to the top two records. Since the results are ordered by the count of messages in descending order, these two records will be the senders with the highest number of messages in August 2022.