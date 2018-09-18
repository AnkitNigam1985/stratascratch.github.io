[![strata scratch](../assets/sslogo.jpg)](https://stratascratch.com)

# Strata Scratch

## Apple Case Study

#### Accessing The Data Resources
- Apple historical stock price dataset can be found under `datasets.aapl_historical_price`
- Access the data at www.stratascratch.com
- You can connect to the datasets and answer the questions using python.
- You can now run Jupyter notebooks on Google CoLab so there’s no software installation needed. Use this [introductory notebook](https://colab.research.google.com/drive/1tHxAbgbxM60VUIrVQW508EwB1b3wFk5g) as a template to start the analytics case.


### Business Case

Unlike other business cases you won't have to answer a list of precisely guided questions but will be faced with an open problem which in the solution is solved in one way but you can solve it some other way.

The problem is to predict the opening price direction (price rises, price remains constant, price falls) given previous opening prices for some fixed length number of days to look in the past.

In the solution we use random forests and try 14, 30 and 90 days in the back in two variations:
- Using raw numbers as features
- Using the direction of change as features

Our solution reaches around 55% accuracy which is low but gives you the base to explore various approaches which will likely yield better results. We limited our study only to use previous opening prices but you can incorporate other informations into your model.

You can try to use k-NN, to do feature selection, to use closing and low prices, to aggregate the data and make a classifier which works week by week instead of day by day, to use some aggregate data as features (like mean opening price) and so on.
