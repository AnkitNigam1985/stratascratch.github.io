# SQL Guide

This SQL guide is meant to help you get started with SQL. It's helpful for absolute beginners but better for beginners that need a reference when coding. This guide is adapted from Mode Analytics Intro to SQL which is a great introduction to SQL, however, this guide with the accompanying datasets provide a more hands-on experience that allows you to code live with tools used in industry, All tables found in the Mode Analytics guide are loaded in our databases but we added dozens more to get you better acquainted with SQL and analytics. 

# Basic SQL
### Anatomy of a Basic SQL Query

The anatomy of a SQL query is the same every single time. The clauses (e.g., SELECT, FROM, WHERE) are always in the same order, however, many of the clauses are optional. In this section, I’ll explain the SQL clauses that are always required to pull data as well as a few operators and math operations that help convert raw data into something useful. 

Note: The SQL code examples use the Strata Scratch database and is executable on Strata Scratch through your web browser. I would recommend copying and pasting the code, executing it, and taking a look at the output. 

### At the Bare Minimum: SELECT and FROM

There are two required ingredients in any SQL query: SELECT and FROM — and they have to be in that order.  
Back to SELECT and FROM
SELECT indicates which columns of the table you’d like to view, and FROM identifies the table you want to pull data from.

[image_1]

- In this example, we’re pulling data from a schema called `datasets` and a table called `us_housing_units`. Within the table, we’re interested in the data that are stored in the year, month, and west columns.
- a schema is a logical way to group objects like tables, procedures, views. Think of a schema as a container for your database tables.

Note that the three column names were separated by a comma in the query. Whenever you select multiple columns, they must be separated by commas, but you should not include a comma after the last column name.
If you want to select every column in a table, you can use `*` instead of typing out the column names:

[image_2]

### Column Names

If you’d like your results to look a bit more presentable, you can rename columns to include spaces. For example, if you want the west column to appear as West Region in the results, you would have to type:

[image_3]

- Without the double quotes, that query would read ‘West’ and ‘Region’ as separate objects and would return an error. 
Note that the results will only be case sensitive if you put column names in double quotes. The following query, for example, will return results with lower-case column names.

[image_4]

- West_Region would be returned as west_region since double quotes are missing.
- In practice, you’d want to stick to one naming convention, either “West Region” or west_region. Having a consistent naming convention helps standardize coding practices and makes everyone more efficient when pulling data.

### An Important Tip: Explore the Dataset

Now that you understand the basics of query data from a table, the next step is to query, format, and aggregate data so that it’s useful. What’s difficult is that there are often no way to preview the data in the tables. Before diving too deep in any SQL query or analysis, I will always explore tables before starting to write complex queries. All you need to do is perform what I call a SELECT STAR or

[image_5]

This will allow you to see all the columns and some data in the table so that you better understand the data types and column names before writing any complicated query.

### Slicing Your Data: WHERE 

Now you know how to pull data from tables and even specify what columns you want in your output. But what if you’re interested only in housing units sold in January? The WHERE clause allows you to returns rows of data that meet your criteria. 

The WHERE clause, in this example, will go after the FROM clause. In the WHERE clause you need to write a logical operator. For example, if you’re interested in pulling data from month 1, simple write `month = 1` in the WHERE clause.

[image_6]

- Note that `month` is a column in the table and the months are represented by numbers. Remember to do a `SELECT * ` to explore the table before writing your queries.

### Controlling the Output: LIMIT

The LIMIT clause is optional and is used to control the number of rows displayed in the output. The LIMIT clause goes at the very end of your SQL query. I find this clause useful when exploring data.  The following syntax limits the number of rows to only 100:

[image_7]

# Intermediate SQL 1

### Super Powering the WHERE Clause

The WHERE clause is powerful. You can leverage operators and mathematical operations to slice your data into different views. In addition, you can chain together all these operators to efficiently narrow in on the data.

### Comparison Operations on Numerical Data

The most basic way to filter data is to use comparison operators. The easiest way to understand them is to start by looking at a list of them:
- Equal to                        =
- Not equal to                    <>  or  !=
- Greater than                    >
- Less than                       < 
- Greater than or equal to        >=
- Less than or equal to           <=

These comparison operators make the most sense when applied to numerical columns. For example, let’s use > to return only the rows where the West Region produced more than 30,000 housing units

[image_8]

- The SQL query is saying, select all the data located in the schema `datasets` and in the table `us_housing_units` where the `west` column (i.e., the west region) has values greater than 30,000. 
- The SQL query will then go through the `west` column and look for values greater than 30,000 then output all the rows in the table where west > 30000. 

### Comparison Operators on Non-Numerical Data

Some of the above operators work on non-numerical data as well. `=` and `!=` make perfect sense — they allow you to select rows that match or don’t match any value, respectively. For example, run the following query and you’ll notice that none of the January rows show up:

[image_9]

### More Operators to Super Power the WHERE Clause

Here’s a list of additional logical operators to use in the WHERE clause: 

[image_10]

### LIKE

LIKE is a logical operator that allows you to match on similar values rather than exact ones.

[image_11]

- The LIKE operator will look for values that start with Snoop. The % symbol is a wildcard (explained below) and ignores any value coming after Snoop. 
- Note that the LIKE operator is case sensitive so `Snoop` and `snoop` are different when using LIKE. 

### Wildcards and ILIKE

The % used above represents any character or set of characters. In this case, % is referred to as a “wildcard.” LIKE is case-sensitive, meaning that the above query will only capture matches that start with a capital “S” and lower-case “noop.” To ignore case when you’re matching values, you can use the ILIKE command:

[image_12]

- In this case, using ILIKE allows you to be case insensitive. `Snoop` is the same as `snoop` according to ILIKE.

You can also use _ (a single underscore) to substitute for an individual character:

[image_13]

- In this case any alphanumeric value can take the place of the `_` symbol. We’re obviously looking for Drake but this query will catch any misspellings in the `a` portion of his name (e.g., drbke)

### IN

IN is a logical operator in SQL that allows you to specify a list of values that you’d like to include in the results. 

[image_14]

- Here I’m only interested in data where year_rank is 1 or 2 or 3. 

As with comparison operators, you can use non-numerical values, but they need to go inside single quotes. Regardless of the data type, the values in the list must be separated by commas. Here’s another example:

[image_15]

The output here should only yield data corresponding to artists named Taylor Swift or Usher or Ludacris.

### BETWEEN

BETWEEN is a logical operator in SQL that allows you to select only rows that are within a specific range. It has to be paired with the AND operator, which you’ll learn about in a later.

[image_16]

- Here I’m only interested in data where year_rank is 5 to 10. 
- Between` is inclusive so the year_rank can include 5 and 10 (i.e., not 6 to 9). 

### IS NULL

IS NULL is a logical operator in SQL that allows you to exclude rows with missing data from your results.
Some tables contain null values—cells with no data in them at all. This can be confusing for heavy Excel users, because the difference between a cell having no data and a cell containing a space isn’t meaningful in Excel. In SQL, the implications can be pretty serious. 

[image_17]

- IS NULL will only catch cells with no data. A space is considered data so IS NULL will not catch any cell with a space. Be mindful of that when exploring your dataset.

### AND

AND is a logical operator in SQL that allows you to select only rows that satisfy two conditions.

[image_18]

You can use AND with any comparison operator, as many times as you want. If you run this query, you’ll notice that all of the requirements are satisfied.

[image_19]

- This query will return all data in the `billboard_top_100_year_end` table for the year 2012, year_rank is less or equal to 10, and where the group has the word `feat` (i.e., Top 10 song collaborations in 2012).

### OR

OR is a logical operator in SQL that allows you to select rows that satisfy either of two conditions. It works the same way as AND, which selects the rows that satisfy both of two conditions.

[image_20]

- The query will return all data where the year_rank is 5 or the artist is named Gotye regardless of his year_rank. 

### NOT

NOT is a logical operator in SQL that you can put before any conditional statement to select rows for which that statement is false.

[image_21]

[image_22]

### Sorting Data: ORDER BY

Once you’ve learned how to filter data, it’s time to learn how to sort data. The ORDER BY clause allows you to reorder your results based on the data in one or more columns. First, take a look at how the table is ordered by default:

[image_23]

[image_24]

You’ll need to specify whether you want the data to be displayed in ascending order or descending order. 

[image_25]

Will output data alphabetically by artist

[image_26]

Will output data reverse alphabetically by artist

# Intermediate SQL 2

Sometimes you’re not necessarily interested in an output of all the data. Your question that you’re trying to answer is simpler like `how many rows are in this table?` or `how many top 10 songs did Usher produce in 2012?`. In these cases, we don’t want a list of values but would rather have one value — the answer.  You can process data in the SELECT clause. 

## AGGREGATIONS IN THE SELECT CLAUSE
### Counting all rows

COUNT is a SQL aggregate function for counting the number of rows in a particular column. COUNT is the easiest aggregate function to begin with because verifying your results is extremely simple. 

[image_27]

Important note: count(*) also counts the null values. If you want to exclude null values, refer below.

### Counting individual columns

Things start to get a little bit tricky when you want to count individual columns. The following code will provide a count of all of rows in which the high column does not contain a null.

[image_28]

- Note that by specifying the name of the column `high` in `count()`, the query will ignore any nulls in the `high` column and only count the rows containing values. 

### SUM 

SUM is a SQL aggregate function that totals the values in a given column. Unlike COUNT, you can only use SUM on columns containing numerical values.

[image_29]

### MIN and MAX

MIN and MAX are SQL aggregation functions that return the lowest and highest values in a particular column.

[image_30]

### AVG

AVG is a SQL aggregate function that calculates the average of a selected group of values. It’s very useful, but has some limitations. First, it can only be used on numerical columns. Second, it ignores nulls completely.

[image_31]

### Simple Arithmetic in SQL

You can perform arithmetic in SQL using the same operators you would in Excel: +, -, *, /. However, in SQL you can only perform arithmetic across columns on values in a given row. To clarify, you can only add values in multiple columns from the same row together using +.

[image_32]

The output will contain as many rows as are in the table. Only west and south will be added together on a row level. 

[image_33]

# Intermediate SQL 3
### GROUP BY

SQL aggregate functions like COUNT, AVG, and SUM have something in common: they all aggregate across the entire table. But what if you want to aggregate only part of a table? For example, you might want to count the number of entries for each year.

In situations like these, you’d need to use the GROUP BY clause. GROUP BY allows you to separate data into groups, which can be aggregated independently of one another. 

The GROUP BY clause always comes towards the end of the SQL query. It technically goes after the WHERE clause but if the WHERE clause is missing then the GROUP BY will come after the FROM clause (or JOIN clause, but we haven’t learned that yet). 
You’ll know which column name to include in the GROUP BY because it’s listed in the SELECT clause. You only want to include column names that are not being operated on in the GROUP BY clause. In the example below, you do not add COUNT(*) in the GROUP BY because COUNT is an operator. 

[image_34]

- The query will output a count of all the rows by year
- You only add `year` in the GROUP BY because you want to split the COUNT by year.

[image_35]

- You add both year and month in the GROUP BY because you’re interested in the row count by year and month.

### GROUP BY with ORDER BY

The order of column names in your GROUP BY clause doesn’t matter—the results will be the same regardless. If you want to control how the aggregations are grouped together, use ORDER BY. Try running the query below, then reverse the column names in the ORDER BY statement and see how it looks:

[image_36]

### HAVING

However, you’ll often encounter datasets where GROUP BY isn’t enough to get what you’re looking for. Let’s say that it’s not enough just to know aggregated stats by month. After all, there are a lot of months in this dataset. Instead, you might want to find every month during which AAPL stock worked its way over $400/share. The WHERE clause won’t work for this because it doesn’t allow you to filter on aggregate columns—that’s where the HAVING clause comes in:

[image_37]

- The HAVING clause always comes after the GROUP BY but before the ORDER BY clauses.
- It might be more intuitive to use a WHERE clause rather than HAVING clause in this query but you’re not allowed to process or aggregate data in the WHERE clause. This is due to the order of operations when the CPU performs the SQL query. The SQL query is processed by first processing the SELECT, FROM, and GROUP BY clauses. From that dataset, the HAVING clause will act on the data and remove any stock prices below 400. Lastly, the SQL query will order the data by year and month as indicated by the ORDER BY clause. 

# Intermediate SQL 4
### DISTINCT

You’ll occasionally want to look at only the unique values in a particular column. You can do this using SELECT DISTINCT syntax.

[image_38]

- Outputs all the distinct values in the month column of the table.
DISTINCT is handy when you want to count the number of unique values in a column (e.g., distinct months or distinct users). 

### CASE STATEMENT

The CASE statement is SQL’s way of handling if/then logic. The CASE statement is followed by at least one pair of WHEN and THEN statements—SQL’s equivalent of IF/THEN in Excel. It must end with the END statement. The ELSE statement is optional, and provides a way to capture values not specified in the WHEN/THEN statements. CASE is easiest to understand in the context of an example:

[image_39]

- The case statement in this example will output a `yes` value for any year with a `SR` value. If the row does not have a `SR` value, the output is `no`. 

#### Adding multiple conditions to a CASE statement

[image_40]

- You can add as many conditions by adding multiple WHENs. 
- There can only be one ELSE statement which is always last in your CASE WHEN. 

# Advanced SQL
### JOINS

Up to this point, we’ve only been working with one table at a time. The real power of SQL, however, comes from working with data from multiple tables at once.

To understand what joins are and why they are helpful, let’s think about Twitter.

Twitter has to store a lot of data. Twitter could (hypothetically, of course) store its data in one big table in which each row represents one tweet. There could be one column for the content of each tweet, one for the time of the tweet, one for the person who tweeted it, and so on. It turns out, though, that identifying the person who tweeted is a little tricky. There’s a lot to a person’s Twitter identity—a username, a bio, followers, followees, and more.

In an organization like this, Twitter will have hundreds of tables, each storing some attribute about the user, tweet, and/or action. If we just have a user table and tweet table, you can store data like this —the first table—the users table—contains profile information, and has one row per user. The second table—the tweets table—contains tweet information, including the username of the person who sent the tweet. By matching—or joining—that username in the tweets table to the username in the users table, Twitter can still connect profile information to every tweet.

Here’s an example using a different dataset:

[image_41]

Can you guess what the query is trying to achieve? We’ve covered all aspects of the SQL query except for the JOIN clause.

- In this query, the SELECT clause tells us what information is going to be displayed. We’re interested in the average weight of college football players by conference.

### JOIN clause

In the example above, the JOIN clause joins the `college_football_players` and `college_football_teams` tables together, presumably so that we can link player attributes with team attributes.
But how do we JOIN two tables together? The key is the `ON` clause. With the `ON` clause, you’re selecting a column in one table and matching it with a column in another table. In this case, we’re taking `school_name` from `datasets.college_football_teams` and matching it with `school_name` from `datasets.college_football_players`.

### Deconstructing the JOIN clause 

Let’s take only the FROM and JOIN clauses from the example above:

[image_42]

- `FROM`: pick a table to start
- You can nickname the table for easy reference. In this case, I’ve named the `datasets.college_football_players` table as `players`. 
- I use `players` in the `ON` clause so that I don’t have to type the name of the entire table again (`datasets.college_football_players`). 
- `JOIN`: pick the table you’re interested in joining. Just like with the `FROM` clause, you can nickname the table. In this case, I nicknamed the table  `datasets.college_football_teams` as `teams`. 
- The `ON` clause matches columns from both tables so that the tables can join together. In this case, I’m matching `school_name` from both tables. 

### INNER JOIN

In the football example above, all of the players in the `players` table match to one school in the `teams` table. But what if the data isn’t so clean? What if there are multiple schools in the `teams` table with the same name? Or if a player goes to a school that isn’t in the teams table?

If there are multiple schools in the `teams` table with the same name, each one of those rows will get joined to matching rows in the `players` table. For example, if there was a player named `Michael Campanaro` and if there were three rows in the `teams` table where `school_name = 'Wake Forest'`, an inner join would return three rows with `Michael Campanaro`.

It’s often the case that one or both tables being joined contain rows that don’t have matches in the other table. The way this is handled depends on whether you’re making an inner join or an outer join.

We’ll start with inner joins, which can be written as either `JOIN datasets.college_football_teams teams` or `INNER JOIN datasets.college_football_teams` . Inner joins eliminate rows from both tables that do not satisfy the join condition set forth in the `ON` statement. In mathematical terms, an inner join is the intersection of the two tables.

[image_43]

Therefore, if a player goes to a school that isn’t in the `teams` table, that player won’t be included in the result from an inner join. Similarly, if there are schools in the `teams` table that don’t match to any schools in the `players` table, those rows won’t be included in the results either.

### OUTER JOINS

When performing an inner join, rows from either table that are unmatched in the other table are not returned. In an outer join, unmatched rows in one or both tables can be returned. There are a few types of outer joins — LEFT JOIN, RIGHT JOIN, and FULL OUTER JOIN.

[image_44]

### LEFT JOIN

[image_45]

Let’s start by running an INNER JOIN on the `Crunchbase` dataset and taking a look at the results. We’ll just look at `company-permalink` in each table, as well as a couple other fields, to get a sense of what’s actually being joined.

[image_46]

You may notice that “280 North” appears twice in this list. That is because it has two entries in the `datasets.crunchbase_acquisitions` table, both of which are being joined onto the `datasets.crunchbase_companies` table.

Now try running that query as a LEFT JOIN:

[image_47]

You can see that the first two companies from the previous result set, `waywire` and `1000memories`, are pushed down the page by a number of results that contain null values in the `acquisitions_permalink` and `acquired_date` fields.

This is because the LEFT JOIN command tells the database to return all rows in the table in the FROM clause, regardless of whether or not they have matches in the table in the LEFT JOIN clause.

### RIGHT JOIN

Right joins are similar to left joins except they return all rows from the table in the RIGHT JOIN clause and only matching rows from the table in the FROM clause.

[image_48]

RIGHT JOIN is rarely used because you can achieve the results of a RIGHT JOIN by simply switching the two joined table names in a LEFT JOIN. For example, in this query of the Crunchbase dataset, the LEFT JOIN section:

[image_49]

produces the same results as this query:

[image_50]

The convention of always using LEFT JOIN probably exists to make queries easier to read and audit, but beyond that there isn’t necessarily a strong reason to avoid using RIGHT JOIN.

It’s worth noting that LEFT JOIN and RIGHT JOIN can be written as LEFT OUTER JOIN and RIGHT OUTER JOIN, respectively.

### FULL OUTER JOIN

LEFT JOIN and RIGHT JOIN each return unmatched rows from one of the tables—FULL JOIN returns unmatched rows from both tables. It is commonly used in conjunction with aggregations to understand the amount of overlap between two tables.

Here’s an example using the `Crunchbase companies and acquisitions` tables:

[image_51]

### UNION

SQL joins allow you to combine two datasets side-by-side, but UNION allows you to stack one dataset on top of the other. Put differently, UNION allows you to write two separate SELECT statements, and to have the results of one statement display in the same table as the results from the other statement.

Let’s try it out with the Crunchbase investment data, which has been split into two tables for the purposes of this lesson. The following query will display all results from the first portion of the query, then all results from the second portion in the same table:

[image_52]

Note that UNION only appends distinct values. More specifically, when you use UNION, the dataset is appended, and any rows in the appended table that are exactly identical to rows in the first table are dropped. If you’d like to append all the values from the second table, use UNION ALL. You’ll likely use UNION ALL far more often than UNION. In this particular case, there are no duplicate rows, so UNION ALL will produce the same results:

[image_53]

SQL has strict rules for appending data:
- Both tables must have the same number of columns
- The columns must have the same data types in the same order as the first table

While the column names don’t necessarily have to be the same, you will find that they typically are. This is because most of the instances in which you’d want to use UNION involve stitching together different parts of the same dataset (as is the case here).

Since you are writing two separate SELECT statements, you can treat them differently before appending. For example, you can filter them differently using different WHERE clauses.

