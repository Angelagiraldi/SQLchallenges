Assume there are three Spotify tables: artists, songs, and global_song_rank, which contain information about the artists, songs, and music charts, respectively.

>Write a query to find the top 5 artists whose songs appear most frequently in the Top 10 of the global_song_rank table. Display the top 5 artist names in ascending order, along with their song appearance ranking.

If two or more artists have the same number of song appearances, they should be assigned the same ranking, and the rank numbers should be continuous (i.e. 1, 2, 2, 3, 4, 5). 

artists Table:
artist_id	| artist_name|	label_owner
| -- | -- | --|
101	|Ed Sheeran|	Warner Music Group
120	|Drake	|Warner Music Group
125|	Bad Bunny|	Rimas Entertainment

songs Table:
song_id	|artist_id	|name
|--|--|--|
55511	|101|	Perfect
45202	|101	|Shape of You
22222	|120	|One Dance
19960	|120	|Hotline Bling

global_song_rank Table:
day|	song_id|	rank
|--|--|--|
1	|45202	|5
3	|45202	|2
1	|19960	|3
9	|19960	|15

Example Output:
artist_name|	artist_rank
|--|--|
Ed Sheeran	|1
Drake	|2

Explanation:
Ed Sheeran's song appeared twice in the Top 10 list of global song rank while Drake's song is only listed once. Therefore, Ed is ranked #1 and Drake is ranked #2.

***

A solution is:
```
WITH top_10_cte AS (
  SELECT 
    artists.artist_name,
    DENSE_RANK() OVER (
      ORDER BY COUNT(songs.song_id) DESC) AS artist_rank
  FROM artists
  INNER JOIN songs
    ON artists.artist_id = songs.artist_id
  INNER JOIN global_song_rank AS ranking
    ON songs.song_id = ranking.song_id
  WHERE ranking.rank <= 10
  GROUP BY artists.artist_name
)

SELECT
  artist_name,
  artist_rank
FROM top_10_cte
WHERE artist_rank<=5;
```


* CTE Definition - **WITH top_10_cte AS (...)**:
The query starts with defining a CTE named top_10_cte. CTEs are used to simplify complex queries by breaking them down into more manageable parts.
* Inside the CTE:
    * **SELECT artists.artist_name**  ,
    Selects the artist_name from the artists table.
    * **DENSE_RANK() OVER (ORDER BY COUNT(songs.song_id) DESC) AS artist_rank**
    A window function DENSE_RANK() is used to assign a rank to each artist based on the count of their songs that appear in the top 10 ranks. The DENSE_RANK() function ensures that artists with the same number of top 10 songs receive the same rank, and there are no gaps in ranking numbers.
    The ranking is ordered by the count of song_id in descending order, indicating that artists with more songs in the top 10 get a higher rank.
    * **FROM artists INNER JOIN songs ON artists.artist_id = songs.artist_id**
    Joins the artists and songs tables on the artist_id field.
    * **INNER JOIN global_song_rank AS ranking ON songs.song_id = ranking.song_id**
    Further joins the global_song_rank table (aliased as ranking) on song_id to include the ranking information.
    * **WHERE ranking.rank <= 10**
    Filters the data to include only those songs that have a rank of 10 or better (higher) in the global_song_rank table.
    * **GROUP BY artists.artist_name**
    Groups the data by artist_name for counting the number of top 10 songs.
* Main Query:
    * **SELECT artist_name, artist_rank**
    Selects artist_name and artist_rank from the CTE.
    * **FROM top_10_cte**
    Indicates that the data is being selected from the CTE top_10_cte.
    * **WHERE artist_rank <= 5;**
    Filters to include only those artists whose rank is 5 or better (higher). This effectively selects the top 5 artists based on their number of top 10 songs.