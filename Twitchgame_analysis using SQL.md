
-- Start by getting a feel for the stream table and the chat table.Select the first 20 rows from each of the two tables.
-- What are the column names?
```sql
SELECT s.*, c.*
FROM stream s, chat c
LIMIT 2;

-- What are the unique channels in the stream table?
```sql
SELECT DISTINCT c.channel
FROM chat c;

-- What are the unique channels in the stream table?
```sql
SELECT DISTINCT s.game
FROM stream s;

-- What are the most popular games in the stream table?

-- Create a list of games and their number of viewers using GROUP BY.
```sql
SELECT game, COUNT(*)
FROM stream
GROUP BY game
ORDER BY COUNT(*) DESC;

-- Create a list of countries and their number of LoL viewers using WHERE and GROUP BY.
```sql
SELECT country, COUNT(*)
FROM stream
WHERE game = 'League of Legends'
GROUP BY 1
ORDER BY 2 DESC;

-- The player column contains the source the user is using to view the stream (site, iphone, android, etc).

-- Create a list of players and their number of streamers.
```sql
SELECT player, COUNT(*)
FROM stream
GROUP BY 1
ORDER BY 2 DESC;

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

-- 8.Before we get started, let’s run this query and take a look at the time column from the stream table:

```sql
SELECT time
FROM stream
LIMIT 10;


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


-- now we understand how strftime() works. Let’s write a query that returns two columns:

-- The hours of the time column
-- The view count for each hour
-- Lastly, filter the result with only the users in your country using a WHERE clause.

```sql
SELECT strftime('%H', time),
   COUNT(*)
FROM stream
GROUP BY 1;



-- The stream table and the chat table share a column: device_id.

-- Let’s join the two tables on that column.

```sql
SELECT *
FROM stream
JOIN chat
  ON stream.device_id = chat.device_id;


-- What are your favorite games? Can you find some insights about its viewers and chat room users?
-- Is there anything you can do after joining the two tables?

```sql
SELECT v.game,
       COUNT(DISTINCT c.user_id) AS chat_users,
       COUNT(c.message) AS total_messages
FROM views v
JOIN chat c ON v.user_id = c.user_id
WHERE v.timestamp::date = c.timestamp::date  -- optional, for same-day activity
GROUP BY v.game
ORDER BY total_messages DESC;

