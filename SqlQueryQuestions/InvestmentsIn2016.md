# [Investments in 2016](#investments-in-2016)

### [SQL Schema](#sql-schema)
```sql
Create Table If Not Exists Insurance (pid int, tiv_2015 float, tiv_2016 float, lat float, lon float)
Truncate table Insurance
insert into Insurance (pid, tiv_2015, tiv_2016, lat, lon) values ('1', '10', '5', '10', '10')
insert into Insurance (pid, tiv_2015, tiv_2016, lat, lon) values ('2', '20', '20', '20', '20')
insert into Insurance (pid, tiv_2015, tiv_2016, lat, lon) values ('3', '10', '30', '20', '20')
insert into Insurance (pid, tiv_2015, tiv_2016, lat, lon) values ('4', '10', '40', '40', '40')
```

---

### [Question](#question)

Table: Insurance
```
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| pid         | int   |
| tiv_2015    | float |
| tiv_2016    | float |
| lat         | float |
| lon         | float |
+-------------+-------+
```
- **pid** is the primary key (column with unique values) for this table.
- Each row of this table contains information about one policy where:
- **pid** is the policyholder's policy ID.
- **tiv_2015** is the total investment value in 2015 and tiv_2016 is the total investment value in 2016.
- **lat** is the latitude of the policy holder's city. It's guaranteed that lat is not NULL.
- **lon** is the longitude of the policy holder's city. It's guaranteed that lon is not NULL.
 

Write a solution to report the sum of all total investment values in 2016 tiv_2016, for all policyholders who:

have the same tiv_2015 value as one or more other policyholders, and
are not located in the same city as any other policyholder (i.e., the (lat, lon) attribute pairs must be unique).
Round tiv_2016 to two decimal places.

The result format is in the following example.

 

#### Example 1:

Input: 

Insurance table:
```
+-----+----------+----------+-----+-----+
| pid | tiv_2015 | tiv_2016 | lat | lon |
+-----+----------+----------+-----+-----+
| 1   | 10       | 5        | 10  | 10  |
| 2   | 20       | 20       | 20  | 20  |
| 3   | 10       | 30       | 20  | 20  |
| 4   | 10       | 40       | 40  | 40  |
+-----+----------+----------+-----+-----+
```
Output:
```
+----------+
| tiv_2016 |
+----------+
| 45.00    |
+----------+
```
Explanation: 
- The first record in the table, like the last record, meets both of the two criteria.
- The **tiv_2015** value 10 is the same as the third and fourth records, and its location is unique.

The second record does not meet any of the two criteria. Its tiv_2015 is not like any other policyholders and its location is the same as the third record, which makes the third record fail, too.
So, the result is the sum of tiv_2016 of the first and last record, which is 45.


---

### [Solution](#solution)


#### **Solution:**
```sql
SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM Insurance
WHERE tiv_2015 IN (
    SELECT tiv_2015
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) > 1
)
AND (lat, lon) IN (
    SELECT lat, lon
    FROM Insurance
    GROUP BY lat, lon
    HAVING COUNT(*) = 1
);
```

---

#### **Explanation:**
We need to find the sum of **tiv_2016** values for policyholders who:
1. **Have the same `tiv_2015` as at least one other policyholder**.
2. **Have a unique `(lat, lon)` pair**, meaning they are the only ones in their city.

##### **Breaking Down the Query:**
1. **Find policyholders with duplicate `tiv_2015`:**
   ```sql
   SELECT tiv_2015
   FROM Insurance
   GROUP BY tiv_2015
   HAVING COUNT(*) > 1
   ```
   - Groups records by `tiv_2015` and selects only those appearing more than once.

2. **Find unique `(lat, lon)` locations:**
   ```sql
   SELECT lat, lon
   FROM Insurance
   GROUP BY lat, lon
   HAVING COUNT(*) = 1
   ```
   - Groups records by `(lat, lon)` and selects only those appearing **exactly once**.

3. **Filter main table using both conditions**:
   ```sql
   WHERE tiv_2015 IN (...)  -- tiv_2015 appears more than once
   AND (lat, lon) IN (...)  -- unique location
   ```
   - The `WHERE` clause ensures that we select only records that satisfy both criteria.

4. **Sum `tiv_2016` and round to two decimal places:**
   ```sql
   SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016
   ```

---

#### **Output for Given Example:**
```
+----------+
| tiv_2016 |
+----------+
| 45.00    |
+----------+
```
✔ **Policyholders included in the sum**:  
- **pid = 1 (tiv_2016 = 5)** → ✅ `tiv_2015` = 10 (exists in multiple rows) + ✅ unique `(lat, lon)`
- **pid = 4 (tiv_2016 = 40)** → ✅ `tiv_2015` = 10 (exists in multiple rows) + ✅ unique `(lat, lon)`

🔴 **Excluded**:
- **pid = 2**: Unique `tiv_2015`, and location is not unique.
- **pid = 3**: Location is not unique.

✔ **Final sum** = `5 + 40 = 45.00` ✅

---
