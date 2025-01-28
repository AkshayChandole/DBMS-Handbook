# [Count Salary Categories](#count-salary-categories)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Accounts (account_id int, income int)
Truncate table Accounts
insert into Accounts (account_id, income) values ('3', '108939')
insert into Accounts (account_id, income) values ('2', '12747')
insert into Accounts (account_id, income) values ('8', '87709')
insert into Accounts (account_id, income) values ('6', '91796')
```

---

### [Question](#question)

Table: Accounts
```
+-------------+------+
| Column Name | Type |
+-------------+------+
| account_id  | int  |
| income      | int  |
+-------------+------+
```
- **account_id** is the primary key (column with unique values) for this table.
- Each row contains information about the monthly income for one bank account.
 

Write a solution to calculate the number of bank accounts for each salary category. The salary categories are:

**"Low Salary"**: All the salaries strictly less than $20000.
**"Average Salary"**: All the salaries in the inclusive range [$20000, $50000].
**"High Salary"**: All the salaries strictly greater than $50000.
The result table must contain all three categories. If there are no accounts in a category, return 0.

Return the result table in any order.

The result format is in the following example.

 

#### Example 1:

Input: 

Accounts table:
```
+------------+--------+
| account_id | income |
+------------+--------+
| 3          | 108939 |
| 2          | 12747  |
| 8          | 87709  |
| 6          | 91796  |
+------------+--------+
```
Output: 
```
+----------------+----------------+
| category       | accounts_count |
+----------------+----------------+
| Low Salary     | 1              |
| Average Salary | 0              |
| High Salary    | 3              |
+----------------+----------------+
```
Explanation: 
- Low Salary: Account 2.
- Average Salary: No accounts.
- High Salary: Accounts 3, 6, and 8.

---

### [Solution](#solution)
```sql
SELECT 'Low Salary' AS category, 
       COUNT(if(income<20000,1,null)) AS accounts_count
FROM Accounts
UNION ALL
SELECT 'Average Salary', 
       COUNT(if(income>=20000 and income<=50000,1,null))
FROM Accounts
UNION ALL
SELECT 'High Salary', 
       COUNT(if(income>50000,1,null))
FROM Accounts;
```

This SQL query uses `UNION ALL` to combine the counts of accounts in each salary category. Here's how it works, step by step:

---

#### Query Breakdown:

##### 1. **First Part (`Low Salary`):**
```sql
SELECT 'Low Salary' AS category, 
       COUNT(IF(income < 20000, 1, NULL)) AS accounts_count
FROM Accounts
```
- `'Low Salary' AS category`: Creates a static column with the value `Low Salary` for this part of the query.
- `IF(income < 20000, 1, NULL)`: Checks if the `income` is strictly less than 20,000:
  - If the condition is `TRUE`, it returns `1`.
  - If the condition is `FALSE`, it returns `NULL`.
- `COUNT(...)`: Counts the non-NULL values returned by the `IF` statement, effectively counting all accounts with income < 20,000.

##### 2. **Second Part (`Average Salary`):**
```sql
SELECT 'Average Salary', 
       COUNT(IF(income >= 20000 AND income <= 50000, 1, NULL))
FROM Accounts
```
- `'Average Salary'`: Creates a static column with the value `Average Salary`.
- `IF(income >= 20000 AND income <= 50000, 1, NULL)`: Checks if the `income` falls in the inclusive range [20,000, 50,000].
- `COUNT(...)`: Counts the non-NULL values, giving the number of accounts with income in this range.

##### 3. **Third Part (`High Salary`):**
```sql
SELECT 'High Salary', 
       COUNT(IF(income > 50000, 1, NULL))
FROM Accounts
```
- `'High Salary'`: Creates a static column with the value `High Salary`.
- `IF(income > 50000, 1, NULL)`: Checks if the `income` is greater than 50,000.
- `COUNT(...)`: Counts the non-NULL values, giving the number of accounts with income > 50,000.

##### 4. **Combining Results with `UNION ALL`:**
- `UNION ALL`: Combines the three `SELECT` statements into a single result set.
- Each query outputs one row with the salary category and the corresponding count of accounts.
- `UNION ALL` is used instead of `UNION` to keep all rows (even if some counts are `0`).

---

#### Example Output:

For the given `Accounts` table:
```
+------------+--------+
| account_id | income |
+------------+--------+
| 3          | 108939 |
| 2          | 12747  |
| 8          | 87709  |
| 6          | 91796  |
+------------+--------+
```

The query outputs:
```
+----------------+----------------+
| category       | accounts_count |
+----------------+----------------+
| Low Salary     | 1              | -- Account 2
| Average Salary | 0              | -- No accounts
| High Salary    | 3              | -- Accounts 3, 6, 8
+----------------+----------------+
```


---
