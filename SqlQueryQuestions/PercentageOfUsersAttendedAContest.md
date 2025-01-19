# [Percentage of Users Attended a Contest](#percentage-of-users-attended-a-contest)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Users (user_id int, user_name varchar(20))
Create table If Not Exists Register (contest_id int, user_id int)
Truncate table Users
insert into Users (user_id, user_name) values ('6', 'Alice')
insert into Users (user_id, user_name) values ('2', 'Bob')
insert into Users (user_id, user_name) values ('7', 'Alex')
Truncate table Register
insert into Register (contest_id, user_id) values ('215', '6')
insert into Register (contest_id, user_id) values ('209', '2')
insert into Register (contest_id, user_id) values ('208', '2')
insert into Register (contest_id, user_id) values ('210', '6')
insert into Register (contest_id, user_id) values ('208', '6')
insert into Register (contest_id, user_id) values ('209', '7')
insert into Register (contest_id, user_id) values ('209', '6')
insert into Register (contest_id, user_id) values ('215', '7')
insert into Register (contest_id, user_id) values ('208', '7')
insert into Register (contest_id, user_id) values ('210', '2')
insert into Register (contest_id, user_id) values ('207', '2')
insert into Register (contest_id, user_id) values ('210', '7')
```

---

### [Question](#question)

Table: Users
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| user_name   | varchar |
+-------------+---------+
```
- **user_id** is the primary key (column with unique values) for this table.
- Each row of this table contains the name and the id of a user.
 

Table: Register
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| contest_id  | int     |
| user_id     | int     |
+-------------+---------+
```
- **(contest_id, user_id)** is the primary key (combination of columns with unique values) for this table.
- Each row of this table contains the id of a user and the contest they registered into.
 

Write a solution to find the percentage of the users registered in each contest rounded to two decimals.

Return the result table ordered by percentage in descending order. In case of a tie, order it by contest_id in ascending order.

The result format is in the following example.

 

#### Example 1:

Input: 

Users table:
```
+---------+-----------+
| user_id | user_name |
+---------+-----------+
| 6       | Alice     |
| 2       | Bob       |
| 7       | Alex      |
+---------+-----------+
```
Register table:
```
+------------+---------+
| contest_id | user_id |
+------------+---------+
| 215        | 6       |
| 209        | 2       |
| 208        | 2       |
| 210        | 6       |
| 208        | 6       |
| 209        | 7       |
| 209        | 6       |
| 215        | 7       |
| 208        | 7       |
| 210        | 2       |
| 207        | 2       |
| 210        | 7       |
+------------+---------+
```
Output: 
```
+------------+------------+
| contest_id | percentage |
+------------+------------+
| 208        | 100.0      |
| 209        | 100.0      |
| 210        | 100.0      |
| 215        | 66.67      |
| 207        | 33.33      |
+------------+------------+
```
Explanation: 
- All the users registered in contests 208, 209, and 210. The percentage is 100% and we sort them in the answer table by contest_id in ascending order.
- Alice and Alex registered in contest 215 and the percentage is ((2/3) * 100) = 66.67%
- Bob registered in contest 207 and the percentage is ((1/3) * 100) = 33.33%

---

### [Solution](#solution)

### Solution:

```sql
SELECT 
    r.contest_id,
    ROUND(COUNT(DISTINCT r.user_id) * 100.0 / (SELECT COUNT(*) FROM Users), 2) AS percentage
FROM 
    Register r
GROUP BY 
    r.contest_id
ORDER BY 
    percentage DESC, 
    r.contest_id ASC;
```

---

#### Explanation:

1. **Calculate the Total Number of Users**:
   - The subquery `(SELECT COUNT(*) FROM Users)` gets the total number of users in the `Users` table. This is the denominator for the percentage calculation.

2. **Count the Distinct Users Registered for Each Contest**:
   - `COUNT(DISTINCT r.user_id)` ensures that duplicate entries for the same user in a contest are not counted multiple times.

3. **Calculate the Percentage**:
   - `COUNT(DISTINCT r.user_id) * 100.0 / (SELECT COUNT(*) FROM Users)` calculates the percentage of users who registered for each contest.

4. **Round to Two Decimal Places**:
   - The `ROUND(..., 2)` function ensures the percentage is rounded to two decimal places.

5. **Group by `contest_id`**:
   - Grouping by `contest_id` aggregates the calculations for each contest.

6. **Order the Results**:
   - Results are ordered first by `percentage` in descending order, and then by `contest_id` in ascending order in case of a tie.

---

#### Output for the Example Input:

For the given input:
- Contest `208`: All 3 users registered (percentage = `100.0`).
- Contest `209`: All 3 users registered (percentage = `100.0`).
- Contest `210`: All 3 users registered (percentage = `100.0`).
- Contest `215`: 2 users registered (percentage = `(2/3) * 100 = 66.67`).
- Contest `207`: 1 user registered (percentage = `(1/3) * 100 = 33.33`).

Output:
```
+------------+------------+
| contest_id | percentage |
+------------+------------+
| 208        | 100.0      |
| 209        | 100.0      |
| 210        | 100.0      |
| 215        | 66.67      |
| 207        | 33.33      |
+------------+------------+
```

---

