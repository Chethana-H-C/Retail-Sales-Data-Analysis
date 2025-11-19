# Retail-Sales-Data-Analysis
From data to decisions: Retail sales analysis using SQL to reveal patterns, performance, and key metrics.

**Project Overview**

This project demonstrates an end-to-end SQL data analysis of a retail sales dataset. It includes table creation, data cleaning, data exploration, and analytical queries to extract valuable business insights such as top customers, sales trends, and category-wise performance.

The goal of this project is to:

•Understand customer purchasing behavior

•Identify sales trends by category, time, and demographics

•Apply SQL for data cleaning, aggregation, and analytical reporting

**1. Table Creation**

Created a table named retail_sales to store retail transaction details including sales date, customer demographics, item category, and sales values.

```sql
CREATE TABLE retail_sales (
    transaction_id INT PRIMARY KEY NOT NULL,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity FLOAT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

**2. Data Cleaning**

• Checked for missing values in all key columns.

• Removed null and incomplete transaction records.

• Verified correct data types for all fields (DATE, TIME, FLOAT, etc.).

• Ensured all transactions_id values were unique (Primary Key).Checked and removed rows with missing (NULL) values to maintain data quality and accuracy.

```sql
-- Check for missing values

SELECT * 
FROM retail_sales
WHERE
    transactions_id IS NULL
    OR sale_date IS NULL
    OR sale_time IS NULL
    OR customer_id IS NULL
    OR gender IS NULL
    OR age IS NULL
    OR category IS NULL
    OR quantity IS NULL
    OR price_per_unit IS NULL
    OR cogs IS NULL
    OR total_sale IS NULL;

-- Delete rows with missing values

DELETE FROM retail_sales
WHERE
    transactions_id IS NULL
    OR sale_date IS NULL
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

**3. Data Exploration**

To gain an initial understanding of the dataset, several exploratory SQL queries were performed. These queries help analyze the volume of transactions, customer diversity, and the range of product categories in the retail dataset.

Key Exploration Queries:

```sql

-- Total number of sales transactions

SELECT COUNT(*) AS total_sales
FROM retail_sales;

-- Total number of unique customers

SELECT COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales;

-- Total number of product categories available

SELECT COUNT(DISTINCT category) AS total_categories
FROM retail_sales;

-- Retrieve a list of all distinct product categories

SELECT DISTINCT category
FROM retail_sales;

```
**4. Data Analysis & Insights**
   
Below are some key business questions answered using SQL queries on the retail_sales dataset. These queries help in better understaning of how sales are performing, what customers prefer, and which products are doing well.

```sql
-- Q1. What sales happened on 2022-11-05?

SELECT * 
FROM retail_sales 
WHERE sale_date = '2022-11-05';

-- Q2. Which clothing orders in Nov 2022 had more than 4 items?

SELECT * 
FROM retail_sales
WHERE category = 'Clothing'
  AND quantity > 4
  AND sale_date BETWEEN '2022-11-01' AND '2022-11-30';

-- Q3. How much revenue did each category generate?

SELECT category, SUM(total_sale) AS total_sales_per_category
FROM retail_sales
GROUP BY category;

-- Q4. What is the average age of customers buying Beauty products?

SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';

-- Q5. Which transactions had a bill amount above ₹1000?

SELECT * 
FROM retail_sales 
WHERE total_sale > 1000;

-- Q6. How many transactions occurred by gender for each category?

SELECT category, gender, COUNT(transactions_id) AS total_transactions
FROM retail_sales
GROUP BY category, gender
ORDER BY category;

-- Q7. Which month performed the best each year?

SELECT *
FROM (
    SELECT sale_year, sale_month, avg_sales,
           RANK() OVER (PARTITION BY sale_year ORDER BY avg_sales DESC) AS month_rank
    FROM (
        SELECT YEAR(sale_date) AS sale_year,
               MONTH(sale_date) AS sale_month,
               ROUND(AVG(total_sale), 2) AS avg_sales
        FROM retail_sales
        GROUP BY YEAR(sale_date), MONTH(sale_date)
    ) AS monthly_avg
) AS ranked_months
WHERE month_rank = 1
ORDER BY sale_year;

-- Q8. Who are the top 5 customers based on total spending?

SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;

-- Q9. How many unique customers did each category attract?

SELECT category, COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;

-- Q10. What time of the day gets the most orders? (Shift Analysis)

SELECT
    CASE
        WHEN HOUR(sale_time) < 12 THEN 'Morning'
        WHEN HOUR(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END AS shift,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY shift;
