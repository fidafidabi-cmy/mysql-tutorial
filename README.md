# Important MySQL Queries for Data Analytics

## 1. Database Operations
```sql
-- Create a database
CREATE DATABASE sales_db;

-- Select a database
USE sales_db;

-- Show all databases
SHOW DATABASES;

-- Delete a database
DROP DATABASE sales_db;
```

## 2. Table Operations
```sql
-- Create a table
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    order_date DATE,
    product VARCHAR(100),
    category VARCHAR(50),
    quantity INT,
    price DECIMAL(10,2)
);

-- Display all tables
SHOW TABLES;

-- View table structure
DESCRIBE orders;

-- Display all records
SELECT * FROM orders;

-- Add a new column
ALTER TABLE orders ADD COLUMN city VARCHAR(50);

-- Delete a table
DROP TABLE orders;
```

## 3. Data Manipulation (CRUD)
```sql
-- Insert records
INSERT INTO orders
VALUES (101, '2025-01-10', 'Laptop', 'Electronics', 2, 50000, 'Delhi');

-- Update records
UPDATE orders
SET price = 55000
WHERE order_id = 101;

-- Delete records
DELETE FROM orders
WHERE order_id = 101;

-- Retrieve specific columns
SELECT product, price FROM orders;
```

## 4. Filtering Data
```sql
-- WHERE condition
SELECT * FROM orders
WHERE price > 10000;

-- AND condition
SELECT * FROM orders
WHERE price > 10000 AND quantity > 2;

-- OR condition
SELECT * FROM orders
WHERE category = 'Electronics' OR category = 'Furniture';

-- BETWEEN
SELECT * FROM orders
WHERE price BETWEEN 10000 AND 50000;

-- IN operator
SELECT * FROM orders
WHERE city IN ('Delhi', 'Mumbai', 'Bangalore');

-- LIKE operator
SELECT * FROM orders
WHERE product LIKE 'L%';

-- IS NULL
SELECT * FROM orders
WHERE city IS NULL;
```

## 5. Sorting and Limiting
```sql
-- Sort in ascending order
SELECT * FROM orders
ORDER BY price ASC;

-- Sort in descending order
SELECT * FROM orders
ORDER BY price DESC;

-- Get top 5 records
SELECT * FROM orders
LIMIT 5;

-- Get top 5 expensive products
SELECT * FROM orders
ORDER BY price DESC
LIMIT 5;
```

## 6. Aggregate Functions
```sql
-- Total number of orders
SELECT COUNT(*) FROM orders;

-- Average price
SELECT AVG(price) FROM orders;

-- Maximum price
SELECT MAX(price) FROM orders;

-- Minimum price
SELECT MIN(price) FROM orders;

-- Total revenue (assuming price is unit price)
SELECT SUM(quantity * price) AS total_revenue
FROM orders;
```

## 7. GROUP BY and HAVING
```sql
-- Total sales by product
SELECT product, SUM(quantity * price) AS revenue
FROM orders
GROUP BY product;

-- Total sales by city
SELECT city, SUM(quantity * price) AS revenue
FROM orders
GROUP BY city;

-- Average price by category
SELECT category, AVG(price) AS avg_price
FROM orders
GROUP BY category;

-- Categories with revenue greater than 100000
SELECT category, SUM(quantity * price) AS revenue
FROM orders
GROUP BY category
HAVING revenue > 100000;
```

## 8. Joins
```sql
-- INNER JOIN
SELECT *
FROM orders o
INNER JOIN customers c
ON o.customer_id = c.customer_id;

-- LEFT JOIN
SELECT *
FROM orders o
LEFT JOIN customers c
ON o.customer_id = c.customer_id;

-- RIGHT JOIN
SELECT *
FROM orders o
RIGHT JOIN customers c
ON o.customer_id = c.customer_id;
```

## 9. Subqueries
```sql
-- Products with price above average
SELECT *
FROM orders
WHERE price > (
    SELECT AVG(price) FROM orders
);

-- Find the most expensive order
SELECT *
FROM orders
WHERE price = (
    SELECT MAX(price) FROM orders
);
```

## 10. Date Functions
```sql
-- Extract year
SELECT YEAR(order_date) FROM orders;

-- Extract month
SELECT MONTH(order_date) FROM orders;

-- Extract month name
SELECT MONTHNAME(order_date) FROM orders;

-- Sales by year
SELECT YEAR(order_date) AS year,
       SUM(quantity * price) AS revenue
FROM orders
GROUP BY YEAR(order_date);

-- Sales by month
SELECT MONTHNAME(order_date) AS month,
       SUM(quantity * price) AS revenue
FROM orders
GROUP BY MONTH(order_date), MONTHNAME(order_date)
ORDER BY MONTH(order_date);
```

## 11. CASE Statements
```sql
-- Categorize products based on price
SELECT product, price,
CASE
    WHEN price >= 50000 THEN 'Expensive'
    WHEN price >= 20000 THEN 'Medium'
    ELSE 'Affordable'
END AS price_category
FROM orders;
```

## 12. Window Functions
```sql
-- Rank products by price
SELECT product, price,
RANK() OVER (ORDER BY price DESC) AS price_rank
FROM orders;

-- Row number
SELECT product, price,
ROW_NUMBER() OVER (ORDER BY price DESC) AS row_num
FROM orders;

-- Running total of revenue
SELECT order_id, order_date,
       SUM(quantity * price) OVER (
           ORDER BY order_date, order_id
       ) AS running_revenue
FROM orders;

-- Average price by category
SELECT product, category, price,
       AVG(price) OVER (
           PARTITION BY category
       ) AS category_avg
FROM orders;
```

## 13. Common Table Expressions (CTE)
```sql
-- Calculate category-wise revenue
WITH category_sales AS (
    SELECT category,
           SUM(quantity * price) AS revenue
    FROM orders
    GROUP BY category
)
SELECT *
FROM category_sales
WHERE revenue > 100000;
```

## 14. Data Cleaning Queries
```sql
-- Find duplicate records
SELECT order_id, COUNT(*)
FROM orders
GROUP BY order_id
HAVING COUNT(*) > 1;

-- Find missing values
SELECT *
FROM orders
WHERE product IS NULL OR price IS NULL;

-- Replace NULL values
SELECT product,
       COALESCE(city, 'Unknown') AS city
FROM orders;

-- Remove leading and trailing spaces
SELECT TRIM(product)
FROM orders;

-- Convert text to uppercase
SELECT UPPER(product)
FROM orders;

-- Convert text to lowercase
SELECT LOWER(product)
FROM orders;
```

## 15. Important Analytical Queries
```sql
-- Total revenue
SELECT SUM(quantity * price) AS total_revenue
FROM orders;

-- Top 5 selling products by quantity
SELECT product, SUM(quantity) AS total_quantity
FROM orders
GROUP BY product
ORDER BY total_quantity DESC
LIMIT 5;

-- Top 5 products by revenue
SELECT product, SUM(quantity * price) AS revenue
FROM orders
GROUP BY product
ORDER BY revenue DESC
LIMIT 5;

-- Monthly revenue
SELECT DATE_FORMAT(order_date, '%Y-%m') AS month,
       SUM(quantity * price) AS revenue
FROM orders
GROUP BY month
ORDER BY month;

-- Number of orders by city
SELECT city, COUNT(*) AS total_orders
FROM orders
GROUP BY city
ORDER BY total_orders DESC;
```

---

### Most Important SQL Concepts for Data Analysts

1. SELECT, DISTINCT
2. WHERE, AND, OR, BETWEEN, IN, LIKE
3. ORDER BY, LIMIT
4. COUNT(), SUM(), AVG(), MIN(), MAX()
5. GROUP BY, HAVING
6. INNER JOIN, LEFT JOIN, RIGHT JOIN
7. Subqueries
8. Date and String Functions
9. CASE WHEN
10. Window Functions (RANK, ROW_NUMBER, LAG, LEAD)
11. CTE (WITH)
12. Data Cleaning and NULL Handling

**Tip:** Practice these queries using a real-world sales dataset. Focus especially on JOINs, GROUP BY, aggregate functions, window functions, and CTEs because they are frequently used in data analytics projects.