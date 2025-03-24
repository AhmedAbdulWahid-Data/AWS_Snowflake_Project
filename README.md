# 📊 Big Data Project: Yelp Reviews Sentiment and Analytics 

## 🚀 Project Overview

This project processes and analyzes Yelp reviews to extract valuable insights using Snowflake and SQL. The dataset consists of **8.5GB** of JSON files containing Yelp reviews and business data. The analysis includes **sentiment analysis** and answering **10 business-related questions** using SQL queries on Snowflake.

# You can download the dataset 👉 [Here! 📂](https://business.yelp.com/data/resources/open-dataset/)

# You can download my Snowflake guide 👉 [Here! 📂](https://github.com/user-attachments/files/19433334/Snowflake.pdf)

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
with cte as (
    select business_id, trim(A.value) as category
    from tbl_yelp_businesses
    ,lateral split_to_table(categories, ',') A
)
select category, count(*) as no_of_business
from cte
group by 1
order by 2 desc;
```


### 📌 Problem 2: Find the top 10 users who have reviewed the most businesses in the “Restaurants” category

```sql
select r.user_id, count(distinct r.business_id)
from tbl_yelp_reviews r
inner join tbl_yelp_businesses b on r.business_id = b.business_id
where b.categories ilike '%restaurant%'
group by 1
order by 2 desc
limit 10;
```


### 📌 Problem 3: Find the most popular categories of businesses (based on the number of reviews).

```sql
with cte as (
    select business_id, trim(A.value) as category
    from tbl_yelp_businesses
    ,lateral split_to_table(categories,',') A
)
select category, count(*) as no_of_reviews
from cte
inner join tbl_yelp_reviews r on cte.business_id = r.business_id
group by 1
order by 2 desc;
```


### 📌 Problem 4: Find the top 3 most recent reviews for each business.

```sql
with cte as (
    select r.*, b.name,
           row_number() over(partition by r.business_id order by review_date desc) as rn
    from tbl_yelp_reviews r
    inner join tbl_yelp_businesses b on r.business_id = b.business_id
)
select * from cte
where rn <= 3;
```


### 📌 Problem 5: Find the month with the highest number of reviews.

```sql
select month(review_date) as review_month, count(*) as no_of_reviews
from tbl_yelp_reviews
group by 1
order by 2 desc;
```


### 📌 Problem 6: Find the percentage of 5-star reviews for each business.

```sql
select 
    b.business_id,
    b.name, 
    count(*) as total_reviews,
    sum(case when r.review_stars = 5 then 1 else 0 end) as five_star_reviews,
    (sum(case when r.review_stars = 5 then 1 else 0 end) * 100.0 / count(*)) as five_star_percentage
from tbl_yelp_reviews r
inner join tbl_yelp_businesses b 
    on r.business_id = b.business_id
group by 1,2;
```


### 📌 Problem 7: Find the top 5 most reviewed businesses in each city.

```sql
with cte as (
    select 
        b.city, 
        b.business_id, 
        b.name, 
        count(*) as total_reviews
    from tbl_yelp_reviews r
    inner join tbl_yelp_businesses b 
        on r.business_id = b.business_id
    group by 1,2,3
)
select * 
from cte
qualify row_number() over (partition by city order by total_reviews desc) <= 5;
```


### 📌 Problem 8: Find the average rating of businesses that have at least 100 reviews.

```sql
select 
    b.business_id, 
    b.name, 
    count(*) as total_reviews, 
    avg(review_stars) as avg_rating
from tbl_yelp_reviews r
inner join tbl_yelp_businesses b 
    on r.business_id = b.business_id
group by 1,2
having count(*) >= 100;
```

### 📌 Problem 9: List the top 10 users who have written the most reviews, along with the businesses they reviewed.

```sql
with cte as (
    select r.user_id, count(*) as total_reviews
    from tbl_yelp_reviews r
    inner join tbl_yelp_businesses b on r.business_id = b.business_id
    group by 1
    order by 2 desc
    limit 10
)

select user_id, business_id
from tbl_yelp_reviews
where user_id in (select user_id from cte)
group by 1, 2
order by user_id;
```


### 📌 Problem 10: Find the top 10 businesses with the highest positive sentiment reviews.

```sql
select r.business_id, b.name, count(*) as total_reviews
from tbl_yelp_reviews r
inner join tbl_yelp_businesses b on r.business_id = b.business_id
where sentiments = 'Positive'
group by 1, 2
order by 3 desc
limit 10;
```


# 🚀 Hope you find this project useful! Feel free to contribute or reach out with any questions. 😊
