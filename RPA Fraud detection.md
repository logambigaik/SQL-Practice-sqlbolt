```sql
-- 1
-- What are the column names?

SELECT *
FROM transaction_data
LIMIT 10;

-- 2
-- Find the full_names and emails
-- of the transactions listing 20252 as the zip code.
SELECT full_name, email, zip
FROM transaction_data
WHERE zip = 20252;


-- 3
-- Use a query to find the names 
-- and emails associated with these transactions.
SELECT full_name, email
FROM transaction_data
WHERE full_name = 'Art Vandelay'
   OR full_name LIKE '% der %';


-- 4
-- Find the ip_addresses and emails listed with these transactions.
SELECT ip_address, email
FROM transaction_data
WHERE ip_address LIKE '10.%';


-- 5
-- Find the emails in transaction_data with
-- ‘temp_email.com’ as a domain.
SELECT email
FROM transaction_data
WHERE email LIKE '%temp_email.com';


-- 6
-- The finance department is looking for a specific transaction. 
-- They know that the transaction occurred from an ip address starting 
-- with ‘120.’ and their full name starts with ‘John’.
SELECT *
FROM transaction_data
WHERE full_name LIKE 'John%'
  AND ip_address LIKE '120.%';

-- Can you find the transaction?


-- 7
-- Challenge
-- Return only those customers residing in GA. Use the list of ZIP CODE prefixes
-- (https://en.wikipedia.org/wiki/List_of_ZIP_Code_prefixes)
-- to determine the best query for zip codes belonging to Georgia(GA).
SELECT DISTINCT full_name
FROM transaction_data
WHERE 
    (zip BETWEEN 30000 AND 31999 OR
     zip BETWEEN 39800 AND 39899);
```


#### RPA Customer Segmentation

```sql
-- 1
-- What are the column names?
SELECT *
FROM users
LIMIT 20;

 
-- 2
-- Find the email addresses and birthdays of users whose 
-- birthday is between 1980-01-01 and 1989-12-31.
SELECT email, birthday
FROM users
WHERE birthday BETWEEN '1980-01-01' AND '1989-12-31';
SELECT email, birthday
FROM users
WHERE birthday >= '1980-01-01'
  AND birthday <= '1989-12-31';


   
-- 3
-- Find the emails and creation date of users 
-- whose created_at date matches this condition.
SELECT email, created_at
FROM users
WHERE created_at < '2017-05-01';

SELECT email, created_at
FROM users
WHERE created_at <= '2017-04-30';


-- 4
-- Find the emails of the users who received the ‘bears’ test.
SELECT email
FROM users
WHERE test = 'bears';


-- 5
-- Find all the emails of all users who 
-- received a campaign on website BBB.

SELECT email
FROM users
WHERE campaign LIKE 'BBB%';

-- 6
-- Find all the emails of all users who received ad copy 2 in 
-- their campaign.
SELECT email
FROM users
WHERE campaign LIKE '%-2';


-- 7
-- Find the emails for all users who received both a campaign and a test. 
-- These users will have non-empty entries in the 
-- campaign and test columns.

SELECT email
FROM users
WHERE campaign IS NOT NULL
  AND test IS NOT NULL;

-- 8
-- Challenge
-- One of the members of the marketing team had an idea of calculating
-- how old users were when they signed up.
SELECT CURRENT_DATE - birthday AS customer_age
FROM users
WHERE birthday IS NOT NULL;
```


#### 
```sql
-- 1
-- What are the column names?
-- SELECT *
-- FROM orders
-- LIMIT 10;


-- 2 
-- How recent is this data?
-- SELECT DISTINCT order_date
-- FROM orders;
-- SELECT DISTINCT order_date
-- FROM orders
-- ORDER BY order_date DESC;


-- 3
-- Instead of selecting all the columns using *, 
-- write a query that selects only the special_instructions column.

-- Limit the result to 20 rows.
-- SELECT special_instructions
-- FROM orders
-- LIMIT 20;



-- 4 
-- Can you edit the query so that we are only 
-- returning the special instructions that are not empty?

-- SELECT special_instructions
-- FROM orders
-- WHERE special_instructions IS NOT NULL
-- ORDER BY special_instructions;
-- 5
-- Let’s go even further and sort the instructions 
-- in alphabetical order (A-Z).
-- SELECT special_instructions
-- FROM orders
-- WHERE special_instructions IS NOT NULL
-- ORDER BY special_instructions;

-- SELECT special_instructions
-- FROM orders
-- WHERE special_instructions IS NOT NULL
-- ORDER BY special_instructions ASC;

-- 6
-- Let’s search for special instructions that have the word ‘sauce’.

-- Are there any funny or interesting ones? 
-- SELECT special_instructions
-- FROM orders
-- WHERE special_instructions LIKE '%sauce%';


-- 7
-- Let’s search for special instructions that have the word ‘door’.
-- Any funny or interesting ones?

-- SELECT special_instructions
-- FROM orders
-- WHERE special_instructions LIKE '%door%';

-- 8
-- Let’s search for special instructions that have the word ‘box’.
-- Any funny or interesting ones?
-- SELECT special_instructions
-- FROM orders
-- WHERE special_instructions LIKE '%box%';


-- 9
-- Instead of just returning the special instructions, also return their order ids.

-- For more readability:
-- Rename id as ‘#’
-- Rename special_instructions as ‘Notes’
-- SELECT id,
--    special_instructions
-- FROM orders
-- WHERE special_instructions LIKE '%box%';
-- SELECT id AS '#',
--    special_instructions AS 'Notes'
-- FROM orders
-- WHERE special_instructions LIKE '%box%';


-- 10
-- Challenge
-- They have asked you to query the customer who made the phrase. 
-- Return the item_name restaurant_id, and user_id for the person created the phrase.

SELECT 
    user_id, 
    item_name, 
    restaurant_id, 
    special_instructions AS "Notes"
FROM orders
WHERE special_instructions LIKE '%phrase%';


```
