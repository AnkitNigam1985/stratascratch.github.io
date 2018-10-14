# Yelp Exercise Solutions
This exercise includes a table of various data gathered on Yelp. The following questions are provided with the correct SQL query solution
and graphs using the graphics function in Strata Scratch.

## Description of datasets.yelp_business 
The datasets.yelp_business table contains various data gathered on Yelp such as users, reviews, ratings, business address and categories. 

### Question 1 
What are the top 5 states with the most 5 star businesses?

*Solution:*

To determine the top 5 states having the most 5 star ratings, we first count the number of stars per state through the `SELECT` statement. We add a conditional statement where star is equal to 5 to display only results with 5 star ratings. The resulting states are then grouped and ranked in descending order.
```sql
SELECT 
    state,
    COUNT(stars) as count_of_5_stars
FROM datasets.yelp_business
WHERE stars = 5
GROUP BY state
ORDER BY count_of_5_stars DESC
LIMIT 5
```

### Question 2 
What are the top 5 cities with the most 5 star businesses? Limit visualization to 10.

*Solution:*

The top 5 cities with 5 star ratings can be determined by counting the star ratings per city and add a data filter where `stars = 5`. The resulting cities are then grouped and ranked in descending order.
```sql
SELECT 
    city,
    COUNT(stars) AS count_of_5_stars
FROM datasets.yelp_business
WHERE stars = 5
GROUP BY city
ORDER BY count_of_5_stars DESC
```

### Question 3 
What are the top 5 businesses with the most reviews?

*Solution:*

We can determine the top 5 businesses having the most reviews by taking the sum of the reviews per business. We group together the same business names from the result and ranked the result in descending order.
```sql
SELECT 
    name,
    SUM (review_count) AS total_of_reviews
FROM datasets.yelp_business
GROUP BY name
ORDER BY total_of_reviews DESC
LIMIT 50
```

### Question 4 
What are the top category businesses most people review for?

*Solution:*

The question can be answered by taking the sum of reviews per category. The resulting categories are grouped and ranked according to the total number of reviews in descending order.
```sql
SELECT 
    categories,
    SUM(review_count) AS total_reviews
FROM datasets.yelp_business
GROUP BY categories
ORDER BY total_reviews DESC
LIMIT 5
```

### Question 5 
What are the most one star review businesses from yelp?

*Solution:*

The businesses having the most number of one star reviews can be determined by selecting the `name, stars`, and `review_count` columns. We then filter the data by adding the conditional statement `star = 1` to neglect other results with more than 1 star. 
```sql
SELECT
    name,
    stars, 
    review_count
FROM datasets.yelp_business
WHERE stars = 1
GROUP BY name, stars, review_count
LIMIT 10
```

### Question 6 
How many businesses are open?

*Solution:*

In our query, we can use the `count` function to determine the number of businesses that are open, followed by a conditional statement `WHERE is_open = 1` to get the desired result.
```sql
SELECT 
    COUNT(is_open) AS business_open
FROM datasets.yelp_business
WHERE is_open = 1
```
*Output:* `146702`

### Question 7 
What is the average stars of each states?

*Solution:*

Here, we compute the average stars per state by using the `AVG` function. To eliminate any duplicates, the `GROUP BY` clause is added on the query.
```sql
SELECT 
    state,
    AVG(stars) AS average_stars
FROM datasets.yelp_business
GROUP BY state
```

## Description of datasets.yelp_checkin
The  datasets.yelp_checkin table provides the amount of check-ins each business has and when that check-in took place. Check-ins are offered with rewards that businesses give to customers whenever they “check-in” to the business on Yelp. By using the check-in feature, customers are able to broadcast to their friends on Yelp that they're at your business.

### Question 8
What are the top 5 businesses with the most check-ins?

*Solution:*

The top 5 businesses having the most number of check-ins can be determined by counting the number of check-ins per business ID. We can group and rank the businesses according to the number of check-ins in descending order to know the top 5 results.
```sql
SELECT 
    business_id,
    COUNT(checkins) AS check_in
FROM datasets.yelp_checkin
GROUP BY business_id
ORDER BY check_in DESC
LIMIT 5
```




