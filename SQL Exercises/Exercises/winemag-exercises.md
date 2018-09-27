# SQL Exercise 3

The datasets for this exercise are `datasets.winemag_p1` and `datasets.winemag_p2`.

The first dataset is about the wines reviewed by the WineMag. We know the geographical origin of each wine like the country, the province and 2 regions from which it originated. We also have descriptions for each wine where we can learn about its aromas. The wine's variety, designation and winery complete the textual information. There are only 2 numeric columns in this dataset and they are the wine's price and the number of points assigned to it by the reviewer.

The technical description of the dataset is in the following table.

| Column Name | Column Type | Has NULL or missing values | Short description |
|---|---|---|---|
|*id*|INTEGER|NO|The primary key for this table.|
|*country*|VARCHAR|**YES**|The country of origin of the wine.|
|*description*|VARCHAR|NO|Description of the wine with information about it's aromas and similar.
|*designation*|VARCHAR|**YES**|The designation of the wine. 
|*points*|INTEGER|NO|How many points are awared to this wine by the wine magazine.
|*price*|DOUBLE PRECISION|**YES**|How much does this wine cost. This value is probably US dollars.
|*province*|VARCHAR|**YES**|The province from which the wine label originates.
|*region_1*|VARCHAR|**YES**|Region where the wine originates.
|*region_2*|VARCHAR|**YES**|Second region from which the wine originates.
|*variety*|VARCHAR|NO|The variety of the wine, e.g. Chardonnay, Malbec
|*winery*|VARCHAR|NO|The winery producing the wine.

The second dataset is very similar to the first one with the following additions:
- Almost everything is of type text and missing values are represented as empty strings, not as NULL. This means some data cleaning is likely needed.
- There are 3 more columns
    * *taster_name* which is the name of the person tasting the name.
    * *taster_twitter_handle* is the twitter user name of the taster.
    * *title* is the name of the review.


To make life easier, always do a select for all the columns needed in the question, look at the returned values, ponder a bit and then try to write the solution.

## Beginner exercise

Unless specified otherwise only `winemag_p1` is used in this section.

1. Find the prices for Spanish, Italian and French wines.

Columns: `country` and `price`

2. Find all top rated wineries. A winery is considered top rated if it has at least one wine which got awarded 95 or more points.

Columns: `winery` and `points`

3. Find all wine varieties which can be considered cheap. A variety is considered cheap if the price of a bottle is between 5 and 20 dollars.

Columns: `variety` and `price`

4. Find all provinces which have at least one wine missing designation and are named same as the country.

Columns: `province`, `designation` and `country`


5. Using the dataset `winemag_p2` find out which varieties were tasted by 'Roger Voss'. In your answer consider only those tastings which have a designated region.

Columns: `variety`, `taster_name` and `region_1`


## Intermediate exercises

1. Find the all possible varieties which occur in either of the datasets.

Hint: Use UNION.

Columns: `variety`


2. Find all wines which have an aroma of plum, cherry, rose or hazelnut. Use the first dataset only.

Columns: `description`


3. Find how many US based wineries have expensive wines (price >= 200). Use only the first dataset.

Columns: `winery`, `country` and `price`


4. Find how many wines of each variety were tasted by each taster.

Hint: Use a group by.

Columns: `taster_name` and `variety`


5. Find the minimum, average and maximum price of all wines. Use both datasets.

Hint: Cleanup `winemag_p2` before union-ing it with the first one `winemag_p1`. Type casting is ok to use. Using a subquery is ok.

Columns: `country` and `price`


6. Find the total possible profit of each winery by variety with the condition that each wine produced in the variety must have at least 90 points. The profit is defined as the sum of all tasted wine prices.

Columns: `winery`, `variety`, `price` and `points`

Hint: Use a HAVING clause.


7. Find all Bodegas outside Spain which produce wines which have taste of blackberries. Now using this data, find the total number of unique wineries per country and region.

Columns: `country`, `region_1`, `winery`, and `description`


8. Order the list of wines by their points to price ration. Use the dataset `winemag_p2`. Which is the wine for which you get the best bang for your buck?

Hint: Use LIMIT.

Columns: `price` and `points`


9. Find the profit each region made for each variety.

Columns: `region_1`, `variety` and `price`

10. Rank US provinces by the number of wines which contain the keyword
balanced in their keyword. Use only the first dataset (`winemag_p1`).

Hint: Use the SUM - CASE 0/1 trick.

Columns: `country`, `description` and `province`

11. How many wines have designations? How many don't? What's the grand total? Make a report by country.

Columns: `country` and `designations`


12. Which countries are present in `winemag_p1` but not in `winemag_p2`?

Columns: `country`

Hint: Use EXCEPT

## Advanced exercises

1. Find each taster's favorite wine variety. Favorite means that they tasted wines of that variety the most.

Columns: `taster_name` and `variety`

Hint: Don't be afraid to use multiple subqueries and joins to solve this question. One of the subqeries will occur twice in the solution.


2. Find all provinces which produced more wines in `winemag_p1` than they did in `winemag_p2`.

Columns: `province`

3. Find the year for all Macedonian wines from `winemag_p2`

Hint: Use the `split_part` function.

Columns: `title`

4. Find all wines from the second dataset which are produced in the country
which has the highest sum points from the first dataset.

Hint: Use a subquery.

Columns: `country` and `points`

5. Find the cheapest and the most expensive variety per region.

Hint: Use pivot table techniques.

Columns: `region_1`, `price` and `variety`

6. Find the top 3 wineries for each country by the average points their wines earned. Use only `winemag_p1`. When there is no second best winery have the output be 'No second winery.' Similar for the case when there is no third best.

Output table has the following format:
|country|top_winery|second_winery|third_winery|
|-------|----------|-------------|------------|
Albania | ArbÌÇri (88) | No second winery. | No third winery |
Argentina | LTU (94) | Bodega Rolland (94) | Bodega Maestre (92)
Australia | Standish (97) | Campbells (95) | Hickinbotham (95)

Columns: `country`, `winery` and `points`

Hint: Use pivot table techniques and the ROW_NUMBER() function with partitions.

7. Find the median price for each wine variety using the union of both datasets.

Columns: `variety` and `price`

Hint: Use NTILE with partitions.

8. Find all wine designations produced in English speaking regions but not produced in Spanish speaking regions. Use the second dataset. The designations under consideration must be top notch, that is they must have a minimum score of 90 for all wine tastings. For these varities find the maximum price in the US. Use only the first dataset.
English speaking: England, Canada, Australia, US, New Zealand
Spanish speaking: Peru, Spain, Argentina, Chile

Hint: Use multiple subqueries and an inner join.

Columns: `variety`, `country`, `price` and `points`

9. CHALLENGE: Using only split_part and ILIKE find the year when each wine was made (`title` column). Limit yourself to wines made in 2000 or later. Using this new knowledge find the average points per year. How does the average points score change year over year? With the final result confirm or disprove the hypothesis that older wines are better. We assume more points mean better wines.