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

####  Multiple Month: Determine Active Status
* is_active: if the subscription started before the given month and has not been canceled before the start of the given month

<img src="https://github.com/user-attachments/assets/f19f1dd2-768c-4b61-b085-e270b301856a" width=220 />

* Add a status temporary table. This table should have the following columns:

• id - selected from the cross_join table
• month - this is an alias of first_day from the cross_join table. We’re using the first day of the month to represent which month this data is for.
• is_active - 0 or 1, derive this column using a CASE WHEN statement
The is_active column should be 1 if the subscription_start is before the month’s first_day and if the subscription_end is either after the month’s first_day or is NULL.

```sql
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
cross_join AS
(SELECT *
FROM subscriptions
CROSS JOIN months),
status AS
(SELECT id, first_day as month,
CASE
  WHEN (subscription_start < first_day)
    AND (
      subscription_end > first_day
      OR subscription_end IS NULL
    ) THEN 1
  ELSE 0
END as is_active
FROM cross_join)

SELECT *
FROM status
LIMIT 100;
```
• Add an is_canceled column to the status temporary table. Ensure that it is equal to 1 in months containing the subscription_end and 0 otherwise.
• Derive this column using a CASE WHEN statement. You can use the BETWEEN function to check if a date falls between two others.

```sql
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
cross_join AS
(SELECT *
FROM subscriptions
CROSS JOIN months),
status AS
(SELECT id, first_day as month,
CASE
  WHEN (subscription_start < first_day)
    AND (
      subscription_end > first_day
      OR subscription_end IS NULL
    ) THEN 1
  ELSE 0
END as is_active,
CASE 
  WHEN subscription_end BETWEEN first_day AND last_day THEN 1
  ELSE 0
END as is_canceled
FROM cross_join)
SELECT *
FROM status
LIMIT 100;
```

<img src="https://github.com/user-attachments/assets/ccd82b0b-8146-4440-be20-11d22ec69d28" width=220 >

#### Multiple Month: Sum Active and Canceled Users

•• Add a status_aggregate temporary table. This table should have the following columns:

  • month - selected from the status table
  • active - the SUM() of active users for this month
  • canceled - the SUM() of canceled users for this month

```sql
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
cross_join AS
(SELECT *
FROM subscriptions
CROSS JOIN months),
status AS
(SELECT id, first_day as month,
CASE
  WHEN (subscription_start < first_day)
    AND (
      subscription_end > first_day
      OR subscription_end IS NULL
    ) THEN 1
  ELSE 0
END as is_active,
CASE 
  WHEN subscription_end BETWEEN first_day AND last_day THEN 1
  ELSE 0
END as is_canceled
FROM cross_join),
status_aggregate AS
(SELECT
  month,
  SUM(is_active) as active,
  SUM(is_canceled) as canceled
FROM status
GROUP BY month)
SELECT *
FROM status_aggregate;
```
<img src="https://github.com/user-attachments/assets/4f8b70ee-7c7b-446c-9326-b138f2c6b6e4" width=220>

#### Multiple Month: Churn Rate Calculation

• We use the number of canceled and active subscriptions to calculate churn for each month: 
** churn_rate = canceled / active **

• Add a SELECT statement to calculate the churn rate. The result should contain two columns:

    • month - selected from status_aggregate
    • churn_rate - calculated from status_aggregate.canceled and status_aggregate.active.

```sql
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
cross_join AS
(SELECT *
FROM subscriptions
CROSS JOIN months),
status AS
(SELECT id, first_day as month,
CASE
  WHEN (subscription_start < first_day)
    AND (
      subscription_end > first_day
      OR subscription_end IS NULL
    ) THEN 1
  ELSE 0
END as is_active,
CASE 
  WHEN subscription_end BETWEEN first_day AND last_day THEN 1
  ELSE 0
END as is_canceled
FROM cross_join),
status_aggregate AS
(SELECT
  month,
  SUM(is_active) as active,
  SUM(is_canceled) as canceled
FROM status
GROUP BY month)
SELECT
  month,
  1.0 * canceled/active AS churn_rate
FROM status_aggregate;
```
<img src="https://github.com/user-attachments/assets/a1642407-58a4-4a23-b2fa-3cec7c1570bd" width=220>



