# CS50 Week 7 — SQL

## Summary
SQL (Structured Query Language) is a domain-specific language for creating, reading, updating, and deleting data in relational databases. SQLite is a lightweight, file-based SQL database used throughout CS50. Tables store data in rows and columns, with a primary key uniquely identifying each row and foreign keys linking tables together. The four core SQL operations are SELECT, INSERT, UPDATE, and DELETE. JOINs combine rows from multiple tables based on a related column. Indexes speed up queries by creating a B-Tree data structure at the cost of extra storage space. SQL can be used directly from Python via the cs50 SQL library, with parameterized queries (? placeholders) preventing SQL injection attacks.

---

## Notes

### Relational Databases & SQLite

- A **relational database** stores data in structured tables with rows and columns
- **SQLite** — lightweight, serverless, stores entire DB in a single `.db` file
- Used via CS50's `SQL` class or Python's built-in `sqlite3` module
- Tables relate to each other through **keys**

---

### CRUD Operations

```sql
-- SELECT (Read)
SELECT * FROM users;
SELECT name, age FROM users WHERE age > 18;
SELECT * FROM users ORDER BY name ASC;
SELECT * FROM users WHERE name LIKE '%alice%';
SELECT COUNT(*) FROM users;

-- INSERT (Create)
INSERT INTO users (name, age) VALUES ('Alice', 21);

-- UPDATE
UPDATE users SET age = 22 WHERE name = 'Alice';

-- DELETE
DELETE FROM users WHERE name = 'Alice';
```

---

### Tables, Keys & Data Types

**Creating a table:**
```sql
CREATE TABLE users (
    id    INTEGER PRIMARY KEY AUTOINCREMENT,
    name  TEXT    NOT NULL,
    age   INTEGER,
    email TEXT    UNIQUE
);
```

**SQLite Data Types:**
- `INTEGER` — whole numbers
- `REAL` — floating point
- `TEXT` — strings
- `BLOB` — binary data
- `NULL` — missing value

**Keys:**
- **Primary key** — uniquely identifies each row (usually `id`)
- **Foreign key** — references a primary key in another table
- **AUTOINCREMENT** — automatically assigns next integer ID

---

### JOINs

JOINs combine rows from two or more tables based on a related column.

```sql
-- JOIN (INNER JOIN) — only matching rows
SELECT users.name, orders.item
FROM users
JOIN orders ON users.id = orders.user_id;

-- LEFT JOIN — all rows from left table, matching from right
SELECT users.name, orders.item
FROM users
LEFT JOIN orders ON users.id = orders.user_id;
```

**Relationship types:**
- **One-to-one** — one user has one profile
- **One-to-many** — one user has many orders
- **Many-to-many** — students and courses (requires junction table)

---

### Indexes

```sql
CREATE INDEX idx_name ON users(name);
```

- An **index** is a data structure (B-Tree) that speeds up lookups on a column
- Without an index: full table scan — O(n)
- With an index: B-Tree lookup — O(log n)
- **Tradeoff**: faster reads, slower writes, more storage space
- Always index columns you frequently search or sort by

---

### Subqueries

```sql
-- Find users who have placed orders
SELECT name FROM users
WHERE id IN (SELECT user_id FROM orders);
```

- A subquery is a query nested inside another query
- Runs the inner query first, uses the result in the outer query

---

### SQL in Python

```python
from cs50 import SQL

db = SQL("sqlite:///mydata.db")

# SELECT — returns list of dicts
rows = db.execute("SELECT * FROM users WHERE age > ?", 18)
for row in rows:
    print(row["name"])

# INSERT
db.execute("INSERT INTO users (name, age) VALUES (?, ?)", "Alice", 21)

# UPDATE
db.execute("UPDATE users SET age = ? WHERE name = ?", 22, "Alice")

# DELETE
db.execute("DELETE FROM users WHERE name = ?", "Alice")
```

---

### Race Conditions & SQL Injection

**SQL Injection** — attacker injects SQL code via user input:
```sql
-- Dangerous (string concatenation)
"SELECT * FROM users WHERE name = '" + name + "'"

-- Safe (parameterized query)
db.execute("SELECT * FROM users WHERE name = ?", name)
```

**Race condition** — two users update the same row simultaneously, causing data corruption. Fixed with **transactions**:
```sql
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

---

## Cue Column

| Term | Definition |
|---|---|
| **SQL** | Structured Query Language — domain-specific language for databases |
| **SQLite** | Lightweight, file-based relational database |
| **Primary key** | Unique identifier for each row in a table |
| **Foreign key** | Column that references a primary key in another table |
| **JOIN** | Combines rows from multiple tables based on a related column |
| **Index** | B-Tree structure that speeds up column lookups at cost of storage |
| **Subquery** | A query nested inside another query |
| **SQL Injection** | Attack that inserts malicious SQL via unsanitized user input |
| **Parameterized query** | Uses `?` placeholders to safely insert user input into SQL |
| **Transaction** | Group of SQL operations that succeed or fail together |
