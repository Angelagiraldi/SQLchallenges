The goal is to determine the largest possible value obtained by inserting mathematical operators (+, *) and brackets into a sequence of three positive integers, denoted as a, b, and c. This task involves exploring various combinations of these numbers and operators to achieve the maximum result.

> Example
With the numbers are 1, 2 and 3 , here are some ways of placing signs and brackets:
> - 1 * (2 + 3) = 5
> - 1 * 2 * 3 = 6
> - 1 + 2 * 3 = 7
> - (1 + 2) * 3 = 9
>
> So the maximum value that you can obtain is 9.

Notes:
* The numbers are always positive.
* The numbers are in the range (1  ≤  a, b, c  ≤  10).
* You can use the same operation more than once.
* It's not necessary to place all the signs and brackets.
* Repetition in numbers may occur .
* You cannot swap the operands. For instance, in the given example you cannot get expression (1 + 3) * 2 = 8.

*** 
A possible solution:

```
SELECT
    GREATEST(
        a + b + c,             -- Expression 1
        a * b * c,             -- Expression 2
        (a + b) * c,           -- Expression 3
        a * (b + c),           -- Expression 4
        (a * b) + c,           -- Expression 5
        a + (b * c)            -- Expression 6
    ) AS max_result
FROM numbers;
```

***This SQL query calculates several predefined expressions (as examples) involving the columns 'a', 'b', and 'c' and selects the greatest result among them for each row in the 'numbers' table. However, this query only evaluates specific expressions and doesn't cover all possible combinations and permutations of operators and brackets.***