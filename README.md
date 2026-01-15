# ⭐️ SQL Survival Guide

### with PostgreSQL notes, written while learning ✏️📓

A **personal SQL cheat sheet** that is:

* beginner-friendly
* PostgreSQL-leaning
* buildable organization like handwritten notes
* visual when possible

I use this as a reference while practicing and building projects — sharing it in case it’s useful to someone else too.

---

## 📘 Sources & Practice

Inspired by:

* **PostgreSQL Exercises** — [https://pgexercises.com](https://pgexercises.com)
* PostgreSQL Official Documentation
* SQLBolt
* Mode SQL Tutorial

🧪 Practice links point to PostgreSQL Exercises.

---

## ⭐️ Visual Legend

| Symbol | Meaning             |
| ------ | ------------------- |
| ✏️     | Keyword             |
| 📄     | Explanation         |
| 🧠     | Mental model        |
| 📓     | Diagram             |
| 🧪     | Practice            |
| ⭐️     | Key idea            |
| ⚠️     | Common mistake      |
| ☕️     | Personal note       |
| 🧩     | PostgreSQL-specific |

---

## 📓 Table of Contents

* [✏️ Basic Data Retrieval](#️-basic-data-retrieval)
* [📄 Filtering Data](#-filtering-data)
* [✏️ Sorting & Limiting](#️-sorting--limiting)
* [📄 Joins](#-joins)
* [✏️ Aggregation](#️-aggregation)
* [📄 Subqueries & CTEs](#-subqueries--ctes)
* [✏️ Data Modification](#️-data-modification)
* [📄 Tables & Relationships](#-tables--relationships)
* [🧩 PostgreSQL-Specific Features](#-postgresql-specific-features)
* [⚠️ Common SQL Mistakes](#️-common-sql-mistakes)
* [📘 Sources & Practice](#-sources--practice)

---

## ✏️ Basic Data Retrieval

### ✏️ SELECT

📄 Choose which columns you want to see.

```sql
SELECT firstname, surname
FROM members;
```

🧠 SELECT = what you’re looking at

📓 Diagram

```text
members
┌─────┬───────────┬──────────┐
│ id  │ firstname │ surname  │
└─────┴───────────┴──────────┘
        ↑ SELECT ↑
```

🧪 Practice
[https://pgexercises.com/questions/basic/selectall.html](https://pgexercises.com/questions/basic/selectall.html)

---

### ✏️ SELECT *

📄 Select all columns.

```sql
SELECT *
FROM members;
```

☕️ I use this a lot while learning.
⚠️ In real apps, it’s usually better to be specific.

---

### ✏️ DISTINCT

📄 Remove duplicate rows.

```sql
SELECT DISTINCT city
FROM members;
```

📓 Diagram

```text
Before:
London
London
Paris

After:
London
Paris
```

⭐️ DISTINCT applies to the entire row.

🧪 Practice
[https://pgexercises.com/questions/basic/distinct.html](https://pgexercises.com/questions/basic/distinct.html)

---

## 📄 Filtering Data

### ✏️ WHERE

📄 Filter rows based on conditions.

```sql
SELECT *
FROM members
WHERE city = 'London';
```

🧠 WHERE decides which rows stay.

📓 Diagram

```text
All rows
↓
London   ✔
Paris    ✖
London   ✔
```

🧪 Practice
[https://pgexercises.com/questions/basic/where.html](https://pgexercises.com/questions/basic/where.html)

---

### ✏️ AND / OR

📄 Combine conditions.

```sql
WHERE city = 'London'
AND age >= 18;
```

☕️ AND always trips me up when queries get longer.

---

### ✏️ LIKE / ILIKE

📄 Match patterns in text.

```sql
WHERE firstname LIKE 'A%';
```

🧩 PostgreSQL tip

```sql
WHERE firstname ILIKE 'a%';
```

📓 Diagram

```text
A%
Ava   ✔
Anna ✔
Mia   ✖
```

🧪 Practice
[https://pgexercises.com/questions/basic/like.html](https://pgexercises.com/questions/basic/like.html)

---

### ✏️ IS NULL

📄 Check for missing values.

```sql
WHERE recommendedby IS NULL;
```

⚠️ `NULL = NULL` doesn’t work in SQL.

---

## ✏️ Sorting & Limiting

### ✏️ ORDER BY

📄 Sort results.

```sql
ORDER BY surname ASC;
```

📓

```text
ASC  A → Z
DESC Z → A
```

---

### ✏️ LIMIT / OFFSET

📄 Control how many rows you see.

```sql
LIMIT 10 OFFSET 20;
```

🧠 LIMIT = how many
🧠 OFFSET = how many to skip

☕️ This finally made pagination make sense to me.

---

## 📄 Joins

### ✏️ INNER JOIN

📄 Only rows that match in both tables.

```sql
SELECT m.firstname, b.starttime
FROM members m
INNER JOIN bookings b
ON m.memid = b.memid;
```

📓 Diagram

```text
members        bookings
  ●────✔────●   keep
  ●            drop
```

⭐️ INNER JOIN = overlap only.

---

### ✏️ LEFT JOIN

📄 Keep all rows from the left table.

```sql
LEFT JOIN bookings b
ON m.memid = b.memid;
```

📓 Diagram

```text
members        bookings
  ●────✔────●
  ●            NULL
```

☕️ This is the join I reach for most often.

---

## ✏️ Aggregation

### ✏️ COUNT / SUM / AVG

📄 Do math across rows.

```sql
SELECT COUNT(*)
FROM members;
```

---

### ✏️ GROUP BY

📄 Group rows before aggregating.

```sql
SELECT city, COUNT(*)
FROM members
GROUP BY city;
```

📓

```text
rows → groups → count
```

🧠 GROUP BY = make buckets first.

🧪 Practice
[https://pgexercises.com/questions/aggregates/basic.html](https://pgexercises.com/questions/aggregates/basic.html)

---

### ✏️ HAVING

📄 Filter grouped results.

```sql
HAVING COUNT(*) > 5;
```

⚠️ WHERE filters rows
⚠️ HAVING filters groups

---

## 📄 Subqueries & CTEs

### ✏️ Subquery

📄 A query inside another query.

```sql
WHERE memid IN (
  SELECT memid FROM bookings
);
```

📓

```text
inner query → list
outer query → filter
```

---

### ✏️ WITH (CTE)

📄 Temporary named query.

```sql
WITH recent AS (
  SELECT * FROM bookings
)
SELECT * FROM recent;
```

☕️ CTEs make long queries readable.

---

## ✏️ Data Modification

### ✏️ INSERT

```sql
INSERT INTO members (firstname)
VALUES ('Jane');
```

---

### ✏️ UPDATE

```sql
UPDATE members
SET city = 'London'
WHERE memid = 1;
```

⚠️ Forgetting WHERE updates everything.

---

### ✏️ DELETE

```sql
DELETE FROM members
WHERE memid = 1;
```

---

### 🧩 RETURNING

📄 Get values back immediately.

```sql
INSERT INTO members (firstname)
VALUES ('Anna')
RETURNING memid;
```

⭐️ PostgreSQL-specific and very useful.

---

## 📄 Tables & Relationships

### ✏️ PRIMARY KEY

📄 Unique identifier for each row.

### ✏️ FOREIGN KEY

📄 Connect related tables.

📓

```text
members.memid  ←───✔───→  bookings.memid
```

---

## 🧩 PostgreSQL-Specific Features

* `ILIKE`
* `RETURNING`
* `ON CONFLICT` (UPSERT)
* `JSONB`
* `ARRAY`
* `UUID`
* `EXPLAIN ANALYZE`

---

## ⚠️ Common SQL Mistakes

❌ WHERE with aggregates
✅ HAVING

❌ Comparing NULL values
✅ Use IS NULL

❌ Forgetting JOIN conditions
✅ Always use ON

❌ Overusing SELECT *
✅ Select what you need

---



---

