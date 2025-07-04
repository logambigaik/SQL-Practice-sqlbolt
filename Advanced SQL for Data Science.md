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


```sql
SELECT
   artist,
   week,
   streams_millions,
   streams_millions - LAG(streams_millions, 1, streams_millions) OVER ( 
      PARTITION BY artist
      ORDER BY week 
   ) AS 'streams_millions_change',
   chart_position,
   LAG(chart_position, 1, chart_position) OVER ( 
      PARTITION BY artist
      ORDER BY week 
) - chart_position AS 'chart_position_change'
FROM
   streams
WHERE 
   artist = 'Lady Gaga';
```
<img src="https://github.com/user-attachments/assets/1302e651-0c90-45a4-ba75-e26fff04af45" width=220 />

#### LEAD
LAG to examine how an artist’s weekly streams and chart position changed. Your final query should have looked something like this:

```sql
SELECT
   artist,
   week,
   streams_millions,
   streams_millions - LAG(streams_millions, 1, streams_millions) OVER ( 
      PARTITION BY artist
      ORDER BY week 
   ) AS 'streams_millions_change',
   chart_position,
   LAG(chart_position, 1, chart_position) OVER ( 
      PARTITION BY artist
      ORDER BY week 
) - chart_position AS 'chart_position_change'
FROM
   streams
WHERE 
   artist = 'Lady Gaga';
```

We are going to modify this to use LEAD, which looks to future rows, instead of LAG, which looks to previous rows.

```SQL
SELECT
   artist,
   week,
   streams_millions,
   LEAD(streams_millions, 1) OVER (
      PARTITION BY artist
      ORDER BY week
   ) - streams_millions AS 'streams_millions_change',
   chart_position,
   chart_position - LEAD(chart_position, 1) OVER ( 
      PARTITION BY artist
      ORDER BY week 
) AS 'chart_position_change'
FROM
   streams;
```

<img src="https://github.com/user-attachments/assets/e07eefd9-b221-457c-b2e1-a12425527330" width=220 />


#### ROW_NUMBER

* The most straight-forward way to order our results is by using the ROW_NUMBER function which adds a sequential integer number to each row.

* Adding a ROW_NUMBER to each row can be useful for seeing where in your result set the row falls.

```sql
SELECT 
   ROW_NUMBER() OVER (
      ORDER BY streams_millions
   ) AS 'row_num', 
   artist, 
   week,
   streams_millions
FROM
   streams;
```
<img src="https://github.com/user-attachments/assets/89de8813-6cb9-4936-b8a4-86b28c2d798e" width=220 />

<B>What happens for the row_num when the streams_millions are equal?</B>

<I>ANSWER: The rows continue to increase even when the values are equal because it is solely based on the number of the row in our results set.

An example of this is when 'row_num' is 7 and 8 and the streams_million is 26.3 for both.</I>

#### Modify our query to return the results with row_num = 1 being the most streams.

(Add DESC after our ORDER BY statement. Descending returns the values from highest to lowest.)

```sql
SELECT 
   ROW_NUMBER() OVER (
      ORDER BY streams_millions DESC
   ) AS 'row_num', 
   artist, 
   week,
   streams_millions
FROM
   streams;
```
<img src="https://github.com/user-attachments/assets/dd97db19-368a-4c90-b1eb-88211958b3cf" width=220 />

#### RANK
If you were to modify your ROW_NUMBER query to use RANK instead, it might appear to be exactly the same at first glance. But if you look more closely, you can see that RANK will follow standard ranking rules so that when two values are the same, they will have the same rank whereas with ROW_NUMBER they would not.

##### RANK with PARTITION :

```sql
SELECT 
   RANK() OVER (
      PARTITION BY week
      ORDER BY streams_millions DESC
   ) AS 'rank', 
   artist, 
   week,
   streams_millions
FROM
   streams;
```
<img src="https://github.com/user-attachments/assets/5ad323aa-a926-4b51-9177-31a9f615ac2b" width=220 />


#### NTILE
NTILE and this can be used to find quartiles, quintiles or whatever ntile your data-driven heart desires.

NTILE allows you to break your data into roughly equal groups, based on what ntile you’d like: so if you were using quartile, it would divide the data into four groups (quarters).

When using NTILE you are required to provide a bucket, which represents the number of groups you’d like your data broken down into: NTILE(4) would be four “buckets” which would represent quartiles.

```sql
SELECT 
   NTILE(5) OVER (
      ORDER BY streams_millions DESC
   ) AS 'weekly_streams_group', 
   artist, 
   week,
   streams_millions
FROM
   streams;
```

```sql
SELECT 
   NTILE(4) OVER (
      PARTITION BY week
      ORDER BY streams_millions DESC
   ) AS 'quartile', 
   artist, 
   week,
   streams_millions
FROM
   streams;
```
<img src='https://github.com/user-attachments/assets/16252b60-48a5-483d-b588-132331439125' width=220>




