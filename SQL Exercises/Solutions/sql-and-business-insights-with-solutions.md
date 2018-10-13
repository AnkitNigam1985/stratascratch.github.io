# SQL and Business Insights *(with Solutions)*

## Instructions 
- Log-in to your Strata Scratch account. 
- All tables used in this exercise are taken from the datasets schema. Make sure you select this schema on the SQL editor settings. 
- Try to answer the following questions by writing the appropriate SQL query on the editor. 
- For most of the questions, you will need to produce a chart that shows a list of dimensions (e.g., music artists) and counts 
(e.g., the number of grammies they’ve won).
- For each topic, there are also one to two open-ended questions (e.g., who is the best athlete to be drafted into the NFL?). 
You’ll need to analyze the dataset, create your own metrics, and provide a convincing argument.
- This is the teacher version of the SQL and business insights exercise. The questions below are followed with the correct solution. 
Each topic includes one to two open-ended questions which could have numerous possible solutions, and one 
of these solutions is shown here. 

## Questions

### 1. Billboard Top 100

Dataset: `billboard_top_100_year_end`

- What were the top songs for the past 30 years?

*Solution*

To find out if a song is a top song or not we check the year_rank for that song. If it is equal to 1 then it was a top song. We also need to check if a song belongs to our time period of interest. We do that with the second filter. `DATE_PART('year', CURRENT_DATE)` will give us the current year (2018 as of writing). 

```sql
SELECT song_name
FROM datasets.billboard_top_100_year_end
WHERE year_rank = 1 
AND DATE_PART('year', CURRENT_DATE) - year <= 30
```

- Which artists have had the most top 10 songs over the years? What were the songs?

*Solution*

The solution to the second question is in the inner query where we get all songs along with their artist which were among top
10 sometime. To answer the first question we need to count the number of songs which we do using `GROUPBY artist` and `count(*) AS top10_songs_count.` The results are also sorted in a descending order so we can quickly say that Elvis Presley, Mariah Carey and Elton John are the 3 most popular authors. 

```sql
SELECT artist, count(*) AS top10_songs_count
FROM
    (SELECT artist, song_name
    FROM datasets.billboard_top_100_year_end
    WHERE year_rank <= 10) temporary
GROUP BY artist
ORDER BY top10_songs_count DESC
```

- Which artists have been on the billboard top 100 the most in the past 10 years?

*solution*

We filter away all songs older than 10 years (` WHERE date_part('year', CURRENT_DATE) - year <= 10`) and then group by artist after which we count the number of occurences and sort descending so the highest counts are presented first.

```sql
SELECT artist, COUNT(*) as count_10yrs
FROM datasets.billboard_top_100_year_end
WHERE date_part('year', CURRENT_DATE) - year <= 10
GROUP BY artist
ORDER BY count_10yrs DESC
```

- Who is the best artist in the past 50 years? What metrics would you use to answer this question?

*solution*

To answer this question we first need to devise a metric. A suggestion is to calculate the score of each artist as (100 - average_rank) * years_present where average_rank is the average yearly ranking of all their songs while years present tells us how many years they are on the billboard. We use 100 - average_rank because we want a lower value of average rank to produce higher scores. We then order the artists on this metric after first filtering everything older than 50 years.

```sql
SELECT 
    artist, 
    AVG(year_rank) AS average_rank, 
    COUNT(DISTINCT year) AS years_present,
    (100 - AVG(year_rank)) * COUNT(DISTINCT year) AS score
FROM datasets.billboard_top_100_year_end
WHERE date_part('year', CURRENT_DATE) - year <= 50
GROUP BY artist
ORDER BY score DESC
```


### 2. SF Crime

Dataset: `sf_crime_incidents_2014_01`

- What were the top crime categories in 2014? How many incidences per category?

*Solution:*

In our solution, we want to determine first the number of categories which can be attained through the `SELECT` statement. To get the results within the year 2014, we added the conditional statement `WHERE date>='2014-01-01' and date<='2014-12-31'`. Knowing the top crime categories can be easily viewed by grouping and sorting the results in descending order.
```sql
SELECT
    category,
    count(category) as count
FROM datasets.sf_crime_incidents_2014_01
WHERE date>='2014-01-01' and date<='2014-12-31'
GROUP BY category
ORDER BY count DESC
```

- What day of the week was there the most crime? Does crime incidence vary depending on the month? Depending on the time?

*Solution:*
```sql
SELECT
    day_of_week,
    count(category) as count
FROM datasets.sf_crime_incidents_2014_01
GROUP BY day_of_week
ORDER BY count DESC
```

Based on the result, the day of the week with the highest crime is Friday.

- What districts had the most crime incidences?

*Solution:*
```sql
SELECT
    pd_district,
    count(category) as count
FROM datasets.sf_crime_incidents_2014_01
GROUP BY pd_district
ORDER BY count DESC
```

Based on the result, the Southern District has the most crime incidences.

- Where is the most dangerous place in SF? What metrics would you use to convince me?

*Solution:*
```sql
SELECT
    address,
    pd_district,
    count(category) as count
FROM datasets.sf_crime_incidents_2014_01
GROUP BY address, pd_district
ORDER BY count DESC
```

Based on the result, the most dangerous place in SF is at 800 Block of BRYANT ST.

### 3. Oscar Nominees

Dataset: `oscar_nominees`

- Who has won the most oscars?

*Solution:*
```sql
SELECT
    nominee,
    count(winner) as count
FROM datasets.oscar_nominees
WHERE winner = true
GROUP BY nominee
ORDER BY count DESC
```

Based on the result, the answers are Meryl Streep, Katharine Hepburn, Walter Brennan, Jack Nicholson and Ingrid Bergman.

- Count the number of nominations which didn't end with a award. Who was the least lucky?

*Solution:*
```sql
SELECT
    nominee,
    count(winner) as count
FROM datasets.oscar_nominees
WHERE winner = false
GROUP BY nominee
ORDER BY count DESC
```

The answer is Meryl Streep.

- Who has the highest win to nomination ratio?

*Solution:*
```sql
SELECT
    nominee,
    
    SUM (CASE 
        WHEN winner = true 
        THEN 1.0 
        ELSE 0.0
        END) / COUNT(*) :: NUMERIC AS ratio
FROM 
    datasets.oscar_nominees
GROUP BY nominee
ORDER BY ratio DESC
```

The answers are the following: Judy Holliday, Burl Ives, Daniel Day Lewis, Brenda Fricker, Donald Crisp, Ginger Rogers, Marion Cotillard, Mo'Nique, Louise Fletcher

- Which movies had the most nominated actors/actresses?

*Solution:*
```sql
SELECT
    movie,
    count(nominee)
FROM datasets.oscar_nominees
GROUP BY movie
ORDER BY count DESC
```

The answers are the following: The Godfather Part II, Network, On the Waterfront, Mrs. Miniver, All about Eve, Peyton Place, From Here to Eternity, Bonnie and Clyde

- Who is the best actor/actress of all time? What metrics would you use to convince me?

*Solution:*
```sql
SELECT
    nominee,
    count(winner) as count
FROM datasets.oscar_nominees
WHERE winner = true
GROUP BY nominee
ORDER BY count DESC
```

The answers are the following: Meryl Streep, Katharine Hepburn, Walter Brennan, Jack Nicholson, Ingrid Bergman

### 4. Video Game Charts

Dataset: `global_weekly_charts_2013_2014`

- Which games have been in the top 100 for the longest? How many weeks were they in the top 100?

*solution*

The answers are FIFA Soccer 13 with 432 weeks, LEGO The Lord of the Rings with 378 weeks, LEGO Batman 2: DC Super Heroes with
339 weeks.

```sql
SELECT 
    game, 
    COUNT(*) AS week_count
FROM datasets.global_weekly_charts_2013_2014
WHERE week <= 100
GROUP BY game
ORDER BY week_count DESC
```

- Which games, by platform, has been in the top 10 for the longest?

*solution*

The answers are:
* Deus Ex: Human Revolution - Director's Cut on WiiU
* Painkiller: Hell & Damnation on PS3
* Painkiller: Hell & Damnation on X360

```sql
SELECT game, platform, COUNT(*) as count_top10
FROM datasets.global_weekly_charts_2013_2014
WHERE week <= 10
GROUP BY game, platform
ORDER BY count_top10 DESC
```

- What genres yielded highest sales?

*solution*

The answers are: Action, Sports, Shooter

```sql
    SELECT genre, SUM(total) AS total_sales
    FROM datasets.global_weekly_charts_2013_2014
    GROUP BY genre
    ORDER BY total_sales DESC
```

- Which are the best publishers? What metrics would you use to analyze?

*solution* 

Total sales were used as the chosen metric.

```sql
SELECT publisher, SUM(total) AS total_sales
FROM datasets.global_weekly_charts_2013_2014
GROUP BY publisher
ORDER BY total_sales DESC
```

### 5. NFL Combine

Datasets: `nfl_combine`

- Which colleges produce the most NFL players?

*solution*

The answers are:
- Georgia with 81 players
- Flordia with 80 players
- USC with 77 players

Do note that this query will also return `null 1469` which means there are 1469 players which do not belong to any college.

```sql
SELECT
    college,
    COUNT(*) AS player_count
FROM datasets.nfl_combine
GROUP BY college
ORDER BY player_count DESC
```

- How do the 40-yard dash, vertical, broad jump, and bench differ between athletes drafted into the NFL vs undrafted? How do the metrics vary by draft round?

- Which colleges produce the best quarterbacks? How about other positions? How did you define the best?

- Who is the best athlete? What metrics would you analyze to prove your case?

### 6. Airbnb

Datasets: `datasets.airbnb_searches`, `datasets.airbnb_contacts`

- How many people search for hosts on Airbnb?

*solution*

For this question we will use only the dataset `datasets.airbnb_searches`. Unlike the majority of queries which return rows this one gives us back only a single number. This number 18605 is the number of unique `id_user` values. The heart of the query is `COUNT (DISTINCT id_user)` which counts rows after discarding repeat entries according to id_user criteria. 

```sql
SELECT COUNT (DISTINCT id_user) AS total_people_searching
FROM datasets.airbnb_searches
```

- How many nights are most people searching for when trying to book a host?

*solution*

We will only need `datasets.airbnb_searches`. We perform groupby over number of nights and sum up the searches for every night to obtain our statistics. The results are:

|number of nights|number of searches|
|----------------|------------------|
|null|60926|
|2|58201|
|3|57177|
|4|39392|
|1|33786|
|5|17621|

Notice that the majority of searches do not specify the number of nights.

```sql
SELECT n_nights, SUM(n_searches) AS total_searches
FROM datasets.airbnb_searches
GROUP BY n_nights
ORDER BY total_searches DESC
```

- What day of the week are most people checking in? What day of the week are most people checking out?

*solution*

We use `datasets.airbnb_contacts` for this task.

**For check-ins**

`DATE_PART('dow', ds_checkin)` tells us the day of the week from 0 to 6. We group on that day and count the number of rows per groups. We sort from maximal checkin_count downwards and take the first value which is 5 (Friday)

```sql 
SELECT 
  DATE_PART('dow', ds_checkin) AS day_of_week,
  COUNT(*) AS checkin_count
FROM datasets.airbnb_contacts
GROUP BY day_of_week
ORDER BY checkin_count DESC
LIMIT 1
```

**For check-outs**

The procedure is very similiar we just need to use the `ds_checkout` column.
The resulting day is 0 (Sunday)

```sql
SELECT 
  DATE_PART('dow', ds_checkout) AS day_of_week,
  COUNT(*) AS checkout_count
FROM datasets.airbnb_contacts
GROUP BY day_of_week
ORDER BY checkout_count DESC
LIMIT 1
```

- What type of rooms are most people searching for?

The majority is searching for: No filters, ',Entire home/apt', 'Entire home/apt', ',Private room', 'Private room'.
We see that there is error in the data with addition of a coma before some entries so we should clean our data to get more reliable results. 

```sql
SELECT
    filter_room_types,
    COUNT(*) as count_searches
FROM datasets.airbnb_searches
GROUP BY filter_room_types
ORDER BY count_searches DESC
```

To clean our data we can use `LTRIM(filter_room_types, ',')` which removes commas from start and now our query becomes:

```sql
SELECT
    LTRIM(filter_room_types, ',') AS cleaned_filter,
    COUNT(*) as count_searches
FROM datasets.airbnb_searches
GROUP BY cleaned_filter
ORDER BY count_searches DESC
```

- What’s the acceptance rate of requests?

*solution*
The result is 0.463632877412757

The final query is

```sql
SELECT
    (SELECT COUNT(*) AS count_accepted
    FROM datasets.airbnb_contacts
    WHERE ts_accepted_at IS NOT NULL) /
    (SELECT COUNT(*) as count_rows
    FROM datasets.airbnb_contacts) :: FLOAT 
AS acceptance_rate
```

There is quite a lot going on in the query so let's break it into pieces.

```sql
(SELECT COUNT(*) AS count_accepted
    FROM datasets.airbnb_contacts
    WHERE ts_accepted_at IS NOT NULL)
```

That will give us a single number which tells us how many of the bookings were accepted based on the idea that having ts_accepted_at being null is a sign of non-acceptance.

```sql
(SELECT COUNT(*) as count_rows
    FROM datasets.airbnb_contacts) :: FLOAT
```

The second piece is just counting how many rows are there in the table.
We convert the integer to FLOAT using :: FLOAT notation so we can divide in the final query

```sql
SELECT piece1 / piece2
```

This is what the final query essentaly ends up to be.


- What can Airbnb do to increase the number of bookings?

Based on our data and the following query and it's results.

```sql
SELECT n_messages, COUNT(*) 
FROM datasets.airbnb_contacts
WHERE ts_accepted_at IS NULL
GROUP BY n_messages
ORDER BY COUNT(*) DESC
```

|n_messages | count|
|-----------|------|
|2 | 1419|
|3 | 1033|
|4 | 677|
|5 | 333|
|1 | 332|
|6 | 157|
|7 | 77|
|8 | 48|
|9 | 31|

We see that majority of non acceptances (around 98%) are linked to a small number of messages, which is less than 10.
This is likely the result of people asking some important questions via the first message, getting an unsatisfactory answer in the second message and then not accepting. So the solution would be to encourage hosts to write that crucial information in the description so visitors don't bother writing to a host they do not plan to visit. 

```sql
SELECT
    (SELECT 
        SUM(cnt) 
     FROM 
        (SELECT COUNT(*) AS cnt
            FROM datasets.airbnb_contacts
            WHERE ts_accepted_at IS NULL AND n_messages <= 10
            GROUP BY n_messages
            ORDER BY count(*) DESC) temp
    ) :: FLOAT
    /
    (SELECT COUNT(*) FROM datasets.airbnb_contacts WHERE ts_accepted_at IS NULL) 
AS acceptance_rate
```
is the query that gave us that number 98%
