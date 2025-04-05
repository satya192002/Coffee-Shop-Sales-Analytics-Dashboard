# Coffee Shop Sales Analysis

Welcome to the **Coffee Shop Sales Analysis** project! This repository contains SQL queries and a sample dataset to analyze sales performance for a coffee shop chain. The project focuses on extracting key performance indicators (KPIs) such as total sales, orders, and quantities sold, along with trends and comparisons across time periods, locations, product categories, and more.

## Table of Contents
- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Prerequisites](#prerequisites)
- [Setup](#setup)
- [SQL Queries](#sql-queries)
- [Usage](#usage)

## Project Overview
This project leverages SQL to analyze transactional data from a coffee shop chain operating in multiple locations. The analysis includes:
- Monthly and daily sales trends.
- Month-over-month (MoM) growth comparisons for sales, orders, and quantities.
- Sales breakdowns by store location, product category, and product type.
- Performance metrics by weekdays/weekends and hourly sales for operational insights.

The dataset and SQL scripts are designed to help stakeholders understand sales performance, identify top-performing products, and optimize business operations.

## Dataset
The dataset (`coffee_shop_sales.csv`) contains transactional data with the following columns:
- `transaction_id`: Unique identifier for each transaction.
- `transaction_date`: Date of the transaction (initially in `DD-MM-YYYY` format).
- `transaction_time`: Time of the transaction (in `HH:MM:SS` format).
- `transaction_qty`: Quantity of items sold in the transaction.
- `store_id`: Identifier for the store.
- `store_location`: Location of the store (e.g., Lower Manhattan, Hell's Kitchen, Astoria).
- `product_id`: Identifier for the product.
- `unit_price`: Price per unit of the product.
- `product_category`: Category of the product (e.g., Coffee, Tea, Bakery).
- `product_type`: Specific type of product (e.g., Gourmet brewed coffee, Scone).
- `product_detail`: Detailed description of the product (e.g., Ethiopia Rg, Oatmeal Scone).

A sample of the dataset is included in this repository, covering transactions from January 1, 2023, to June 30, 2023.

## Prerequisites
To run this project, you’ll need:
- A SQL-compatible database (e.g., MySQL, PostgreSQL, SQLite).
- A tool to execute SQL queries (e.g., MySQL Workbench, DBeaver, or command-line interface).
- Basic knowledge of SQL for modifying or extending the queries.

## Setup
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-username/coffee-shop-sales-analysis.git
   cd coffee-shop-sales-analysis
   ```

2. **Set Up the Database**:
   - Create a new database in your SQL environment:
     ```sql
     CREATE DATABASE coffee_shop_db;
     USE coffee_shop_db;
     ```
   - Import the dataset (`coffee_shop_sales.csv`) into a table:
     ```sql
     CREATE TABLE coffee_shop_sales (
         transaction_id INT,
         transaction_date VARCHAR(10),
         transaction_time VARCHAR(8),
         transaction_qty INT,
         store_id INT,
         store_location VARCHAR(50),
         product_id INT,
         unit_price DECIMAL(5,2),
         product_category VARCHAR(50),
         product_type VARCHAR(50),
         product_detail VARCHAR(100)
     );
     ```
   - Load the CSV data (example for MySQL):
     ```sql
     LOAD DATA INFILE 'path/to/coffee_shop_sales.csv'
     INTO TABLE coffee_shop_sales
     FIELDS TERMINATED BY ',' 
     ENCLOSED BY '"' 
     LINES TERMINATED BY '\n'
     IGNORE 1 ROWS;
     ```

3. **Run Initial Data Preparation Queries**:
   - Execute the data transformation steps from the SQL script to convert `transaction_date` and `transaction_time` to proper date and time formats:
     ```sql
     UPDATE coffee_shop_sales
     SET transaction_date = STR_TO_DATE(transaction_date, '%d-%m-%Y');
     
     ALTER TABLE coffee_shop_sales
     MODIFY COLUMN transaction_date DATE;

     UPDATE coffee_shop_sales
     SET transaction_time = STR_TO_DATE(transaction_time, '%H:%i:%s');

     ALTER TABLE coffee_shop_sales
     MODIFY COLUMN transaction_time TIME;
     ```

4. **Verify the Table Structure**:
   ```sql
   DESCRIBE coffee_shop_sales;
   ```

## SQL Queries
The `queries.sql` file contains a collection of SQL queries for analyzing the coffee shop sales data. Key analyses include:
- **Total Sales**: Monthly sales aggregated by month name.
- **MoM Growth**: Month-over-month sales, orders, and quantity sold comparisons for April and May.
- **Daily Sales**: Sales, quantities, and orders for specific days (e.g., May 18, 2023).
- **Sales Trends**: Average daily sales and comparisons to identify above/below average days.
- **Breakdowns**: Sales by weekday/weekend, store location, product category, and top 10 products.
- **Hourly Analysis**: Sales, quantities, and orders by specific hours and days (e.g., Tuesday at 8 AM).

To run a query, copy it from `queries.sql` and execute it in your SQL environment.

## Usage
1. Open your SQL client and connect to the `coffee_shop_db` database.
2. Load and run the desired query from `queries.sql`. For example:
   - To calculate total sales by product category for May:
     ```sql
     SELECT 
         product_category,
         ROUND(SUM(unit_price * transaction_qty), 1) AS Total_Sales
     FROM coffee_shop_sales
     WHERE MONTH(transaction_date) = 5
     GROUP BY product_category
     ORDER BY Total_Sales DESC;
     ```
3. Export results to CSV or visualize them using a tool like Tableau or Power BI for further analysis.
