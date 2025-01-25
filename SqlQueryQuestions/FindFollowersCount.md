# [Find Followers Count](#find-followers-count)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Followers(user_id int, follower_id int)
Truncate table Followers
insert into Followers (user_id, follower_id) values ('0', '1')
insert into Followers (user_id, follower_id) values ('1', '0')
insert into Followers (user_id, follower_id) values ('2', '0')
insert into Followers (user_id, follower_id) values ('2', '1')
```

---

### [Question](#question)

Table: Followers
```
+-------------+------+
| Column Name | Type |
+-------------+------+
| user_id     | int  |
| follower_id | int  |
+-------------+------+
```
- **(user_id, follower_id)** is the primary key (combination of columns with unique values) for this table.
- This table contains the IDs of a user and a follower in a social media app where the follower follows the user.
 

Write a solution that will, for each user, return the number of followers.

Return the result table ordered by user_id in ascending order.

The result format is in the following example.

 

#### Example 1:

Input: 

Followers table:
```
+---------+-------------+
| user_id | follower_id |
+---------+-------------+
| 0       | 1           |
| 1       | 0           |
| 2       | 0           |
| 2       | 1           |
+---------+-------------+
```
Output: 
```
+---------+----------------+
| user_id | followers_count|
+---------+----------------+
| 0       | 1              |
| 1       | 1              |
| 2       | 2              |
+---------+----------------+
```
Explanation: 
- The followers of 0 are {1}
- The followers of 1 are {0}
- The followers of 2 are {0,1}

---

### [Solution](#solution)

#### SQL Solution
```sql
SELECT user_id, COUNT(follower_id) AS followers_count
FROM Followers
GROUP BY user_id
ORDER BY user_id ASC;
```

---

#### Explanation

1. **Count Followers**:
   - `COUNT(follower_id)` counts the number of followers for each `user_id`.

2. **Group By**:
   - The `GROUP BY user_id` groups all rows by each unique `user_id`, ensuring that the count of followers is calculated per user.

3. **Order By**:
   - The `ORDER BY user_id ASC` sorts the result in ascending order of `user_id`.

---

#### Input Example

**Followers Table**:
```
+---------+-------------+
| user_id | follower_id |
+---------+-------------+
| 0       | 1           |
| 1       | 0           |
| 2       | 0           |
| 2       | 1           |
+---------+-------------+
```

#### Query Result
```
+---------+----------------+
| user_id | followers_count|
+---------+----------------+
| 0       | 1              |
| 1       | 1              |
| 2       | 2              |
+---------+----------------+
```

---

#### Explanation of the Output
- **User 0**: Has 1 follower (ID 1).
- **User 1**: Has 1 follower (ID 0).
- **User 2**: Has 2 followers (IDs 0 and 1).

---
