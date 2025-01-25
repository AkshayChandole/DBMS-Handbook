# [Classes More Than 5 Students](#classes-more-than-5-students)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Courses (student varchar(255), class varchar(255))
Truncate table Courses
insert into Courses (student, class) values ('A', 'Math')
insert into Courses (student, class) values ('B', 'English')
insert into Courses (student, class) values ('C', 'Math')
insert into Courses (student, class) values ('D', 'Biology')
insert into Courses (student, class) values ('E', 'Math')
insert into Courses (student, class) values ('F', 'Computer')
insert into Courses (student, class) values ('G', 'Math')
insert into Courses (student, class) values ('H', 'Math')
insert into Courses (student, class) values ('I', 'Math')
```

---

### [Question](#question)

Table: Courses
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| student     | varchar |
| class       | varchar |
+-------------+---------+
```
- **(student, class)** is the primary key (combination of columns with unique values) for this table.
- Each row of this table indicates the name of a student and the class in which they are enrolled.
 

Write a solution to find all the classes that have at least five students.

Return the result table in any order.

The result format is in the following example.

 

#### Example 1:

Input: 

Courses table:
```
+---------+----------+
| student | class    |
+---------+----------+
| A       | Math     |
| B       | English  |
| C       | Math     |
| D       | Biology  |
| E       | Math     |
| F       | Computer |
| G       | Math     |
| H       | Math     |
| I       | Math     |
+---------+----------+
```
Output: 
```
+---------+
| class   |
+---------+
| Math    |
+---------+
```
Explanation: 
- Math has 6 students, so we include it.
- English has 1 student, so we do not include it.
- Biology has 1 student, so we do not include it.
- Computer has 1 student, so we do not include it.

---

### [Solution](#solution)

Here’s the solution to find all classes that have at least five students:

---

#### SQL Solution
```sql
SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(student) >= 5;
```

---

#### Explanation
1. **Group By**:  
   - The `GROUP BY class` clause groups all rows in the `Courses` table by the `class` column.

2. **Count**:  
   - The `COUNT(student)` function counts the number of students in each class.

3. **Having Clause**:  
   - The `HAVING COUNT(student) >= 5` filters the groups to only include those with at least 5 students.

---

#### Output for the Given Input
**Courses Table**:
```
+---------+----------+
| student | class    |
+---------+----------+
| A       | Math     |
| B       | English  |
| C       | Math     |
| D       | Biology  |
| E       | Math     |
| F       | Computer |
| G       | Math     |
| H       | Math     |
| I       | Math     |
+---------+----------+
```

**Query Result**:
```
+---------+
| class   |
+---------+
| Math    |
+---------+
```

In this example, only `Math` has 6 students, which is greater than or equal to 5, so it's the only class included in the result.

---
