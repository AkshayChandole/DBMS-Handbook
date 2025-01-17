# [Managers with at Least 5 Direct Reports](#managers-with-at-least-5-direct-reports)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Employee (id int, name varchar(255), department varchar(255), managerId int)
Truncate table Employee
insert into Employee (id, name, department, managerId) values ('101', 'John', 'A', NULL)
insert into Employee (id, name, department, managerId) values ('102', 'Dan', 'A', '101')
insert into Employee (id, name, department, managerId) values ('103', 'James', 'A', '101')
insert into Employee (id, name, department, managerId) values ('104', 'Amy', 'A', '101')
insert into Employee (id, name, department, managerId) values ('105', 'Anne', 'A', '101')
insert into Employee (id, name, department, managerId) values ('106', 'Ron', 'B', '101')
```

---

### [Question](#question)

Table: Employee
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |
+-------------+---------+
```
- **id** is the primary key (column with unique values) for this table.
- Each row of this table indicates the name of an employee, their department, and the id of their manager.
- If managerId is null, then the employee does not have a manager.
- No employee will be the manager of themself.
 

Write a solution to find managers with at least five direct reports.

Return the result table in any order.

The result format is in the following example.

 

#### Example 1:

Input: 

Employee table:
```
+-----+-------+------------+-----------+
| id  | name  | department | managerId |
+-----+-------+------------+-----------+
| 101 | John  | A          | null      |
| 102 | Dan   | A          | 101       |
| 103 | James | A          | 101       |
| 104 | Amy   | A          | 101       |
| 105 | Anne  | A          | 101       |
| 106 | Ron   | B          | 101       |
+-----+-------+------------+-----------+
```
Output: 
```
+------+
| name |
+------+
| John |
+------+
```
---

### [Solution](#solution)

Here’s how you can solve the problem using a `JOIN`:

```sql
SELECT 
    m.name AS name
FROM 
    Employee m
JOIN 
    Employee e
ON 
    m.id = e.managerId
GROUP BY 
    m.id, m.name
HAVING 
    COUNT(e.id) >= 5;
```

#### Explanation:
1. **`JOIN` Clause**:
   - The `JOIN` connects the `Employee` table to itself.
   - `m` represents the manager, and `e` represents the employees who report to that manager.
   - `m.id = e.managerId` ensures we are associating employees with their managers.

2. **`GROUP BY` Clause**:
   - We group the records by `m.id` and `m.name` to count the number of employees (`e.id`) reporting to each manager.

3. **`HAVING` Clause**:
   - Filters the results to include only those managers who have at

---
