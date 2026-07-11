# What Is SQL?

## Description

SQL (Structured Query Language) is the standard language for managing relational databases. Every developer encounters data persistence, and SQL remains the most widely used interface for storing, querying, and manipulating structured data — from embedded SQLite databases in mobile apps to petabyte-scale PostgreSQL and MySQL clusters.

## Prerequisites

- [Programming Fundamentals](../../programming/fundamentals/index.md) — variables, functions, conditionals, basic data structures

## Table of Contents

- [What Is a Database?](#what-is-a-database)
- [The Relational Model](#the-relational-model)
- [What Is SQL?](#what-is-sql)
- [SQL Sublanguages](#sql-sublanguages)
- [Data Types in SQL](#data-types-in-sql)
- [NULL and Three-Valued Logic](#null-and-three-valued-logic)
- [Constraints](#constraints)
- [Creating Tables](#creating-tables)
- [Inserting Data](#inserting-data)
- [Basic SELECT Queries](#basic-select-queries)
- [Updating Data](#updating-data)
- [Deleting Data](#deleting-data)
- [Filtering with WHERE](#filtering-with-where)
- [Sorting](#sorting)
- [Limiting and Pagination](#limiting-and-pagination)
- [Aliases](#aliases)
- [Comments in SQL](#comments-in-sql)
- [Tools](#tools)
- [Connecting from Code](#connecting-from-code)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is a Database?

A database is an organized collection of data stored electronically. Unlike plain files (CSV, JSON, text), a database provides:

- **Persistence** — data survives program restarts and power loss
- **Structured access** — query data without loading everything into memory
- **Concurrency** — multiple readers and writers safely
- **Consistency guarantees** — data remains valid under defined rules
- **Indexing** — fast lookups without scanning every record

Modern databases are classified by their data model:

| Family | Examples | Data model |
|--------|----------|------------|
| Relational | PostgreSQL, MySQL, SQLite, MariaDB, Oracle, SQL Server | Tables, rows, columns, SQL |
| Document | MongoDB, CouchDB, Firestore | JSON-like documents |
| Key-Value | Redis, DynamoDB, LevelDB | Key-value pairs |
| Graph | Neo4j, Dgraph | Nodes and edges |

**ACID** describes guarantees for database transactions:

- **Atomicity** — a transaction either completes fully or has no effect (all-or-nothing)
- **Consistency** — a transaction brings the database from one valid state to another; constraints are preserved
- **Isolation** — concurrent transactions execute as if sequential; intermediate states are invisible to other transactions
- **Durability** — once a transaction commits, its changes persist even after a system crash

Most relational databases offer ACID guarantees. Some NoSQL systems relax these (CAP theorem trade-off) for scalability.

### The Relational Model

Proposed by E. F. Codd in 1970, the relational model organizes data into **relations** (tables). Each relation is a set of **tuples** (rows) sharing the same **attributes** (columns).

A table has:

- A fixed **schema** — the set of columns and their data types
- **Rows** (records, tuples) — individual observations or entities
- **Columns** (fields, attributes) — named properties with a defined type

```
Table: users
+----+-----------+---------------------+
| id | name      | email               |
+----+-----------+---------------------+
| 1  | Alice     | alice@example.com   |
| 2  | Bob       | bob@example.com     |
| 3  | Charlie   | charlie@example.com |
+----+-----------+---------------------+
```

Rows are unordered. If you need a specific order, you ask for it in the query (ORDER BY). Each row should be uniquely identifiable — typically through a **primary key**.

### What Is SQL?

SQL (Structured Query Language, pronounced "ess-que-el" or "sequel") is the standard language for interacting with relational databases. Key characteristics:

- **Declarative** — you say *what* you want, not *how* to get it. The database's query planner decides the execution strategy.
- **Set-based** — operations work on sets of rows, not individual records
- **Standardized** — ANSI SQL (1986) and ISO SQL (1987) with revisions: SQL:1999, SQL:2003, SQL:2008, SQL:2011, SQL:2016, SQL:2023
- **40+ years** of history — originated at IBM in the early 1970s (System R)

Each database vendor implements a dialect of the standard with extensions:

| Database | Dialect notes |
|----------|---------------|
| SQLite | Minimal, dynamically typed, no native BOOLEAN or DATETIME |
| PostgreSQL | Full-featured, strict typing, excellent SQL standard compliance |
| MySQL / MariaDB | Lenient defaults (e.g., silent truncation, implicit type coercion) |
| Microsoft SQL Server | T-SQL dialect with proprietary extensions |
| Oracle | PL/SQL dialect with procedural extensions |

Despite differences, the core SQL syntax (SELECT, INSERT, UPDATE, DELETE, CREATE TABLE) is portable across all major engines.

### SQL Sublanguages

SQL is divided into four sublanguages by function:

**DDL — Data Definition Language**

Used to define and modify database structure (schemas, tables, indexes, views).

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL
);

ALTER TABLE users ADD COLUMN email TEXT;

DROP TABLE users;
```

**DML — Data Manipulation Language**

Used to read and write data.

```sql
SELECT * FROM users;
INSERT INTO users (id, name) VALUES (1, 'Alice');
UPDATE users SET name = 'Alicia' WHERE id = 1;
DELETE FROM users WHERE id = 1;
```

**DCL — Data Control Language**

Used to manage permissions and access.

```sql
GRANT SELECT, INSERT ON users TO app_user;
REVOKE DELETE ON users FROM app_user;
```

**TCL — Transaction Control Language**

Used to manage transactions.

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
-- or ROLLBACK if something goes wrong
```

### Data Types in SQL

Every column in a table has a data type that defines what values it can hold.

| Type | Description | Example values |
|------|-------------|----------------|
| INTEGER | Whole number (4 or 8 bytes) | 42, -7, 0 |
| VARCHAR(n) | Variable-length string with max n chars | 'hello', 'PostgreSQL' |
| TEXT | Unlimited-length string | 'A very long article...' |
| BOOLEAN | True or false | TRUE, FALSE |
| DATE | Calendar date | '2026-06-26' |
| TIMESTAMP | Date + time | '2026-06-26 14:30:00' |
| TIMESTAMPTZ | Timestamp with time zone | '2026-06-26 14:30:00+00' |
| DECIMAL(p,s) | Exact numeric with precision p and scale s | DECIMAL(10,2) for 1234567.89 |
| FLOAT / REAL | Approximate floating-point | 3.14159 |
| BLOB | Binary large object | Images, files, raw bytes |
| UUID | Universally unique identifier | '550e8400-e29b-41d4-a716-446655440000' |
| JSON / JSONB | JSON data (native in PostgreSQL) | '{"key": "value"}' |

Choosing the right type matters for correctness and performance. Storing prices as FLOAT causes rounding errors; use DECIMAL instead. Storing phone numbers as INTEGER drops leading zeros; use VARCHAR.

### NULL and Three-Valued Logic

NULL in SQL means "unknown" or "missing value." It is not zero, not an empty string, and not false.

Because NULL represents an unknown, comparisons involving NULL use **three-valued logic**:

| Expression | Result |
|------------|--------|
| NULL = NULL | UNKNOWN (not TRUE) |
| NULL = 1 | UNKNOWN |
| NULL <> 1 | UNKNOWN |
| TRUE AND NULL | UNKNOWN |
| FALSE AND NULL | FALSE |
| TRUE OR NULL | TRUE |
| NOT NULL | UNKNOWN |

In WHERE clauses, only rows where the condition evaluates to TRUE are included. UNKNOWN behaves like FALSE for filtering.

```sql
SELECT * FROM users WHERE email = NULL;
-- Returns zero rows. NULL = NULL is UNKNOWN, not TRUE.

SELECT * FROM users WHERE email IS NULL;
-- Correct way: returns rows where email is NULL.
```

Use `IS NULL` and `IS NOT NULL` to test for NULL. Never use `= NULL`.

```sql
-- COALESCE returns the first non-NULL argument
SELECT COALESCE(middle_name, '') FROM users;

-- NULLIF returns NULL if both arguments are equal
SELECT NULLIF(a, b);

-- IFNULL (vendor-specific: SQLite, MySQL) or COALESCE (PostgreSQL)
```

### Constraints

Constraints enforce rules on the data in a table.

| Constraint | Purpose |
|------------|---------|
| PRIMARY KEY | Uniquely identifies each row; implies NOT NULL and UNIQUE |
| FOREIGN KEY | Ensures a column value exists in another table (referential integrity) |
| UNIQUE | All values in the column (or column group) must be distinct |
| NOT NULL | Column cannot contain NULL |
| CHECK | Column values must satisfy a boolean expression |
| DEFAULT | Provides a default value when none is specified |

```sql
CREATE TABLE departments (
    id INTEGER PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE employees (
    id INTEGER PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    salary DECIMAL(10,2) CHECK (salary >= 0),
    department_id INTEGER NOT NULL DEFAULT 1,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);
```

A **primary key** can be a single column or composite (multiple columns). It is always indexed automatically.

A **foreign key** creates a parent-child relationship. Every `department_id` in `employees` must exist in `departments.id`. Trying to insert `department_id = 999` would be rejected.

### Creating Tables

The `CREATE TABLE` statement defines a new table and its columns, types, and constraints.

```sql
CREATE TABLE books (
    id INTEGER PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    author_id INTEGER NOT NULL,
    isbn VARCHAR(13) UNIQUE,
    published_date DATE,
    price DECIMAL(10,2) CHECK (price >= 0),
    stock INTEGER DEFAULT 0,
    FOREIGN KEY (author_id) REFERENCES authors(id)
);
```

To avoid recreating a table that already exists, use `IF NOT EXISTS`:

```sql
CREATE TABLE IF NOT EXISTS books (
    id INTEGER PRIMARY KEY,
    title VARCHAR(255) NOT NULL
);
```

Create a table from query results:

```sql
CREATE TABLE books_backup AS SELECT * FROM books;
```

Temporary tables exist only for the duration of a session:

```sql
CREATE TEMP TABLE recent_books AS
SELECT * FROM books WHERE published_date >= '2025-01-01';
```

### Inserting Data

The `INSERT INTO` statement adds new rows to a table.

```sql
-- Explicit column list (safer than positional)
INSERT INTO authors (id, name) VALUES (1, 'J.K. Rowling');

-- Insert multiple rows
INSERT INTO books (id, title, author_id, isbn, published_date, price)
VALUES
    (1, 'Harry Potter and the Philosopher''s Stone', 1, '9780747532699', '1997-06-26', 25.00),
    (2, 'Harry Potter and the Chamber of Secrets', 1, '9780747538493', '1998-07-02', 25.00);

-- Insert from SELECT (copy data between tables)
INSERT INTO archive_books (id, title, author_id, isbn)
SELECT id, title, author_id, isbn
FROM books
WHERE published_date < '2000-01-01';
```

### Basic SELECT Queries

SELECT retrieves data from tables. It is the most frequently used SQL statement.

```sql
-- All columns, all rows
SELECT * FROM books;

-- Specific columns
SELECT title, price FROM books;

-- Expressions and computed columns
SELECT title, price, price * 1.1 AS price_with_tax FROM books;

-- Filtering rows
SELECT title, price FROM books WHERE author_id = 1;

-- Sorting
SELECT title, price FROM books WHERE price > 20 ORDER BY price DESC;

-- Limiting results
SELECT title, price FROM books ORDER BY price DESC LIMIT 5;
```

Execution order: FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT/OFFSET.

### Updating Data

The `UPDATE` statement modifies existing rows.

```sql
-- Update a single column
UPDATE books SET price = 29.99 WHERE id = 1;

-- Update multiple columns
UPDATE books SET price = 35.00, stock = 100 WHERE id = 1;

-- Update using a subquery
UPDATE books
SET price = (SELECT AVG(price) FROM books)
WHERE price IS NULL;
```

**Always use a WHERE clause.** An UPDATE without WHERE affects every row. Use transactions to protect against mistakes:

```sql
BEGIN;
UPDATE books SET price = 0 WHERE id = 1;
SELECT * FROM books WHERE id = 1; -- verify
COMMIT; -- or ROLLBACK if wrong
```

### Deleting Data

Three ways to remove data, each with different semantics:

**DELETE** — removes rows one at a time, fires triggers, logs to WAL, can be rolled back.

```sql
DELETE FROM books WHERE id = 1;
DELETE FROM books WHERE published_date < '2000-01-01';
DELETE FROM books; -- removes all rows, keeps table structure
```

**TRUNCATE** — removes all rows in one fast operation. Does not fire triggers. Resets auto-increment counters. Cannot be rolled back in some databases.

```sql
TRUNCATE TABLE books;
```

**DROP** — removes the entire table (structure and data). Irreversible (except from backup).

```sql
DROP TABLE books;
```

| Operation | Removes data | Removes structure | Can rollback | Fires triggers | Speed |
|-----------|-------------|-------------------|-------------|----------------|-------|
| DELETE | Yes | No | Yes | Yes | Slow |
| TRUNCATE | Yes | No | Depends | No | Fast |
| DROP | Yes | Yes | Depends | No | Fastest |

### Filtering with WHERE

The WHERE clause filters rows before they are returned or modified.

```sql
-- Comparison operators
SELECT * FROM books WHERE price >= 20;
SELECT * FROM books WHERE author_id <> 1;
SELECT * FROM books WHERE stock = 0;

-- AND, OR, NOT
SELECT * FROM books WHERE price > 15 AND stock > 0;
SELECT * FROM books WHERE author_id = 1 OR author_id = 2;
SELECT * FROM books WHERE NOT stock = 0;

-- IN — match any value in a list
SELECT * FROM books WHERE author_id IN (1, 2, 3);

-- BETWEEN — inclusive range
SELECT * FROM books WHERE published_date BETWEEN '1997-01-01' AND '2000-12-31';

-- LIKE — pattern matching
-- % matches any sequence, _ matches exactly one character
SELECT * FROM authors WHERE name LIKE 'J%';       -- Starts with J
SELECT * FROM authors WHERE name LIKE '%Rowling'; -- Ends with Rowling
SELECT * FROM books WHERE title LIKE '%Harry%';   -- Contains Harry

-- IS NULL / IS NOT NULL
SELECT * FROM books WHERE isbn IS NULL;
SELECT * FROM books WHERE price IS NOT NULL;

-- Combining filters with parentheses
SELECT * FROM books
WHERE (author_id = 1 OR author_id = 2)
  AND price > 20
  AND stock > 0;
```

Operator precedence: NOT > AND > OR. Use parentheses to make intent explicit.

### Sorting

The ORDER BY clause sorts the result set.

```sql
-- Ascending (default)
SELECT title, price FROM books ORDER BY price;

-- Descending
SELECT title, price FROM books ORDER BY price DESC;

-- Multiple sort columns
SELECT title, author_id, price FROM books
ORDER BY author_id ASC, price DESC;

-- Sort by column position (avoid in production)
SELECT title, price FROM books ORDER BY 2 DESC;

-- Sort by expression
SELECT title, price FROM books ORDER BY price * 1.1 DESC;

-- NULLS FIRST / NULLS LAST (PostgreSQL, Oracle)
SELECT title, price FROM books ORDER BY price NULLS LAST;
```

In SQL, NULL is considered larger than all non-NULL values for sorting. `NULLS FIRST` or `NULLS LAST` overrides this.

### Limiting and Pagination

Two syntaxes exist:

**LIMIT / OFFSET** (MySQL, PostgreSQL, SQLite)

```sql
-- First 10 rows
SELECT * FROM books ORDER BY id LIMIT 10;

-- Rows 11-20 (page 2)
SELECT * FROM books ORDER BY id LIMIT 10 OFFSET 10;

-- Offset without limit
SELECT * FROM books ORDER BY id OFFSET 10;
```

**FETCH / OFFSET** (SQL standard, PostgreSQL, SQL Server, Oracle)

```sql
SELECT * FROM books
ORDER BY id
OFFSET 10 ROWS
FETCH NEXT 10 ROWS ONLY;
```

**Important**: Always use ORDER BY with LIMIT/OFFSET. Without it, the database may return an unpredictable subset because rows have no inherent order.

### Aliases

An alias temporarily renames a column or table for the duration of a query.

```sql
-- Column alias
SELECT title AS book_title, price * 1.1 AS price_with_tax FROM books;

-- The AS keyword is optional
SELECT title book_title FROM books;

-- Table alias (essential for joins)
SELECT b.title, a.name
FROM books AS b
JOIN authors AS a ON b.author_id = a.id;

-- Table alias without AS
SELECT b.title FROM books b;
```

### Comments in SQL

```sql
-- Single-line comment

/*
 * Multi-line comment.
 */

SELECT * FROM books; -- inline comment
```

Comments are ignored by the database. Use them to document complex logic.

### Tools

Common tools for running SQL interactively:

| Tool | Type | Best for |
|------|------|----------|
| sqlite3 CLI | Command-line | Learning, prototyping, embedded databases |
| psql | Command-line | PostgreSQL |
| mysql / mariadb | Command-line | MySQL, MariaDB |
| DBeaver | GUI (cross-platform) | Universal — supports 80+ database engines |
| pgAdmin | GUI | PostgreSQL |
| MySQL Workbench | GUI | MySQL |
| Beekeeper Studio | GUI (cross-platform) | SQLite, PostgreSQL, MySQL |
| DataGrip | GUI (JetBrains) | Professional, IDE integration |

To start the sqlite3 CLI:

```bash
sqlite3 devbook.db
```

```sql
sqlite> .tables
sqlite> SELECT * FROM books;
sqlite> .exit
```

### Connecting from Code

Applications use a database driver to run SQL from their programming language. Always use **parameterized queries** (`?`, `%s`, `$1`) to prevent SQL injection.

**Python with sqlite3** (built into standard library):

```python
import sqlite3

conn = sqlite3.connect("devbook.db")
cur = conn.cursor()

cur.execute("CREATE TABLE IF NOT EXISTS books (id INTEGER PRIMARY KEY, title TEXT NOT NULL, price REAL NOT NULL)")

cur.execute("INSERT INTO books (title, price) VALUES (?, ?)", ("Clean Code", 45.00))
conn.commit()

cur.execute("SELECT id, title, price FROM books WHERE price > ?", (30.00,))
for row in cur.fetchall():
    print(row)

conn.close()
```

**Python with psycopg2** (PostgreSQL):

```python
import psycopg2

conn = psycopg2.connect(host="localhost", dbname="devbook", user="app_user", password="secret")
cur = conn.cursor()

cur.execute("CREATE TABLE IF NOT EXISTS books (id SERIAL PRIMARY KEY, title VARCHAR(255) NOT NULL, price DECIMAL(10,2) NOT NULL)")

cur.execute("INSERT INTO books (title, price) VALUES (%s, %s)", ("Clean Code", 45.00))
conn.commit()

cur.execute("SELECT id, title, price FROM books WHERE price > %s", (30.00,))
for row in cur.fetchall():
    print(row)

cur.close()
conn.close()
```

**JavaScript with better-sqlite3** (SQLite):

```javascript
import Database from 'better-sqlite3';
const db = new Database('devbook.db');

db.exec("CREATE TABLE IF NOT EXISTS books (id INTEGER PRIMARY KEY, title TEXT NOT NULL, price REAL NOT NULL)");

const insert = db.prepare('INSERT INTO books (title, price) VALUES (?, ?)');
insert.run('Clean Code', 45.00);

const rows = db.prepare('SELECT id, title, price FROM books WHERE price > ?').all(30.00);
console.log(rows);
db.close();
```

## Glossary

| Term | Definition |
|------|------------|
| SQL | Structured Query Language — standard language for relational databases |
| Database | Organized collection of structured data stored electronically |
| Table | A collection of related data organized in rows and columns (a relation) |
| Row | A single record in a table (a tuple) |
| Column | A named attribute of a table with a defined data type (a field) |
| Schema | The structure defining a table's columns, types, and constraints |
| Constraint | A rule enforced on data (PRIMARY KEY, FOREIGN KEY, etc.) |
| Primary Key | Column(s) uniquely identifying each row in a table |
| Foreign Key | Column referencing a primary key in another table |
| Index | Data structure speeding up data retrieval on specific columns |
| NULL | Marker indicating missing or unknown data |
| DDL | Data Definition Language — CREATE, ALTER, DROP |
| DML | Data Manipulation Language — SELECT, INSERT, UPDATE, DELETE |
| DCL | Data Control Language — GRANT, REVOKE |
| TCL | Transaction Control Language — BEGIN, COMMIT, ROLLBACK |
| ACID | Atomicity, Consistency, Isolation, Durability |
| Relational | Data model organizing data into relations (tables) |
| SELECT | Statement for retrieving data from tables |
| INSERT | Statement for adding new rows to a table |
| UPDATE | Statement for modifying existing rows |
| DELETE | Statement for removing rows from a table |
| WHERE | Clause for filtering rows based on conditions |
| ORDER BY | Clause for sorting query results |
| LIMIT | Clause for restricting the number of returned rows |
| Alias | Temporary name assigned to a column or table for a query |
| Parameterized Query | Query using placeholders for values, preventing SQL injection |
| Three-Valued Logic | SQL logic yielding TRUE, FALSE, or UNKNOWN due to NULL |

## Quick References

- [SQLite Documentation](https://www.sqlite.org/docs.html) — Official SQLite documentation with syntax reference and SQL overview
- [PostgreSQL Documentation](https://www.postgresql.org/docs/) — Comprehensive PostgreSQL manual with tutorials and SQL language reference
- [MySQL Reference Manual](https://dev.mysql.com/doc/refman/en/) — Official MySQL documentation covering SQL syntax, data types, and functions
- [Learn SQL (Khan Academy)](https://www.khanacademy.org/computing/computer-programming/sql) — Interactive SQL tutorials with hands-on exercises
- [SQL Tutorial (Mode Analytics)](https://mode.com/sql-tutorial/) — Practical SQL tutorial from basic to advanced topics

## Next Steps

- [Querying Data](../querying-data.md) — advanced SELECT: joins, subqueries, aggregate functions, GROUP BY, HAVING
- [Data Modeling](../data-modeling.md) — designing schemas, normalization, entity-relationship diagrams
- [Indexes and Performance](../indexes-performance.md) — how indexes work, query planning, EXPLAIN, optimization
