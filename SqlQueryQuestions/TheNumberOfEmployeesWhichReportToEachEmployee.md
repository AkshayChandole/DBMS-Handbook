# [The Number of Employees Which Report to Each Employee](#the-number-of-employees-which-report-to-each-employee)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Employees(employee_id int, name varchar(20), reports_to int, age int)
Truncate table Employees
insert into Employees (employee_id, name, reports_to, age) values ('9', 'Hercy', NULL, '43')
insert into Employees (employee_id, name, reports_to, age) values ('6', 'Alice', '9', '41')
insert into Employees (employee_id, name, reports_to, age) values ('4', 'Bob', '9', '36')
insert into Employees (employee_id, name, reports_to, age) values ('2', 'Winston', NULL, '37')
```

---

### [Question](#question)

Table: Employees
```
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| employee_id | int      |
| name        | varchar  |
| reports_to  | int      |
| age         | int      |
+-------------+----------+
```
- **employee_id** is the column with unique values for this table.
- This table contains information about the employees and the id of the manager they report to. Some employees do not report to anyone (reports_to is null). 
 

For this problem, we will consider a manager an employee who has at least 1 other employee reporting to them.

Write a solution to report the ids and the names of all managers, the number of employees who report directly to them, and the average age of the reports rounded to the nearest integer.

Return the result table ordered by employee_id.

The result format is in the following example.

 

#### Example 1:

Input: 

Employees table:
```
+-------------+---------+------------+-----+
| employee_id | name    | reports_to | age |
+-------------+---------+------------+-----+
| 9           | Hercy   | null       | 43  |
| 6           | Alice   | 9          | 41  |
| 4           | Bob     | 9          | 36  |
| 2           | Winston | null       | 37  |
+-------------+---------+------------+-----+
```
Output: 
```
+-------------+-------+---------------+-------------+
| employee_id | name  | reports_count | average_age |
+-------------+-------+---------------+-------------+
| 9           | Hercy | 2             | 39          |
+-------------+-------+---------------+-------------+
```
Explanation: Hercy has 2 people report directly to him, Alice and Bob. Their average age is (41+36)/2 = 38.5, which is 39 after rounding it to the nearest integer.
Example 2:

Input: 
Employees table:
+-------------+---------+------------+-----+ 
| employee_id | name    | reports_to | age |
|-------------|---------|------------|-----|
| 1           | Michael | null       | 45  |
| 2           | Alice   | 1          | 38  |
| 3           | Bob     | 1          | 42  |
| 4           | Charlie | 2          | 34  |
| 5           | David   | 2          | 40  |
| 6           | Eve     | 3          | 37  |
| 7           | Frank   | null       | 50  |
| 8           | Grace   | null       | 48  |
+-------------+---------+------------+-----+ 
Output: 
+-------------+---------+---------------+-------------+
| employee_id | name    | reports_count | average_age |
| ----------- | ------- | ------------- | ----------- |
| 1           | Michael | 2             | 40          |
| 2           | Alice   | 2             | 37          |
| 3           | Bob     | 1             | 37          |
+-------------+---------+---------------+-------------+

---

### [Solution](#solution)

#### SQL Solution

```sql
SELECT 
    e.employee_id,
    e.name,
    COUNT(r.employee_id) AS reports_count,
    ROUND(AVG(r.age), 0) AS average_age
FROM 
    Employees e
LEFT JOIN 
    Employees r
ON 
    e.employee_id = r.reports_to
WHERE 
    r.employee_id IS NOT NULL
GROUP BY 
    e.employee_id, e.name
ORDER BY 
    e.employee_id;
```

---

#### Explanation

1. **Join Employees Table with Itself**:
   - `LEFT JOIN Employees r ON e.employee_id = r.reports_to`:
     - This matches managers (`e`) with their direct reports (`r`).

2. **Exclude Non-Managers**:
   - `WHERE r.employee_id IS NOT NULL`:
     - Filters out employees without any direct reports.

3. **Count Direct Reports**:
   - `COUNT(r.employee_id) AS reports_count`:
     - Counts the number of employees directly reporting to each manager.

4. **Calculate Average Age**:
   - `AVG(r.age)` calculates the average age of reports.
   - `ROUND(AVG(r.age), 0)` rounds the average age to the nearest integer.

5. **Group by Manager**:
   - `GROUP BY e.employee_id, e.name`:
     - Groups the results by manager to calculate metrics.

6. **Order by Employee ID**:
   - `ORDER BY e.employee_id` ensures the output is sorted by `employee_id`.

---

#### Input Example

**Employees Table**:
```
+-------------+---------+------------+-----+
| employee_id | name    | reports_to | age |
+-------------+---------+------------+-----+
| 9           | Hercy   | null       | 43  |
| 6           | Alice   | 9          | 41  |
| 4           | Bob     | 9          | 36  |
| 2           | Winston | null       | 37  |
+-------------+---------+------------+-----+
```

---

#### Query Output

**Result**:
```
+-------------+-------+---------------+-------------+
| employee_id | name  | reports_count | average_age |
+-------------+-------+---------------+-------------+
| 9           | Hercy | 2             | 39          |
+-------------+-------+---------------+-------------+
```

---

#### Explanation of Output

1. **Hercy**:
   - Direct reports: Alice and Bob.
   - `reports_count`: 2.
   - `average_age`: `(41 + 36) / 2 = 38.5` → Rounded to `39`.


---
