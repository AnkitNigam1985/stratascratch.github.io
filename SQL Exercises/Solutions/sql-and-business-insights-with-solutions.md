# SQL and Business Insights *(with Solutions)*

## Instructions 
- Log-in to your Strata Scratch account. 
- All tables used in this exercise are taken from the datasets schema. Make sure you select this schema on the SQL editor settings. 
- Try to answer the following questions by writing the appropriate SQL query on the editor. 
- For most of the questions, you will need to produce a chart that shows a list of dimensions (e.g., music artists) and counts 
(e.g., the number of grammies they’ve won).
- For each topic, there are also one to two open ended questions (e.g., who is the best athlete to be drafted into the NFL?). 
You’ll need to analyze the dataset, create your own metrics, and provide a convincing argument.
- This is the teacher version of the SQL and business insights exercise. The questions below are followed with the correct solution. 
Each topic includes one to two open ended questions which could have numerous possible solutions, and one 
of these solutions is shown here. 

## Questions

### 1. Billboard Top 100

Dataset: `billboard_top_100_year_end`

- What were the top songs for the past 30 years?
- Which artists has had the most top 10 songs over the years? What were the songs?
- Which artists has been on the billboard top 100 the most in the past 10 years?
- Who is the best artist in the past 50 years? What metrics would you use to answer this question?

### 2. SF Crime

Dataset: `sf_crime_incidents_2014_01`

- What were the top crime categories in 2014? How many incidences per category?

*Solution:*
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

### 3. Oscar Nominees

Dataset: `oscar_nominees`

- Who has won the most oscars?

*Solution:*
```sql
   SELECT
      nominee,
      count(winner) as count
   FROM datasets.oscar_nominees
   WHERE winner ='true'
   GROUP BY nominee
   ORDER BY count DESC
```

- Who has been nominated the most times but has never won?

*Solution:*
```sql
   SELECT
      nominee,
      count(winner) as count
   FROM datasets.oscar_nominees
   WHERE winner ='false'
   GROUP BY nominee
   ORDER BY count DESC
```
- Who has the highest win to nomination ratio?

*Solution:*
```sql
   SELECT
      nominee,
      sum (case when winner = 'true' then 1 else 0 end)/count(nominee)::float as ratio
   FROM datasets.oscar_nominees
   GROUP BY nominee
   ORDER BY ratio desc
```

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

- Who is the best actor/actress of all time? What metrics would you use to convince me?

*Solution:*
```sql
   SELECT
      nominee,
      count(winner) as count
   FROM datasets.oscar_nominees
   WHERE winner ='true'
   GROUP BY nominee
   ORDER BY count DESC
```

### 4. Video Game Charts

Dataset: `global_weekly_charts_2013_2014`

- Which games has been in the top 100 for the longest? How many weeks were they in the top 100?
- Which games, by platform, has been in the top 10 for the longest?
- What genres are the most popular?
- Which are the best publishers? What metrics would you use to analyze?

### 5. NFL Combine

Datasets: `nfl_combine`

- Which colleges produces the most nfl players?
- How does the 40 yard dash, vertical, broad jump, and bench differ between atheletes drafted into the NFL vs undrafted? How do the metrics vary by draft round?
- Which colleges produces the best quarterbacks? How about other positions? How did you define the best?
- Who is the best athelete? What metrics would you analyze to prove your case?

### 6. Airbnb

Datasets: `datasets.airbnb_searches`, `datasets.airbnb_contacts`

- How many people search for hosts on airbnb?
- How many nights are most people searching for when trying to book a host?
- What day of the week are most people checking in? What day of the week are most people checking out?
- What type of rooms are most people searching for?
- What’s the acceptance rate of requests?
- What can airbnb do to increase the number of bookings?


