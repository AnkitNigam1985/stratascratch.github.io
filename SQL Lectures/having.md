# Two different types of filtering

By now you are already a master of `WHERE` and know all about various types of filtering. In the previous lesson we have introduced aggregations which are used very often. We have also mixed filtering and aggregations in some examples.

In SQL there are two types of filtering, the one in `WHERE` which we covered extensively and one in `HAVING` which is what we will introduce today. Most simply put `HAVING` works per group, keeping only groups which pass a condition which is an aggregation by itself. Ok it is not easy to explain it with a single definition but as always our barrage of examples will guide us through the murky waters of SQL.

You can use either or both types of filtering your queries.

## Conceptual examples

These examples are not linked to any specific SQL table but to general idea of `HAVING`:
- Find the number of employees per departments in a company such that **all** employees **in the department** have an average wage larger than 500$. Departments which have employees whose salary is less than 500$ are thus not part of the output table. 
- Find the number of speakers per language such that the minimum language score **in the group** is bigger than 90. This means that if even **one** speaker has score less than 90 then the corresponding language is not included in the output table.
- Find the average age per county for counties having more than 1000 inhabitants. Thus counties where `count(*) < 1000` are not part of the output.

The general idea is to to look at the aggregate property for a group and then filter away groups which do not satisfy that aggregate property.

## Real examples

Here is an example of an SQL query using the `HAVING` statement.

The question is: "What are the schools where the average verbal score is less than 495?"

```sql
SELECT
    school,
    avg(sat_verbal)
FROM datasets.sat_scores
GROUP BY 
    school
HAVING
    avg(sat_verbal) < 495
```

The keyword `HAVING` comes after `GROUP BY`. Using `HAVING` without group by does not make sense and will give you an error.

**NB**: Until you master `HAVING` you will often see the dreaded message `The query returned no data`. To solve this write your query without `HAVING` but with the aggregation you care about in the `SELECT` part so you can analyze the numbers.

### Example 1

"Which passenger classes have more than 100 survivors?"

This question is a bit tricky because it does not appear to be a group by question, but it is. The only trick is that we don't calculate any aggregate value in `SELECT` but use aggregations to filter away groups using `HAVING`

```sql
SELECT
    pclass
FROM datasets.titanic
GROUP BY
    pclass
HAVING 
    sum(survived) > 100
```

### Example 2

"Which passenger classes have more than 10 survivors while the average age is less than 30"

```sql
SELECT
    pclass
FROM datasets.titanic
GROUP BY
    pclass
HAVING 
    sum(survived) > 10 AND 
    avg(age) <= 30
```

Just like regular `WHERE` you can use `AND`, `OR` and `NOT`. You can use the standard suite of operators with the exception of `ILIKE`.

### Example 3

"Which passenger classes have more than 10 female survivors with the average age being less than 30"

```sql
SELECT
    pclass
FROM datasets.titanic
WHERE
    sex = 'female'
GROUP BY
    pclass
HAVING 
    sum(survived) > 10 AND 
    avg(age) <= 30
```

As you see, `WHERE` and `HAVING` can coexist in the same query. 

When wondering what should go to `WHERE` and what should go to `HAVING` ask the question:
- Does the property I care about apply per entity or for all entities in the group?

If the answer is 'per entity' then it goes to `WHERE`, otherwise it goes to `HAVING`

## Conclusion

This lesson is rather short because it covers only a single concept.