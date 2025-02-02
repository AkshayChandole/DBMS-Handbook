# [Exchange Seats](#exchange-seats)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Seat (id int, student varchar(255))
Truncate table Seat
insert into Seat (id, student) values ('1', 'Abbot')
insert into Seat (id, student) values ('2', 'Doris')
insert into Seat (id, student) values ('3', 'Emerson')
insert into Seat (id, student) values ('4', 'Green')
insert into Seat (id, student) values ('5', 'Jeames')
```

---

### [Question](#question)

Table: Seat
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| student     | varchar |
+-------------+---------+
```
- id is the primary key (unique value) column for this table.
- Each row of this table indicates the name and the ID of a student.
- The ID sequence always starts from 1 and increments continuously.
 

Write a solution to swap the seat id of every two consecutive students. If the number of students is odd, the id of the last student is not swapped.

Return the result table ordered by id in ascending order.

The result format is in the following example.

 

#### Example 1:

Input: 

Seat table:
```
+----+---------+
| id | student |
+----+---------+
| 1  | Abbot   |
| 2  | Doris   |
| 3  | Emerson |
| 4  | Green   |
| 5  | Jeames  |
+----+---------+
```
Output: 
```
+----+---------+
| id | student |
+----+---------+
| 1  | Doris   |
| 2  | Abbot   |
| 3  | Green   |
| 4  | Emerson |
| 5  | Jeames  |
+----+---------+
```
Explanation: 
Note that if the number of students is odd, there is no need to change the last one's seat.

---

### [Solution](#solution)

#### **Solution Explanation**
```sql
SELECT 
    CASE 
        WHEN id % 2 = 1 AND id != (SELECT MAX(id) FROM Seat) THEN id + 1
        WHEN id % 2 = 0 THEN id - 1
        ELSE id
    END AS id,
    student
FROM Seat
ORDER BY id;
```

---

#### **Breakdown of the Query**
1. **Swapping Seat IDs for Consecutive Pairs:**
   - `id % 2 = 1 AND id != (SELECT MAX(id) FROM Seat) THEN id + 1`
     - If the `id` is **odd**, move the student **to the next seat**.
     - The condition `id != (SELECT MAX(id) FROM Seat)` ensures that if the **total number of students is odd**, the last student **remains in place**.
   - `id % 2 = 0 THEN id - 1`
     - If the `id` is **even**, move the student **to the previous seat**.

2. **Handling the Last Student in Odd Cases:**
   - The `ELSE id` condition keeps the last student **unchanged** when the total count is odd.

3. **Ordering the Output by ID:**
   - `ORDER BY id` ensures the result is displayed in **ascending order** of seat IDs.

---

#### **Example Execution**
##### **Input Table:**
| id | student  |
|----|---------|
| 1  | Abbot   |
| 2  | Doris   |
| 3  | Emerson |
| 4  | Green   |
| 5  | Jeames  |

---

##### **Step-by-Step Execution**
| Current ID | New ID | Student  |
|------------|--------|---------|
| 1          | 2      | Abbot   |
| 2          | 1      | Doris   |
| 3          | 4      | Emerson |
| 4          | 3      | Green   |
| 5          | 5      | Jeames  |

---

##### **Final Output Table:**
| id | student  |
|----|---------|
| 1  | Doris   |
| 2  | Abbot   |
| 3  | Green   |
| 4  | Emerson |
| 5  | Jeames  |

✅ **This query efficiently swaps students while keeping the last one unchanged if the total count is odd.** 🚀

---
