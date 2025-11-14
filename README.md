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
```sql
