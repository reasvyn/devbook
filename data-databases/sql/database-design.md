# Database Design

## Description

How to translate application requirements into a well-structured relational schema. Covers the full design lifecycle, normalization theory, indexing strategies, and practical schema evolution patterns that matter in production.

## Prerequisites

- [Joins & Relationships](../joins-and-relationships.md) -- inner, outer, cross joins and how tables relate via foreign keys

## Table of Contents

- [The Design Process](#the-design-process)
- [Entity-Relationship Modeling](#entity-relationship-modeling)
- [Entity Types](#entity-types)
- [Cardinality](#cardinality)
- [Keys](#keys)
- [Normalization](#normalization)
- [Practical Design Walkthrough: E-Commerce Database](#practical-design-walkthrough-e-commerce-database)
- [Indexes](#indexes)
- [Migrations](#migrations)
- [Seed Data](#seed-data)

## Content / Material

### The Design Process

Four stages translate a business problem into a working schema.

**Requirements analysis** gathers what the system must store and retrieve. List every data point and every question the database must answer.

**Conceptual model** produces a high-level description independent of any database system. Entities (nouns) and relationships (verbs) are identified without worrying about tables or columns. Output: an entity-relationship diagram (ERD).

**Logical model** maps the conceptual model to relational structures: tables, columns, keys, constraints. Normalization is applied. Every relationship becomes a foreign key or a junction table. Data types are chosen.

**Physical model** adds implementation details: index types, partitioning, storage engines, configuration tuning. This stage is revisited as the system scales.

These stages are not strictly sequential. A discovery during physical modeling often forces a change in the logical model. The point is to think about each layer deliberately rather than jumping straight to CREATE TABLE.

### Entity-Relationship Modeling

An **entity** is a thing in the real world that can be distinctly identified -- a user, a product, an order. An **attribute** is a property of an entity -- name, price, order date. A **relationship** is an association between entities -- a user *places* an order, a product *belongs to* a category.

In ER notation, rectangles represent entities, ellipses represent attributes, diamonds represent relationships, and lines connect entities to relationships with cardinality markers.

```text
[Book] --<Borrows>-- [Member]     [Book] --<WrittenBy>-- [Author]
  |         |           |           |                        |
  title     isbn       name        email                    name
  publish_year                    birth_year
```

Each relationship has a **cardinality** constraint and can be **optional** or **mandatory**.

### Entity Types

**Strong entities** exist independently. They have their own primary key and do not depend on another entity for identification. A `User` table is a strong entity -- users exist whether or not they have placed orders.

**Weak entities** depend on a strong entity for identification. They borrow part or all of their primary key from a parent entity. An `OrderItem` is a weak entity -- an order line item has no meaning without its parent `Order`. Its primary key is typically `(order_id, line_number)` where `order_id` references the `Order` table.

```sql
CREATE TABLE orders (
    id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date TIMESTAMP NOT NULL
);

CREATE TABLE order_items (
    order_id INT NOT NULL,
    line_number INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    PRIMARY KEY (order_id, line_number),
    FOREIGN KEY (order_id) REFERENCES orders(id)
);
```

`order_items` is a weak entity: its primary key includes `order_id` from the parent. The relationship is identifying.

### Cardinality

Three basic cardinalities describe how many instances of one entity relate to another.

**One-to-one (1:1):** A user has one profile; a profile belongs to one user. Enforce with a UNIQUE constraint on the foreign key.

```sql
CREATE TABLE users (id INT PRIMARY KEY, email VARCHAR(255) NOT NULL);
CREATE TABLE profiles (
    id INT PRIMARY KEY,
    user_id INT NOT NULL UNIQUE,
    bio TEXT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

**One-to-many (1:N):** A customer has many orders; an order belongs to one customer. The foreign key goes on the many side.

```sql
CREATE TABLE customers (id INT PRIMARY KEY, name VARCHAR(100));
CREATE TABLE orders (
    id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

**Many-to-many (M:N):** A product belongs to many categories; a category contains many products. Resolve with a junction (associative) table.

```sql
CREATE TABLE products (id INT PRIMARY KEY, name VARCHAR(100));
CREATE TABLE categories (id INT PRIMARY KEY, name VARCHAR(100));

CREATE TABLE product_categories (
    product_id INT NOT NULL REFERENCES products(id),
    category_id INT NOT NULL REFERENCES categories(id),
    PRIMARY KEY (product_id, category_id)
);
```

The junction table `product_categories` decomposes the many-to-many into two one-to-many relationships.

### Keys

**Candidate keys** are columns (or column sets) that uniquely identify every row in a table. Every candidate key is a potential primary key. From the set of candidates, one is chosen as the **primary key**; the others become alternate keys (enforced with UNIQUE constraints).

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE,
    username VARCHAR(50) NOT NULL UNIQUE
);
```

Both `email` and `username` are candidate keys. `id` is chosen as the primary key.

**Natural keys** are inherent to the data and have real-world meaning -- a user's email, a product's ISBN, a country's ISO code. They simplify queries (no JOIN needed to get the meaningful value) but can cause problems: email changes, ISBNs have different formats, values can be long.

**Surrogate keys** are artificial, system-generated identifiers with no business meaning. Auto-increment integers and UUIDs are the two most common types.

```sql
-- Auto-increment surrogate key (PostgreSQL SERIAL / MySQL AUTO_INCREMENT)
CREATE TABLE users (id SERIAL PRIMARY KEY, email VARCHAR(255) NOT NULL UNIQUE);

-- UUID surrogate key
CREATE TABLE users (id UUID PRIMARY KEY DEFAULT gen_random_uuid(), email VARCHAR(255) NOT NULL UNIQUE);
```

| Property | Auto-increment INT | UUID |
|----------|--------------------|------|
| Size | 4 bytes (INT) / 8 bytes (BIGINT) | 16 bytes |
| Readable | Yes, sequential | No, opaque |
| Ordering | Insert order | Random (or time-based with UUID v7) |
| Merge-friendly | No (collisions across databases) | Yes (globally unique) |
| Index performance | Excellent (sequential, clustered) | Poor with v4 random (B-tree fragmentation) |
| Exposure in URLs | Predictable, information leak | Opaque, safe |

**Rule of thumb:** Use auto-increment for internal systems where IDs never leave the database. Use UUIDs for distributed systems, microservices, or when IDs are exposed in client-facing URLs.

**Foreign keys** reference a primary key in another table and enforce referential integrity.

```sql
ALTER TABLE orders ADD CONSTRAINT fk_orders_customer
    FOREIGN KEY (customer_id) REFERENCES customers(id)
    ON DELETE CASCADE;
```

`ON DELETE CASCADE` propagates deletes: removing a customer removes all their orders. `ON DELETE SET NULL` sets the FK to NULL. `ON DELETE RESTRICT` (or `NO ACTION`) prevents deletion if references exist.

### Normalization

Normalization removes redundancy and prevents update anomalies. The three most common normal forms build on each other.

#### First Normal Form (1NF)

A table is in 1NF when all columns contain atomic (indivisible) values and there are no repeating groups or arrays.

**Violation -- storing multiple phone numbers in one column:**

```sql
CREATE TABLE contacts (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    phone_numbers VARCHAR(200)  -- "555-0101,555-0102,555-0103"
);
```

Querying "find everyone with phone 555-0102" requires a text search. Updating one number requires parsing and rewriting the whole string.

**Violation -- repeating columns:**

```sql
CREATE TABLE contacts (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    phone_home VARCHAR(20),
    phone_work VARCHAR(20),
    phone_mobile VARCHAR(20)
);
```

Adding a fourth phone number requires a schema change. The normalized fix for both violations: a child table `contact_phones` with one row per phone number.

```sql
CREATE TABLE contacts (id INT PRIMARY KEY, name VARCHAR(100));
CREATE TABLE contact_phones (
    contact_id INT NOT NULL REFERENCES contacts(id),
    phone VARCHAR(20) NOT NULL,
    phone_type VARCHAR(10),
    PRIMARY KEY (contact_id, phone)
);
```

#### Second Normal Form (2NF)

A table is in 2NF when:
1. It is in 1NF.
2. Every non-key column is fully functionally dependent on the **entire** primary key (no partial dependencies).

2NF only matters for tables with composite primary keys. A table with a single-column primary key that is in 1NF is automatically in 2NF.

**Violation -- partial dependency:**

```sql
CREATE TABLE order_details (
    order_id INT,
    product_id INT,
    order_date DATE,                   -- depends only on order_id
    product_name VARCHAR(100),         -- depends only on product_id
    quantity INT,
    unit_price DECIMAL(10,2),
    PRIMARY KEY (order_id, product_id)
);
```

If the same order has 10 products, `order_date` is repeated 10 times. Updating it means touching every row.

**Fix -- decompose into three tables:**

```sql
CREATE TABLE orders (id INT PRIMARY KEY, order_date DATE NOT NULL, customer_id INT NOT NULL);
CREATE TABLE products (id INT PRIMARY KEY, name VARCHAR(100) NOT NULL);
CREATE TABLE order_items (
    order_id INT NOT NULL REFERENCES orders(id),
    product_id INT NOT NULL REFERENCES products(id),
    quantity INT NOT NULL,
    unit_price DECIMAL(10,2) NOT NULL,
    PRIMARY KEY (order_id, product_id)
);
```

Now `order_date` lives in `orders` once. `product_name` lives in `products` once.

#### Third Normal Form (3NF)

A table is in 3NF when:
1. It is in 2NF.
2. Every non-key column is **non-transitively** dependent on the primary key (no transitive dependencies).

A transitive dependency exists when column A determines column B, and column B determines column C, making C transitively dependent on A through B.

**Violation -- transitive dependency:**

```sql
CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    warehouse_id INT,
    warehouse_city VARCHAR(100)   -- transitively depends on id via warehouse_id
);
```

If ten thousand products are stored in warehouse 42, the string "Chicago" is repeated ten thousand times. Renaming the warehouse city requires updating every product row.

**Fix -- extract the dependent entity:**

```sql
CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    warehouse_id INT NOT NULL REFERENCES warehouses(id)
);

CREATE TABLE warehouses (id INT PRIMARY KEY, city VARCHAR(100) NOT NULL);
```

Now `city` lives in `warehouses` once.

#### When to Stop Normalizing

Every normal form reduces redundancy but increases the number of JOINs. Full 3NF is a good default for OLTP (Online Transaction Processing) workloads. Beyond 3NF are BCNF, 4NF, 5NF, and DKNF -- rarely needed in practice.

**Denormalization** is the intentional reintroduction of redundancy for performance reasons. Common cases:

- Read-heavy dashboards that aggregate millions of rows. A pre-computed `order_total` column on `orders` avoids summing `order_items` on every page load.
- Reporting tables that flatten a 3NF schema into a wide denormalized structure for analytical queries.
- High-traffic reads where a JOIN on every request is too expensive.

Denormalize deliberately and document the trade-off: you gain read speed at the cost of write complexity and potential inconsistency.

### Practical Design Walkthrough: E-Commerce Database

Design the core tables for a typical online store. Every decision follows from a requirement.

#### Users Table

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    email VARCHAR(255) NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    full_name VARCHAR(200) NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
CREATE UNIQUE INDEX idx_users_email ON users(email);
```

BIGSERIAL handles future growth. Separating the unique index from the column gives explicit naming control. TIMESTAMPTZ avoids time zone confusion.

#### Products Table

```sql
CREATE TABLE products (
    id BIGSERIAL PRIMARY KEY,
    name VARCHAR(300) NOT NULL,
    description TEXT,
    price DECIMAL(12,2) NOT NULL CHECK (price >= 0),
    stock_quantity INT NOT NULL DEFAULT 0 CHECK (stock_quantity >= 0),
    sku VARCHAR(50) NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
CREATE UNIQUE INDEX idx_products_sku ON products(sku);
```

DECIMAL avoids floating-point rounding on money. CHECK constraints enforce business rules at the database level. SKU is a natural key with a unique index.

#### Categories and Product-Categories (Many-to-Many)

```sql
CREATE TABLE categories (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    parent_id INT REFERENCES categories(id),
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
CREATE INDEX idx_categories_parent ON categories(parent_id);

CREATE TABLE product_categories (
    product_id BIGINT NOT NULL REFERENCES products(id) ON DELETE CASCADE,
    category_id INT NOT NULL REFERENCES categories(id) ON DELETE CASCADE,
    PRIMARY KEY (product_id, category_id)
);
```

Self-referencing `parent_id` supports hierarchies (Electronics > Computers > Laptops). The composite PK enforces a product is in a category at most once.

#### Orders and Order Items

```sql
CREATE TABLE orders (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT NOT NULL REFERENCES users(id),
    status VARCHAR(20) NOT NULL DEFAULT 'pending'
        CHECK (status IN ('pending','confirmed','shipped','delivered','cancelled')),
    total_amount DECIMAL(14,2) NOT NULL DEFAULT 0.00,
    shipping_address TEXT NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
CREATE INDEX idx_orders_user ON orders(user_id);
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_orders_created ON orders(created_at);

CREATE TABLE order_items (
    id BIGSERIAL PRIMARY KEY,
    order_id BIGINT NOT NULL REFERENCES orders(id) ON DELETE CASCADE,
    product_id BIGINT NOT NULL REFERENCES products(id),
    quantity INT NOT NULL CHECK (quantity > 0),
    unit_price DECIMAL(12,2) NOT NULL,
    subtotal DECIMAL(14,2) NOT NULL
);
CREATE INDEX idx_order_items_order ON order_items(order_id);
CREATE INDEX idx_order_items_product ON order_items(product_id);
```

`unit_price` is a snapshot at purchase time since product prices change. `total_amount` on `orders` is denormalized (maintained by trigger or application code) so that listing orders does not require summing `order_items` every time.

#### Reviews Table

```sql
CREATE TABLE reviews (
    id BIGSERIAL PRIMARY KEY,
    product_id BIGINT NOT NULL REFERENCES products(id) ON DELETE CASCADE,
    user_id BIGINT NOT NULL REFERENCES users(id),
    rating INT NOT NULL CHECK (rating >= 1 AND rating <= 5),
    title VARCHAR(200),
    body TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
CREATE INDEX idx_reviews_product ON reviews(product_id);
CREATE INDEX idx_reviews_user ON reviews(user_id);
CREATE UNIQUE INDEX idx_reviews_unique ON reviews(product_id, user_id);
```

The unique composite index prevents a user from reviewing the same product twice.

### Indexes

Indexes are data structures that speed up retrieval at the cost of slower writes and increased storage. Without an index, the database performs a **sequential scan** -- reading every row in the table. On a 10-million-row table, that is catastrophic for a point lookup.

#### How Indexes Work (B-Tree)

The default index type in every major SQL database is the **B-tree** (balanced tree). Keys are stored in sorted order in a tree where every leaf is at the same depth, guaranteeing O(log n) search time. For 1,000,000 rows, a B-tree finds any row in about 3-4 node traversals. A sequential scan reads up to 1,000,000 rows.

```text
Search for id = 42 in a B-tree:
          [50]
         /    \
      [25]    [75]
     /   \    /   \
  [10,20] [30,40] [60,70] [80,90]
                      ^
                 3 hops vs ~42 rows scanned
```

#### Clustered vs Non-Clustered Indexes

**Clustered index** determines the physical order of rows on disk. A table can have only one. In MySQL (InnoDB), the primary key is the clustered index. Leaf nodes contain the full row data, so a PK lookup is a single B-tree traversal.

**Non-clustered index** stores index keys plus a pointer (row ID or clustered key) to the actual row. A table can have many. Querying on a non-clustered index requires two traversals: first the non-clustered B-tree, then the clustered index to fetch the full row (a **bookmark lookup**).

#### Composite Indexes (Column Order Matters)

A composite index on multiple columns is a B-tree sorted by column 1, then column 2, then column 3.

```sql
CREATE INDEX idx_composite ON orders (user_id, status, created_at);
```

This index accelerates queries starting with `user_id` (leftmost column). It does **not** accelerate queries filtering on `status` or `created_at` alone -- this is the **leftmost prefix rule**.

**Column order strategy:** Put equality conditions first, range conditions last (>, <, BETWEEN), and high-cardinality columns before low-cardinality columns.

#### When Indexes Help

**WHERE clause filtering:** Without an index on `email`, a sequential scan reads 5M rows. With an index, a B-tree lookup finds the row in ~4 reads.

**JOIN operations:** Without an index on `orders.user_id`, the database scans every order row for each matching user. With an index, it performs an index seek per match.

**ORDER BY without extra sort:** If an index matches the ORDER BY clause, the database reads index pages in order and avoids a full sort operation.

#### When Indexes Hurt

Every index adds overhead on write operations. Each INSERT adds an entry to every index on the table. An UPDATE of an indexed column requires deleting the old entry and inserting the new one. DELETE removes entries from every index.

```sql
-- Table with 5 secondary indexes: each INSERT performs 5 B-tree insertions
CREATE TABLE t (id INT PRIMARY KEY, a INT, b INT, c INT, d INT, e INT);
CREATE INDEX idx_a ON t(a); CREATE INDEX idx_b ON t(b);
CREATE INDEX idx_c ON t(c); CREATE INDEX idx_d ON t(d);
CREATE INDEX idx_e ON t(e);
INSERT INTO t VALUES (1, 10, 20, 30, 40, 50);  -- 5 index writes
```

**Storage:** Indexes consume disk space. A 100 GB table with 10 indexes can consume 300 GB total.

#### EXPLAIN / EXPLAIN ANALYZE

Every SQL database provides a way to see the query plan. Understanding the plan is essential for diagnosing slow queries.

```sql
EXPLAIN SELECT * FROM users WHERE email = 'alice@example.com';
-- Output: Index Scan using idx_users_email on users (good)
```

A query with no matching index:

```sql
EXPLAIN ANALYZE SELECT * FROM orders WHERE total_amount > 1000;
```

```text
 Seq Scan on orders  (cost=0.00..83427.00 rows=5231 width=72)
                     (actual time=0.025..342.127 rows=5200 loops=1)
   Filter: (total_amount > 1000)
   Rows Removed by Filter: 4994800
 Planning Time: 0.084 ms
 Execution Time: 342.451 ms
```

342 ms for a filtered scan of 5 million rows. An index would drop this to sub-millisecond.

#### Avoid Over-Indexing

- Index columns used in JOIN conditions and WHERE filters on large tables (100k+ rows).
- Do not index small tables (a few hundred rows) -- a sequential scan is faster than a B-tree.
- Do not index low-cardinality columns (boolean, status) unless part of a composite index.
- Remove unused indexes. Run `pg_stat_user_indexes` (PostgreSQL) or `sys.dm_db_index_usage_stats` (SQL Server).
- Monitor index bloat and rebuild periodically.

### Migrations

The schema is not static. Migrations are version-controlled scripts that evolve the schema over time.

#### Adding Columns

```sql
ALTER TABLE users ADD COLUMN phone VARCHAR(20);                               -- safe
ALTER TABLE users ADD COLUMN newsletter_optin BOOLEAN NOT NULL DEFAULT true;   -- metadata-only in PG
```

Adding a `NOT NULL` column without a default on a table with existing data will fail.

#### Adding Constraints

```sql
ALTER TABLE orders ADD CONSTRAINT fk_orders_user FOREIGN KEY (user_id) REFERENCES users(id);
ALTER TABLE products ADD CONSTRAINT chk_products_price CHECK (price >= 0);
```

Adding a foreign key acquires a table lock in many databases. For large tables use `NOT VALID` (PostgreSQL) and validate later.

#### Backfilling Data

Backfill in batches to avoid long transactions and replication lag:

```sql
ALTER TABLE orders ADD COLUMN shipping_region VARCHAR(50);
UPDATE orders SET shipping_region = 'domestic'
WHERE shipping_region IS NULL AND shipping_address LIKE '%United States%'
LIMIT 10000;
```

Repeat until no NULLs remain, then add the NOT NULL constraint.

#### Zero-Downtime Migration Patterns

**Expand-contract pattern:**
1. Add the new column or table (expand). Application writes to both old and new.
2. Backfill data.
3. Deploy code that reads from the new location.
4. Remove the old column or table (contract).

**Online schema change tools:** pt-online-schema-change (MySQL), pgroll (PostgreSQL), or database-native features like `ALTER TABLE ... ALTER COLUMN ... SET NOT NULL` with a CHECK option.

**No-lock migrations:** In PostgreSQL, adding a column with a non-volatile default is instant (metadata-only). In MySQL 8.0+, `ALGORITHM=INSTANT` supports non-blocking DDL for certain operations.

### Seed Data

Seed data populates a development database with representative records.

```sql
INSERT INTO users (email, password_hash, full_name) VALUES
    ('alice@example.com', '$2a$10$abc...', 'Alice Johnson'),
    ('bob@example.com', '$2a$10$def...', 'Bob Smith');
INSERT INTO categories (id, name, parent_id) VALUES
    (1, 'Electronics', NULL), (2, 'Computers', 1), (3, 'Laptops', 2);
INSERT INTO products (name, price, stock_quantity, sku) VALUES
    ('Wireless Mouse', 29.99, 150, 'WM-001'),
    ('Mechanical Keyboard', 89.99, 75, 'MK-002');
INSERT INTO product_categories (product_id, category_id) VALUES (1, 1), (1, 2), (2, 1), (2, 2);
```

Best practices: use transactions for atomicity, make seeds idempotent (`ON CONFLICT DO NOTHING`), keep them in version control under `seeds.sql`, and separate them from migration scripts.

## Glossary

| Term | Definition |
|------|------------|
| Attribute | A property or column of an entity (e.g., email, price) |
| B-tree | A balanced tree data structure used by default indexes for O(log n) lookup |
| Cardinality | The numerical constraint on a relationship (1:1, 1:N, M:N) |
| Cascade | A referential action that propagates changes (DELETE CASCADE removes child rows when parent is deleted) |
| Clustered index | An index that determines the physical order of rows on disk; one per table |
| Composite index | An index on two or more columns, sorted by column order |
| Denormalization | Intentional reintroduction of redundancy to improve read performance |
| Entity | A real-world thing distinguishable from others (user, product, order) |
| ER diagram | A visual representation of entities, attributes, and relationships |
| Foreign key | A column (or set of columns) that references a primary key in another table |
| Index | An auxiliary data structure that speeds up data retrieval at the cost of write overhead |
| Migration | A version-controlled script that evolves the database schema over time |
| Natural key | A key with real-world meaning (email, ISBN, ISO code) |
| Normalization | The process of eliminating redundancy to prevent update anomalies |
| Primary key | A column (or set of columns) that uniquely identifies each row |
| Referential integrity | The guarantee that foreign key values match an existing primary key |
| Relationship | An association between two entities (user places order) |
| Schema | The structure of a database: tables, columns, types, constraints, indexes |
| Seed data | Representative data used to populate a database for development and testing |
| Surrogate key | An artificial system-generated identifier (auto-increment, UUID) |
| Weak entity | An entity whose identification depends on a parent entity (order_items depends on orders) |

## Quick References

- [Use the Index, Luke](https://use-the-index-luke.com/) -- comprehensive guide to SQL indexing
- [Database Design Basics (PostgreSQL Docs)](https://www.postgresql.org/docs/current/ddl.html) -- official DDL reference
- [Normalization by TutorialsPoint](https://www.tutorialspoint.com/sql/sql-normalization.htm) -- step-by-step normal form examples
- [pt-online-schema-change (Percona)](https://docs.percona.com/percona-toolkit/pt-online-schema-change.html) -- zero-downtime schema changes for MySQL
- [Schema Design and Data Modeling (MongoDB)](https://www.mongodb.com/docs/manual/core/data-modeling-introduction/) -- comparisons for document databases

## Next Steps

- Back to [SQL Module Index](../index.md) -- overview of all SQL documents
- [Performance Tuning](performance-tuning.md) -- deeper dive into query optimization, indexing strategies, and execution plans
- [Constraints & Triggers](constraints-triggers.md) -- advanced integrity rules and automatic actions
- Explore the [Software Engineer career profile](../../careers/software-engineer/index.md) for database design in production systems
