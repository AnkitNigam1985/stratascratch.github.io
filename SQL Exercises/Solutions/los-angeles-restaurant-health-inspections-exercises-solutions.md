# SQL Exercise 6

The dataset for this exercise is `datasets.los_angeles_restaurant_health_inspections`

This dataset lists all restaurant health inspections which happened in the city of Los Angeles. As is often the case in life the columns of this dataset are completely different compared to the similar data which we have for San Francisco. Should we want to compare these two cities we would need to do a lot of preprocessing before we can join these two tables. This happens very often, but luckily in this exercise we will stick only to this dataset. This dataset lists inspections with date and place information. We also know the score and grade each facility got, the corporation which owns the facility, the id of the employee who was working when the inspection happened. Additionally we know the inspection results in form of textual description.

The technical description of the dataset is in the following table.

| Column Name | Column Type | Has NULL or missing values | Short description |
|---|---|---|---|
|*serial_number*|VARCHAR|NO|The id of the inspection.|
|*activity_date*|VARCHAR|NO|The date when the inspection occurred|
|*facility_name*|VARCHAR|NO|The name of the facility in which the inspection took place.|
|*score*|INTEGER|NO|The score assigned to the facility.
|*grade*|VARCHAR|**YES**|A symbolic grade assigned to the facility. One of 'A', 'B', 'C' or ' '.
|*service_code*|INTEGER|NO|One of 1, 401.
|*service_description*|VARCHAR|NO|One of ROUTINE INSPECTION, OWNER INITIATED ROUTINE INSPECT.
|*employee_id*|VARCHAR|NO|The id of the employee which was working when the inspection happened.
|*facility_address*|VARCHAR|NO|The street address of the facility.
|*facility_city*|VARCHAR|NO|Always LOS ANGELES.
|*facility_id*|VARCHAR|NO|The id of the facility.
|*facility_state*|VARCHAR|NO|Always CA.
|*facility_zip*|VARCHAR|NO|The zip code of the facility.
|*owner_id*|VARCHAR|NO|The id of the owner.
|*owner_name*|VARCHAR|NO|The full name of the owner. Information like INC, LLC is included here.
|*pe_description*|VARCHAR|NO|The textual description of the issue found with the restaurant.
|*program_element_pe*|INTEGER|NO|The code for the issue.
|*program_name*|VARCHAR|NO|Usually same as *facility_name*
|*program_status*|VARCHAR|NO|One of ACTIVE, INACTIVE.
|*record_id*|VARCHAR|NO|Unknown.

To make life easier, always do a select for all the columns needed in the question, look at the returned values, ponder a bit and then try to write the solution.

## Beginner exercise

1. Find the activity date and the issue description for 'STREET CHURROS' when they had less than 95 points.

Columns: `facility_name`, `pe_description`, `activity_date` and `score`

```sql
SELECT
    activity_date,
    pe_description
FROM
    datasets.los_angeles_restaurant_health_inspections
WHERE
    facility_name = 'STREET CHURROS' AND score < 95
```

2. Find all inspections made in the facilities of 'GLASSELL COFFEE SHOP LLC'

Columns: `owner_name`

```sql
SELECT
    *
FROM
    datasets.los_angeles_restaurant_health_inspections
WHERE
    owner_name = 'GLASSELL COFFEE SHOP LLC'
```

3. Find all routine inspections which found an issue with High Risk.

Hint: Use ILIKE.

Columns: `service_description` and `pe_description`

```sql
SELECT
    *
FROM
    datasets.los_angeles_restaurant_health_inspections
WHERE pe_description ILIKE '%HIGH%' AND
      service_description = 'ROUTINE INSPECTION'
```

4. Find all facilities which have a zip code in 90049, 90034, 90035.

```sql
SELECT
    DISTINCT facility_name
FROM
    datasets.los_angeles_restaurant_health_inspections
WHERE facility_zip IN ('90049', '900034', '90045')
```

5. Find all inspections which are part of an inactive program.

```sql
SELECT
    *
FROM
    datasets.los_angeles_restaurant_health_inspections
WHERE program_status = 'INACTIVE'
```

## Intermediate exercise

1. Using UNION find the average score for grades A, B and C. Do not use GROUP BY.

Hint: You can do things like `'A' AS grade` in the `SELECT` part. This can be used to disambiguate after the union occurs.

```sql
SELECT
    'A' AS grade,
    AVG(score) AS avg_score
FROM
    datasets.los_angeles_restaurant_health_inspections
WHERE grade = 'A'

UNION

SELECT
    'B' AS grade,
    AVG(score) AS avg_score
FROM
    datasets.los_angeles_restaurant_health_inspections
WHERE grade = 'B'

UNION

SELECT
    'C' AS grade,
    AVG(score) AS avg_score
FROM
    datasets.los_angeles_restaurant_health_inspections
WHERE grade = 'C'
```

2. Find all corporations which have only a single facility.

Hint: Use HAVING.

```sql
SELECT
    owner_name
FROM    
    datasets.los_angeles_restaurant_health_inspections
GROUP BY owner_name
HAVING COUNT(DISTINCT facility_name) = 1
```

3. Find the average number of inspections per facility for all corporations. Present the results in a descending order by that average.

```sql
SELECT
    owner_name,
    COUNT(*) AS n_inspections,
    COUNT(DISTINCT facility_name) AS n_facilities,
    COUNT(*) / COUNT(DISTINCT facility_name) AS avg_inspections_per_facility
FROM    
    datasets.los_angeles_restaurant_health_inspections
GROUP BY owner_name
ORDER BY avg_inspections_per_facility DESC
```

4. Check if `record_id` is unique for every row.

Hint: It is unique if count = count distinct

```sql
SELECT
    COUNT(record_id) AS c1,
    COUNT(DISTINCT record_id) AS c2
FROM
    datasets.los_angeles_restaurant_health_inspections
```

5. Count the number of low risk issues for bakeries.

Hint: To get correct results use the SUM - CASE technique.

```sql
SELECT
    owner_name,
    SUM(
        CASE
            WHEN pe_description ILIKE '%LOW%'
            THEN 1
            ELSE 0
        END
    ) AS no_low_risk_issues
FROM    
    datasets.los_angeles_restaurant_health_inspections
WHERE owner_name ILIKE '%BAKERY%'
GROUP BY owner_name
```

6. Find the minimal score for all facilities on Hollywood boulevard. What is the facility with the highest minimum score?

```sql
SELECT
    facility_name,
    MIN(score)
FROM    
    datasets.los_angeles_restaurant_health_inspections
WHERE facility_address ILIKE '%HOLLYWOOD BLVD%'
GROUP BY facility_name
ORDER BY min DESC
```

7. Classify each owner as LLC, INC or other.

```sql
SELECT
    DISTINCT ON (owner_name)
    
    owner_name,
    
    CASE
        WHEN owner_name ILIKE '%LLC%' OR owner_name ILIKE '%LL%' THEN 'LLC'
        WHEN owner_name ILIKE '%INC%' THEN 'INC'
        ELSE 'OTHER'
    END AS classification
FROM    
    datasets.los_angeles_restaurant_health_inspections
```

8. Find the rules used to determine the grades. Have the rule as a separate column with a textual description like 'Score > X AND Score < Y => Grade = A' where X and Y are the lower and upper bounds for a grade.

Hint: Find the minimum and maximum scores for each grade. Use || to form the rule string.

```sql
SELECT
    grade,
    MIN(score),
    MAX(score),
    
    'Score > ' || MIN(score) || ' AND Score < ' || MAX(score) || ' => Grade = ' || grade AS rule 
FROM
    datasets.los_angeles_restaurant_health_inspections
GROUP BY grade
ORDER BY grade
```

9. Find all facilities which offer beverages. Assume they offer beverages if their name contains the words tea, coffee or juice. What is the most common issue for this type of venues?

```sql
SELECT
    pe_description,
    COUNT(*) AS cnt
FROM
    datasets.los_angeles_restaurant_health_inspections
WHERE
    facility_name ILIKE '%TEA%' OR facility_name ILIKE '%CAFE%' OR facility_name ILIKE '%JUICE%'
GROUP BY pe_description
ORDER BY cnt DESC
LIMIT 1
```

10. Count the number of facilities per municipality along with the number of inspections. Does this data confirm the hypothesis that more facilities = higher number of inspections. Make a scatter plot of this data.

```sql
SELECT
    COUNT(DISTINCT facility_name) AS no_facilities,
    COUNT(*) AS no_inspections
FROM
    datasets.los_angeles_restaurant_health_inspections
GROUP BY facility_zip
ORDER BY no_inspections DESC
```

11. Find all bakeries and the most common grade they earned as a collective.

```sql
SELECT
    grade
FROM
    datasets.los_angeles_restaurant_health_inspections
WHERE facility_name ILIKE '%BAKERY%'
GROUP BY grade
ORDER BY COUNT(*) DESC
LIMIT 1
```

12. Find the number of inspections per day ordered by the date?

```sql
SELECT
    activity_date :: DATE,
    COUNT(*)
FROM
    datasets.los_angeles_restaurant_health_inspections
GROUP BY activity_date :: DATE
ORDER BY activity_date :: DATE
```

13. How many inspections of low risk happened in 2017?

```sql
SELECT
    COUNT(*)
FROM 
    datasets.los_angeles_restaurant_health_inspections
WHERE
    EXTRACT(YEAR FROM activity_date :: DATE) = 2017 AND
    pe_description ILIKE '%LOW%'
```

14. Which month had the lowest number of inspections for fish markets over all years?

```sql
SELECT
    EXTRACT(MONTH FROM activity_date :: DATE) AS month,
    COUNT(*)
FROM 
    datasets.los_angeles_restaurant_health_inspections
WHERE
    facility_name ILIKE '%FISH%'
GROUP BY month
ORDER BY count DESC
LIMIT 1
```

15. Under assumption that the scores are normally distributed, the mean per groups should be 95 for A. Find the actual mean for these scores using BETWEEN and verify this claim.

```sql
SELECT
    AVG(score)
FROM datasets.los_angeles_restaurant_health_inspections
WHERE score BETWEEN 91 AND 100
```

## Advanced exercise

1. What is the variance of scores which have grade A? The formula is avg((X_i - mean_x) ^ 2). What does this tell you about the normality assumption of scores for grade A?

Hint: You can use an implicit join to solve this or a left join with a always TRUE condition in the ON clause.

```sql
SELECT
    AVG(x) AS variance,
    SQRT(AVG(X)) AS std
FROM
    (SELECT
        main.score,
        avgs.mean,
        (main.score - avgs.mean) * (main.score - avgs.mean) AS x
    FROM
        (SELECT
            AVG(score) AS mean
        FROM datasets.los_angeles_restaurant_health_inspections
        WHERE score BETWEEN 90 AND 100) avgs,
        datasets.los_angeles_restaurant_health_inspections main) tmp
```

2. What is the most popular street/boulevard/road based on the number of low, medium and high risk inspections. 

Hint: To find the street/boulevard/road name use split_part. Sometimes the name is second or third word. Use UNION to combine all 3 queries into one result. You might have to use subqueries.

```sql
SELECT
    street_name,
    popularity,
    'Low Risk' AS risk_category 
FROM
    (SELECT
        CASE 
            WHEN LENGTH(split_part(facility_address, ' ', 2)) >= 3 THEN split_part(facility_address, ' ', 2)
            WHEN LENGTH(split_part(facility_address, ' ', 3)) >= 3 THEN split_part(facility_address, ' ', 3)
            ELSE split_part(facility_address, ' ', 4)
        END AS street_name,
        
        COUNT(*) AS popularity
    FROM datasets.los_angeles_restaurant_health_inspections
    WHERE pe_description ILIKE '%LOW%'
    GROUP BY street_name
    ORDER BY popularity DESC
    LIMIT 1) tmp1

UNION

SELECT
    street_name,
    popularity,
    'Moderate Risk' AS risk_category 
FROM
    (SELECT
        CASE 
            WHEN LENGTH(split_part(facility_address, ' ', 2)) >= 3 THEN split_part(facility_address, ' ', 2)
            WHEN LENGTH(split_part(facility_address, ' ', 3)) >= 3 THEN split_part(facility_address, ' ', 3)
            ELSE split_part(facility_address, ' ', 4)
        END AS street_name,
        
        COUNT(*) AS popularity
    FROM datasets.los_angeles_restaurant_health_inspections
    WHERE pe_description ILIKE '%MODERATE%'
    GROUP BY street_name
    ORDER BY popularity DESC
    LIMIT 1) tmp2

UNION

SELECT
    street_name,
    popularity,
    'High Risk' AS risk_category 
FROM
    (SELECT
        CASE 
            WHEN LENGTH(split_part(facility_address, ' ', 2)) >= 3 THEN split_part(facility_address, ' ', 2)
            WHEN LENGTH(split_part(facility_address, ' ', 3)) >= 3 THEN split_part(facility_address, ' ', 3)
            ELSE split_part(facility_address, ' ', 4)
        END AS street_name,
        
        COUNT(*) AS popularity
    FROM datasets.los_angeles_restaurant_health_inspections
    WHERE pe_description ILIKE '%HIGH%'
    GROUP BY street_name
    ORDER BY popularity DESC
    LIMIT 1) tmp3
```

3. Find all owners who have at least one facility with grade A, grade B and grade C.

Hint: Even though it may not seem like so, this is easily solvable using pivot table techniques. The trick is to think of pivot table columns as variables which can be used in filters and aggregations.

```sql
SELECT
    owner_name
FROM
    (SELECT
        owner_name,
        
        -- Type juggling so we can use MAX over booleans
        COALESCE(MAX(has_A :: INTEGER) :: BOOL, FALSE) AS has_A,
        COALESCE(MAX(has_B :: INTEGER) :: BOOL, FALSE) AS has_B,
        COALESCE(MAX(has_C :: INTEGER) :: BOOL, FALSE) AS has_C
    FROM
        (SELECT
            owner_name,
            
            CASE
                WHEN grade = 'A' THEN TRUE ELSE NULL
            END AS has_A,
            
            CASE
                WHEN grade = 'B' THEN TRUE ELSE NULL
            END AS has_B,
            
            CASE
                WHEN grade = 'C' THEN TRUE ELSE NULL
            END AS has_C
        FROM datasets.los_angeles_restaurant_health_inspections
        GROUP BY owner_name, grade
        HAVING(COUNT(*) >= 1)) tmp
    GROUP BY owner_name) tmp2
WHERE has_A = TRUE AND has_B = TRUE AND has_C = TRUE
```

4. Which facility got most inspections in 2017 compared to other years?

Hint: This is very similar to the preceding question.

```sql
SELECT
    facility_name
FROM
    (SELECT
        facility_name,
        
        MAX(cnt_15) AS cnt_15,
        MAX(cnt_16) AS cnt_16,
        MAX(cnt_17) AS cnt_17,
        MAX(cnt_18) AS cnt_18
    FROM
        (SELECT
            facility_name,
            
            CASE WHEN year = 2015 THEN no_inspections ELSE 0 END AS cnt_15,
            CASE WHEN year = 2016 THEN no_inspections ELSE 0 END AS cnt_16,
            CASE WHEN year = 2017 THEN no_inspections ELSE 0 END AS cnt_17,
            CASE WHEN year = 2018 THEN no_inspections ELSE 0 END AS cnt_18
        FROM
            (SELECT
                facility_name,
                EXTRACT(YEAR FROM activity_date :: DATE) AS year,
                COUNT(*) AS no_inspections
            FROM
                datasets.los_angeles_restaurant_health_inspections
            GROUP BY facility_name, year) tmp) tmp2
    GROUP BY facility_name) tmp3
WHERE cnt_17 > cnt_15 AND cnt_17 > cnt_16 AND cnt_17 > cnt_18
```

5. When was the first and last time the maximum score was awarded?

```sql
SELECT
    MIN(activity_date) AS first_time,
    MAX(activity_date) AS last_time
FROM
    (SELECT
        DISTINCT activity_date :: DATE
    FROM 
        datasets.los_angeles_restaurant_health_inspections
    WHERE score = (
        SELECT 
            MAX(score)
        FROM 
            datasets.los_angeles_restaurant_health_inspections
    )) tmp
```

6. Find the 4 quartiles of score for each company as a pivot table. Have the rows of this table ordered in ascending order by the average value of the 4 quartiles.

Hint: Use NTILE and pivot table techniques.

```sql
SELECT
    *
FROM
    (SELECT
        owner_name,
        
        MAX(q1) AS q1,
        MAX(q2) AS q2,
        MAX(q3) AS q3,
        MAX(q4) AS q4
    FROM
        (SELECT
            owner_name,
            
            CASE 
                WHEN quartile = 1
                THEN score
                ELSE NULL
            END AS q1,
            
            CASE 
                WHEN quartile = 2
                THEN score
                ELSE NULL
            END AS q2,
            
            CASE 
                WHEN quartile = 3
                THEN score
                ELSE NULL
            END AS q3,
            
            CASE 
                WHEN quartile = 4
                THEN score
                ELSE NULL
            END AS q4
        FROM
            (SELECT
                owner_name,
                score,
                NTILE(4) 
                OVER (PARTITION BY owner_name ORDER BY score) AS quartile
            FROM
                datasets.los_angeles_restaurant_health_inspections) tmp) tmp2
    GROUP BY owner_name) tmp3
ORDER BY 0.25 * (q1 + q2 + q3 + q4) ASC
```

7. Find the dates when the most sanitary restaurants got their last inspection. Assume highest number of points is most sanitary. Keep in mind that facility must have the word restaurant in its name. What is the number of days between these inspections?

Hint: Use LAG to answer the last demand.

```sql
SELECT
    facility_name,
    
    score,
    
    activity_date,
    
    LAG(activity_date, 1) OVER(ORDER BY activity_date) AS prev_activity_date,
    
    activity_date - LAG(activity_date, 1) OVER(ORDER BY activity_date) AS number_of_days_between_high_scoring_inspections
FROM
    -- Answer for first two demands
    (SELECT
        facility_name,
        activity_date :: DATE,
        score
    FROM
        datasets.los_angeles_restaurant_health_inspections
    WHERE
        score = 
            (SELECT 
                MAX(score)
            FROM 
                datasets.los_angeles_restaurant_health_inspections
            WHERE facility_name ILIKE '%RESTAURANT%')
        
    ORDER BY activity_date :: DATE ASC) tmp1
```

8. Which street is the trendiest for each year? Assume trendiest as the one with highest total score. Total score is sum of all scores.

Hint: Use ROW_NUMBER()

```sql
SELECT
    year,
    
    street_name,
    
    total_score
FROM
    (SELECT
        street_name,
        year,
        total_score,
        
        ROW_NUMBER()
        OVER (PARTITION BY year ORDER BY total_score DESC) AS position
    FROM
        (SELECT
            CASE 
                WHEN LENGTH(split_part(facility_address, ' ', 2)) >= 3 THEN split_part(facility_address, ' ', 2)
                WHEN LENGTH(split_part(facility_address, ' ', 3)) >= 3 THEN split_part(facility_address, ' ', 3)
                ELSE split_part(facility_address, ' ', 4)
            END AS street_name,
            
            EXTRACT(YEAR FROM activity_date :: DATE) AS year,
            
            SUM(score) AS total_score
        FROM 
            datasets.los_angeles_restaurant_health_inspections
        GROUP BY street_name, year) tmp) tmp2
WHERE position = 1
```

9. For each owner find the top 3 facilities. In the output have the address and the average score they earned. Decide on the top 3 by that average score. When there are less than 3 facilities display 'No Data'.

```sql
SELECT
    owner_name,
    
    MAX(facility_1) AS facility_1,
    
    COALESCE(MAX(facility_2), 'No Data') AS facility_2,
    
    COALESCE(MAX(facility_3), 'No Data') AS facility_3
FROM
    (SELECT
        owner_name,
        
        CASE 
            WHEN position = 1 
            THEN facility_address
            ELSE NULL
        END AS facility_1,
        
        CASE 
            WHEN position = 2
            THEN facility_address
            ELSE NULL
        END AS facility_2,
        
        CASE 
            WHEN position = 13
            THEN facility_address
            ELSE NULL
        END AS facility_3
    FROM
        (SELECT
            owner_name,
            
            facility_address,
            
            avg_score,
            
            ROW_NUMBER()
            OVER (PARTITION BY owner_name ORDER BY avg_score DESC) AS position
        FROM
            (SELECT
                owner_name,
                facility_address,
                AVG(score) AS avg_score
            FROM
                datasets.los_angeles_restaurant_health_inspections
            GROUP BY owner_name, facility_address) aliazed) aliazed2
    WHERE position IN (1, 2, 3)) aliazed3
GROUP BY owner_name
```