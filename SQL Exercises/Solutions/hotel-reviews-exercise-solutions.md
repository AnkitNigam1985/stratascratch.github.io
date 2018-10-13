# Hotel Reviews Exercise Solutions

This dataset contains 515,000 customer reviews and scores of 1493 luxury hotels across Europe.
The geographical location of hotels is also provided for further analysis.

The exercises below uses the `datasets.hotel_reviews` table.

### Question 1
Which hotel has the most reviews?

*Solution:*

In our query, we select the distinct names of hotels and determine the maximum number of hotel reviews. 

By sorting our data in descending order, we can find top hotels with highest number of reviews.

```sql
SELECT 
    DISTINCT hotel_name,
    max(total_number_of_reviews) AS max_reviews
FROM datasets.hotel_reviews 
GROUP BY hotel_name
ORDER BY max_reviews DESC
```
*Answer:* `Hotel Da Vinci = 16670`

### Question 2
What is the average of total negative reviews' word counts?

*Solution:*

We can answer the question by simply getting the average total word counts of negative reviews from the dataset.
```sql
SELECT 
    AVG(review_total_negative_word_counts) AS avg_no_of_words
FROM datasets.hotel_reviews
```
*Answer:* `19`

### Question 3
What is the average of total positive reviews' word counts?

*Solution:*

We can answer the question by simply getting the average total word counts of positive reviews from the dataset.
```sql
SELECT 
    AVG(review_total_positive_word_counts) AS avg_no_of_words
FROM datasets.hotel_reviews
```
*Answer:* `18`

### Question 4
Which hotels have the highest rating? Show the top 10.

*Solution:*

The problem can be solved by getting the hotel name and average score of each hotel from the dataset. Using the `GROUP BY` and `ORDER BY` statements, we can easily get the top 10 with the highest rating in descending order using LIMIT.

```sql
SELECT
    hotel_name, 
    average_score
FROM 
    datasets.hotel_reviews 
GROUP BY hotel_name, average_score 
ORDER BY average_score DESC 
LIMIT 10
```

### Question 5
Which hotels have the worst rating? Show the top 10.

*Solution:*

The problem can be solved by getting the hotel name and average score of each hotel from the dataset. Using the `GROUP BY` and `ORDER BY` statements, we can easily get the top 10 with the worst rating in descending order using LIMIT.

```sql
SELECT 
    hotel_name, 
    average_score 
FROM 
    datasets.hotel_reviews 
GROUP BY hotel_name,average_score
ORDER BY average_score ASC
LIMIT 10
```

### Question 6
Which hotels have the most negative reviews in the summertime (June-Aug)? Show the top 10.

*Solution:*

To answer the question, we simply select the hotel name and count the negative reviews of each hotel. We filter out data through a condition statement that exclude `No Negative` reviews and reviews that are not within the dates '6/1/17' AND '8/31/17'. THe final output is then grouped and ranked in descending order so we can easily view the top 10 result.

```sql
SELECT 
    hotel_name, 
    COUNT(negative_review) AS no_negative 
FROM
    datasets.hotel_reviews 
WHERE 
    negative_review <> 'No Negative' AND 
    review_date BETWEEN '6/1/17' AND '8/31/17'
GROUP BY hotel_name
ORDER BY no_negative DESC
LIMIT 10
```

### Question 7
Which hotels have the most positive reviews in the summertime (June-Aug)? Show the top 10.

*Solution:*

To answer the question, we simply select the hotel name and count the positive reviews of each hotel. We filter out data through a condition statement that exclude `No Positive` reviews and reviews that are not within the dates '6/1/17' AND '8/31/17'. THe final output is then grouped and ranked in descending order so we can easily view the top 10 result.
```sql
SELECT 
    hotel_name,
    COUNT(positive_review) AS positive 
FROM 
    datasets.hotel_reviews 
WHERE 
    NOT positive_review= 'No Positive' AND 
        review_date BETWEEN '6/1/17' AND '8/31/17'
GROUP BY hotel_name
ORDER BY positive DESC
LIMIT 10
```

### Question 8
Which countries have the most negative reviews?

*Solution:*

Here, we simply count the negative reviews from the dataset and add a condition to exclude a review with `No Negative.` We order and rank the results by reviewer_nationality and negative reviews, respectively.
```sql
SELECT 
    reviewer_nationality, 
    count(negative_review) AS no_negative
FROM datasets.hotel_reviews
WHERE NOT negative_review = 'No Negative'  
GROUP BY reviewer_nationality
ORDER BY no_negative DESC
```

### Question 9
Which countries have the most positive reviews?

*Solution:*

To answer the question, we simply count the positive reviews from the dataset and add a condition to exclude a review with `No Positive.` We order and rank the results by `reviewer_nationality` and positive reviews, respectively.
```sql
SELECT 
    reviewer_nationality, 
    COUNT(positive_review) AS positive
FROM datasets.hotel_reviews
WHERE NOT positive_review = 'No Positive'
GROUP BY reviewer_nationality
ORDER BY  positive desc
```

### Question 10
Which hotels got the most reviews that a particular reviewer has given?

*Solution:*

Here, we want to get first the maximum number of reviews a reviewer has given for a hotel through the `SELECT` and `MAX` statements. This is followed by grouping and ranking the hotels and maximum number of reviews, respectively.
```sql
SELECT 
    hotel_name, 
    MAX(total_number_of_reviews_reviewer_has_given) AS max
FROM datasets.hotel_reviews
GROUP BY hotel_name
ORDER BY max DESC
LIMIT 5
```
