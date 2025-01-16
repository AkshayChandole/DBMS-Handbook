# [Replace Employee ID With The Unique Identifier](#replace-employee-id-with-the-unique-identifier)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Employees (id int, name varchar(20))
Create table If Not Exists EmployeeUNI (id int, unique_id int)
Truncate table Employees
insert into Employees (id, name) values ('1', 'Alice')
insert into Employees (id, name) values ('7', 'Bob')
insert into Employees (id, name) values ('11', 'Meir')
insert into Employees (id, name) values ('90', 'Winston')
insert into Employees (id, name) values ('3', 'Jonathan')
Truncate table EmployeeUNI
insert into EmployeeUNI (id, unique_id) values ('3', '1')
insert into EmployeeUNI (id, unique_id) values ('11', '2')
insert into EmployeeUNI (id, unique_id) values ('90', '3')
```

---

### [Question](#question)

Table: Employees
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
```
- **id** is the primary key (column with unique values) for this table.
- Each row of this table contains the id and the name of an employee in a company.
 

Table: EmployeeUNI

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+
```
- **(id, unique_id)** is the primary key (combination of columns with unique values) for this table.
- Each row of this table contains the id and the corresponding unique id of an employee in the company.
 

Write a solution to show the unique ID of each user, If a user does not have a unique ID replace just show null.

Return the result table in any order.

The result format is in the following example.

 
#### Example 1:

Input: 

Employees table:
```
+----+----------+
| id | name     |
+----+----------+
| 1  | Alice    |
| 7  | Bob      |
| 11 | Meir     |
| 90 | Winston  |
| 3  | Jonathan |
+----+----------+
```

EmployeeUNI table:
```
+----+-----------+
| id | unique_id |
+----+-----------+
| 3  | 1         |
| 11 | 2         |
| 90 | 3         |
+----+-----------+
```
Output: 
```
+-----------+----------+
| unique_id | name     |
+-----------+----------+
| null      | Alice    |
| null      | Bob      |
| 2         | Meir     |
| 3         | Winston  |
| 1         | Jonathan |
+-----------+----------+
```
Explanation: 
Alice and Bob do not have a unique ID, We will show null instead.
The unique ID of Meir is 2.
The unique ID of Winston is 3.
The unique ID of Jonathan is 1.

---

### [Solution](#solution)

Here is the SQL solution for the **Replace Employee ID With The Unique Identifier** problem:

```sql
SELECT 
    e.name, 
    eu.unique_id
FROM 
    Employees e
LEFT JOIN 
    EmployeeUNI eu
ON 
    e.id = eu.id;
```

---

#### Explanation:
1. **`SELECT e.name, eu.unique_id`**:
   - Retrieves the `name` of the employee from the `Employees` table and the `unique_id` from the `EmployeeUNI` table.

2. **`FROM Employees e`**:
   - Specifies the `Employees` table as the main data source.

3. **`LEFT JOIN EmployeeUNI eu`**:
   - Performs a left join to include all rows from the `Employees` table, and only matching rows from the `EmployeeUNI` table. 
   - If there is no match, `unique_id` will be `NULL`.

4. **`ON e.id = eu.id`**:
   - Joins the two tables on the `id` column, which is common in both tables.

---

#### Example Walkthrough:

**Input**:

**Employees**:
```
+----+----------+
| id | name     |
+----+----------+
| 1  | Alice    |
| 7  | Bob      |
| 11 | Meir     |
| 90 | Winston  |
| 3  | Jonathan |
+----+----------+
```

**EmployeeUNI**:
```
+----+-----------+
| id | unique_id |
+----+-----------+
| 3  | 1         |
| 11 | 2         |
| 90 | 3         |
+----+-----------+
```

**Steps**:
1. Perform a left join on `id`:
   ```
   +----+----------+-----------+
   | id | name     | unique_id |
   +----+----------+-----------+
   | 1  | Alice    | NULL      |
   | 7  | Bob      | NULL      |
   | 11 | Meir     | 2         |
   | 90 | Winston  | 3         |
   | 3  | Jonathan | 1         |
   +----+----------+-----------+
   ```

2. Select only `name` and `unique_id`:
   ```
   +-----------+----------+
   | unique_id | name     |
   +-----------+----------+
   | NULL      | Alice    |
   | NULL      | Bob      |
   | 2         | Meir     |
   | 3         | Winston  |
   | 1         | Jonathan |
   +-----------+----------+
   ```

**Output**:

```
+-----------+----------+
| unique_id | name     |
+-----------+----------+
| null      | Alice    |
| null      | Bob      |
| 2         | Meir     |
| 3         | Winston  |
| 1         | Jonathan |
+-----------+----------+
```

---

