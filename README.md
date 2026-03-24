# Factors on Airbnb Pricing (Montreal Listings)

This project studies which factors are associated with Airbnb nightly prices in Montreal. The analysis combines:

- **structured listing features** such as room type, accommodates, bedrooms, beds, and minimum nights
- **text analysis** of listing descriptions using tokenization, stopword removal, TF-IDF, and exploratory wordclouds
- **statistical modeling** using linear regression on `log1p(price)`, AIC-based model comparison, and interaction testing

The analysis is written in **R Markdown** and rendered in a GitHub-friendly format.

---

## Project Goal

The main goal is to understand which listing characteristics are most strongly related to Airbnb prices in Montreal, while also exploring whether listing descriptions contain language patterns associated with higher- and lower-priced properties.

---

## What the Project Does

### 1. Data Loading
- Imports `listings.csv` from an Inside Airbnb-style Montreal dataset.

### 2. Data Cleaning and Preparation
- Converts variables to appropriate types:
  - `price` from text to numeric
  - response/acceptance rates from percentages to proportions
  - numeric housing variables such as `accommodates`, `bathrooms`, `bedrooms`, and `beds`
  - categorical variables such as `property_type` and `room_type` to factors
- Creates rule-based cleaning flags for transparency
- Removes listings with:
  - missing, zero, or negative prices
  - missing key predictors
  - logical inconsistencies in capacity information
- Creates the transformed outcome:
  - `log_price = log1p(price)`

### 3. Exploratory Data Analysis
- Summarizes nightly price using:
  - minimum
  - median
  - mean
  - maximum
- Visualizes the distribution of raw price
- Compares raw price vs. log-transformed price to show why the log scale is more suitable

### 4. Text Analysis of Listing Descriptions
- Splits listings into **High price** and **Low price** groups using the median price
- Tokenizes descriptions
- Removes standard stopwords
- Keeps sensible alphabetical tokens only
- Produces wordclouds for:
  - high-price listings
  - low-price listings
  - all listings combined

### 5. Airbnb-Specific Stopword Construction
- Identifies words that are very common in both price groups
- Treats highly shared generic listing terms as **Airbnb-specific stopwords**
- Removes these words to improve the usefulness of later text analysis

### 6. TF-IDF Feature Construction
- Builds TF-IDF features for each listing description
- Compares average TF-IDF values between high-price and low-price groups
- Identifies words that are more associated with one group than the other

### 7. Dimension Reduction on Text Features
- Builds a TF-IDF document-term matrix
- Applies PCA using `irlba::prcomp_irlba` for efficient dimension reduction on sparse text data
- Examines explained variance and component structure
- Tests whether PCA scores differ across price groups

> **Note:** The PCA section is still being refined, so this part of the project is currently exploratory and in progress.

### 8. Statistical Modeling
- Fits multiple linear regression models using `log1p(price)` as the response
- Compares candidate models using **AIC**
- Examines whether the effect of bedrooms depends on room type through an interaction term
- Uses model comparison to identify the strongest and most interpretable pricing model

---

## Main Variables Used

The main structured predictors in the pricing models are:

- `accommodates`
- `bedrooms`
- `beds`
- `room_type`
- `minimum_nights`

These were chosen because they are intuitively important for pricing and had relatively manageable missing-data issues.

---

## Key Results So Far

### Structured Listing Features
The regression results suggest that:

- **Room type is one of the strongest drivers of price**
  - hotel rooms tend to be priced higher than the baseline category
  - private rooms tend to be priced lower than entire home/apartment listings
- **Accommodates has a positive effect**
  - listings that host more guests tend to have higher nightly prices
- **Minimum nights has a negative effect**
  - listings with longer minimum stay requirements tend to have lower nightly prices
- **Bedrooms matter, but the effect depends on room type**
  - the bedroom premium is smaller for private rooms than for entire-home listings

### Text Features
The text analysis suggests that listing descriptions do contain differences between higher- and lower-priced listings. TF-IDF comparisons show that some words are more characteristic of one price group than the other.

Since the PCA section is still being improved, the text-based findings should currently be interpreted as exploratory.

---

## Why Use `log1p(price)`?

Raw Airbnb prices are strongly right-skewed, with a small number of very high-priced listings. Modeling `log1p(price)` helps:

- reduce skewness
- reduce the influence of extreme values
- make regression assumptions more reasonable
- improve interpretability of proportional price effects

---

## Repository Structure

- `Factors on Airbnb Pricing.Rmd` — full analysis with code and explanation
- `Factors on Airbnb Pricing.md` — rendered GitHub-friendly report
- `Factors on Airbnb Pricing_files/` — figures generated during knitting
- `listings.csv` — input dataset (if included in the repository)

---

## Tools and Packages

### R Packages
- `tidyverse`
- `dplyr`
- `readr`
- `stringr`
- `tidyr`
- `ggplot2`
- `scales`
- `tidytext`
- `wordcloud`
- `RColorBrewer`
- `irlba`
- `broom`

### Methods
- rule-based data cleaning with removal flags
- log transformation of price
- TF-IDF feature engineering
- PCA on text features
- multiple linear regression
- AIC-based model comparison
- interaction modeling

---

## How to Run

1. Open the `.Rmd` file in **RStudio**
2. Install any required packages
3. Knit the document using:
   - **Knit to GitHub Document** for GitHub display
   - or **Knit to HTML** for local viewing

---

## Notes

- The cleaning process is designed to be transparent by using separate removal flags before filtering.
- The text analysis depends on tokenization choices and the Airbnb-specific stopword list.
- The PCA section is still under development and may change as interpretation improves.

---

## Current Status

- ✅ Data cleaning completed  
- ✅ EDA completed  
- ✅ TF-IDF and word frequency comparison completed  
- ✅ Regression modeling completed  
- 🚧 PCA interpretation still in progress  

---