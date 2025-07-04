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
- SELECT month, change_in_followers: Same as usual, selecting the columns.
- SUM(change_in_followers): Here is our aggregate function to find the SUM of our chosen column.
>> OVER: This is the clause that designates SUM as a window function.

>> ORDER BY month: Here we declare what we would like our window function to do.

>> This window function is taking the sum of followers for each month.

>> So for each month, the window function adds the current month’s change_in_followers to our running total.

>> Then name the running total column 'running_total'.
- And lastly, this is all coming from table social_media where the username is instagram.
