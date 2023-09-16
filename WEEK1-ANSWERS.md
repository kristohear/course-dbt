# Analytics engineering with dbt

## Week 1 Answers

### How many users we have?

`SELECT COUNT(*) 
FROM {{ ref('stg_users') }};`


### On average, how many orders do we receive per hour?

`SELECT COUNT(*) 
FROM {{ ref('stg_users') }};`


### On average, how long does an order take from being placed to being delivered?

`SELECT COUNT(*) 
FROM {{ ref('stg_users') }};`


### How many users have only made one purchase? Two purchases? Three+ purchases?

`SELECT COUNT(*) 
FROM {{ ref('stg_users') }};`


### On average, how many unique sessions do we have per hour?

`SELECT COUNT(*) 
FROM {{ ref('stg_users') }};`