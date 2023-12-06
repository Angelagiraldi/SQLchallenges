Write a select statement that takes *name* from *person* table and return "Hello, *name>* how are you doing today?" results in a column named *greeting*.

***

A possible solution is:

```
SELECT
    name,
    CONCAT('Hello,',name,'how are you doing today?') as greeting
FROM person
```

**Used CONCAT() to add together the strings and the column with variables.**