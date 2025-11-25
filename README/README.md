This README provides a structured, technical overview of the `Data Cleaning.ipynb` Jupyter Notebook, outlining its purpose, methodology, and resulting data enhancements.

-----

# Data Cleaning and Feature Engineering for Sales Analysis

## 1\. Overview

This notebook serves as a robust data transformation and preparation pipeline for raw sales data. The primary objective is to convert a raw transactional dataset (`sales_data.csv`) into an analytically ready format by addressing common data quality issues (missing values, duplicates) and performing comprehensive feature engineering. The resulting dataset is enriched with temporal features and derived metrics, facilitating subsequent exploratory data analysis and modeling.

## 2\. Prerequisites

The following Python libraries are required to execute the notebook:

  * **pandas:** For data manipulation and structuring.
  * **numpy:** For numerical operations (e.g., calculating the mean for imputation).
  * **seaborn** and **matplotlib.pyplot:** For data visualization.

<!-- end list -->

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```

## 3\. Data Source and Loading

The notebook loads data from a local file path: `sales_data.csv`.

  * **Initial Data:
  ![alt text](Screenshots/df.jpg)
  The dataset contains columns such as `id`, `customers`, `date`, `amount`, and `last_updated`.
  * **Data Type Handling:** The `'date'` and `'last_updated'` columns are explicitly parsed as `datetime64[ns]` objects during the loading process to ensure correct temporal calculations.

## 4\. Cleaning and Transformation Methodology

The notebook applies a sequence of critical data quality and standardization steps:

| Step | Action Performed | Rationale |
| :--- | :--- | :--- |
| **Data Inspection** | Execution of `df.info()`, `df.describe()`, and `df.isnull().sum()`. | Initial profiling to understand data types, non-null counts, and statistical distribution. |
![alt text](<Screenshots/Summary Stats.jpg>)
| **Missing Values** | Fills any null values in the `'amount'` column using the column's calculated mean. | Ensures the completeness of the primary transactional metric for aggregation. |
| **Duplicate Removal** | Drops rows that are duplicates based on the combination of `'id'` and `'date'`. | Ensures that each unique transaction on a given day is represented only once, maintaining data integrity. |
| **Data Standardization** | Converts all entries in the `'customers'` column to Title Case (`.str.title()`). | Standardizes textual data for consistent grouping and aggregation. |
[text](Data/transformed_sales_data.csv)
## 5\. Feature Engineering and Derived Metrics

New features are computed to enrich the dataset for deeper analysis:

  * **Temporal Features (Date Part Extraction):**
      * Extracts `'year'`, `'month'`, `'day'`, and `'day_of_week'` from the original `'date'` column.
      ![alt text](Screenshots/Date.jpg)
  * **Transaction Categorization:**
      * A custom function, `categorize_sales(x)`, is applied to the `'amount'` column to assign a categorical size (e.g., 'High', 'Medium', 'Low') to each transaction.
      ![alt text](<Screenshots/Sales Category.jpg>)
  * **Daily Aggregation and Joining:**
      * The total daily sales amount is computed per customer by grouping on `['date', 'customers']`.
      * This aggregated metric (`'daily_total'`) is then merged back into the main DataFrame, providing a high-level context for each individual transaction.

## 6\. Visualization

The final section of the notebook utilizes `seaborn` and `matplotlib` to perform basic exploratory visualization, including a bar plot to display the **Average Sales by Category**.

![alt text](<Visualizations/Average Sales by Category.png>)
![alt text](<Visualizations/Daily Sales Trend by Customer.png>)