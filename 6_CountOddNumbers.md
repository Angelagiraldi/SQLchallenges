Create a function that takes a positive integer *n* as input and computes the count of positive odd numbers that are less than *n*. 
The function should efficiently determine the quantity of odd numbers below 'n'.

For instance:

If 'n' is 7, the function should return 3 since there are 3 odd numbers ([1, 3, 5]) below 7.
If 'n' is 15, the function should return 7 as there are 7 odd numbers ([1, 3, 5, 7, 9, 11, 13]) below 15.
The function is expected to handle large input values effectively.

Note: The function should specifically count positive odd numbers less than 'n' and provide the count as the output.

***


A possible solution is:

```
SELECT n, n/2 as results
FROM number
```

**This is calculating an estimation of the count of odd numbers below 'n' based on the logic that half of the numbers below 'n' are odd.**
**The query is straightforward and easy to understand.**
**The calculation 'n/2' is fast and does not involve iterating through a sequence of numbers, making it efficient.**