# [Confirmation Rate](#confirmation-rate)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Signups (user_id int, time_stamp datetime)
Create table If Not Exists Confirmations (user_id int, time_stamp datetime, action ENUM('confirmed','timeout'))
Truncate table Signups
insert into Signups (user_id, time_stamp) values ('3', '2020-03-21 10:16:13')
insert into Signups (user_id, time_stamp) values ('7', '2020-01-04 13:57:59')
insert into Signups (user_id, time_stamp) values ('2', '2020-07-29 23:09:44')
insert into Signups (user_id, time_stamp) values ('6', '2020-12-09 10:39:37')
Truncate table Confirmations
insert into Confirmations (user_id, time_stamp, action) values ('3', '2021-01-06 03:30:46', 'timeout')
insert into Confirmations (user_id, time_stamp, action) values ('3', '2021-07-14 14:00:00', 'timeout')
insert into Confirmations (user_id, time_stamp, action) values ('7', '2021-06-12 11:57:29', 'confirmed')
insert into Confirmations (user_id, time_stamp, action) values ('7', '2021-06-13 12:58:28', 'confirmed')
insert into Confirmations (user_id, time_stamp, action) values ('7', '2021-06-14 13:59:27', 'confirmed')
insert into Confirmations (user_id, time_stamp, action) values ('2', '2021-01-22 00:00:00', 'confirmed')
insert into Confirmations (user_id, time_stamp, action) values ('2', '2021-02-28 23:59:59', 'timeout')
```


---

### [Question](#question)

Table: Signups
```
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
+----------------+----------+
```
- **user_id** is the column of unique values for this table.
- Each row contains information about the signup time for the user with ID user_id.
 

Table: Confirmations
```
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
| action         | ENUM     |
+----------------+----------+
```
- **(user_id, time_stamp)** is the primary key (combination of columns with unique values) for this table.
- **user_id** is a foreign key (reference column) to the Signups table.
- action is an ENUM (category) of the type ('confirmed', 'timeout')
- Each row of this table indicates that the user with ID user_id requested a confirmation message at time_stamp and that confirmation message was either confirmed ('confirmed') or expired without confirming ('timeout').
 

The confirmation rate of a user is the number of 'confirmed' messages divided by the total number of requested confirmation messages. The confirmation rate of a user that did not request any confirmation messages is 0. Round the confirmation rate to two decimal places.

Write a solution to find the confirmation rate of each user.

Return the result table in any order.

The result format is in the following example.

 

#### Example 1:

Input: 

Signups table:
```
+---------+---------------------+
| user_id | time_stamp          |
+---------+---------------------+
| 3       | 2020-03-21 10:16:13 |
| 7       | 2020-01-04 13:57:59 |
| 2       | 2020-07-29 23:09:44 |
| 6       | 2020-12-09 10:39:37 |
+---------+---------------------+
```
Confirmations table:
```
+---------+---------------------+-----------+
| user_id | time_stamp          | action    |
+---------+---------------------+-----------+
| 3       | 2021-01-06 03:30:46 | timeout   |
| 3       | 2021-07-14 14:00:00 | timeout   |
| 7       | 2021-06-12 11:57:29 | confirmed |
| 7       | 2021-06-13 12:58:28 | confirmed |
| 7       | 2021-06-14 13:59:27 | confirmed |
| 2       | 2021-01-22 00:00:00 | confirmed |
| 2       | 2021-02-28 23:59:59 | timeout   |
+---------+---------------------+-----------+
```
Output: 
```
+---------+-------------------+
| user_id | confirmation_rate |
+---------+-------------------+
| 6       | 0.00              |
| 3       | 0.00              |
| 7       | 1.00              |
| 2       | 0.50              |
+---------+-------------------+
```
Explanation: 
- User 6 did not request any confirmation messages. The confirmation rate is 0.
- User 3 made 2 requests and both timed out. The confirmation rate is 0.
- User 7 made 3 requests and all were confirmed. The confirmation rate is 1.
- User 2 made 2 requests where one was confirmed and the other timed out. The confirmation rate is 1 / 2 = 0.5.

---

### [Solution](#solution)

Here’s the SQL solution to calculate the confirmation rate of each user:

```sql
SELECT 
    s.user_id,
    ROUND(
        IFNULL(SUM(CASE WHEN c.action = 'confirmed' THEN 1 ELSE 0 END) / COUNT(c.user_id), 0),
        2
    ) AS confirmation_rate
FROM 
    Signups s
LEFT JOIN 
    Confirmations c
ON 
    s.user_id = c.user_id
GROUP BY 
    s.user_id;
```

#### Explanation:

1. **`LEFT JOIN`**:
   - Joins the `Signups` table (`s`) with the `Confirmations` table (`c`) on the `user_id` column.
   - Ensures all users from the `Signups` table are included, even if they don't have any entries in the `Confirmations` table.

2. **`CASE` Statement**:
   - `CASE WHEN c.action = 'confirmed' THEN 1 ELSE 0 END` counts only the rows where the action is `confirmed`.

3. **`COUNT(c.user_id)`**:
   - Counts all confirmation requests (both `confirmed` and `timeout`) for each user.

4. **`SUM` and `IFNULL`**:
   - `SUM` calculates the total number of `confirmed` actions for a user.
   - `IFNULL` ensures that users without any confirmation requests have a rate of `0`.

5. **`ROUND`**:
   - Rounds the confirmation rate to 2 decimal places as required.

6. **`GROUP BY`**:
   - Groups the results by `s.user_id` to calculate rates per user.

#### Output for Example Input:

| user_id | confirmation_rate |
|---------|-------------------|
| 6       | 0.00              |
| 3       | 0.00              |
| 7       | 1.00              |
| 2       | 0.50              |

This output matches the expected results.

---
