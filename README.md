**Airbnb Price Prediction: Regression Analysis**
**Project Overview**

This project investigates the factors that influence Airbnb listing prices using multiple linear regression. The goal is to identify which property characteristics significantly impact price and to build a statistically sound model that explains price variation.

The analysis demonstrates skills in:

Data cleaning and preprocessing

Exploratory data analysis

Log transformation and justification

Variable selection

Interaction modeling

Model diagnostics and assumption checking

Dataset Description

The dataset contains Airbnb listing information including:

  Price

  Number of bedrooms

  Number of bathrooms

  Room type

  Number of guests accommodated

  Minimum nights

  Review scores

  Other listing characteristics
  

Each observation represents a single Airbnb listing.


**Data Cleaning Process**

A systematic approach was used to clean the dataset:

1. Missing Values

  Observations with missing key predictors (e.g., price, bedrooms, room type) were removed.

  The number and percentage of removed observations were documented.

2. Filtering Unrealistic Values

  Listings with zero or extremely low prices were removed.

  Extreme price outliers were examined and filtered where justified.

3. Variable Formatting

  Categorical variables (e.g., room_type) were converted to factors.
  
  Numeric variables were verified to ensure correct data type.

4. Log Transformation of Price

  Price was highly right-skewed.
  Histograms of price showed a long right tail, violating normality assumptions.

  To address this, a log transformation was applied:

  log_price = log(price)


This:

Reduced skewness

Improved normality

Stabilized variance

Made regression assumptions more reasonable
