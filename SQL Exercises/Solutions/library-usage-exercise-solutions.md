# Library Usage Exercise Solutions

This exercise uses the `datasets.library_usage` table and includes the demographics of the different amount of checkouts from different types of people and libraries.

The column `patron_type_definition` describes whether they are an adult, senior, junior, staff, etc. `Total_checkouts` shows the number of times this person checked out books in the SF libraries. `Total_renewals` show how many times they renewed. The `age-range` shows what age bracket the users belong. `Home_library_definition` shows where their home library in SF is. `circulation_active-month` shows the month they checked out the book from the SF library. `circulation year` shows the year that the book was checked out from the SF libraries. `notice_preference-definition` provides the option whether they want their notification in email, print, or phone call. `year-patron registered` contains the year they registered as a member of the SF libraries. Questions in the exercises involve the patron type, number of checkouts and renewal, age range, library name, circulation month and year, email address, patron resisted and supervisor district.

### Question 1 
How many patrons made a checkout once, twice... up to 10 times?

*Solution:*

In our solution, we simply counted the total number of patrons for each checkout count and filter the result where `total_checkouts < 10`. 

```sql
SELECT 
    total_checkouts,
    COUNT(*) AS cnt
FROM 
    datasets.library_usage
WHERE total_checkouts < 10
GROUP BY total_checkouts
ORDER BY total_checkouts DESC
```

### Question 2
How many libraries had 100 total checkouts in February 2015?

*Solution:*


The filtering in our query happens on month, year and total checkouts. After the filtering we count the number of rows which remain after the filter and that is the solution we are looking for.


```sql
SELECT
    COUNT(*) AS number_of_libraries
FROM
    datasets.library_usage
WHERE 
    total_checkouts = 100 AND
    circulation_active_month = 'February' AND
    circulation_active_year = 2015
```
*Output:* `8`

### Question 3
How many emails were FALSE in 2016 in each library?

*Solution:*

We can solve this problem by selecting the `home_library_code` from the dataset. We then added a condition where the emails were `FALSE` and the active year was 2016.

```sql
SELECT 
    home_library_code
FROM
    datasets.library_usage
WHERE 
    notice_preference_definition = 'email' AND 
    provided_email_address = 'FALSE' AND 
    circulation_active_year = 2016
```

### Question 4
How many people registered in 2016?

*Solution:*

Here, we just count the number of people who registered in 2016. 

This is attained by adding a condition in the `WHERE` statement.

```sql
SELECT 
    COUNT(*) as Total_People
FROM 
    datasets.library_usage
WHERE 
    Year_Patron_Registered = 2016
```

*Output:* `26288`

### Question 5
Which library had the most total checkouts from ADULTS in 2010?

*Solution:*

In our solution, we first count the number of patrons registered and determine the maximum number of total checkouts. We further filter our results by adding the `ADULT` as patron type definition and the year registered which is 2010. The total checkouts are group and sorted to determine the top results.

```sql
SELECT
    Year_Patron_Registered,
    Home_Library_Definition,
    MAX(total_checkouts) AS Total_Checkouts_Number
FROM
    datasets.library_usage
WHERE 
    Patron_Type_Definition = 'ADULT' AND 
    Year_Patron_Registered = 2010
GROUP BY 
    Home_Library_Definition, 
    Year_Patron_Registered
ORDER BY Total_Checkouts_Number DESC
```

### Question 6
Which library had the most checkouts from adults aging 65 to 74 years in April 2015?

*Solution:*

In our solution, we first select the patrons registered, the library definition and then determine the maximum number of total checkouts. We further filter our results by adding the age range, the year registered, and the active month on the conditional statement. The results are then grouped and sorted in descending order to determine the top results.

```sql
SELECT 
    Year_Patron_Registered,
    Home_Library_Definition,
    MAX(total_checkouts) AS Total_Checkouts_Number
FROM 
    datasets.library_usage
WHERE 
    age_range = '65 to 74 years' AND 
    Year_Patron_Registered = 2015 AND
    circulation_active_month = 'April'
GROUP BY 
    Home_Library_Definition,
    Year_Patron_Registered
ORDER BY Total_Checkouts_Number DESC
```

### Question 7
What is the month during which the main library had the highest number of checkouts in 2013? 
 
*Solution:*

Here, we first select the circulation month and determine the sum of total checkouts per month. Since we want to know the total checkouts from the main library in 2013, we added these requirements on the conditional statement.

```sql
SELECT 
    circulation_active_month,
    SUM(total_checkouts) AS monthly_checkouts
FROM
    datasets.library_usage
WHERE 
    home_library_definition = 'Main Library' AND 
    circulation_active_year = 2013
GROUP by circulation_active_month
ORDER By monthly_checkouts DESC
```

### Question 8
What is the average of total checkouts from the Chinatown library in January 2016?

*Solution:*

We find the average of total checkouts using the `avg` function.

Our output is filtered by adding the library definition as 'Chinatown' and circulation yea `2016`.

```sql
SELECT
    AVG(total_checkouts) AS AVG_Total_Checkouts
FROM 
    datasets.library_usage
WHERE 
    home_library_definition = 'Chinatown' AND 
    circulation_active_year = 2016
```
*Output:* `539`

### Question 9
Which libraries have highest numbers of total renewals?

*Solution:*

Here, we select the home library definition and solve the sum of the total renewals from each of the libraries. The results are grouped and sorted in descending order to determine the top results.
```sql
SELECT 
    home_library_definition, 
    SUM(total_renewals) AS total_lib_renewals
FROM 
    datasets.library_usage
GROUP BY home_library_definition
ORDER BY total_lib_renewals DESC
```

### Question 10
How many library patrons renewed books less than 10 times in July 2014? 

*Solution:*

We filter by month, year and <= 10 total renewals. After the filter we take the count to see how many rows passed the filter.

```sql
SELECT
    COUNT(*)
FROM 
    datasets.library_usage
WHERE
    total_renewals < 10 AND 
    circulation_active_month = 'July' AND
    circulation_active_year = '2014'
```

