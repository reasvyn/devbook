# SQL JOINs and Table Relationships

## Description

Relational databases store data across multiple normalized tables to avoid redundancy and maintain integrity. JOINs are the mechanism to reassemble related data at query time. Mastering JOINs and understanding table relationships is essential for any developer working with SQL -- from backend APIs to data analysis to application development.

## Prerequisites

- [Querying Data](../querying-data.md) -- SELECT, FROM, WHERE, ORDER BY, GROUP BY, and basic filtering

## Table of Contents

- [Why JOIN](#why-join)
- [Sample Data](#sample-data)
- [INNER JOIN](#inner-join)
- [LEFT JOIN](#left-join)
- [RIGHT JOIN](#right-join)
- [FULL OUTER JOIN](#full-outer-join)
- [CROSS JOIN](#cross-join)
- [Self-Join](#self-join)
- [Multiple JOINs](#multiple-joins)
- [JOIN with Conditions: ON vs WHERE](#join-with-conditions-on-vs-where)
- [Table Aliases](#table-aliases)
- [Composite Keys](#composite-keys)
- [The USING Clause](#the-using-clause)

## Content / Material

### Why JOIN

Relational database design encourages **normalization**: splitting data into separate tables linked by keys. This eliminates duplication and preserves consistency. A single logical entity (e.g., an order) might span five tables (`orders`, `customers`, `order_items`, `products`, `inventories`). Without JOINs, you would need multiple queries and application-level assembly. JOINs let the database engine do that work in a single, declarative statement.

When you write a JOIN, you specify:

- Which tables to combine
- How rows from one table relate to rows in another (the **join condition**)
- Which columns to include in the result

The result is a virtual table produced by the database engine combining rows according to the join type.

### Sample Data

All examples in this document use the following tables unless stated otherwise. Execute this DDL and sample inserts in PostgreSQL to follow along.

```sql
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL,
    city TEXT
);

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(customer_id),
    order_date DATE NOT NULL DEFAULT CURRENT_DATE,
    total NUMERIC(10,2) NOT NULL
);

CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    price NUMERIC(10,2) NOT NULL,
    category TEXT
);

CREATE TABLE order_items (
    order_id INTEGER REFERENCES orders(order_id),
    product_id INTEGER REFERENCES products(product_id),
    quantity INTEGER NOT NULL,
    unit_price NUMERIC(10,2) NOT NULL,
    PRIMARY KEY (order_id, product_id)
);

INSERT INTO customers (name, email, city) VALUES
    ('Alice Wang',   'alice@example.com',  'New York'),
    ('Bob Chen',     'bob@example.com',    'London'),
    ('Carol Davis',  'carol@example.com',  'Tokyo'),
    ('Danilo Souza', 'danilo@example.com', 'Sao Paulo'),
    ('Eve Park',     'eve@example.com',    'Seoul');

INSERT INTO orders (customer_id, order_date, total) VALUES
    (1, '2025-01-10', 150.00),
    (1, '2025-02-15', 275.00),
    (2, '2025-01-20', 99.99),
    (3, '2025-03-01', 450.00),
    (3, '2025-03-05', 32.50),
    (5, '2025-04-12', 210.00);

INSERT INTO products (name, price, category) VALUES
    ('Wireless Mouse',     25.00,  'Electronics'),
    ('Mechanical Keyboard', 89.99,  'Electronics'),
    ('Notebook',           4.50,   'Stationery'),
    ('Desk Lamp',          45.00,  'Furniture'),
    ('USB-C Hub',          35.00,  'Electronics'),
    ('Monitor Stand',      60.00,  'Furniture');

INSERT INTO order_items (order_id, product_id, quantity, unit_price) VALUES
    (1, 1, 2, 25.00),
    (1, 3, 5, 4.50),
    (2, 2, 1, 89.99),
    (2, 5, 2, 35.00),
    (2, 6, 1, 60.00),
    (3, 1, 1, 25.00),
    (4, 2, 2, 89.99),
    (4, 4, 1, 45.00),
    (4, 5, 3, 35.00),
    (5, 3, 10, 4.50),
    (6, 1, 3, 25.00),
    (6, 2, 1, 89.99);
```

Every JOIN example below works against these tables. You can copy-and-paste each query into `psql` to see results.

### INNER JOIN

`INNER JOIN` returns rows only when the join condition finds a match in **both** tables. Non-matching rows in either table are excluded. This is the most common JOIN type.

**Venn diagram concept:** the intersection of two sets.

```sql
SELECT c.name, o.order_id, o.order_date, o.total
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id;
```

```
    name     | order_id | order_date | total
-------------+----------+------------+--------
 Alice Wang  |        1 | 2025-01-10 | 150.00
 Alice Wang  |        2 | 2025-02-15 | 275.00
 Bob Chen    |        3 | 2025-01-20 |  99.99
 Carol Davis |        4 | 2025-03-01 | 450.00
 Carol Davis |        5 | 2025-03-05 |  32.50
 Eve Park    |        6 | 2025-04-12 | 210.00
(6 rows)
```

Only customers who have placed at least one order appear. `Danilo Souza` (customer 4) has no orders and is excluded.

The `INNER` keyword is optional. `JOIN` alone defaults to `INNER JOIN`.

```sql
-- Equivalent shorthand
SELECT c.name, o.order_id
FROM customers c
JOIN orders o USING (customer_id);
```

**Join condition != filter condition.** The ON clause specifies how tables relate. Additional filtering belongs in WHERE:

```sql
SELECT c.name, o.total
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
WHERE o.total > 200;
```

```
    name     | total
-------------+--------
 Alice Wang  | 275.00
 Carol Davis | 450.00
 Eve Park    | 210.00
(3 rows)
```

### LEFT JOIN

`LEFT JOIN` returns **every row** from the left table and matching rows from the right table. When no match exists, the right-table columns are filled with `NULL`.

**Venn diagram concept:** all of set A plus the intersection with set B.

```sql
SELECT c.name, o.order_id, o.total
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id;
```

```
    name     | order_id |  total
-------------+----------+---------
 Alice Wang  |        1 |  150.00
 Alice Wang  |        2 |  275.00
 Bob Chen    |        3 |   99.99
 Carol Davis |        4 |  450.00
 Carol Davis |        5 |   32.50
 Danilo Souza |     NULL |     NULL
 Eve Park    |        6 |  210.00
(7 rows)
```

`Danilo Souza` now appears even though he has no orders. The `order_id` and `total` columns are `NULL`.

Use `LEFT JOIN` when you need to preserve all rows from the primary table, such as displaying all customers and optionally their orders.

**Finding non-matches** is a common pattern:

```sql
SELECT c.name
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

```
    name
-------------
 Danilo Souza
(1 row)
```

This finds customers who have never placed an order. The `WHERE o.order_id IS NULL` filter isolates rows where the JOIN produced no match. Using a primary-key column from the right table in the `IS NULL` check is safest because primary keys are never legitimately `NULL`.

### RIGHT JOIN

`RIGHT JOIN` is the mirror of `LEFT JOIN`: it returns all rows from the right table and matching rows from the left. Non-matching left-table columns become `NULL`.

```sql
SELECT c.name, o.order_id
FROM customers c
RIGHT JOIN orders o ON c.customer_id = o.customer_id;
```

Because every order in our sample data has a valid `customer_id`, this result looks identical to `INNER JOIN`. To see the difference, imagine an order with an invalid (non-existent) `customer_id`:

```sql
-- Hypothetical: order orphaned by deleted customer
INSERT INTO orders (customer_id, order_date, total) VALUES (99, '2025-05-01', 10.00);
```

```sql
SELECT c.name, o.order_id
FROM customers c
RIGHT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id = 7;
```

```
 name | order_id
------+----------
 NULL |        7
(1 row)
```

**Convention:** Most SQL developers use `LEFT JOIN` exclusively and never `RIGHT JOIN`. A `RIGHT JOIN` can always be rewritten as a `LEFT JOIN` by swapping the table order. Prefer `LEFT JOIN` for readability.

```sql
-- RIGHT JOIN equivalent using LEFT JOIN
SELECT c.name, o.order_id
FROM orders o
LEFT JOIN customers c ON c.customer_id = o.customer_id;
```

### FULL OUTER JOIN

`FULL OUTER JOIN` returns all rows from both tables. Where a match exists, the rows are combined. Where no match exists, missing-side columns are `NULL`.

**Venn diagram concept:** the union of both sets.

```sql
SELECT c.name, o.order_id, o.total
FROM customers c
FULL OUTER JOIN orders o ON c.customer_id = o.customer_id;
```

The result includes customers without orders (Danilo Souza with NULL order columns) and orders without customers (if any orphan orders exist). It is the only way to see both orphaned rows from either side in a single query.

`FULL OUTER JOIN` is less common in transactional applications but frequently appears in data migration validation, reconciliation reports, and ETL auditing where you want to compare two datasets side by side.

### CROSS JOIN

`CROSS JOIN` produces the **Cartesian product** of two tables: every row from the first table is paired with every row from the second. No `ON` clause is used because there is no join condition.

```sql
SELECT c.name, p.name AS product
FROM customers c
CROSS JOIN products p;
```

If there are 5 customers and 6 products, the result has 30 rows (`5 x 6`).

```
    name     |      product
-------------+-------------------
 Alice Wang  | Wireless Mouse
 Alice Wang  | Mechanical Keyboard
 Alice Wang  | Notebook
...
 Danilo Souza | Monitor Stand
 Eve Park    | Monitor Stand
(30 rows)
```

`CROSS JOIN` is rarely useful in production queries. Common use cases:

- Generating all possible combinations (e.g., dates x time slots for a calendar)
- Creating test data by cross-referencing dimension tables
- Creating number or sequence tables

```sql
-- Calendario: all days in January 2025 for every product
SELECT p.name, d.dt
FROM products p
CROSS JOIN generate_series('2025-01-01'::date, '2025-01-31'::date, '1 day') AS d(dt);
```

**Danger:** Omitting the `ON` clause by accident in an `INNER JOIN` silently becomes a `CROSS JOIN` and can produce millions of rows, crashing the client or the database server.

### Self-Join

A **self-join** joins a table to itself. You must use table aliases to disambiguate the two roles. Self-joins model hierarchical or network relationships stored in a single table.

**Classic example: employees and managers.**

```sql
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    manager_id INTEGER REFERENCES employees(employee_id)
);

INSERT INTO employees (name, manager_id) VALUES
    ('Sarah CEO',    NULL),
    ('Tom VP Eng',   1),
    ('Uma VP Ops',   1),
    ('Victor Eng Mgr', 2),
    ('Wendy Eng',    4),
    ('Xavier Eng',   4),
    ('Yuko Ops Mgr', 3),
    ('Zara Ops',     7);
```

Find each employee and their manager:

```sql
SELECT e.name AS employee,
       m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.employee_id
ORDER BY m.name NULLS FIRST;
```

```
  employee   |  manager
-------------+-----------
 Sarah CEO   | NULL
 Tom VP Eng  | Sarah CEO
 Uma VP Ops  | Sarah CEO
 Victor Eng Mgr | Tom VP Eng
 Yuko Ops Mgr | Uma VP Ops
 Wendy Eng   | Victor Eng Mgr
 Xavier Eng  | Victor Eng Mgr
 Zara Ops    | Yuko Ops Mgr
(8 rows)
```

Key points:

- The `employees` table appears twice: once as `e` (employee) and once as `m` (manager)
- The `LEFT JOIN` ensures the CEO (who has no manager) still appears
- Self-joins work with any JOIN type (INNER, LEFT, FULL)

Other self-join patterns:

- **Adjacency lists** (comments replying to comments)
- **Graph edges** (friend-of-friend in social networks)
- **Date ranges** (finding overlapping bookings)
- **Hierarchical categories** (parent-child categories)

```sql
-- Find employees who earn more than their manager
-- (requires a salary column, omited in our schema for brevity)
-- SELECT e.name, e.salary AS emp_salary, m.salary AS mgr_salary
-- FROM employees e
-- JOIN employees m ON e.manager_id = m.employee_id
-- WHERE e.salary > m.salary;
```

### Multiple JOINs

A single query can join three or more tables. Each JOIN is evaluated left-to-right conceptually (though the optimizer may reorder).

```sql
SELECT c.name AS customer,
       o.order_id,
       p.name AS product,
       oi.quantity,
       oi.unit_price
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id;
```

```
  customer  | order_id |      product      | quantity | unit_price
------------+----------+-------------------+----------+------------
 Alice Wang |        1 | Wireless Mouse    |        2 |      25.00
 Alice Wang |        1 | Notebook          |        5 |       4.50
 Alice Wang |        2 | Mechanical Keyboard |      1 |      89.99
 Alice Wang |        2 | USB-C Hub         |        2 |      35.00
 Alice Wang |        2 | Monitor Stand     |        1 |      60.00
 Bob Chen   |        3 | Wireless Mouse    |        1 |      25.00
 Carol Davis|        4 | Mechanical Keyboard |      2 |      89.99
 Carol Davis|        4 | Desk Lamp         |        1 |      45.00
 Carol Davis|        4 | USB-C Hub         |        3 |      35.00
 Carol Davis|        5 | Notebook          |       10 |       4.50
 Eve Park   |        6 | Wireless Mouse    |        3 |      25.00
 Eve Park   |        6 | Mechanical Keyboard |      1 |      89.99
(12 rows)
```

You can mix different JOIN types in one query:

```sql
SELECT c.name, o.order_id, p.name AS product
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
LEFT JOIN order_items oi ON o.order_id = oi.order_id
LEFT JOIN products p ON oi.product_id = p.product_id;
```

This preserves customers even if they have no orders (Danilo Souza), and orders even if they have no items.

**Performance note:** Each JOIN adds work. Joining 10 large tables without proper indexes can create multi-gigabyte intermediate result sets. Always check the execution plan with `EXPLAIN ANALYZE`.

### JOIN with Conditions: ON vs WHERE

The `ON` clause specifies the join condition. You can add extra conditions in `ON` with `AND`. The placement matters for `OUTER JOINs` but not for `INNER JOINs`.

**INNER JOIN -- ON and WHERE are equivalent:**

```sql
SELECT c.name, o.order_id, o.total
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
              AND o.total > 200;
```

This is equivalent to:

```sql
SELECT c.name, o.order_id, o.total
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
WHERE o.total > 200;
```

Both return the same result for `INNER JOIN`.

**LEFT JOIN -- ON vs WHERE matters:**

```sql
SELECT c.name, o.order_id, o.total
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
                   AND o.total > 200;
```

Result:

```
    name     | order_id |  total
-------------+----------+---------
 Alice Wang  |        2 |  275.00
 Alice Wang  |     NULL |    NULL
 Bob Chen    |     NULL |    NULL
 Carol Davis |        4 |  450.00
 Carol Davis |     NULL |    NULL
 Danilo Souza |     NULL |    NULL
 Eve Park    |        6 |  210.00
(7 rows)
```

Compare with filtering in `WHERE`:

```sql
SELECT c.name, o.order_id, o.total
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.total > 200;
```

Result:

```
    name     | order_id |  total
-------------+----------+---------
 Alice Wang  |        2 |  275.00
 Carol Davis |        4 |  450.00
 Eve Park    |        6 |  210.00
(3 rows)
```

**Key difference:**

- Condition in `ON` is applied **during** the JOIN. Non-matching rows still appear from the left table with `NULL`s.
- Condition in `WHERE` is applied **after** the JOIN. Rows that fail the filter are removed entirely, effectively converting the `LEFT JOIN` into an `INNER JOIN` for filtered rows.

Use `ON` for conditions that define the relationship. Use `WHERE` for post-JOIN filtering. When you want to keep all left-side rows, put right-table filters in `ON`.

### Table Aliases

Table aliases are shorthand names assigned to tables. They are essential when:

- The same table appears multiple times (self-joins, subqueries)
- Querying three or more tables to keep SQL readable
- Referencing columns from specific tables in complex expressions

```sql
-- Without aliases (verbose, hard to read)
SELECT customers.name, orders.order_id
FROM customers
INNER JOIN orders ON customers.customer_id = orders.customer_id;

-- With aliases (concise, clear)
SELECT c.name, o.order_id
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id;
```

Alias rules:

- Defined in `FROM` or `JOIN` clause after the table name
- The `AS` keyword is optional: `FROM customers c` equals `FROM customers AS c`
- Aliases must be unique within a query
- Once an alias is defined, the original table name cannot be used in that query

```sql
-- Error: ambiguous column reference
SELECT customer_id FROM customers c JOIN orders o USING (customer_id);
-- PostgreSQL error: column reference "customer_id" is ambiguous

-- Fix: qualify with alias
SELECT c.customer_id FROM customers c JOIN orders o USING (customer_id);
```

Derived tables and subqueries also require aliases:

```sql
SELECT avg_data.avg_total
FROM (
    SELECT customer_id, AVG(total) AS avg_total
    FROM orders
    GROUP BY customer_id
) avg_data
WHERE avg_data.avg_total > 100;
```

### Composite Keys

A **composite key** is a primary key or foreign key consisting of more than one column. Joining on composite keys requires multiple conditions in the `ON` clause.

In our sample data, `order_items` has a composite primary key: `(order_id, product_id)`.

```sql
SELECT o.order_id, p.name, oi.quantity
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id;
```

For tables with composite foreign keys, the join uses multiple `AND` conditions:

```sql
-- Hypothetical: order_detail with composite FK referencing order_items
-- CREATE TABLE order_detail (
--     order_id INTEGER,
--     product_id INTEGER,
--     detail TEXT,
--     FOREIGN KEY (order_id, product_id) REFERENCES order_items(order_id, product_id)
-- );

-- SELECT oi.order_id, oi.product_id, od.detail
-- FROM order_items oi
-- LEFT JOIN order_detail od
--     ON oi.order_id = od.order_id
--    AND oi.product_id = od.product_id;
```

Composite key joins are common in:

- Junction tables (many-to-many relationships)
- Log tables partitioned by date + tenant
- Temporal tables with `valid_from` + `valid_to` columns
- Multi-tenant systems where `tenant_id` is part of every key

### The USING Clause

When the join columns have the **same name** in both tables, `USING` simplifies the syntax:

```sql
SELECT c.name, o.order_id
FROM customers c
JOIN orders o USING (customer_id);
```

This is equivalent to:

```sql
SELECT c.name, o.order_id
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;
```

Advantages of `USING`:

- Less repetitive
- The join column appears once, not twice, in the result set
- Eliminates ambiguous-column errors for the join key

```sql
-- USING deduplicates the join column
SELECT customer_id, name, order_id
FROM customers
JOIN orders USING (customer_id);
```

Without `USING`, `customer_id` would be ambiguous.

Disadvantages:

- Only works with exact column-name matches
- Cannot prefix the join column with a table alias
- Less explicit; some teams prefer `ON` for clarity

For composite keys:

```sql
SELECT *
FROM order_items
JOIN order_detail USING (order_id, product_id);
```

## Glossary

| Term | Definition |
|------|------------|
| Cartesian product | Every combination of rows from two tables; result of CROSS JOIN or a missing join condition |
| Composite key | A primary or foreign key consisting of two or more columns |
| CROSS JOIN | A join producing the Cartesian product of two tables with no join condition |
| Derived table | A subquery in the FROM clause that acts as a virtual table |
| FULL OUTER JOIN | A join returning all rows from both tables, with NULLs where no match exists |
| INNER JOIN | A join returning only rows where the join condition finds a match in both tables |
| JOIN | A SQL operation combining rows from two or more tables based on a related column |
| JOIN condition | The expression in the ON clause that determines how rows are matched |
| LEFT JOIN | A join returning all rows from the left table and matching rows from the right, NULLs for non-matches |
| N+1 problem | A performance anti-pattern where one query is followed by N additional queries per result row |
| RIGHT JOIN | A join returning all rows from the right table; functionally equivalent to a swapped LEFT JOIN |
| SELF JOIN | A join of a table to itself, requiring table aliases |
| Table alias | A shorthand name assigned to a table in a query |
| USING | A shorthand clause for JOIN when the matching columns have identical names |

## Quick References

- [PostgreSQL Documentation: Table Expressions](https://www.postgresql.org/docs/current/queries-table-expressions.html) — Official PostgreSQL reference on JOIN syntax with examples
- [Use the Index, Luke: SQL JOINs](https://use-the-index-luke.com/sql/join) — Practical guide to writing fast JOIN queries with proper indexing
- [Modern SQL: JOIN Types](https://modern-sql.com/feature/join) — Visual explanation of all JOIN types with Venn diagrams and historical context
- [SQL Join Visualizer](https://sql-join.com/) — Interactive tool to experiment with JOIN types and see results immediately
- [Joe Celko's SQL for Smarties](https://www.elsevier.com/books/sql-for-smarties/celko/978-0-12-800761-7) — Advanced SQL techniques including set-based thinking and complex JOIN patterns

## Next Steps

- [Relationships & Normalization](../relationships-and-normalization.md) — foreign keys, normalization, denormalization, and schema design
- [Database Design](../database-design.md) — indexing strategies, schema design patterns, and advanced data modeling

