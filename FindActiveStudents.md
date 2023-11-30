
# EASY SQL Query to Display Active Student Information

The goal is simply to create a SELECT query to display student information only of all ACTIVE students.
The table structure of *students* is the following:


|students| | | |
| ----------- | ----------- | ----------- | ----------- |
| Id	| FirstName	| LastName	| IsActive | 

Where IsActive can be True or False.


----------------------------------

A possible solution is:

```
SELECT Id, FirstName, LastName,
FROM students
WHERE IsActive; 
```


There is no need to set *IsActive* to True because by saying `Where IsActive`is already only validating and showing only True values.