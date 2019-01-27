# Basic SQL Exercises (*with Solutions*)

## Instructions
- Log-in to your Strata Scratch account.
- All tables used in this exercise are taken from the datasets schema. Make sure you select this schema on the SQL editor settings.
- Try to answer the following questions by writing the appropriate SQL query on the editor.
- This is the teacher version of the SQL basic exercises. Each question is followed with the correct solution and output.

# Questions

## 1. How many people embarked on the Titanic?	

Schema: `datasets`

Table:  `titanic`

Hint:
- Use the count function

```sql
SELECT
    count(passengerid) AS n_passengers_embarked
FROM datasets.titanic
```

## 2. How many of survirors were women first class passengers?	

Schema:	`datasets`

Table:	`titanic`	

Hints:
- Use the columns pclass, sex and survived to filter.
- Use the count function

```sql
SELECT
   count(passengerid) AS n_women_first_class_survivors
FROM datasets.titanic
WHERE
   pclass = 1 AND 
   sex = 'female' AND
   survived = 1
```

## 3. What is the average fare paid by those who died in the accident?	

Schema:	`datasets`

Table:	`titanic`

Hints:
- Filter by survived = 0
- Use the avg function

```sql
SELECT 
    avg(fare) AS non_survivor_average_fare
FROM datasets.titanic
WHERE 
    survived = 0
```
   
## 4. How many people survived per passenger class?	

Schema: `datasets`

Table:	`titanic`	

Hints:
- Use the sum function
- Use the columns pclass and survived. The column survived is 1 when the passenger survived and 0 when (s)he died.

```sql
SELECT 
    pclass,
    SUM(survived) AS n_survived
FROM datasets.titanic
GROUP BY 
    pclass
```

## 5. What is the average height of quarterbacks?

Schema:	`datasets`

Table:	`nfl_combine`

Hints:
- Use the avg function
- Quarterbacks can be found by having the position column valued as 'QB'

```sql
SELECT 
   avg(heightinchestotal) AS avg_height_inches
FROM datasets.nfl_combine
WHERE 
    position = 'QB'
```

## 6. Which year had the highest number of players?	

Schema:	`datasets`

Table:	`nfl_combine`

Hints:
- Use order by along with limit.
- Perform group by over the year column"

```sql
SELECT
    year,
    count(*) AS n_players
FROM
    datasets.nfl_combine
GROUP BY 
    year
ORDER BY 
   n_players DESC
LIMIT 1
```

## 7. How many players with weight <= 50 or weight >= 200 are there for each college?	

Schema: `datasets`

Table: `nfl_combine`

Hints:
- Use OR to find people which have weight below 50 and above 200 pounds
- Use count distinct over the name column to find the solution.

```sql
SELECT
    college,
    count(DISTINCT name) AS n_players
FROM datasets.nfl_combine
WHERE
    weight >= 200 OR
    weight <= 50
GROUP BY 
    college
```

## 8. What are the best and worst total SAT scores per school?	

Schema:	`datasets`

Table:	`sat_scores`	

Hints:
- Use the min and max functions
- Perform the grouping over the school column

```sql
SELECT
    school,
    
    max(sat_writing + sat_verbal + sat_math) AS best_score,
    min(sat_writing + sat_verbal + sat_math) AS worst_score
FROM datasets.sat_scores
GROUP BY
    school
```

## 9. How many students each teacher lectured to?	

Schema:	`datasets`

Table:	`sat_scores`

Hints:
- Group by teacher and use the count function.

```sql
SELECT
    teacher,
    
    COUNT(student_id) AS n_students
FROM datasets.sat_scores
GROUP BY
    teacher
```

## 10. Who is the student who has highest efficency for mathematics? Efficency is defined as number of points obtained divided by hours studied.

Schema:	`datasets`

Table:	`sat_scores`	

Hints
- First remove all students who have studied less than a single hour (column hrs_studied)
- Use order by and limit 1.

```sql
SELECT
    student_id,
    hrs_studied,
    sat_math,
    sat_math / hrs_studied AS points_per_hour
FROM datasets.sat_scores
WHERE hrs_studied > 0
ORDER BY 
    points_per_hour DESC
LIMIT 1
```