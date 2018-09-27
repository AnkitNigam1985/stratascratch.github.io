# SQL Exercise 4

The dataset for this exercise is `datasets.sf_restaurant_health_violations`

This dataset is about inspections conducted by SF health inspectors. It deals with businesses operating restaurants. The knowledge we have here is about the business like it's name, address and postal code. We also know the date when the inspection happened, the score they assigned to the restaurant and the type of the inspection. Possible validations of the rules are also noted with a description and a risk category. Thus a single row of this table is a single inspection of some business with a possible violation noted by the inspectors.

The technical description of the dataset is in the following table.

| Column Name | Column Type | Has NULL or missing values | Short description |
|---|---|---|---|
|*business_id*|INTEGER|NO|The ID of the business inspected|
|*business_name*|VARCHAR|NO|The name of the business inspected|
|*business_address*|VARCHAR|NO|The address in format Street Number, Street Name
|*business_city*|VARCHAR|NO|Always is 'San Francisco'.
|*business_state*|VARCHAR|NO|Always is 'CA'.
|*business_postal_code*|VARCHAR|NO|The postal code of the area were the business was located.
|*business_latitude*|DOUBLE|NO|The geograhical latitude.
|*business_longitude*|DOUBLE|NO|The geographical longitude.
|*business_location*|VARCHAR|NO|Useless for this exercise.
|*business_phone_number*|VARCHAR|**YES**|The phone number of the business.
|*inspection_id*|VARCHAR|NO|The id of the inspection in format *businessid*_*inspectiondate*
|*inspection_date*|VARCHAR|NO|Inspection date in the format 2015-09-22T00:00:00. Stored as varchar, cleaning needed before casting to valid DATE.
|*inspection_score*|DOUBLE|**YES**|The score the inspector awarded to the business.
|*inspection_type*|VARCHAR|NO|The type of the inspection e.g. Routine - Unscheduled, Routine - Scheduled ...
|*violation_id*|VARCHAR|**YES**|Looks like 34181_20151230_103141, the first number is business id, second number is the date inspection was done and the third one is the violation id number.
|*violation_description*|VARCHAR|**YES**|Description of what was wrong with the restaurant.
|*risk_category*|VARCHAR|**YES**|One of NULL, Low Risk, Moderate Risk, High Risk

To make life easier, always do a select for all the columns needed in the question, look at the returned values, ponder a bit and then try to write the solution.

## Beginner exercise

1. Discover all business which have safety violations which are of low risk.

Columns: `risk_category` and `business_name`

```sql
SELECT 
    business_name
FROM datasets.sf_restaurant_health_violations
WHERE risk_category = 'Low Risk'
```

2. Find all business which have a phone number.

Columns: `business_name` and `business_phone_number`

Hint: Use null check.

```sql
SELECT 
    DISTINCT business_name
FROM datasets.sf_restaurant_health_violations
WHERE business_phone_number IS NOT NULL
```

3. Find all postal codes where there are issues with water.

Columns: `business_postal_code` and `violation_description`

Hint: Use ILIKE.

```sql
SELECT 
    DISTINCT business_postal_code
FROM datasets.sf_restaurant_health_violations
WHERE violation_description ILIKE '%water%'
```

4. Find the business names along when inspections dates when the inspection score was below 50.

Columns: `business_name`, `inspection_date` and `inspection_score`

```sql
SELECT 
    business_name,
    inspection_date :: DATE,
    inspection_score
FROM datasets.sf_restaurant_health_violations
WHERE inspection_score < 50
```

5. Find all inspections made over restaurants with the business name and inspection score shown.

Columns: `business_name`, `inspection_score`

Hint: Use ILIKE.

```sql
SELECT 
    business_name,
    inspection_score
FROM datasets.sf_restaurant_health_violations
WHERE business_name ILIKE '%Restaurant%'
```

## Intermediate exercises

1. Find the number of inspections per street name. Count only the inspections which have some level of risk associated. Which street has the highest number of inspections which resulted in the inspector assigning non null risk categories?

Columns: `business_adress` and `risk_category`

Hint: Use split_part to find the street name from business address.

```sql
SELECT 
    -- lower because Mission and MISSION should be same streets
    lower(split_part(business_address, ' ', 2)) AS street,
    count(*) AS number_of_risky_restaurants
FROM datasets.sf_restaurant_health_violations
WHERE risk_category IS NOT NULL
GROUP BY street
ORDER BY number_of_risky_restaurants DESC
```

2. Your supervisor wants to know how many inspections of each type resulted in violations for all 3 categories and also for no violation cases. The output should look like:

| inspection_type | no_risk_results | low_risk_results | medium_risk_results | high_risk_results |total_inspections |
|-|-|-|-|-|-|
| New Ownership - Followup | 180 | 11 | 21 | 4 | 216 |
| Complaint | 1003 | 530 | 461 | 217 | 2211

Columns: `inspection_type` and `risk_category`

Hint: Use the SUM with CASE technique.

```sql
SELECT 
    inspection_type,
    
    SUM(CASE 
        WHEN risk_category IS NULL THEN 1 ELSE 0
        END
    ) AS no_risk_results,
    
    SUM(CASE 
        WHEN risk_category = 'Low Risk' THEN 1 ELSE 0
        END
    ) AS low_risk_results,
    
    
    SUM(CASE 
        WHEN risk_category = 'Moderate Risk' THEN 1 ELSE 0
        END
    ) AS medium_risk_results,
    
    SUM(CASE 
        WHEN risk_category = 'High Risk' THEN 1 ELSE 0
        END
    ) AS high_risk_results,
    
    COUNT(*) AS total_inspections
FROM datasets.sf_restaurant_health_violations
GROUP BY inspection_type
ORDER BY total_inspections DESC
```

3. Find the one postal code which has the highest average inspection score. Take special care to deal with NULL values.

Columns: `business_postal_code` and `inspection_score`

```sql
SELECT 
    business_postal_code,
    AVG(inspection_score) AS avg_score
FROM datasets.sf_restaurant_health_violations
WHERE inspection_score IS NOT NULL
GROUP BY business_postal_code
HAVING AVG(inspection_score) IS NOT NULL
ORDER BY avg_score DESC
LIMIT 1
```

4. Find the number of different streets for each postal code. Present the results ordered by the count in descending order.

Columns: `business_postal_code` and `business_adress`.

Hint: Use COUNT DISTINCT

```sql
SELECT 
    business_postal_code,
    COUNT (DISTINCT lower(split_part(business_address, ' ', 2))) AS count_streets
FROM datasets.sf_restaurant_health_violations
GROUP BY business_postal_code
ORDER BY count_streets DESC
```

5. Your task is to classify each business as either a restaurant, cafe, taqueria, kitchen, garden, school or other. 

Columns: `business_name`

Hint: Use CASE with ILIKE.

```sql
SELECT
    DISTINCT ON (business_name)

    business_name,
    
    CASE 
        WHEN business_name ILIKE '%restaurant%' THEN 'Restaurant'
        WHEN business_name ILIKE '%cafe%' THEN 'Cafe'
        WHEN business_name ILIKE '%taqueria%' THEN 'Taqueria'
        WHEN business_name ILIKE '%kitchen%' THEN 'Kitchen'
        WHEN business_name ILIKE '%garden%' THEN 'Garden'
        WHEN business_name ILIKE '%School%' THEN 'School'
        ELSE 'Other'
    END AS business_type
FROM datasets.sf_restaurant_health_violations
```

6. How many violations did each school have? A violation is any inspection with a non-null risk category.

Columns: `business_name` and `risk_category`

Hint: Use the SUM - CASE technique. 

```sql
SELECT 
    business_name,
    SUM (CASE WHEN risk_category IS NOT NULL THEN 1 ELSE 0 END) AS number_of_violations
FROM datasets.sf_restaurant_health_violations
WHERE business_name ILIKE '%school%'
GROUP BY business_name
ORDER BY number_of_violations DESC
```

7. How many inspections with violations happened to 'Roxanne Cafe' for each year?

Columns: `business_name`, `inspection_date` and `risk_category`

Hint: Use EXTRACT to get the year from `inspection_date`.

```sql
SELECT 
    EXTRACT (YEAR FROM inspection_date :: DATE) AS year,
    COUNT(*)
FROM datasets.sf_restaurant_health_violations
WHERE business_name = 'Roxanne Cafe' AND risk_category IS NOT NULL
GROUP BY year
ORDER BY year
```

8. Present the count of inspections by risk category. Display null values as 'No Risk'

Columns: `risk_category`

Hint: Use COALESCE

```sql
SELECT
    COALESCE(risk_category, 'No Risk'),
    COUNT(*)
FROM datasets.sf_restaurant_health_violations
GROUP BY risk_category
ORDER BY count DESC
```

9. Find the number of words in the each business name. Assume the maximal length of a name is 5 words. You can count special signs as words (e.g. &)

Columns: `business_name`

Hint: split_part returns an empty string when given an index which exceeds the bounds. For example: split_part('hello world', ' ', 1) is 'hello', split_part('hello world', ' ', 2) is 'world', while split_part('hello world', ' ', 3) = split_part('hello world', ' ', 4) = split_part('hello world', ' ', 5) = split_part('hello world', ' ', 1000000) = '' with '' denoting the empty string.

```sql
SELECT
    DISTINCT ON (business_name)

    business_name,
    split_part(business_name, ' ', 1) AS t1,
    split_part(business_name, ' ', 2) AS t2,
    split_part(business_name, ' ', 3) AS t3,
    split_part(business_name, ' ', 4) AS t4,
    split_part(business_name, ' ', 5) AS t5,
    split_part(business_name, ' ', 6) AS t6,
    
    CASE 
        WHEN split_part(business_name, ' ', 1) = '' THEN 0
        WHEN split_part(business_name, ' ', 2) = '' THEN 1
        WHEN split_part(business_name, ' ', 3) = '' THEN 2
        WHEN split_part(business_name, ' ', 4) = '' THEN 3
        WHEN split_part(business_name, ' ', 5) = '' THEN 4
        WHEN split_part(business_name, ' ', 6) = '' THEN 5
        ELSE 6
    END AS name_word_count
FROM datasets.sf_restaurant_health_violations
```

10. Find all businesses whose minimum inspection score is different from their maximum inspection score.

Columns: `business_name` and `inspection_score`

Hint: Use a HAVING clause.

```sql
SELECT
    business_name,
    MIN(inspection_score) AS min_score,
    MAX(inspection_score) AS max_score
FROM datasets.sf_restaurant_health_violations
GROUP BY business_name
HAVING MIN(inspection_score) <> MAX(inspection_score)
```

11. When were the first and last inspections for vermin infestations per municipality?

Columns: `business_postal_code`, `violation_description` and `inspection_date`

Hint: First date is minimal date while last date is maximal date.

```sql
SELECT
    business_postal_code,
    MIN(inspection_date :: DATE) AS first_inspection,
    MAX(inspection_date :: DATE) AS last_inspection
FROM datasets.sf_restaurant_health_violations
WHERE violation_description ILIKE '%vermin%' AND business_postal_code IS NOT NULL
GROUP BY business_postal_code
```

12. How many complaints ended in a violation?

Columns: `inspection_type` and `risk_category`

```sql
SELECT
    COUNT(*)
FROM datasets.sf_restaurant_health_violations
WHERE inspection_type = 'Complaint' AND 
      risk_category IS NOT NULL
```


## Advanced exercises

1. How many inspections happened in municipality with postal code 94102 in January, May or November for each year?

Columns: `inspection_date` and `business_postal_code`

Hint: Use pivot table techniques.

```sql
SELECT
    year, 
    
    MAX(january_counts) AS january_counts,
    MAX(may_counts) AS may_counts,
    MAX(november_counts) AS november_counts
FROM
(SELECT 
    EXTRACT (YEAR FROM inspection_date :: DATE) AS year,
    EXTRACT (MONTH FROM inspection_date :: DATE) AS month,
    
    SUM(
        CASE 
        WHEN EXTRACT (MONTH FROM inspection_date :: DATE) = 1 THEN 1 ELSE 0
        END
    ) AS january_counts,
    
    SUM(
        CASE 
        WHEN EXTRACT (MONTH FROM inspection_date :: DATE) = 5 THEN 1 ELSE 0
        END
    ) AS may_counts,
    
    SUM(
        CASE 
        WHEN EXTRACT (MONTH FROM inspection_date :: DATE) = 11 THEN 1 ELSE 0
        END
    ) AS november_counts
FROM datasets.sf_restaurant_health_violations
WHERE 
    business_postal_code = '94102' AND
    EXTRACT (MONTH FROM inspection_date :: DATE) IN (1, 5, 11)
GROUP BY year, month
ORDER BY year, month) tmp
GROUP BY tmp.year
```

2. What is the average number of businesses per street? What is the maximum? Consider only these streets which have more than 4 businesses.

Columns: `business_name` and `business_adress`

```sql
SELECT
    AVG(bc) AS solution1,
    MAX(bc) AS solution2
FROM
(SELECT
    split_part(business_address, ' ', 2) AS street_name,
    COUNT (DISTINCT business_name) AS bc
FROM datasets.sf_restaurant_health_violations
WHERE split_part(business_address, ' ', 2) <> ''
GROUP BY street_name
HAVING COUNT (DISTINCT business_name) >= 5) tmp
```

3. Find all inspections for the business with the highest number of high risk violations.

Columns: `business_name` and `risk_category`

```sql
SELECT
    main.*
FROM 
    datasets.sf_restaurant_health_violations main
INNER JOIN
    (SELECT
        business_name
    FROM datasets.sf_restaurant_health_violations
    WHERE risk_category = 'High Risk'
    GROUP BY business_name
    ORDER BY COUNT(*) DESC
    LIMIT 1) tmp
ON main.business_name = tmp.business_name
```

4. Using a subquery, the function LEFT and type casting take the first 4 digits of the phone number and verify that these 4 digits are equal to 1415 for all phone numbers which are not null. 

Columns: `business_phone_number`

Hint: Think of the phone number as a string of digits.

```sql
SELECT
    *
FROM
(SELECT
    LEFT(business_phone_number :: TEXT, 4) AS calling_code
FROM
    datasets.sf_restaurant_health_violations
WHERE business_phone_number IS NOT NULL) tmp
WHERE calling_code <> '1415'
```

5. Find the average inspection score over inspection types and business types as given by the query in question 5 from the previous section.

Hint: You can combine joins with groupby.

```sql
SELECT
    inspection_type,
    business_type,
    AVG(inspection_score) AS avg_score
FROM
    datasets.sf_restaurant_health_violations main
INNER JOIN
    (SELECT
        DISTINCT ON (business_name)
    
        business_name,
        
        CASE 
            WHEN business_name ILIKE '%restaurant%' THEN 'Restaurant'
            WHEN business_name ILIKE '%cafe%' THEN 'Cafe'
            WHEN business_name ILIKE '%taqueria%' THEN 'Taqueria'
            WHEN business_name ILIKE '%kitchen%' THEN 'Kitchen'
            WHEN business_name ILIKE '%garden%' THEN 'Garden'
            WHEN business_name ILIKE '%School%' THEN 'School'
            ELSE 'Other'
        END AS business_type
    FROM datasets.sf_restaurant_health_violations) bc
ON main.business_name = bc.business_name
GROUP BY main.inspection_type, bc.business_type
```

6. For every year find the worst 3 business judged by the number of violations they committed during the year. More violations means a worse business.

Columns: `business_name`, `inspection_date` and `risk_category`

Hint: Use ROW_NUMBER()

```sql
SELECT
    year,
    
    MAX(worst_offender) AS worst_offender,
    MAX(second_worst_offender) AS second_worst_offender,
    MAX(third_worst_offender) AS third_worst_offender
FROM
    (SELECT
        year,
        
        CASE 
            WHEN yearly_position = 1
            THEN business_name || ' (' || cnt || ')'
            ELSE NULL
        END AS worst_offender,
        
        CASE 
            WHEN yearly_position = 2
            THEN business_name || ' (' || cnt || ')'
            ELSE NULL
        END AS second_worst_offender,
        
        CASE 
            WHEN yearly_position = 3
            THEN business_name || ' (' || cnt || ')'
            ELSE NULL
        END AS third_worst_offender
    FROM
        (SELECT
            business_name,
            year,
            cnt,
            ROW_NUMBER() OVER (PARTITION BY year ORDER BY cnt DESC) AS yearly_position
            FROM
            (SELECT
                business_name,
                EXTRACT(YEAR FROM inspection_date :: DATE) AS year,
                COUNT(*) AS cnt
            FROM
                datasets.sf_restaurant_health_violations
            WHERE risk_category IS NOT NULL
            GROUP BY business_name, year) tmp) tmp2
    WHERE yearly_position <= 3) tmp3
GROUP BY year
```

7. Determine the change in number of daily inspections. Count only inspections which resulted in found violations.

Columns: `inspection_date` and `risk_category`

Hint: Use LAG.

```sql
SELECT
    inspection_date,
    daily_violations_count - COALESCE(prev_daily_violations_count, 0) AS diff
FROM
    (SELECT
        inspection_date,
        daily_violations_count,
        LAG(daily_violations_count, 1)
        OVER (ORDER BY inspection_date) AS prev_daily_violations_count
    FROM
        (SELECT 
            inspection_date :: DATE,
            COUNT(*) AS daily_violations_count
        FROM datasets.sf_restaurant_health_violations
        WHERE risk_category IS NOT NULL
        GROUP BY inspection_date
        ORDER BY inspection_date ASC) tmp) tmp2
```

8. Find the median inspection score given to each business.

Columns: `business_name` and `inspection_score`

```sql
SELECT
    business_name,
    inspection_score
FROM
    (SELECT
        business_name,
        
        inspection_score,
        
        NTILE(100) OVER 
        (PARTITION BY business_name ORDER BY inspection_score) 
        AS percentile
    FROM 
        datasets.sf_restaurant_health_violations
    WHERE inspection_score IS NOT NULL) kms
WHERE percentile = 50
ORDER BY inspection_score DESC
```