#  **Retail Sales Analysis SQL Project**

## 📌 Overview

This project is a **beginner-friendly SQL analysis** of retail sales data.
As a fresher in data analytics, I built this project to practice:

* Writing clean SQL queries
* Performing data cleaning
* Exploring sales data
* Solving real business questions using SQL

The project uses a single table **`retail_sales`**, containing transaction-level information such as date, time, customer details, product categories, pricing, and total sales.

This project helped me build confidence in SQL and understand how data analysts approach real-world business problems.

---

## 🎯 Project Goals

✔ Create and set up a SQL database
✔ Clean the dataset (remove NULLs, validate fields)
✔ Perform basic exploratory data analysis
✔ Solve 25+ business queries
✔ Generate insights about customers, products, and sales trends

---

## 🏗️ Database Setup

```sql
CREATE DATABASE sql_project_p2;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

---

## 🧹 Data Cleaning & Exploration

### 🔍 Basic Checks

* Count of records
* Unique customers
* Product categories
* NULL value detection

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;
```

###  Remove NULL Records

```sql
DELETE FROM retail_sales
WHERE sale_date IS NULL
   OR sale_time IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR age IS NULL
   OR category IS NULL
   OR quantity IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
```

---

## 📊 Business Analysis (25+ Queries)

Here are some examples of the business problems solved in the project:

### 1️⃣ Sales on a specific date

```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

### 2️⃣ Clothing purchases (Quantity > 4) in Nov 2022

```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
```

### 3️⃣ Total revenue for each category

```sql
SELECT 
    category,
    SUM(total_sale) AS net_sale
FROM retail_sales
GROUP BY category;
```

### 4️⃣ Top 5 customers by total spending

```sql
SELECT
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

### 5️⃣ Best-selling month in each year

```sql
SELECT year, month, avg_sale
FROM (
    SELECT
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date)
                     ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY 1,2
) t
WHERE rank = 1;
```

### 6️⃣ Weekend revenue

```sql
SELECT
    SUM(total_sale) AS weekend_sales
FROM retail_sales
WHERE EXTRACT(DOW FROM sale_date) IN (6, 0);
```

### 7️⃣ Profit per category

```sql
SELECT
    category,
    SUM(total_sale - cogs) AS total_profit
FROM retail_sales
GROUP BY category;
```

### 8️⃣ Customers who increased spending month-over-month

(Advanced Query)

```sql
WITH monthly_spend AS (
    SELECT
        customer_id,
        DATE_TRUNC('month', sale_date) AS month,
        SUM(total_sale) AS monthly_revenue
    FROM retail_sales
    GROUP BY customer_id, month
),
compare AS (
    SELECT
        customer_id,
        month,
        monthly_revenue,
        LAG(monthly_revenue) OVER (PARTITION BY customer_id ORDER BY month) AS prev
    FROM monthly_spend
)
SELECT *
FROM compare
WHERE prev IS NOT NULL
  AND monthly_revenue > prev;
```

---

## ⭐ Key Insights

Here are some insights I discovered from the analysis:

* Several customers made **high-value purchases** (₹1000+).
* Categories like **Clothing and Beauty** show high engagement.
* Weekends have **stronger sales activity**.
* Best-performing months can help identify **seasonal patterns**.
* Afternoon and Evening shifts show significant order counts.
* Some customers show **increasing monthly spending**, useful for retention strategies.

This analysis helped me understand how data can support retail business decisions.

---

## 📁 Recommended Folder Structure (for GitHub)

```
Retail-Sales-SQL-Project/
│
├── Retail_Sales(SQL_Project).sql   # Full SQL script
├── README.md                       # Project documentation
└── data                            # CSV file

---

## 🚀 How to Run This Project

1. Open PostgreSQL / MySQL / DBeaver
2. Create the database
3. Run the SQL script
4. Execute queries one by one
5. Explore insights and modify queries as needed

---

## 🎓 Final Thoughts

As a fresher in the data field, this project helped me practice:

* Writing SQL queries
* Cleaning and preparing data
* Understanding customer and sales patterns
* Using analytical SQL functions
* Extracting real business insights

