You received an invitation to an amazing party. Now you need to write an insert statement to 
add yourself to the table of participants.

participants table schema
|participants| | |
| -- | -- | -- |
| name | age | attending |


NOTES:
Since alcohol will be served, you can only attend if you are 21 or older.
You can't attend if the attending column returns anything but true

***

A possible solution is:

```
INSERT INTO participants (name, age, attending)
VALUES ('Angela',30, True)

SELECT *
FROM participants
```

**Add myself into the table and then select all to check if I was properly added.**