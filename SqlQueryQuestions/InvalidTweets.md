# [Invalid Tweets](#invalid-tweets)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Tweets(tweet_id int, content varchar(50))
Truncate table Tweets
insert into Tweets (tweet_id, content) values ('1', 'Let us Code')
insert into Tweets (tweet_id, content) values ('2', 'More than fifteen chars are here!')
```

---

### [Question](#question)
```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| tweet_id       | int     |
| content        | varchar |
+----------------+---------+
```
- **tweet_id** is the primary key (column with unique values) for this table.
- **content** consists of characters on an American Keyboard, and no other special characters.
- This table contains all the tweets in a social media app.
 

Write a solution to find the IDs of the invalid tweets. The tweet is invalid if the number of characters used in the content of the tweet is strictly greater than 15.

Return the result table in any order.

The result format is in the following example.

 
#### Example 1:

Input: 

Tweets table:
```
+----------+-----------------------------------+
| tweet_id | content                           |
+----------+-----------------------------------+
| 1        | Let us Code                       |
| 2        | More than fifteen chars are here! |
+----------+-----------------------------------+
```
Output: 
```
+----------+
| tweet_id |
+----------+
| 2        |
+----------+
```
Explanation: 
Tweet 1 has length = 11. It is a valid tweet.
Tweet 2 has length = 33. It is an invalid tweet.

---

### [Solution](#solution)

Here is the solution for the **Invalid Tweets** problem in SQL:

```sql
SELECT tweet_id
FROM Tweets
WHERE LENGTH(content) > 15;
```

---

#### Explanation:
1. **`SELECT tweet_id`**:
   - Retrieves the `tweet_id` of the tweets that are invalid.

2. **`FROM Tweets`**:
   - Specifies the `Tweets` table as the data source.

3. **`WHERE LENGTH(content) > 15`**:
   - Filters tweets where the length of the `content` exceeds 15 characters, marking them as invalid.

---

#### Example Walkthrough:

**Input**:

```text
+----------+-----------------------------------+
| tweet_id | content                           |
+----------+-----------------------------------+
| 1        | Let us Code                       |
| 2        | More than fifteen chars are here! |
+----------+-----------------------------------+
```

**Steps**:
1. Calculate the length of each `content`:
   ```text
   +----------+-------------------------------+
   | tweet_id | LENGTH(content)               |
   +----------+-------------------------------+
   | 1        | 11                            |
   | 2        | 33                            |
   +----------+-------------------------------+
   ```

2. Filter rows where `LENGTH(content) > 15`:
   ```text
   +----------+
   | tweet_id |
   +----------+
   | 2        |
   +----------+
   ```

**Output**:

```text
+----------+
| tweet_id |
+----------+
| 2        |
+----------+
```

---
