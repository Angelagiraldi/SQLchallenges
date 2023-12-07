Create a function that calculates the time elapsed in milliseconds since midnight based on the input values representing hours (h), minutes (m), and seconds (s) displayed on a clock.

Given `clock` table with these columns`:
* *h*: Hours displayed on the clock (0 to 23)
* *m*: Minutes displayed on the clock (0 to 59)
* *s*: Seconds displayed on the clock (0 to 59)

The task is to create a function that accurately converts the input time (hours, minutes, and seconds) into milliseconds and returns the total time elapsed since midnight as a result.

Example:
If h = 0, m = 1, and s = 1, the function should return the total time since midnight in milliseconds considering 1 minute and 1 second has passed after midnight.  The result corresponds to 61000.

-------------------------------------------
A possible solution is:

```
SELECT 
  (h * 3600000) + (m * 60000) + (s * 1000) AS milliseconds_since_midnight
FROM 
  clock;
```
 

Explanation:

1) Conversion to Milliseconds:
    * Each hour consists of 3600 seconds (60 seconds/minute * 60 minutes/hour).
    * Each minute consists of 60 seconds.
    * To calculate the total milliseconds:
        * Convert hours to milliseconds: h * 3600 * 1000 (3600 seconds * 1000 milliseconds)
        * Convert minutes to milliseconds: m * 60 * 1000 (60 seconds * 1000 milliseconds)
        * Convert seconds to milliseconds: s * 1000 (1 second * 1000 milliseconds)
2) Total Milliseconds Calculation:
Add the converted milliseconds from hours, minutes, and seconds together to obtain the total time since midnight.