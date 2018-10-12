# Advanced SQL Exercises

## Instructions
- Log-in to your Strata Scratch account.
- All tables used in this exercise are taken from the datasets schema. Make sure you select this schema on the SQL editor settings.
- Try to answer the following questions by writing the appropriate SQL query on the editor.

## Questions

1. Give back all SAT scores from students whose highschool name does NOT end with 'HS'

    `Tables: sat_scores`

2. What is the old to young ratio per olympic game? Young are people whose age <= 30, while old are those whose age >= 45.

    `Tables: olympics_athletes_events`

3. Which day of the week would it be best to trade in AAPL stock? What about the best day in a month? Find the average opening and closing prices. (Hint: You can use EXTRACT(‘dow’ FROM date)  to get the day of week)

    `Tables: aapl_historical_stock_price`

4. Find all hotels in the Netherlands where guests complain about dirty rooms. (Hint: Search for the word dirty in negative_review)

    `Tables: hotel_reviews`

5.  Using a self join on the titanic dataset find the average absolute fare difference between age groups which are in same pclass and which are comprised of non-survivors. Assume that two passengers are in the same age group if their ages are less than 5 years apart.

    `Tables: titanic`

6. Fix the review_date in the hotel_reviews dataset to be of proper YYYY-MM-DD format. (Hint: You should be versatile and liberal in your slicing of the date format, type conversion and whatever it takes)

    `Tables: hotel_reviews`

7. Find the average rating of each movie star along with their name and birthday. (Hint: Use inner queries)

    `Tables: nominee_filmography, nominee_information`

8. Enhance the query from question 7 to calculate the difference in average rating along the years. You can ignore the name column. (Hint: LAG function)

    `Tables: nominee_filmography, nominee_information`

9. Find the most expensive products on Amazon for each product category. Use the price column (Hint: Use subquery joins)

    `Tables: innerwear_amazon_com`

10. Find the products which are exclusive to Amazon compared to Top Shop and Macy's. Two products are considered equal if they have the same brand name and same price.

    `Tables: innerwear_amazon_com, innerwear_topshop_com, innerwear_macys_com`

11. Find all users which are located in Italy but do not speak Italian. You must also filter away all non control group entries and the device they used must be a Nexus 5.

    `Tables: playbook_experiments, playbook_users`

12. Who is the student who has the median writing score? What is the 80th percentile of hours studied? 

    `Tables: sat_scores`

13. Find Yelp reviews about food. Search for keywords like food, pizza, sandwich or burger. Find the business address and the state.

    `Tables: yelp_reviews, yelp_business`

14. Find the date when Apple's opening stock price reached maximum.

    `Tables: aapl_historical_stock_price`

15. What is the average number of days between booking and check-in dates for AirBnB hosts?

    `Tables: airbnb_contacts`
