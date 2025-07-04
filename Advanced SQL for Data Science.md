## Advanced SQL for Data Science.md
* GROUP BY and SUM reduce the number of rows in your query results because they are combining or grouping rows.
* Window functions, on the other hand, allow you to maintain the values of your original table while displaying grouped or summative information alongside in another column.

```sql
SELECT username, SUM(change_in_followers) AS total_followers_change
FROM social_media
WHERE username = 'instagram'
GROUP BY username;
```
<img src="https://github.com/user-attachments/assets/ecb7d4f3-2cca-45ae-837b-7ea5156f641b" width=120 />

### With windows function:

```sql
SELECT username, 
month,
SUM(change_in_followers) 
OVER (PARTITION BY month)
 AS total_followers_change
FROM social_media
WHERE username = 'instagram'
;
```
<img src="https://github.com/user-attachments/assets/46a86d59-3ff4-4d94-9204-63f6b17b6920" width=120 />

### GROUP BY with month and username

```sql
SELECT username, 
month,
SUM(change_in_followers) 
AS total_followers_change
FROM social_media
WHERE username = 'instagram'
group by month,username 
;
```
<img src="https://github.com/user-attachments/assets/46a86d59-3ff4-4d94-9204-63f6b17b6920" width=120 />

### Window Function Syntax
