# [Average Selling Price](#average-selling-price)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Prices (product_id int, start_date date, end_date date, price int)
Create table If Not Exists UnitsSold (product_id int, purchase_date date, units int)
Truncate table Prices
insert into Prices (product_id, start_date, end_date, price) values ('1', '2019-02-17', '2019-02-28', '5')
insert into Prices (product_id, start_date, end_date, price) values ('1', '2019-03-01', '2019-03-22', '20')
insert into Prices (product_id, start_date, end_date, price) values ('2', '2019-02-01', '2019-02-20', '15')
insert into Prices (product_id, start_date, end_date, price) values ('2', '2019-02-21', '2019-03-31', '30')
Truncate table UnitsSold
insert into UnitsSold (product_id, purchase_date, units) values ('1', '2019-02-25', '100')
insert into UnitsSold (product_id, purchase_date, units) values ('1', '2019-03-01', '15')
insert into UnitsSold (product_id, purchase_date, units) values ('2', '2019-02-10', '200')
insert into UnitsSold (product_id, purchase_date, units) values ('2', '2019-03-22', '30')
```

---

### [Question](#question)

Table: Prices
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| start_date    | date    |
| end_date      | date    |
| price         | int     |
+---------------+---------+
```
- **(product_id, start_date, end_date)** is the primary key (combination of columns with unique values) for this table.
- Each row of this table indicates the price of the product_id in the period from start_date to end_date.
- For each product_id there will be no two overlapping periods. That means there will be no two intersecting periods for the same product_id.
 

Table: UnitsSold
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| purchase_date | date    |
| units         | int     |
+---------------+---------+
```
- This table may contain duplicate rows.
- Each row of this table indicates the date, units, and product_id of each product sold. 
 

Write a solution to find the average selling price for each product. average_price should be rounded to 2 decimal places. If a product does not have any sold units, its average selling price is assumed to be 0.

Return the result table in any order.

The result format is in the following example.

 

#### Example 1:

Input:

Prices table:
```
+------------+------------+------------+--------+
| product_id | start_date | end_date   | price  |
+------------+------------+------------+--------+
| 1          | 2019-02-17 | 2019-02-28 | 5      |
| 1          | 2019-03-01 | 2019-03-22 | 20     |
| 2          | 2019-02-01 | 2019-02-20 | 15     |
| 2          | 2019-02-21 | 2019-03-31 | 30     |
+------------+------------+------------+--------+
```
UnitsSold table:
```
+------------+---------------+-------+
| product_id | purchase_date | units |
+------------+---------------+-------+
| 1          | 2019-02-25    | 100   |
| 1          | 2019-03-01    | 15    |
| 2          | 2019-02-10    | 200   |
| 2          | 2019-03-22    | 30    |
+------------+---------------+-------+
```
Output: 
```
+------------+---------------+
| product_id | average_price |
+------------+---------------+
| 1          | 6.96          |
| 2          | 16.96         |
+------------+---------------+
```
Explanation: 
- Average selling price = Total Price of Product / Number of products sold.
Average selling price for product 1 = ((100 * 5) + (15 * 20)) / 115 = 6.96
Average selling price for product 2 = ((200 * 15) + (30 * 30)) / 230 = 16.96

---

### [Solution](#solution)

Here is the SQL solution for the problem:

### SQL Query:
```sql
SELECT 
    p.product_id,
    IFNULL(ROUND(SUM(u.units * p.price) / SUM(u.units), 2), 0) AS average_price
FROM 
    UnitsSold u
JOIN 
    Prices p
ON 
    u.product_id = p.product_id 
    AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY 
    p.product_id;
```

---

#### Explanation:
1. **`JOIN`**: Combines `UnitsSold` and `Prices` tables based on `product_id` and ensures the `purchase_date` falls within the `start_date` and `end_date` of the `Prices` table.
2. **`u.units * p.price`**: Calculates the total price for each transaction (units sold multiplied by price).
3. **`SUM(u.units * p.price)`**: Computes the total revenue for a product.
4. **`SUM(u.units)`**: Computes the total number of units sold for a product.
5. **`ROUND(SUM(u.units * p.price) / SUM(u.units), 2)`**: Calculates the average selling price for each product, rounded to 2 decimal places.
6. **`GROUP BY p.product_id`**: Groups the results by each product to calculate the average price for each product.

---

#### Output:
For the provided input:
```
+------------+---------------+
| product_id | average_price |
+------------+---------------+
| 1          | 6.96          |
| 2          | 16.96         |
+------------+---------------+
```


---
