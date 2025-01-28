# [Product Price at a Given Date](#product-price-at-a-given-date)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Products (product_id int, new_price int, change_date date)
Truncate table Products
insert into Products (product_id, new_price, change_date) values ('1', '20', '2019-08-14')
insert into Products (product_id, new_price, change_date) values ('2', '50', '2019-08-14')
insert into Products (product_id, new_price, change_date) values ('1', '30', '2019-08-15')
insert into Products (product_id, new_price, change_date) values ('1', '35', '2019-08-16')
insert into Products (product_id, new_price, change_date) values ('2', '65', '2019-08-17')
insert into Products (product_id, new_price, change_date) values ('3', '20', '2019-08-18')
```

---

### [Question](#question)

Table: Products
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| new_price     | int     |
| change_date   | date    |
+---------------+---------+
```
- (product_id, change_date) is the primary key (combination of columns with unique values) of this table.
- Each row of this table indicates that the price of some product was changed to a new price at some date.
 

Write a solution to find the prices of all products on 2019-08-16. Assume the price of all products before any change is 10.

Return the result table in any order.

The result format is in the following example.

 

#### Example 1:

Input:

Products table:
```
+------------+-----------+-------------+
| product_id | new_price | change_date |
+------------+-----------+-------------+
| 1          | 20        | 2019-08-14  |
| 2          | 50        | 2019-08-14  |
| 1          | 30        | 2019-08-15  |
| 1          | 35        | 2019-08-16  |
| 2          | 65        | 2019-08-17  |
| 3          | 20        | 2019-08-18  |
+------------+-----------+-------------+
```
Output: 
```
+------------+-------+
| product_id | price |
+------------+-------+
| 2          | 50    |
| 1          | 35    |
| 3          | 10    |
+------------+-------+
```
---

### [Solution](#solution)

#### SQL Solution:

```sql
SELECT 
    p.product_id, 
    COALESCE(
        (SELECT new_price 
         FROM Products 
         WHERE product_id = p.product_id 
           AND change_date <= '2019-08-16'
         ORDER BY change_date DESC 
         LIMIT 1), 
        10
    ) AS price
FROM 
    (SELECT DISTINCT product_id FROM Products) p;
```

---

#### Explanation:

1. **Subquery for Latest Price Before or on 2019-08-16**:
   - `(SELECT new_price FROM Products WHERE product_id = p.product_id AND change_date <= '2019-08-16' ORDER BY change_date DESC LIMIT 1)`
     - This subquery finds the most recent `new_price` for a product where the `change_date` is on or before `2019-08-16`.
     - `ORDER BY change_date DESC` ensures we get the latest price by sorting the dates in descending order.
     - `LIMIT 1` ensures only the latest price is fetched.

2. **COALESCE**:
   - If a product has no price change before or on `2019-08-16`, the subquery returns `NULL`.
   - `COALESCE` replaces `NULL` with the default price of `10`.

3. **Outer Query**:
   - `SELECT DISTINCT product_id FROM Products`: Ensures we get all unique products from the table.
   - For each product, the subquery fetches the price as of `2019-08-16`.

---

#### Example Walkthrough:

##### Input:

| product_id | new_price | change_date |
|------------|-----------|-------------|
| 1          | 20        | 2019-08-14  |
| 2          | 50        | 2019-08-14  |
| 1          | 30        | 2019-08-15  |
| 1          | 35        | 2019-08-16  |
| 2          | 65        | 2019-08-17  |
| 3          | 20        | 2019-08-18  |

##### Execution:

1. **Product 1**:
   - Latest change before or on `2019-08-16`: `35` (from `2019-08-16`).
   - Result: `35`.

2. **Product 2**:
   - Latest change before or on `2019-08-16`: `50` (from `2019-08-14`).
   - Result: `50`.

3. **Product 3**:
   - No price change before `2019-08-16`, default price is `10`.
   - Result: `10`.

##### Output:

| product_id | price |
|------------|-------|
| 1          | 35    |
| 2          | 50    |
| 3          | 10    |

---

This query efficiently calculates the price of each product as of `2019-08-16`.

---

