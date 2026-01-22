# [Indexing](#indexing)

Explain **types of indexes** and **their trade-offs**.

---

## ✅ What is an Index?

### Definition (Interview-ready)

> An **index** is a data structure that improves the **speed of data retrieval** operations on a table at the cost of **extra storage and slower writes**.

Think of it like:

* **Book index** → jump directly to a page
* Instead of scanning every page

---

## 🔧 How an Index Works (High Level)

Without index:

```sql
SELECT * FROM Employee WHERE emp_id = 101;
```

➡️ Full table scan (`O(n)`)

With index on `emp_id`:
➡️ Direct lookup using index (`O(log n)` or better)

---

## 1️⃣ Clustered Index

### What it is

* Determines the **physical order** of data in the table
* Table data is stored **in the order of the index**

```sql
PRIMARY KEY (emp_id)
```

### Key points

* Only **one** clustered index per table
* Usually created on **PRIMARY KEY**
* Faster range queries

---

## 2️⃣ Non-Clustered Index

### What it is

* Separate structure from table data
* Stores **key → pointer to row**

```sql
CREATE INDEX idx_email ON Employee(email);
```

### Key points

* Multiple allowed per table
* Extra lookup to fetch actual row
* Slightly slower than clustered for reads

---

## 📊 Clustered vs Non-Clustered

| Feature          | Clustered    | Non-Clustered |
| ---------------- | ------------ | ------------- |
| Physical order   | Yes          | No            |
| Number per table | One          | Many          |
| Storage          | Data + index | Separate      |
| Range queries    | Very fast    | Slower        |

---

## 3️⃣ Composite (Multi-Column) Index

### What it is

* Index on **multiple columns**

```sql
CREATE INDEX idx_dept_salary
ON Employee(department, salary);
```

### Important rule (VERY IMPORTANT)

**Left-most prefix rule**

Index works for:

* `(department)`
* `(department, salary)`

Does NOT work for:

* `(salary)` alone ❌

---

## 4️⃣ Unique Index

### What it is

* Ensures **uniqueness**
* Prevents duplicate values

```sql
CREATE UNIQUE INDEX idx_email
ON Employee(email);
```

---

## 🔥 When Indexes Hurt Performance

### Writes become slower

* INSERT
* UPDATE
* DELETE

Why?

* Index must also be updated

---

## ⏱ Complexity Impact

| Operation | Without Index | With Index |
| --------- | ------------- | ---------- |
| SELECT    | O(n)          | O(log n)   |
| INSERT    | Fast          | Slower     |
| UPDATE    | Fast          | Slower     |

---

## 🔥 Follow-Up Questions & Answers

---

### 🔹 Q1: Why can we have only one clustered index?

**Answer:**
Because data rows can be physically sorted in **only one order**.

---

### 🔹 Q2: When should you NOT use an index?

**Answer:**

* Small tables
* Columns with low cardinality (e.g., gender)
* Tables with heavy writes

---

### 🔹 Q3: What is cardinality?

**Answer:**
Number of **unique values** in a column.
High cardinality → good index candidate.

---

### 🔹 Q4: Does index speed up `LIKE '%abc%'` queries?

**Answer:**
❌ No.
Leading wildcard prevents index usage.

---

### 🔹 Q5: How does an index affect memory?

**Answer:**

* Uses extra disk space
* Consumes cache memory
* Needs maintenance during writes

---

## 🎯 Interview-Ready Summary

> “An index is a data structure that improves read performance by reducing search time. Clustered indexes define physical order, while non-clustered indexes maintain pointers. Indexes speed up reads but slow down writes.”

---
