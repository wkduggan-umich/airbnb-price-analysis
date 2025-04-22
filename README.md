# AirBNB Price Analysis

## Introduction

The goal of this project is to analyze the factors that control the price of an AirBNB. The AirBNB open data set contains data on all 2,667,574 AirBNB units in New York City since the company was founded in 2008.

The goal of the analysis in this project is to predict the price of an AirBNB based on factors that an owner would know when they first register their unit. Predictions like this would allow AirBNB help their owners to find an accurate and competitive listing price.

# TODO: add more info about the relevance of this and the general research question

The following describes all of the columns found in the data set that we'll be used in this analysis. Note, that there are many other features in the AirBNB data set. However, these are the ones deemed to be most relevant to predicting price.

| Column    | Description |
|-------------|-------|
| id   | The unique id assigned to the AirBNB unit in the dataset     |
| name | The name of the AirBNB     |
| price | The price of the AirBNB each night     |
| construction year | The year the AirBNB property was built     |
| neighbourhood group   | The NYC borough the AirBNB is located in (Manhattan, Brooklyn, Bronx, Queens, Staten Island)    |
| room type   | The type of the AirBNB (Entire home/apt, Private room, Shared room, Hotel room)    |
| house rules   | The house rules of the AirBNB    |

## Data Cleaning and Exploratory Data Analysis

### Cleaning the Data

The following are the changes made to the dataset before the analysis:
- The 'price' column contained dollar signs and commas, which were removed so the variable could be analyzed as a floating point number.
- The 'neighbourhood group' column contained mispelled boroughs, which were replaced with the correctly spelled borough name.
- The 'house rules' column contained many empty/null and "#NAME?" (default value) values. These were replaced with the empty string (no description)
- For all other variables, there is no way to predict their value without potentially changing the forecoming analysis. Therefore, these rows were dropped. The dataset was large enough to still contain 2,648,932 AirBNB units after removing these rows.

### Univariate Analysis

Before beginning the prediction analysis, plots were constructed to get a better understanding of the distribution of variables in the data set.

#### Neighbourhood group bar chart

#### Price distribution

### Bivariate Analysis

It is also important to get an initial understanding of the relationship between variables in the dataset.

#### Price vs. NYC Borough Chart

#### Price vs. Room Type

#### Room Type vs. NYC Borough Chart

### Aggregations

#### Price in each NYC Borough, Room Type Combination

### Imputation

Due to the small amount of data rows that needed to be dropped to remove null values from the dataset, it was deemed unnessecary to do any imputation on the data set. However, as mentioned above, any null or default house rules were replaced with an empty string to represent a unit having no rules.

## Prediction Problem

Based upon the previous variable analysis, I will try to predict the AirBNB price based upon the data that will be availible to AirBNB when a user first registers their unit. The name, construction year, neighbourhood group, room type, and house rules are all items that the owner of an AirBNB would have to enter when they first register their unit. Most other variables in the data set would be unavailible when a unit is first registered. For example, the average reviews would not be availible until the unit was listed.

The goal of the prediction is to quantify the price of the unit based upon these inputs. Therefore, I am trying to solve a regression problem to best predict the AirBNB's price. In order to determine the effectivity of the model, I will measure its effectiveness using mean squared error.

## Baseline Model

### Features

The features used in the inital regression will are construction year, neighbourhood group, and room type. These features will be used to predict the price of the AirBNB unit.

Construction year was considered to be a categorical variable. Although it is numeric, the year is made up of ~20 different categorical integers.

Neighbourhood group and room type are clearly categorical variables, with neighbourhood group being one of the five NYC boroughs and room type being one of four types.

All of these variables are categorical and we're passed into the regression model using one hot encodings.

### Train-test Split

The data set was split into two different groups. 80% of the data was used to make the initial training and the final 20% was used to test the performance of the model.

The model was trained using the construction year, neighbourhood group, and room type columns in training set. These columns were first transformed into hot encodings (as mentioned above) before using a linear regression model to predict the price.

### Baseline Model Evaluation

## TODO

## Final Model

The final model still uses a linear regression model to predict the price of the AirBNB. However, two new inputs were engineered to predict the price of the unit.

### New Features

The name and house rules of the AirBNB contain important information about the AirBNB that can be used to predict the price of the unit. For example, the name could contain keywords such as "luxury", which would correlate to a more expensive unit, or "budget", meaning the AirBNB is cheaper. Additionally, keywords in the house rules could help predict the price. If a unit describes rules related to a pool, we would expect it to be more expensive. Likewise, if the rules contain information about waiting, for example, it may lead to a cheaper price.

In order to find the most important keywords related to the price, a linear regression model was created using just these features and the price. The model was fit using the test data set. Next, I found the most important words by looking at their respective coefficients in the model. The coefficients were sorted by their absolute values in order to capture the words that have the most affect on the price. Note, any words that appear in less than 30 house rules or AirBNB names were thrown out. Words like this were deemed to be too hyperspecific to a AirBNB unit, and we want to capture general trends in names and rules.

#### Most Effective AirBNB Name and House Rules Keywords 

Finally, a manual iterative search was done on the training data to see how many of the most effective keywords should be used when predicting the price of an AirBNB, using MSE to determine the most effective number. For the name, it was determined that the first 370 most effective keywords were the best predictor. Likewise, for the house rules, the first 390 were used.

### Modeling Algorithm

The new model uses a similar one hot encoding on the variables that were used in the baseline model (construction year, neighbourhood group, and room type). Additionally, it now builds a TF-IDF matrix on the name and house rules column. The vocabulary set for these two columns are the list of most effective keywords found previously. Finally, the model uses linear regression on these variables to predict the price of the AirBNB.

### Baseline Model Evaluation

## TODO