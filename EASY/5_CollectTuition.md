Retrieve a comprehensive list of students who haven't paid their tuition yet, including all available information about these students. This should be accomplished using a SQL select statement. The table schema consists of

| students | | | | |
| -- | -- | -- | -- | -- |
| name | age | semester | mentor | tuition_received |

---------------

A possible solution is:

```
SELECT name, age, semester, mentor
FROM students 
WHERE tuition_received = FALSE;
```

**The query filters the students based on the 'tuition_received' column, accurately retrieving those who haven't paid their tuition yet. 
Instead of using '*', I specify the exact columns needed in the output to enhance efficiency.**