# SQL Cheat Sheet for Data Analytics

**Legend:**

- **DQL** — Data Query Language
- **DML** — Data Manipulation Language
- **DDL** — Data Definition Language

---

## 1. Basic Queries

**SELECT, DISTINCT**

```sql
SELECT col1, col2, DISTINCT col3
FROM table_name;
```

**FROM**

```sql
SELECT *
FROM table_name;
```

Operators: `AND`, `OR`, `NOT`, `BETWEEN`, `IN`, `LIKE`, `IS NULL`, `IS NOT NULL`

**WHERE**

```sql
SELECT *
FROM table_name
WHERE condition;
```

---

## 2. Sorting & Limiting

**ORDER BY**

```sql
SELECT * FROM table_name
ORDER BY col1 ASC;  -- default ASC
ORDER BY col1 DESC;
```

**LIMIT / TOP / OFFSET**

```sql
-- Standard (LIMIT / OFFSET)
SELECT * FROM table_name
ORDER BY col1
LIMIT 10 OFFSET 20;

-- SQL Server (TOP)
SELECT TOP 10 * FROM table_name
ORDER BY col1;
```

---

## 3. Aggregations

**Aggregate Functions**

```sql
SELECT COUNT(*)   AS cnt,
       SUM(sales) AS total_sales,
       AVG(sales) AS avg_sales,
       MIN(sales) AS min_sales,
       MAX(sales) AS max_sales
FROM orders;
```

**GROUP BY**

```sql
SELECT region, SUM(sales)
FROM orders
GROUP BY region;
```

**HAVING** _(filter on aggregates)_

```sql
SELECT region, SUM(sales) AS total_sales
FROM orders
GROUP BY region
HAVING SUM(sales) > 10000;
```

---

## 4. Joins

**INNER JOIN**

```sql
SELECT *
FROM tableA A
INNER JOIN tableB B
  ON A.id = B.id;
```

**LEFT JOIN**

```sql
SELECT *
FROM tableA A
LEFT JOIN tableB B
  ON A.id = B.id;
```

**RIGHT JOIN**

```sql
SELECT *
FROM tableA A
RIGHT JOIN tableB B
  ON A.id = B.id;
```

**FULL JOIN**

```sql
SELECT *
FROM tableA A
FULL OUTER JOIN tableB B
  ON A.id = B.id;
```

> ⚠️ Note: FULL OUTER JOIN is not supported in all databases (e.g., MySQL).

---

## 5. Subqueries & CTEs

**Subquery in SELECT**

```sql
SELECT name,
  (SELECT MAX(order_date)
   FROM orders o
   WHERE o.customer_id = c.id) AS last_order
FROM customers c;
```

**Subquery in WHERE**

```sql
SELECT * FROM employees
WHERE department_id IN
  (SELECT id FROM departments
   WHERE location = 'New York');
```

**WITH (CTE)**

```sql
WITH sales_cte AS (
  SELECT region, SUM(sales) AS total_sales
  FROM orders
  GROUP BY region
)
SELECT * FROM sales_cte
WHERE total_sales > 10000;
```

---

## 6. Window Functions

**ROW_NUMBER()**

```sql
SELECT col1, col2,
       ROW_NUMBER() OVER (ORDER BY col1) AS rn
FROM table_name;
```

**RANK() / DENSE_RANK()**

```sql
SELECT col1, col2,
       RANK()       OVER (ORDER BY col1) AS rnk,
       DENSE_RANK() OVER (ORDER BY col1) AS drnk
FROM table_name;
```

**PARTITION BY**

```sql
SELECT department, employee, salary,
       ROW_NUMBER() OVER (PARTITION BY department
                          ORDER BY salary DESC) AS rn
FROM employees;
```

> 💡 Use window functions for running totals, rankings, moving averages, and more.

---

## 7. Conditional Logic

```sql
SELECT name, salary,
  CASE
    WHEN salary >= 100000 THEN 'High'
    WHEN salary >= 50000  THEN 'Medium'
    ELSE 'Low'
  END AS salary_level
FROM employees;
```

---

## 8. Data Manipulation (DML)

**INSERT**

```sql
INSERT INTO table_name (col1, col2)
VALUES (value1, value2);
```

**UPDATE**

```sql
UPDATE table_name
SET col1 = value1
WHERE condition;
```

**DELETE**

```sql
DELETE FROM table_name
WHERE condition;
```

> ⚠️ Always use WHERE in UPDATE and DELETE!

---

## 9. Table Operations (DDL Basics)

**CREATE TABLE**

```sql
CREATE TABLE employees (
  id     INT PRIMARY KEY,
  name   VARCHAR(100) NOT NULL,
  salary DECIMAL(10,2)
);
```

**ALTER TABLE**

```sql
ALTER TABLE employees
ADD COLUMN email VARCHAR(100);
```

**DROP TABLE**

```sql
DROP TABLE employees;
```

---

## 10. String Functions

```sql
-- CONCAT
CONCAT(a, b)
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM employees;

-- SUBSTRING
SUBSTRING(str, start, length)
SELECT SUBSTRING(name, 1, 3) FROM employees;

-- LENGTH
SELECT LENGTH(name) FROM employees;

-- TRIM
SELECT TRIM('  hello  ') AS clean;

-- UPPER / LOWER
SELECT UPPER(name), LOWER(name) FROM employees;
```

---

## 11. Date Functions

**CURRENT_DATE**

```sql
SELECT CURRENT_DATE;
```

**DATEADD / INTERVAL** _(syntax varies)_

```sql
-- Add 7 days
SELECT DATEADD(day, 7, CURRENT_DATE);

-- ANSI SQL
SELECT CURRENT_DATE + INTERVAL '7' DAY;
```

**DATEDIFF** _(syntax varies)_

```sql
-- Days between two dates
SELECT DATEDIFF(day, ...);
```

---

## 12. Set Operations

**UNION** _(remove duplicates)_

```sql
SELECT col FROM tableA
UNION
SELECT col FROM tableB;
```

**UNION ALL** _(keep duplicates)_

```sql
SELECT col FROM tableA
UNION ALL
SELECT col FROM tableB;
```

**INTERSECT** _(common rows)_

```sql
SELECT col FROM tableA
INTERSECT
SELECT col FROM tableB;
```

**EXCEPT** _(A minus B)_

```sql
SELECT col FROM tableA
EXCEPT
SELECT col FROM tableB;
```

---

## 13. Keys & Indexes

**PRIMARY KEY**

Ensures unique, not null values in a column.

```sql
CREATE TABLE users (
  id   INT PRIMARY KEY,
  name VARCHAR(100)
);
```

**FOREIGN KEY**

```sql
CREATE TABLE orders (
  id      INT PRIMARY KEY,
  user_id INT,
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

---

## 14. Common Tips

**NULL Handling**

Use `COALESCE` / `IFNULL` to replace NULLs.

```sql
SELECT COALESCE(phone, 'N/A') AS phone
FROM employees;
```

**Aliases (AS)**

Use aliases for readability.

```sql
SELECT e.name AS employee_name,
       d.name AS department_name
FROM employees e
JOIN departments d ON e.dept_id = d.id;
```