# [Employee Bonus](#employee-bonus)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Employee (empId int, name varchar(255), supervisor int, salary int)
Create table If Not Exists Bonus (empId int, bonus int)
Truncate table Employee
insert into Employee (empId, name, supervisor, salary) values ('3', 'Brad', NULL, '4000')
insert into Employee (empId, name, supervisor, salary) values ('1', 'John', '3', '1000')
insert into Employee (empId, name, supervisor, salary) values ('2', 'Dan', '3', '2000')
insert into Employee (empId, name, supervisor, salary) values ('4', 'Thomas', '3', '4000')
Truncate table Bonus
insert into Bonus (empId, bonus) values ('2', '500')
insert into Bonus (empId, bonus) values ('4', '2000')
```

---

### [Question](#question)

Table: Employee
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |
+-------------+---------+
```
- empId is the column with unique values for this table.
- Each row of this table indicates the name and the ID of an employee in addition to their salary and the id of their manager.
 

Table: Bonus
```
+-------------+------+
| Column Name | Type |
+-------------+------+
| empId       | int  |
| bonus       | int  |
+-------------+------+
```
- empId is the column of unique values for this table.
- empId is a foreign key (reference column) to empId from the Employee table.
- Each row of this table contains the id of an employee and their respective bonus.
 

Write a solution to report the name and bonus amount of each employee with a bonus less than 1000.

Return the result table in any order.

The result format is in the following example.

 

#### Example 1:

Input:

Employee table:
```
+-------+--------+------------+--------+
| empId | name   | supervisor | salary |
+-------+--------+------------+--------+
| 3     | Brad   | null       | 4000   |
| 1     | John   | 3          | 1000   |
| 2     | Dan    | 3          | 2000   |
| 4     | Thomas | 3          | 4000   |
+-------+--------+------------+--------+
```
Bonus table:
```
+-------+-------+
| empId | bonus |
+-------+-------+
| 2     | 500   |
| 4     | 2000  |
+-------+-------+
```
Output: 
```
+------+-------+
| name | bonus |
+------+-------+
| Brad | null  |
| John | null  |
| Dan  | 500   |
+------+-------+
```

---

### [Solution](#solution)

To solve this using `JOIN` clauses, we can join the `Employee` table with the `Bonus` table on `empId`. Then, we filter out employees with a bonus of 1000 or more or those not present in the `Bonus` table (by checking for `NULL` bonuses).

Here’s the SQL solution:

```sql
SELECT 
    e.name,
    COALESCE(b.bonus, NULL) AS bonus
FROM 
    Employee e
LEFT JOIN 
    Bonus b
ON 
    e.empId = b.empId
WHERE 
    b.bonus < 1000 OR b.bonus IS NULL;
```

#### Explanation:

1. **LEFT JOIN**:
   - We use a `LEFT JOIN` to include all employees from the `Employee` table, even if they don't have a corresponding entry in the `Bonus` table.

2. **COALESCE**:
   - The `COALESCE` function is used to handle cases where an employee doesn’t have a bonus, returning `NULL` instead.

3. **WHERE Clause**:
   - Filters employees with:
     - A bonus less than 1000 (`b.bonus < 1000`).
     - No entry in the `Bonus` table (`b.bonus IS NULL`).

#### Result:
For the given data, the output will be:

```
+------+-------+
| name | bonus |
+------+-------+
| Brad | NULL  |
| John | NULL  |
| Dan  | 500   |
+------+-------+
```

---
