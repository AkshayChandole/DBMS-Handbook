# [Biggest Single Number](#biggest-single-number)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists MyNumbers (num int)
Truncate table MyNumbers
insert into MyNumbers (num) values ('8')
insert into MyNumbers (num) values ('8')
insert into MyNumbers (num) values ('3')
insert into MyNumbers (num) values ('3')
insert into MyNumbers (num) values ('1')
insert into MyNumbers (num) values ('4')
insert into MyNumbers (num) values ('5')
insert into MyNumbers (num) values ('6')
```

---

### [Question](#question)

Table: MyNumbers
```
+-------------+------+
| Column Name | Type |
+-------------+------+
| num         | int  |
+-------------+------+
```
- This table may contain duplicates (In other words, there is no primary key for this table in SQL).
- Each row of this table contains an integer.
 

A single number is a number that appeared only once in the MyNumbers table.

Find the largest single number. If there is no single number, report null.

The result format is in the following example.

 

#### Example 1:

Input: 

MyNumbers table:
```
+-----+
| num |
+-----+
| 8   |
| 8   |
| 3   |
| 3   |
| 1   |
| 4   |
| 5   |
| 6   |
+-----+
```
Output: 
```
+-----+
| num |
+-----+
| 6   |
+-----+
```
Explanation: The single numbers are 1, 4, 5, and 6.
Since 6 is the largest single number, we return it.
Example 2:

Input: 

MyNumbers table:
```
+-----+
| num |
+-----+
| 8   |
| 8   |
| 7   |
| 7   |
| 3   |
| 3   |
| 3   |
+-----+
```
Output: 
```
+------+
| num  |
+------+
| null |
+------+
```
Explanation: There are no single numbers in the input table so we return null.

---

### [Solution](#solution)

#### SQL Solution

```sql
SELECT MAX(num) AS num
FROM (
    SELECT num
    FROM MyNumbers
    GROUP BY num
    HAVING COUNT(num) = 1
) AS SingleNumbers;
```

---

#### Explanation

1. **Group By and Filter Single Numbers**:
   - `GROUP BY num`: Groups rows by each unique `num` value.
   - `HAVING COUNT(num) = 1`: Filters out numbers that appear more than once, keeping only those that appear exactly once (single numbers).

2. **Find the Maximum**:
   - `MAX(num)`: Finds the largest number among the filtered single numbers.

3. **Handle Null Case**:
   - If there are no single numbers, the `MAX` function will return `null`.

---

#### Input Example 1

**MyNumbers Table**:
```
+-----+
| num |
+-----+
| 8   |
| 8   |
| 3   |
| 3   |
| 1   |
| 4   |
| 5   |
| 6   |
+-----+
```

**Query Result**:
```
+-----+
| num |
+-----+
| 6   |
+-----+
```

#### Input Example 2

**MyNumbers Table**:
```
+-----+
| num |
+-----+
| 8   |
| 8   |
| 7   |
| 7   |
| 3   |
| 3   |
| 3   |
+-----+
```

**Query Result**:
```
+------+
| num  |
+------+
| null |
+------+
```

---

#### Explanation of Outputs

1. **Example 1**:
   - Single numbers: 1, 4, 5, and 6.
   - Largest single number: 6.

2. **Example 2**:
   - No single numbers exist, so the result is `null`.

---
