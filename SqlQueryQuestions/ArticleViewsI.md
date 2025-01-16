# [Article Views I](#article-views-i)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Views (article_id int, author_id int, viewer_id int, view_date date)
Truncate table Views
insert into Views (article_id, author_id, viewer_id, view_date) values ('1', '3', '5', '2019-08-01')
insert into Views (article_id, author_id, viewer_id, view_date) values ('1', '3', '6', '2019-08-02')
insert into Views (article_id, author_id, viewer_id, view_date) values ('2', '7', '7', '2019-08-01')
insert into Views (article_id, author_id, viewer_id, view_date) values ('2', '7', '6', '2019-08-02')
insert into Views (article_id, author_id, viewer_id, view_date) values ('4', '7', '1', '2019-07-22')
insert into Views (article_id, author_id, viewer_id, view_date) values ('3', '4', '4', '2019-07-21')
insert into Views (article_id, author_id, viewer_id, view_date) values ('3', '4', '4', '2019-07-21')
```

---

### [Question](#question)
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| article_id    | int     |
| author_id     | int     |
| viewer_id     | int     |
| view_date     | date    |
+---------------+---------+
```
- There is no primary key (column with unique values) for this table, the table may have duplicate rows.
- Each row of this table indicates that some viewer viewed an article (written by some author) on some date. 
- Note that equal author_id and viewer_id indicate the same person.
 

Write a solution to find all the authors that viewed at least one of their own articles.

Return the result table sorted by id in ascending order.

The result format is in the following example.

 

#### Example 1:

Input: 

Views table:
```
+------------+-----------+-----------+------------+
| article_id | author_id | viewer_id | view_date  |
+------------+-----------+-----------+------------+
| 1          | 3         | 5         | 2019-08-01 |
| 1          | 3         | 6         | 2019-08-02 |
| 2          | 7         | 7         | 2019-08-01 |
| 2          | 7         | 6         | 2019-08-02 |
| 4          | 7         | 1         | 2019-07-22 |
| 3          | 4         | 4         | 2019-07-21 |
| 3          | 4         | 4         | 2019-07-21 |
+------------+-----------+-----------+------------+
```

Output:
```
+------+
| id   |
+------+
| 4    |
| 7    |
+------+
```


---

### [Solution](#solution)

Here is the solution for the **Article Views I** problem in SQL:

```sql
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY id ASC;
```

---

#### Explanation:
1. **`SELECT DISTINCT author_id AS id`**:
   - Retrieves the distinct `author_id` values where the author has viewed their own articles.

2. **`FROM Views`**:
   - Specifies the `Views` table as the source.

3. **`WHERE author_id = viewer_id`**:
   - Filters rows where the `author_id` is equal to the `viewer_id`, indicating the author viewed their own article.

4. **`ORDER BY id ASC`**:
   - Sorts the result by `id` in ascending order.

---

#### Example Walkthrough:

**Input**:

```text
+------------+-----------+-----------+------------+
| article_id | author_id | viewer_id | view_date  |
+------------+-----------+-----------+------------+
| 1          | 3         | 5         | 2019-08-01 |
| 1          | 3         | 6         | 2019-08-02 |
| 2          | 7         | 7         | 2019-08-01 |
| 2          | 7         | 6         | 2019-08-02 |
| 4          | 7         | 1         | 2019-07-22 |
| 3          | 4         | 4         | 2019-07-21 |
| 3          | 4         | 4         | 2019-07-21 |
+------------+-----------+-----------+------------+
```

**Steps**:
1. Filter rows where `author_id = viewer_id`:
   ```text
   +------------+-----------+-----------+------------+
   | article_id | author_id | viewer_id | view_date  |
   +------------+-----------+-----------+------------+
   | 2          | 7         | 7         | 2019-08-01 |
   | 3          | 4         | 4         | 2019-07-21 |
   | 3          | 4         | 4         | 2019-07-21 |
   +------------+-----------+-----------+------------+
   ```

2. Select distinct `author_id` and rename it as `id`:
   ```text
   +------+
   | id   |
   +------+
   | 7    |
   | 4    |
   +------+
   ```

3. Order by `id` in ascending order:
   ```text
   +------+
   | id   |
   +------+
   | 4    |
   | 7    |
   +------+
   ```

**Output**:
```text
+------+
| id   |
+------+
| 4    |
| 7    |
+------+
```

---
