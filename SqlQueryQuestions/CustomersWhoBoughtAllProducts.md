# [Customers Who Bought All Products](#customers-who-bought-all-products)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Customer (customer_id int, product_key int)
Create table Product (product_key int)
Truncate table Customer
insert into Customer (customer_id, product_key) values ('1', '5')
insert into Customer (customer_id, product_key) values ('2', '6')
insert into Customer (customer_id, product_key) values ('3', '5')
insert into Customer (customer_id, product_key) values ('3', '6')
insert into Customer (customer_id, product_key) values ('1', '6')
Truncate table Product
insert into Product (product_key) values ('5')
insert into Product (product_key) values ('6')
```

---

### [Question](#question)

Table: Customer
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| customer_id | int     |
| product_key | int     |
+-------------+---------+
```
- This table may contain duplicates rows. 
- **customer_id** is not NULL.
- **product_key** is a foreign key (reference column) to Product table.
 

Table: Product
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_key | int     |
+-------------+---------+
```
- **product_key** is the primary key (column with unique values) for this table.
 

Write a solution to report the customer ids from the Customer table that bought all the products in the Product table.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 

Customer table:
```
+-------------+-------------+
| customer_id | product_key |
+-------------+-------------+
| 1           | 5           |
| 2           | 6           |
| 3           | 5           |
| 3           | 6           |
| 1           | 6           |
+-------------+-------------+
```
Product table:
```
+-------------+
| product_key |
+-------------+
| 5           |
| 6           |
+-------------+
```
Output:
```
+-------------+
| customer_id |
+-------------+
| 1           |
| 3           |
+-------------+
```
Explanation: 
The customers who bought all the products (5 and 6) are customers with IDs 1 and 3.

---

### [Solution](#solution)
#### SQL Solution

```sql
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(*) FROM Product);
```

---

#### Explanation

1. **Group Customers by Product**:
   - `GROUP BY customer_id`: Groups rows by `customer_id`.

2. **Count Distinct Products Bought**:
   - `COUNT(DISTINCT product_key)`: Counts the unique products bought by each customer.

3. **Compare with Total Products**:
   - `(SELECT COUNT(*) FROM Product)`: Counts the total number of products in the `Product` table.
   - The `HAVING` clause ensures we only select customers who bought all products.

4. **Result**:
   - Returns `customer_id` for customers who bought all products.

---

#### Input Example

**Customer Table**:
```
+-------------+-------------+
| customer_id | product_key |
+-------------+-------------+
| 1           | 5           |
| 2           | 6           |
| 3           | 5           |
| 3           | 6           |
| 1           | 6           |
+-------------+-------------+
```

**Product Table**:
```
+-------------+
| product_key |
+-------------+
| 5           |
| 6           |
+-------------+
```

**Query Result**:
```
+-------------+
| customer_id |
+-------------+
| 1           |
| 3           |
+-------------+
```

---

#### Explanation of Output

1. **Customer 1**:
   - Bought products: 5, 6 (all products).
   - Included in the result.

2. **Customer 2**:
   - Bought products: 6 (not all products).
   - Excluded from the result.

3. **Customer 3**:
   - Bought products: 5, 6 (all products).
   - Included in the result.

---
