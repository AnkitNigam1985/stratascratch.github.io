# French Employment Exercise Solutions

This exercise contains the information about the French employment. The table `datasets.french_employment_base_etablissement_par_tranche_effectif`
provides information about the establishments present in a town while `datasets.french_employment_net_salary_per_town_categories` presents 
the net salary per town.

### Question 1
Which town has the most companies (employment opportunities)?

*Solution:*

We can determine the town having the most number of companies by selecting `libgeo` and `e14tst` which will output the town name and number of companies from the dataset. Using the `ORDER BY` statement, we can sort the data in descending order to know the top town on the list with the highest nunmber of companies.
```sql
  SELECT libgeo, e14tst
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif
  ORDER BY e14tst DESC
```
*Answer:* `Paris`

### Question 2
What is the average number of firms in a town?

*Solution:*

Using `avg`, we compute the average number of firms in a town and display the result on the editor.
```sql
  SELECT avg(e14tst)
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif
```
*Answer:* `123`

### Question 3
What is the French town’s mean net salary (economy condition)? 

*Solution:*

To determine the mean net salary, we simply select and compute the average of the `snhm14` from the `datasets.french_employment_net_salary_per_town_categories` table.
```sql
  SELECT
  AVG(snhm14)
  FROM datasets.french_employment_net_salary_per_town_categories
```
*Answer:* `13.71`

### Question 4
Which town has the most mean net salary for middle manager?

*Solution:*

To know the town with the most mean net salary, we select the `libgeo` and `snhmp14` columns. The data is then sorted in descending order so that the top list of towns can be seen on the output.
```sql
  SELECT libgeo, snhmp14
  FROM datasets.french_employment_net_salary_per_town_categories
  ORDER BY snhmp14 DESC
```
*Answer:* `Chambourcy`

### Question 5
Which age range makes the most money in the largest city (Paris)?

*Solution:*

We simply select the columns `libgeo`, `snhmf1814,` `SNHMf2614,` `SNHMf5014,` `snhmh1814,` `SNHMH2614,` and `SNHMH5014` to display the net salaries of both men and women for each town. Paris is known as the largets city and we include a condition where `libgeo = 'Paris'` to exclude other towns.
```sql
  SELECT libgeo, snhmf1814, SNHMf2614, SNHMf5014, snhmh1814, SNHMH2614, SNHMH5014
  FROM datasets.french_employment_net_salary_per_town_categories
  WHERE libgeo = 'Paris'
```
*Note: This is for both men and women.*

### Question 6
Which age range by gender makes the most money?

*Solution:*

For males, we select the columns `libgeo, snhmh1814, SNHMH2614,` and `SNHMH5014` to output the net salaries of the respectives towns. On the last statement, we add a condition `libgeo = 'Paris'` to display only results for Paris and exclude other towns.
```sql
  SELECT libgeo, snhmh1814, SNHMH2614, SNHMH5014
  FROM datasets.french_employment_net_salary_per_town_categories
  WHERE libgeo = 'Paris'
```

For females, we select the columns `libgeo, snhmf1814, SNHMf2614,` and `SNHMf5014` to output the net salaries of the respectives towns. On the last statement, we add a condition `libgeo = 'Paris'` to display only results for Paris and exclude other towns.
```sql
  SELECT libgeo, snhmf1814, SNHMf2614, SNHMf5014 
  FROM datasets.french_employment_net_salary_per_town_categories
  WHERE libgeo = 'Paris'
```
How about in a much smaller city(48 firms, Marzan)?

*Solution:*

For males, we select the columns `libgeo, snhmh1814, SNHMH2614,` and `SNHMH5014` to output the net salaries of the respectives towns. On the last statement, we add a condition `libgeo = 'Marzan'` to display only results for Marzan and exclude other towns.
```sql
  SELECT libgeo, snhmh1814, SNHMH2614, SNHMH5014
  FROM datasets.french_employment_net_salary_per_town_categories
  WHERE libgeo = 'Marzan'
```

For females, we select the columns `libgeo, snhmh1814, SNHMH2614,` and `SNHMH5014` to output the net salaries of the respectives towns. On the last statement, we add a condition `libgeo = 'Marzan'` to display only results for Marzan and exclude other towns.
```sql
  SELECT libgeo, snhmf1814, SNHMf2614, SNHMf5014 
  FROM datasets.french_employment_net_salary_per_town_categories
  WHERE libgeo = 'Marzan'
```

For both males and females, we can simply select the columns `snhmf1814, SNHMf2614, SNHMf5014, snhmh1814, SNHMH2614, SNHMH5014` and `libgeo` to output the net salaries of the respectives towns. On the last statement, we add a condition `libgeo = 'Marzan'` to display only results for Marzan and exclude other towns.
```sql
  SELECT libgeo, snhmf1814, SNHMf2614, SNHMf5014, snhmh1814, SNHMH2614, SNHMH5014
  FROM datasets.french_employment_net_salary_per_town_categories
  WHERE libgeo = 'Marzan'
```

### Question 7
What is the breakdown of small to large firms in the town with the most firms (Paris)? 

*Solution:*

To answer the question, we can simply select the town name `libgeo` and columns `e14ts1, e14ts6, e14ts10, e14ts20, e14ts50, e14ts100, e14ts200, e14ts500` to output the number of firms. We add the condition `libgeo = 'Paris'` since we are only concerned with the town having the most number of firms.
```sql
  SELECT libgeo, e14ts1, e14ts6, e14ts10, e14ts20, e14ts50, e14ts100, e14ts200, e14ts500
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif
  WHERE libgeo = 'Paris'
```

How about in Marzan?

*Solution:*

To answer the question, we can simply select the town name `libgeo` and columns `e14ts1, e14ts6, e14ts10, e14ts20, e14ts50, e14ts100, e14ts200, e14ts500` to output the number of firms. We add the condition `libgeo = 'Marzan'` to exclude other towns.
```sql
  SELECT libgeo, e14ts1, e14ts6, e14ts10, e14ts20, e14ts50, e14ts100, e14ts200, e14ts500
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif
  WHERE libgeo = 'Marzan'
```

### Question 8
What is the relationship between Total_Firms and Female Executive Salaries in Towns?

*Solution:*

Our query starts by selecting the appropriate columns to answer the question and these are `firms.libgeo, salary.snhmfc14 as fem_exec, ` and `firms.e14tst as Total_Firms`. Then we `JOIN` two datasets so that we can get the net salaries of women per firm and town. We add `ORDER BY` to sort the resulting data in descending order.
```sql
  SELECT firms.libgeo, salary.snhmfc14 as fem_exec, firms.e14tst as Total_Firms
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif firms
  JOIN datasets.french_employment_net_salary_per_town_categories salary
  ON firms.libgeo = salary.libgeo
  ORDER BY total_firms DESC
```

What is the relationship between Total_Firms and Male Executive Net Mean Salaries in Towns?

*Solution:*

Our query starts by selecting the appropriate columns to answer the question and these are `firms.libgeo, salary.snhmfc14 as fem_exec, ` and `firms.e14tst as Total_Firms`. Then we `JOIN` two datasets so that we can get the net salaries of men per firm and town. We add `ORDER BY` to sort the resulting data in descending order.
```sql
  SELECT firms.libgeo,salary.SNHMHC14 as man_exec, firms.e14tst as Total_Firms
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif firms
  JOIN datasets.french_employment_net_salary_per_town_categories salary
  ON firms.libgeo = salary.libgeo
  ORDER BY total_firms DESC
```

Note: 

SNHMFC14 : mean net salary per hour for feminin executive

SNHMHC14 : mean net salary per hour for masculin executive

### Question 9
What is the relationship between Big500_Firms and Female Executive Salaries in Towns?

*Solution:*

The same with the previous solution, we start our query by selecting the appropriate columns to answer the question and these are `firms.libgeo, salary.SNHMFC14 as FEM_exec, ` and `firms.E14TS500 as Big500_Firms`. Then we `JOIN` two datasets so that we can get the net salaries of women per firm and town. We add `ORDER BY` to sort the resulting data in descending order.
```sql
  SELECT firms.libgeo, salary.SNHMFC14 as FEM_exec, firms.E14TS500 as Big500_Firms
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif firms
  JOIN datasets.french_employment_net_salary_per_town_categories salary
  ON firms.libgeo = salary.libgeo
  ORDER BY Big500_firms DESC
```

What is the relationship between Big500_Firms and Male Executive Salaries in Towns?

*Solution:*

We can answer the question by selecting the appropriate columns and these are `firms.libgeo,salary.SNHMHC14 as MEN_exec, ` and `firms.E14TS500 as Big500_Firms`. Then we `JOIN` two datasets so that we can get the net salaries of men per firm and town. We add `ORDER BY` to sort the resulting data in descending order.
```sql
  SELECT firms.libgeo,salary.SNHMHC14 as MEN_exec, firms.E14TS500 as Big500_Firms
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif firms
  JOIN datasets.french_employment_net_salary_per_town_categories salary
  ON firms.libgeo = salary.libgeo
  ORDER BY Big500_firms DESC
```

Note:

E14TS500 : number of firms with more than 500 employees in the town

### Question 10
What is the relationship between Total_Firms and Female (18-50) Net Mean Salaries in Towns?

*Solution:*

Our query starts by selecting the appropriate columns to answer the question and these are `firms.libgeo, salary.SNHMF2614 as fem_1850, ` and `firms.e14tst as Total_Firms`. Then we `JOIN` two datasets so that we can get the net salaries of women (aging 18 - 50) per firm and town. We add `ORDER BY` to sort the resulting data in descending order. 
```sql
  SELECT firms.libgeo, salary.SNHMF2614 as fem_1850, firms.e14tst as Total_Firms
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif firms
  JOIN datasets.french_employment_net_salary_per_town_categories salary
  ON firms.libgeo = salary.libgeo
  ORDER BY total_firms DESC
```

What is the relationship between Total_Firms and Male (18-50) Net Mean Salaries in Towns?

*Solution:*

We can solve the problem by selecting the appropriate columns and these are `firms.libgeo, salary.SNHMH2614 as MEN_1850,` and `firms.e14tst as Total_Firms`. Then we `JOIN` two datasets so that we can get the net salaries of men (aging 18 - 50) per firm and town. We add `ORDER BY` to sort the resulting data in descending order. 
```sql
  SELECT firms.libgeo,salary.SNHMH2614 as MEN_1850, firms.e14tst as Total_Firms
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif firms
  JOIN datasets.french_employment_net_salary_per_town_categories salary
  ON firms.libgeo = salary.libgeo
  ORDER BY total_firms DESC
```

Note:

SNHMF2614 : mean net salary per hour for women between 26-50 years old

SNHMH2614 : mean net salary per hour for men between 26-50 years old

