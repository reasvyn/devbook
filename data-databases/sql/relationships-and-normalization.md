# Relationships & Normalization

## Description

Table relationships, foreign keys, normalization, and when to denormalize — designing database schemas that are consistent, efficient, and maintainable.

## Prerequisites

- [Joins & Relationships](../joins-and-relationships.md) — INNER, LEFT, RIGHT, FULL, CROSS joins; self-joins; JOIN syntax

## Table of Contents

- [Natural Relationships](#natural-relationships)
- [Foreign Keys](#foreign-keys)
- [Normalization](#normalization)
- [Denormalization](#denormalization)
- [JOIN vs Subquery](#join-vs-subquery)
- [LATERAL JOIN (PostgreSQL)](#lateral-join-postgresql)

## Content / Material

### Natural Relationships

Relational databases model three fundamental relationship types.

#### One-to-One

Each row in Table A relates to exactly zero or one row in Table B.

```sql
CREATE TABLE user_profiles (
    user_id INTEGER PRIMARY KEY REFERENCES customers(customer_id),
    bio TEXT,
    avatar_url TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);
```

One-to-one relationships are used for:

- Splitting sensitive data (e.g., login credentials in a separate table with stricter access control)
- Wide tables with rarely-used columns (vertical partitioning for performance)
- Extending a third-party schema without modifying the original table

The foreign key is typically also the primary key of the dependent table.

#### One-to-Many

A single row in Table A relates to zero, one, or many rows in Table B. This is the most common relationship.

```sql
-- One customer has many orders
SELECT c.name, COUNT(o.order_id) AS order_count
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.name
ORDER BY order_count DESC;
```

```
    name     | order_count
-------------+-------------
 Alice Wang  |           2
 Carol Davis |           2
 Bob Chen    |           1
 Eve Park    |           1
 Danilo Souza |           0
(5 rows)
```

Key design: the foreign key lives in the "many" table (`orders.customer_id`).

#### Many-to-Many

Multiple rows in Table A relate to multiple rows in Table B. This requires a **junction table** (also called join table, associative table, or bridge table).

```sql
-- Example: tags for blog posts
CREATE TABLE posts (
    post_id SERIAL PRIMARY KEY,
    title TEXT NOT NULL
);

CREATE TABLE tags (
    tag_id SERIAL PRIMARY KEY,
    name TEXT UNIQUE NOT NULL
);

CREATE TABLE post_tags (
    post_id INTEGER REFERENCES posts(post_id),
    tag_id INTEGER REFERENCES tags(tag_id),
    PRIMARY KEY (post_id, tag_id)
);
```

A junction table contains foreign keys referencing both related tables. The composite primary key prevents duplicate associations.

Querying a many-to-many relationship:

```sql
SELECT p.title, t.name AS tag
FROM posts p
JOIN post_tags pt ON p.post_id = pt.post_id
JOIN tags t ON pt.tag_id = t.tag_id
ORDER BY p.title;
```

Many-to-many patterns in real systems:

- Students and courses (a student takes many courses, a course has many students)
- Products and categories (a product can belong to multiple categories)
- Users and roles (RBAC systems)
- Books and authors

### Foreign Keys

A **foreign key** enforces referential integrity: it ensures values in one column (or column set) match values in the primary key (or unique constraint) of another table.

```sql
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(customer_id),
    ...
);
```

The `REFERENCES` clause creates a foreign key constraint. Any `customer_id` inserted into `orders` must exist in `customers.customer_id`. Violations raise an error:

```sql
INSERT INTO orders (customer_id, total) VALUES (99, 100.00);
-- ERROR: insert or update on table "orders" violates foreign key constraint
-- DETAIL: Key (customer_id)=(99) is not present in table "customers".
```

#### Referential Actions

The `ON DELETE` and `ON UPDATE` clauses define behavior when the referenced row is deleted or updated:

| Action | Behavior |
|--------|----------|
| `RESTRICT` | Prevents deletion if references exist (default, same as NO ACTION in PostgreSQL) |
| `CASCADE` | Deletes referencing rows when the referenced row is deleted |
| `SET NULL` | Sets the foreign key columns to NULL |
| `SET DEFAULT` | Sets the foreign key columns to their default values |
| `NO ACTION` | Like RESTRICT but deferrable in some databases |

```sql
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(customer_id)
        ON DELETE CASCADE
        ON UPDATE CASCADE,
    ...
);
```

With `ON DELETE CASCADE`, deleting customer 1 also deletes all their orders and order_items (if cascading FKs are set throughout). `ON DELETE SET NULL` is appropriate when the relationship is optional and you want to preserve the child record after the parent is removed.

#### Self-Referencing Foreign Keys

Foreign keys can reference the same table:

```sql
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    manager_id INTEGER REFERENCES employees(employee_id)
);
```

This models hierarchies (org charts, category trees) as adjacency lists.

#### Indexing Foreign Keys

Foreign key columns should be indexed. Without an index, every `JOIN` and `ON DELETE CASCADE` operation triggers a sequential scan of the referencing table.

```sql
CREATE INDEX idx_orders_customer_id ON orders(customer_id);
CREATE INDEX idx_order_items_order_id ON order_items(order_id);
CREATE INDEX idx_order_items_product_id ON order_items(product_id);
```

PostgreSQL does **not** automatically create indexes on foreign key columns. You must create them manually. Other databases (e.g., MySQL with InnoDB) index foreign keys automatically.

### Normalization

Normalization is the process of organizing data to reduce redundancy and improve integrity. It applies a series of rules called **normal forms**.

#### First Normal Form (1NF)

Each column must contain **atomic** (indivisible) values. There must be no repeating groups or arrays.

```sql
-- Violates 1NF: multiple values in one column
-- CREATE TABLE bad_orders (
--     order_id SERIAL PRIMARY KEY,
--     products TEXT  -- "Mouse,Keyboard,Monitor"
-- );

-- 1NF compliant
CREATE TABLE order_items_1nf (
    order_id INTEGER NOT NULL,
    product_id INTEGER NOT NULL,
    quantity INTEGER NOT NULL,
    PRIMARY KEY (order_id, product_id)
);
```

#### Second Normal Form (2NF)

The table must be in 1NF **and** every non-key column must depend on the **entire** primary key (no partial dependency). This only applies to tables with composite primary keys.

```sql
-- Violates 2NF: product_name depends only on product_id, not on (order_id, product_id)
-- CREATE TABLE bad_order_items (
--     order_id INTEGER,
--     product_id INTEGER,
--     product_name TEXT,   -- partial dependency on product_id
--     quantity INTEGER,
--     PRIMARY KEY (order_id, product_id)
-- );

-- 2NF compliant: split into two tables
-- order_items: (order_id, product_id, quantity)
-- products: (product_id, product_name, price)
```

#### Third Normal Form (3NF)

The table must be in 2NF **and** every non-key column must depend **only** on the primary key (no transitive dependency).

```sql
-- Violates 3NF: category_name depends on category_id, not directly on product_id
-- CREATE TABLE bad_products (
--     product_id SERIAL PRIMARY KEY,
--     name TEXT,
--     price NUMERIC,
--     category_id INTEGER,
--     category_name TEXT  -- transitively dependent on category_id
-- );

-- 3NF compliant
-- products: (product_id, name, price, category_id)
-- categories: (category_id, category_name)
```

#### Boyce-Codd Normal Form (BCNF)

A stricter version of 3NF where every determinant must be a candidate key. Rarely violated in practice if 3NF is applied correctly.

**When to normalize:**

- OLTP (Online Transaction Processing) systems with frequent writes
- Data that must be consistent under concurrent modifications
- Systems where storage cost is not the primary concern

The sample `customers`, `orders`, `order_items`, `products` tables in this document are normalized to 3NF.

### Denormalization

Denormalization is the deliberate introduction of redundancy to improve read performance. It trades storage and write complexity for faster queries.

**When to denormalize:**

- **Read-heavy workloads.** Analytics dashboards, reporting, read APIs. Pre-joining data into a single wide table avoids JOIN overhead.
- **Avoiding expensive JOINs at scale.** When tables have billions of rows, even indexed JOINs become slow. A materialized view or denormalized table can serve queries in milliseconds.
- **Caching computed values.** Store pre-aggregated totals or counts to avoid recalculating them on every read.

```sql
-- Denormalized: store customer_name directly in orders
-- Avoids JOIN for common lookup
CREATE TABLE orders_denormalized (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER,
    customer_name TEXT,        -- denormalized from customers
    order_date DATE,
    total NUMERIC(10,2)
);
```

**Risks of denormalization:**

- **Update anomalies.** Changing a customer name requires updating every row in `orders_denormalized`. A missed update causes inconsistent data.
- **Increased storage.** Redundant columns consume disk and memory.
- **Write slowdown.** Every INSERT or UPDATE must maintain multiple copies.

**Strategies to manage denormalization:**

- **Materialized views** -- PostgreSQL refreshes them on demand or with a scheduler:

```sql
CREATE MATERIALIZED VIEW customer_order_summary AS
SELECT c.customer_id,
       c.name,
       COUNT(o.order_id) AS order_count,
       COALESCE(SUM(o.total), 0) AS lifetime_value
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.name;

-- Refresh
REFRESH MATERIALIZED VIEW customer_order_summary;
```

- **Triggers or application-level logic** to keep denormalized columns synchronized.
- **CQRS (Command Query Responsibility Segregation)** -- maintain normalized tables for writes and denormalized projections for reads.

**Rule of thumb:** Start normalized. Denormalize only when you measure a performance problem that indexing, query tuning, or caching cannot solve.

### JOIN vs Subquery

Any query that uses a JOIN can also be written with a subquery, and vice versa. The choice affects readability, maintainability, and sometimes performance.

**Subquery in WHERE (semi-join):**

```sql
-- Customers who have placed orders (with JOIN)
SELECT DISTINCT c.name
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;

-- Equivalent with subquery
SELECT c.name
FROM customers c
WHERE c.customer_id IN (SELECT customer_id FROM orders);
```

**Subquery in SELECT (scalar subquery):**

```sql
-- Total per customer with JOIN and GROUP BY
SELECT c.name, COALESCE(SUM(o.total), 0) AS total_spent
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.name;

-- Equivalent with scalar subquery
SELECT c.name,
       (SELECT COALESCE(SUM(o.total), 0)
        FROM orders o
        WHERE o.customer_id = c.customer_id) AS total_spent
FROM customers c;
```

**Subquery in FROM (derived table):**

```sql
-- Average order value by customer
SELECT c.name, avg_data.avg_order
FROM customers c
JOIN (
    SELECT customer_id, AVG(total) AS avg_order
    FROM orders
    GROUP BY customer_id
) avg_data ON c.customer_id = avg_data.customer_id;
```

**When to prefer JOINs:**

- You need columns from the joined table in the final result
- The query involves multiple related tables (JOIN chains are often clearer than nested subqueries)
- The database optimizer can reorder JOINs for better performance

**When to prefer subqueries:**

- You only need to check existence (`EXISTS` / `IN`)
- You need aggregated data scoped to the outer row (correlated subquery)
- The subquery returns a single value used in a comparison
- You want to avoid data multiplication from JOINs (e.g., a `DISTINCT` JOIN that could be an `EXISTS`)

**EXISTS vs IN:**

```sql
-- EXISTS (often faster, stops at first match)
SELECT c.*
FROM customers c
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id);

-- IN (may be slower, evaluates all rows in subquery)
SELECT c.*
FROM customers c
WHERE c.customer_id IN (SELECT customer_id FROM orders);
```

`EXISTS` is generally preferred because it short-circuits on the first match and handles `NULL`s correctly. `IN` can be surprising when the subquery contains `NULL` values.

### LATERAL JOIN (PostgreSQL)

`LATERAL` is a PostgreSQL extension that allows a subquery in the `FROM` or `JOIN` clause to reference columns from preceding tables. This enables row-by-row subquery execution where each row drives the subquery.

```sql
-- For each customer, find their most recent order
SELECT c.name,
       recent.order_id,
       recent.total,
       recent.order_date
FROM customers c
LEFT JOIN LATERAL (
    SELECT order_id, total, order_date
    FROM orders o
    WHERE o.customer_id = c.customer_id
    ORDER BY o.order_date DESC
    LIMIT 1
) recent ON true;
```

```
    name     | order_id |  total  | order_date
-------------+----------+---------+------------
 Alice Wang  |        2 |  275.00 | 2025-02-15
 Bob Chen    |        3 |   99.99 | 2025-01-20
 Carol Davis |        5 |   32.50 | 2025-03-05
 Danilo Souza |    NULL |    NULL | NULL
 Eve Park    |        6 |  210.00 | 2025-04-12
(5 rows)
```

Key characteristics:

- The `LATERAL` subquery runs once for each row in the left table
- `LIMIT 1` gives the top-N-per-group pattern
- The `ON true` makes it behave like a `CROSS JOIN LATERAL`; `LEFT JOIN LATERAL ... ON true` preserves rows without matches
- Without `LATERAL`, a `FROM` subquery cannot reference preceding `FROM` items

**Comparison with window functions:**

```sql
-- Same result using ROW_NUMBER() window function
SELECT name, order_id, total, order_date
FROM (
    SELECT c.name, o.*,
           ROW_NUMBER() OVER (
               PARTITION BY c.customer_id
               ORDER BY o.order_date DESC
           ) AS rn
    FROM customers c
    LEFT JOIN orders o ON c.customer_id = o.customer_id
) ranked
WHERE rn = 1 OR rn IS NULL;
```

`LATERAL` is often more readable for top-N-per-group queries, especially when the subquery uses its own `ORDER BY` and `LIMIT`.

**Common LATERAL use cases:**

- Top-N per group
- Nearest neighbor searches (geospatial with `ORDER BY distance LIMIT 1`)
- Calling set-returning functions for each row
- Complex filtering where the subquery must reference outer columns

```sql
-- Top 2 products by revenue for each category
SELECT cat.category, top.name, top.revenue
FROM (SELECT DISTINCT category FROM products) cat
CROSS JOIN LATERAL (
    SELECT p.name,
           SUM(oi.quantity * oi.unit_price) AS revenue
    FROM products p
    JOIN order_items oi ON p.product_id = oi.product_id
    WHERE p.category = cat.category
    GROUP BY p.product_id, p.name
    ORDER BY revenue DESC
    LIMIT 2
) top;
```

## Glossary

| Term | Definition |
|------|------------|
| 1NF (First Normal Form) | Every column contains atomic values; no repeating groups |
| 2NF (Second Normal Form) | 1NF plus every non-key column depends on the entire primary key |
| 3NF (Third Normal Form) | 2NF plus no transitive dependencies on non-key columns |
| BCNF (Boyce-Codd Normal Form) | 3NF plus every determinant is a candidate key |
| Candidate key | A minimal set of columns that uniquely identifies each row |
| Cascade | A referential action that propagates deletes or updates to child rows |
| Composite key | A primary or foreign key consisting of two or more columns |
| Denormalization | Intentional redundancy to improve read performance at the cost of writes and storage |
| Determinant | Any column or set of columns on which another column is functionally dependent |
| Foreign key | A column referencing the primary key of another table, enforcing referential integrity |
| Functional dependency | A relationship where one column's value determines another's value |
| Index | A database structure that speeds up row retrieval at the cost of slower writes |
| Junction table | A table used to implement a many-to-many relationship |
| Many-to-many | A relationship where each row in Table A can relate to many rows in Table B and vice versa |
| Materialized view | A pre-computed query result stored as a table, refreshed on demand or by schedule |
| Normalization | Organizing data to reduce redundancy and dependency by applying normal forms |
| One-to-many | A relationship where each row in Table A relates to many rows in Table B, but each B relates to exactly one A |
| One-to-one | A relationship where each row in Table A relates to at most one row in Table B and vice versa |
| Partial dependency | A non-key column depending on only part of a composite primary key (violates 2NF) |
| Primary key | A column or set of columns that uniquely identifies each row in a table |
| Referential action | Behavior when a referenced row is deleted or updated (RESTRICT, CASCADE, SET NULL, SET DEFAULT) |
| Referential integrity | The guarantee that foreign key values correspond to valid primary key values |
| Self-referencing FK | A foreign key that references the primary key of the same table |
| Transitive dependency | A non-key column depending on another non-key column (violates 3NF) |
| Update anomaly | An inconsistency caused by updating one copy of redundant data but not others |

## Quick References

- [Database Normalization Explained (GeeksforGeeks)](https://www.geeksforgeeks.org/normalization-in-dbms/) — Comprehensive guide to 1NF through 5NF with examples
- [PostgreSQL Documentation: Foreign Keys](https://www.postgresql.org/docs/current/ddl-constraints.html#DDL-CONSTRAINTS-FK) — Official PostgreSQL reference on foreign key constraints and referential actions
- [Use the Index, Luke: Database Design](https://use-the-index-luke.com/sql/join) — Practical schema design guidance with index considerations
- [LATERAL Join in PostgreSQL (PgDash)](https://www.pgmustard.com/blog/lateral-join-postgresql) — Deep dive into LATERAL with performance benchmarks and real-world examples
- [The CQRS Pattern (Martin Fowler)](https://martinfowler.com/bliki/CQRS.html) — Overview of Command Query Responsibility Segregation as a denormalization strategy

## Next Steps

- [Database Design](../database-design.md) — indexing strategies, schema design patterns, and advanced data modeling
- [Joins & Relationships](../joins-and-relationships.md) — SQL JOIN syntax, self-joins, and composite key joins
