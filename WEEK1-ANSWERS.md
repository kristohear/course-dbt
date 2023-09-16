# Analytics engineering with dbt

## Week 1 Answers

### How many users we have?

```sql
SELECT COUNT(*) 
FROM {{ ref('stg_users') }};
```

### On average, how many orders do we receive per hour?

```sql
SELECT AVG(order_count) AS avg_orders_per_hour 
FROM (
    SELECT DATE_TRUNC('HOUR', created_at) AS order_hour, 
           COUNT(*) AS order_count 
    FROM {{ ref('stg_orders') }}
    GROUP BY order_hour
);
```


### On average, how long does an order take from being placed to being delivered?

```sql
SELECT AVG(DATEDIFF('HOUR', created_at, delivered_at)) AS avg_hours_to_delivery 
FROM {{ ref('stg_orders') }}
WHERE delivered_at IS NOT NULL;
```


### How many users have only made one purchase? Two purchases? Three+ purchases?

```sql
WITH user_purchases AS (
    SELECT user_id, COUNT(*) AS purchase_count 
    FROM {{ ref('stg_orders') }}
    GROUP BY user_id
)

SELECT 
    SUM(CASE WHEN purchase_count = 1 THEN 1 ELSE 0 END) AS one_purchase,
    SUM(CASE WHEN purchase_count = 2 THEN 1 ELSE 0 END) AS two_purchases,
    SUM(CASE WHEN purchase_count >= 3 THEN 1 ELSE 0 END) AS three_or_more_purchases
FROM user_purchases;
```


### On average, how many unique sessions do we have per hour?

```sql
SELECT AVG(session_count) AS avg_sessions_per_hour 
FROM (
    SELECT DATE_TRUNC('HOUR', created_at) AS session_hour, 
           COUNT(DISTINCT session_id) AS session_count 
    FROM {{ ref('stg_events') }}
    GROUP BY session_hour
);
```