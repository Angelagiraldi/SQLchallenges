In this game, the hero moves from left to right. The player rolls the dice and moves the number of spaces indicated by the dice two times.

In SQL, you will be given a table *moves* with columns *position* and *roll*. 
Return a table which uses the current position of the hero and the roll (1-6) and returns the new position in a column *res*.

***

A possible solution is:

```
SELECT
    position, roll, position + (roll*2) as res
FROM
    moves
```

***Take the original position, and add onto it 2 times whatever roll you make.***
