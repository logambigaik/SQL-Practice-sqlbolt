
-- Start by getting a feel for the stream table and the chat table.Select the first 20 rows from each of the two tables.
-- What are the column names?
```sql
SELECT s.*, c.*
FROM stream s, chat c
LIMIT 2;
```
<img src="https://github.com/user-attachments/assets/f20de354-c919-45c6-90f5-966869d2b54f" width=400>



-- What are the unique channels in the stream table?
```sql
SELECT DISTINCT c.channel
FROM chat c;

-- What are the unique channels in the stream table?
```sql
SELECT DISTINCT s.game
FROM stream s;
```
<img src="https://github.com/user-attachments/assets/a63382b0-d605-437e-aabf-5fdf3bb32234" width=450>


-- What are the most popular games in the stream table?

-- Create a list of games and their number of viewers using GROUP BY.
```sql
SELECT game, COUNT(*)
FROM stream
GROUP BY game
ORDER BY COUNT(*) DESC;
```
<img src="https://github.com/user-attachments/assets/6c444988-2101-467e-8c95-fe998070811b" width=450>


-- Create a list of countries and their number of LoL viewers using WHERE and GROUP BY.
```sql
SELECT country, COUNT(*)
FROM stream
WHERE game = 'League of Legends'
GROUP BY 1
ORDER BY 2 DESC;
```

<img src="https://github.com/user-attachments/assets/a2504d76-9423-400f-af8a-86bf6522b181" width=450>

-- The player column contains the source the user is using to view the stream (site, iphone, android, etc).

-- Create a list of players and their number of streamers.
```sql
SELECT player, COUNT(*)
FROM stream
GROUP BY 1
ORDER BY 2 DESC;
```
<img src="https://github.com/user-attachments/assets/fe3b19b1-f5ef-4095-a4fa-acc243e52250" width=450>

-- Create a new column named genre for each of the games.

-- Group the games into their genres: Multiplayer Online Battle Arena (MOBA), First Person Shooter (FPS), Survival, and Other.

-- Using CASE, your logic should be:

-- If League of Legends → MOBA
-- If Dota 2 → MOBA
-- If Heroes of the Storm → MOBA
-- If Counter-Strike: Global Offensive → FPS
-- If DayZ → Survival
-- If ARK: Survival Evolved → Survival
-- Else → Other
-- Use GROUP BY and ORDER BY to showcase only the unique game titles.

```sql
SELECT game,
 CASE
  WHEN game = 'Dota 2'
      THEN 'MOBA'
  WHEN game = 'League of Legends' 
      THEN 'MOBA'
  WHEN game = 'Heroes of the Storm'
      THEN 'MOBA'
    WHEN game = 'Counter-Strike: Global Offensive'
      THEN 'FPS'
    WHEN game = 'DayZ'
      THEN 'Survival'
    WHEN game = 'ARK: Survival Evolved'
      THEN 'Survival'
  ELSE 'Other'
  END AS 'genre',
  COUNT(*)
FROM stream
GROUP BY 1
ORDER BY 3 DESC;
```
<img src="https://github.com/user-attachments/assets/6a1bb505-d039-472a-8521-413e722b1a39" width=220>

-- 8.Before we get started, let’s run this query and take a look at the time column from the stream table:

```sql
SELECT time
FROM stream
LIMIT 10;
```
<img src="https://github.com/user-attachments/assets/63c0a4cf-1a3a-4be0-bb4d-df92d7c737d3" width=220>


-- SQLite comes with a strftime() function - a very powerful function that allows you to return a formatted date.

-- It takes two arguments:

-- strftime(format, column)

-- Let’s test this function out:

```sql
SELECT time,
   strftime('%S', time)
FROM stream
GROUP BY 1
LIMIT 20;
```
<img src="https://github.com/user-attachments/assets/5a997d7f-7482-4032-9563-d15032203359" width=450>

-- now we understand how strftime() works. Let’s write a query that returns two columns:

-- The hours of the time column
-- The view count for each hour
-- Lastly, filter the result with only the users in your country using a WHERE clause.

```sql
SELECT strftime('%H', time),
   COUNT(*)
FROM stream
GROUP BY 1;
```
<img src="https://github.com/user-attachments/assets/cade2453-0dce-4266-9b5c-29f5cae47818" width=450>


-- The stream table and the chat table share a column: device_id.

-- Let’s join the two tables on that column.

```sql
SELECT *
FROM stream
JOIN chat
  ON stream.device_id = chat.device_id;
```
<img src="https://github.com/user-attachments/assets/62038620-9cd1-4ac6-a761-5e85e6d4e42b" width=450>

-- What are your favorite games? Can you find some insights about its viewers and chat room users?
-- Is there anything you can do after joining the two tables?

```sql
SELECT c.game,
       COUNT(DISTINCT c.device_id) AS chat_users,
       COUNT(c.player) AS total_player
FROM stream v
JOIN chat c ON v.device_id = c.device_id
WHERE DATE(v.time) = DATE(c.time) 
GROUP BY c.game
ORDER BY total_player DESC;
```
<img src="https://github.com/user-attachments/assets/4ff20567-ece0-41f8-92b3-d610ca7f53ea" width=500>

