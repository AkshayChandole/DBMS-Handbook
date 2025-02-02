# [Employees Whose Manager Left the Company](#employees-whose-manager-left-the-company)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Employees (employee_id int, name varchar(20), manager_id int, salary int)
Truncate table Employees
insert into Employees (employee_id, name, manager_id, salary) values ('3', 'Mila', '9', '60301')
insert into Employees (employee_id, name, manager_id, salary) values ('12', 'Antonella', NULL, '31000')
insert into Employees (employee_id, name, manager_id, salary) values ('13', 'Emery', NULL, '67084')
insert into Employees (employee_id, name, manager_id, salary) values ('1', 'Kalel', '11', '21241')
insert into Employees (employee_id, name, manager_id, salary) values ('9', 'Mikaela', NULL, '50937')
insert into Employees (employee_id, name, manager_id, salary) values ('11', 'Joziah', '6', '28485')
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
| manager_id  | int      |
| salary      | int      |
+-------------+----------+
```
- In SQL, employee_id is the primary key for this table.
- This table contains information about the employees, their salary, and the ID of their manager. Some employees do not have a manager (manager_id is null). 
 

Find the IDs of the employees whose salary is strictly less than $30000 and whose manager left the company. When a manager leaves the company, their information is deleted from the Employees table, but the reports still have their manager_id set to the manager that left.

Return the result table ordered by employee_id.

The result format is in the following example.

 

#### Example 1:

Input: 

Employees table:
```
+-------------+-----------+------------+--------+
| employee_id | name      | manager_id | salary |
+-------------+-----------+------------+--------+
| 3           | Mila      | 9          | 60301  |
| 12          | Antonella | null       | 31000  |
| 13          | Emery     | null       | 67084  |
| 1           | Kalel     | 11         | 21241  |
| 9           | Mikaela   | null       | 50937  |
| 11          | Joziah    | 6          | 28485  |
+-------------+-----------+------------+--------+
```
Output: 
```
+-------------+
| employee_id |
+-------------+
| 11          |
+-------------+
```

Explanation: 
- The employees with a salary less than $30000 are 1 (Kalel) and 11 (Joziah).
- Kalel's manager is employee 11, who is still in the company (Joziah).
- Joziah's manager is employee 6, who left the company because there is no row for employee 6 as it was deleted.

---

### [Solution](#solution)
```sql
SELECT employee_id
FROM Employees
WHERE salary<30000
AND manager_id NOT IN (
    select employee_id
    from Employees
)
ORDER BY employee_id;
```

### **Solution Explanation**
```sql
SELECT employee_id
FROM Employees
WHERE salary < 30000
AND manager_id NOT IN (
    SELECT employee_id
    FROM Employees
)
ORDER BY employee_id;
```

---

#### **Breakdown of the Query**
1. **Filter Employees with Salary Less than $30,000:**
   - `WHERE salary < 30000` selects only employees whose salary is strictly **less than 30,000**.

2. **Find Employees Whose Manager Left the Company:**
   - `AND manager_id NOT IN (SELECT employee_id FROM Employees)`
   - The subquery retrieves all **existing employee IDs** (i.e., those still in the company).
   - `manager_id NOT IN (...)` ensures that we filter employees whose **manager is not found** in the current list of employees.
   - If a manager's ID is not present in the `Employees` table, it means they have **left the company**.

3. **Sort the Result by Employee ID:**
   - `ORDER BY employee_id` ensures the output is sorted in **ascending order**.

---

#### **Example Execution**
##### **Input Table:**
| employee_id | name      | manager_id | salary |
|------------|-----------|------------|--------|
| 3          | Mila      | 9          | 60301  |
| 12         | Antonella | NULL       | 31000  |
| 13         | Emery     | NULL       | 67084  |
| 1          | Kalel     | 11         | 21241  |
| 9          | Mikaela   | NULL       | 50937  |
| 11         | Joziah    | 6          | 28485  |

---

##### **Step-by-Step Execution**
1. **Find Employees with Salary < $30,000:**
   - `Kalel (1) - $21,241`
   - `Joziah (11) - $28,485`

2. **Find Managers Who Have Left (manager_id NOT IN existing employee_id):**
   - The `Employees` table contains `{3, 12, 13, 1, 9, 11}`.
   - `Kalel (1)` reports to `Joziah (11)`, and `Joziah (11)` **exists** in the table → **Not included**.
   - `Joziah (11)` reports to `Manager 6`, but `6` is **not in the table** → **Joziah (11) is included in the output**.

---

##### **Final Output:**
```
+-------------+
| employee_id |
+-------------+
| 11          |
+-------------+
```

✅ **This query efficiently finds employees whose managers have left while ensuring salary constraints are met.** 🚀

---
