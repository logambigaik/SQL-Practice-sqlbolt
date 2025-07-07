#### What is Churn?
Churn rate is the percent of subscribers that have canceled within a certain period, usually a month. For a user base to grow, the churn rate must be less than the new subscriber rate 
for the same period.

To calculate the churn rate, we only will be considering users who are subscribed at the beginning of the month. The churn rate is the number of these users who cancel 
during the month divided by the total number:
 * Cancellation/total subscribers

For example, suppose you were analyzing data for a monthly video streaming service called CodeFlix. At the beginning of February, CodeFlix has 1,000 customers. In February, 250 of these customers cancel. The churn rate for February would be:
 * 250/1000 = 25% churn rate 

```SQL
SELECT 1.0 * 
(
  SELECT COUNT(*)
  FROM subscriptions
  WHERE subscription_start < '2016-12-01'
  AND (
    subscription_end
    BETWEEN '2016-12-01'
    AND '2016-12-31'
  )
) / (
  SELECT COUNT(*) 
  FROM subscriptions 
  WHERE subscription_start < '2016-12-01'
  AND (
    (subscription_end >= '2016-12-01')
    OR (subscription_end IS NULL)
  )
) 
AS result;
```

```SQL
SELECT 1.0 * 
(
  SELECT COUNT(*)
  FROM subscriptions
  WHERE subscription_start < '2017-01-01'
  AND (
    subscription_end
    BETWEEN '2017-01-01'
    AND '2017-01-31'
  )
) / (
  SELECT COUNT(*) 
  FROM subscriptions 
  WHERE subscription_start < '2017-01-01'
  AND (
    (subscription_end >= '2017-01-01')
    OR (subscription_end IS NULL)
  )
) 
AS result;
```

```sql
WITH enrollments AS 
(SELECT *
FROM subscriptions
WHERE subscription_start < '2017-01-01'
AND (
  (subscription_end >= '2017-01-01')
  OR (subscription_end IS NULL)
)),
status AS 
(SELECT 
CASE
  WHEN (subscription_end > '2017-01-31')
    OR (subscription_end IS NULL) THEN 0
  ELSE 1
  END as is_canceled,
CASE
  WHEN (subscription_start < '2017-01-01')
    AND (
      (subscription_end >= '2017-01-01')
        OR (subscription_end IS NULL)
    ) THEN 1
    ELSE 0
  END as is_active
FROM enrollments
)
SELECT 1.0 * SUM(is_canceled)/SUM(is_active) FROM status;
```
<img src="https://github.com/user-attachments/assets/1afdb029-cc01-43e5-8b43-0110fc897c11" width=220>

#### Multiple Month: Create Months Temporary Table
```sql
SELECT
  '2016-12-01' AS first_day,
  '2016-12-31' AS last_day
UNION
SELECT
  '2017-01-01' AS first_day,
  '2017-01-31' AS last_day;
```
<img src="https://github.com/user-attachments/assets/fe8cccc9-772b-4fad-95f4-314d485fb59c" width=220 >

* Create the months temporary table using WITH and SELECT everything from it so that you can see the structure.We need a table for January, February, and March of 2017.

``` SQL
WITH months as
(SELECT
  '2017-01-01' as first_day,
  '2017-01-31' as last_day
UNION
SELECT
  '2017-02-01' as first_day,
  '2017-02-28' as last_day
UNION
SELECT
  '2017-03-01' as first_day,
  '2017-03-31' as last_day
)
SELECT *
FROM months;
```
<img src="https://github.com/user-attachments/assets/8c0b8b85-cfe7-4968-b825-a524883e9f72" width=220>

#### Multiple Month: Cross Join Months and Users

* Create a cross_join temporary table that is a CROSS JOIN of subscriptions and months.
  
```SQL
WITH months AS
(SELECT
  '2017-01-01' as first_day,
  '2017-01-31' as last_day
UNION
SELECT
  '2017-02-01' as first_day,
  '2017-02-28' as last_day
UNION
SELECT
  '2017-03-01' as first_day,
  '2017-03-31' as last_day
),
-- Add temporary cross_join definition here
cross_join AS (
SELECT *
FROM subscriptions CROSS JOIN months)

SELECT *
FROM cross_join
LIMIT 100;
```

<IMG SRC="https://github.com/user-attachments/assets/4c69ce32-a968-4366-9e8f-ecd7202d9b7c" width=220 >


