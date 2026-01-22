
# [Normalization](#normalization)

## ❓ What is **Normalization**?

Explain different **normal forms (1NF, 2NF, 3NF, BCNF)**.

---

## ✅ What is Normalization?

### Definition (Interview-ready)

> **Normalization** is the process of organizing data in a database to **reduce redundancy** and **avoid data anomalies** (insert, update, delete).

### Goals

* Remove duplicate data
* Improve data integrity
* Make database easier to maintain

---

## 1️⃣ First Normal Form (1NF)

### Rule

* Each column must have **atomic (indivisible) values**
* No repeating groups or multivalued attributes

### ❌ Not in 1NF

| emp_id | skills    |
| ------ | --------- |
| 1      | C++, Java |

### ✅ In 1NF

| emp_id | skill |
| ------ | ----- |
| 1      | C++   |
| 1      | Java  |

---

### Why 1NF is needed

* Queries become simpler
* Avoids ambiguity in data access

---

## 2️⃣ Second Normal Form (2NF)

### Rule

* Must be in **1NF**
* No **partial dependency**
* Non-key attributes must depend on **entire primary key**

### ❌ Partial Dependency Example

Primary key: `(order_id, product_id)`

| order_id | product_id | product_name |
| -------- | ---------- | ------------ |

Here:

* `product_name` depends only on `product_id` → ❌

### ✅ Fix (2NF)

Split tables:

* Order_Product(order_id, product_id)
* Product(product_id, product_name)

---

### Why 2NF is needed

* Prevents redundant data
* Reduces update anomalies

---

## 3️⃣ Third Normal Form (3NF)

### Rule

* Must be in **2NF**
* No **transitive dependency**
* Non-key attributes depend **only on the primary key**

### ❌ Transitive Dependency Example

| emp_id | dept_id | dept_name |
| ------ | ------- | --------- |

Here:

* emp_id → dept_id
* dept_id → dept_name
* emp_id → dept_name ❌

### ✅ Fix (3NF)

* Employee(emp_id, dept_id)
* Department(dept_id, dept_name)

---

### Why 3NF is needed

* Eliminates indirect dependencies
* Improves consistency

---

## 4️⃣ Boyce–Codd Normal Form (BCNF)

### Rule

> For every functional dependency **A → B**,
> **A must be a super key**

### Why BCNF exists

* 3NF doesn’t handle **all anomalies**
* BCNF is a **stronger version** of 3NF

### ❌ BCNF Violation Example

| course | instructor | room |
| ------ | ---------- | ---- |

Dependencies:

* instructor → room
* course → instructor

Instructor is **not a super key** → ❌ BCNF violation

### ✅ Fix

Split tables:

* Instructor_Room(instructor, room)
* Course_Instructor(course, instructor)

---

## 📊 Summary Table

| Normal Form | Removes                  |
| ----------- | ------------------------ |
| 1NF         | Repeating groups         |
| 2NF         | Partial dependency       |
| 3NF         | Transitive dependency    |
| BCNF        | All non-key dependencies |

---

## 🔥 Follow-Up Questions & Answers

---

### 🔹 Q1: Is BCNF always better than 3NF?

**Answer:**
Not always.
BCNF may cause:

* More tables
* More joins
  So 3NF is often preferred for **performance vs purity balance**.

---

### 🔹 Q2: What are data anomalies?

**Answer:**

* **Insert anomaly** → can’t insert data without other data
* **Update anomaly** → same data updated in multiple places
* **Delete anomaly** → deleting data removes unintended information

---

### 🔹 Q3: Does normalization hurt performance?

**Answer:**
Yes, sometimes.

* More joins
* Slower reads
  That’s why **controlled denormalization** is used in practice.

---

### 🔹 Q4: What level of normalization is usually enough?

**Answer:**

* **3NF** is sufficient for most applications
* BCNF for critical consistency systems

---

## 🎯 Interview-Ready Summary

> “Normalization organizes data to remove redundancy and anomalies. 1NF removes repeating groups, 2NF removes partial dependencies, 3NF removes transitive dependencies, and BCNF enforces that every determinant is a super key.”

---
