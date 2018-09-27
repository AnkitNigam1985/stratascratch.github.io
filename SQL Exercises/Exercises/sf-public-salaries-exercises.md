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

2. Find the base pay for Police Captains.

Columns: `basepay` and `jobtitle`

Hint: Use ILIKE

3. Find the job titles which had 0 hours of overtime.

Columns: `jobtitle` and `overtimepay`

4. What benefits do people called Patrick have?

Columns: `benefits` and `employeename`

5. Find all employees of METROPOLITAN TRANSIT AUTHORITY and their total pay with benefits.

Columns: `employeename` and `totalpaybenefits`

## Intermediate and advanced exercises

1. Find the average total pay for each employee.

Columns: `employeename` and `totalpay`

2. Find the number of police officers, firefighters and medical staff.

Columns: `employeename` and `jobtitle`

Hint: Use CASE with ILIKE. Use a subquery.

3. Find the people who earned least and most money without benefits.

Columns: `employeename` and `totalpay`


4. Find the number of people who have negative total pay?

Columns: `totalpay`

5. Find the top 5 best paid and top 5 worst paid employees in 2012.

Hint: Use two queries and union the results. Hint you must use subqeries when unioning queries which contain orderby clauses.

Columns: `employeename` and `totalpaybenefits`

6. Find all people who earned more money via bonuses then by their base pay. From these people find which first name yields the highest minimal total pay with benefits.

Columns: `otherpay`, `basepay`, `employee_name`

Hint: Use split_part to find the first name.

7. You are looking for a public job where a lot of people have benefits. To help you in this search you decide to write a query which counts the number of people without benefits, the total number of employees and the ratio between these two quantities. Based on this ratio you should decide where to apply for a job.

Columns: `jobtitle` and `benefits`

8. Make a pivot table where we can see how well the employees fared for each year.

Columns: `employeename`, `year` and `totalpay`

9. What is the median total pay for each job?

Columns: `jobtitle` and `totalpay`

10. What is the difference between the highest `totalpay` and lowest `totalpay` for each `jobtitle`? What is the ratio of highest `totalpay` to lowest one? Which job titles are least fair? Ignore jobs which have `totalpay` of 0 or less.

11. Find all people who earned more than the average in 2013 for their position but were not amongst the top 20 earners in their position.

Columns: `employeename`, `jobtitle` and `totalpaybenefits`

Hint: Use EXCEPT between the two different queries.

12. For each job title find the 5 employees which were paid the least. The result should be a pivot table.

Columns: `employeename`, `jobtitle` and `totalpaybenefits`

13. Who earned most from working overtime?

Columns: `employeename` and `overtimepay`

14. What are top 3 job titles which paid the highest amount of overtime?

Columns: `jobtitle` and `overtimepay`


15. Who are the 2 most paid technicians per field?

Columns: `jobtitle`, `employeename` and `totalpaybenefits`