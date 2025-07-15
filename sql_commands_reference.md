
# Common SQL Commands Reference

## ALTER TABLE
```sql
ALTER TABLE table_name 
ADD column_name datatype;
```
Adds a new column to an existing table.

---

## AND
```sql
SELECT column_name(s)
FROM table_name
WHERE column_1 = value_1
  AND column_2 = value_2;
```
Combines two conditions — both must be true.

---

## AS
```sql
SELECT column_name AS 'Alias'
FROM table_name;
```
Renames a column or table using an alias.

---

## AVG()
```sql
SELECT AVG(column_name)
FROM table_name;
```
Returns the average value of a numeric column.

---

## BETWEEN
```sql
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value_1 AND value_2;
```
Filters results within a range (inclusive).

---

## CASE
```sql
SELECT column_name,
  CASE
    WHEN condition THEN 'Result_1'
    WHEN condition THEN 'Result_2'
    ELSE 'Result_3'
  END
FROM table_name;
```
SQL’s way of handling if-then logic.

---

## COUNT()
```sql
SELECT COUNT(column_name)
FROM table_name;
```
Counts non-NULL rows in a column.

---

## CREATE TABLE
```sql
CREATE TABLE table_name (
  column_1 datatype, 
  column_2 datatype, 
  column_3 datatype
);
```
Creates a new table.

---

## DELETE
```sql
DELETE FROM table_name
WHERE some_column = some_value;
```
Removes rows from a table.

---

## GROUP BY
```sql
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name;
```
Groups rows that have the same values.

---

## HAVING
```sql
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name
HAVING COUNT(*) > value;
```
Filters groups after aggregation (unlike `WHERE`).

---

## INNER JOIN
```sql
SELECT column_name(s)
FROM table_1
JOIN table_2
  ON table_1.column_name = table_2.column_name;
```
Combines rows from two tables where the condition is met.

---

## INSERT
```sql
INSERT INTO table_name (column_1, column_2, column_3) 
VALUES (value_1, 'value_2', value_3);
```
Adds a new row to a table.

---

## IS NULL / IS NOT NULL
```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IS NULL;
```
Checks for NULL values in a column.

---

## LIKE
```sql
SELECT column_name(s)
FROM table_name
WHERE column_name LIKE pattern;
```
Searches for a specified pattern in a column.

---

## LIMIT
```sql
SELECT column_name(s)
FROM table_name
LIMIT number;
```
Limits the number of rows returned.

---

## MAX()
```sql
SELECT MAX(column_name)
FROM table_name;
```
Returns the largest value in a column.

---

## MIN()
```sql
SELECT MIN(column_name)
FROM table_name;
```
Returns the smallest value in a column.

---

## OR
```sql
SELECT column_name
FROM table_name
WHERE column_name = value_1
   OR column_name = value_2;
```
Returns rows where either condition is true.

---

## ORDER BY
```sql
SELECT column_name
FROM table_name
ORDER BY column_name ASC | DESC;
```
Sorts results in ascending or descending order.

---

## OUTER JOIN
```sql
SELECT column_name(s)
FROM table_1
LEFT JOIN table_2
  ON table_1.column_name = table_2.column_name;
```
Returns all rows from the left table; NULLs for unmatched rows from the right.

---

## ROUND()
```sql
SELECT ROUND(column_name, integer)
FROM table_name;
```
Rounds values to the specified number of decimal places.

---

## SELECT
```sql
SELECT column_name 
FROM table_name;
```
Fetches data from a database table.

---

## SELECT DISTINCT
```sql
SELECT DISTINCT column_name
FROM table_name;
```
Returns unique values in a column.

---

## SUM()
```sql
SELECT SUM(column_name)
FROM table_name;
```
Returns the total sum of a numeric column.

---

## UPDATE
```sql
UPDATE table_name
SET some_column = some_value
WHERE some_column = some_value;
```
Edits rows in a table.

---

## WHERE
```sql
SELECT column_name(s)
FROM table_name
WHERE column_name operator value;
```
Filters rows based on a condition.

---

## WITH (Common Table Expression - CTE)
```sql
WITH temporary_name AS (
   SELECT *
   FROM table_name
)
SELECT *
FROM temporary_name
WHERE column_name operator value;
```
Creates a temporary result set that can be referenced in a SELECT, INSERT, UPDATE, or DELETE statement.

---
