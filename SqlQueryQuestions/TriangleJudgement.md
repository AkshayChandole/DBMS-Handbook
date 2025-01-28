# [Triangle Judgement](#triangle-judgement)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Triangle (x int, y int, z int)
Truncate table Triangle
insert into Triangle (x, y, z) values ('13', '15', '30')
insert into Triangle (x, y, z) values ('10', '20', '15')
```

---

### [Question](#question)

Table: Triangle
```
+-------------+------+
| Column Name | Type |
+-------------+------+
| x           | int  |
| y           | int  |
| z           | int  |
+-------------+------+
```
- In SQL, (x, y, z) is the primary key column for this table.
- Each row of this table contains the lengths of three line segments.
 

Report for every three line segments whether they can form a triangle.

Return the result table in any order.

The result format is in the following example.

 

#### Example 1:

Input:

Triangle table:
```
+----+----+----+
| x  | y  | z  |
+----+----+----+
| 13 | 15 | 30 |
| 10 | 20 | 15 |
+----+----+----+
```
Output: 
```
+----+----+----+----------+
| x  | y  | z  | triangle |
+----+----+----+----------+
| 13 | 15 | 30 | No       |
| 10 | 20 | 15 | Yes      |
+----+----+----+----------+
```
---

### [Solution](#solution)

To solve the problem, you need to determine whether three line segments can form a triangle. For three line segments `x`, `y`, and `z`, they can form a triangle if they satisfy the **triangle inequality theorem**:

1. \( x + y > z \)
2. \( x + z > y \)
3. \( y + z > x \)

If all three conditions are satisfied, then the segments form a triangle; otherwise, they do not.

#### SQL Solution:

```sql
SELECT 
    x, 
    y, 
    z, 
    CASE 
        WHEN x + y > z AND x + z > y AND y + z > x THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM 
    Triangle;
```

#### Explanation:
1. **SELECT Columns**:
   - `x`, `y`, `z`: The lengths of the three line segments from the `Triangle` table.

2. **CASE Statement**:
   - Checks the **triangle inequality theorem**:
     - \( x + y > z \)
     - \( x + z > y \)
     - \( y + z > x \)
   - If all conditions are true, the result is `'Yes'`.
   - Otherwise, the result is `'No'`.

3. **Output**:
   - Adds a new column `triangle` that specifies whether the given segments form a triangle (`Yes` or `No`).

---

#### Example Walkthrough:

##### Input:

| x  | y  | z  |
|----|----|----|
| 13 | 15 | 30 |
| 10 | 20 | 15 |

##### Execution:

**First Row (13, 15, 30)**:
- \( x + y = 13 + 15 = 28 \), which is **not greater than 30**. (Fails)
- \( x + z = 13 + 30 = 43 \), which is **greater than 15**. (Passes)
- \( y + z = 15 + 30 = 45 \), which is **greater than 13**. (Passes)
- Result: **`No`**

**Second Row (10, 20, 15)**:
- \( x + y = 10 + 20 = 30 \), which is **greater than 15**. (Passes)
- \( x + z = 10 + 15 = 25 \), which is **greater than 20**. (Passes)
- \( y + z = 20 + 15 = 35 \), which is **greater than 10**. (Passes)
- Result: **`Yes`**

##### Output:

| x  | y  | z  | triangle |
|----|----|----|----------|
| 13 | 15 | 30 | No       |
| 10 | 20 | 15 | Yes      |

This query correctly identifies whether the segments can form a triangle for each row.

---
