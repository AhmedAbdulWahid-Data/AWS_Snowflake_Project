# 📊 Analytics Project: Yelp Reviews Sentiment and Data Analysis

## 🚀 Project Overview

This project processes and analyzes Yelp reviews to extract valuable insights using Snowflake and SQL. The dataset consists of **8.5GB** of JSON files containing Yelp reviews and business data. The analysis includes **sentiment analysis** and answering **10 business-related questions** using SQL queries on Snowflake.

# You can download the dataset 👉 [Here! 📂](https://business.yelp.com/data/resources/open-dataset/)

## 🔄 Workflow

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

🚀 Hope you find this project useful! Feel free to contribute or reach out with any questions. 😊
