# Basic SQL Exercises 2 (*with Solutions*)

## Instructions
- Log-in to your Strata Scratch account.
- All tables used in this exercise are taken from the datasets schema. Make sure you select this schema on the SQL editor settings.
- Try to answer the following questions by writing the appropriate SQL query on the editor.
- This is the teacher version of the SQL basic exercises. Each question is followed with the correct solution and output.

## Questions

1. How many different origin airports exist? What are their IATA codes? 

    `Table: datasets.us_flights`

    *Solution*
    ```sql
    SELECT DISTINCT origin 
    FROM datasets.us_flights
    ```

    ```sql
    SELECT COUNT(DISTINCT origin)
    FROM datasets.us_flights
    ```

    *Output:* `312`

2. Give me a list of 5 origin, destination airport pairs which are furthest apart from each other (Hint: Use the distance column)

    `Table: datasets.us_flights`

    *Solution*

    ```sql
    SELECT DISTINCT
        unique_carrier,
        origin,
        dest,
        distance
    FROM datasets.us_flights
    ORDER BY distance DESC
    LIMIT 5
    ```

3. Select all US flights which had no delay. (Hint: Use the arr_delay column)

    `Table: datasets.us_flights`

    *Solution*:

    ```sql
    SELECT
        *
    FROM datasets.us_flights
    WHERE arr_delay = '0.0'
    ```

4. What is the average distance an airplane travels from each origin airport.

    `Table: datasets.us_flights`

    *Solution*:

    ```sql
    SELECT
        origin,
        AVG(distance) AS avg_distance
    FROM datasets.us_flights
    GROUP BY origin
    ```

5. How many flights did American Airlines (AA) cancel? (Hint: cancelled column has 1 for canceled flights)

    `Table: datasets.us_flights`

    *Solution*:

    ```sql
        SELECT
            count(*)
        FROM datasets.us_flights
        WHERE cancelled = 1 AND unique_carrier = 'AA'
    ```

6. Which companies are present in the financial sector in Eurasia.

    `Table: datasets.forbes_global_2010_2014`

    *Solution*: Eurasia is same as Europe or Asia

    ```sql
    SELECT
        company
    FROM datasets.forbes_global_2010_2014
    WHERE 
        (continent = 'Asia' OR continent = 'Europe') AND
        (sector = 'Financials')
    ```

7. What is the profit to sales ratio (profit / sales) for Royal Dutch Shell?

    `Table: datasets.forbes_global_2010_2014`

    *Solution*: We can write queries which return only one row.

    ```sql
        SELECT
            company,
            profits / sales AS profit_to_sales_ratio
        FROM datasets.forbes_global_2010_2014
        WHERE 
            company = 'Royal Dutch Shell'
    ```

8. What are the 3 most profitable companies in the entire world? (Hint: order by profit)

    `Table: datasets.forbes_global_2010_2014`

    *Solution*: When we order by DESC we go from maximal profit to minimal profit

    ```sql
    SELECT
        *
    FROM datasets.forbes_global_2010_2014
    ORDER BY profits DESC
    LIMIT 3
    ```

9. Find the biggest market value for each sector.

    `Table: datasets.forbes_global_2010_2014`

    *Solution*: 

    ```sql
    SELECT
        sector,
        MAX(marketvalue)
    FROM datasets.forbes_global_2010_2014
    GROUP BY sector
    ```

10. Which industry has the lowest sales while still making an average profit higher than 0. (Hint: Use a HAVING clause)

    `Table: datasets.forbes_global_2010_2014`

    *Solution*: 

    ```sql
    SELECT
        industry,
        -- These two columns are here for explanation purposes so you can see the numbers
        -- They are not required for the output.
        MIN(sales) AS min_sales,
        AVG(profits) AS avg_profit
    FROM
        datasets.forbes_global_2010_2014
    GROUP BY industry
    -- These two lines matter most for this query
    HAVING AVG(profits) > 0
    ORDER BY MIN(sales) ASC
    ```

11. Show me the breakdown of languages spoken? (Hint: use count)

    `Table: datasets.playbook_users`

    *Solution*:

    ```sql
    SELECT
        language,
        COUNT(*)
    FROM
        datasets.playbook_users
    GROUP BY language
    ORDER BY count DESC
    ```

12. Find a list of users who speak English, French, German or Spanish (Hint: Use IN)

    `Table: datasets.playbook_users`

    *Solution*: Using IN we can remove a lot of ORs

    ```sql
    SELECT
        *
    FROM
        datasets.playbook_users
    WHERE language IN ('english', 'german', 'french', 'spanish')
    ```

13. What are the companies that have at least 10 Chinese speaking users?

    `Table: datasets.playbook_users`

    *Solution*: 
    - The trick is to know what filter to put in what place. 
    - We filter first by language in WHERE so we get only Chinese speaking users.
    - The HAVING filter is where we discard all companies which have less than 10 users.

    Keep in mind that all filters which are based on aggregations (like COUNT(*) here) must go in the HAVING clause. 

    ```sql
    SELECT
        company_id
    FROM
        datasets.playbook_users
    WHERE language='chinese'
    GROUP BY company_id
    HAVING COUNT(*) >= 10
    ```

14. In how many movies did Abigail Breslin star?

    `Table: datasets.oscar_nominees`

    *Solution*: 1

    ```sql
    SELECT
        count(*)
    FROM datasets.oscar_nominees
    WHERE nominee = 'Abigail Breslin'
    ```

15. Show me the Oscar winners between 2001 and 2009

    `Table: datasets.oscar_nominees`

    *Solution*:

    ```sql
    SELECT
        *
    FROM datasets.oscar_nominees
    WHERE 
        winner = 'true' AND 
        year BETWEEN 2001 AND 2009  
    ```

