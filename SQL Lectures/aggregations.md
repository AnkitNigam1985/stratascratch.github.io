# The big picture

People tend to collect all sorts of data with a lot of details. The tables have many entities, sometimes even millions, and it becomes rather tedious, downright impossible for a human to analyze each entity. To overcome these problems and still find some useful information from the sea of data we have aggregation functions. 

Take for example the `sum` aggregations. It will turn the list of profits per day into monthly profit, or a list of profits per month into yearly profits. Notice how the time scale changes from days to months and months to years revealing the bigger picture. 

Unlike in this example aggregations don't have to happen over time, they can happen over space, think village -> city -> country, or they can happen over people, e.g. employee -> manager -> C-suite. They can also happen over nothing and be global aggregations, for example, finding the total profit for all companies since day 0.

 In this lesson we will still use our tried and trusted friend `titanic` but we will also get to play with the SAT scores dataset under the table `sat_scores`.

## Global aggregations

Let's start with a motivating example: "How many passengers survived?"

Copy the following query in the editor and run it:

```sql
SELECT
    count(*) AS n_survivors
FROM datasets.titanic
WHERE
    survived = 1 
```

You should get the number **342** as the answer.

The function `count` is the simplest aggregation. It tells us the number of entities which are part of our table after filtering, in this case after keeping only survivors. The keyword `AS` is used to give another name to a column in our output table. By default the name of the column produced by `count` is *count*. We often use `AS` to provide more descriptive names like `n_survivors`.

### The keyword `distinct`

The main problem which can arise in counting is how to handle repeating cases. 

Take for example the list called `numbers`: `1, 1, 1, 5, 8, 1`

It has 6 numbers in it but only 3 distinct ones (1, 5, 8), thus we say that in this case `count(numbers)` is 6, but if we were to use the `distinct` keyword we will get 3, like this: `count(distinct numbers)`

Here is another list called `sex: `'male', 'male', null, 'female', null, 'female'`

Using just `count(sex)` we would get 6 but using `count(distinct sex)` we will get 3. NULL is considered a separate value and is counted in both cases. 

To get things even more practical consider the following question with it's associated query:

"How many cabins were on the titanic and how many passengers?"

```sql
SELECT
   count(distinct cabin) AS n_unique_cabins,
   count(cabin) AS n_passengers
FROM datasets.titanic
```

The output is:

|n_unique_cabins|n_passengers|
|---------------|------------|
|147|204|

To see why these numbers are different notice that a single cabin can hold multiple passengers.

### Sums

Because example is king let's jump straight into one: "What is the total money paid for fares by 1st class passengers?"

Words like total almost always mean a sum. The query thought process applies here too albeit in an extended fashion. We see that we need the columns `fare` and `pclass`. We need to keep only `pclass = 1` cases and we need to sum over all fares for 1st class passengers. All of this combined forms the query:

```sql
SELECT
  sum(fare) AS total_sales
FROM datasets.titanic
WHERE
    pclass = 1
```

We can see that first class passengers paid `18177` pounds to board the Titanic. What about 2nd and 3rd class passengers? What do you need to change in the query to find that information?

**NB**: Unlike count, `sum` works only over numeric data.

### Averages

The question we have now is "What was the average age of passengers who died in the accident?"

To solve our question we have the function `avg` to help us. It is used in a similar manner as both `count` and `sum`.

```sql
SELECT
    avg(age) AS average_age
FROM datasets.titanic
WHERE
    survived = 0
```

The answer is around `30.5` 

If you don't know what averages are then consider the following examples:
- avg(1, 3) = 2 by the formula (1 + 3) / 2
- avg(3, 7, 15) = (3 + 9 + 15) / 3 = 9
- avg(5, 5, 5) = (5 + 5 + 5) / 3 = 5
- In essence the average is a measure of what is the central number among the given list of numbers. 

**NB**: Unlike count, `avg` works only over numeric data.

### Min and max

There are 5 aggregation functions in SQL: `count`, `sum`, `avg`, `min` and `max`. We have learned the first 3 and now we need to focus on `min` and `max`. Minimum of a list is the smallest number in that list while maximum of a list is the largest number in a list. 

For example consider the list: `4, 7, 2, 11, 6`.
- The minimum is 2
- The maximum is 11

If you remember `ORDER BY` from lecture 1 you will notice that `min` is related to `ORDER BY ASC` and `max` is related to `ORDER BY DESC`. Usually we prefer to use `ORDER BY` over `min` and `max` but we can't always do that so `min` and `max` were introduced to SQL.

Consider the question: "What is the highest fare paid and what is the lowest fare paid?"

The answer lies with:

```sql
SELECT
    min(fare) AS lowest_fare,
    max(fare) AS highest_fare
FROM datasets.titanic
```

Notice that `ORDER BY` would force us to either find the lowest or highest fare while we can get both by using aggregation functions. What we can't find using aggregation functions is who paid the lowest fare or highest fare and that is where `ORDER BY` proves to be more useful. The conclusion is that all tools have their use and none is studied in vain.

## Aggregations per groups

Global aggregations are ok to get the first impression but they tell us very little about the factors forming the outcome we are seeing. To drill deeper in search for our data oil we need to consider how the output depends on some group of interest. Groups can be anything and it might take some time to understand that some values can form a group and that behind that group lies some value of interest. 

Here are some example groups: 
- `'male','female', null` (3 groups)
- 1st, 2nd, 3rd class (3 groups)
- age 1, age 2, age 3, ..., age 100 (100 groups)
- 'Henry', 'William' (2 groups)
- Binary columns like `survived` also form a group: 0, 1 (2 groups) 

Groups by themselves are not very interesting. The aggregate value of some column when measured per group is what is we seek. Take for example the total sales per gender, the number of surviours per passenger class, the average age of survivors and non-survivors and many more questions.

To take aggregations per groups we need to tell our query that we want to make a group over some column or even over multiple columns. For starters let's focus on a single column and on the question: "What are total sales per gender?"

```sql
SELECT
    sex,
    
    sum(fare) AS total_sales
FROM datasets.titanic
GROUP BY 
    sex
```

The output we get is:

|sex|total_sales|
|---|-----------|
|male|14727.2865|
|female|13966.6628|

It means that male passengers yielded 14727 pounds while female passengers yielded 13966 pounds.

The main changes we have compared to our global total sales query is the `GROUP BY` part. After the **2** keywords we put the column name over which we wish to 'split' the table. In our case this is the column `sex`. We also provide the same column in the `SELECT` part so we can see it in the output. This is not strictly necessary but it is a good practice to have it.

Concerning ordering of blocks `GROUP BY` comes after `WHERE` if it is present but before `ORDER BY`.

Here is an example query which uses all 3 of them so you can see the correct ordering:

```sql
SELECT
    sex,
    
    sum(fare) AS total_sales
FROM datasets.titanic
WHERE
    survived = 1
GROUP BY sex
ORDER BY 
    total_sales DESC
```

What do you think is the question behind this query?

### Common problems

Before going any further I'd like you to be aware of the following common problems which happen when being new to group aggregations:
- There are two keywords `GROUP` and `BY`, never `GROUPBY`.
- The only columns you are allowed to use outside of aggregation functions are the ones present in your `GROUP BY` block. For example in our previous query we had `sex` in our group by and that is the only column we were allowed to use on its own. `sum(fare)` is the aggregation and aggregations can use any column. If you try to use another column like for example `age`, this is the error you will get: `column "titanic.age" must appear in the GROUP BY clause or be used in an aggregate function LINE 4: age, ^`
- Types matter and you can't do `sum` or `avg` over text. Here is an example error: `function sum(character varying) does not exist LINE 8: sum(name) ^ HINT: No function matches the given name and argument types. You might need to add explicit type casts.`

As a friendly suggestion please follow the formatting we follow in our queries. Not following it will not cause errors but if you get an error somewhere having your query nicey typed will allow you to find the cause of it faster.

### All 5 functions

You can use all 5 functions when doing aggregations per groups. You can use multiple of them in a same query and you can use a single one multiple times.

Here is an example where we do a mini statistical report on earnings per passenger class.

```sql
SELECT
    pclass,
    
    count(*) AS n_passengers, 
    sum(fare) AS total_sales,
    avg(fare) AS average_fare_paid,
    min(fare) AS minimal_fare_paid,
    max(fare) AS maximal_fare_paid
FROM datasets.titanic
GROUP BY
    pclass
ORDER BY 
    total_sales DESC
```

### Multiple group by columns

We will extend our mini report to also include information about passenger sex.

```sql
SELECT
    pclass,
    sex,
    
    count(*) AS n_passengers, 
    sum(fare) AS total_sales,
    avg(fare) AS average_fare_paid,
    min(fare) AS minimal_fare_paid,
    max(fare) AS maximal_fare_paid
FROM datasets.titanic
GROUP BY
    pclass,
    sex
ORDER BY 
    pclass ASC
```

Notice the block:

```sql
GROUP BY
    pclass,
    sex
```

When using multiple columns we must separate them with a comma with the last entry having no comma after it.

### Sums of binary variables

Binary variables are those who have only two possibles values, 0 and 1. Zero usually means something is not present while one usually means the opposite, that something is present. In the `titanic` table we have the `survived` column which is binary, it is either 0 meaning the passenger died or 1 meaning the passenger survived.

When we sum binary variables we get the number of rows for which the condition is true, for example the number of passengers who survived. This stems from the following basic principle: sum(0, 0, 1, 0, 1) = 0 + 0 + 1 + 0 + 1 = 2 which means that 2 out of 5 are 1s while others are 0s. In the titanic case this could mean that 2 out of 5 survived. 

The following query uses `sum` over the `survived` column to find the number of survivors.

```sql
SELECT
    sum(survived) AS n_survivors,
    count(*) AS n_passengers
FROM datasets.titanic
```

We can also get the number of survivours by taking `count` after filtering for `survived = 1`. In some cases this is a possibility but as we get more advanced we will need to use the sum of binary values concept to solve some tasks which are not possible with the methodology filter followed by `count`.

## More on distinct

The keyword `distinct` is not specific to aggregation functions and can be used in regular queries to obtain only unique output rows. The statement `distinct on` can be used to obtain unique rows conditioned on some column.

For example: `Find all unique cabins`

```sql
SELECT
    distinct cabin
FROM datasets.titanic
```

Consider the following text book example: "Find names such that each name must be of a person whose age is unique."

```sql
SELECT
    distinct on(age)
    
    name, 
    age
FROM datasets.titanic
ORDER BY 
    age ASC
```

## Examples

By this time you are probably bored of `titanic` so to liven the atmosphere a bit we will work with `sat_scores` till the end of this lesson.

Here are some example queries and you should try to think of the questions lying behind them:

```sql
SELECT
    teacher,
    min(sat_math) AS min_math_score,
    max(sat_math) AS max_math_score,
    max(sat_math) - min(sat_math) AS math_score_range
FROM datasets.sat_scores
GROUP BY
    teacher
```

**NB**: It is ok to do arithmetics like `+`, `-`, `*` or `/` between aggregations just as it is allowed between regular columns.

```sql
SELECT
    school,
    count(*) AS n_students,
    sum(sat_verbal) / count(*) AS average_verbal_score
FROM datasets.sat_scores
GROUP BY
    school
```

Here we have implemented our own version of `avg` using `sum` and `count`.

## Try it yourself

Try answering the following questions:
- What is the minimal number of hours studied per Teacher? (Hint: use min)
- How many teachers does each school have? (Hint: use count distinct)

## Conclusion

Many concepts were covered in this lesson so if you are unsure about something it is a good idea to revisit some section, consult the Basic SQL Guide or write in the forum if you'd like to discuss with us. Best way to learn is through practice. Try answering the questions from the basic difficulty tier and honing your skills. Next time we will learn about filtering inside groups and the `HAVING` clause, both of which build upon knowledge obtained today. 