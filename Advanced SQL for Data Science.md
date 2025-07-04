## Advanced SQL for Data Science.md
* GROUP BY and SUM reduce the number of rows in your query results because they are combining or grouping rows.
* Window functions, on the other hand, allow you to maintain the values of your original table while displaying grouped or summative information alongside in another column.

```sql
SELECT username, SUM(change_in_followers) AS total_followers_change
FROM social_media
WHERE username = 'instagram'
GROUP BY username;
```
<img src="https://github.com/user-attachments/assets/ecb7d4f3-2cca-45ae-837b-7ea5156f641b" width=220 />


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
<img src="https://github.com/user-attachments/assets/46a86d59-3ff4-4d94-9204-63f6b17b6920" width=220 />


### Above query with windows function:

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
<img src="https://github.com/user-attachments/assets/46a86d59-3ff4-4d94-9204-63f6b17b6920" width=220 />

### Window Function Syntax 
```sql
SELECT month,
  change_in_followers,
  SUM(change_in_followers) OVER (
    ORDER BY month
  ) AS running_total
FROM social_media
WHERE username = 'instagram';
```
- SELECT month, change_in_followers: Same as usual, selecting the columns.
- SUM(change_in_followers): Here is our aggregate function to find the SUM of our chosen column.
>> OVER: This is the clause that designates SUM as a window function.

>> ORDER BY month: Here we declare what we would like our window function to do.

>> Then name the running total column 'running_total'.

<b>This window function is taking the sum of followers for each month.So for each month, the window function adds the current month’s change_in_followers to our running total.</b>

- And lastly, this is all coming from table social_media where the username is instagram.


### AVG, SUM and count with windows functions

```sql
SELECT month,
  change_in_followers,
  SUM(change_in_followers) OVER (
    ORDER BY month
  ) AS running_total,
  AVG(change_in_followers) OVER(
    ORDER BY month
  )AS running_avg,
  COUNT(change_in_followers) OVER(
    ORDER BY month
  )AS runing_count
FROM social_media
WHERE username = 'instagram';
```
<img src="https://github.com/user-attachments/assets/f954ef2c-173e-4950-877b-da99ce39ece9" width=220/>

### PARTITION BY:

```sql
SELECT username, month,
    change_in_followers,
    SUM(change_in_followers) 
    OVER(PARTITION BY username 
    ORDER BY month) 
    'running_total_follower_change'
FROM social_media;
```
<img src="https://github.com/user-attachments/assets/c954584d-3876-47fa-8523-fdd1260527ff" width=220>

