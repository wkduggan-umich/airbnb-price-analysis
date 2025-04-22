# AirBNB Price Analysis

## Introduction

The goal of this project is to analyze the factors that control the price of an AirBNB. The AirBNB open data set contains data on all 2,667,574 AirBNB units in New York City since the company was founded in 2008.

The research question I'll be answering is: "How can AirBNB most accuratley predict the price of a unit when it is first added?". The goal of the analysis in this project will be to predict the price of an AirBNB based on factors that an owner would know when they first register their unit. Predictions like this would allow AirBNB help their owners to find an accurate and competitive listing price.

The following describes all of the columns found in the data set that we'll be used in this analysis. Note, that there are many other features in the AirBNB data set. However, these are the ones deemed to be most relevant to predicting price.

| Column    | Description |
|------------------|-------|
| `id`   | The unique id assigned to the AirBNB unit in the dataset     |
| `name` | The name of the AirBNB     |
| `price` | The price of the AirBNB each night     |
| `construction year` | The year the AirBNB property was built     |
| `neighbourhood group`   | The NYC borough the AirBNB is located in (Manhattan, Brooklyn, Bronx, Queens, Staten Island)    |
| `room type`   | The type of the AirBNB (Entire home/apt, Private room, Shared room, Hotel room)    |
| `house rules`   | The house rules of the AirBNB    |

## Data Cleaning and Exploratory Data Analysis

### Cleaning the Data

The following are the changes made to the dataset before the analysis:
- The `price` column contained dollar signs and commas, which were removed so the variable could be analyzed as a floating point number.
- The `neighbourhood group` column contained mispelled boroughs, which were replaced with the correctly spelled borough name.
- The `house rules` column contained many empty/null and "#NAME?" (default value) values. These were replaced with the empty string (no description)
- For all other variables, there is no way to predict their value without potentially changing the forecoming analysis. Therefore, these rows were dropped. The dataset was large enough to still contain 2,648,932 AirBNB units after removing these rows.

### Univariate Analysis

Before beginning the prediction analysis, plots were constructed to get a better understanding of the distribution of variables in the data set.

#### Neighbourhood group bar chart

<div style="margin-bottom: -150px;">
    <iframe
    src="assets/univariate_neighbourhood.html"
    width="800"
    height="600"
    frameborder="0"
    ></iframe>
</div>
 The plot above shows the distribution of AirBNBs across the five NYC boroughs. As seen from the plot, most of units are in Manhattan and Brooklyn. This feels intuitive because these are the highest areas of tourism in NYC.

#### Price distribution

<div style="margin-bottom: -150px;">
    <iframe
    src="assets/univariate_price.html"
    width="800"
    height="600"
    frameborder="0"
    ></iframe>
</div>
 
 The plot above shows a histogram of the price of AirBNBs. As seen from the plot, the price is relativley uniform. Almost all of the units have a nightly price between 100 and 1200 dollars, except for a few outliers.

### Bivariate Analysis

It is also important to get an initial understanding of the relationship between variables in the dataset.

#### Price vs. NYC Borough Chart
<div style="margin-bottom: -150px;">
<iframe
 src="assets/bivariate_neighbourhood_price.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
</div>
 The box plot above shows the relationship between AirBNB price and the NYC borough it is located in. As seen from the chart, the median, Q1, and Q3 of the price are relativley constant across the five boroughs.

#### Price vs. Room Type

<div style="margin-bottom: -150px;">
<iframe
 src="assets/bivariate_type_price.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
</div>
 The box plot above shows the relationship between AirBNB price and the room type. Again the price is fairly constant across the different room types. The main note is that their tend to be less hotels on the lower end of the price distribution. This should feel intuitive as many hotels have base rates that would be higher than a typical AirBNB.


#### Room Type vs. NYC Borough Chart
<div style="margin-bottom: -150px;">
<iframe
 src="assets/bivariate_neighbourhood_type.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
</div>
 The plot above shows the number of rentals across both NYC borough and AirBNB type. The key things to note are that there are far more entire homes/apartments in Manhattan compared to other boroughs. Additionally, there are very few hotel listings outside of Manhattan. Additionally, shared rooms are far less common in Staten Island and the Bronx.

### Aggregations

#### Price in each NYC Borough, Room Type Combination

| Neighbourhood Group | Entire home/apt | Hotel room | Private room | Shared room |
|---------------------|------------------|-------------|---------------|--------------|
| Bronx               | 620.45           | NaN         | 635.52        | 592.78       |
| Brooklyn            | 626.81           | 736.12      | 625.77        | 634.42       |
| Manhattan           | 623.27           | 681.87      | 620.44        | 632.46       |
| Queens              | 626.93           | 433.25      | 632.39        | 645.31       |
| Staten Island       | 643.03           | NaN         | 604.47        | 715.60       |

### Imputation

Due to the small amount of data rows that needed to be dropped to remove null values from the dataset, it was deemed unnessecary to do any imputation on the data set. However, as mentioned above, any null or default house rules were replaced with an empty string to represent a unit having no rules.

## Prediction Problem

Based upon the previous variable analysis, I will try to predict the AirBNB price based upon the data that will be availible to AirBNB when a user first registers their unit. The name, construction year, neighbourhood group, room type, and house rules are all items that the owner of an AirBNB would have to enter when they first register their unit. Most other variables in the data set would be unavailible when a unit is first registered. For example, the average reviews would not be availible until the unit was listed.

The goal of the prediction is to quantify the price of the unit based upon these inputs. Therefore, I am trying to solve a regression problem to best predict the AirBNB's price. In order to determine the effectivity of the model, I will measure its effectiveness using mean squared error.

## Baseline Model

### Features

The features used in the inital regression will are `construction year`, `neighbourhood group`, and `room type`. These features will be used to predict the price of the AirBNB unit.

Construction year was considered to be a categorical variable. Although it is numeric, the year is made up of ~20 different categorical integers.

Neighbourhood group and room type are clearly categorical variables, with neighbourhood group being one of the five NYC boroughs and room type being one of four types.

All of these variables are categorical and we're passed into the regression model using one hot encodings.

### Train-test Split

The data set was split into two different groups. 80% of the data was used to make the initial training and the final 20% was used to test the performance of the model.

The model was trained using the `construction year`, `neighbourhood group`, and `room type` columns in training set. These columns were first transformed into hot encodings (as mentioned above) before using a linear regression model to predict the price.

### Baseline Model Evaluation

To evaluate the different models, I'll compare the mean squared error, mean absolute error, and R<sup>2</sup> score. Below is the performance of the baseline model.



## Final Model

The final model still uses a linear regression model to predict the price of the AirBNB. However, two new inputs were engineered to predict the price of the unit.

### New Features

The `name` and `house rules` of the AirBNB contain important information about the AirBNB that can be used to predict the price of the unit. For example, the name could contain keywords such as "luxury", which would correlate to a more expensive unit, or "budget", meaning the AirBNB is cheaper. Additionally, keywords in the house rules could help predict the price. If a unit describes rules related to a pool, we would expect it to be more expensive. Likewise, if the rules contain information about waiting, for example, it may lead to a cheaper price.

In order to find the most important keywords related to the price, a linear regression model was created using just these features and the price. The model was fit using the test data set. Next, I found the most important words by looking at their respective coefficients in the model. The coefficients were sorted by their absolute values in order to capture the words that have the most affect on the price. Note, any words that appear in less than 30 house rules or AirBNB names were thrown out. Words like this were deemed to be too hyperspecific to a AirBNB unit, and we want to capture general trends in names and rules.

Finally, a manual iterative search was done on the data set to see how many of the most effective keywords (sorted by TF-IDF) should be used when predicting the price of an AirBNB, using MSE to determine the most effective number of keywords. For the name, it was determined that the 140 most effective keywords were the best predictor. Likewise, for the house rules, the 220 highest keywords were used.

#### Most Effective AirBNB Name and House Rules Keywords 

Through this analysis, I found the keywords that have the largest postitive and negative impact on the AirBNB price. The following data table shows the five keywords with the highest TF-IDF for `name` and `house rules`. The name impact and house rules impact column contains a "+" if the keyword had a postitive impact on price and a "-" for a negative impact on price.

| House Rules | House Rules Impact | Name   | Name Impact |
|-------------|--------------------|--------|-------------|
| Printed     | +                  | Dorm   | -           |
| Nespresso   | +                  | Young  | -           |
| Order       | +                  | Mott   | +           |
| Affiliated  | +                  | Crash  | -           |
| Photo       | +                  | Vibes  | -           |

### Modeling Algorithm

The new model uses a similar one hot encoding on the variables that were used in the baseline model (construction year, neighbourhood group, and room type). Additionally, it now builds a TF-IDF matrix on the name and house rules column. The vocabulary set for these two columns are the list of most effective keywords found previously. Finally, the model uses linear regression on these variables to predict the price of the AirBNB. Below, is the performance of the new model, using this model.

### Finding the Best Prediction Model

Finally, the model was tested using four new prediction models in replacement of linear regression: 
- Ridge Regression 
- Random Forest
- Gradient Boosted
- Support Vector Regression

Each of these prediction models' hyper-parameters were tuned using GridSearchCV to find the parameters with the minimmum absolute error on the training data. 

### Final Model Evaluation

Below is the performance of each of the models tested throughout the analysis process. 

## TODO - insert table

## Conclusion

The most effective model on the test data was the model utilizing all five input parameters. The `room type`, `construction year`, and `neighbourhood group` variables were encoded as one hot variables. Next, I used tf-idf to find the most important keywords in the `name` and `house rules` columns for predicting price. Using an iterative search, I found the most effective number of the highest correlation keywords to use for predicting the AirBNB price. Finally, I compared the effectiveness of different predition models. From this analysis, the most accurate model uses random forest as its prediction model. This model had an RMSLE of 0.74.

There were many short comings in predicting the price of the AirBNB. The most difficult part was the lack of numerical data related to the prediction of the AirBNB price. As a result, I was forced to use purely categorical data in my prediction model. This led to unique prediction practices, such as using TF-IDF to find important key words in predicting the price.

Since the variables used were all categorical, the random forest model was a much better predictor of AirBNB price. This is due to the fact that random forest creates decision trees, benefitting the model as it decides between categorical variables.