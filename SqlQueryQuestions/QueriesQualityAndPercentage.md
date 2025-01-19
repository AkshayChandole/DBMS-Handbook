# [Queries Quality and Percentage](#queries-quality-and-percentage)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Queries (query_name varchar(30), result varchar(50), position int, rating int)
Truncate table Queries
insert into Queries (query_name, result, position, rating) values ('Dog', 'Golden Retriever', '1', '5')
insert into Queries (query_name, result, position, rating) values ('Dog', 'German Shepherd', '2', '5')
insert into Queries (query_name, result, position, rating) values ('Dog', 'Mule', '200', '1')
insert into Queries (query_name, result, position, rating) values ('Cat', 'Shirazi', '5', '2')
insert into Queries (query_name, result, position, rating) values ('Cat', 'Siamese', '3', '3')
insert into Queries (query_name, result, position, rating) values ('Cat', 'Sphynx', '7', '4')
```

---

### [Question](#question)

Table: Queries
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| query_name  | varchar |
| result      | varchar |
| position    | int     |
| rating      | int     |
+-------------+---------+
```
- This table may have duplicate rows.
- This table contains information collected from some queries on a database.
- The position column has a value from 1 to 500.
- The rating column has a value from 1 to 5. Query with rating less than 3 is a poor query.
 

We define query quality as:

> The average of the ratio between query rating and its position.

We also define poor query percentage as:

> The percentage of all queries with rating less than 3.

Write a solution to find each query_name, the quality and poor_query_percentage.

Both quality and poor_query_percentage should be rounded to 2 decimal places.

Return the result table in any order.

The result format is in the following example.


#### Example 1:

Input: 

Queries table:
```
+------------+-------------------+----------+--------+
| query_name | result            | position | rating |
+------------+-------------------+----------+--------+
| Dog        | Golden Retriever  | 1        | 5      |
| Dog        | German Shepherd   | 2        | 5      |
| Dog        | Mule              | 200      | 1      |
| Cat        | Shirazi           | 5        | 2      |
| Cat        | Siamese           | 3        | 3      |
| Cat        | Sphynx            | 7        | 4      |
+------------+-------------------+----------+--------+
```
Output: 
```
+------------+---------+-----------------------+
| query_name | quality | poor_query_percentage |
+------------+---------+-----------------------+
| Dog        | 2.50    | 33.33                 |
| Cat        | 0.66    | 33.33                 |
+------------+---------+-----------------------+
```
Explanation: 
- Dog queries quality is ((5 / 1) + (5 / 2) + (1 / 200)) / 3 = 2.50
- Dog queries poor_ query_percentage is (1 / 3) * 100 = 33.33

- Cat queries quality equals ((2 / 5) + (3 / 3) + (4 / 7)) / 3 = 0.66
- Cat queries poor_ query_percentage is (1 / 3) * 100 = 33.33

---

### [Solution](#solution)

### Solution:

```sql
SELECT 
    query_name,
    ROUND(SUM(rating * 1.0 / position) / COUNT(*), 2) AS quality,
    ROUND(SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS poor_query_percentage
FROM 
    Queries
GROUP BY 
    query_name;
```

---

### Explanation:

1. **Query Quality Calculation**:
   - **`rating / position`**: Calculate the ratio of the `rating` to the `position` for each query.
   - **`SUM(rating / position) / COUNT(*)`**: Calculate the average of the ratios for each `query_name`. This is the quality of the queries for that query name.
   - **`ROUND(..., 2)`**: Round the quality to 2 decimal places.

2. **Poor Query Percentage Calculation**:
   - **`CASE WHEN rating < 3 THEN 1 ELSE 0 END`**: Identify queries with a rating less than 3 (poor queries) and assign a value of 1 to them; otherwise, assign 0.
   - **`SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END)`**: Count the number of poor queries for each `query_name`.
   - **`SUM(...) * 100.0 / COUNT(*)`**: Calculate the percentage of poor queries relative to the total number of queries for each `query_name`.
   - **`ROUND(..., 2)`**: Round the percentage to 2 decimal places.

3. **Grouping**:
   - **`GROUP BY query_name`**: Perform the calculations separately for each `query_name`.

---

### Output for the Example Input:

For the given input:
- **Dog**:
  - Quality:
    
    > Quality = ((5 / 1) + (5 / 2) + (1 / 200)) / 3 = 2.50
  - Poor Query Percentage:
    
    > Percentage = (1 / 3) * 100 = 33.33

- **Cat**:
  - Quality:
    
    > Quality = ((2 / 5) + (3 / 3) + (4 / 7)) / 3 = 0.66
  - Poor Query Percentage:
    
    > Percentage = (1 / 3) * 100 = 33.33

Output:
```
+------------+---------+-----------------------+
| query_name | quality | poor_query_percentage |
+------------+---------+-----------------------+
| Dog        | 2.50    | 33.33                 |
| Cat        | 0.66    | 33.33                 |
+------------+---------+-----------------------+
```
---

