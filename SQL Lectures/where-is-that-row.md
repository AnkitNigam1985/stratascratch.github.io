# SQL Lecture 2 - Filtering Fantasy

## More ways to focus on what matters

In our previous lecture we introduced the basic parts of a query giving filtering only a single paragraph. Filtering is a bigger topic and this lesson will teach you some more ways to slice and dice your tables so you can extract useful knowledge from them. We will learn about conditions, propositional logic and ways to filter our data on columns which contain textual information. 

## Conditions and Operators

Everything of the form `column_name <operator> <value>` or `column_name <operator> other_column_name` is called a condition. Here are some conditions:
- `survived = 1`
- `pclass   = 1`
- `sex      = 'male'`

`survived`, `pclass` and `sex` are columns, `=` is an operator and `1`, `1` and `'male'` are values.

Any column can be used as a condition, as long as it part of a table of course. The list of operators is fixed while the kind of values you can use depends on the data type. For now we will learn about number and text data types while more advanced cases will be explained in a future lecture.

### Operators

The most basic operator is equals (`=`), which checks if left and right side are equal. This operator can be used for both numbers and text.

Operators `<` and `>` mean strictly less than and strictly more than. The behavior is same as in mathematics. These operators make sense on numbers. They also make sense on text but for now assume `<` and `>` don't work for text even if they do. The behavior on textual data is explained in a future lecture.

You can also use `<=` and `>=` which are less or equal and more or equal. 

The final operator is `<>` which is not equal. It might seem weird and it is, but because keyboards don't have an easy way to type the ≠ character the people making SQL thought that `<>` is the best way to represent the not equals intent.

### Numbers and text

Number constants are typed with number characters, for example 1, 5, 111, 12157751 are all valid numeric constants in SQL.

Text constants are written in **single** quotes, for example 'male', 'C85', '13'.

'13' and 13 are not equal, one is text and other is number.

I have bolded the word single because using double quotes will give you an error like the following: `column "male" does not exist LINE 7: sex = "male" ^`

In SQL columns like `sex`, `name`, `age` can be written as either sex, name, age or as "sex", "name", "age", that is double quoting a text means that you want a column with such name. Because a column `male` does not exist using "male" will give you an error.

When speaking of text it is also important to notice that 'BIG' is different from 'big', that is `=` and `<>` are dependent on case.

### Examples

Here is an example where you see how we can compare values from two column.

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    survived = pclass
```

What do you think is the result of this and why?

Here is another example where we compare textual column with a value using `<>`.

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    embarked <> 'S'
```

## Propositional logic

You might have studied propositional logic during middle or high school or this term might sound totally alien to you. It doesn't matter because we will teach you the 3 keywords which give every `WHERE` statement the power it needs to shine bright. We will start with `AND`, go over `ON` continue with `NOT` and finalize on the ways to combine them to get the juicy part of the data apple.

### Me and you, 1st class passengers

In SQL `AND` has the same meaning as in English language. When we say that a passenger survived, had a 1rd class cabin and was a male we have three conditions of whom all 3 must be true for our output table to include a passenger. The following query gives us all entities which satisfy all these 3 conditions.

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    survived = 1 AND
    pclass   = 1 AND
    sex      = 'male'
```

We can have as many conditions `AND`ed together as we want (`AND`ed is not an English word but you will hear it a lot in the data analytics community, so please bear with us) 

The behavior of `AND` is described with the following table:

|X|Y|X `AND` Y|
|-|-|---------|
|True|True|True|
|True|False|False|
|False|False|False|
|False|True|False|

In essence you will get a row back only if both conditions are true. When you have more than 2 conditions like in our previous example you will get the row back only when **all** conditions are true, that is passenger survived and passenger was in first class and passenger was a male.

### Beginner or expert, everyone learns

In the world of SQL `OR` follows the same meaning as in regular life and mathematics. Some entity is considered fit for being part of our output table if one of the conditions are true. For example if we have the question "We want just 1st class and 3rd class passengers" our conditions would look like:
- `pclass = 1`
- `pclass = 3`

Our query will then look like:

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    pclass = 1 OR
    pclass = 3
```

What do you think would happen if we were to use `AND` instead of `OR` in the query? 

For reference here is the truth table for `OR`:

|x|y|x `OR` y|
|-|-|--------|
|True|True|True|
|True|False|True|
|False|True|True|
|False|False|False|

Thus the only way an entity is not considered fit to be part of the output table is if both conditions are not satisfied.

### IN - A supercharged OR

In the real world it is very unlikely that you will use `OR` often but it's close associate `IN` is used a lot. The meaning of `IN` is essentially the same as the meaning of a few conditions linked with `OR` except that it is easier to write queries with IN.

An example is worthy of a thousand definitions so here is one:

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    pclass IN (1, 3)
```

This query and the query with OR are absolutely identical in the results they will produce.

Here is another example where using `OR` will end in a lot of typing but `IN` handles the issue well. 

Assume the question: "Find all passengers whose age is exactly a multiple of 10, that is 10, 20, 30, 40, 50, ..."

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    age IN (10, 20, 30, 40, 50, 60, 70, 80, 90)
```

The alternative is to do: `age = 10 OR age = 20 OR age = 30 OR age = 40` etc.

### BETWEEN two worlds

This operation is similar to `IN` with the main exception that `IN` takes a discrete set of values to check against while `BETWEEN` takes a continous range. Perhaps an example will clarify this best:

"Who are the passengers who have paid a fare between 10$ and 20$?"

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    fare BETWEEN 10 AND 20
```

As you can see the translation from English language went really smooth with our only change being that we don't use the dollar sign in our query. Keep in mind that this `AND` here is related to the `AND` from before by the relation:

`fare BETWEEN 10 AND 20` is same as `fare >= 10 AND fare <= 20`

It is prefered to use `BETWEEN` because it is easier to read the query that way.

### NOT the end

Negation of True is False and negation of False is True. Negating constants is not very useful but negating variables and expressions can be very useful. Take the following example: "Find all passengers who were not of age 24".

There are two ways to think about this condition:
- `NOT age = 20` (`age <> 20`)
- `age = 1 OR age = 2 OR age = 3 ... OR age = 23 OR age = 25 ... OR age = 100` (notice we skiped 24)

It is obvious which one is easier to work with. In this simple examples it might not seem that important but as you write more complex queries a clever use of `NOT` might make your `WHERE` blocks much shorter while still giving correct results.

Logical negation is the way to link `AND` with `OR` with the following formula holding true: `x OR y = ((NOT x) AND  (NOT y))`

If you stop to ponder for a second you can see that our two ways of thinking about this condition are linked by this formula called De Morgan's law.

## Textual data filtering with LIKE and ILIKE

All questions we were able to ask ourselves had to be based on numbers or on simple equality tests for text. This limitation stops right now when we learn about `LIKE` and it's closely related cousin `ILIKE`.

The beauty of, or horror, of text is that unlike numbers it comes in rather nonuniform structure. Take for example the number 1. When you write as a number it is always 1 but when written as a text it can be 'ONE', 'one', 'ace', 'AcE' or many other possibilities. Take also names, is it Jhon, Jhon or Jon?

To combat these issues `LIKE` and `ILIKE` were developed. `ILIKE` is the case insensitive brother of `LIKE` so everything we know about `LIKE` applies to `ILIKE` too.

In it's most basic form `LIKE` acts exactly as the equals operator (`=`). Here is an example query showing that:

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    sex LIKE 'male'
```

The power of like comes with the characters '_' and '%'.
- '_' means match exactly one character 
- '%' means match characters of any length.

The '%' wild card is used much more so we will focus on it. Here is an illustrative example: "Find all people named John".

We first need to reformulate this question as: "Find all people whose `name` column contains the word 'John'". Next we need to define our matching pattern which will be '%John%'. This means whatever before John and whatever after John we only care that Jhon must exist there somewhere. When combining all of this we get the following query:

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    name LIKE '%John%'
```

Consider another example: "Find all married passengers?"

First we define maried as having their `name` column contain either 'Mr.' or 'Mrs.'. Then we define two patterns '%Mr.%' and '%Mrs.%' and we `OR` them together.

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    name LIKE '%Mr.%' OR
    name LIKE '%Mrs.%'
```

Here is an problem for you to solve: "Find all females with two names." Use only LIKE and a single pattern.
Hint: The second name is wrapped in brackets like 'Nasser, Mrs. Nicholas (Adele Achem)'

## NULL and IS NULL

Data collection is not a flawless process and very often certain information is missing be that for privacy reasons, laws, faulty procedures or broken sensor equipment. Missing information is represented by a special NULL value which applies to both numbers and text.

In our titanic dataset we have missing data in almost all columns, most notably in `cabin` and `age` columns. 

We can ignore NULL values in our queries using the `IS NOT NULL` operation. Here is an example query where we ignore all passengers whose age is not known.

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    age IS NOT NULL
```

## Examples

1. Find all people whose first name is exactly six letters long.

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    name LIKE '______,%'
```

We check for this by using six consecutive '_' symbols.

2. Who is the oldest unmarried female passenger?

```sql
SELECT
    name, age
FROM datasets.titanic
WHERE
    name ILIKE '%Miss%' AND
    age IS NOT NULL
ORDER BY 
    age DESC
LIMIT 1
```

We ignore missing ages here because when ordering NULL is always both lowest and highest at the same time.

3. Operators presentation

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    age BETWEEN 18 AND 30 AND
    name ILIKE '%Henry%' AND
    pclass IN (2, 3) AND 
    (survived <> 0 OR fare > 10)
```

This is a made up query which uses almost all operators in a single query.

The question it answers would go along the lines: "Find all passengers named/surnamed Henry older than 18, younger than 30 years, who were in either second or third class and who either survived or paid a fare greater than 10$"

## Try it yourself

1. Find all survivors who had a numbers only ticket. (Hint: numbers only tickets have no spaces and or of maximum length 7)

You can solve this by checking if it is of length 1 and containing no spaces, is of length 2 and containing no spaces or something else.

2. Find all cabins which are not null and start with a 'B'. What do you notice of passenger class to cabin number relation?

3. Find all males whose age is unknown and who don't have a middle name. (Hint: check that there are not 2 or not 3 spaces in name)
