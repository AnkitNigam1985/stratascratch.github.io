# Spotify Exercise Solutions

The table used on this exercise provides information about the ranking of popular songs worldwide. The following questions are provided with the correct SQL query and graphs using the graphics function in Strata Scratch.

Use the `datasets.spotify_worldwide_daily_song_ranking` table to answer the questions below.

### Question 1
Which songs have more than 3 million streams?

*Solution:*

Our query starts by selecting the columns `trackname, artist` and `Streams` since they are useful parameters to answer the question. We add a conditional statement where `streams > 3000000` and sort the data in ascending order to determine the top results.
```sql
   SELECT trackname,
      artist,
      Streams
   FROM datasets.spotify_worldwide_daily_song_ranking
   WHERE streams > 3000000
   ORDER BY streams ASC
   LIMIT 100
```

### Question 2
Which songs had been frequently placed in the top 1 position over the years?

*Solution:*

Here, we start to select the trackname and count the rows as `TIME` from the dataset. Since we are interested of the songs placed as top 1, we add a conditional statement `position = 1`. We can group the `trackname` to avoid duplicates and sort the results by time in descending order.
```sql
   SELECT trackname,
      count(*) AS TIME
   FROM datasets.spotify_worldwide_daily_song_ranking
   WHERE position = 1
   GROUP BY "trackname"
   ORDER BY TIME DESC
   LIMIT 100 
```

### Question 3
Which artists have been on Spotify the most?

*Solution:*

We want to know which artists have been on Spotify the most, so the best approach is to query the artist name and count the associated rows. We group the results and sort them in descending order to determine the top artists on Spotify.
```sql
   SELECT artist,
      count (*) as top_artists
   FROM datasets.spotify_worldwide_daily_song_ranking
   GROUP BY artist
   ORDER BY top_artists DESC
   LIMIT 100 
```

### Question 4
Which artists had the most top 10 songs over the years?

*Solution:*

We can determine which artists have their songs frequently included on the top 10 by querying the artist name and adding a conditional statement where position is less than or equal to 10. We group the data having the same artist name and sort the results in descending order to view the top artists.
```sql
   SELECT artist,
      count(*) AS TIME
   FROM datasets.spotify_worldwide_daily_song_ranking
   WHERE position < = 10 
   GROUP BY "artist"
   ORDER BY TIME DESC
   Limit 100.
```

### Question 5
Which songs have less than 2000 streams?

*Solution:*

Here, we first select the `trackname` and `Streams` from the table. We can display the results having less than 2000 streams by adding this requirement to the conditional statement `WHERE`. The top results in ascending order are then determined using the `ORDER BY` clause.
```sql
   SELECT trackname,
      Streams
   FROM datasets.spotify_worldwide_daily_song_ranking
   WHERE streams < 2000
   ORDER BY streams ASC
   LIMIT 100
```

### Question 6
What are the top 10 songs listened to?

*Solution:*

We can easily answer this question by querying the `trackname` from the table. We add a conditional statement where `position < 10` to get the top 10 results only. Grouping and sorting the data will help us determine the ranking of the songs mostly listened to.
```sql
   SELECT trackname
   FROM datasets.spotify_worldwide_daily_song_ranking
   WHERE position < 10
   GROUP BY trackname, position
   ORDER BY position ASC
   LIMIT 100
```

### Question 7
What is the average amount of streams of songs?

*Solution:*

Here, we compute the average amount of streams from the dataset using the `avg` function. We limit our results to the top 100 by adding the clause `LIMIT 100`.
```sql
   SELECT
      avg(streams)
   FROM datasets.spotify_worldwide_daily_song_ranking 
   LIMIT 100
```

*Output:* `48389`

### Question 8
How many streams are in the top 100?  

*Solution:*

In our solution, we count the number of streams from the table wherein the position is less than or equal to 100. 
```sql
   SELECT 
      count (*) as streams
   FROM datasets.spotify_worldwide_daily_song_ranking
   WHERE position <= 100
   LIMIT 100 
```

*Output:* `554906`

### Question 9
What is the highest stream of songs? 

*Solution:*

To determine the highest stream of songs, we simply get the maximum streams using the `max` function from the dataset.
```sql
   SELECT max(streams) as max_streams
   FROM datasets.spotify_worldwide_daily_song_ranking 
   LIMIT 100 
```

*Output:* `4068152`

### Question 10
Which songs are placed in the positions 8-10?

*Solution:*

Here, we simply select all the columns and filter the results using a conditional statement `WHERE  position in (8,9,10)`.  
```sql
   SELECT *
   FROM datasets.spotify_worldwide_daily_song_ranking 
   WHERE  position in (8,9,10) 
   LIMIT 100
```
