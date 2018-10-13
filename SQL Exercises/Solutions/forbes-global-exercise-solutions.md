# Forbes Global Exercise Solutions

The Forbes Global 2000 is an annual ranking of the top 2,000 public companies in the world by Forbes magazine. 
The ranking is based on a mix of four metrics: sales, profit, assets and market value. The list has been published since 2003. 
Our dataset consists of 2000 rows and 10 columns. The columns are rank, company, country, continent, sector, industry, market value, 
sales, profits, and assets. The purpose of this dataset is to help you figure out what kind of industries are currently the most
profitable and popular, as well as the top countries that are currently having these companies.

The table used for the following exercises can be found at `datasets.forbes_global_2010_2014`.

### Question 1
Which country has the most companies from the dataset?

*Solution:*

We can determine the country with the most companies through the `SELECT` statement, followed by the `count` function to count the number of companies listed on the `datasets.forbes_global_2010_2014` table. By grouping and sorting the result, we can determine the country with the highest number of companies.
```sql
SELECT country, count(company)
FROM datasets.forbes_global_2010_2014
GROUP BY country
ORDER BY count (company) DESC 
```

### Question 2
Which is the most popular sector from the list?

*Solution:*

We can answer this question by simply counting all the rows in the table and arrange the data in descending order. This can be achieved using the statements `GROUP BY`, `ORDER BY` and `DESC`.
```sql
SELECT sector, count(*) 
FROM datasets.forbes_global_2010_2014
GROUP BY sector
ORDER BY count DESC
```

### Question 3
Which industries boast the highest number of companies?

*Solution:*

To solve the problem, we can start by selecting the industry column and counting the number of companies. We can simply group and sort the data in descending order to know which industries have the highest number of companies.

```sql
SELECT industry, count(company)
FROM datasets.forbes_global_2010_2014
GROUP BY industry
ORDER BY count (company) DESC 
```

### Question 4
What is the average profit for the major banks industry?

*Solution:*

We can solve the average profit by including the `avg` function on the `SELECT` statement. Adding a condition statement `industry = 'Major Banks'` will show results under the major banking industry only.
```sql
SELECT avg(profits)
FROM datasets.forbes_global_2010_2014
WHERE industry = 'Major Banks' 
```
*Answer:* `4.58`

### Question 5
Which industry has the highest market value in Asia?

*Solution:*

To answer the question, we need to compute first the sum of the market value through the `SELECT` statement. We add the condition `continent = 'Asia'` to exclude other countries outside of Asia, and sort the data in descending order so that we can determine the top industry with the highest market value.
```sql
SELECT industry, sum(marketvalue) AS total_marketvalue
FROM datasets.forbes_global_2010_2014 
WHERE continent = 'Asia'
GROUP BY industry
ORDER BY total_marketvalue DESC
```

### Question 6
What is the total market value of the financial sector?

*Solution:*

To solve the problem, we simply sum up the total market value through the `SELECT` statement from the `datasets.forbes_global_2010_2014` table.
```sql
SELECT 
    SUM(marketvalue) AS total_marketvalue
FROM datasets.forbes_global_2010_2014 
WHERE sector = 'Financials'
```

*Answer:* `10767.3`

### Question 7
How many American companies are on the list?

*Solution:*

We can determine the number of American companies by firt applying the `count` function and a condition where `country = 'United States'`.
```sql
SELECT count(company)
FROM datasets.forbes_global_2010_2014
WHERE country= 'United States'
```
*Answer:* `563`

### Question 8
What are the top 3 sectors in the United States by the average rank?

*Solution:*


To find the desired 3 sectors we calculate the average rank over all sectors, order the output in an ascending fashion and take the three sectors which have the highest rank.

```sql
SELECT
    sector,
    AVG(rank) AS avg_rank
FROM
    datasets.forbes_global_2010_2014
GROUP BY sector
ORDER BY avg_rank ASC
LIMIT 3
```

### Question 9
What are the total assets of the energy sector?

*Solution:*

To solve the problem, we simply sum up the total assets from the table, and add a condition where `sector = 'Energy'`. 
```sql
SELECT sum(assets)
FROM datasets.forbes_global_2010_2014
WHERE sector = 'Energy'
```
*Answer:* `6564.7`

### Question 10
Which continent has the highest number of companies?

*Solution:*

We can determine the continent with the highest number of companies by first selecting the continent column and count the number of companies within the continent. Then we group and sort the data in descending order to know the top continent having the highest number of companies.

```sql
SELECT continent, count(company)
FROM datasets.forbes_global_2010_2014
GROUP BY continent
ORDER BY count (company) DESC
```
