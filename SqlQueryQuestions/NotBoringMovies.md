# [Not Boring Movies](#not-boring-movies)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists cinema (id int, movie varchar(255), description varchar(255), rating float(2, 1))
Truncate table cinema
insert into cinema (id, movie, description, rating) values ('1', 'War', 'great 3D', '8.9')
insert into cinema (id, movie, description, rating) values ('2', 'Science', 'fiction', '8.5')
insert into cinema (id, movie, description, rating) values ('3', 'irish', 'boring', '6.2')
insert into cinema (id, movie, description, rating) values ('4', 'Ice song', 'Fantacy', '8.6')
insert into cinema (id, movie, description, rating) values ('5', 'House card', 'Interesting', '9.1')
```

---

### [Question](#question)


Table: Cinema
```
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| id             | int      |
| movie          | varchar  |
| description    | varchar  |
| rating         | float    |
+----------------+----------+
```
- **id** is the primary key (column with unique values) for this table.
- Each row contains information about the name of a movie, its genre, and its rating.
- rating is a 2 decimal places float in the range [0, 10]
 

Write a solution to report the movies with an odd-numbered ID and a description that is not "boring".

Return the result table ordered by rating in descending order.

The result format is in the following example.

 

#### Example 1:

Input: 

Cinema table:
```
+----+------------+-------------+--------+
| id | movie      | description | rating |
+----+------------+-------------+--------+
| 1  | War        | great 3D    | 8.9    |
| 2  | Science    | fiction     | 8.5    |
| 3  | irish      | boring      | 6.2    |
| 4  | Ice song   | Fantacy     | 8.6    |
| 5  | House card | Interesting | 9.1    |
+----+------------+-------------+--------+
```
Output: 
```
+----+------------+-------------+--------+
| id | movie      | description | rating |
+----+------------+-------------+--------+
| 5  | House card | Interesting | 9.1    |
| 1  | War        | great 3D    | 8.9    |
+----+------------+-------------+--------+
```
Explanation: 
We have three movies with odd-numbered IDs: 1, 3, and 5. The movie with ID = 3 is boring so we do not include it in the answer.

---

### [Solution](#solution)

Here is the SQL solution for the problem:

#### SQL Query:
```sql
SELECT id, movie, description, rating
FROM cinema
WHERE id % 2 = 1 AND description != 'boring'
ORDER BY rating DESC;
```

---

#### Explanation:
1. **`id % 2 = 1`**: Filters rows where the `id` is an odd number.
2. **`description != 'boring'`**: Ensures the description of the movie is not "boring."
3. **`ORDER BY rating DESC`**: Sorts the results by the `rating` column in descending order to prioritize higher-rated movies.

---

#### Output:
For the provided input:
```
+----+------------+-------------+--------+
| id | movie      | description | rating |
+----+------------+-------------+--------+
| 5  | House card | Interesting | 9.1    |
| 1  | War        | great 3D    | 8.9    |
+----+------------+-------------+--------+
```

---
