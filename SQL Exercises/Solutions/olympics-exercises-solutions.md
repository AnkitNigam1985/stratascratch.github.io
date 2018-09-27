# SQL Exercise 2

The dataset for this exercise is `datasets.olympics_athletes_events`.

The data is about the athletes for all Olympic games, starting from the first Athens, 1896 and ending with the games in Rio De Janeiro, 2016. We have their physical characteristics like their sex, age, weight and height. Additionally, we know their team, their national Olympic committee and their name. The dataset is completed with information about the sport, the event and the possible medal.

The technical description of the dataset is in the following table.

| Column Name | Column Type | Has NULL or missing values | Short description |
|---|---|---|---|
|*id*|INTEGER|NO|The primary key for this table.|
|*name*|VARCHAR|NO|The full name of the Athlete. VARCHAR is the same type as TEXT.
|*sex*|VARCHAR|NO|The sex of the athlete, one of M or F.
|*age*|DOUBLE PRECISION|**YES**|The age of the athlete during the games. The double precision type is same as NUMERIC. Can be NULL for some early games.
|*height*|DOUBLE PRECISION|**YES**|The height of the athlete in centimeters during the games. Can be NULL. 
|*weight*|DOUBLE PRECISION|**YES**|The weight of the athlete in kilograms during the games. Can be NULL.
|*team*|VARCHAR|NO|The national team of the athlete.
|*noc*|VARCHAR|NO|National Olympic Comittee.
|*games*|VARCHAR|NO|The year of the games and the season in the same column.
|*year*|INTEGER|NO|The year when the games happened.
|*season*|VARCHAR|NO|The season when the games happened. Can be Summer or Winter.
|*city*|VARCHAR|NO|The city in which the games took place.
|*sport*|VARCHAR|NO|The sport in which the athlete competed. Example: Speed Skating
|*event*|VARCHAR|NO|The event in the sport in which athlete competed. Example: Speed Skating 1,000 metres.
|*medal*|VARCHAR|**YES**|The medal won by the athlete can be one of: NULL, Bronze, Silver, Gold.

To make life easier, always do a select for all the columns needed in the question, look at the returned values, ponder a bit and then try to write the solution.

## Beginner exercise

1. Find all women who participated in the olympics before World War 2.

Columns: `sex` and `year`

```sql
SELECT
    DISTINCT name
FROM datasets.olympics_athletes_events
WHERE sex = 'F' AND year <= 1936
```

2. Find all Danish athletes who won a medal.

Columns: `team` and `medal`

```sql
SELECT
    DISTINCT name
FROM datasets.olympics_athletes_events
WHERE 
    team = 'Denmark' AND
    medal IS NOT NULL
```

3. Find the first 15 athlethes which competed in swimming in any game which happened in London.

Columns: `sport` and `city`

```sql
SELECT
    name
FROM datasets.olympics_athletes_events
WHERE 
    sport = 'Swimming' AND city = 'London'
LIMIT 15
```

4. Find all events in which Christine Jacoba Aaftink participated.

Columns: `name` and `event`

```sql
SELECT
    name, event
FROM datasets.olympics_athletes_events
WHERE 
    name = 'Christine Jacoba Aaftink'
```

5. Find all non adult players and the games they participated in when they were still not adults. Assume the age of becoming and adult is 18 years

Columns: `name`, `age` and `games`.

```sql
SELECT
    name, age, games
FROM datasets.olympics_athletes_events
WHERE 
    age <= 18
```

## Intermediate exercises

1. Find all athletes who were older than 40 years at the time of the games who won either bronze or silver medals. Use the IN clause in your solution.

Columns: `name`, `age` and `medal`

```sql
SELECT
    name
FROM datasets.olympics_athletes_events
WHERE 
    age >= 40 AND medal IN ('Bronze', 'Silver')
```

2. Find all events during any winter olympics in which all athletes were of height between 180 and 210 centimeters. 

Columns: `event`, `season`, `height`

```sql
SELECT
    event
FROM datasets.olympics_athletes_events
WHERE 
    season = 'Winter' AND 
    height BETWEEN 180 AND 210
```

3. There were athletes which participated in multiple teams. For example John Mary Pius Boland who played for both Germany and Great Britain. Find all these along with the games they participated in, their sport and their possible medal.

Columns: `name`, `team`, `games`, `sport`, `medal`

```sql
SELECT
    name, team, games, sport, medal
FROM datasets.olympics_athletes_events
WHERE 
    team ILIKE '%/%'
```

4. Order all countries by the year they first participated in the olympics.

Hint: Use the `noc` column in the group by clause.

Columns: `noc` and `year`

```sql
SELECT
    noc,
    MIN(year) AS first_time_year
FROM datasets.olympics_athletes_events
GROUP BY noc
ORDER BY first_time_year
```

5. Which games attract more athletes? The winter or summer ones?

Hint: To find the number of athletes count only the unique names.

Columns: `season`, `name`

```sql
SELECT
    season,
    COUNT(DISTINCT name)
FROM datasets.olympics_athletes_events
GROUP BY season
```

6. Which games in the whole history of the olympics had the highest number of athletes. Your solution must return only a single row.

Columns: `games`, `name`

```sql
SELECT
    games,
    COUNT(DISTINCT name) AS athlethes_count
FROM datasets.olympics_athletes_events
GROUP BY games
ORDER BY athlethes_count DESC
LIMIT 1
```

7. What are the min, mean and max ages over all olympics?

Columns: `age`

```sql
SELECT
    MIN(age),
    MAX(age),
    AVG(age)
FROM 
    datasets.olympics_athletes_events
```

8. Find the average weight of medal winning Judo players for each team with the condition that the minimum age of team is 20 years and the maximum age of the team is 30 years. Minimum age of team is the minimum of all ages of all athletes which are part of that team.

Hint: Use a HAVING clause.

Columns: `team`, `weight`, `sport`, `medal`, `age`

```sql
SELECT
   team,
   AVG(weight) AS average_player_weight
FROM 
    datasets.olympics_athletes_events
WHERE 
    sport = 'Judo' AND medal IS NOT NULL
GROUP BY team
HAVING MIN(age) >= 20 AND MAX(age) <= 30
ORDER BY average_player_weight
```

9. Find all distinct sports in which obese people participated. Obese people are those whose Body mass index exceeds 30. The body mass index is calculated as weight / (height * height). The height in the formula is in meters while height in our dataset is in centimeters. You can ignore athletes for whom no weight or height is measured.

Columns: `weight`, `height`, `sport`

```sql
SELECT
   DISTINCT sport
FROM 
    datasets.olympics_athletes_events
WHERE 
    weight IS NOT NULL AND 
    height IS NOT NULL AND 
    weight / ((height / 100) * (height / 100)) > 30
```

10. Find the year in which the shortest athlete participated.

Columns: `year` and `height`

```sql
SELECT
   year,
   MIN(height) AS smallest_height
FROM 
    datasets.olympics_athletes_events
GROUP BY year
ORDER BY smallest_height ASC
LIMIT 1
```

11. How many athletes competing in Football won Gold medals aggregated by their national olympic committees and their sex. 

Columns: `noc`, `sex`, `sport`, `medal`

```sql
SELECT
    noc, 
    sex,
    COUNT(*) AS cnt
FROM datasets.olympics_athletes_events
WHERE
    sport = 'Football' AND
    medal = 'Gold'
GROUP BY noc, sex
ORDER BY noc, sex, cnt
```

12. Which games had the highest number of people who went home without a medal.

Columns: `games` and `medal`

```sql
SELECT
    games,
    COUNT(*) AS cnt
FROM datasets.olympics_athletes_events
WHERE
    medal IS NULL
GROUP BY games
ORDER BY cnt DESC
LIMIT 1
```

13. Classify each athlete as either a Patriot for playing for only a single team or as Globalist for playing for multiple teams.

Hint: Use a CASE statement.

Columns: `name` and `team`

```sql
SELECT
    name, 
    (CASE 
        WHEN team ILIKE '%/%' THEN 'Globalist'
        ELSE 'Patriot'
        END) AS player_belief
FROM datasets.olympics_athletes_events
```

14. The following European cities hosted the olympics. Use a CASE statement to classify each olympics as being either European or not. Cities: London, Roma, Antwerpen, Amsterdam, Stockholm, Sarajevo, Berlin, Grenoble, Moskva, Oslo, Athina, Paris, Munich, Garmisch-Partenkirchen, Sochi, Torino, Innsbruck, Helsinki, Barcelona. 

Columns: `city`

```sql
SELECT
   *,
   (CASE
        WHEN city IN ( 'London', 'Roma', 'Antwerpen', 'Amsterdam', 'Stockholm', 'Sarajevo',
        'Berlin', 'Grenoble', 'Moskva', 'Oslo', 'Athina', 'Paris', 'Munich','Garmisch-Partenkirchen', 
        'Sochi', 'Torino', 'Innsbruck', 'Helsinki', 'Barcelona' ) 
        THEN 'European'
        ELSE 'NonEuropean' END) AS city_classification
FROM datasets.olympics_athletes_events
```

## Advanced exercises

1. Use the classification from previous question and count the number of athletes which participated in European cities using a subquery.

Columns: `city`

```sql
SELECT
    COUNT(*)
FROM
(SELECT
   *,
   (CASE
        WHEN city IN ( 'London', 'Roma', 'Antwerpen', 'Amsterdam', 'Stockholm', 'Sarajevo',
                       'Berlin', 'Grenoble', 'Moskva', 'Oslo', 'Athina', 'Paris', 'Munich', 'Garmisch-Partenkirchen', 
                       'Sochi', 'Torino', 'Innsbruck', 'Helsinki', 'Barcelona' ) 
        THEN 'European'
        ELSE 'NonEuropean' END) AS city_classification
FROM datasets.olympics_athletes_events) tmp
WHERE tmp.city_classification = 'European'
```

2. Find the length of the first name of each player and use it to find the total number of medals of each type, including null, each length of first name won.

The output should look like:

|fixed_length_of_name | no_medals |bronze_medals | silver_medals | gold_medals 
|---|---|---|---|---|
1 | 447 | 17 | 16 | 5
2 | 3419 | 194 | 230 | 164


This means people whose name is of length 1 won 5 gold medals, 16 silver medals and 17 bronze medals. 447 of them won no medals.

It is ok if the numbers are different, it is the structure that counts.

Hint 1: To find the first name find the position of the space character (' ') in the `name` column. 

Hint 2: Use techniques for making pivot tables to reach the solution.

Hint 3: SUM(1, 1, 0, 0, 1) = 3 

Columns: `name`, `medal`

```sql
SELECT
    tmp.fixed_length_of_name,
    
    SUM(CASE 
          WHEN medal IS NULL
          THEN 1
          ELSE 0 END) AS no_medals,

    SUM(CASE 
         WHEN medal = 'Bronze'
         THEN 1
         ELSE 0 END) AS bronze_medals,
         
    SUM(CASE 
         WHEN medal = 'Silver'
         THEN 1
         ELSE 0 END) AS silver_medals,
         
    SUM(CASE 
         WHEN medal = 'Gold'
         THEN 1
         ELSE 0 END) AS gold_medals
FROM
(SELECT
    POSITION (' ' IN name) - 1 AS length_of_name,
    
    (CASE 
        WHEN POSITION (' ' IN name) - 1 <= 0
        THEN 1
        ELSE POSITION (' ' IN name) - 1 END) AS fixed_length_of_name,
        
    medal
FROM 
    datasets.olympics_athletes_events) tmp
GROUP BY tmp.fixed_length_of_name
```

3. Find the number of men and women for each olympic games and order by gender balance ratio. The formula for this balance is (total_men) / (total_woman + 1). We add 1 to avoid division by 0.

Hint: Use pivot table techniques.

Columns: `games` and `sex`

```sql
SELECT
    *
FROM
(SELECT
  games,
  
  SUM(
    CASE 
    WHEN sex = 'M' 
    THEN 1 
    ELSE 0
    END
  ) AS total_men,
  
  SUM(
    CASE 
    WHEN sex = 'F' 
    THEN 1 
    ELSE 0
    END
  ) AS total_women
  
FROM 
    datasets.olympics_athletes_events
GROUP BY games) tmp
ORDER BY tmp.total_men / (tmp.total_women + 1)
```

4. Find which year was each sport first played, last played and the total number of years that sport was played. Which is the newest olympic sport?

Columns: `year` and `sport`

```sql
SELECT
    sport,
    MIN(year)   AS first_time_played,
    MAX(year)   AS last_time_played,
    COUNT(year) AS total_years_played
FROM
(SELECT
    DISTINCT

    sport,
    year
FROM datasets.olympics_athletes_events) tmp
GROUP BY sport
ORDER BY first_time_played DESC
```

5. Find all Norwegian alpine skiers who played in 1992 but didn't play in 1994.

Columns: `name`, `team`, `year`

Hint: Use the EXCEPT statement.

```sql
-- Find all norwegians who played in 1992
SELECT
    name
FROM 
    datasets.olympics_athletes_events
WHERE 
    team = 'Norway' AND sport = 'Alpine Skiing' AND year = 1992
    
-- Names from query above which do not occur in query below
EXCEPT

-- Find all norwegians who played in 1994
SELECT
    name
FROM 
    datasets.olympics_athletes_events
WHERE 
    team = 'Norway' AND sport = 'Alpine Skiing' AND year = 1994
```

6. Find out the youngest and the oldest athlete's data.

Columns: `age`

```sql
SELECT
    main.*
FROM 
    datasets.olympics_athletes_events main
INNER JOIN
    (SELECT
        MIN(age) AS min_age,
        MAX(age) AS max_age
    FROM 
        datasets.olympics_athletes_events) tmp
ON main.age = tmp.min_age OR main.age = tmp.max_age
```

7.How did average male height change in the course of the olympics from 1896 till 2016?

Columns: `year`, `height` and `sex`

Hint: Use LAG

```sql
SELECT
    tmp.year,
    tmp.avg_height,
    COALESCE(LAG(avg_height, 1) OVER (ORDER BY year), 172.73) AS prev_avg_height,
    tmp.avg_height - COALESCE(LAG(avg_height, 1) OVER (ORDER BY year), 172.73) AS avg_height_diff
FROM
    (SELECT
        year,
        AVG(height) AS avg_height
    FROM
        datasets.olympics_athletes_events
    WHERE sex = 'M'
    GROUP BY year
    ORDER BY year) tmp
```

8. What is the median age of gold medal winners?

Columns: `age`, `medal`

Hint: Use NTILE. Ignore NULL ages.

```sql
SELECT
    DISTINCT age
FROM
(SELECT
    age,
    NTILE(100) OVER (ORDER BY age) AS percentile
FROM datasets.olympics_athletes_events
WHERE medal = 'Gold' AND age is NOT NULL
ORDER BY age) tmp
WHERE percentile = 50
```

9. Make a pivot table of Chinese medalists from 2000 to 2016, counting only summer olympics. The rows are the medals, the columns are years and a sum total and the table entries are the counts.

Columns: `medal`  and `year`

```sql
SELECT
    medal,
    SUM(year2000) AS year2000,
    SUM(year2004) AS year2004,
    SUM(year2008) AS year2008,
    SUM(year2012) AS year2012,
    SUM(year2016) AS year2016,
    
    SUM(year2000) + SUM(year2004) + SUM(year2008) + SUM(year2012) + SUM(year2016) AS total
FROM
    (SELECT
        medal,
        
        (CASE 
            WHEN year = 2000 THEN cnt ELSE 0 END
        ) AS year2000,
        
        (CASE  
            WHEN year = 2004 THEN cnt ELSE 0 END
        ) AS year2004,
        
        (CASE 
            WHEN year = 2008 THEN cnt ELSE 0 END
        ) AS year2008,
        
        (CASE 
            WHEN year = 2012 THEN cnt ELSE 0 END
        ) AS year2012,
        
        (CASE 
            WHEN year = 2016 THEN cnt ELSE 0 END
        ) AS year2016
    FROM
        (SELECT
            year,
            medal,
            COUNT(*) AS cnt
        FROM
            datasets.olympics_athletes_events
        WHERE team = 'China' AND year IN (2000, 2004, 2008, 2012, 2016) AND medal IS NOT null
        GROUP BY year, medal) tmp) tmp2
GROUP BY tmp2.medal
ORDER BY total
```


10. CHALLENGE: Make a pivot table whose rows are all events and whose columns are top 3 teams by the count of their won medals of any type from the last olympics (Rio De Janeiro 2016).

The output looks like:

| event | gold_team | silver_team | bronze_team |
|---|---|---|---|
Archery Men's Individual| France with 1 medals | United States with 1 medals| South Korea with 1 medals | 
Archery Men's Team | United States with 3 medals | Australia with 3 medals | South Korea with 3 medals

Keep in mind that for some events only athletes from one or two nations compete so there is no top 3. Use 'No Team' in such cases. 

Columns: `event`, `team` and `medal`

```sql
SELECT
    event,
    -- MAX works here because MAX('abc', NULL) is 'abc'
    MAX(gold_team) AS gold_team,
    COALESCE(MAX(silver_team), 'No Team') AS silver_team,
    COALESCE(MAX(bronze_team), 'No Team') AS bronze_team
FROM
-- Make the first version of the pivot table which we must compact using groupby
(SELECT
    event, 
    
    CASE 
        WHEN team_position = 1 
        THEN team || ' with ' || medals_count || ' medals'
        ELSE NULL 
    END AS gold_team,
    
    CASE 
        WHEN team_position = 2
        THEN team || ' with ' || medals_count || ' medals'
        ELSE NULL 
    END AS silver_team,
    
    CASE 
        WHEN team_position = 3
        THEN team || ' with ' || medals_count || ' medals'
        ELSE NULL 
    END AS bronze_team
FROM
-- Using ROW_NUMBER() find the position of the team in that event 
(SELECT
    event,
    team,
    medals_count,
    ROW_NUMBER()
    OVER (PARTITION BY event ORDER BY medals_count DESC) AS team_position
FROM
    -- Find the count of medals won for each team and event pair in 2016
    (SELECT
        event,
        team,
        COUNT(*) AS medals_count
    FROM
        datasets.olympics_athletes_events
    WHERE medal is NOT NULL AND year = 2016
    GROUP BY event, team) tmp) tmp2
WHERE tmp2.team_position <= 3) tmp3
GROUP BY event
```