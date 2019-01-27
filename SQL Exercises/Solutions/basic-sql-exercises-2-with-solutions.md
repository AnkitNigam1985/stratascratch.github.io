# Basic SQL Exercises 2 (*with Solutions*)

## Instructions
- Log-in to your Strata Scratch account.
- All tables used in this exercise are taken from the datasets schema. Make sure you select this schema on the SQL editor settings.
- Try to answer the following questions by writing the appropriate SQL query on the editor.
- This is the teacher version of the SQL basic exercises. Each question is followed with the correct solution and output.

# Questions

## 1. List all companies working in finacials sector which are headquarted in Europe or Asia.	

Schema:	`datasets`

Table:	`forbes_global_2010_2014`	

Hints:
- Use the continent and sector columns
- Utilize both OR with AND

```sql
SELECT
    company
FROM datasets.forbes_global_2010_2014
WHERE 
    (continent = 'Asia' OR continent = 'Europe') AND
    (sector = 'Financials')
```

## 2. Find the most profitable company from the financials sector in the entire world. What continent is it from?	

Schema:	`datasets`

Table:	`forbes_global_2010_2014`

Hint:	
- Use order by and limit

```sql
SELECT
    company,
    continent
FROM datasets.forbes_global_2010_2014
WHERE 
    sector = 'Financials'
ORDER BY 
    profits DESC
LIMIT 1
```

## 3. For each sector find the maximum market value and order the sectors by it. Which sector is it best to invest in?	

Schema:	`datasets`

Table:	`forbes_global_2010_2014`

Hints:
- Group by sector
- Order by max of marketvalue in descending order.

```sql
SELECT
    sector,
    max(marketvalue) AS max_marketvalue
FROM datasets.forbes_global_2010_2014
GROUP BY 
    sector
ORDER BY 
    max_marketvalue DESC
```

## 4. How are companies distributed among countries considering only the best sector from previous question?	

Schema:	`datasets`

Tables:	`forbes_global_2010_2014`

Hints:
- You are studying knowledge from the best sector right now.

```sql
SELECT
    country,
    count(*) AS n_companies
FROM datasets.forbes_global_2010_2014
WHERE
    sector = 'Information Technology'
GROUP BY 
    country
ORDER BY
    n_companies DESC
```

## 5. Which industry shows profit on average while having the lowest sales of all industries?	

Schema:	`datasets`

Table: `forbes_global_2010_2014`	

Hints:
- Use having to filter away all industries whose average profit is 0 or lower.
- Order by the minimum of sales in descending order to obtain the wanted industry.

```sql
SELECT
    industry
FROM
    datasets.forbes_global_2010_2014
GROUP BY 
    industry
HAVING 
    avg(profits) > 0
ORDER BY 
    min(sales) ASC
LIMIT 1
```

## 6. How many users speak English, German, French or Spanish?	

Schema:	`datasets`
Table:	`playbook_users`

Hints:
- Use the IN statement along with a list of languages enclosed in brackets.

```sql
SELECT
    count(*) AS n_wanted_speakers
FROM datasets.playbook_users
WHERE
    language IN ('english', 'german', 'french', 'spanish')
```


## 7. Find the id of companies which have more than 10 users which are not speaking English, German, French or Spanish.

Schema:	`datasets`

Table:	`playbook_users`	

Hints:
- Invert the logic now. Use not in.
- Make a groupby over company ids and using having filter away all companies with less than 10 users

```sql
SELECT
    company_id
FROM datasets.playbook_users
WHERE
    language NOT IN ('english', 'german', 'french', 'spanish')
GROUP BY
    company_id
HAVING (count(*)) > 10
```

## 8. Is English more popular compared to French? What about other languages? Order all languages by the number of users speaking them.

Schema:	`datasets`

Table:	`playbook_users`

Hints:
- Group by language

```sql
SELECT
    language,
    count(*) AS n_speakers
FROM datasets.playbook_users
GROUP BY 
    language
ORDER BY 
    n_speakers DESC
```

## 9. Find the company with a highest number of users which has a difference of more than 365 days between first and last activation dates.	

Schema:	`datasets`

Table:	`playbook_users`

Hints:
- Use min and max functions over the activated_at column to filter away in having column
- max activated_at - min activated_at gives the number of days between these two dates

```sql
SELECT
    company_id,

    count(user_id) AS n_users
FROM
    datasets.playbook_users
GROUP BY
    company_id
HAVING
    max(activated_at) - min(activated_at) >= 365
ORDER BY 
    n_users DESC
LIMIT 1
```

## 10. What is the language breakdown for the company from previous question?	

Schema:	`datasets`
Table:	`playbook_users`

Hints:
- The company in question has id 1 in case you were unable to answer the previous question.

```sql
SELECT
    language,
    
    count(*) AS n_speakers
FROM
    datasets.playbook_users
WHERE 
    company_id = 1
GROUP BY
    language
```