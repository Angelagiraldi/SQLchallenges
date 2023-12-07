Given soccer players with goals in three leagues:

| leagues | | |
| -- | -- | -- |
| la_liga_goals | copa_del_rey_goals | champions_league_goals |

Complete the function to return the total number of goals in all three leagues for each player.

***

A possible solution is:

```
SELECT
    la_liga_goals + copa_del_rey_goals + champions_league_goals as res
FROM leagues
```

***Simply add together the values in each column mentioned to find total goals scored.***