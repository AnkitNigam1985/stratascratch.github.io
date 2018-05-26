# French Employment Exercises

This exercise contains the information about the French employment. The table `datasets.french_employment_base_etablissement_par_tranche_effectif`
provides information about the establishments present in a town while `datasets.french_employment_net_salary_per_town_categories` presents 
the net salary per town.

### Question 1
Which town has the most companies (employment opportunities)?

*Solution:*
```sql
  SELECT libgeo, e14tst
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif
  ORDER BY e14tst DESC
```
*Answer:* `Paris`

### Question 2
What is the average number of firms in a town?

*Solution:*
```sql
  SELECT avg(e14tst)
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif
```
*Answer:* `123`

### Question 3
What is the French town’s mean net salary (economy condition)? 

*Solution:*
```sql
  SELECT
  AVG(snhm14)
  FROM datasets.french_employment_net_salary_per_town_categories
```
*Answer:* `13.71`

### Question 4
Which town has the most mean net salary for middle manager?

*Solution:*
```sql
  SELECT libgeo, snhmp14
  FROM datasets.french_employment_net_salary_per_town_categories
  ORDER BY snhmp14 DESC
```
*Answer:* `Chambourcy`

### Question 5
Which age range makes the most money in the largest city (Paris)?

*Solution:*
```sql
  SELECT libgeo, snhmf1814, SNHMf2614, SNHMf5014, snhmh1814, SNHMH2614, SNHMH5014
  FROM datasets.french_employment_net_salary_per_town_categories
  WHERE libgeo = 'Paris'
```
*Note: This is for both male and female.*

### Question 6
Which age range by gender makes the most money?

*Solution:*
`For males:`
```sql
  SELECT libgeo, snhmh1814, SNHMH2614, SNHMH5014
  FROM datasets.french_employment_net_salary_per_town_categories
  WHERE libgeo = 'Paris'
```
`For females:`
```sql
  SELECT libgeo, snhmf1814, SNHMf2614, SNHMf5014 
  FROM datasets.french_employment_net_salary_per_town_categories
  WHERE libgeo = 'Paris'
```
How about in a much smaller city(48 firms, Marzan)?

*Solution:*
`For males:`
```sql
  SELECT libgeo, snhmh1814, SNHMH2614, SNHMH5014
  FROM datasets.french_employment_net_salary_per_town_categories
  WHERE libgeo = 'Marzan'
```
`For females:`
```sql
  SELECT libgeo, snhmf1814, SNHMf2614, SNHMf5014 
  FROM datasets.french_employment_net_salary_per_town_categories
  WHERE libgeo = 'Marzan'
```
`For both males and females:`
```sql
  SELECT libgeo, snhmf1814, SNHMf2614, SNHMf5014, snhmh1814, SNHMH2614, SNHMH5014
  FROM datasets.french_employment_net_salary_per_town_categories
  WHERE libgeo = 'Marzan'
```

### Question 7
What is the breakdown of small to large firms in the town with the most firms (Paris)? 

*Solution:*
```sql
  SELECT libgeo, e14ts1, e14ts6, e14ts10, e14ts20, e14ts50, e14ts100, e14ts200, e14ts500
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif
  WHERE libgeo = 'Paris'
```

How about in Marzan?

*Solution:*
```sql
  SELECT libgeo, e14ts1, e14ts6, e14ts10, e14ts20, e14ts50, e14ts100, e14ts200, e14ts500
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif
  WHERE libgeo = 'Marzan'
```

### Question 8
What is the relationship between Total_Firms and Female Executive Salaries in Towns?

*Solution:*
```sql
  SELECT firms.libgeo,salary.snhmfc14 as fem_exec, firms.e14tst as Total_Firms
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif firms
  JOIN datasets.french_employment_net_salary_per_town_categories salary
  ON firms.libgeo = salary.libgeo
  ORDER BY total_firms DESC
```

What is the relationship between Total_Firms and Male Executive Net Mean Salaries in Towns?

*Solution:*
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
```sql
  SELECT firms.libgeo,salary.SNHMFC14 as FEM_exec, firms.E14TS500 as Big500_Firms
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif firms
  JOIN datasets.french_employment_net_salary_per_town_categories salary
  ON firms.libgeo = salary.libgeo
  ORDER BY Big500_firms DESC
```

What is the relationship between Big500_Firms and Male Executive Salaries in Towns?

*Solution:*
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
```sql
  SELECT firms.libgeo, salary.SNHMF2614 as fem_1850, firms.e14tst as Total_Firms
  FROM datasets.french_employment_base_etablissement_par_tranche_effectif firms
  JOIN datasets.french_employment_net_salary_per_town_categories salary
  ON firms.libgeo = salary.libgeo
  ORDER BY total_firms DESC
```

What is the relationship between Total_Firms and Male (18-50) Net Mean Salaries in Towns?

*Solution:*
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

