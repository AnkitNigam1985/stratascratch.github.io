# Basic SQL Exercises

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

## 2. How many of survirors were women first class passengers?	

Schema:	`datasets`

Table:	`titanic`	

Hints:
- Use the columns pclass, sex and survived to filter.
- Use the count function

## 3. What is the average fare paid by those who died in the accident?	

Schema:	`datasets`

Table:	`titanic`

Hints:
- Filter by survived = 0
- Use the avg function
   
## 4. How many people survived per passenger class?	

Schema: `datasets`

Table:	`titanic`	

Hints:
- Use the sum function
- Use the columns pclass and survived. The column survived is 1 when the passenger survived and 0 when (s)he died.

## 5. What is the average height of quarterbacks?

Schema:	`datasets`

Table:	`nfl_combine`

Hints:
- Use the avg function
- Quarterbacks can be found by having the position column valued as 'QB'

## 6. Which year had the highest number of players?	

Schema:	`datasets`

Table:	`nfl_combine`

Hints:
- Use order by along with limit.
- Perform group by over the year column"
   
## 7. How many players with weight <= 50 or weight >= 200 are there for each college?	

Schema: `datasets`

Table: `nfl_combine`

Hints:
- Use OR to find people which have weight below 50 and above 200 pounds
- Use count distinct over the name column to find the solution.

## 8. What are the best and worst total SAT scores per school?	

Schema:	`datasets`

Table:	`sat_scores`	

Hints:
- Use the min and max functions
- Perform the grouping over the school column

## 9. How many students each teacher lectured to?	

Schema:	`datasets`

Table:	`sat_scores`

Hints:
- Group by teacher and use the count function.


## 10. Who is the student who has highest efficency for mathematics? Efficency is defined as number of points obtained divided by hours studied.

Schema:	`datasets`

Table:	`sat_scores`	

Hints
- First remove all students who have studied less than a single hour (column hrs_studied)
- Use order by and limit 1.