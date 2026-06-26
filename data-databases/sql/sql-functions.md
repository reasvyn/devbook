# SQL Functions and Conditional Expressions

## Description

SQL provides a rich set of built-in functions for string manipulation, date/time arithmetic, numeric calculations, and conditional logic. This document covers the most commonly used functions across SQLite, PostgreSQL, and MySQL, with notes on syntax differences.

## Prerequisites

- [What Is SQL?](intro/what-is-sql.md) — basic understanding of SELECT queries and table structure
- [Querying Data](querying-data.md) — SELECT, WHERE, GROUP BY, and other core clauses

## Table of Contents

- [String Functions](#string-functions)
- [Date/Time Functions](#datetime-functions)
- [Numeric Functions](#numeric-functions)
- [Conditional Expressions](#conditional-expressions)
- [Study Cases](#study-cases)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

All examples use this sample table:

```sql
CREATE TABLE employees (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    department TEXT NOT NULL,
    salary INTEGER NOT NULL,
    joined_at TEXT NOT NULL,  -- ISO-8601 date string
    email TEXT
);

INSERT INTO employees VALUES
    (1, 'Alice',   'Engineering',    120000, '2019-03-15', 'alice@example.com'),
    (2, 'Bob',     'Engineering',    110000, '2020-07-01', 'bob@example.com'),
    (3, 'Charlie', 'Marketing',       85000, '2021-01-10', NULL),
    (4, 'Diana',   'Marketing',       95000, '2022-04-22', 'diana@example.com'),
    (5, 'Eve',     'Engineering',    130000, '2018-11-05', 'eve@example.com'),
    (6, 'Frank',   'Sales',           90000, '2023-06-12', 'frank@example.com'),
    (7, 'Grace',   'Sales',           92000, '2023-06-12', NULL),
    (8, 'Hank',    'Sales',          125000, '2017-09-30', 'hank@example.com'),
    (9, 'Ivy',     'Engineering',    115000, '2021-08-18', 'ivy@example.com'),
    (10, 'Jack',   'Marketing',       80000, '2023-12-01', 'jack@example.com');
```

### String Functions

#### UPPER and LOWER

Convert a string to uppercase or lowercase:

```sql
SELECT UPPER(name) AS upper_name, LOWER(department) AS lower_dept FROM employees;
```

```
UPPER_NAME  LOWER_DEPT
----------  ----------
ALICE       engineering
BOB         engineering
CHARLIE     marketing
```

#### LENGTH

Count characters in a string:

```sql
SELECT name, LENGTH(name) AS name_length FROM employees;
```

```
NAME    NAME_LENGTH
------  -----------
Alice   5
Bob     3
Charlie 7
```

LENGTH counts characters, not bytes. For byte length, some databases offer `OCTET_LENGTH` or `BIT_LENGTH`.

#### SUBSTR / SUBSTRING

Extract a substring. SQLite uses `SUBSTR`. PostgreSQL and MySQL accept both `SUBSTRING` and `SUBSTR`.

```sql
-- First 3 characters
SELECT name, SUBSTR(name, 1, 3) AS short_name FROM employees;
```

```
NAME    SHORT_NAME
------  ----------
Alice   Ali
Bob     Bob
Charlie Cha
```

```sql
-- Extract domain from email (everything after @)
SELECT name,
       SUBSTR(email, INSTR(email, '@') + 1) AS domain
FROM employees
WHERE email IS NOT NULL;
```

`INSTR` works in SQLite and MySQL. PostgreSQL uses `STRPOS()`.

```sql
-- PostgreSQL equivalent
SELECT name, SUBSTRING(email FROM POSITION('@' IN email) + 1) AS domain
FROM employees
WHERE email IS NOT NULL;
```

#### TRIM

Remove leading and trailing characters (default: whitespace):

```sql
SELECT TRIM('  hello  ') AS trimmed;          -- 'hello'
SELECT LTRIM('  hello') AS left_trimmed;      -- 'hello'
SELECT RTRIM('hello  ') AS right_trimmed;     -- 'hello'
```

```sql
-- Trim specific characters
SELECT TRIM('...hello...', '.') AS trimmed;   -- 'hello' (SQLite, PostgreSQL)
```

PostgreSQL uses `TRIM('.', '...hello...')` syntax. MySQL uses `TRIM('.' FROM '...hello...')`.

#### CONCAT / ||

Standard SQL uses `||` for string concatenation. MySQL uses `CONCAT()` (or `||` with `PIPES_AS_CONCAT` mode enabled).

```sql
-- PostgreSQL, SQLite
SELECT 'Mr./Ms. ' || name AS titled_name FROM employees;
```

```
TITLED_NAME
--------------
Mr./Ms. Alice
Mr./Ms. Bob
Mr./Ms. Charlie
```

```sql
-- MySQL
SELECT CONCAT('Mr./Ms. ', name) AS titled_name FROM employees;
```

```sql
-- Combining multiple columns
SELECT name || ' (' || department || ')' AS info FROM employees;
```

```
INFO
--------------------
Alice (Engineering)
Bob (Engineering)
Charlie (Marketing)
```

In MySQL with default settings:

```sql
SELECT CONCAT(name, ' (', department, ')') AS info FROM employees;
```

#### REPLACE

Replace all occurrences of a substring:

```sql
SELECT REPLACE(email, '@example.com', '@company.org') AS new_email
FROM employees
WHERE email IS NOT NULL;
```

```
NEW_EMAIL
---------------------
alice@company.org
bob@company.org
diana@company.org
```

```sql
-- Remove hyphens from phone numbers (conceptual)
SELECT REPLACE('555-123-4567', '-', '') AS clean_phone;  -- '5551234567'
```

### Date/Time Functions

Date handling varies significantly across databases. This section covers patterns and syntax differences.

#### Getting the current date and time

```sql
-- SQLite
SELECT DATE('now') AS today, TIME('now') AS now_time, DATETIME('now') AS now;
```

```sql
-- PostgreSQL
SELECT CURRENT_DATE AS today, CURRENT_TIME AS now_time, CURRENT_TIMESTAMP AS now;
```

```sql
-- MySQL
SELECT CURDATE() AS today, CURTIME() AS now_time, NOW() AS now;
```

#### Extracting parts of a date

```sql
-- SQLite: use SUBSTR or strftime
SELECT name, joined_at,
       SUBSTR(joined_at, 1, 4) AS year,
       SUBSTR(joined_at, 6, 2) AS month
FROM employees;
```

```sql
-- PostgreSQL: EXTRACT
SELECT name, joined_at,
       EXTRACT(YEAR FROM joined_at::DATE) AS year,
       EXTRACT(MONTH FROM joined_at::DATE) AS month
FROM employees;
```

```sql
-- MySQL: EXTRACT or YEAR()/MONTH()
SELECT name, joined_at,
       EXTRACT(YEAR FROM joined_at) AS year,
       MONTH(joined_at) AS month
FROM employees;
```

#### SQLite strftime format codes

```sql
SELECT name, joined_at,
       strftime('%Y', joined_at) AS year,
       strftime('%m', joined_at) AS month,
       strftime('%d', joined_at) AS day,
       strftime('%w', joined_at) AS day_of_week,  -- 0=Sunday
       strftime('%j', joined_at) AS day_of_year
FROM employees;
```

```sql
-- Date arithmetic: employees who joined in the last 3 years
SELECT name, joined_at
FROM employees
WHERE joined_at >= DATE('now', '-3 years');
```

#### PostgreSQL date functions

```sql
-- Age calculation
SELECT name, joined_at,
       AGE(joined_at::DATE) AS tenure
FROM employees;
```

```
NAME    JOINED_AT   TENURE
------  ----------  --------------------
Alice   2019-03-15  7 years 3 mons 11 days
Bob     2020-07-01  5 years 11 mons 25 days
```

```sql
-- Format dates with TO_CHAR
SELECT name, TO_CHAR(joined_at::DATE, 'YYYY-MM-DD') AS formatted_date FROM employees;
```

```sql
-- PostgreSQL: using INTERVAL for date arithmetic
SELECT name, joined_at
FROM employees
WHERE joined_at::DATE >= CURRENT_DATE - INTERVAL '3 years';
```

#### MySQL date functions

```sql
-- DATE_FORMAT
SELECT name, DATE_FORMAT(joined_at, '%Y-%m-%d') AS formatted_date FROM employees;
```

```sql
-- MySQL: date arithmetic with DATE_SUB
SELECT name, joined_at
FROM employees
WHERE joined_at >= DATE_SUB(CURDATE(), INTERVAL 3 YEAR);
```

```sql
-- DATEDIFF
SELECT name, DATEDIFF(CURDATE(), joined_at) AS days_since_joining FROM employees;
```

```sql
-- Extract day of week (1=Sunday, 2=Monday, ...)
SELECT name, DAYOFWEEK(joined_at) AS day_of_week FROM employees;
```

#### Common date function patterns

```sql
-- Employees who joined in 2021 (works in all three databases)
SELECT name, joined_at
FROM employees
WHERE joined_at >= '2021-01-01' AND joined_at < '2022-01-01';
```

```sql
-- Employees hired on a Monday (SQLite: day 1 = Monday)
SELECT name, joined_at
FROM employees
WHERE strftime('%w', joined_at) = '1';
```

```sql
-- PostgreSQL: Monday check
SELECT name, joined_at
FROM employees
WHERE EXTRACT(DOW FROM joined_at::DATE) = 1;
```

```sql
-- MySQL: Monday check (1=Sunday, 2=Monday)
SELECT name, joined_at
FROM employees
WHERE DAYOFWEEK(joined_at) = 2;
```

### Numeric Functions

#### ROUND

Round to a specified number of decimal places (or significant digits with negative precision):

```sql
SELECT AVG(salary) AS raw_avg,
       ROUND(AVG(salary), 2) AS rounded_avg
FROM employees;
```

```
RAW_AVG     ROUNDED_AVG
----------  -----------
104200.0    104200.00
```

```sql
-- Round to nearest thousand
SELECT ROUND(salary, -3) AS approx_salary
FROM employees;
```

```
APPROX_SALARY
-------------
120000
110000
85000
```

`ROUND` with negative precision works in PostgreSQL and MySQL. SQLite supports it as of version 3.35.0.

#### CEIL and FLOOR

CEIL rounds up to the nearest integer. FLOOR rounds down.

```sql
SELECT CEIL(5.1) AS up, FLOOR(5.9) AS down;  -- 6, 5
```

```sql
-- Ceiling the average salary per department
SELECT department, CEIL(AVG(salary)) AS avg_salary_ceiling
FROM employees
GROUP BY department;
```

```
DEPARTMENT    AVG_SALARY_CEILING
----------    ------------------
Engineering   118750
Marketing     86667
Sales         102334
```

#### ABS

Absolute value:

```sql
SELECT ABS(-42) AS absolute_value;  -- 42
```

```sql
-- Compute absolute deviation from an average (conceptual)
SELECT name, salary,
       ABS(salary - (SELECT AVG(salary) FROM employees)) AS deviation_from_avg
FROM employees;
```

#### MOD

Modulo (remainder after division). `MOD` works in PostgreSQL and MySQL. SQLite uses the `%` operator.

```sql
-- PostgreSQL, MySQL
SELECT name, id,
       CASE WHEN MOD(id, 2) = 0 THEN 'even' ELSE 'odd' END AS parity
FROM employees;
```

```sql
-- SQLite, MySQL (also works in PostgreSQL)
SELECT name, id,
       CASE WHEN id % 2 = 0 THEN 'even' ELSE 'odd' END AS parity
FROM employees;
```

```
NAME    ID  PARITY
------  --  -----
Alice   1   odd
Bob     2   even
Charlie 3   odd
```

### Conditional Expressions

#### CASE — simple form

The simple CASE compares an expression to multiple values:

```sql
SELECT name,
       CASE department
           WHEN 'Engineering' THEN 'Tech'
           WHEN 'Sales' THEN 'Revenue'
           WHEN 'Marketing' THEN 'Growth'
           ELSE 'Other'
       END AS category
FROM employees;
```

```
NAME    CATEGORY
------  ----------
Alice   Tech
Bob     Tech
Charlie Growth
Diana   Growth
Eve     Tech
Frank   Revenue
Grace   Revenue
Hank    Revenue
Ivy     Tech
Jack    Growth
```

#### CASE — searched form

The searched CASE evaluates independent boolean conditions:

```sql
SELECT name, salary,
       CASE
           WHEN salary < 90000 THEN 'Junior'
           WHEN salary BETWEEN 90000 AND 120000 THEN 'Mid'
           WHEN salary > 120000 THEN 'Senior'
       END AS level
FROM employees;
```

```
NAME    SALARY  LEVEL
------  ------  ------
Alice   120000  Mid
Bob     110000  Mid
Charlie 85000   Junior
Diana   95000   Mid
Eve     130000  Senior
Frank   90000   Mid
Grace   92000   Mid
Hank    125000  Senior
Ivy     115000  Mid
Jack    80000   Junior
```

#### CASE inside aggregate functions

CASE enables conditional aggregation — counting or summing only rows that satisfy a condition:

```sql
SELECT
    COUNT(*) AS total,
    COUNT(CASE WHEN salary < 90000 THEN 1 END) AS junior_count,
    COUNT(CASE WHEN salary BETWEEN 90000 AND 120000 THEN 1 END) AS mid_count,
    COUNT(CASE WHEN salary > 120000 THEN 1 END) AS senior_count
FROM employees;
```

```
TOTAL  JUNIOR_COUNT  MID_COUNT  SENIOR_COUNT
-----  ------------  ---------  ------------
10     2             6          2
```

```sql
-- Conditional sum: total salary by tier
SELECT
    SUM(CASE WHEN salary < 90000 THEN salary ELSE 0 END) AS junior_total,
    SUM(CASE WHEN salary BETWEEN 90000 AND 120000 THEN salary ELSE 0 END) AS mid_total,
    SUM(CASE WHEN salary > 120000 THEN salary ELSE 0 END) AS senior_total
FROM employees;
```

#### COALESCE

COALESCE returns the first non-NULL argument. Useful for substituting default values:

```sql
SELECT name, COALESCE(email, 'no-email@example.com') AS contact_email
FROM employees;
```

```
NAME    CONTACT_EMAIL
------  --------------------
Alice   alice@example.com
Bob     bob@example.com
Charlie no-email@example.com
Diana   diana@example.com
```

```sql
-- Multiple fallbacks in order
SELECT name, COALESCE(email, 'no-email', 'unknown') AS contact
FROM employees;
```

COALESCE can take any number of arguments. It stops at the first non-NULL.

#### NULLIF

NULLIF returns NULL if the two arguments are equal, otherwise returns the first argument:

```sql
-- Avoid division by zero
SELECT department, SUM(salary) / NULLIF(COUNT(*), 0) AS avg_salary
FROM employees
GROUP BY department;
```

```sql
-- Normalize empty strings to NULL
SELECT NULLIF(TRIM(COALESCE(email, '')), '') AS normalized_email
FROM employees;
```

```sql
-- Compare two columns; if they are equal, show NULL (indicating no change)
SELECT name, salary,
       NULLIF(salary, 100000) AS diff_from_100k
FROM employees;
```

If salary equals 100000, the result is NULL. Otherwise, the salary value is returned.

## Study Cases

### Case 1: Using CASE to create salary bands

**Problem:** A report requires employees grouped into salary bands (0-89999, 90000-119999, 120000+) with counts per department.

```sql
SELECT department,
       COUNT(CASE WHEN salary < 90000 THEN 1 END) AS band_low,
       COUNT(CASE WHEN salary BETWEEN 90000 AND 119999 THEN 1 END) AS band_mid,
       COUNT(CASE WHEN salary >= 120000 THEN 1 END) AS band_high
FROM employees
GROUP BY department
ORDER BY department;
```

```
DEPARTMENT    BAND_LOW  BAND_MID  BAND_HIGH
----------    --------  --------  ---------
Engineering   0         2         2
Marketing     2         1         0
Sales         0         2         1
```

**Key insight:** CASE inside COUNT counts only rows where the WHEN condition is true. Rows that do not match contribute 0 (COUNT ignores NULL, and CASE returns NULL when no condition matches if no ELSE is specified).

### Case 2: COALESCE for safe arithmetic

**Problem:** A bonus table has columns for performance_score and years_of_service, both nullable. The bonus formula is `salary * (score * 0.1 + years * 0.05)`. NULL values should be treated as zero.

```sql
-- Hypothetical bonus table (conceptual)
-- Avoid NULL propagation in arithmetic
SELECT e.name, e.salary,
       COALESCE(b.performance_score, 0) AS perf_score,
       COALESCE(b.years_of_service, 0) AS years,
       e.salary * (
           COALESCE(b.performance_score, 0) * 0.1 +
           COALESCE(b.years_of_service, 0) * 0.05
       ) AS bonus
FROM employees e
LEFT JOIN bonuses b ON e.id = b.employee_id;
```

Without COALESCE, if either column is NULL, the entire bonus expression becomes NULL. COALESCE ensures missing values are treated as zero.

### Case 3: Validating data with NULLIF

**Problem:** An import process stored empty strings in the email column instead of NULL. Normalize them for consistent querying.

```sql
-- First, see which emails are actually empty strings
SELECT name, email, LENGTH(email) AS len
FROM employees
WHERE email = '';
```

```sql
-- Update to set empty strings to NULL
UPDATE employees
SET email = NULLIF(TRIM(email), '')
WHERE email = '';
```

```sql
-- Verify
SELECT name, email FROM employees WHERE email IS NULL;
```

**Key insight:** NULLIF(TRIM(email), '') handles both empty strings and whitespace-only strings, converting them to NULL for consistent NULL handling throughout the application.

## Glossary

| Term | Definition |
|------|------------|
| COALESCE | Function returning the first non-NULL argument from a list |
| NULLIF | Function returning NULL if two arguments are equal, otherwise the first argument |
| CASE | Conditional expression returning a value based on WHEN conditions |
| Simple CASE | CASE form that compares an expression to fixed values using WHEN |
| Searched CASE | CASE form that evaluates independent boolean conditions in WHEN |
| strftime | SQLite date/time formatting function using `%` format codes |
| EXTRACT | SQL standard function for extracting date parts like YEAR, MONTH, DAY |
| TO_CHAR | PostgreSQL function for formatting dates and numbers as strings |
| DATE_FORMAT | MySQL function for formatting date values with `%` format codes |
| TRIM | Function that removes leading and trailing whitespace (or specified characters) |
| SUBSTR | Function that extracts a portion of a string from a given position |
| MOD | Function that returns the remainder of a division operation |

## Quick References

- [SQLite Core Functions](https://www.sqlite.org/lang_corefunc.html) — complete list of SQLite built-in scalar functions including string, numeric, and date functions
- [PostgreSQL Function Docs](https://www.postgresql.org/docs/current/functions.html) — comprehensive reference for PostgreSQL functions by category
- [MySQL String Functions](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html) — MySQL-specific string function reference
- [PostgreSQL Date/Time Functions](https://www.postgresql.org/docs/current/functions-datetime.html) — all date/time operators, functions, and formatting patterns in PostgreSQL
- [Modern SQL — CASE](https://modern-sql.com/feature/case) — in-depth explanation of CASE expression behavior and common use cases

## Next Steps

- [Querying Data](querying-data.md) — core SELECT, WHERE, GROUP BY, HAVING, subqueries, CTEs, and set operations
- [Joins & Relationships](../joins-and-relationships.md) — combining tables with INNER, LEFT, RIGHT, and CROSS JOIN
