Convert the numeric columns *arms* and *legs* from the *monsters* table into their corresponding hexadecimal (hex) values for the output table.

| monster | | | | |
| -- | -- | -- | -- | -- | 
| id | name | legs | arms | characteristics |


The goal is to generate an output table with the columns *legs* and *arms*, containing the hexadecimal representations of the *legs* and *arms* columns from the *monsters* table, respectively.


***

A possible solution is:

```
SELECT 
    HEX(legs) AS legs,
    HEX(arms) AS arms
FROM monsters;
```

**The SQL query uses the SELECT statement to retrieve the hexadecimal representations of the 'legs' and 'arms' columns from the 'monsters' table.
The HEX() built-in function is applied to each numeric column ('legs' and 'arms'), which converts the numeric values into their corresponding hexadecimal representations.**