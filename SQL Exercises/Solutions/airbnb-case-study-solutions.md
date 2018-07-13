- There are two tables: airbnb_contacts and airbnb_searches. Explore the columns the datasets and merge the two on an appropriate key. Then display the first 5 rows of the merged dataset

We do a join on keys id_user and id_guest

```sql
SELECT *
FROM datasets.airbnb_searches search 
    INNER JOIN datasets.airbnb_contacts contact 
        ON search.id_user = contact.id_guest
LIMIT 5
```

- What is the average number of searches across users?

Each user is represented by a unique id (id_user) so we can group by id_user to find some
aggregate quantity, in our case the average number of searches (n_average_searches).

```sql
SELECT
    id_user,
    AVG(n_searches) AS n_average_searches
FROM airbnb_searches
GROUP BY id_user
```

- How many unique searchers are there in the dataset? And how many different properties are there?

To find unique searches.

```sql
SELECT COUNT(DISTINCT id_user)
FROM datasets.airbnb_searches
```

To find the different properties.

The query has 3 operations from inner to outer:
* 1. ltrim to remove the coma which is sometimes the first character
* 2. regexp_split_to_array which splits the filter string into an array with the separator being ','
* 3. unnest which converts an array into a table which can be used in a query

Finally from that table we just `SELECT DISTINCT`

```sql
SELECT DISTINCT
    unnest(regexp_split_to_array(ltrim(filter_room_types, ','), ','))
FROM datasets.airbnb_searches
```

- How many times does a user perform a search on average? 

```sql
SELECT 
    AVG(n_searches)
FROM datasets.airbnb_searches
```

- Is there a difference between the number of times a user searches and successfully books vs the number of times a user search does not book?

Compare the results of the following two queries.

```sql
SELECT
    AVG(n_searches) as average_searches
FROM 
    datasets.airbnb_contacts contacts, datasets.airbnb_searches searches
WHERE 
    contacts.ts_booking_at IS NULL
    AND contacts.id_guest = searches.id_user
```

```sql
SELECT
    AVG(n_searches) as average_searches
FROM 
    datasets.airbnb_contacts contacts, datasets.airbnb_searches searches
WHERE 
    contacts.ts_booking_at IS NOT NULL
    AND contacts.id_guest = searches.id_user
```

