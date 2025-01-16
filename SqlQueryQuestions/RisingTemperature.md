# [Rising Temperature](#rising-temperature)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Weather (id int, recordDate date, temperature int)
Truncate table Weather
insert into Weather (id, recordDate, temperature) values ('1', '2015-01-01', '10')
insert into Weather (id, recordDate, temperature) values ('2', '2015-01-02', '25')
insert into Weather (id, recordDate, temperature) values ('3', '2015-01-03', '20')
insert into Weather (id, recordDate, temperature) values ('4', '2015-01-04', '30')
```

---

### [Question](#question)

Table: Weather
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
```
- **id** is the column with unique values for this table.
- There are no different rows with the same recordDate.
- This table contains information about the temperature on a certain day.
 

Write a solution to find all dates' id with higher temperatures compared to its previous dates (yesterday).

Return the result table in any order.

The result format is in the following example.

 

#### Example 1:

Input:

Weather table:
```
+----+------------+-------------+
| id | recordDate | temperature |
+----+------------+-------------+
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |
+----+------------+-------------+
```
Output: 
```
+----+
| id |
+----+
| 2  |
| 4  |
+----+
```
Explanation: 
- In 2015-01-02, the temperature was higher than the previous day (10 -> 25).
- In 2015-01-04, the temperature was higher than the previous day (20 -> 30).

---

### [Solution](#solution)
Here’s the SQL query to find all dates' IDs where the temperature was higher compared to the previous day:

```sql
SELECT 
    w1.id
FROM 
    Weather w1
JOIN 
    Weather w2
ON 
    DATE(w1.recordDate) = DATE(w2.recordDate) + INTERVAL 1 DAY
WHERE 
    w1.temperature > w2.temperature;
```

---

#### Explanation:
1. **Self-Join**:
   - We join the `Weather` table with itself. `w1` refers to the current record, and `w2` refers to the record for the previous day.

2. **Date Matching**:
   - The condition `DATE(w1.recordDate) = DATE(w2.recordDate) + INTERVAL 1 DAY` ensures we compare each date's temperature with the previous day's temperature.

3. **Filter Higher Temperature**:
   - The `WHERE w1.temperature > w2.temperature` condition selects only those records where the temperature of the current day (`w1.temperature`) is higher than the temperature of the previous day (`w2.temperature`).

4. **Select ID**:
   - The query outputs the `id` column for the dates with a rising temperature.

---

#### Output for Given Data:
```
+----+
| id |
+----+
| 2  |
| 4  |
+----+
```

This result matches the example, as:
- On `2015-01-02`, the temperature (25) is higher than `2015-01-01` (10).
- On `2015-01-04`, the temperature (30) is higher than `2015-01-03` (20).

---
