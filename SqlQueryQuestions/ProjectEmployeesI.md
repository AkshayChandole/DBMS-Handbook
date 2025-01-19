# [Project Employees I](#project-employees-i)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Project (project_id int, employee_id int)
Create table If Not Exists Employee (employee_id int, name varchar(10), experience_years int)
Truncate table Project
insert into Project (project_id, employee_id) values ('1', '1')
insert into Project (project_id, employee_id) values ('1', '2')
insert into Project (project_id, employee_id) values ('1', '3')
insert into Project (project_id, employee_id) values ('2', '1')
insert into Project (project_id, employee_id) values ('2', '4')
Truncate table Employee
insert into Employee (employee_id, name, experience_years) values ('1', 'Khaled', '3')
insert into Employee (employee_id, name, experience_years) values ('2', 'Ali', '2')
insert into Employee (employee_id, name, experience_years) values ('3', 'John', '1')
insert into Employee (employee_id, name, experience_years) values ('4', 'Doe', '2')
```

---

### [Question](#question)

Table: Project
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| project_id  | int     |
| employee_id | int     |
+-------------+---------+
```
- **(project_id, employee_id)** is the primary key of this table.
- **employee_id** is a foreign key to Employee table.
- Each row of this table indicates that the employee with employee_id is working on the project with project_id.
 

Table: Employee
```
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| employee_id      | int     |
| name             | varchar |
| experience_years | int     |
+------------------+---------+
```
- **employee_id** is the primary key of this table. It's guaranteed that experience_years is not NULL.
- Each row of this table contains information about one employee.
 

Write an SQL query that reports the average experience years of all the employees for each project, rounded to 2 digits.

Return the result table in any order.

The query result format is in the following example.

 

#### Example 1:

Input: 

Project table:
```
+-------------+-------------+
| project_id  | employee_id |
+-------------+-------------+
| 1           | 1           |
| 1           | 2           |
| 1           | 3           |
| 2           | 1           |
| 2           | 4           |
+-------------+-------------+
```
Employee table:
```
+-------------+--------+------------------+
| employee_id | name   | experience_years |
+-------------+--------+------------------+
| 1           | Khaled | 3                |
| 2           | Ali    | 2                |
| 3           | John   | 1                |
| 4           | Doe    | 2                |
+-------------+--------+------------------+
```
Output: 
```
+-------------+---------------+
| project_id  | average_years |
+-------------+---------------+
| 1           | 2.00          |
| 2           | 2.50          |
+-------------+---------------+
```
Explanation: The average experience years for the first project is (3 + 2 + 1) / 3 = 2.00 and for the second project is (3 + 2) / 2 = 2.50

---

### [Solution](#solution)

### Solution:

```sql
SELECT 
    p.project_id,
    ROUND(AVG(e.experience_years), 2) AS average_years
FROM 
    Project p
JOIN 
    Employee e
ON 
    p.employee_id = e.employee_id
GROUP BY 
    p.project_id;
```

---

#### Explanation:
1. **Join the `Project` and `Employee` tables**:
   - Use an `INNER JOIN` to link the `employee_id` column in both tables.

2. **Calculate the average experience years**:
   - Use the `AVG()` function on the `experience_years` column from the `Employee` table.

3. **Round the result**:
   - Use the `ROUND()` function to round the average to 2 decimal places.

4. **Group by `project_id`**:
   - To calculate the average experience for each project, group the data by `project_id`.

---

#### Output for the Example Input:
For the given input:
- Project `1` includes employees with experience `3`, `2`, and `1`. Average = `(3 + 2 + 1) / 3 = 2.00`.
- Project `2` includes employees with experience `3` and `2`. Average = `(3 + 2) / 2 = 2.50`.

Output:
```
+-------------+---------------+
| project_id  | average_years |
+-------------+---------------+
| 1           | 2.00          |
| 2           | 2.50          |
+-------------+---------------+
```

---
