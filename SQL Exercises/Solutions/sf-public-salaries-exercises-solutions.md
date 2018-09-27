# SQL Exercise 5

The dataset for this exercise is `datasets.sf_public_salaries`

This dataset contains salaries for public executives and administrations. We know the basiscs, the employee name and the job title they hold. Number wise we have the base pay, over time pay, other pay and benefits. These are stored as varchar because there are values 'Not Provided' which we will treat as null when casting to numbers. Thus one row is one employee's yearly income for a single year.

The technical description of the dataset is in the following table.

| Column Name | Column Type | Has NULL or missing values | Short description |
|---|---|---|---|
|id|INTEGER|NO|The primary key of this table.
|employeename|VARCHAR|NO|The full name of the employee.
|jobtitle|VARCHAR|NO|The title of the job the person does. Sometimes also contains information about the agency. Written in all caps.
|basepay|VARCHAR|**YES**|The base amount of money the employee earns per year. Stored as varchar and must be type casted to numeric for use in aggregations.
|overtimepay|VARCHAR|NO|The amount of money earned for the whole year for work done overtime. Stored as varchar and must be type casted to numeric for use in aggregations.
|otherpay|VARCHAR|NO|Extra money coming from other activities. Stored as varchar and must be type casted to numeric for use in aggregations.
|benefits|VARCHAR|**YES**|Money from benefits. Stored as varchar and must be type casted to numeric for use in aggregations.
|totalpay|DOUBLE PRECISION|NO|The total without benefits.|
|totalpaybenefits|DOUBLE PRECISION|NO|Total pay with benefits.
|year|INTEGER|NO|The year when the money was paid out.
|notes|DOUBLE PRECISION|**YES**|Useless column.
|agency|VARCHAR|NO|Always is 'San Francisco'. Useless column no 2.
|status|VARCHAR|**YES**|One of PT, FT or NULL.

To make life easier, always do a select for all the columns needed in the question, look at the returned values, ponder a bit and then try to write the solution.

## Beginner exercise

1. Verify that when provided `totalpaybenefits` = `totalpay` + `benefits` for all columns.

```sql
SELECT
    totalpaybenefits :: NUMERIC,
    totalpay :: NUMERIC + benefits :: NUMERIC AS calculated,
    totalpay :: NUMERIC + benefits :: NUMERIC = totalpaybenefits :: NUMERIC AS same
FROM datasets.sf_public_salaries
WHERE benefits <> 'Not Provided'
```

2. Find the base pay for Police Captains.

Columns: `basepay` and `jobtitle`

Hint: Use ILIKE

```sql
SELECT
    employeename, basepay
FROM datasets.sf_public_salaries
WHERE jobtitle ILIKE '%CAPTAIN%POLICE%'
```

3. Find the job titles which had 0 hours of overtime.

Columns: `jobtitle` and `overtimepay`

```sql
SELECT
    DISTINCT jobtitle
FROM datasets.sf_public_salaries
WHERE overtimepay = '0.0'
```

4. What benefits do people called Patrick have?

Columns: `benefits` and `employeename`

```sql
SELECT
    employeename, benefits
FROM datasets.sf_public_salaries
WHERE 
    employeename ILIKE 'patrick%' AND 
    benefits IS NOT NULL
```

5. Find all employees of METROPOLITAN TRANSIT AUTHORITY and their total pay with benefits.

Columns: `employeename` and `totalpaybenefits`

```sql
SELECT
    employeename,
    totalpaybenefits
FROM datasets.sf_public_salaries
WHERE 
    jobtitle ILIKE '%METROPOLITAN TRANSIT AUTHORITY%'
```

## Intermediate and advanced exercises

1. Find the average total pay for each employee.

Columns: `employeename` and `totalpay`

```sql
SELECT
    employeename,
    AVG(totalpay) AS avg_tp
FROM datasets.sf_public_salaries
GROUP BY employeename
```

2. Find the number of police officers, firefighters and medical staff.

Columns: `employeename` and `jobtitle`

Hint: Use CASE with ILIKE. Use a subquery.

```sql
SELECT
    company,
    count(*) AS cnt
FROM
(SELECT
    DISTINCT ON (lower(employeename))

    lower(employeename) AS employeename,

    CASE
        WHEN jobtitle ILIKE '%Police%'  THEN 'Police'
        WHEN jobtitle ILIKE '%Fire%'    THEN 'Firefighter'
        WHEN jobtitle ILIKE '%Medical%' THEN 'Medical'
    END AS company
FROM datasets.sf_public_salaries) tfmfp
WHERE company IS NOT NULL
GROUP BY company
```

3. Find the people who earned least and most money without benefits.

Columns: `employeename` and `totalpay`

```sql
SELECT
    main.*
FROM
    datasets.sf_public_salaries main
INNER JOIN
    (SELECT
        MAX(totalpay) AS mx,
        MIN(totalpay) AS mn
    FROM
        datasets.sf_public_salaries) AS tmp
ON
    main.totalpay = tmp.mx OR main.totalpay = tmp.mn
```

4. Find the number of people who have negative total pay?

Columns: `totalpay`

```sql
SELECT
    COUNT (main.*)
FROM
    datasets.sf_public_salaries main
WHERE 
    totalpay <= 0
```

5. Find the top 5 best paid and top 5 worst paid employees in 2012.

Hint: Use two queries and union the results. Hint you must use subqeries when unioning queries which contain orderby clauses.

Columns: `employeename` and `totalpaybenefits`

```sql
SELECT * FROM
    (SELECT
        employeename,
        totalpaybenefits
    FROM datasets.sf_public_salaries main
    WHERE year = 2012
    ORDER BY totalpaybenefits DESC
    LIMIT 5) eins
UNION 
SELECT * FROM
    (SELECT
        employeename,
        totalpaybenefits
    FROM datasets.sf_public_salaries main
    WHERE year = 2012
    ORDER BY totalpaybenefits ASC
    LIMIT 5) zwei
-- This order by happens after the UNION
ORDER BY totalpaybenefits ASC
```

6. Find all people who earned more money via bonuses then by their base pay. From these people find which first name yields the highest minimal total pay with benefits.

Columns: `otherpay`, `basepay`, `employee_name`

Hint: Use split_part to find the first name.

```sql
SELECT
    first_name,
    MIN(totalpaybenefits) AS tpb
FROM
(SELECT
    lower(split_part(employeename, ' ', 1)) As first_name,
    totalpaybenefits
FROM 
    datasets.sf_public_salaries
WHERE otherpay > basepay) cutap
GROUP BY first_name
ORDER BY tpb DESC
LIMIT 1
```

7. You are looking for a public job where a lot of people have benefits. To help you in this search you decide to write a query which counts the number of people without benefits, the total number of employees and the ratio between these two quantities. Based on this ratio you should decide where to apply for a job.

Columns: `jobtitle` and `benefits`

```sql
SELECT
    jobtitle,
    no_employees_without_benefits,
    total_people,
    no_employees_without_benefits / total_people AS rto
FROM
    (SELECT
        jobtitle,
        SUM(
            CASE
                WHEN benefits IS NULL OR benefits = 'Not Provided'
                THEN 1
                ELSE 0
            END
        ) AS no_employees_without_benefits,
        
        COUNT(*) AS total_people
    FROM
        datasets.sf_public_salaries
    GROUP BY jobtitle) tmp
ORDER BY rto ASC
```

8. Make a pivot table where we can see how well the employees fared for each year.

Columns: `employeename`, `year` and `totalpay`

```sql
SELECT
    employeename,
    MAX(pay_2011) AS pay_2011,
    MAX(pay_2012) AS pay_2012,
    MAX(pay_2013) AS pay_2013,
    MAX(pay_2014) AS pay_2014
FROM
(SELECT
    employeename,

    CASE 
        WHEN year = 2011
        THEN totalpay
        ELSE 0
    END AS pay_2011,
    
    CASE 
        WHEN year = 2012
        THEN totalpay
        ELSE 0
    END AS pay_2012,
    
    CASE 
        WHEN year = 2013
        THEN totalpay
        ELSE 0
    END AS pay_2013,
    
    CASE 
        WHEN year = 2014
        THEN totalpay
        ELSE 0
    END AS pay_2014
FROM datasets.sf_public_salaries) pmt
GROUP BY employeename
```

9. What is the median total pay for each job?

Columns: `jobtitle` and `totalpay`

```sql
SELECT
    DISTINCT ON (jobtitle, totalpay)
    jobtitle,
    totalpay
FROM
(SELECT
    jobtitle,
    totalpay,
    NTILE(100)
    OVER (PARTITION BY jobtitle ORDER BY totalpay) AS percentile
FROM datasets.sf_public_salaries) fml
WHERE percentile = 50
ORDER BY totalpay DESC
```

10. What is the difference between the highest `totalpay` and lowest `totalpay` for each `jobtitle`? What is the ratio of highest `totalpay` to lowest one? Which job titles are least fair? Ignore jobs which have `totalpay` of 0 or less.

```sql
SELECT
    jobtitle,
    MAX(totalpay) - MIN(totalpay) AS difference,
    MAX(totalpay) / (MIN(totalpay) + 1) AS ratio,
    MAX(totalpay) AS max,
    MIN(totalpay) AS min
FROM datasets.sf_public_salaries
GROUP BY jobtitle
HAVING MIN(totalpay) > 0
ORDER BY ratio DESC
```

11. Find all people who earned more than the average in 2013 for their position but were not amongst the top 20 earners in their position.

Columns: `employeename`, `jobtitle` and `totalpaybenefits`

Hint: Use EXCEPT between the two different queries.

```sql
--  Find all people who earned more than the average in 2013 for their position
SELECT
    employeename
FROM 
    datasets.sf_public_salaries main
INNER JOIN
    (SELECT
        jobtitle,
        AVG(totalpay)
    FROM datasets.sf_public_salaries
    WHERE year = 2013
    GROUP BY jobtitle) aves
ON
    main.jobtitle = aves.jobtitle AND
    main.totalpay > aves.avg
    
-- except = but not
EXCEPT

-- were amongst the top 20 earners in their position
SELECT
    employeename
FROM
    (SELECT
        employeename,
        ROW_NUMBER()
        OVER (PARTITION BY jobtitle ORDER BY totalpay DESC) AS position
    FROM
        datasets.sf_public_salaries) poses
WHERE
    position <= 20

```

12. For each job title find the 5 employees which were paid the least. The result should be a pivot table.

Columns: `employeename`, `jobtitle` and `totalpaybenefits`

```sql
SELECT
    jobtitle,
    
    MAX(first) AS first,
    MAX(second) AS second,
    MAX(third) AS third,
    MAX(fourth) AS fourth,
    MAX(fifth) AS fifth
FROM
    (SELECT
        jobtitle,
    
        CASE
            WHEN pos = 1
            THEN employeename || ' (' || totalpaybenefits || ')'
            ELSE NULL
        END AS first,
        
        CASE
            WHEN pos = 2
            THEN employeename || ' (' || totalpaybenefits || ')'
            ELSE NULL
        END AS second,
        
        CASE
            WHEN pos = 3
            THEN employeename || ' (' || totalpaybenefits || ')'
            ELSE NULL
        END AS third,
        
        CASE
            WHEN pos = 4
            THEN employeename || ' (' || totalpaybenefits || ')'
            ELSE NULL
        END AS fourth,
        
        CASE
            WHEN pos = 5
            THEN employeename || ' (' || totalpaybenefits || ')'
            ELSE NULL
        END AS fifth
    FROM
        (SELECT
            employeename,
            jobtitle,
            totalpaybenefits,
            ROW_NUMBER()
            OVER (PARTITION BY jobtitle ORDER BY totalpaybenefits ASC) as pos
        FROM datasets.sf_public_salaries) uno
    WHERE pos <= 5) dos  
GROUP BY jobtitle
```

13. Who earned most from working overtime?

Columns: `employeename` and `overtimepay`

```sql
SELECT
    employeename
FROM
    datasets.sf_public_salaries
WHERE 
    overtimepay = (SELECT
                    MAX(overtimepay)
                FROM datasets.sf_public_salaries
                WHERE overtimepay IS NOT NULL AND
                      overtimepay <> 'Not Provided')
```

14. What are top 3 job titles which paid the highest amount of overtime?

Columns: `jobtitle` and `overtimepay`

```sql
SELECT
    jobtitle
FROM
    datasets.sf_public_salaries
WHERE overtimepay IS NOT NULL AND overtimepay <> 'Not Provided'
ORDER BY overtimepay DESC
LIMIT 3
```

15. Who are the 2 most paid technicians per field?

Columns: `jobtitle`, `employeename` and `totalpaybenefits`

```sql
SELECT
    jobtitle,
    
    MAX(primus) AS primus,
    MAX(secundum) AS secundum
FROM
    (SELECT
        jobtitle,
        
        CASE
            WHEN pos = 1
            THEN employeename
            ELSE NULL
        END AS primus,
        
        CASE WHEN pos = 2
            THEN employeename
            ELSE NULL
        END AS secundum
    FROM
        (SELECT
            employeename,
            jobtitle,
            totalpaybenefits,
            ROW_NUMBER()
            OVER (PARTITION BY jobtitle ORDER BY totalpaybenefits DESC) AS pos
        FROM
            datasets.sf_public_salaries
        WHERE jobtitle ILIKE '%TECH%') one
    WHERE pos <= 2) two
GROUP BY jobtitle
```