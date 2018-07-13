- Investigate the data and see if anything needs cleaning. 

We make a group by over stars and count everything to emulate the `value_counts` function in pandas.

```sql
SELECT stars, COUNT(*)
FROM datasets.yelp_reviews
GROUP BY stars
ORDER BY stars
```

- Clean the data by removing the reviews with '?' for stars rating

We just select all rows whose stars column is NOT EQUAL to '?'

```sql
SELECT *
FROM datasets.yelp_reviews
WHERE stars <> '?'
```

- Replace the stars values that are text with integers

To transform stars to integers we use the syntax `stars :: INTEGER`.

Changing the type of any column follows the same syntax `column_name :: NEW_TYPE`.

```sql
SELECT 
    business_name,
    review_id,
    user_id,
    stars :: INTEGER,
    review_date,
    review_text,
    funny,
    useful,
    cool
FROM datasets.yelp_reviews
WHERE stars <> '?'
```

- How many 5 star reviews does Lo-Lo's Chicken & Waffles have?

We need to use the wildcard symbol (%) because we can't use the apostrophe symbol (') in sql strings.

This query is filter first then count.

```sql
SELECT COUNT(*)
FROM datasets.yelp_reviews
WHERE business_name LIKE 'Lo-Lo%s Chicken & Waffles'
      AND stars = '5'
```

- What is Lo-Lo's Chicken & Waffles star review breakdown?

This is a modification of query from question 1 to include a filter by business_name

```sql
SELECT stars, count(*)
FROM datasets.yelp_reviews
WHERE business_name LIKE 'Lo-Lo%s Chicken & Waffles'
GROUP BY stars
ORDER BY stars
```

- What's the most number of cool votes a review received?

```sql
SELECT MAX(cool)
FROM datasets.yelp_reviews
```

- What was the business' name and review_text for the business that received the most cool number of votes

This is the standard way of performing the argmax operation.
We find the maximal value using a single query and do inner join
with the max_cool value as join the key joining this temporary table with the main table.

```sql
SELECT 
    business_name,
    review_text
FROM 
    datasets.yelp_reviews reviews,
    (SELECT MAX(cool) AS max_cool
    FROM datasets.yelp_reviews) mc
WHERE reviews.cool = mc.max_cool
```

