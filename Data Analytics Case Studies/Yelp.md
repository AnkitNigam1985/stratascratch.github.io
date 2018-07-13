[![strata scratch](../assets/sslogo.jpg)](https://stratascratch.com)

# Strata Scratch

## Airbnb Case Study

#### Accessing The Data Resources
- Reviews dataset can be found under `datasets.yelp_reviews`
- Access the data at www.stratascratch.com
- You can use SQL on Strata Scratch to answer the questions or connect to the datasets and answer the questions using other tools like python
  - [How to Connect to the Database Using Python and Other Programs](https://github.com/stratascratch/stratascratch.github.io/blob/master/guides/how-to-connect-to-the-database-using-python-and-other-programs/how-to-connect-to-the-database-using-python-and-other-programs.md)


### Business Case

- Investigate the data and see if anything needs cleaning.  Hint: Check the unique values and value_counts for the stars column.

- Clean the data by removing the reviews with '?' for stars rating

- Replace the stars values that are text with integers

- How many 5 star reviews does Lo-Lo's Chicken & Waffles have?

- What is Lo-Lo's Chicken & Waffles star review breakdown? (i.e., tell me how many 5 star reviews for Lo-Lo's, 4 star, 3 star, etc) *Hint: try using value_counts()

- What's the most number of cool votes a review received?

- What was the business' name and review_text for the business that received the most cool number of votes

- Which business has the most reviews? (hint: use .value_counts())