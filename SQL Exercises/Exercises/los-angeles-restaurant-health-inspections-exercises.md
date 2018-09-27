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


2. Find all inspections made in the facilities of 'GLASSELL COFFEE SHOP LLC'

Columns: `owner_name`


3. Find all routine inspections which found an issue with High Risk.

Hint: Use ILIKE.

Columns: `service_description` and `pe_description`


4. Find all facilities which have a zip code in 90049, 90034, 90035.


5. Find all inspections which are part of an inactive program.


## Intermediate exercise

1. Using UNION find the average score for grades A, B and C. Do not use GROUP BY.

Hint: You can do things like `'A' AS grade` in the `SELECT` part. This can be used to disambiguate after the union occurs.

2. Find all corporations which have only a single facility.

Hint: Use HAVING.

3. Find the average number of inspections per facility for all corporations. Present the results in a descending order by that average.

4. Check if `record_id` is unique for every row.

Hint: It is unique if count = count distinct


5. Count the number of low risk issues for bakeries.

Hint: To get correct results use the SUM - CASE technique.

6. Find the minimal score for all facilities on Hollywood boulevard. What is the facility with the highest minimum score?

7. Classify each owner as LLC, INC or other.


8. Find the rules used to determine the grades. Have the rule as a separate column with a textual description like 'Score > X AND Score < Y => Grade = A' where X and Y are the lower and upper bounds for a grade.

Hint: Find the minimum and maximum scores for each grade. Use || to form the rule string.

9. Find all facilities which offer beverages. Assume they offer beverages if their name contains the words tea, coffee or juice. What is the most common issue for this type of venues?


10. Count the number of facilities per municipality along with the number of inspections. Does this data confirm the hypothesis that more facilities = higher number of inspections. Make a scatter plot of this data.


11. Find all bakeries and the most common grade they earned as a collective.

12. Find the number of inspections per day ordered by the date?


13. How many inspections of low risk happened in 2017?


14. Which month had the lowest number of inspections for fish markets over all years?


15. Under assumption that the scores are normally distributed, the mean per groups should be 95 for A. Find the actual mean for these scores using BETWEEN and verify this claim.

## Advanced exercise

1. What is the variance of scores which have grade A? The formula is avg((X_i - mean_x) ^ 2). What does this tell you about the normality assumption of scores for grade A?

Hint: You can use an implicit join to solve this or a left join with a always TRUE condition in the ON clause.

2. What is the most popular street/boulevard/road based on the number of low, medium and high risk inspections. 

Hint: To find the street/boulevard/road name use split_part. Sometimes the name is second or third word. Use UNION to combine all 3 queries into one result. You might have to use subqueries.

3. Find all owners who have at least one facility with grade A, grade B and grade C.

Hint: Even though it may not seem like so, this is easily solvable using pivot table techniques. The trick is to think of pivot table columns as variables which can be used in filters and aggregations.

4. Which facility got most inspections in 2017 compared to other years?

Hint: This is very similar to the preceding question.

6. Find the 4 quartiles of score for each company as a pivot table. Have the rows of this table ordered in ascending order by the average value of the 4 quartiles.

Hint: Use NTILE and pivot table techniques.

7. Find the dates when the most sanitary restaurants got their last inspection. Assume highest number of points is most sanitary. Keep in mind that facility must have the word restaurant in its name. What is the number of days between these inspections?

Hint: Use LAG to answer the last demand.

8. Which street is the trendiest for each year? Assume trendiest as the one with highest total score. Total score is sum of all scores.

Hint: Use ROW_NUMBER()

9. For each owner find the top 3 facilities. In the output have the address and the average score they earned. Decide on the top 3 by that average score. When there are less than 3 facilities display 'No Data'.
