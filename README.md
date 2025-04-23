# Airbnb Price Analysis and Prediction

Will Duggan - wkduggan@umich.edu

## Introduction

The goal of this project is to analyze the factors that control the price of an Airbnb. The Airbnb open data set contains data on all 2,667,574 Airbnb units in New York City since the company was founded in 2008.

This project will attempt to answer:
- **"How can Airbnb most accurately predict the price of a newly added unit?"**
The goal of the analysis in this project will be to introduce factors that predict the price of an Airbnb. Next, I'll use these factors to predict the price of an Airbnb. Predictions like this would allow Airbnb to help their owners find an accurate and competitive listing price for their unit when they first add it to the app.

The following table describes all of the columns found in the data set that will be used in this analysis. Note, that there are many other features in the Airbnb data set. However, these are the ones deemed to be most relevant to predicting price.

| Column    | Description |
|------------------|-------|
| `id`   | The unique id assigned to the Airbnb unit in the dataset     |
| `name` | The name of the Airbnb     |
| `price` | The price of the Airbnb each night     |
| `construction year` | The year the Airbnb property was built     |
| `neighbourhood group`   | The NYC borough the Airbnb is located in (Manhattan, Brooklyn, Bronx, Queens, Staten Island)    |
| `room type`   | The type of Airbnb (Entire home/apt, Private room, Shared room, Hotel room)    |
| `house rules`   | The house rules of the Airbnb    |

## Data Cleaning and Exploratory Data Analysis

### Cleaning the Data

The following changes were made to clean the dataset before the analysis:
- The `price` column contained dollar signs and commas, which were removed so the variable could be analyzed as a floating point number.
- The `neighbourhood group` column contained misspelled boroughs, which were replaced with the correctly spelled borough name.
- The `house rules` column contained many empty/null and "#NAME?" (default) values. These were replaced with the empty string to signify no description.
- For all other variables, there is no way to predict their value without potentially changing the forthcoming analysis. Therefore, these rows were dropped. The dataset was large enough to still contain 2,648,932 Airbnb units after removing these rows.

### Univariate Analysis

Before beginning the prediction analysis, plots were constructed to get a better understanding of the distribution of variables in the data set.

#### Neighbourhood group bar chart

<div style="margin-bottom: -150px; text-align:center;">
    <iframe
    src="assets/univariate_neighbourhood.html"
    width="800"
    height="600"
    frameborder="0"
    ></iframe>
</div>

 The plot above shows the distribution of Airbnbs across the five NYC boroughs. As seen from the plot, most of the units are in Manhattan and Brooklyn. This feels intuitive because these are the highest areas of tourism in NYC.

#### Price distribution

<div style="margin-bottom: -150px; text-align:center;">
    <iframe
    src="assets/univariate_price.html"
    width="800"
    height="600"
    frameborder="0"
    ></iframe>
</div>
 
 The plot above shows a histogram of the price of Airbnbs. As seen from the plot, the price is relativley uniform. Almost all of the unit's nightly price lies uniformly between 100 and 1200 dollars, except for a few outliers.

### Bivariate Analysis

It is also important to get an initial understanding of the relationship between variables in the dataset.

#### Price vs. NYC Borough Chart
<div style="margin-bottom: -150px; text-align:center;">
<iframe
 src="assets/bivariate_neighbourhood_price.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
</div>

 The box plot above shows the relationship between Airbnb price and the NYC borough it is located in. As seen from the chart, the median, Q1, and Q3 of the price are relativley constant across the five boroughs.

#### Price vs. Room Type

<div style="margin-bottom: -150px; text-align:center;">
<iframe
 src="assets/bivariate_type_price.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
</div>

 The box plot above shows the relationship between Airbnb price and the room type. Again the price is fairly constant across the different room types. The main note is that there tend to be less hotels on the lower end of the price distribution. This should feel intuitive as many hotels have base rates that would be higher than a typical Airbnb.

#### Room Type vs. NYC Borough Chart
<div style="margin-bottom: -150px; text-align:center;">
<iframe
 src="assets/bivariate_neighbourhood_type.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
</div>

 The plot above shows the number of rentals across both NYC borough and Airbnb type. The key things to note are that there are far more entire homes/apartments in Manhattan compared to other boroughs. Additionally, there are very few hotel listings outside of Manhattan. Additionally, shared rooms are far less common in Staten Island and the Bronx.

### Aggregations

The following two tables show the mean and median price across different NYC borough and room type combinations.

#### Mean Price in each NYC Borough, Room Type Combination

| Neighbourhood Group | Entire home/apt | Hotel room | Private room | Shared room |
|---------------------|------------------|-------------|---------------|--------------|
| Bronx               | 620.45           | NaN         | 635.52        | 592.78       |
| Brooklyn            | 626.81           | 736.12      | 625.77        | 634.42       |
| Manhattan           | 623.27           | 681.87      | 620.44        | 632.46       |
| Queens              | 626.93           | 433.25      | 632.39        | 645.31       |
| Staten Island       | 643.03           | NaN         | 604.47        | 715.60       |

#### Median Price in each NYC Borough, Room Type Combination

| Neighbourhood Group | Entire home/apt | Hotel room | Private room | Shared room |
|---------------------|------------------|-------------|---------------|--------------|
| Bronx               | 613.0           | NaN         | 654.0         | 601.0        |
| Brooklyn            | 626.0           | 836.0       | 624.0         | 662.0        |
| Manhattan           | 623.0           | 644.5       | 619.0         | 640.5        |
| Queens              | 624.0           | 367.5       | 629.0         | 659.5        |
| Staten Island       | 667.0           | NaN         | 593.0         | 680.0        |

As seen from the tables, the mean and median often don't defer by much, telling us that the data is not often skewed in these combinations. Additionally, we can see that hotels have a lot of variance in price across different boroughs. The `NaN` values for hotel rooms in the Bronx and Staten Island imply there are no hotel room listings in those boroughs. Overall, the data does vary across these different combinations but not by much (except for hotel rooms).

### Imputation

Due to the small amount of data rows that needed to be dropped to remove null values from the dataset, it was deemed unnecessary to do any imputation on the data set. However, as mentioned above, any null or default house rules were replaced with an empty string to represent a unit having no rules.

## Prediction Problem

The goal of the prediction analysis will be to predict the Airbnb price, using solely data that will be available to Airbnb when a user first registers their unit. The name, construction year, neighbourhood group, room type, and house rules are all items that the owner of an Airbnb would have to enter when they first register their unit. Most other variables in the data set would be unavailable when a unit is first registered. For example, the average reviews would not be available until the unit was listed.

The goal of the prediction is to quantify the price of the unit based upon these inputs. Therefore, I am trying to solve a regression problem to best predict the Airbnb's price. To evaluate the different models, I'll compare the mean absolute error (MAE). This was chosen because mean absolute error is robust to outliers, giving extreme luxury or low budget rentals less of a dramatic impact on the model.

## Baseline Model

### Features

The features used in the inital regression are `construction year`, `neighbourhood group`, and `room type`. These features will be used to predict the price of the Airbnb unit.

Construction year was considered to be a categorical variable. Although it is numeric, the year is made up of ~20 different categorical integers.

Neighbourhood group and room type are clearly categorical variables, with neighbourhood group being one of the five NYC boroughs and room type being one of four types.

All of these variables are categorical. Therefore, a one-hot encoding was used to pass these variables into the prediction model.

### Train-test Split

The data set was split into two different groups. 80% of the data was used to complete the initial training and the final 20% was used to test the performance of the model.

The model was trained using the `construction year`, `neighbourhood group`, and `room type` columns in the training set. These columns were first transformed into one-hot encodings (as mentioned above) before using a linear regression model to predict the price.

### Baseline Model Evaluation

The baseline model had an MAE of **287.18** when tested on unseen test data. 

Currently, this is not an effective model. Just looking at its general location, construction year, and type are not effective enough to distinguish between a higher end and lower end Airbnb. In order to further distinguish between units, we need a way of quantifying other attributed of the Airbnb.

## Final Model

### New Features

The `name` and `house rules` of the Airbnb contain important information about the Airbnb that can be used to predict the price of the unit. For example, the name could contain keywords such as "luxury", which would correlate to a more expensive unit, or "budget", meaning the Airbnb is cheaper. Additionally, keywords in the house rules could help predict the price. If a unit describes rules related to a pool, we would expect it to be more expensive. Likewise, if the rules contain information about waiting, for example, it may lead to a cheaper price.

In order to find the most important keywords related to the price, a linear regression model was created using just these features and the price. The model was fit using the training data set. Next, I found the most important words by looking at their respective coefficients in the model. The coefficients were sorted by their absolute values in order to capture the words that have the most effect on the price. Note, any words that appear in less than 30 house rules or Airbnb names were thrown out. Words like this were deemed to be too hyperspecific to a Airbnb unit, and we want to capture general trends in names and house rules.

Finally, a manual iterative search was done on the data set to see how many of the most effective keywords (sorted by TF-IDF) should be used when predicting the price of an Airbnb, using MSE to determine the most effective number of keywords. For the name, it was determined that the 530 most effective keywords were the best predictor. Likewise, for the house rules, the 690 highest keywords were used.

When using the TF-IDF of the `name` and `house rules` columns, they are now considered to be numerical features. They are represented as continuous numerical values between `0.0` and `1.0`. These values represent the importance of specific keywords in the text column.

#### Most Effective Airbnb Name and House Rules Keywords 

Through this analysis, I found the keywords that have the largest postitive and negative impact on the Airbnb price. The following data table shows the five keywords with the highest TF-IDF for `name` and `house rules`. The name impact and house rules impact column contains a `+` if the keyword had a postitive impact on price and a `-` for a negative impact on price. Again, these are only words that appear in at least 30 names or house rules.

| House Rules | House Rules Impact | Name   | Name Impact |
|-------------|--------------------|--------|-------------|
| Printed     | +                  | Dorm   | -           |
| Nespresso   | +                  | Young  | -           |
| Order       | +                  | Mott   | +           |
| Affiliated  | +                  | Crash  | -           |
| Photo       | +                  | Vibes  | -           |

### Modeling Algorithm

The new model uses a similar one hot encoding on the variables that were used in the baseline model (construction year, neighbourhood group, and room type). Additionally, it now builds a TF-IDF matrix on the name and house rules column. The vocabulary set for these two columns are the list of most effective keywords found previously. Finally, the model uses linear regression on these variables to predict the price of the Airbnb.

### Finding the Best Prediction Model

Finally, the model was tested using four new prediction models in replacement of linear regression: 
- Ridge Regression 
- Random Forest
- Gradient Boosted
- Support Vector Regression

Each of these prediction models' hyper-parameters were tuned using GridSearchCV to find the parameters with the minimum absolute error on the training data. 

### Final Model Evaluation

Below is the performance of each of the models tested throughout the analysis process. Unless specified, the model uses linear regression as the predictor.

| Model                                  | Mean Absolute Error |
|----------------------------------------|----------------------|
| Baseline Model                         | 287.18               |
| TF-IDF Model With 50 Most Significant Name Keywords               | 287.03               |
| TF-IDF Model With 50 Most Significant House Rules Keywords         | 287.14               |
| Final Model With Linear Regression       | 285.62               |
| Final Model With Ridge Regression        | 285.57               |
| Final Model With Random Forest           | 217.75               |

## Conclusion

The most effective model on the test data was the model utilizing all five input parameters. The `room type`, `construction year`, and `neighbourhood group` variables were encoded as one hot variables. Next, I used tf-idf to find the most important keywords in the `name` and `house rules` columns for predicting price. Using an iterative search, I found the best number of keywords to use for predicting the Airbnb price. Finally, I compared the effectiveness of different prediction models. From this analysis, the most accurate model on these features uses random forest as its prediction model. This model had an MAE of **217.75**.

Since three of the five variables used were all categorical, the random forest model was a much better predictor of Airbnb price. This is due to the fact that random forest creates decision trees, benefitting a model that's heavy in categorical variables.

There were many short comings in predicting the price of the Airbnb. The most difficult part was the lack of numerical data related to the prediction of the Airbnb price. As a result, I was forced to use purely categorical data in my prediction model. This led to unique prediction practices, such as using TF-IDF to find important key words in predicting the price. Additionally, the model doesn't account for other factors that could affect the price, such as proximity to the subway or tourist sights, seasonal pricing, and overall quality of the unit. Overall, this model could serve as an initial attempt to price an Airbnb. With more data to help quantify the quality and cost of a certain unit, the model could be improved to serve as a concrete recomendation for the listing price.