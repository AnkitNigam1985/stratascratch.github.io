# QB Stats Exercise Solutions

This dataset gives information on every NFL game and every passer over 5000 regular season games from 1996 to 2016. 
The excel files supply us with over ten thousand quarterbacks’ names over 21 years and specify the success of each one. 
Each year contains every single game from that season and gives statistics like completions, attempts, yards, and an overall 
rating for the quarterback for that game.

### Question 1
Which quarterback has the highest game points?

*Solution: *

In our query, we begin by selecting the qb column and solving the average of the game points. To know which quarterback has the highest game points, we group the qb results and sort the data in descending order.
```sql
  SELECT qb, avg(game_points) AS avg_points
  FROM datasets.qbstats_1996_2016
  GROUP BY qb
  ORDER BY avg_points DESC
```

### Question 2
Who were the top 10 highest rating quarterbacks?

*Solution:*

To answer the question, we can select the qb and rate columns from the table. We add a condition where rate is not equal to `NULL` so that we can skip rows with `NULL` values. The results are then grouped and sorted so that we can determine the top 10 results.
```sql  
  SELECT qb, rate
  FROM datasets.qbstats_1996_2016
  WHERE rate is NOT NULL
  GROUP BY qb,rate
  ORDER BY rate DESC
  LIMIT 10
```

### Question 3
Who were the top 10 quarterbacks with the highest game points in 2016?

*Solution:*

We can determine the top 10 quarterbacks by selecting the qb and game points columns. To display only the results for the year 2016, we add a conditional statement where `year = 2016`. We can determine the top 10 by simply sorting the results in descending order.
```sql
  SELECT qb, game_points
  FROM datasets.qbstats_1996_2016
  WHERE year = 2016
  ORDER BY game_points DESC
  LIMIT 10
```

### Question 4
Where do the quarterbacks perform better in 2016, at home or away?

*Solution:*

We can determine the quarterback performance by selecting the columns `qb, game_points,` and `home_away` from the table. We set the year to 2016 to filter the results and sort the output in descending order to check the quarterback performance.
```sql
  SELECT qb, game_points, home_away
  FROM datasets.qbstats_1996_2016
  WHERE year = 2016
  ORDER BY game_points DESC
```

### Question 5
What is the average game points each year?

*Solution:*

To answer the question, we simply solve for the average game points and sort the data by year in descending order. This will allow us to easily view the results starting from the latest year.
```sql
  SELECT year, avg(game_points) as average_points
  FROM datasets.qbstats_1996_2016
  GROUP BY year
  ORDER BY year DESC
```

### Question 6
Who throws the longest in 2016?

*Solution:*

Our query starts by selecting the qb and lg which contains the longest throw data from the dataset. Since we are only interested in the results for the year 2016, we add a conditional statement where `year = 2016`. We want to group the results by qb and `longest_throw`, and rank the results in descending order. The top result is the answer. 
```sql
  Select qb, lg as longest_throw
  From datasets.qbstats_1996_2016
  Where year = 2016
  Group by qb, longest_throw
  Order by longest_throw DESC
```

### Question 7
Who had the most touchdowns (TD) in 2016?

*Solution:*

We can answer the question by counting the touchdowns `td` and add the `year = 2016` on the conditional statement. We group and sort the data in descending order to determine the top results.
```sql
  SELECT qb, count(td) as times
  FROM datasets.qbstats_1996_2016
  WHERE year = 2016
  GROUP BY qb
  ORDER BY times DESC
```

### Question 8
Which quarterback played the most games in 2016?

*Solution:*

The question can be answered by first counting `qb` or quarterback rating from the dataset. We add the conditional statement `WHERE year = 2016`, group the data by `qb` and sort the data to know the top results.
```sql
  SELECT qb, count(qb) AS most_appearances
  FROM datasets.qbstats_1996_2016
  WHERE year = 2016 
  GROUP BY qb
  ORDER BY most_appearances DESC
```

### Question 9
Which quarterback had the least amount of interceptions in 2016?

*Solution:*

To know which quarterback has the least amount of interceptions, we must first count the number of interceptions `int` from the dataset. We add the `year = 2016` to filter the results, and group and sort the data in ascending order to know the results which have the least number of interceptions.
```sql
  SELECT qb, count(int) AS least_interceptions
  FROM datasets.qbstats_1996_2016
  WHERE year = 2016
  GROUP BY qb
  ORDER BY least_interceptions ASC
```

### Question 10
Which quarterback has the most QB that tried to throw the ball in 2016?

*Solution:*

We can determine which quarterback has the most QB by counting the `att` from the table. To filter the results, a conditional statement where `year = 2016` is added. We can easily determine from the results which quarterback has the most QB that threw a ball by sorting the data in descending order.
```sql
  SELECT qb, count(att) as times
  FROM datasets.qbstats_1996_2016
  WHERE year = 2016
  GROUP BY qb
  ORDER BY times DESC
```
