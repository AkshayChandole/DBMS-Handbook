# [Friend Requests II: Who Has the Most Friends](#friend-requests-ii-who-has-the-most-friends)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists RequestAccepted (requester_id int not null, accepter_id int null, accept_date date null)
Truncate table RequestAccepted
insert into RequestAccepted (requester_id, accepter_id, accept_date) values ('1', '2', '2016/06/03')
insert into RequestAccepted (requester_id, accepter_id, accept_date) values ('1', '3', '2016/06/08')
insert into RequestAccepted (requester_id, accepter_id, accept_date) values ('2', '3', '2016/06/08')
insert into RequestAccepted (requester_id, accepter_id, accept_date) values ('3', '4', '2016/06/09')
```

---

### [Question](#question)

Table: RequestAccepted
```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| requester_id   | int     |
| accepter_id    | int     |
| accept_date    | date    |
+----------------+---------+
```
- **(requester_id, accepter_id)** is the primary key (combination of columns with unique values) for this table.
- This table contains the ID of the user who sent the request, the ID of the user who received the request, and the date when the request was accepted.
 

Write a solution to find the people who have the most friends and the most friends number.

The test cases are generated so that only one person has the most friends.

The result format is in the following example.

 

#### Example 1:

Input:

RequestAccepted table:
```
+--------------+-------------+-------------+
| requester_id | accepter_id | accept_date |
+--------------+-------------+-------------+
| 1            | 2           | 2016/06/03  |
| 1            | 3           | 2016/06/08  |
| 2            | 3           | 2016/06/08  |
| 3            | 4           | 2016/06/09  |
+--------------+-------------+-------------+
```
Output: 
```
+----+-----+
| id | num |
+----+-----+
| 3  | 3   |
+----+-----+
```
Explanation: 
- The person with id 3 is a friend of people 1, 2, and 4, so he has three friends in total, which is the most number than any others.
 

Follow up: In the real world, multiple people could have the same most number of friends. Could you find all these people in this case?

---

### [Solution](#solution)


Here’s the SQL solution to find the person with the most friends using **UNION ALL** to count both requester and accepter as friends.

#### **Solution:**
```sql
WITH FriendCount AS (
    SELECT requester_id AS id, COUNT(accepter_id) AS num
    FROM RequestAccepted
    GROUP BY requester_id
    UNION ALL
    SELECT accepter_id AS id, COUNT(requester_id) AS num
    FROM RequestAccepted
    GROUP BY accepter_id
)
SELECT id, SUM(num) AS num
FROM FriendCount
GROUP BY id
ORDER BY num DESC
LIMIT 1;
```

---

#### **Explanation:**
1. **Count Friends for Each User**  
   - We count friendships where the user is the **requester**.
   - We count friendships where the user is the **accepter**.
   - `UNION ALL` is used to combine both counts because friendships are **bidirectional**.

2. **Summing Friendships for Each User**  
   - We group by `id` and sum the `num` values to get the **total number of friends**.

3. **Sorting and Selecting the Most Popular Person**  
   - We **order by `num` DESC** to get the person with the highest friend count.
   - We use `LIMIT 1` to return **only the top user**.

---

#### **Output for Given Example:**
```
+----+-----+
| id | num |
+----+-----+
| 3  | 3   |
+----+-----+
```
**User 3 has the most friends (1, 2, and 4).** ✅


---

