# 🍜 Case Study #2: Pizza Runner

## Solution

***


### 1. How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)

````sql
SELECT CASE 
WHEN registration_date BETWEEN '2021-01-01' AND '2021-01-07' THEN '2021-01-01'
WHEN registration_date BETWEEN '2021-01-08' AND '2021-01-14' THEN '2021-01-08'
WHEN registration_date BETWEEN '2021-01-15' AND '2021-01-21' THEN '2021-01-15'
END AS registration_week,
COUNT(runner_id) as runner_signups
FROM pizza_runner.runners
GROUP BY registration_week
ORDER BY registration_week;
````

#### Answer:
| registration_week | runner_signups |
| ----------------  |  ------------  |           
|     2021-01-01    |       2        |
|     2021-01-08    |       1        |
|     2021-01-15    |       1        |


### 2. What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?

````sql
 SELECT customer_orders.order_id,runner_id,order_time,pickup_time, AVG(
              DATE_PART('minute', pickup_time::TIME - order_time::TIME)) AS time_diff
FROM pizza_runner.customer_orders
LEFT JOIN pizza_runner.runner_orders
ON customer_orders.order_id = runner_orders.order_id
WHERE pickup_time != 'null'
GROUP BY customer_orders.order_id,order_time,pickup_time,runner_id
ORDER BY customer_orders.order_id;
````

#### Answer:

| order_id | runner_id | order_time               | pickup_time         | time_diff |
| -------- | --------- | ------------------------ | ------------------- | --------- |
| 1        | 1         | 2020-01-01T18:05:02.000Z | 2020-01-01 18:15:34 | 10        |
| 2        | 1         | 2020-01-01T19:00:52.000Z | 2020-01-01 19:10:54 | 10        |
| 3        | 1         | 2020-01-02T23:51:23.000Z | 2020-01-03 00:12:37 | -38       |
| 4        | 2         | 2020-01-04T13:23:46.000Z | 2020-01-04 13:53:03 | 29        |
| 5        | 3         | 2020-01-08T21:00:29.000Z | 2020-01-08 21:10:57 | 10        |
| 7        | 2         | 2020-01-08T21:20:29.000Z | 2020-01-08 21:30:45 | 10        |
| 8        | 2         | 2020-01-09T23:54:33.000Z | 2020-01-10 00:15:02 | -39       |
| 10       | 1         | 2020-01-11T18:34:49.000Z | 2020-01-11 18:50:20 | 15        |


Note: For some reason date_part is failing if the date is different for order_time and pickup_time.




