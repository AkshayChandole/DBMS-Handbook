# [Immediate Food Delivery II](#immediate-food-delivery-ii)

### [SQL Schema](#sql-schema)
```sql
Create table If Not Exists Delivery (delivery_id int, customer_id int, order_date date, customer_pref_delivery_date date)
Truncate table Delivery
insert into Delivery (delivery_id, customer_id, order_date, customer_pref_delivery_date) values ('1', '1', '2019-08-01', '2019-08-02')
insert into Delivery (delivery_id, customer_id, order_date, customer_pref_delivery_date) values ('2', '2', '2019-08-02', '2019-08-02')
insert into Delivery (delivery_id, customer_id, order_date, customer_pref_delivery_date) values ('3', '1', '2019-08-11', '2019-08-12')
insert into Delivery (delivery_id, customer_id, order_date, customer_pref_delivery_date) values ('4', '3', '2019-08-24', '2019-08-24')
insert into Delivery (delivery_id, customer_id, order_date, customer_pref_delivery_date) values ('5', '3', '2019-08-21', '2019-08-22')
insert into Delivery (delivery_id, customer_id, order_date, customer_pref_delivery_date) values ('6', '2', '2019-08-11', '2019-08-13')
insert into Delivery (delivery_id, customer_id, order_date, customer_pref_delivery_date) values ('7', '4', '2019-08-09', '2019-08-09')
```

---

### [Question](#question)

Table: Delivery
```
+-----------------------------+---------+
| Column Name                 | Type    |
+-----------------------------+---------+
| delivery_id                 | int     |
| customer_id                 | int     |
| order_date                  | date    |
| customer_pref_delivery_date | date    |
+-----------------------------+---------+
```
- **delivery_id** is the column of unique values of this table.
- The table holds information about food delivery to customers that make orders at some date and specify a preferred delivery date (on the same order date or after it).
 

If the customer's preferred delivery date is the same as the order date, then the order is called immediate; otherwise, it is called scheduled.

The first order of a customer is the order with the earliest order date that the customer made. It is guaranteed that a customer has precisely one first order.

Write a solution to find the percentage of immediate orders in the first orders of all customers, rounded to 2 decimal places.

The result format is in the following example.

 

#### Example 1:

Input: 

Delivery table:
```
+-------------+-------------+------------+-----------------------------+
| delivery_id | customer_id | order_date | customer_pref_delivery_date |
+-------------+-------------+------------+-----------------------------+
| 1           | 1           | 2019-08-01 | 2019-08-02                  |
| 2           | 2           | 2019-08-02 | 2019-08-02                  |
| 3           | 1           | 2019-08-11 | 2019-08-12                  |
| 4           | 3           | 2019-08-24 | 2019-08-24                  |
| 5           | 3           | 2019-08-21 | 2019-08-22                  |
| 6           | 2           | 2019-08-11 | 2019-08-13                  |
| 7           | 4           | 2019-08-09 | 2019-08-09                  |
+-------------+-------------+------------+-----------------------------+
```
Output: 
```
+----------------------+
| immediate_percentage |
+----------------------+
| 50.00                |
+----------------------+
```
Explanation: 
- The customer id 1 has a first order with delivery id 1 and it is scheduled.
- The customer id 2 has a first order with delivery id 2 and it is immediate.
- The customer id 3 has a first order with delivery id 5 and it is scheduled.
- The customer id 4 has a first order with delivery id 7 and it is immediate.
- Hence, half the customers have immediate first orders.

---

### [Solution](#solution)


```sql
SELECT 
    ROUND(
        SUM(CASE WHEN order_date = customer_pref_delivery_date THEN 1 ELSE 0 END) * 100.0 
        / COUNT(DISTINCT customer_id), 
        2
    ) AS immediate_percentage
FROM 
    Delivery
WHERE 
    (customer_id, order_date) IN (
        SELECT customer_id, MIN(order_date)
        FROM Delivery
        GROUP BY customer_id
    );
```

---

#### Explanation:

1. **Subquery**:
   - Identifies the first order for each customer (`MIN(order_date)` grouped by `customer_id`).

2. **Main Query**:
   - Filters rows where `(customer_id, order_date)` matches the first order.
   - Uses a `CASE` statement to count immediate first orders (`order_date = customer_pref_delivery_date`).
   - Divides this count by the total number of customers (`COUNT(DISTINCT customer_id)`).

3. **ROUND**:
   - Calculates the percentage and rounds it to two decimal places.

---

#### Output:
The query produces the same output as before:
```
+----------------------+
| immediate_percentage |
+----------------------+
| 50.00                |
+----------------------+
```

---
