# Retail-Sales-Analysis
## Project Overview
Project Title: Retail Sales Analysis
Level: Beginner
Database:Retail_sales_db

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries.

## Objectives
**Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
**Data Cleaning**: Identify and remove any records with missing or null values.
**Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
**Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

# Project Structure
## 1. Database Setup
**Database Creation**: The project starts by creating a database named Retail_sales_db.
**Table Creation**: A table named sales is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

CREATE DATABASE Retail_sales_db;

CREATE TABLE sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);

# 2. Data Exploration & Cleaning
**Record Count**: Determine the total number of records in the dataset.
**Customer Count**: Find out how many unique customers are in the dataset.
**Category Count**: Identify all unique product categories in the dataset.
**Null Value Check**: Check for any null values in the dataset and delete records with missing data.
## Data Exploration 
## How many sales records we have?

SELECT COUNT(*) FROM sales;

#### How many unique customers we have?

SELECT COUNT(DISTINCT customer_id) FROM sales;

## How many unique categories we have?

SELECT COUNT(DISTINCT category) FROM sales;

## Data Cleaning
SELECT * FROM sales
WHERE transactions_id IS NULL
	OR sale_date IS NULL
	OR sale_time IS NULL	
	OR customer_id IS NULL
	OR gender IS NULL
	OR Age IS NULL
	OR category IS NULL
	OR quantiy IS NULL
	OR price_per_unit IS NULL
	OR cogs IS NULL
	OR total_sale IS NULL;

DELETE FROM sales
WHERE transactions_id IS NULL
	OR sale_date IS NULL
	OR sale_time IS NULL	
	OR customer_id IS NULL
	OR gender IS NULL
	OR Age IS NULL
	OR category IS NULL
	OR quantiy IS NULL
	OR price_per_unit IS NULL
	OR cogs IS NULL
	OR total_sale IS NULL;

SELECT COUNT(*) FROM sales;

# 3. Data Analysis & Findings
## 1.Write a SQL query to retrieve all columns for sales made on '2022-11-05:

SELECT * 
FROM sales
WHERE sale_date = '2022-11-05';

## 2. Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022:

SELECT transactions_id, category, quantiy, sale_date
FROM sales
WHERE category='Clothing' 
AND quantiy >= 4
AND TO_CHAR(sale_date, 'YYYY-MM')='2022-11';

## 3.Write a SQL query to calculate the total sales (total_sale) for each category.:

SELECT category,
SUM(total_sale) Total_sales,
COUNT(*) Total_orders
FROM sales
GROUP BY category
ORDER BY Total_sales DESC;

## 4. Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.

SELECT category,ROUND(AVG(age),2)
FROM sales
WHERE category='Beauty'
GROUP BY category

## 5.Write a SQL query to find all transactions where the total_sale is greater than 1000.:

SELECT *
FROM sales
WHERE total_sale > 1000

## 6.Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.:

SELECT category,gender, 
COUNT(transactions_id) Total_transactions
FROM sales
GROUP BY category,gender
ORDER BY category

## 7.Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:

WITH CTE AS(
	SELECT EXTRACT(YEAR FROM sale_date) as Years,
	EXTRACT(MONTH FROM sale_date) as months,
	AVG(total_sale) Avg_sales,
	RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC)
	FROM sales
	GROUP BY Years,months
	)
	SELECT * 
	FROM CTE
	WHERE rank=1;

## 8.Write a SQL query to find the top 5 customers based on the highest total sales 

SELECT customer_id, SUM(total_sale) Total_sales
FROM sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;

## 9.Write a SQL query to find the number of unique customers who purchased items from each category.:

SELECT category,
COUNT(DISTINCT customer_id) unique_customers
FROM sales
GROUP BY category

## 10.Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):

WITH Hourly_sales AS(
	SELECT *,
	CASE
	WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
	WHEN EXTRACT(HOUR FROM sale_time) Between 12 AND 17 THEN 'Afternoon'
	WHEN EXTRACT(HOUR FROM sale_time) > 17 THEN 'Evening'
END as Shift
FROM sales
)
SELECT shift,COUNT(*) No_of_orders
FROM Hourly_sales
GROUP BY shift;

# Findings
**Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
**High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
**Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
Customer Insights: The analysis identifies the top-spending customers and the most popular product categories.

# Reports
**Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
**Trend Analysis**: Insights into sales trends across different months and shifts.
Customer Insights: Reports on top customers and unique customer counts per category.

# Conclusion
This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.












