# [Monthly Transactions I](#monthly-transactions-i)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Transactions (id int, country varchar(4), state enum('approved', 'declined'), amount int, trans_date date)
Truncate table Transactions
insert into Transactions (id, country, state, amount, trans_date) values ('121', 'US', 'approved', '1000', '2018-12-18')
insert into Transactions (id, country, state, amount, trans_date) values ('122', 'US', 'declined', '2000', '2018-12-19')
insert into Transactions (id, country, state, amount, trans_date) values ('123', 'US', 'approved', '2000', '2019-01-01')
insert into Transactions (id, country, state, amount, trans_date) values ('124', 'DE', 'approved', '2000', '2019-01-07')
```

---

### [Question](#question)


Table: Transactions
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| country       | varchar |
| state         | enum    |
| amount        | int     |
| trans_date    | date    |
+---------------+---------+
```
- **id** is the primary key of this table.
- The table has information about incoming transactions.
- The state column is an enum of type ["approved", "declined"].
 

Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.

Return the result table in any order.

The query result format is in the following example.

 

#### Example 1:

Input:

Transactions table:
```
+------+---------+----------+--------+------------+
| id   | country | state    | amount | trans_date |
+------+---------+----------+--------+------------+
| 121  | US      | approved | 1000   | 2018-12-18 |
| 122  | US      | declined | 2000   | 2018-12-19 |
| 123  | US      | approved | 2000   | 2019-01-01 |
| 124  | DE      | approved | 2000   | 2019-01-07 |
+------+---------+----------+--------+------------+
```
Output: 
```
+----------+---------+-------------+----------------+--------------------+-----------------------+
| month    | country | trans_count | approved_count | trans_total_amount | approved_total_amount |
+----------+---------+-------------+----------------+--------------------+-----------------------+
| 2018-12  | US      | 2           | 1              | 3000               | 1000                  |
| 2019-01  | US      | 1           | 1              | 2000               | 2000                  |
| 2019-01  | DE      | 1           | 1              | 2000               | 2000                  |
+----------+---------+-------------+----------------+--------------------+-----------------------+
```

---

### [Solution](#solution)

#### Solution:

```sql
SELECT 
    DATE_FORMAT(trans_date, '%Y-%m') AS month,
    country,
    COUNT(*) AS trans_count,
    SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM 
    Transactions
GROUP BY 
    month, country;
```

---

#### Explanation:

1. **Extracting Month**:
   - **`DATE_FORMAT(trans_date, '%Y-%m')`**: Extracts the year and month in the format `YYYY-MM` from the `trans_date`.

2. **Transaction Count**:
   - **`COUNT(*)`**: Counts the total number of transactions for each month and country.

3. **Approved Transaction Count**:
   - **`SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END)`**: Counts only the transactions with `state = 'approved'`.

4. **Total Transaction Amount**:
   - **`SUM(amount)`**: Sums up the `amount` column for all transactions in the group.

5. **Total Approved Transaction Amount**:
   - **`SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END)`**: Sums up the `amount` for transactions where `state = 'approved'`.

6. **Grouping**:
   - **`GROUP BY month, country`**: Groups the data by month and country to calculate the required metrics for each group.

---

#### Output for the Example Input:

For the given input:
- **2018-12, US**:
  - Transactions: 2
  - Approved Transactions: 1
  - Total Amount: 3000
  - Approved Amount: 1000

- **2019-01, US**:
  - Transactions: 1
  - Approved Transactions: 1
  - Total Amount: 2000
  - Approved Amount: 2000

- **2019-01, DE**:
  - Transactions: 1
  - Approved Transactions: 1
  - Total Amount: 2000
  - Approved Amount: 2000

Output:
```
+----------+---------+-------------+----------------+--------------------+-----------------------+
| month    | country | trans_count | approved_count | trans_total_amount | approved_total_amount |
+----------+---------+-------------+----------------+--------------------+-----------------------+
| 2018-12  | US      | 2           | 1              | 3000               | 1000                  |
| 2019-01  | US      | 1           | 1              | 2000               | 2000                  |
| 2019-01  | DE      | 1           | 1              | 2000               | 2000                  |
+----------+---------+-------------+----------------+--------------------+-----------------------+
```

---
