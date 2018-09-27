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


2. Find all Danish athletes who won a medal.

Columns: `team` and `medal`


3. Find the first 15 athlethes which competed in swimming in any game which happened in London.

Columns: `sport` and `city`


4. Find all events in which Christine Jacoba Aaftink participated.

Columns: `name` and `event`


5. Find all non adult players and the games they participated in when they were still not adults. Assume the age of becoming and adult is 18 years

Columns: `name`, `age` and `games`.


## Intermediate exercises

1. Find all athletes who were older than 40 years at the time of the games who won either bronze or silver medals. Use the IN clause in your solution.

Columns: `name`, `age` and `medal`


2. Find all events during any winter olympics in which all athletes were of height between 180 and 210 centimeters. 

Columns: `event`, `season`, `height`


3. There were athletes which participated in multiple teams. For example John Mary Pius Boland who played for both Germany and Great Britain. Find all these along with the games they participated in, their sport and their possible medal.

Columns: `name`, `team`, `games`, `sport`, `medal`


4. Order all countries by the year they first participated in the olympics.

Hint: Use the `noc` column in the group by clause.

Columns: `noc` and `year`


5. Which games attract more athletes? The winter or summer ones?

Hint: To find the number of athletes count only the unique names.

Columns: `season`, `name`


6. Which games in the whole history of the olympics had the highest number of athletes. Your solution must return only a single row.

Columns: `games`, `name`



7. What are the min, mean and max ages over all olympics?

Columns: `age`


8. Find the average weight of medal winning Judo players for each team with the condition that the minimum age of team is 20 years and the maximum age of the team is 30 years. Minimum age of team is the minimum of all ages of all athletes which are part of that team.

Hint: Use a HAVING clause.

Columns: `team`, `weight`, `sport`, `medal`, `age`


9. Find all distinct sports in which obese people participated. Obese people are those whose Body mass index exceeds 30. The body mass index is calculated as weight / (height * height). The height in the formula is in meters while height in our dataset is in centimeters. You can ignore athletes for whom no weight or height is measured.

Columns: `weight`, `height`, `sport`


10. Find the year in which the shortest athlete participated.

Columns: `year` and `height`


11. How many athletes competing in Football won Gold medals aggregated by their national olympic committees and their sex. 

Columns: `noc`, `sex`, `sport`, `medal`



12. Which games had the highest number of people who went home without a medal.

Columns: `games` and `medal`



13. Classify each athlete as either a Patriot for playing for only a single team or as Globalist for playing for multiple teams.

Hint: Use a CASE statement.

Columns: `name` and `team`



14. The following European cities hosted the olympics. Use a CASE statement to classify each olympics as being either European or not. Cities: London, Roma, Antwerpen, Amsterdam, Stockholm, Sarajevo, Berlin, Grenoble, Moskva, Oslo, Athina, Paris, Munich, Garmisch-Partenkirchen, Sochi, Torino, Innsbruck, Helsinki, Barcelona. 

Columns: `city`


## Advanced exercises

1. Use the classification from previous question and count the number of athletes which participated in European cities using a subquery.

Columns: `city`


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


3. Find the number of men and women for each olympic games and order by gender balance ratio. The formula for this balance is (total_men) / (total_woman + 1). We add 1 to avoid division by 0.

Hint: Use pivot table techniques.

Columns: `games` and `sex`


4. Find which year was each sport first played, last played and the total number of years that sport was played. Which is the newest olympic sport?

Columns: `year` and `sport`


5. Find all Norwegian alpine skiers who played in 1992 but didn't play in 1994.

Columns: `name`, `team`, `year`

Hint: Use the EXCEPT statement.


6. Find out the youngest and the oldest athlete's data.

Columns: `age`


7.How did average male height change in the course of the olympics from 1896 till 2016?

Columns: `year`, `height` and `sex`

Hint: Use LAG


8. What is the median age of gold medal winners?

Columns: `age`, `medal`

Hint: Use NTILE. Ignore NULL ages.


9. Make a pivot table of Chinese medalists from 2000 to 2016, counting only summer olympics. The rows are the medals, the columns are years and a sum total and the table entries are the counts.

Columns: `medal`  and `year`


10. CHALLENGE: Make a pivot table whose rows are all events and whose columns are top 3 teams by the count of their won medals of any type from the last olympics (Rio De Janeiro 2016).

The output looks like:

| event | gold_team | silver_team | bronze_team |
|---|---|---|---|
Archery Men's Individual| France with 1 medals | United States with 1 medals| South Korea with 1 medals | 
Archery Men's Team | United States with 3 medals | Australia with 3 medals | South Korea with 3 medals

Keep in mind that for some events only athletes from one or two nations compete so there is no top 3. Use 'No Team' in such cases. 

Columns: `event`, `team` and `medal`