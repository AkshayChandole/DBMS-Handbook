# [Movie Rating](#movie-rating)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Movies (movie_id int, title varchar(30))
Create table If Not Exists Users (user_id int, name varchar(30))
Create table If Not Exists MovieRating (movie_id int, user_id int, rating int, created_at date)
Truncate table Movies
insert into Movies (movie_id, title) values ('1', 'Avengers')
insert into Movies (movie_id, title) values ('2', 'Frozen 2')
insert into Movies (movie_id, title) values ('3', 'Joker')
Truncate table Users
insert into Users (user_id, name) values ('1', 'Daniel')
insert into Users (user_id, name) values ('2', 'Monica')
insert into Users (user_id, name) values ('3', 'Maria')
insert into Users (user_id, name) values ('4', 'James')
Truncate table MovieRating
insert into MovieRating (movie_id, user_id, rating, created_at) values ('1', '1', '3', '2020-01-12')
insert into MovieRating (movie_id, user_id, rating, created_at) values ('1', '2', '4', '2020-02-11')
insert into MovieRating (movie_id, user_id, rating, created_at) values ('1', '3', '2', '2020-02-12')
insert into MovieRating (movie_id, user_id, rating, created_at) values ('1', '4', '1', '2020-01-01')
insert into MovieRating (movie_id, user_id, rating, created_at) values ('2', '1', '5', '2020-02-17')
insert into MovieRating (movie_id, user_id, rating, created_at) values ('2', '2', '2', '2020-02-01')
insert into MovieRating (movie_id, user_id, rating, created_at) values ('2', '3', '2', '2020-03-01')
insert into MovieRating (movie_id, user_id, rating, created_at) values ('3', '1', '3', '2020-02-22')
insert into MovieRating (movie_id, user_id, rating, created_at) values ('3', '2', '4', '2020-02-25')
```

---

### [Question](#question)

Table: Movies
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| movie_id      | int     |
| title         | varchar |
+---------------+---------+
```
- **movie_id** is the primary key (column with unique values) for this table.
- **title** is the name of the movie.
 

Table: Users
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| name          | varchar |
+---------------+---------+
```
- **user_id** is the primary key (column with unique values) for this table.
- The column '**name**' has unique values.

  
Table: MovieRating
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| movie_id      | int     |
| user_id       | int     |
| rating        | int     |
| created_at    | date    |
+---------------+---------+
```
- **(movie_id, user_id)** is the primary key (column with unique values) for this table.
- This table contains the rating of a movie by a user in their review.
- **created_at** is the user's review date. 
 

Write a solution to:

Find the name of the user who has rated the greatest number of movies. In case of a tie, return the lexicographically smaller user name.
Find the movie name with the highest average rating in February 2020. In case of a tie, return the lexicographically smaller movie name.
The result format is in the following example.

 

#### Example 1:

Input: 

Movies table:
```
+-------------+--------------+
| movie_id    |  title       |
+-------------+--------------+
| 1           | Avengers     |
| 2           | Frozen 2     |
| 3           | Joker        |
+-------------+--------------+
```
Users table:
```
+-------------+--------------+
| user_id     |  name        |
+-------------+--------------+
| 1           | Daniel       |
| 2           | Monica       |
| 3           | Maria        |
| 4           | James        |
+-------------+--------------+
```
MovieRating table:
```
+-------------+--------------+--------------+-------------+
| movie_id    | user_id      | rating       | created_at  |
+-------------+--------------+--------------+-------------+
| 1           | 1            | 3            | 2020-01-12  |
| 1           | 2            | 4            | 2020-02-11  |
| 1           | 3            | 2            | 2020-02-12  |
| 1           | 4            | 1            | 2020-01-01  |
| 2           | 1            | 5            | 2020-02-17  | 
| 2           | 2            | 2            | 2020-02-01  | 
| 2           | 3            | 2            | 2020-03-01  |
| 3           | 1            | 3            | 2020-02-22  | 
| 3           | 2            | 4            | 2020-02-25  | 
+-------------+--------------+--------------+-------------+
```
Output: 
```
+--------------+
| results      |
+--------------+
| Daniel       |
| Frozen 2     |
+--------------+
```
Explanation: 
Daniel and Monica have rated 3 movies ("Avengers", "Frozen 2" and "Joker") but Daniel is smaller lexicographically.
Frozen 2 and Joker have a rating average of 3.5 in February but Frozen 2 is smaller lexicographically.

---

### [Solution](#solution)


#### **Solution Explanation**
```sql
(
  SELECT u.name AS results
  FROM Users u
  JOIN MovieRating mr ON u.user_id = mr.user_id
  GROUP BY u.user_id
  ORDER BY COUNT(mr.movie_id) DESC, u.name ASC
  LIMIT 1
)
UNION ALL
(
  SELECT m.title AS results
  FROM Movies m
  JOIN MovieRating mr ON m.movie_id = mr.movie_id
  WHERE DATE_FORMAT(mr.created_at, '%Y-%m') = '2020-02'
  GROUP BY m.movie_id
  ORDER BY AVG(mr.rating) DESC, m.title ASC
  LIMIT 1
);
```

---

#### **Breakdown of the Query**
##### **1️⃣ Finding the User Who Rated the Most Movies**
```sql
SELECT u.name AS results
FROM Users u
JOIN MovieRating mr ON u.user_id = mr.user_id
GROUP BY u.user_id
ORDER BY COUNT(mr.movie_id) DESC, u.name ASC
LIMIT 1
```
- **Joins** `Users` and `MovieRating` tables on `user_id`.
- **Counts** the number of ratings each user has given (`COUNT(mr.movie_id)`).
- **Sorts** by:
  - **Highest count first** (`DESC`).
  - **Lexicographically smallest name in case of tie** (`ASC`).
- **Returns the top user** (`LIMIT 1`).

---

##### **2️⃣ Finding the Highest-Rated Movie in February 2020**
```sql
SELECT m.title AS results
FROM Movies m
JOIN MovieRating mr ON m.movie_id = mr.movie_id
WHERE DATE_FORMAT(mr.created_at, '%Y-%m') = '2020-02'
GROUP BY m.movie_id
ORDER BY AVG(mr.rating) DESC, m.title ASC
LIMIT 1
```
- **Filters** the `MovieRating` table to only consider ratings from **February 2020**.
- **Joins** `Movies` and `MovieRating` on `movie_id`.
- **Calculates** the average rating for each movie (`AVG(mr.rating)`).
- **Sorts** by:
  - **Highest average rating first** (`DESC`).
  - **Lexicographically smallest title in case of tie** (`ASC`).
- **Returns the top-rated movie** (`LIMIT 1`).

---

#### **Example Execution**
##### **Input Tables**
**Movies Table**
| movie_id | title    |
|----------|---------|
| 1        | Avengers |
| 2        | Frozen 2 |
| 3        | Joker |

**Users Table**
| user_id | name   |
|---------|--------|
| 1       | Daniel |
| 2       | Monica |
| 3       | Maria  |
| 4       | James  |

**MovieRating Table**
| movie_id | user_id | rating | created_at  |
|----------|---------|--------|-------------|
| 1        | 1       | 3      | 2020-01-12  |
| 1        | 2       | 4      | 2020-02-11  |
| 1        | 3       | 2      | 2020-02-12  |
| 2        | 1       | 5      | 2020-02-17  |
| 2        | 2       | 2      | 2020-02-01  |
| 3        | 1       | 3      | 2020-02-22  |
| 3        | 2       | 4      | 2020-02-25  |

---

#### **Step-by-Step Execution**
##### **Step 1: Finding the Most Active User**
- **Daniel** and **Monica** both rated **3 movies**.
- **Lexicographically smaller** name: ✅ **Daniel**.

##### **Step 2: Finding the Best-Rated Movie in February 2020**
- **Frozen 2** has an **average rating of 3.5** (`(5+2)/2 = 3.5`).
- **Joker** has an **average rating of 3.5** (`(3+4)/2 = 3.5`).
- **Lexicographically smaller**: ✅ **Frozen 2**.

---

#### **Final Output**
| results   |
|-----------|
| Daniel    |
| Frozen 2  |

✅ **This query efficiently finds the top movie reviewer and the highest-rated movie in February 2020.** 🚀

---
