# SQL Exercise 3

The datasets for this exercise are `datasets.winemag_p1` and `datasets.winemag_p2`.

The first dataset is about the wines reviewed by the WineMag. We know the geographical origin of each wine like the country, the province and 2 regions from which it originated. We also have descriptions for each wine where we can learn about its aromas. The wine's variety, designation and winery complete the textual information. There are only 2 numeric columns in this dataset and they are the wine's price and the number of points assigned to it by the reviewer.

The technical description of the dataset is in the following table.

| Column Name | Column Type | Has NULL or missing values | Short description |
|---|---|---|---|
|*id*|INTEGER|NO|The primary key for this table.|
|*country*|VARCHAR|**YES**|The country of origin of the wine.|
|*description*|VARCHAR|NO|Description of the wine with information about it's aromas and similar.
|*designation*|VARCHAR|**YES**|The designation of the wine. 
|*points*|INTEGER|NO|How many points are awared to this wine by the wine magazine.
|*price*|DOUBLE PRECISION|**YES**|How much does this wine cost. This value is probably US dollars.
|*province*|VARCHAR|**YES**|The province from which the wine label originates.
|*region_1*|VARCHAR|**YES**|Region where the wine originates.
|*region_2*|VARCHAR|**YES**|Second region from which the wine originates.
|*variety*|VARCHAR|NO|The variety of the wine, e.g. Chardonnay, Malbec
|*winery*|VARCHAR|NO|The winery producing the wine.

The second dataset is very similar to the first one with the following additions:
- Almost everything is of type text and missing values are represented as empty strings, not as NULL. This means some data cleaning is likely needed.
- There are 3 more columns
    * *taster_name* which is the name of the person tasting the name.
    * *taster_twitter_handle* is the twitter user name of the taster.
    * *title* is the name of the review.


To make life easier, always do a select for all the columns needed in the question, look at the returned values, ponder a bit and then try to write the solution.

## Beginner exercise

Unless specified otherwise only `winemag_p1` is used in this section.

1. Find the prices for Spanish, Italian and French wines.

Columns: `country` and `price`

```sql
SELECT
   price 
FROM
    datasets.winemag_p1
WHERE
    country = 'Spain' OR country = 'Italy' OR country = 'France'
```

2. Find all top rated wineries. A winery is considered top rated if it has at least one wine which got awarded 95 or more points.

Columns: `winery` and `points`

```sql
SELECT
   DISTINCT winery
FROM
    datasets.winemag_p1
WHERE
    points >= 95
```

3. Find all wine varieties which can be considered cheap. A variety is considered cheap if the price of a bottle is between 5 and 20 dollars.

Columns: `variety` and `price`

```sql
SELECT
   DISTINCT variety
FROM
    datasets.winemag_p1
WHERE
    price BETWEEN 5 AND 20
```

4. Find all provinces which have at least one wine missing designation and are named same as the country.

Columns: `province`, `designation` and `country`

```sql
SELECT
   DISTINCT province
FROM
    datasets.winemag_p1
WHERE
    designation IS NULL AND 
    province = country
```

5. Using the dataset `winemag_p2` find out which varieties were tasted by 'Roger Voss'. In your answer consider only those tastings which have a designated region.

Columns: `variety`, `taster_name` and `region_1`

```sql
SELECT
   DISTINCT variety
FROM
    datasets.winemag_p2
WHERE
    taster_name = 'Roger Voss' AND
    region_1 <> ''
```

## Intermediate exercises

1. Find the all possible varieties which occur in either of the datasets.

Hint: Use UNION.

Columns: `variety`

```sql
SELECT
   DISTINCT variety
FROM
    datasets.winemag_p1
UNION
SELECT
    DISTINCT variety
FROM 
    datasets.winemag_p2
WHERE variety <> ''
ORDER BY variety
```

2. Find all wines which have an aroma of plum, cherry, rose or hazelnut. Use the first dataset only.

Columns: `description`

```sql
SELECT
    *
FROM datasets.winemag_p1
WHERE 
    description ILIKE '%plum%' OR 
    description ILIKE '%cherry%' OR
    description ILIKE '%rose%'OR
    description ILIKE '%hazelnut%'
```

3. Find how many US based wineries have expensive wines (price >= 200). Use only the first dataset.

Columns: `winery`, `country` and `price`

```sql
SELECT
   COUNT(DISTINCT winery)
FROM
    datasets.winemag_p1
WHERE
    country = 'US' AND price >= 200
```

4. Find how many wines of each variety were tasted by each taster.

Hint: Use a group by.

Columns: `taster_name` and `variety`

```sql
SELECT
   taster_name, 
   variety,
   COUNT(*) AS cnt
FROM
    datasets.winemag_p2
WHERE taster_name <> ''
GROUP BY taster_name, variety
ORDER BY taster_name, variety, cnt DESC
```

5. Find the minimum, average and maximum price of all wines. Use both datasets.

Hint: Cleanup `winemag_p2` before union-ing it with the first one `winemag_p1`. Type casting is ok to use. Using a subquery is ok.

Columns: `country` and `price`

```sql
SELECT
    country,
    MIN(price),
    AVG(price),
    MAX(price)
FROM 
    (SELECT country, price FROM datasets.winemag_p1 WHERE country IS NOT NULL
     UNION
     SELECT country, 
            (CASE 
                WHEN price = '' THEN NULL
                ELSE price :: DOUBLE PRECISION 
                END) AS price FROM datasets.winemag_p2
     WHERE country <> ''
    ) tmp
GROUP BY country
```

6. Find the total possible profit of each winery by variety with the condition that each wine produced in the variety must have at least 90 points. The profit is defined as the sum of all tasted wine prices.

Columns: `winery`, `variety`, `price` and `points`

Hint: Use a HAVING clause.

```sql
SELECT
    winery,
    variety,
    COALESCE(SUM(price), 0) AS total_profit
FROM datasets.winemag_p1
GROUP BY winery, variety
HAVING MIN(points) > 90
ORDER BY winery, total_profit DESC
```

7. Find all Bodegas outside Spain which produce wines which have taste of blackberries. Now using this data, find the total number of unique wineries per country and region.

Columns: `country`, `region_1`, `winery`, and `description`

```sql
SELECT
    country,
    region_1,
    COUNT(DISTINCT winery) AS cnt
FROM datasets.winemag_p1
WHERE 
    winery ILIKE '%bodega%' AND 
    country <> 'Spain' AND 
    description ILIKE '%blackberry%'
GROUP BY country, region_1
ORDER BY cnt DESC
```

8. Order the list of wines by their points to price ration. Use the dataset `winemag_p2`. Which is the wine for which you get the best bang for your buck?

Hint: Use LIMIT.

Columns: `price` and `points`

```sql
SELECT
    *,
    (points :: NUMERIC) / (price :: NUMERIC) AS ptpr
FROM 
    datasets.winemag_p2
WHERE 
    price  <> '' AND
    points <> ''
ORDER BY ptpr DESC
LIMIT 1
```

9. Find the profit each region made for each variety.

Columns: `region_1`, `variety` and `price`

```sql
SELECT
    region_1,
    variety,
    SUM(price) AS total_profit
FROM 
    datasets.winemag_p1
GROUP BY region_1, variety
```

10. Rank US provinces by the number of wines which contain the keyword
balanced in their keyword. Use only the first dataset (`winemag_p1`).

Hint: Use the SUM - CASE 0/1 trick.

Columns: `country`, `description` and `province`

```sql
SELECT
    province,
    SUM(
        CASE 
            WHEN description ILIKE '%balanced%'
            THEN 1
            ELSE 0
        END
    ) AS total_balanced_wines
FROM
    datasets.winemag_p1
WHERE
    country = 'US'
GROUP BY province
ORDER BY total_balanced_wines DESC
```

11. How many wines have designations? How many don't? What's the grand total? Make a report by country.

Columns: `country` and `designations`

```sql
SELECT
    country,
    
    SUM(
        CASE
            WHEN designation = '' THEN 1 ELSE 0
        END
    ) AS total_without_designation,
    
    COUNT(*) - SUM(
        CASE
            WHEN designation = '' THEN 1 ELSE 0
        END
    ) AS total_with_designation,
    
    COUNT(*) AS grand_total
FROM
    datasets.winemag_p2
GROUP BY country
```

12. Which countries are present in `winemag_p1` but not in `winemag_p2`?

Columns: `country`

Hint: Use EXCEPT

```sql
SELECT
    DISTINCT country
FROM
    datasets.winemag_p1
WHERE country IS NOT NULL
EXCEPT
SELECT
    DISTINCT country
FROM
    datasets.winemag_p2
WHERE country <> ''
```

## Advanced exercises

1. Find each taster's favorite wine variety. Favorite means that they tasted wines of that variety the most.

Columns: `taster_name` and `variety`

Hint: Don't be afraid to use multiple subqueries and joins to solve this question. One of the subqeries will occur twice in the solution.

```sql
SELECT
    t3.taster_name,
    t3.variety,
    t3.cnt
FROM
(SELECT
    taster_name,
    MAX(cnt) AS max_cnt
FROM
    (SELECT
       taster_name, 
       variety,
       COUNT(*) AS cnt
    FROM
        datasets.winemag_p2
    WHERE taster_name <> ''
    GROUP BY taster_name, variety) t1
GROUP BY taster_name) t2
INNER JOIN
    (SELECT
       taster_name, 
       variety,
       COUNT(*) AS cnt
    FROM
        datasets.winemag_p2
    WHERE taster_name <> ''
    GROUP BY taster_name, variety) t3
ON t2.taster_name = t3.taster_name AND
   t3.cnt         = t2.max_cnt
```

2. Find all provinces which produced more wines in `winemag_p1` than they did in `winemag_p2`.

Columns: `province`

```sql
SELECT
    tmp1.province,
    tmp1.cnt_1
FROM
    (SELECT
       province,
       COUNT(*) AS cnt_1
    FROM datasets.winemag_p1
    WHERE province IS NOT NULL
    GROUP BY province) tmp1
INNER JOIN
    (SELECT
       province,
       COUNT(*) AS cnt_2
    FROM datasets.winemag_p1
    WHERE province IS NOT NULL
    GROUP BY province) tmp2
ON
    tmp1.province = tmp2.province AND 
    tmp1.cnt_1 >= tmp2.cnt_2
ORDER BY tmp1.cnt_1 DESC
```

3. Find the year for all Macedonian wines from `winemag_p2`

Hint: Use the `split_part` function.

Columns: `title`

```sql
SELECT
    title,
    split_part(title, ' ', 2) :: NUMERIC AS year
FROM
    datasets.winemag_p2
WHERE 
    country = 'Macedonia'
```

4. Find all wines from the second dataset which are produced in the country
which has the highest sum points from the first dataset.

Hint: Use a subquery.

Columns: `country` and `points`

```sql
SELECT
    *
FROM datasets.winemag_p2
WHERE country IN 
    (SELECT
        country
    FROM
        datasets.winemag_p1
    WHERE country IS NOT NULL
    GROUP BY country
    ORDER BY SUM(points) DESC
    LIMIT 1)
```

5. Find the cheapest and the most expensive variety per region.

Hint: Use pivot table techniques.

Columns: `region_1`, `price` and `variety`

```sql
SELECT
    tmp3.region,
    
    MAX(cheapest_variety) AS cheapest_variety,
    MAX(most_expensive_variety) AS most_expensive_variety,
    MAX(lowest_price) AS lowest_price,
    MAX(highest_price) AS highest_price
FROM
    (SELECT 
        region,
        
        CASE 
            WHEN price = lowest_price
            THEN variety 
        END AS cheapest_variety,
        
        CASE 
            WHEN price = highest_price
            THEN variety 
        END AS most_expensive_variety,
        
        lowest_price, 
        highest_price
        
    FROM
        (SELECT
            main.region_1 AS region,
            main.price    AS price,
            tmp.lowest_price,
            tmp.highest_price,
            main.variety
        FROM
            datasets.winemag_p1 main
        INNER JOIN
            (SELECT
                region_1,
                MIN(price) AS lowest_price,
                MAX(price) AS highest_price
            FROM
                datasets.winemag_p1
            WHERE region_1 IS NOT NULL
            GROUP BY region_1) tmp
        ON
            main.region_1 = tmp.region_1 AND
            main.price IS NOT NULL AND
            (
                main.price = tmp.lowest_price OR
                main.price = tmp.highest_price
            )
        ) tmp2
    ) tmp3
GROUP BY tmp3.region
```

6. Find the top 3 wineries for each country by the average points their wines earned. Use only `winemag_p1`. When there is no second best winery have the output be 'No second winery.' Similar for the case when there is no third best.

Output table has the following format:
|country|top_winery|second_winery|third_winery|
|-------|----------|-------------|------------|
Albania | ArbÌÇri (88) | No second winery. | No third winery |
Argentina | LTU (94) | Bodega Rolland (94) | Bodega Maestre (92)
Australia | Standish (97) | Campbells (95) | Hickinbotham (95)

Columns: `country`, `winery` and `points`

Hint: Use pivot table techniques and the ROW_NUMBER() function with partitions.

```sql
SELECT
    country,
    
    MAX(top_winery) AS top_winery,
    
    COALESCE(MAX(second_winery), 'No second winery.') AS second_winery,
    
    COALESCE(MAX(third_winery), 'No third winery') AS third_winery
FROM
(SELECT
    country,
    
    CASE 
        WHEN position = 1
        THEN winery || ' (' || round(avg_points) || ')'
        ELSE NULL
    END AS top_winery,
    
    CASE 
        WHEN position = 2
        THEN winery || ' (' || round(avg_points) || ')'
        ELSE NULL
    END AS second_winery,
    
    CASE 
        WHEN position = 3
        THEN winery || ' (' || round(avg_points) || ')'
        ELSE NULL
    END AS third_winery
FROM
(SELECT
    country,
    winery,
    ROW_NUMBER()
    OVER (PARTITION BY country ORDER BY avg_points DESC) AS position,
    avg_points
FROM
    (SELECT
        country,
        winery,
        AVG(points) AS avg_points
    FROM 
        datasets.winemag_p1
    WHERE country IS NOT NULL
    GROUP BY country, winery
    ORDER BY country ASC, avg_points DESC) tmp1) tmp2
WHERE
    position <= 3) tmp3
GROUP BY country
```

7. Find the median price for each wine variety using the union of both datasets.

Columns: `variety` and `price`

Hint: Use NTILE with partitions.

```sql
SELECT
    variety,
    price AS median_price
FROM
(SELECT
    variety,
    price,
    NTILE (100)
    OVER (PARTITION BY variety ORDER BY price) AS price_percentile
FROM 
(SELECT
    variety,
    price
 FROM 
    datasets.winemag_p1
 WHERE variety IS NOT NULL AND price IS NOT NULL
 UNION
 SELECT
    variety,
    price :: DOUBLE PRECISION
 FROM datasets.winemag_p2
 WHERE variety <> '' AND price <> ''
) all_data) with_percentiles
WHERE price_percentile = 50
```

8. Find all wine designations produced in English speaking regions but not produced in Spanish speaking regions. Use the second dataset. The designations under consideration must be top notch, that is they must have a minimum score of 90 for all wine tastings. For these varities find the maximum price in the US. Use only the first dataset.
English speaking: England, Canada, Australia, US, New Zealand
Spanish speaking: Peru, Spain, Argentina, Chile

Hint: Use multiple subqueries and an inner join.

Columns: `variety`, `country`, `price` and `points`

```sql
SELECT
    table1.variety,
    table2.max_price
FROM
(SELECT
    variety
FROM
    datasets.winemag_p1
WHERE country IN ('England', 'Canada', 'Australia', 'US', 'New Zealand')
GROUP BY variety
HAVING MIN(points) > 90

EXCEPT

SELECT
    variety
FROM
    datasets.winemag_p1
WHERE country IN ('Peru', 'Spain', 'Argentina', 'Chile')
GROUP BY variety
HAVING MIN(points) > 90) table1

INNER JOIN

(SELECT
    variety,
    MAX(price) AS max_price
 FROM
    datasets.winemag_p1
 WHERE 
    country = 'US'
 GROUP BY variety
) table2

ON
    table1.variety = table2.variety
```

9. CHALLENGE: Using only split_part and ILIKE find the year when each wine was made (`title` column). Limit yourself to wines made in 2000 or later. Using this new knowledge find the average points per year. How does the average points score change year over year? With the final result confirm or disprove the hypothesis that older wines are better. We assume more points mean better wines.

```sql
SELECT
    year,
    avg_points,
    COALESCE(LAG(avg_points, 1) OVER (ORDER BY year), 87) AS prev_avg_points,
    avg_points - COALESCE(LAG(avg_points, 1) OVER (ORDER BY year), 87) AS difference
FROM
(SELECT
    year,
    AVG(points) AS avg_points
FROM
(SELECT
    points,
    CASE 
        WHEN yc1 ILIKE '201_' OR yc1 ILIKE '200_' THEN yc1 :: INTEGER
        WHEN yc2 ILIKE '201_' OR yc2 ILIKE '200_' THEN yc2 :: INTEGER
        WHEN yc3 ILIKE '201_' OR yc3 ILIKE '200_' THEN yc3 :: INTEGER
        WHEN yc4 ILIKE '201_' OR yc4 ILIKE '200_' THEN yc4 :: INTEGER
        WHEN yc5 ILIKE '201_' OR yc5 ILIKE '200_' THEN yc5 :: INTEGER
        ELSE NULL
    END AS year
FROM
(SELECT
    title,
    points,
    
    split_part(title, ' ', 2) AS yc1,
    split_part(title, ' ', 3) AS yc2,
    split_part(title, ' ', 4) AS yc3,
    split_part(title, ' ', 5) AS yc4,
    split_part(title, ' ', 6) AS yc5
FROM datasets.winemag_p2
WHERE points IS NOT NULL AND title <> '') tmp1) tmp2
WHERE year IS NOT NULL
GROUP BY year
ORDER BY year ASC) tmp3
```