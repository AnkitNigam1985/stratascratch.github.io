# Library Usage Exercises

The tables explain demographics of the different amount of checkouts from different types of people and libraries.
The column `patron_type_definition` describes whether they are an adult, senior, junior, staff, etc. `Total_checkouts` shows the number of times this person checked out books in the SF libraries. `Total_renewals` show how many times they renewed. The `age-range` shows what age bracket the users belong. `Home_library_definition` shows where their home library in SF is. `circulation_active-month` shows the month they checked out the book from the SF library. `circulation year` shows the year that the book was checked out from the SF libraries. `notice_preference-definition` provides the option whether they want their notification in email, print, or phone call. `year-patron registered` contains the year they registered as a member of the SF libraries. Questions in the exercises involve the patron type, number of checkouts and renewal, age range, library name, circulation month and year, email address, patron resisted and supervisor district.

### Question 1 
What are the top 10 numbers of total checkouts?

*Solution:*
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
```sql
  SELECT circulation_active_month,
    SUM(total_checkouts) as monthly_checkouts
  FROM datasets.library_usage
  WHERE home_library_definition = 'Main Library'and circulation_active_year = 2013
  GROUP by circulation_active_month
  ORDER By monthly_checkouts DESC
```

### Question 8
What is the average total checkouts of Chinatown library in January 2016?

*Solution:*
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
```sql
  SELECT home_library_definition, 
    SUM(total_renewals) as total_lib_renewals
  FROM datasets.library_usage
  GROUP BY home_library_definition
  ORDER BY total_lib_renewals DESC
```

### Question 10
How many times does each library have less than 10 total renewals in July 2014?

*Solution:*
```sql
  SELECT home_library_code
  FROM datasets.library_usage
  WHERE total_renewals < 10
    and circulation_active_month = 'July'
    and circulation_active_year = '2014'
```

