# SQLBolt Lesson 1 – SELECT Queries 101

This repository contains my completed exercises from [SQLBolt Lesson 1](https://sqlbolt.com/lesson/select_queries_intro).  
It demonstrates foundational SQL querying skills including `SELECT`, `WHERE`, `BETWEEN`, `LIKE`, `JOIN`, `ORDER BY`, and more.

## 🧠 What I Learned

- Selecting specific columns from a table
- Filtering rows with `WHERE`
- Using `BETWEEN`, `LIKE`, `NOT`, and `OFFSET`
- Ordering and limiting results
- Performing JOIN operations
- Sorting results based on a column (e.g., rating)
- Handling duplicates with `DISTINCT`

---

## 📄 SQL Queries

<details>
<summary>Click to expand the full list of queries</summary>

```sql
-- Find the title of each film
SELECT Title FROM movies;

-- Find the director of each film
SELECT Director FROM movies;

-- Find the title and director of each film
SELECT Title, Director FROM movies;

-- Find the title and year of each film
SELECT Title, Year FROM movies;

-- Find all the information about each film
SELECT * FROM movies;

-- Find the movie with a row id of 6
SELECT * FROM movies WHERE Id = 6;

-- Find the movies released in the years between 2000 and 2010
SELECT * FROM movies WHERE Year BETWEEN 2000 AND 2010;

-- Find the movies not released in the years between 2000 and 2010
SELECT * FROM movies WHERE Year NOT BETWEEN 2000 AND 2010;

-- Find the first 5 Pixar movies and their release year
SELECT Title, Year FROM movies LIMIT 5;

-- Find all the Toy Story movies
SELECT * FROM movies WHERE Title LIKE 'Toy Story%';

-- Find all the movies directed by John Lasseter
SELECT * FROM movies WHERE Director = 'John Lasseter';

-- Find all the movies (and director) not directed by John Lasseter
SELECT * FROM movies WHERE Director <> 'John Lasseter';

-- Find all the WALL-* movies
SELECT * FROM movies WHERE Title LIKE 'WALL-%';

-- List all directors of Pixar movies (alphabetically), without duplicates
SELECT DISTINCT Director FROM movies;

-- List the last four Pixar movies released (most recent to least)
SELECT Title, Year FROM movies
ORDER BY Year DESC
LIMIT 4;

-- List the first five Pixar movies sorted alphabetically
SELECT Title, Year FROM movies
ORDER BY Title ASC
LIMIT 5;

-- List the next five Pixar movies sorted alphabetically
SELECT Title, Year FROM movies
ORDER BY Title ASC
LIMIT 5 OFFSET 5;

-- Find the domestic and international sales for each movie
SELECT Title, Domestic_sales, International_sales 
FROM movies 
JOIN Boxoffice ON movies.id = Boxoffice.movie_id;

-- Show movies that did better internationally than domestically
SELECT Title, Domestic_sales, International_sales 
FROM movies 
JOIN Boxoffice ON movies.id = Boxoffice.movie_id 
WHERE International_sales > Domestic_sales;

-- List all the movies by their ratings in descending order
SELECT Title, Rating 
FROM movies 
JOIN Boxoffice ON movies.id = Boxoffice.movie_id 
ORDER BY Rating DESC;
```
