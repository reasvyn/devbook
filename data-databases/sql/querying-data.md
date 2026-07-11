# Querying Data with SQL SELECT

## Description

The SELECT statement is the foundation of reading data from a relational database. This document covers every clause of a SELECT query — projection, filtering, sorting, aggregation, subqueries, CTEs, and set operations — with practical examples that work across SQLite, PostgreSQL, and MySQL.

## Prerequisites

- [What Is SQL?](intro/what-is-sql.md) — tables, rows, columns, and the basic SELECT syntax

## Table of Contents

- [SELECT Clause](#select-clause)
- [WHERE Clause](#where-clause)
- [ORDER BY](#order-by)
- [LIMIT and OFFSET](#limit-and-offset)
- [Aggregate Functions](#aggregate-functions)
- [GROUP BY](#group-by)
- [HAVING](#having)
- [Subqueries](#subqueries)
- [Common Table Expressions (CTEs)](#common-table-expressions-ctes)
- [Set Operations](#set-operations)
- [Performance Awareness](#performance-awareness)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

All examples use this schema:

```sql
CREATE TABLE employees (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    department TEXT NOT NULL,
    role TEXT NOT NULL,
    salary INTEGER NOT NULL,
    joined_at TEXT NOT NULL,
    email TEXT
);

INSERT INTO employees VALUES
    (1, 'Alice',   'Engineering', 'Backend',  120000, '2019-03-15', 'alice@example.com'),
    (2, 'Bob',     'Engineering', 'Frontend', 110000, '2020-07-01', 'bob@example.com'),
    (3, 'Charlie', 'Marketing',   'Content',   85000, '2021-01-10', NULL),
    (4, 'Diana',   'Marketing',   'Analytics', 95000, '2022-04-22', 'diana@example.com'),
    (5, 'Eve',     'Engineering', 'Backend',  130000, '2018-11-05', 'eve@example.com'),
    (6, 'Frank',   'Sales',       'Account',   90000, '2023-06-12', 'frank@example.com'),
    (7, 'Grace',   'Sales',       'Account',   92000, '2023-06-12', NULL),
    (8, 'Hank',    'Sales',       'Lead',     125000, '2017-09-30', 'hank@example.com'),
    (9, 'Ivy',     'Engineering', 'Frontend', 115000, '2021-08-18', 'ivy@example.com'),
    (10, 'Jack',   'Marketing',   'Content',   80000, '2023-12-01', 'jack@example.com');
```

### SELECT clause

#### Choosing columns

```sql
SELECT name, department, salary FROM employees;
```

```sql
SELECT * FROM employees;
```

`SELECT *` returns all columns in table order. Convenient for exploration but discouraged in production (see Performance Awareness).

#### Expressions and aliases

```sql
SELECT name, salary * 1.1 AS proposed_raise FROM employees;
SELECT name, salary - (salary * 0.25) AS after_tax FROM employees;
```

AS renames a column; the keyword is optional:

```sql
SELECT name AS employee_name, department dept FROM employees;
```

Aliases are visible in ORDER BY but not in WHERE or FROM within the same level.

#### DISTINCT

```sql
SELECT DISTINCT department FROM employees;
SELECT DISTINCT department, role FROM employees;
SELECT COUNT(DISTINCT department) FROM employees;
```

DISTINCT considers the combination of all selected columns.

### WHERE clause

#### Comparison operators

```sql
SELECT name, salary FROM employees WHERE salary > 100000;
SELECT name, department FROM employees WHERE department <> 'Sales';
```

Operators: `=`, `<>` or `!=`, `>`, `<`, `>=`, `<=`.

#### Logical operators

```sql
SELECT name, salary FROM employees WHERE salary > 100000 AND department = 'Engineering';
SELECT name, department FROM employees WHERE department = 'Sales' OR department = 'Marketing';
SELECT name, department FROM employees WHERE NOT department = 'Engineering';
SELECT name, salary FROM employees WHERE (department = 'Engineering' OR department = 'Sales') AND salary >= 100000;
```

Precedence: NOT > AND > OR. Use parentheses for clarity.

#### IN

```sql
SELECT name, department FROM employees WHERE department IN ('Engineering', 'Sales');
```

IN also works with subqueries.

#### BETWEEN

```sql
SELECT name, salary FROM employees WHERE salary BETWEEN 90000 AND 120000;
SELECT name, joined_at FROM employees WHERE joined_at BETWEEN '2020-01-01' AND '2022-12-31';
```

BETWEEN is inclusive.

#### LIKE

`%` matches any sequence; `_` matches one character.

```sql
SELECT name FROM employees WHERE name LIKE 'A%';
SELECT name FROM employees WHERE name LIKE '%e';
SELECT name FROM employees WHERE name LIKE '%a%';
SELECT name FROM employees WHERE name LIKE '____';
SELECT name, email FROM employees WHERE email LIKE '%@example.com';
```

LIKE on NULL is NULL (falsy). SQLite LIKE is case-insensitive for ASCII. PostgreSQL uses `ILIKE` for case-insensitive.

#### IS NULL

```sql
SELECT name, email FROM employees WHERE email IS NULL;
SELECT name, email FROM employees WHERE email IS NOT NULL;
```

Never write `= NULL` — it always evaluates to NULL.

### ORDER BY

```sql
SELECT name, salary FROM employees ORDER BY salary;
SELECT name, salary FROM employees ORDER BY salary DESC;
SELECT name, department, salary FROM employees ORDER BY salary DESC, name ASC;
SELECT name, salary FROM employees ORDER BY salary * 1.1 DESC;
```

Control NULL placement:

```sql
SELECT name, email FROM employees ORDER BY email ASC NULLS LAST;
SELECT name, email FROM employees ORDER BY email DESC NULLS FIRST;
```

NULLS FIRST/LAST supported in SQLite and PostgreSQL but not MySQL.

### LIMIT and OFFSET

```sql
SELECT name, salary FROM employees ORDER BY salary DESC LIMIT 3;
SELECT name, salary FROM employees ORDER BY salary DESC LIMIT 3 OFFSET 0;  -- page 1
SELECT name, salary FROM employees ORDER BY salary DESC LIMIT 3 OFFSET 3;  -- page 2
```

`LIMIT {count} OFFSET {skip}` works in all three databases.

### Aggregate functions

```sql
SELECT COUNT(*) FROM employees;
SELECT COUNT(email) FROM employees;  -- counts non-NULL only
SELECT COUNT(DISTINCT department) FROM employees;
SELECT SUM(salary) FROM employees;
SELECT AVG(salary) FROM employees;
SELECT ROUND(AVG(salary), 2) FROM employees;
SELECT MIN(salary), MAX(salary) FROM employees;
SELECT MIN(joined_at), MAX(joined_at) FROM employees;
SELECT MIN(name), MAX(name) FROM employees;
```

AVG, SUM, and COUNT ignore NULLs. Use `COALESCE(col, 0)` to treat NULL as zero.

### GROUP BY

Non-aggregated columns in SELECT must appear in GROUP BY.

```sql
SELECT department, COUNT(*) AS headcount FROM employees GROUP BY department;
SELECT department, AVG(salary) AS avg_salary FROM employees GROUP BY department;
SELECT department, role, COUNT(*) AS headcount FROM employees GROUP BY department, role;
SELECT department, role, AVG(salary) AS avg_salary FROM employees GROUP BY department, role ORDER BY department, avg_salary DESC;
```

Grouping by expression requires repeating it — alias references may not work in GROUP BY:

```sql
SELECT SUBSTR(joined_at, 1, 4) AS year, COUNT(*) AS hires FROM employees GROUP BY year;
```

### HAVING

HAVING filters groups after aggregation. WHERE filters rows before.

```sql
SELECT department, AVG(salary) AS avg_salary FROM employees GROUP BY department HAVING AVG(salary) > 100000;
SELECT role, COUNT(*) AS headcount FROM employees GROUP BY role HAVING COUNT(*) > 1;
```

WHERE + HAVING together:

```sql
SELECT department, AVG(salary) AS avg_salary
FROM employees
WHERE joined_at >= '2020-01-01'
GROUP BY department
HAVING AVG(salary) > 100000;
```

### Subqueries

#### Scalar subquery

Returns a single value for each row:

```sql
SELECT name, salary FROM employees WHERE salary > (SELECT AVG(salary) FROM employees);
```

```sql
SELECT name, salary,
       (SELECT AVG(salary) FROM employees) AS avg_salary,
       salary - (SELECT AVG(salary) FROM employees) AS diff
FROM employees;
```

#### Row subquery

```sql
SELECT name, salary, department
FROM employees
WHERE (salary, department) = (SELECT MAX(salary), department FROM employees);
```

Row constructors work in PostgreSQL and MySQL; SQLite has limited support.

#### IN with subquery

```sql
SELECT name, department
FROM employees
WHERE department IN (
    SELECT department FROM employees GROUP BY department HAVING COUNT(*) >= 3
);
```

#### EXISTS

Returns true when the subquery produces at least one row:

```sql
SELECT DISTINCT department
FROM employees e1
WHERE EXISTS (
    SELECT 1 FROM employees e2
    WHERE e2.department = e1.department AND e2.salary > 120000
);
```

```sql
SELECT name, department
FROM employees e1
WHERE EXISTS (
    SELECT 1 FROM employees e2
    WHERE e2.department = e1.department
      AND e2.joined_at >= '2023-01-01'
      AND e2.id <> e1.id
);
```

EXISTS handles NULLs correctly and often outperforms IN on large subquery results.

### Common Table Expressions (CTEs)

#### Simple CTE

```sql
WITH engineering AS (
    SELECT name, salary FROM employees WHERE department = 'Engineering'
)
SELECT name, salary FROM engineering WHERE salary > 120000;
```

#### Multiple CTEs

```sql
WITH dept_stats AS (
    SELECT department, AVG(salary) AS avg_sal FROM employees GROUP BY department
),
high_depts AS (
    SELECT department FROM dept_stats WHERE avg_sal > 100000
)
SELECT e.name, e.salary, e.department
FROM employees e
JOIN high_depts hd ON e.department = hd.department;
```

#### Recursive CTE

Three parts: anchor member, recursive member referencing the CTE, and UNION ALL.

```sql
WITH RECURSIVE numbers(n) AS (
    SELECT 1
    UNION ALL
    SELECT n + 1 FROM numbers WHERE n < 5
)
SELECT n FROM numbers;
```

Recursive CTEs are used for hierarchies, graphs, and sequences. Most databases impose a depth limit (SQLite: 1000, PostgreSQL: 100).

### Set operations

Combine results from two or more queries. Column counts and types must match.

#### UNION

Removes duplicates:

```sql
SELECT name AS identifier, 'employee' AS source FROM employees
UNION
SELECT department AS identifier, 'department' AS source FROM employees;
```

#### UNION ALL

Faster — does not deduplicate:

```sql
SELECT name, 'employee' FROM employees
UNION ALL
SELECT department, 'department' FROM employees;
```

#### INTERSECT

Rows present in both queries:

```sql
SELECT department FROM employees
INTERSECT
SELECT department FROM employees WHERE role = 'Backend';
```

#### EXCEPT

Rows in the first query but not the second:

```sql
SELECT department FROM employees
EXCEPT
SELECT department FROM employees WHERE role = 'Backend';
```

MySQL lacks INTERSECT and EXCEPT. Simulate with IN / NOT IN:

```sql
SELECT DISTINCT department FROM employees
WHERE department IN (SELECT department FROM employees WHERE role = 'Backend');
SELECT DISTINCT department FROM employees
WHERE department NOT IN (SELECT department FROM employees WHERE role = 'Backend');
```

### Performance awareness

#### Why SELECT * is discouraged

```sql
SELECT * FROM employees;  -- wasteful in production
```

Problems: returns unnecessary columns (I/O, memory, network), prevents index-only scans, creates implicit dependencies, and blocks covering index usage. Use explicit columns:

```sql
SELECT name, salary FROM employees;
```

#### Index awareness

Indexes speed up WHERE, JOIN, and ORDER BY:

```sql
SELECT name, salary FROM employees WHERE salary > 100000;  -- uses index on salary
```

Wrapping columns in functions blocks index usage:

```sql
-- Avoid: LOWER(name) prevents index scan
SELECT name FROM employees WHERE LOWER(name) = 'alice';
-- Prefer bare column
SELECT name FROM employees WHERE name = 'Alice';
```

#### EXPLAIN intro

Shows the query plan:

```sql
-- SQLite
EXPLAIN QUERY PLAN SELECT name, salary FROM employees WHERE salary > 100000;
-- PostgreSQL / MySQL
EXPLAIN SELECT name, salary FROM employees WHERE salary > 100000;
```

Key plan indicators: **Seq Scan / SCAN TABLE** (full table scan — consider an index), **Index Scan** (used index), **Sort** (needs sort), **rows** (estimate; mismatch suggests stale stats).

#### Logical execution order

1. FROM → 2. WHERE → 3. GROUP BY → 4. HAVING → 5. SELECT → 6. ORDER BY → 7. LIMIT/OFFSET

#### Common pitfalls

```sql
-- NOT IN with NULLs: returns zero rows if subquery contains NULL
-- Prefer NOT EXISTS
SELECT name FROM employees e1
WHERE NOT EXISTS (
    SELECT 1 FROM employees e2
    WHERE e2.department = e1.department AND e2.salary < 80000
);
```

```sql
-- Implicit type conversion: compare same types
SELECT name FROM employees WHERE department = 42;  -- bad
```

## Glossary

| Term | Definition |
|------|------------|
| SELECT | Clause specifying columns or expressions to return |
| WHERE | Clause filtering rows before aggregation via predicates |
| GROUP BY | Clause grouping rows by column values for aggregation |
| HAVING | Clause filtering groups after aggregation |
| ORDER BY | Clause sorting the result set |
| LIMIT | Clause restricting rows returned |
| Aggregate function | Function computing one value from many rows (COUNT, SUM, AVG, MIN, MAX) |
| Subquery | A SELECT nested inside another query clause |
| CTE | Common Table Expression — named subquery with WITH |
| UNION | Set operator combining queries and removing duplicates |
| JOIN | Operation combining rows from two tables on a condition |
| Scalar | A single value (one row, one column) |
| Predicate | Expression evaluating to TRUE/FALSE/NULL in WHERE/HAVING |
| Expression | Combination of columns, operators, functions, and literals |
| Alias | Temporary name for a column or table (AS keyword) |
| DISTINCT | Keyword removing duplicate rows |
| EXPLAIN | Command showing query execution plan |
| Index | Data structure accelerating row lookups |
| Keyset pagination | Pagination using WHERE filters instead of OFFSET |
| Recursive CTE | CTE referencing itself for hierarchies and sequences |
| Set operation | UNION, INTERSECT, or EXCEPT combining multiple query results |

## Quick References

- [SQLite SELECT](https://www.sqlite.org/lang_select.html) — complete SELECT syntax
- [PostgreSQL SELECT](https://www.postgresql.org/docs/current/sql-select.html) — full specification including WITH and set operations
- [MySQL SELECT](https://dev.mysql.com/doc/refman/8.0/en/select.html) — MySQL SELECT, LIMIT, and JOIN syntax
- [Use the Index, Luke](https://use-the-index-luke.com/) — guide to indexing and query performance
- [Modern SQL](https://modern-sql.com/) — standard SQL features including window functions and CTEs

## Next Steps

- [SQL Functions](sql-functions.md) — string, date/time, numeric functions, and conditional expressions
- [Joins & Relationships](../joins-and-relationships.md) — INNER, LEFT, RIGHT, and CROSS JOIN
