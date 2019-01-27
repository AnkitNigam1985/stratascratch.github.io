# Intermediate SQL Exercises

This exercises help you master the intermediate level concepts.

# Questions

## 1. Make a report showing the number of survivors and nonsurvivors per passenger class.	

Schema: `datasets`

Table: `titanic`

Hints:
- Use pivot table techniques to make the report.
- Use statements of form SUM (CASE WHEN ... THEN 1 ELSE 0 END)

## 2. How are survivors distributed by gender and passenger class?

Schema:	`datasets`

Table:	`titanic`	

Hints:
- Use the query from question 1 and extend it

## 3. What is the age of oldest survivor per passenger class?

Schema:	`datasets`

Table: `titanic`

Hints:
- You will have to use subqueries.

## 4. Show the team divisions per player.	

Schema: `datasets`

Tables:	
- `college_football_teams`
- `college_football_players`

Hints:
- Use inner join on the two tables
- Use school_name as the join key


## 5. Find the average player height per division.

Schema:	`datasets`

Tables:	
- `college_football_teams`
- `college_football_players`	

Hints:
- Extend the query from previous question with a GROUP BY.


## 6. Show user language breakdown per country.	

Schema:	`datasets`

Tables:
- `playbook_users`
- `playbook_events`	

Hints:
- Join the the two tables on the user_id key.
- Group by on both location and language


## 7. How many events happened on mac book in Argentina from users which are not speaking Spanish? What language are they speaking?	
    
Schema: `datasets`

Tables:
- `playbook_users`
- `playbook_events`	

Hints:
- Use inner join on the two tables with user_id as the join key
- In where use column from both tables to construct the filtering predicate
- Use groupby over language and company id to find the answer

## 8. For each language find the number of users who prefer apple products. Assume apple products are only macbook pro, iphone 5s and ipad air.	

Schema:	`datasets`

Table:	
- `playbook_users`
- `playbook_events`	
    
Hints:
- Use inner join on the two tables with user_id as the join key
- Use statements of form SUM (CASE WHEN ... THEN 1 ELSE 0 END)


## 9. How many logins Spanish speakers made per country?

Schema:	`datasets`

Table:
- `playbook_users`
- `playbook_events`	

Hints:
- Use the event_type, language, location and user_id columns
- Use both inner join and group by


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
