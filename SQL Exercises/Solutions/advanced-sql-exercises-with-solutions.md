# Advanced SQL Exercises (*with Solutions*)

## Instructions
- Log-in to your Strata Scratch account.
- All tables used in this exercise are taken from the datasets schema. Make sure you select this schema on the SQL editor settings.
- Try to answer the following questions by writing the appropriate SQL query on the editor.
- This is the teacher version of the SQL basic exercises. Each question is followed with the correct solution and output.

## Questions

1. Give back all SAT scores from students whose highschool name does NOT end with 'HS'

    `Tables: sat_scores`

    *Solution*

    ```sql
    SELECT *
    FROM datasets.sat_scores
    WHERE RIGHT(school, 2) <> 'HS'
    ```

2. What is the old to young ratio per olympic game? Young are people whose age <= 30, while old are those whose age >= 45.

    `Tables: olympics_athletes_events`

    *Solution*:

    ```sql
    SELECT
        games,
    
        total_old :: NUMERIC / total_young AS old_to_young_ratio
    FROM
        (SELECT 
            games, 
            
            SUM(CASE
                WHEN age <= 25
                THEN 1
                ELSE 0
                END
            ) AS total_young,
            
            SUM(CASE
                WHEN age >= 45
                THEN 1
                ELSE 0
                END
            ) AS total_old
        FROM datasets.olympics_athletes_events
        GROUP BY games) subquery
    ```

3. Which day of the week would it be best to trade in AAPL stock? What about the best day in a month? Find the average opening and closing prices. (Hint: You can use EXTRACT(‘dow’ FROM date)  to get the day of week)

    `Tables: aapl_historical_stock_price`

    *Solution*:

    ```sql
    SELECT
        EXTRACT("dow" FROM date) AS day_of_week,
        AVG(open) AS avg_open,
        AVG(close) AS avg_close
    FROM datasets.aapl_historical_stock_price
    GROUP BY day_of_week
    ORDER BY day_of_week
    ```
    
    ```sql
    SELECT
        EXTRACT("day" FROM date) AS day_of_month,
        AVG(open) AS avg_open,
        AVG(close) AS avg_close
    FROM datasets.aapl_historical_stock_price
    GROUP BY day_of_month
    ORDER BY day_of_month
    ```

4. Find all hotels in the Netherlands where guests complain about dirty rooms. (Hint: Search for the word dirty in negative_review)

    `Tables: hotel_reviews`

    *Solution:* If STRPOS returns 0 it means it did not find the word in the string so we check for the opposite case.

    ```sql
    SELECT
        *
    FROM datasets.hotel_reviews
    WHERE 
        RIGHT(hotel_address, 11) = 'Netherlands' AND 
        STRPOS(negative_review, 'dirty') <> 0
    ```

5.  Using a self join on the titanic dataset find the average absolute fare difference between age groups which are in same pclass and which are comprised of non-survivors. Assume that two passengers are in the same age group if their ages are less than 5 years apart.

    `Tables: titanic`

    *Solution*: Like in the guide we first check to see that we do not compare a passenger againts itself. We also check that they have equal pclass are in the same age group and they did not survive. After all this we perform a group over titanic1.name and calculate the statistic we are interested in.
    
    ```sql
    SELECT
        titanic1.name AS name1,
        AVG(ABS(titanic1.fare - titanic2.fare)) AS avg_fare
    FROM 
        datasets.titanic titanic1,
        datasets.titanic titanic2
    WHERE
        titanic1.passengerid <> titanic2.passengerid AND
        titanic1.pclass = titanic2.pclass AND
        ABS(titanic1.age - titanic2.age) <= 5 AND
        titanic1.survived = 0 AND 
        titanic2.survived = 0
    GROUP BY name1
    ```

6. Fix the review_date in the hotel_reviews dataset to be of proper YYYY-MM-DD format. (Hint: You should be versatile and liberal in your slicing of the date format, type conversion and whatever it takes)

    `Tables: hotel_reviews`

    *Solution*: 

    Run the query first to see the output.

    The reason for using a subquery has to do with the fact that you can't reference previous values in the SELECT part, that is one can't build nice_review_date using year, month and day columns but only by copying the code that builds and that would look really ugly so a subquery to the rescue.

    We use only STRPOS, SUBSTR and RIGHT. 

    To see what is going on let's focus on a single example: '8/3/17' which is 2017-08-03 when properly formatted.
    - STRPOS('8/3/17', '/') is 2
    - SUBSTR('8/3/17', 0, 2) is '8'
    - That is how we get the month.
    - To get the year we always take the last 2 characters.
    - Extracting the day is the hardest part.
    - SUBSTR(review_date, 0, LENGTH(review_date) - 2) gives us '8/3'
    - STRPOS(SUBSTR(review_date, 0, LENGTH(review_date) - 2), '/') + 1) gives us 2
    - So the day is substring of '8/3' starting from position 2 and till the end of string.

    In the outer query we just concatenate the values with the year being shifted to 2000s and cast to date.

    ```sql
    SELECT
        review_date,
        ((2000 + year :: INTEGER) || '-' || month || '-' || day) :: DATE AS nice_review_date
    FROM
    (SELECT
        review_date,
        
        SUBSTR(review_date, 0, STRPOS(review_date, '/')) AS month,
        
        SUBSTR(
            SUBSTR(review_date, 0, LENGTH(review_date) - 2),
            STRPOS(SUBSTR(review_date, 0, LENGTH(review_date) - 2), '/') + 1) AS day,
        
        RIGHT(review_date, 2) AS year
    FROM datasets.hotel_reviews
    ) sub
    ``` 

7. Find the average rating of each movie star along with their name and birthday. (Hint: Use inner queries)

    `Tables: nominee_filmography, nominee_information`

    *Solution*:

    ```sql
    SELECT
        info.birthday,
        info.name,
        film.avg_rating
    FROM
        (
        SELECT
            name,
            AVG(rating) AS avg_rating
        FROM
            datasets.nominee_filmography 
        GROUP BY name
        ) film
    INNER JOIN 
        datasets.nominee_information info
    ON
        info.name = film.name
    ORDER BY birthday ASC
    ```

8. Enhance the query from question 7 to calculate the difference in average rating along the years. You can ignore the name column. (Hint: LAG function)

    `Tables: nominee_filmography, nominee_information`

    *Solution*: The query called `sub` is what we had in question 7, now we just added the standard code to calculate differences.

    ```sql
    SELECT
        birthday,
        avg_rating,
        LAG(avg_rating, 1) OVER (ORDER BY birthday) AS prev_avg_rating,
        avg_rating - LAG(avg_rating, 1) OVER (ORDER BY birthday) AS difference
    FROM
    (SELECT
        info.birthday,
        film.avg_rating
    FROM
        (
        SELECT
            name,
            AVG(rating) AS avg_rating
        FROM
            datasets.nominee_filmography 
        GROUP BY name
        ) film
    INNER JOIN 
        datasets.nominee_information info
    ON
    info.name = film.name
    ORDER BY birthday ASC) sub
    ```

9. Find the most expensive products on Amazon for each product category. Use the price column (Hint: Use subquery joins)

    `Tables: innerwear_amazon_com`

    *Solution*:

    ```sql
    SELECT 
         product_category,
         MAX(SUBSTR(price, 2) :: NUMERIC) AS max_price
     FROM datasets.innerwear_amazon_com
     GROUP BY product_category
    ```

    We have to clean the price by removing the $ and casting to NUMERIC so MAX will work.

    Now we use this query as an inner query for the more complex one.

    ```sql
    SELECT
        iwa.*
    FROM
     datasets.innerwear_amazon_com iwa
    INNER JOIN
     (SELECT 
         product_category,
         MAX(SUBSTR(price, 2) :: NUMERIC) AS max_price
     FROM datasets.innerwear_amazon_com
     GROUP BY product_category) mp
    ON iwa.product_category = mp.product_category AND 
        SUBSTR(iwa.price, 2) :: NUMERIC = mp.max_price
    ```

10. Find the products which are exclusive to Amazon compared to Top Shop and Macy's. Two products are considered equal if they have the same brand name and same price.

    `Tables: innerwear_amazon_com, innerwear_topshop_com, innerwear_macys_com`
    
    *Solution:*

    From bottom up we start by merging Macy's and Top Shop data using UNION ALL. We select only the columns we want and that is brand_name and price. We must use DISTINCT. The results of all that are an input to the IN clause for filtering the amazon data.

    Notice that brand_name and price have a different format in different datasets so you might need to do some string magic to obtain completely accurate results.

    ```sql
    SELECT 
        *
    FROM 
        datasets.innerwear_amazon_com
    WHERE 
        (brand_name, price) NOT IN 
            (SELECT DISTINCT
                brand_name,
                price
            FROM 
                (
                    SELECT * FROM datasets.innerwear_macys_com
                        UNION ALL
                    SELECT * FROM datasets.innerwear_topshop_com
                ) mha
            )
    ```

11. Find all users which are located in Italy but do not speak Italian. You must also filter away all non control group entries and the device they used must be a Nexus 5.

    `Tables: playbook_experiments, playbook_users`

    *Solution*:

    ```sql
    SELECT 
        usr.user_id,
        usr.language,
        exp.location
    FROM 
        datasets.playbook_experiments exp
        INNER JOIN
        datasets.playbook_users usr
        ON 
        exp.user_id = usr.user_id
    WHERE 
        exp.device = 'nexus 5' AND 
        exp.experiment_group = 'control_group' AND
        exp.location = 'Italy' AND
        usr.language <> 'italian'
    ORDER BY exp.occurred_at ASC
    ```

12. Who is the student who has the median writing score? What is the 80th percentile of hours studied? 

    `Tables: sat_scores`

    *Solution*:

    In the inner query we calculate percentile over sat_writing score for each student.
    In the outer query we take only the student which is at the median (50th percentile)

    ```sql
    SELECT 
        *
    FROM
    (SELECT
        *,
        NTILE(100) OVER (ORDER BY sat_writing) AS writing_percentile
    FROM datasets.sat_scores) sub
    WHERE writing_percentile = 50
    ```

    The second query is same logic as here except that we take the 80th percentile over hours studied.

    ```sql
    SELECT 
        *
    FROM
    (SELECT
        *,
        NTILE(100) OVER (ORDER BY hrs_studied) AS study_percentile
    FROM datasets.sat_scores) sub
    WHERE study_percentile = 80
    ```

13. Find Yelp reviews about food. Search for keywords like food, pizza, sandwich or burger. Find the business address and the state.

    `Tables: yelp_reviews, yelp_business`

    *Solution*:
    - Using STRPOS we check the reviews 
    - We need to BTRIM because in yelp_business names are "Taco Bell" while they are Taco Bell in yelp_reviews.

    ```sql
    SELECT DISTINCT
        business_name,
        address,
        state
    FROM
        (SELECT
            business_name
        FROM datasets.yelp_reviews
        WHERE
            STRPOS(review_text, 'food') <> 0 OR 
            STRPOS(review_text, 'sandwich') <> 0 OR
            STRPOS(review_text, 'pizza') <> 0 OR
            STRPOS(review_text, 'burger') <> 0) reviews
        JOIN 
            datasets.yelp_business bus
        ON
            BTRIM(bus.name, '"') = reviews.business_name
    ```

14. Find the date when Apple's opening stock price reached maximum.

    `Tables: aapl_historical_stock_price`

    *Solution*:

    ```sql
    SELECT 
        date
    FROM datasets.aapl_historical_stock_price
    WHERE open = 
        (SELECT 
            MAX(open) 
        FROM 
            datasets.aapl_historical_stock_price)
    ```

15. What is the average number of days between booking and check-in dates for AirBnB hosts?

    `Tables: airbnb_contacts`

    *Solution*:

    To find the number of days we take the difference between `ds_checkin` and `ts_booking_at`. Because `ts_booking_at` is of type TIMESTAMP we cast it to a date so get a substraction of two dates. The average over all such days is taken per host to get the results we need.
    
    Because of errors in the table where the check-in date comes before the booking date we need to use a HAVING clause to deal with them.
    
    For better presentability we also order by the average we just calculated.

    ```sql
    SELECT
        id_host,
        AVG(ds_checkin - ts_booking_at :: DATE) AS days_between_booking_and_checkin_avg
    FROM datasets.airbnb_contacts
    GROUP BY id_host
    HAVING AVG(ds_checkin - ts_booking_at :: DATE) >= 0
    ORDER BY days_between_booking_and_checkin_avg DESC
    ```
