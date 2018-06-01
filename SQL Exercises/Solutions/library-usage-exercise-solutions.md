# Library Usage Exercise Solutions

This exercise uses the `datasets.library_usage` table and includes the demographics of the different amount of checkouts from different types of people and libraries.

The column `patron_type_definition` describes whether they are an adult, senior, junior, staff, etc. `Total_checkouts` shows the number of times this person checked out books in the SF libraries. `Total_renewals` show how many times they renewed. The `age-range` shows what age bracket the users belong. `Home_library_definition` shows where their home library in SF is. `circulation_active-month` shows the month they checked out the book from the SF library. `circulation year` shows the year that the book was checked out from the SF libraries. `notice_preference-definition` provides the option whether they want their notification in email, print, or phone call. `year-patron registered` contains the year they registered as a member of the SF libraries. Questions in the exercises involve the patron type, number of checkouts and renewal, age range, library name, circulation month and year, email address, patron resisted and supervisor district.

### Question 1 
What are the top 10 numbers of total checkouts?

*Solution:*

In our solution, we simply counted the total number of checkouts and filter the result where `total_checkouts < 10`. To know the top 10, we group the ranked the results in descending order.
```sql
  SELECT count(*)
    total_checkouts
  FROM datasets.library_usage
  WHERE total_checkouts < 10
  GROUP BY "total_checkouts"
  ORDER BY total_checkouts DESC
```

### Question 2
How many libraries had 100 total checkouts in February 2015?

*Solution:*

Our query begins by counting the number of `home_library_code` from the table. Since we want to know the libraries with 100 checkouts in February 2015, we added the condition on our query to satify our requirements.
```sql
  SELECT count(*) home_library_code
  FROM datasets.library_usage
  WHERE total_checkouts = 100 
    and circulation_active_month = 'February' 
    and circulation_active_year = 2015 
```
*Output:* `8`

### Question 3
How many emails were FALSE in 2016 in each library?

*Solution:*

We can solve this problem by selecting the `home_library_code` from the dataset. We then added a condition where the emails were `FALSE` and the active year was 2016.
```sql
  SELECT home_library_code
  FROM datasets.library_usage
  WHERE notice_preference_definition = 'email'
    and provided_email_address = 'FALSE'
    and circulation_active_year = '2016'
```

### Question 4
How many people registered in 2016?

*Solution:*

Here, we simple count the number of people who registered in 2016. This is attained by adding a condition in the `WHERE` statement.
```sql
  SELECT Year_Patron_Registered,
    count (*) as Total_People
  FROM datasets.library_usage
  WHERE Year_Patron_Registered = 2016
  GROUP BY Year_Patron_Registered
```
*Output:* `26288`

### Question 5
Which library had the most total checkouts from ADULTS in 2010?

*Solution:*

In our solution, we first count the number of patrons registered and determine the maximum number of total checkouts. We further filter our results by adding the `ADULT` as patron type definition and the year registered which is 2010. The total checkouts are group and sorted to determine the top results.
```sql
  SELECT Year_Patron_Registered,
    Home_Library_Definition,
    max(total_checkouts) AS Total_Checkouts_Number
  FROM datasets.library_usage
  WHERE Patron_Type_Definition = 'ADULT' AND Year_Patron_Registered = 2010
  GROUP BY Home_Library_Definition, Year_Patron_Registered
  ORDER BY Total_Checkouts_Number DESC
```

### Question 6
Which library had the most checkouts from adults aging 65 to 74 years in April 2015?

*Solution:*

In our solution, we first select the patrons registered, the library definition and then determine the maximum number of total checkouts. We further filter our results by adding the age range, the year registered, and the active month on the conditional statement. The results are then grouped and sorted in descending order to determine the top results.
```sql
  SELECT Year_Patron_Registered,
    Home_Library_Definition,
    max(total_checkouts) AS Total_Checkouts_Number
  FROM datasets.library_usage
  WHERE age_range = '65 to 74 years' AND Year_Patron_Registered = 2015 AND
    circulation_active_month = 'April'
  GROUP BY Home_Library_Definition, Year_Patron_Registered
  ORDER BY Total_Checkouts_Number DESC
```

### Question 7
Which month did the main library had the most total checkouts in 2013?

*Solution:*

Here, we first select the circulation month and determine the sum of total checkouts per month. Since we want to know the total checkouts from the main library in 2013, we added these requirements on the conditional statement.
```sql
  SELECT circulation_active_month,
    SUM(total_checkouts) as monthly_checkouts
  FROM datasets.library_usage
  WHERE home_library_definition = 'Main Library'and circulation_active_year = 2013
  GROUP by circulation_active_month
  ORDER By monthly_checkouts DESC
```

### Question 8
What is the average of total checkouts from the Chinatown library in January 2016?

*Solution:*

We find the average of total checkouts using the `avg` function. Our output is filtered by adding the library definition as 'Chinatown' and circulation yea `2016`.
```sql
  SELECT
    Avg (total_checkouts) as Total_Checkouts
  FROM datasets.library_usage
  WHERE home_library_definition = 'Chinatown' and circulation_active_year = 2016
```
*Output:* `539`

### Question 9
Which library has the most total renewal?

*Solution:*

Here, we select the home library definition and solve the sum of the total renewals from each of the libraries. The results are grouped and sorted in descending order to determine the top results.
```sql
  SELECT home_library_definition, 
    SUM(total_renewals) as total_lib_renewals
  FROM datasets.library_usage
  GROUP BY home_library_definition
  ORDER BY total_lib_renewals DESC
```

### Question 10
How many library patrons have renewed books less than 10 times on July 2014

*Solution:*

We start our query by selecting the home library code from the table. A `WHERE` condition is added wherein the active month is July and the active year is 2014.
```sql
  SELECT home_library_code
  FROM datasets.library_usage
  WHERE total_renewals < 10
    and circulation_active_month = 'July'
    and circulation_active_year = '2014'
```

