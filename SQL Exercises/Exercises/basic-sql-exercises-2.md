# Basic SQL Exercises 2

## Instructions
- Log-in to your Strata Scratch account.
- All tables used in this exercise are taken from the datasets schema. Make sure you select this schema on the SQL editor settings.
- Try to answer the following questions by writing the appropriate SQL query on the editor.

# Questions

## 1. List all companies working in finacials sector which are headquarted in Europe or Asia.	

Schema:	`datasets`

Table:	`forbes_global_2010_2014`	

Hints:
- Use the continent and sector columns
- Utilize both OR with AND


## 2. Find the most profitable company from the financials sector in the entire world. What continent is it from?	

Schema:	`datasets`

Table:	`forbes_global_2010_2014`

Hint:	
- Use order by and limit


## 3. For each sector find the maximum market value and order the sectors by it. Which sector is it best to invest in?	

Schema:	`datasets`

Table:	`forbes_global_2010_2014`

Hints:
- Group by sector
- Order by max of marketvalue in descending order.


## 4. How are companies distributed among countries considering only the best sector from previous question?	

Schema:	`datasets`

Tables:	`forbes_global_2010_2014`

Hints:
- You are studying knowledge from the best sector right now.


## 5. Which industry shows profit on average while having the lowest sales of all industries?	

Schema:	`datasets`

Table: `forbes_global_2010_2014`	

Hints:
- Use having to filter away all industries whose average profit is 0 or lower.
- Order by the minimum of sales in descending order to obtain the wanted industry.


## 6. How many users speak English, German, French or Spanish?	

Schema:	`datasets`
Table:	`playbook_users`

Hints:
- Use the IN statement along with a list of languages enclosed in brackets.


## 7. Find the id of companies which have more than 10 users which are not speaking English, German, French or Spanish.

Schema:	`datasets`

Table:	`playbook_users`	

Hints:
- Invert the logic now. Use not in.
- Make a groupby over company ids and using having filter away all companies with less than 10 users


## 8. Is English more popular compared to French? What about other languages? Order all languages by the number of users speaking them.

Schema:	`datasets`

Table:	`playbook_users`

Hints:
- Group by language


## 9. Find the company with a highest number of users which has a difference of more than 365 days between first and last activation dates.	

Schema:	`datasets`

Table:	`playbook_users`

Hints:
- Use min and max functions over the activated_at column to filter away in having column
- max activated_at - min activated_at gives the number of days between these two dates


## 10. What is the language breakdown for the company from previous question?	

Schema:	`datasets`
Table:	`playbook_users`

Hints:
- The company in question has id 1 in case you were unable to answer the previous question.