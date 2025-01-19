# [Number of Unique Subjects Taught by Each Teacher](#number-of-unique-subjects-taught-by-each-teacher)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Teacher (teacher_id int, subject_id int, dept_id int)
Truncate table Teacher
insert into Teacher (teacher_id, subject_id, dept_id) values ('1', '2', '3')
insert into Teacher (teacher_id, subject_id, dept_id) values ('1', '2', '4')
insert into Teacher (teacher_id, subject_id, dept_id) values ('1', '3', '3')
insert into Teacher (teacher_id, subject_id, dept_id) values ('2', '1', '1')
insert into Teacher (teacher_id, subject_id, dept_id) values ('2', '2', '1')
insert into Teacher (teacher_id, subject_id, dept_id) values ('2', '3', '1')
insert into Teacher (teacher_id, subject_id, dept_id) values ('2', '4', '1')
```

---

### [Question](#question)

Table: Teacher
```
+-------------+------+
| Column Name | Type |
+-------------+------+
| teacher_id  | int  |
| subject_id  | int  |
| dept_id     | int  |
+-------------+------+
```
- **(subject_id, dept_id)** is the primary key (combinations of columns with unique values) of this table.
- Each row in this table indicates that the teacher with teacher_id teaches the subject subject_id in the department dept_id.
 

Write a solution to calculate the number of unique subjects each teacher teaches in the university.

Return the result table in any order.

The result format is shown in the following example.

 

#### Example 1:

Input: 

Teacher table:
```
+------------+------------+---------+
| teacher_id | subject_id | dept_id |
+------------+------------+---------+
| 1          | 2          | 3       |
| 1          | 2          | 4       |
| 1          | 3          | 3       |
| 2          | 1          | 1       |
| 2          | 2          | 1       |
| 2          | 3          | 1       |
| 2          | 4          | 1       |
+------------+------------+---------+
```
Output:  
```
+------------+-----+
| teacher_id | cnt |
+------------+-----+
| 1          | 2   |
| 2          | 4   |
+------------+-----+
```
Explanation: 
Teacher 1:
  - They teach subject 2 in departments 3 and 4.
  - They teach subject 3 in department 3.
Teacher 2:
  - They teach subject 1 in department 1.
  - They teach subject 2 in department 1.
  - They teach subject 3 in department 1.
  - They teach subject 4 in department 1.

---

### [Solution](#solution)

Here’s the SQL solution for the problem:

```sql
SELECT 
    teacher_id, 
    COUNT(DISTINCT subject_id) AS cnt
FROM 
    Teacher
GROUP BY 
    teacher_id;
```

---

#### Explanation:

1. **DISTINCT Subject IDs**:
   - Use `DISTINCT subject_id` to count unique subjects taught by each teacher. This ensures that duplicate subject entries in different departments are counted only once.

2. **GROUP BY**:
   - Group the data by `teacher_id` to calculate the count for each teacher separately.

3. **COUNT**:
   - Calculate the number of unique subjects (`cnt`) for each `teacher_id`.

---

#### Output:

For the given data, the output is:

```
+------------+-----+
| teacher_id | cnt |
+------------+-----+
| 1          | 2   |
| 2          | 4   |
+------------+-----+
```

This matches the expected results:
- Teacher 1 teaches 2 unique subjects.
- Teacher 2 teaches 4 unique subjects.

---
