# 📊 Analytics Project: Yelp Reviews Sentiment and Data Analysis

## 🚀 Project Overview

This project processes and analyzes Yelp reviews to extract valuable insights using Snowflake and SQL. The dataset consists of **8.5GB** of JSON files containing Yelp reviews and business data. The analysis includes **sentiment analysis** and answering **10 business-related questions** using SQL queries on Snowflake.

# You can download the dataset 👉 [Here! 📂](https://business.yelp.com/data/resources/open-dataset/)

---

## 🔄 Workflow

<img width="1304" alt="Screenshot 2025-03-23 at 1 00 00 PM" src="https://github.com/user-attachments/assets/da64924d-5a40-470a-9ac7-19d696f04767" />


### 📥 Data Ingestion
- 📂 **Yelp reviews** (5GB - 7 million records) and **business data** (100MB) in JSON format.
- 🐍 A **Python program** processes and splits the JSON files.

### ☁️ Cloud Storage
- 📦 Processed JSON files are stored in an **Amazon S3** bucket.

### ❄️ Data Warehousing with Snowflake
- 📡 JSON data is **ingested into Snowflake**.
- 🗄️ The data is **flattened into structured tables**.

### 🤖 Sentiment Analysis
- 🛠️ A **User-Defined Function (UDF)** is applied to perform **sentiment analysis** on reviews.

### 🧐 SQL-Based Data Analysis
- 🔎 **10 business-related questions** were answered using **SQL queries** in Snowflake.

## 🛠️ Technologies Used

- 🐍 **Python**: Data processing and transformation.
- ☁️ **Amazon S3**: Cloud storage for JSON files.
- ❄️ **Snowflake**: Data warehousing and analytics.
- 🗄️ **SQL**: Data querying and analysis.

---
## 🔢 SQL Business Questions and Solutions

### 📌 Problem 1: Find the number of businesses in each category

```sql
-- Find number of businesses in each category
with cte as (
    select business_id, trim(A.value) as category
    from tbl_yelp_businesses
    ,lateral split_to_table(categories, ',') A
)
select category, count(*) as no_of_business
from cte
group by 1;
```

🚀 Hope you find this project useful! Feel free to contribute or reach out with any questions. 😊
