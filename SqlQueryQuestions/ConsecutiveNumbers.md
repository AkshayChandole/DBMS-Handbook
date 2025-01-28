# [Consecutive Numbers](#consecutive-numbers)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Logs (id int, num int)
Truncate table Logs
insert into Logs (id, num) values ('1', '1')
insert into Logs (id, num) values ('2', '1')
insert into Logs (id, num) values ('3', '1')
insert into Logs (id, num) values ('4', '2')
insert into Logs (id, num) values ('5', '1')
insert into Logs (id, num) values ('6', '2')
insert into Logs (id, num) values ('7', '2')
```

---

### [Question](#question)

Table: Logs
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| num         | varchar |
+-------------+---------+
```
- In SQL, id is the primary key for this table.
- id is an autoincrement column starting from 1.
 
Find all numbers that appear at least three times consecutively.

Return the result table in any order.

The result format is in the following example.

 

#### Example 1:

Input: 

Logs table:
```
+----+-----+
| id | num |
+----+-----+
| 1  | 1   |
| 2  | 1   |
| 3  | 1   |
| 4  | 2   |
| 5  | 1   |
| 6  | 2   |
| 7  | 2   |
+----+-----+
```
Output: 
```
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
```
Explanation: 1 is the only number that appears consecutively for at least three times.

---

### [Solution](#solution)

To solve this problem, we need to find numbers that appear at least three times consecutively in the `Logs` table. This can be achieved by comparing the `num` values of consecutive rows using `id`.

#### SQL Solution:

```sql
SELECT DISTINCT l1.num AS ConsecutiveNums
FROM Logs l1
JOIN Logs l2 ON l1.id = l2.id - 1
JOIN Logs l3 ON l1.id = l3.id - 2
WHERE l1.num = l2.num AND l2.num = l3.num;
```

---

#### Explanation:

1. **Table Alias**:
   - We use the same `Logs` table multiple times by assigning aliases (`l1`, `l2`, `l3`) to compare consecutive rows.

2. **Join Conditions**:
   - `l1.id = l2.id - 1`: Ensures `l1` and `l2` are consecutive rows.
   - `l1.id = l3.id - 2`: Ensures `l1`, `l2`, and `l3` are three consecutive rows.

3. **Filter with `WHERE` Clause**:
   - `l1.num = l2.num AND l2.num = l3.num`: Checks that the `num` value is the same for all three consecutive rows.

4. **DISTINCT**:
   - Ensures the result only includes unique numbers that meet the condition.

---

#### Example Walkthrough:

##### Input:

| id | num |
|----|-----|
| 1  | 1   |
| 2  | 1   |
| 3  | 1   |
| 4  | 2   |
| 5  | 1   |
| 6  | 2   |
| 7  | 2   |

##### Execution:

- Row `(1, 1)` joins with `(2, 1)` and `(3, 1)`:
  - `num` values are equal (`1, 1, 1`), so `1` is added to the result.

- Row `(2, 1)` joins with `(3, 1)` and `(4, 2)`:
  - `num` values are not equal (`1, 1, 2`), so no result.

- Other combinations do not meet the conditions for three consecutive rows.

##### Output:

| ConsecutiveNums |
|-----------------|
| 1               |

---

This query efficiently finds numbers that appear three times consecutively in the table.

---

