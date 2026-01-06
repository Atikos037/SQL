# SQL Cheatsheet

A practical SQL reference for querying relational databases.  
Created as a personal knowledge base and portfolio resource.

---

## Table of Contents
- Introduction
- SELECT Basics
- Filtering Data
- Logical Operators & Sorting
- Functions
- NULL Handling & CASE
- JOINs
- Subqueries
- Best Practices

---

## Introduction

This repository contains a clean and structured SQL cheatsheet focused on:

- Selecting data
- Filtering and sorting results
- Using SQL functions
- Working with JOINs
- Writing subqueries

All examples use **generic table and column names** to keep the content reusable and database-agnostic.

---

## SELECT Basics

```sql
SELECT *
FROM staff;
sql
Code kopiëren
SELECT first_name, last_name
FROM staff;
sql
Code kopiëren
SELECT *
FROM staff
WHERE role_code = 'DEV';
Aliases
sql
Code kopiëren
SELECT salary AS monthly_salary
FROM staff;
Calculations
sql
Code kopiëren
SELECT salary * 12 AS yearly_salary
FROM staff;
Limiting Results
sql
Code kopiëren
SELECT TOP (5) *
FROM staff;
Filtering Data
DISTINCT
sql
Code kopiëren
SELECT DISTINCT team_id
FROM staff;
Comparison Operators
javascript
Code kopiëren
=   <>   !=   >   <   >=   <=
BETWEEN
sql
Code kopiëren
WHERE salary BETWEEN 4000 AND 9000;
IN
sql
Code kopiëren
WHERE country_code IN ('NL', 'BE', 'DE');
LIKE (Pattern Matching)
sql
Code kopiëren
WHERE last_name LIKE 'A%';
WHERE last_name LIKE '%son';
WHERE last_name LIKE '_o%';
NULL Checks
sql
Code kopiëren
WHERE manager_id IS NULL;
WHERE bonus IS NOT NULL;
Logical Operators & Sorting
AND / OR / NOT
sql
Code kopiëren
WHERE team_id = 10 AND salary > 3000;
sql
Code kopiëren
WHERE role_code = 'DEV' OR role_code = 'OPS';
Operator Precedence
Parentheses ()

NOT

AND

OR

sql
Code kopiëren
WHERE (team_id = 10 OR team_id = 20)
AND salary > 5000;
ORDER BY
sql
Code kopiëren
ORDER BY salary;
ORDER BY salary DESC;
ORDER BY 2;
ORDER BY yearly_salary;
Functions
String Functions
sql
Code kopiëren
LOWER(text)
UPPER(text)
LEFT(text, 2)
RIGHT(text, 2)
SUBSTRING(text, 1, 3)
REPLACE(text, 'old', 'new')
Numeric Functions
sql
Code kopiëren
ROUND(value, 2)
CEILING(value)
FLOOR(value)
Date Functions
sql
Code kopiëren
GETDATE()
YEAR(start_date)
MONTH(start_date)
DAY(start_date)
DATEDIFF(YEAR, start_date, GETDATE())
DATEADD(MONTH, 1, start_date)
Aggregate Functions
sql
Code kopiëren
MIN()
MAX()
SUM()
AVG()
COUNT()
COUNT(*)
COUNT(DISTINCT column)
Aggregate functions cannot be used directly in a WHERE clause.

NULL Handling & CASE
ISNULL / COALESCE
sql
Code kopiëren
ISNULL(bonus, 0)
COALESCE(bonus, commission, 0)
CASE
sql
Code kopiëren
SELECT
CASE
  WHEN salary >= 8000 THEN 'High'
  WHEN salary >= 4000 THEN 'Medium'
  ELSE 'Low'
END AS salary_level
FROM staff;
JOINs
INNER JOIN
sql
Code kopiëren
SELECT s.last_name, t.team_name
FROM staff s
INNER JOIN teams t
ON s.team_id = t.team_id;
LEFT JOIN
sql
Code kopiëren
SELECT s.last_name, t.team_name
FROM staff s
LEFT JOIN teams t
ON s.team_id = t.team_id;
Subqueries
Subquery with IN
sql
Code kopiëren
SELECT last_name
FROM staff
WHERE team_id IN (
  SELECT team_id
  FROM teams
  WHERE site_id = 100
);
Single-Value Subquery
sql
Code kopiëren
SELECT last_name
FROM staff
WHERE salary = (
  SELECT MAX(salary)
  FROM staff
);
EXISTS
sql
Code kopiëren
SELECT last_name
FROM staff s
WHERE EXISTS (
  SELECT 1
  FROM work_log w
  WHERE w.staff_id = s.staff_id
);
Best Practices
Write SQL readable (one clause per line)

Use clear aliases when joining tables

Avoid SELECT * in production

Use parentheses to control logic

Test subqueries separately

License
Free to use for learning and portfolio purposes.
