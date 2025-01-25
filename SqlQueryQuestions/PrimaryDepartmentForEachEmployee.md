# [Primary Department for Each Employee](#primary-department-for-each-employee)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Employee (employee_id int, department_id int, primary_flag ENUM('Y','N'))
Truncate table Employee
insert into Employee (employee_id, department_id, primary_flag) values ('1', '1', 'N')
insert into Employee (employee_id, department_id, primary_flag) values ('2', '1', 'Y')
insert into Employee (employee_id, department_id, primary_flag) values ('2', '2', 'N')
insert into Employee (employee_id, department_id, primary_flag) values ('3', '3', 'N')
insert into Employee (employee_id, department_id, primary_flag) values ('4', '2', 'N')
insert into Employee (employee_id, department_id, primary_flag) values ('4', '3', 'Y')
insert into Employee (employee_id, department_id, primary_flag) values ('4', '4', 'N')
```

---

### [Question](#question)


Table: Employee
```
+---------------+---------+
| Column Name   |  Type   |
+---------------+---------+
| employee_id   | int     |
| department_id | int     |
| primary_flag  | varchar |
+---------------+---------+
```
- **(employee_id, department_id)** is the primary key (combination of columns with unique values) for this table.
- **employee_id** is the id of the employee.
- **department_id** is the id of the department to which the employee belongs.
- **primary_flag** is an ENUM (category) of type ('Y', 'N'). If the flag is 'Y', the department is the primary department for the employee. If the flag is 'N', the department is not the primary.
 

Employees can belong to multiple departments. When the employee joins other departments, they need to decide which department is their primary department. 
Note that when an employee belongs to only one department, their primary column is 'N'.

Write a solution to report all the employees with their primary department. For employees who belong to one department, report their only department.

Return the result table in any order.

The result format is in the following example.

 

#### Example 1:

Input: 

Employee table:
```
+-------------+---------------+--------------+
| employee_id | department_id | primary_flag |
+-------------+---------------+--------------+
| 1           | 1             | N            |
| 2           | 1             | Y            |
| 2           | 2             | N            |
| 3           | 3             | N            |
| 4           | 2             | N            |
| 4           | 3             | Y            |
| 4           | 4             | N            |
+-------------+---------------+--------------+
```
Output: 
```
+-------------+---------------+
| employee_id | department_id |
+-------------+---------------+
| 1           | 1             |
| 2           | 1             |
| 3           | 3             |
| 4           | 3             |
+-------------+---------------+
```
Explanation: 
- The Primary department for employee 1 is 1.
- The Primary department for employee 2 is 1.
- The Primary department for employee 3 is 3.
- The Primary department for employee 4 is 3.

---

### [Solution](#solution)


#### Part 1: Employees with `primary_flag = 'Y'`
```sql
SELECT 
  employee_id, 
  department_id 
FROM 
  Employee 
WHERE 
  primary_flag = 'Y'
```
- This part retrieves all employees who have a primary department explicitly marked with `primary_flag = 'Y'`.
- These are the cases where an employee belongs to multiple departments, and one of them is designated as their primary department.

---

#### Part 2: Employees in exactly one department
```sql
SELECT 
  employee_id, 
  department_id 
FROM 
  Employee 
GROUP BY 
  employee_id 
HAVING 
  COUNT(employee_id) = 1
```
- **GROUP BY employee_id**: Groups the rows in the `Employee` table by `employee_id`.
- **COUNT(employee_id) = 1**: This filters for employees who appear in exactly one department in the table.
  - Such employees don't have a `primary_flag = 'Y'` because they belong to only one department, and the `primary_flag` is set to `'N'` by default.
  - For these employees, their only department is treated as their "primary" department.

---

#### Combining Results with `UNION`
```sql
SELECT 
  employee_id, 
  department_id 
FROM 
  Employee 
WHERE 
  primary_flag = 'Y' 
UNION 
SELECT 
  employee_id, 
  department_id 
FROM 
  Employee 
GROUP BY 
  employee_id 
HAVING 
  COUNT(employee_id) = 1;
```
- **First Query**: Adds all employees with a designated primary department (`primary_flag = 'Y'`).
- **Second Query**: Adds employees who are in exactly one department (their only department is treated as primary).
- **UNION**: Combines the results of both queries, removing duplicates (though none should exist in this scenario).

---

#### Why This Solution Works
This query ensures:
1. Employees who explicitly have a primary department are included.
2. Employees who only belong to one department are also included as their only department is de facto their primary department.

---

#### Example Input:
| employee_id | department_id | primary_flag |
|-------------|---------------|--------------|
| 1           | 1             | N            |
| 2           | 1             | Y            |
| 2           | 2             | N            |
| 3           | 3             | N            |
| 4           | 2             | N            |
| 4           | 3             | Y            |
| 4           | 4             | N            |

---

#### Result:
| employee_id | department_id |
|-------------|---------------|
| 1           | 1             |
| 2           | 1             |
| 3           | 3             |
| 4           | 3             |

- **Employee 1**: Only belongs to department 1 (`primary_flag = N`, but they only belong to one department).
- **Employee 2**: Belongs to departments 1 and 2, with department 1 marked as primary.
- **Employee 3**: Only belongs to department 3 (`primary_flag = N`, but they only belong to one department).
- **Employee 4**: Belongs to departments 2, 3, and 4, with department 3 marked as primary.

---
