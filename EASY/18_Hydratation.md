Nathan drinks 0.5 litres of water per hour of cycling.
You get given the time in *hours* and you need to return the number of litres Nathan will drink, rounded to the smallest value.

For example:
* hours = 3 ----> liters = 1
* hours = 6.7---> liters = 3
* hours = 11.8--> liters = 5

You have to return 3 columns: id, hours and liters (not litres, it's a difference from the kata description)

***

A solution could be:

```
SELECT
    id,
    hours,
    FLOOR(hours*0.5) AS liters
```

**Select the two standard columns the problem require and then use the floor function to round down. Instead of hours*0.5 could also do hours/2.**
