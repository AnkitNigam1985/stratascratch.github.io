# Basics of a SQL query

## Data

Data is the new oil. It is a resource worth billions whose extractors are highly sought after just as petrochemical engineers were in high demand
during the past century. Just like oil extraction requires precise tools and processes, data requires precise approaches. 

In this set of lectures we set our goal to teach you about the proper way to work with data, the way which will empower you to do more with your (future) business.

 The question what is data is actually surprisingly hard to answer, because unlike oil which is usually a black liquid, data comes in many shapes and sizes. The most common form of data are relational tables, also known by the term data frames. 
 
 Our set of lectures will teach you to understand tables, what kind of questions can you ask when you see a table and how to ask them. We don't teach you just SQL, we teach you how to think in data. You can find us at https://www.stratascratch.com/

## Tables

Tables are a very common concept which you have likely seen many times through your previous education, sometimes drawn on a piece of paper, sometimes digitally in Excel or other spreadsheet software. In our view of data, tables are defined by column names (e.g. `name`, `sex`, `age`), data types like number or date and each table has a name, for example `titanic`. Here is an example of what a table looks like:

|name|sex|age|
|----|---|---|
|Braund, Mr. Owen Harris|male|22|
|Cumings, Mrs. John Bradley (Florence Briggs Thayer)|female|38|
|Heikkinen, Miss. Laina|female|26|
|Futrelle, Mrs. Jacques Heath (Lily May Peel)|female|35|
|Allen, Mr. William Henry|male|35|

This is the table whose name is `titanic` and which has 3 columns: `name`, `sex` and `age`. This table also has 5 rows. Each row represents a single person with their `name`, `sex` and `age`. Rows are sometimes called entities because one row corresponds to some kind of real world entity, be it a human, a corporation, a racing car or a shopping cart product. Thus a table, in its simplest form, is a list of entities. 

Our example table is a list of passengers on the ship `titanic`. When you open the SQL editor on your left you will see two dropdowns. In the first one select `datasets`. After doing that select `titanic` from the second dropdown. A new tab called `Preview for titanic` will open in which you will see the table `titanic`. You will notice there are many more columns than with our example table and that is ok. We can always add or take away columns to tables and they will still be tables. 

Tables organize data and provide it in a form which allows us to ask many questions in our effort to get the answers we desire. A question in the context of SQL is called a query.

## Queries

Before we learn how to make a question against the table we need to think about what questions we can ask and which ones we can't. 

Here are some questions we might ask ourselves about passengers on the titanic:
- How many were male and how many were female?
- Did more women survive the crash?
- What is the mean age of people who died?

There are also questions we can't ask ourselves because we don't have the data present. In the case of titanic:
- How many passengers were British? We can't ask this because we don't know the ethnicity of passengers.
- What is the average height of survivors? We can't ask this question because we don't have data on height present.

Tables are their own self-contained world and we must work with the restrictions they impose on us. This is especially true in the data analytics sector where you don't get to make a choice in table design but get only to read the data from them.

### Query thought process

Tables are lists of entities and that is our mental model on how they function. The computer is not sapient enough to understand our mental model and thus we must get to its level and think of tables in terms of columns and rows. Whenever we ask ourselves some question we must formulate it in terms of columns and rows. 

Take the question "What is the mean age of people who died?". We see that out of all columns we need only `age` and `survived` and out of all passengers we need only those whose `survived` property is equal to 1. In computer science 1 often means *Yes* while 0 means *No*. So `survived = 1` means that the passenger survived.

What are the columns and rows you will need for the other two questions?

For now these two questions are enough but as your progress you will also need to ask yourself what tables do I need to answer my questions. In this exercise there is only a single and that table is always `titanic`. You don't need to bother with this too much for now because this concept is covered in future lessons.

### Simplest query

Copy and paste the following query into the editor and press the blue 'Run Query' button:

```sql
SELECT *
FROM datasets.titanic
```

In the tab `Results` you will get the same table as present in `Preview for titanic`. Notice that every query starts with a table and provides another table as its output. The output table will not have a name but it is still a table.

When you compare the two tabs you will see they are equal. This is not an error. It is intended to look like that. Let's first analyze what this little piece of code does and then you should understand why they are the same.

Starting from bottom we have the `FROM datasets.titanic` line. SQL is modeled after the English language so keywords like `FROM` usually follow some common sense logic. `FROM` means take the entities, in our case passengers, from the `titanic` table. The part `datasets.` in `datasets.titanic` is a technical neccessity with which you should not bother for now, just keep in mind you must always write `datasets.titanic` instead of just `titanic`. 

Ok so we take the entities from the `titanic` table. We must do something meaningful with these entities so we do `SELECT *`. This statement means make a table with these same entities which will have the same columns as the original table. The star (`*`) always means all columns from the input table.

### Projection

The very first query we wrote was rather useless but it has shown us the general format we must always follow, `SELECT` followed by column names or `*` followed by `FROM` followed by our table name.

The first useful operation we will learn is projection. It is a fancy name for selecting only some columns to be present in the output table. Without further ado copy and paste the following query and press the blue 'Run Query' button:

```sql
SELECT
    name,
    sex,
    age
FROM datasets.titanic
```

Now we get a new table which provides a narrower view on our passengers. We don't care if they survived, how much money they spent on their fare and other properties. We only care about their `name`, `sex` and `age`. 

The format we must follow is that wanted column names are separated with a comma (`,`) and the last column name does NOT have a comma after it. If you were to put a comma after `age` in our sample query you would get the following error: `syntax error at or near "FROM" LINE 5: FROM datasets.titanic ^`. Unfortunately SQL syntax errors are rather useless in telling you where exactly is your problem but in 99% of the cases it is either an extra comma, a missing comma, an extra bracket or a missing bracket.

Try projection a different set of columns, for example: `pclass`, `fare` and `cabin`

You will notice that some columns like `pclass` are abbreviations, `pclass` stands for passenger class. Other than typing less characters there is no specific reason not to have the column name be `passenger_class` but don't judge us too harshly. You will too have such funky naming systems once you become a veteran in data analytics and have to work with tens of tables having hundreds of columns :)

### Filtering

From the perspective of the answer we seek by a question not all entities are created equal. In our answer we need only those entities which satisfy some condition on one or more of their properties. Take the question "I want a list of all passengers who were younger than 21 years." Out of all entities we take only those who `age` property is `< 21`. To incorporate filtering into our queries we must use a `WHERE` statement. Try executing the following query:

```sql
SELECT
    *
FROM datasets.titanic
WHERE
    age < 21
```

You see in the `Results` tab that you only get some rows and that in all rows the age is less than 21.

On our basic query we have added an extension after `FROM`. The extension comes in the form of a condition `age < 21`. Don't let the condition being on a new line baffle you. In SQL it is perfectly ok to have as much whitespace and new lines as you want. We format our queries like this for ease of reading access and you will find that all our 300+ question solutions are formatted in such a manner. 

The order of query parts is always `SELECT`, column names, `FROM` tables, optional `WHERE` followed by conditions. Optional means that you have a valid query without it as shown in the previous sections.

### Projection + Filtering

Projection and filtering by themselves are rather useless but when combined they form the backbone of all queries, both the ones you will learn today and the ones powering multibillion companies like Salesforce. 

Let us ask the following question: "What are the fares for passengers in class 3". 

Using our query thought process we first find the columns we need:
- The wanted columns are: `fare` and `pclass`
- We need only passengers which booked a 3rd class cabin thus our rows must satisfy the condition `pclass = 3`

Congratulations. You have just finished 99% of the work by answering these two questions. Everything that's left is to fill in the template.

Here is our every single possible SQL query (for now) template:

```sql
SELECT
    some_columns
FROM some_table
WHERE
    some_condition
```

We just have to fill in the blanks:

```sql
SELECT
    fare
FROM datasets.titanic
WHERE
    pclass = 3
```

Try filtering for other two passenger classes. What do you need to change? If you are not sure start from the first step of query thought process.

### Ordering

Our previous query gave us a bunch of numbers which don't tell us much about the fare structure for 3rd class passengers. Being dilligent data analysts we try to think of ways to have these numbers be more useful to us. Ordering these numbers from lowest to highest will allow us to reason about how the price for 3rd class cabins changes while ordering from highest to lowest will let us think about who possibly paid too much compared to others. 

In case you are not familiar with ordering:
- Ordering 5, 19, 3, 13, 8, 3 from lowest to highest gives: 3, 3, 5, 8, 13, 19
- Ordering the same list from highest to lowest gives: 19, 13, 8, 5, 3, 3

In SQL speak lowest to highest is called ascending, `asc` short, while highest to lowest is called descending, `desc` short.

To order the output rows we need to ask ourselves two questions:
1. By which column do we want to order?
2. Do we want an ascending or descending ordering?

For our fares problem we need the `fare` column. We will try both ascending and descending so we can compare the results.

The template changes with the addition of `ORDER BY` after `WHERE`, two separate words never `ORDERBY`. Try executing the following query:

```sql
SELECT
    fare
FROM datasets.titanic
WHERE
    pclass = 3
ORDER BY
    fare ASC
```

You will see that first few rows are 0s followed by 4 and so on. The numbers are ordered from lowest to highest. What will happen when you use `DESC` instead of `ASC` in the same query? Try it.

Keep in mind that just like `WHERE`, `ORDER BY` is also optional and all queries without it are valid queries. Even further because `WHERE` is optional it is completely ok to have queries which have only `SELECT`, `FROM` and `ORDER BY`.

### Limit

When you work in a managerial position powered by your awesome data analytics skills the sky is the limit but in SQL limits are something different and a useful tool to help you reach the sky and maybe even beyond.

A limit is a number which determines how many rows of your output table you want to keep. For example a limit of 5 means that only the **first** 5 rows are kept while others are discarded. What is **first** depends on your use of `ORDER BY` or lack of it. When you don't use it the so called natural ordering is used to define the first 5 rows. This means that you get rows the way they were added to the table. When you use `ORDER BY` you can control which rows come first.

Execute the following query:

```sql
SELECT
    name,
    sex,
    age
FROM datasets.titanic
LIMIT 5
```

You will get the table we used as an example in the beginning of this document.

The pattern here is that `LIMIT` is also optional and that it comes after `SELECT` and `FROM` and when not provided you get back all rows. If you use `WHERE` or `ORDER BY`, `LIMIT` will come last. In fact `LIMIT` always comes last. Later we will learn about other query building blocks but `LIMIT` will always be last.

Other than being useful to make example tables, limiting the number of output rows in conjunction with ordering rows gives us an easy way to answer questions like "What was the highest fare paid?" or "What was the lowest fare paid for 2nd class passengers". We will answer the second question while leaving the first one as exercise for the reader.

```sql
SELECT
    fare
FROM datasets.titanic
WHERE
    pclass = 2
ORDER BY
    fare
LIMIT 1
```

You should get 0. The result you got is a single number but it is still a table. A weird table with a single row and a single column but a table nonetheless. 
Notice how we did not specify `ASC` or `DESC`. By default `ASC` is assumed. Also notice we used `LIMIT 1`. The meaning behind our query is: Order all 2nd class fares such that the lowest fare has to come first and then take only the first using `LIMIT 1`.

Another question which we can ask is "Who was the passenger who paid the highest fare". How would you answer this question? Hint: you will need to use everything learned thus far. The columns you will need are: `name` and `fare`. 

## The world of SQL

You can pat yourself on the back for a job well done after absolving this lecture. Yet you can't stop here. The world of SQL is big with many possibilities and many pitfalls. In our future lessons we will teach you about the possibilities like advanced filtering, ordering by multiple columns, aggregations, working with multiple tables, working with date and time, processing text, making pivot tables and many more concepts which will empower you to finish your job faster and with less hassle. We will also warn you of potential pitfalls and bad practices which you should avoid. If you get stuck somewhere in this or any other lesson please write on the forum.

Accompanying these lectures are 3 sql guides: Basic, Intermediate and Advanced. Feel free to consult these guides for additional learning material. You can also enhance your learning process through our 300+ questions with solutions for all possible topics.

This lesson concludes with a set of example queries for which you should try to guess the questions behind them and with a few questions for you to write queries for. Additionally we provide fixes for common issues you might reach when starting out.

## Examples

Hint: The query thought process can work in reverse.

```sql
SELECT
    pclass,
    age
FROM datasets.titanic
WHERE
    fare > 10
ORDER BY
    pclass DESC
```

```sql
SELECT
    name
FROM datasets.titanic
ORDER BY
    survived DESC
LIMIT 10
```

## Try it yourself

1. What is the name and ticket code for all survived passengers?

2. Order first class passengers from youngest to oldest.

3. Who is the youngest female passenger who survived?


## Common beginner errors

When starting out you will likely come across syntax error problems and similar problems. To be error free make sure that you don't do any of the following:
- Don't write a comma after the last column before `FROM`
- Don't write things like `WHEREpclass=3`. Keywords like `WHERE` must be separate words. Our suggestion is follow our formatting guidelines when writing your own queries so you can spot errors faster.
- `ORDER BY` is always two words. Same for `GROUP BY` you will learn later about.
- Remember to write `datasets.table_name` instead of just `table_name`
