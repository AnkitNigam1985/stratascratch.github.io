# Intermediate SQL Exercises

This exercises help you master the intermediate level concepts.

# Questions

## 1. Make a report showing the number of survivors and nonsurvivors per passenger class.	

Schema: `datasets`

Table: `titanic`

Hints:
- Use pivot table techniques to make the report.
- Use statements of form SUM (CASE WHEN ... THEN 1 ELSE 0 END)

```sql
SELECT
    survived,
    sum(CASE WHEN pclass = 1 THEN 1 ELSE 0 END) AS first_class,
    sum(CASE WHEN pclass = 2 THEN 1 ELSE 0 END) AS second_class,
    sum(CASE WHEN pclass = 3 THEN 1 ELSE 0 END) AS third_class
FROM datasets.titanic
GROUP BY 
    survived
```

## 2. How are survivors distributed by gender and passenger class?

Schema:	`datasets`

Table:	`titanic`	

Hints:
- Use the query from question 1 and extend it

```sql
SELECT
    sex,
    sum(CASE WHEN pclass = 1 THEN 1 ELSE 0 END) AS first_class,
    sum(CASE WHEN pclass = 2 THEN 1 ELSE 0 END) AS second_class,
    sum(CASE WHEN pclass = 3 THEN 1 ELSE 0 END) AS third_class
FROM
    datasets.titanic
WHERE
    survived = 1
GROUP BY 
    sex
```

## 3. What is the age of oldest survivor per passenger class?

Schema:	`datasets`

Table: `titanic`

Hints:
- You will have to use subqueries.

```sql
SELECT
    t.pclass,
    t.name
FROM
    datasets.titanic t
INNER JOIN
    (SELECT
        pclass,
        MAX(age) AS oldest_survivor_age
    FROM
        datasets.titanic
    WHERE
        survived = 0
    GROUP BY pclass) tmp
ON
    t.pclass = tmp.pclass AND
    t.age = tmp.oldest_survivor_age
```

## 4. Show the team divisions per player.	

Schema: `datasets`

Tables:	
- `college_football_teams`
- `college_football_players`

Hints:
- Use inner join on the two tables
- Use school_name as the join key

```sql
SELECT
    players.player_name,
    teams.division
FROM
    datasets.college_football_teams teams 
INNER JOIN
    datasets.college_football_players players
ON
    teams.school_name = players.school_name
```


## 5. Find the average player height per division.

Schema:	`datasets`

Tables:	
- `college_football_teams`
- `college_football_players`	

Hints:
- Extend the query from previous question with a GROUP BY.

```sql
SELECT
    teams.division,
    avg(height) AS average_height
FROM
    datasets.college_football_teams teams 
INNER JOIN
    datasets.college_football_players players
ON
    teams.school_name = players.school_name AND
    players.position = 'RB'
GROUP BY
    teams.division
```

## 6. Show user language breakdown per country.	

Schema:	`datasets`

Tables:
- `playbook_users`
- `playbook_events`	

Hints:
- Join the the two tables on the user_id key.
- Group by on both location and language

```sql
SELECT
    events.location,
    users.language,
    count(*) AS n_speakers
FROM
    datasets.playbook_users users
INNER JOIN
    datasets.playbook_events events
ON
    users.user_id = events.user_id
GROUP BY
    events.location,
    users.language
ORDER BY
    events.location ASC,
    n_speakers DESC
```

## 7. How many events happened on mac book in Argentina from users which are not speaking Spanish? What language are they speaking?	
    
Schema: `datasets`

Tables:
- `playbook_users`
- `playbook_events`	

Hints:
- Use inner join on the two tables with user_id as the join key
- In where use column from both tables to construct the filtering predicate
- Use groupby over language and company id to find the answer

```sql
SELECT
    users.company_id,
    users.language,
    count(*) AS n_macbook_pro_users
FROM
    datasets.playbook_users users
INNER JOIN
    datasets.playbook_events events
ON
    users.user_id = events.user_id
WHERE 
    users.language != 'spanish' AND
    events.location = 'Argentina' AND
    events.device = 'macbook pro'
GROUP BY
    users.company_id,
    users.language
```

## 8. For each language find the number of users who prefer apple products. Assume apple products are only macbook pro, iphone 5s and ipad air.	

Schema:	`datasets`

Table:	
- `playbook_users`
- `playbook_events`	
    
Hints:
- Use inner join on the two tables with user_id as the join key
- Use statements of form SUM (CASE WHEN ... THEN 1 ELSE 0 END)

```sql
SELECT
    users.language,
    sum(CASE WHEN device IN ('macbook pro', 'iphone 5s', 'ipad air') THEN 1 ELSE 0 END) AS n_apple_users,
    count(*) AS n_total_users
FROM
    datasets.playbook_users users
INNER JOIN
    datasets.playbook_events events
ON
    users.user_id = events.user_id
GROUP BY
    users.language
ORDER BY
    n_total_users DESC
```

## 9. How many logins Spanish speakers made per country?

Schema:	`datasets`

Table:
- `playbook_users`
- `playbook_events`	

Hints:
- Use the event_type, language, location and user_id columns
- Use both inner join and group by

```sql
SELECT
    events.location,
    count(*) AS n_logins
FROM
    datasets.playbook_events events
INNER JOIN
    datasets.playbook_users users
ON
    events.user_id = users.user_id AND
    events.event_name = 'login' AND
    users.language = 'spanish'
GROUP BY 
    events.location
ORDER BY 
    count(*) DESC
```

## 10. Tabulate occurence counts for a list of event names along with the number of days between their occurence and the registration date of the user.	

Schema:	`datasets`

Tables:
- `playbook_users`
- `playbook_events`	

Hints:
- Use the event_name, occured_at, activated_at and user_id columns.
- Use both inner join on user_id and group by
- The group by columns are event_name and days_since_activation
- days_since_activation is a new column you must make yourself using EXTRACT ('DAY' FROM occured_at - activated_at)
- Use the count function.

```sql
SELECT
    events.event_name,
    EXTRACT('DAY' FROM events.occurred_at - users.activated_at) AS days_since_activation,
    count(*) AS n_times_event_happened
FROM
    datasets.playbook_events events
INNER JOIN
    datasets.playbook_users users
ON
    events.user_id = users.user_id
GROUP BY
    events.event_name,
    days_since_activation
```