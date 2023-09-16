# Analytics engineering with dbt

## Week 1 Answers

### How many users we have?

```sql
SELECT COUNT(*) 
FROM STG_USERS;
```
#### 130

### On average, how many orders do we receive per hour?

```sql
SELECT AVG(order_count) AS avg_orders_per_hour 
FROM (
    SELECT DATE_TRUNC('HOUR', created_at) AS order_hour, 
           COUNT(*) AS order_count 
    FROM STG_ORDERS
    GROUP BY order_hour
);
```
#### 7.520833

### On average, how long does an order take from being placed to being delivered?

```sql
SELECT AVG(DATEDIFF('HOUR', created_at, delivered_at)) AS avg_hours_to_delivery 
FROM STG_ORDERS
WHERE delivered_at IS NOT NULL;
```
#### 93.403279


### How many users have only made one purchase? Two purchases? Three+ purchases?

```sql
WITH user_purchases AS (
    SELECT user_id, COUNT(*) AS purchase_count 
    FROM STG_ORDERS
    GROUP BY user_id
)

SELECT 
    SUM(CASE WHEN purchase_count = 1 THEN 1 ELSE 0 END) AS one_purchase,
    SUM(CASE WHEN purchase_count = 2 THEN 1 ELSE 0 END) AS two_purchases,
    SUM(CASE WHEN purchase_count >= 3 THEN 1 ELSE 0 END) AS three_or_more_purchases
FROM user_purchases;
```
#### One: 25, Two: 28, Three+: 71


### On average, how many unique sessions do we have per hour?

```sql
SELECT AVG(session_count) AS avg_sessions_per_hour 
FROM (
    SELECT DATE_TRUNC('HOUR', created_at) AS session_hour, 
           COUNT(DISTINCT session_id) AS session_count 
    FROM STG_EVENTS
    GROUP BY session_hour
);
```
#### 16.327586