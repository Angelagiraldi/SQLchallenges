In the historical timeline, centuries are defined by specific year ranges. The first century spans from the year 1 up to and including the year 100, the second century from the year 101 up to and including the year 200, and so forth.

Task:
Given a specific year, the objective is to determine and return the century in which the year falls.

Examples:
* For the year 1705, the function should return 18, indicating it falls within the 18th century.
* If the input year is 1900, the result should be 19, signifying that it corresponds to the 19th century.
* Similarly, for the year 1601, the function should return 17, placing it within the 17th century.
* For the year 2000, the function should output 20, representing the 20th century.

You will be provided a table named `years` with a column `yr` containing the year values. The task in SQL involves returning a table with an additional column named `century`. This new column `century` should denote the century to which each year in the years table belongs.

-----------------------------------------

A possible solution is:

```
SELECT yr, (yr + 99) / 100 AS century
FROM years
```

**(yr + 99) / 100: This formula calculates the century by adding 99 to the year (yr) and then dividing the result by 100. This method determines the century by considering the year's proximity to the start or end of a century range without rounding down.**