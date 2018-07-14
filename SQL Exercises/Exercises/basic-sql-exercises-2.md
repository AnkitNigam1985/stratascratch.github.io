# Basic SQL Exercises 2

## Instructions
- Log-in to your Strata Scratch account.
- All tables used in this exercise are taken from the datasets schema. Make sure you select this schema on the SQL editor settings.
- Try to answer the following questions by writing the appropriate SQL query on the editor.

## Questions

1. How many different origin airports exist? What are their IATA codes? 

    `Table: datasets.us_flights`

2. Give me a list of 5 origin, destination airport pairs which are furthest apart from each other (Hint: Use the distance column)

    `Table: datasets.us_flights`

3. Select all US flights which had no delay. (Hint: Use the arr_delay column)

    `Table: datasets.us_flights`

4. What is the average distance an airplane travels from each origin airport.

    `Table: datasets.us_flights`

5. How many flights did American Airlines (AA) cancel? (Hint: cancelled column has 1 for canceled flights)

    `Table: datasets.us_flights`

6. Which companies are present in the financial sector in Eurasia.

    `Table: datasets.forbes_global_2010_2014`

7. What is the profit to sales ratio (profit / sales) for Royal Dutch Shell?

    `Table: datasets.forbes_global_2010_2014`

8. What are the 3 most profitable companies in the entire world? (Hint: order by profit)

    `Table: datasets.forbes_global_2010_2014`

9. Find the biggest market value for each sector.

    `Table: datasets.forbes_global_2010_2014`

10. Which industry has the lowest sales while still making an average profit higher than 0. (Hint: Use a HAVING clause)

    `Table: datasets.forbes_global_2010_2014`


11. Show me the breakdown of languages spoken? (Hint: use count)

    `Table: datasets.playbook_users`


12. Find a list of users who speak English, French, German or Spanish (Hint: Use IN)

    `Table: datasets.playbook_users`

13. What are the companies that have at least 10 Chinese speaking users?

    `Table: datasets.playbook_users`


14. In how many movies did Abigail Breslin star?

    `Table: datasets.oscar_nominees`

15. Show me the Oscar winners between 2001 and 2009

    `Table: datasets.oscar_nominees`
