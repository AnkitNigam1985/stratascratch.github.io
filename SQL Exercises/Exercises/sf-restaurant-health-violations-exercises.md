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

2. Find all business which have a phone number.

Columns: `business_name` and `business_phone_number`

Hint: Use null check.

3. Find all postal codes where there are issues with water.

Columns: `business_postal_code` and `violation_description`

Hint: Use ILIKE.

4. Find the business names along when inspections dates when the inspection score was below 50.

Columns: `business_name`, `inspection_date` and `inspection_score`

5. Find all inspections made over restaurants with the business name and inspection score shown.

Columns: `business_name`, `inspection_score`

Hint: Use ILIKE.

## Intermediate exercises

1. Find the number of inspections per street name. Count only the inspections which have some level of risk associated. Which street has the highest number of inspections which resulted in the inspector assigning non null risk categories?

Columns: `business_adress` and `risk_category`

Hint: Use split_part to find the street name from business address.

2. Your supervisor wants to know how many inspections of each type resulted in violations for all 3 categories and also for no violation cases. The output should look like:

| inspection_type | no_risk_results | low_risk_results | medium_risk_results | high_risk_results |total_inspections |
|-|-|-|-|-|-|
| New Ownership - Followup | 180 | 11 | 21 | 4 | 216 |
| Complaint | 1003 | 530 | 461 | 217 | 2211

Columns: `inspection_type` and `risk_category`

Hint: Use the SUM with CASE technique.

3. Find the one postal code which has the highest average inspection score. Take special care to deal with NULL values.

Columns: `business_postal_code` and `inspection_score`

4. Find the number of different streets for each postal code. Present the results ordered by the count in descending order.

Columns: `business_postal_code` and `business_adress`.

Hint: Use COUNT DISTINCT

5. Your task is to classify each business as either a restaurant, cafe, taqueria, kitchen, garden, school or other. 

Columns: `business_name`

Hint: Use CASE with ILIKE.

6. How many violations did each school have? A violation is any inspection with a non-null risk category.

Columns: `business_name` and `risk_category`

Hint: Use the SUM - CASE technique. 

7. How many inspections with violations happened to 'Roxanne Cafe' for each year?

Columns: `business_name`, `inspection_date` and `risk_category`

Hint: Use EXTRACT to get the year from `inspection_date`.

8. Present the count of inspections by risk category. Display null values as 'No Risk'

Columns: `risk_category`

Hint: Use COALESCE

9. Find the number of words in the each business name. Assume the maximal length of a name is 5 words. You can count special signs as words (e.g. &)

Columns: `business_name`

Hint: split_part returns an empty string when given an index which exceeds the bounds. For example: split_part('hello world', ' ', 1) is 'hello', split_part('hello world', ' ', 2) is 'world', while split_part('hello world', ' ', 3) = split_part('hello world', ' ', 4) = split_part('hello world', ' ', 5) = split_part('hello world', ' ', 1000000) = '' with '' denoting the empty string.

10. Find all businesses whose minimum inspection score is different from their maximum inspection score.

Columns: `business_name` and `inspection_score`

Hint: Use a HAVING clause.

11. When were the first and last inspections for vermin infestations per municipality?

Columns: `business_postal_code`, `violation_description` and `inspection_date`

Hint: First date is minimal date while last date is maximal date.

12. How many complaints ended in a violation?

Columns: `inspection_type` and `risk_category`

## Advanced exercises

1. How many inspections happened in municipality with postal code 94102 in January, May or November for each year?

Columns: `inspection_date` and `business_postal_code`

Hint: Use pivot table techniques.

2. What is the average number of businesses per street? What is the maximum? Consider only these streets which have more than 4 businesses.

Columns: `business_name` and `business_adress`

3. Find all inspections for the business with the highest number of high risk violations.

Columns: `business_name` and `risk_category`

4. Using a subquery, the function LEFT and type casting take the first 4 digits of the phone number and verify that these 4 digits are equal to 1415 for all phone numbers which are not null. 

Columns: `business_phone_number`

Hint: Think of the phone number as a string of digits.

5. Find the average inspection score over inspection types and business types as given by the query in question 5 from the previous section.

Hint: You can combine joins with groupby.

6. For every year find the worst 3 business judged by the number of violations they committed during the year. More violations means a worse business.

Columns: `business_name`, `inspection_date` and `risk_category`

Hint: Use ROW_NUMBER()

7. Determine the change in number of daily inspections. Count only inspections which resulted in found violations.

Columns: `inspection_date` and `risk_category`

Hint: Use LAG.

8. Find the median inspection score given to each business.

Columns: `business_name` and `inspection_score`