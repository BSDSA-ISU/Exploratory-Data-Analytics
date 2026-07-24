# Exploratory Data Analysis

## Module 2: Data Acquisition and Pre-processing

---

- [Exploratory Data Analysis](#exploratory-data-analysis)
  - [Module 2: Data Acquisition and Pre-processing](#module-2-data-acquisition-and-pre-processing)
  - [Overview](#overview)
  - [Data Sources](#data-sources)
  - [Steps in Identifying Good Data Sources](#steps-in-identifying-good-data-sources)
  - [Quality \& Pre-Processing](#quality--pre-processing)
  - [What Makes Data “High Quality”?](#what-makes-data-high-quality)
  - [Common Data Quality Issues](#common-data-quality-issues)
  - [General Pre-processing Pipeline](#general-pre-processing-pipeline)
  - [Handling Missing Data and Outliers](#handling-missing-data-and-outliers)
    - [Types of Missingness](#types-of-missingness)
  - [Data Transformation](#data-transformation)
    - [Common types of Transformations](#common-types-of-transformations)
      - [Example of one-hot encoding table](#example-of-one-hot-encoding-table)
      - [Example of Ordinal Encoding](#example-of-ordinal-encoding)
      - [Example of Label Encoding](#example-of-label-encoding)

![Cirno hates this topic](https://media.tenor.com/iPKa5SFvaKAAAAAi/touhou-cirno.gif)

> Cirno hates this topic

---

## Overview

Before the rise of modern machine learning, most of the effort in analytics was not in building models, but in collecting and cleaning the data. It’s often said that:

> “80% of a data scientist’s time is spent preparing data, and only 20% analyzing it.”

In the 1960s–70s, statisticians emphasized the need for “clean data” before applying regression or hypothesis testing. Today, with huge, messy datasets from sensors, social media, and enterprise systems, data preprocessing is critical. Models trained on bad data will produce misleading results, no matter how advanced the algorithm.

Data is the fuel of analytics and machine learning. The quality of your insights depends on the quality of your data sources. If the source is biased, outdated, or irrelevant, your entire analysis will suffer.

## Data Sources

- **A. Primary Data**
  - Collected firsthand for a specific study or analysis.
  - **Examples:** Surveys, experiments, IoT sensor readings.
  - **Advantage:** Tailored to your research question. Offers good control over quality.
  - **Disadvantage:** Expensive, time-consuming.

- **B. Secondary Data**
  - Collected by others, reused for analysis.
  - **Examples:** Government databases (PSA, World Bank), company records, Kaggle datasets.
  - **Advantage:** Accessible, cheaper. Provides a foundation or context for primary research.
  - **Disadvantage:** May not perfectly fit your problem, quality may vary.

- **C. Streaming Data**
  - Real-time continuous data flow.
  - **Examples:** Stock prices, social media feeds, weather sensors, server logs.
  - Requires tools like Apache Kafka, Spark, or Python tweepy.

- **D. APIs (Application Programming Interfaces)**
  - Provide structured data access.
  - **Examples:**
    - Twitter API (tweets).
    - OpenWeather API (weather data).
    - NewsAPI (headlines).
  - **Advantage:** Real-time, structured.
  - **Disadvantage:** Rate limits, authentication, sometimes paid.

- **E. Databases & Files**
  - **Relational Databases (SQL):** MySQL, PostgreSQL.
  - **NoSQL Databases:** MongoDB, Cassandra.
  - **Files:** CSV, Excel, JSON, Parquet.

- **F. Web Scraping**
  - Extracting data directly from websites.
  - **Tools:** Python BeautifulSoup, Scrapy, Selenium.
  - Always check legality & terms of service.

## Steps in Identifying Good Data Sources

1. **Define your objective** – What question are you trying to answer?
2. **Check relevance** – Does the source contain variables you need?
3. **Assess reliability** – Who collected the data? Is it trustworthy?
4. **Check accessibility** – Can you legally and technically access it?
5. **Evaluate quality** – Completeness, consistency, timeliness.

## Quality & Pre-Processing

Data quality issues are one of the biggest risks in analytics. Even the most advanced machine learning model is useless if it’s trained on bad data.

In the early days of statistics, quality meant careful sampling and controlled experiments. In modern data science, quality checks are about fixing real-world issues: incomplete logs, duplicated transactions, inconsistent formats.

Companies lose billions annually from “dirty data” (wrong invoices, duplicated customers, outdated addresses).

Think of data like ingredients in cooking: if your vegetables are spoiled or mislabeled, even the best chef (your algorithm) can’t make a good dish.

## What Makes Data “High Quality”?

High-quality data has five key characteristics:

1. **Accuracy** - Correct and free from error.
   - *Example:* Birthdate recorded as 1998-07-12 vs. a typo like 1989-17-12.
2. **Completeness** - No critical information missing.
   - *Example:* Customer records missing phone numbers.
3. **Consistency** - Uniform formatting and standards.
   - *Example:* PH vs. Philippines vs. Pilipinas.
4. **Timeliness** - Data is up to date.
   - *Example:* Using 2015 census data for a 2025 market analysis = outdated.
5. **Relevance** - Data serves the analysis goal.
   - *Example:* Shoe size is irrelevant in predicting house prices.

## Common Data Quality Issues

- **Typos & Human Errors** (misspellings, wrong codes).
- **Duplicates** (same customer registered twice).
- **Inconsistent Units** (weight in pounds vs. kilograms).
- **Outdated Data** (addresses of customers who moved years ago).
- **Irrelevant Data** (columns that don’t matter for your problem).
- **Outliers** (values far outside the normal range, e.g., negative age).

## General Pre-processing Pipeline

When working with a new dataset, a data scientist’s first step is to inspect and prepare it. The pipeline usually looks like this:

1. **Data Inspection**
   - Load dataset, then check rows, columns, data types.
   - Use `df.info()` / `df.describe()` in Python or `str(df)` / `summary(df)` in R.
2. **Standardization of Formats**
   - Fix column names, consistent casing.
   - Convert dates to proper datetime format.
3. **Validation & Cleaning**
   - Remove duplicates.
   - Handle missing values (next subtopic).
   - Detect outliers (to be handled later in EDA).
4. **Integration**
   - If multiple sources are used, merge/join carefully.
   - Ensure keys match (e.g., “CustomerID” is consistent across datasets).
5. **Final Checks**
   - Re-check summary statistics after cleaning.

## Handling Missing Data and Outliers

In real-world data, missing values and outliers are unavoidable. Missing data can occur from survey non-responses, sensor failures, data corruption, or incomplete integration. Outliers may arise from measurement errors, data entry mistakes, or true rare events (like fraud or natural disasters). Both can bias your analysis, weaken models, and even lead to wrong decisions if not handled properly.

### Types of Missingness

It’s important to know why data is missing, because that guides your handling strategy:

1. **MCAR (Missing Completely At Random)** - The probability of data being missing is unrelated to the data itself, this implies that missingness happens by pure chance.
   - **Why this happens:**
     - Random technical glitches (e.g., server timeout, dropped sensor reading).
     - Accidental data entry issues (e.g., a survey respondent skips a question by mistake).
   - **Example:**
     - A dataset of student grades where one exam score is missing because the paper got lost.
     - Sensor network logging hourly temperatures, but one timestamp is missing due to temporary power loss.
   - **Implication:**
     - Safe to remove or impute, because the missingness doesn’t depend on any pattern.

2. **MAR (Missing At Random)** - The probability of data being missing is related to observed data, but not the missing values themselves.
   - **Why this happens:**
     - Human tendencies: people with certain characteristics may be less likely to answer some questions.
     - Operational constraints: data collection methods may depend on other recorded variables.
   - **Example:**
     - In a medical survey, younger participants are less likely to report their income. Here, missingness is related to age (observed), but not to the income itself.
     - In an online shopping dataset, discount codes are often missing, but only for users who shop via mobile apps (observed behavior).
   - **Implication:**
     - Requires imputation using other observed variables (regression, KNN imputation).

3. **MNAR (Missing Not At Random)** - Missingness depends on the value of the unobserved data itself. In other words, the reason it’s missing is directly tied to what’s missing.
   - **Why this happens:**
     - Sensitive or stigmatized information where respondents deliberately skip answers.
     - Systematic bias in data collection.
   - **Example:**
     - High-income individuals deliberately leave the “annual income” field blank.
     - In a hospital dataset, patients with severe conditions are more likely to have incomplete lifestyle information (because they avoid answering or are unable to).
   - **Implication:**
     - Hardest case to deal with. Requires advanced statistical modeling, domain expertise, or even external data sources to estimate.
     - If unaddressed, can severely bias analysis.

## Data Transformation

Once you’ve handled missing data and outliers, the next step in preparing your dataset is transformation. Transformation modifies variables so they are easier to analyze and more suitable for machine learning models. Think of it as reshaping your data so it fits into the mold (algorithm). The reasons why we transform data are as follows:

1. **Normalize scale** - Put variables on the same playing field.
2. **Reduce skewness** - Fix heavy tails or lopsided data.
3. **Stabilize variance** - Helpful for regression and statistical tests.
4. **Improve interpretability** - Makes trends easier to spot.

### Common types of Transformations

1. **Normalization**
   - Normalization rescales values to a fixed range, this range is typically [0,1]. This transformation preserves the shape of the distribution but compresses the scale.

   $$x' = \frac{x - \min(x)}{\max(x) - \min(x)}$$

   - **Steps:**
     1. Identify min and max of the column.
     2. Apply the formula to each data point.
     3. Replace raw values with normalized values.

2. **Standardization (Z-Score Scaling)**
   - This transformation converts data to have mean = 0 and standard deviation = 1. This is done when you want to compare variables with different units.

   $$z = \frac{x - \mu}{\sigma}$$

3. **Logarithmic Transformation**
   - This is a mathematical process in which a variable's values are replaced by their logarithms. This is typically done to compress large values and reduce right skew, making data more suitable for analysis, especially linear regression. Commonly we replace each data point with the base 10 ($\log$) or Natural log ($\ln$) in some statistical modeling.

4. **Square/Cube Root Transformation**
   - Square root and cube root transformations involve changing the parent functions $y = \sqrt{x}$ and $y = \sqrt[3]{x}$ by applying horizontal and vertical shifts, stretches/compressions, and reflections.

5. **Encoding Categorical Variables**
   - The process of converting categorical or textual data into numerical format, so that it can be used as input for algorithms to process. The reason for encoding is that most machine learning algorithms work with numbers and not with text or categorical variables. This allows the model to identify patterns in the data and make predictions based on those patterns. Encoding also helps to prevent bias in the model by ensuring that all features are equally weighted.

   - **a. One-Hot Encoding**
     - One-Hot Encoding is the most common method for encoding categorical variables. A binary column is created for each unique category in the variable. If a category is present in a sample, the corresponding column is set to 1, and all other columns are set to 0.

   - **b. Ordinal Encoding**
     - Ordinal Encoding is used when the categories in a variable have a natural ordering. In this method, the categories are assigned a numerical value based on their order, such as 1, 2, 3, etc.
   - **c. Label Encoding**
     - Each unique category is assigned a unique integer value. This is a simpler encoding method, but it has a drawback in that the assigned integers may be misinterpreted by the machine learning algorithm as having an ordered relationship when in fact they do not.

#### Example of one-hot encoding table

**Original data:**

| Places |
| --------- |
| New York |
| Boston |
| Chicago |
| California |
| New Jersey |

⬇️ **One-Hot Encoding**

| Places | New York | Boston | Chicago | California | New Jersey |
| -------------- | :--------:| :------: |:-------:| :----------: | :----------: |
| New York     | 1 | 0 | 0 | 0 | 0 |
| Boston       | 0 | 1 | 0 | 0 | 0 |
| Chicago      | 0 | 0 | 1 | 0 | 0 |
| California   | 0 | 0 | 0 | 1 | 0 |
| New Jersey   | 0 | 0 | 0 | 0 | 1 |

#### Example of Ordinal Encoding

**original data:**

| Size |
|------|
| Small |
| Medium |
| Large |
| Medium |
| Small |

⬇️ **Ordinal Encoding**

| Size   | Encoded |
|---------|:------:|
| Small   | 1 |
| Medium  | 2 |
| Large   | 3 |
| Medium  | 2 |
| Small   | 1 |

#### Example of Label Encoding

**Original data:**

| Places |
| --------- |
| New York |
| Boston |
| Chicago |
| California |
| New Jersey |

⬇️ **Label Encoding**

| Places      | Encoded |
|--------------|:------:|
| New York     | 0 |
| Boston       | 1 |
| Chicago      | 2 |
| California   | 3 |
| New Jersey   | 4 |
