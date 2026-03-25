# Factors on Airbnb Pricing (Montreal Listings)

This project analyzes what drives Airbnb nightly prices in Montreal using a combination of:

- **structured listing features** (room type, accommodates, bedrooms, beds, minimum nights)
- **text features** from listing descriptions (tokenization, TF–IDF, word frequency analysis, PCA)
- **statistical modeling** (linear regression, AIC-based model selection, interaction effects)

The analysis is implemented in **R Markdown** and rendered for GitHub viewing.

---

## 🎯 Analytical Research Question

This project is guided by the following core research question:

> **How do listing characteristics such as capacity, room type, and minimum night requirements influence Airbnb pricing, and does the effect of bedrooms vary across different room types?**

To support this, the analysis also explores:

- Do listing descriptions differ systematically between high-priced and low-priced listings?
- Can text-based features (TF–IDF, PCA) reveal latent patterns associated with price?

---

## Project Goal

The goal is to **quantify how both structured listing attributes and textual description features relate to Airbnb pricing**, using interpretable statistical models and text-based feature engineering.

---

## What the Project Does

### 1. Data Loading
- Imports `listings.csv` (Inside Airbnb dataset for Montreal)

---

### 2. Data Cleaning and Preparation
- Converts variables to appropriate types:
  - `price` → numeric
  - percentage variables → proportions
  - numeric housing variables → numeric
  - categorical variables → factors
- Creates **rule-based cleaning flags** for transparency
- Removes listings with:
  - missing or invalid prices
  - missing key predictors
  - logical inconsistencies (e.g., 0 bedrooms with positive capacity)
- Constructs:
  - `log_price = log1p(price)`

> This ensures valid, comparable observations and reduces the impact of extreme values.

---

### 3. Exploratory Data Analysis (EDA)
- Summary statistics: min, median, mean, max
- Distribution plots for:
  - raw price
  - log-transformed price
- Demonstrates strong right-skew in raw prices and motivates the use of `log(price)`

---

### 4. Text Analysis of Listing Descriptions
- Splits listings into:
  - **High price**
  - **Low price** (median split)
- Tokenizes descriptions using `tidytext`
- Removes standard stopwords
- Filters to meaningful tokens
- Produces wordclouds for:
  - high-price listings
  - low-price listings
  - overall dataset

---

### 5. Airbnb-Specific Stopword Construction
- Identifies words common across both price groups using within-group proportions
- Removes highly shared generic terms (e.g., “apartment”, “downtown”)
- Improves signal quality for downstream text analysis

---

### 6. TF–IDF Feature Engineering
- Computes TF–IDF for each listing description
- Compares average TF–IDF between price groups
- Identifies **words most associated with high vs low price listings**
- Visualizes differences using wordclouds

---

### 7. Dimension Reduction (PCA on Text Features)
- Builds a TF–IDF document-term matrix
- Applies PCA using `irlba::prcomp_irlba`
- Examines:
  - variance explained
  - principal component structure
- Tests whether PC scores differ across price groups (t-tests)

> Note: Individual components explain small variance, which is typical for sparse text data.

---

### 8. Statistical Modeling (Pricing Drivers)
- Fits regression models with response:
  - `log1p(price)`
- Uses **forward selection + AIC** for model comparison
- Evaluates:
  - baseline additive model
  - interaction model (`bedrooms × room_type`)
- Resolves interaction redundancy via structured variable design

---

## Main Variables Used

- `accommodates`
- `bedrooms`
- `beds`
- `room_type`
- `minimum_nights`

Selected based on:
- domain relevance (pricing intuition)
- data quality (low missingness)

---

## Key Findings

### 📊 Structured Features

- **Room type is a dominant pricing factor**
  - hotel rooms → higher prices
  - private rooms → lower prices than entire homes

- **Capacity matters**
  - higher `accommodates` → higher price

- **Minimum nights has a negative effect**
  - longer required stays → lower nightly price

- **Bedrooms effect depends on room type (interaction)**
  - bedrooms increase price overall
  - effect is **smaller for private rooms**
  - strongest for entire-home listings

---

### 🧠 Text Features

- Listing descriptions show **systematic differences** across price groups
- TF–IDF highlights words associated with higher vs lower priced listings
- PCA suggests **latent language patterns**, though each component explains a small portion of variance

---

## Why Use `log1p(price)`?

Airbnb prices are heavily right-skewed. Using `log1p(price)`:

- stabilizes variance
- reduces influence of extreme outliers
- improves regression assumptions
- allows interpretation in **percentage terms**

---

## Repository Structure

- `Factors on Airbnb Pricing.Rmd` — full analysis (code + narrative)
- `Factors on Airbnb Pricing.md` — rendered GitHub report
- `Factors on Airbnb Pricing_files/` — generated figures
- `listings.csv` — dataset (if included)

---

## Tools and Methods

### R Packages
- tidyverse, dplyr, readr, stringr, tidyr
- ggplot2, scales
- tidytext, wordcloud, RColorBrewer
- irlba (fast PCA)
- broom

### Methods
- rule-based data cleaning with transparency flags
- log transformation
- TF–IDF feature engineering
- PCA on high-dimensional text data
- linear regression
- AIC-based model selection
- interaction modeling

---

## How to Run

1. Open `.Rmd` in RStudio  
2. Install required packages  
3. Click **Knit → GitHub Document**  

---

## Notes

- Cleaning decisions are fully transparent via removal flags
- Text results depend on tokenization and custom stopword filtering
- PCA results are exploratory due to high-dimensional sparsity

---

## Current Status

- ✅ Data cleaning completed  
- ✅ EDA completed  
- ✅ TF–IDF analysis completed  
- ✅ Regression modeling completed  
- 🚧 PCA interpretation ongoing  

---
