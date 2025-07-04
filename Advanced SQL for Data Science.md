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

```sql
SELECT username, month,
    -- change_in_followers,
    SUM(change_in_followers) 
    OVER(PARTITION BY username 
    ORDER BY month) 
    'running_total_follower_change',
   AVG(change_in_followers) 
    OVER(PARTITION BY username 
    ORDER BY month) 
    'running_avg_follower_change' 
FROM social_media
where username='instagram'
;
```

<img src="https://github.com/user-attachments/assets/639b717d-37f5-42e1-bd8f-8d12b0a61983" width=220 />

### FIRST_VALUE and LAST_VALUE in Windows function:
- FIRST_VALUE() returns the first value in an ordered set of values.
- LAST_VALUE() returns the last value in an ordered set of values.

```sql
SELECT username, 
month,
FIRST_VALUE(posts) 
OVER (PARTITION BY username 
ORDER BY posts RANGE BETWEEN UNBOUNDED PRECEDING AND 
      UNBOUNDED FOLLOWING)
 AS fewest_posts,
 LAST_VALUE(posts) 
OVER (PARTITION BY username 
ORDER BY posts
RANGE BETWEEN UNBOUNDED PRECEDING AND 
      UNBOUNDED FOLLOWING)
 AS most_posts
FROM social_media
WHERE username = 'instagram'
group by month,username 
;
```
<img src="https://github.com/user-attachments/assets/5f1ea509-d096-42e2-bb48-4da72563f042" width=220 />


### LAG or LEAD

-  can use in order to access information from a row at a specified offset which comes before (LAG) or after (LEAD) the current row.
- This means that by using LAG or LEAD you can access any row before or after the current row, which can be very useful in calculating the difference between the current and adjacent row.

```sql
SELECT artist,
   week,
   streams_millions,
   LAG(streams_millions, 1, 0) 
   OVER (ORDER BY week ) previous_week_streams 
FROM streams 
WHERE artist = 'Lady Gaga';
```
<img src="https://github.com/user-attachments/assets/dff3990a-c13b-4e8b-bad0-5d6dfc0579a9" width=220 />

##### LAG takes up to three arguments:

 * column (required)
 * offset (optional, default 1 row offset)
 * default (optional, what to replace default null values with)
 
```sql
SELECT
   artist,
   week,
   streams_millions,
   streams_millions - LAG(streams_millions, 1, streams_millions) OVER ( 
      ORDER BY week 
   ) streams_millions_change
FROM
   streams 
WHERE
   artist = 'Lady Gaga';
```
<img src="https://github.com/user-attachments/assets/7b5396b6-aa13-4cbf-a5ba-d4e388d2ebfb" width=220 />


