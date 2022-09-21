# M05W11 - SQL Intro

### To Do
- [x] Introduction to RDBMS
- [x] The Relational Data Model (Tables, Columns, Rows)
- [x] `SELECT` Statements
- [x] Filtering and ordering
- [x] Joining tables
- [x] Grouping records
- [x] Aggregation functions
- [x] `LIMIT` and `OFFSET`

RDBMS === Relational Database Management System

client <===TCP/HTTP===> web server <==TCP/POSTGRES==> rdbms

mysql://

Database === collection of tables
Relational Database === collection of tables where each table is related to one or more tables

Food Item     Calories    Price
Big Mac         500       $4.99
Large Fries     750       $3.99


row === record
column === field










### SELECT Challenges

For the first 6 queries, we'll be using the `users` table.

![users table](https://andydlindsay-portfolio.s3.amazonaws.com/lighthouse/w5d1-users.io.png)

1. List total number of users

```sql
SELECT COUNT(*)
FROM users;

-- using an alias
SELECT COUNT(*) AS num_users -- aliased count as num_users
-- in between
FROM users;
```

2. List users over the age of 18

```sql
SELECT *
FROM users
WHERE age > 18;
```

3. List users who are over the age of 18 and have the last name 'Barrows'

```sql
SELECT *
FROM users
WHERE age > 18 AND last_name = 'Barrows';
```

4. List users over the age of 18 who live in Canada sorted by age from oldest to youngest and then last name alphabetically

```sql
SELECT *
FROM users
WHERE age > 18 AND country = 'Canada'
ORDER BY age DESC, last_name;
```

5. List users who live in Canada and whose accounts are overdue

```sql
SELECT *
FROM users
WHERE country = 'Canada' AND payment_due_date < 'Sep 20, 2022';

SELECT *, NOW()
FROM users
WHERE country = 'Canada' AND payment_due_date < NOW();

SELECT *, CURRENT_DATE
FROM users
WHERE country = 'Canada' AND payment_due_date < CURRENT_DATE;
```

6. List all the countries users live in; don't repeat any countries

```sql
SELECT DISTINCT country
FROM users
ORDER BY country;
```

For the rest of the queries, we'll be using the `albums` and `songs` tables.

![albums and songs](https://andydlindsay-portfolio.s3.amazonaws.com/lighthouse/albums-and-songs.png)

7. List all albums along with their songs

```sql
SELECT *
FROM albums
JOIN songs ON albums.id = album_id;
```

8. List all albums along with how many songs each album has

```sql
SELECT albums.*, COUNT(songs.id)
FROM albums
JOIN songs ON albums.id = album_id
GROUP BY albums.id;
```

9. Enhance previous query to only include albums that have more than 10 songs

```sql
SELECT albums.*
FROM albums
JOIN songs ON albums.id = album_id
GROUP BY albums.id
HAVING COUNT(songs.id) > 10;
```

10. List ALL albums in the database, along with their songs if any

```sql
SELECT *
FROM albums
LEFT JOIN songs ON albums.id = album_id;
```

11. List albums along with average song rating

```sql
SELECT albums.*, AVG(rating)
FROM albums
JOIN songs ON albums.id = songs.album_id
GROUP BY albums.id;
```

12. List albums and songs with rating higher than album average

```sql
SELECT *, (SELECT AVG(rating) FROM songs WHERE album_id = albums.id) AS album_avg_rating
FROM albums
JOIN songs ON albums.id = songs.album_id
WHERE rating > (SELECT AVG(rating) FROM songs WHERE album_id = albums.id);
```

### Useful Links
- [Top 10 Most Popular RDBMSs](https://www.c-sharpcorner.com/article/what-are-the-most-popular-relational-databases/)
- [Another Ranking of DBMSs](https://db-engines.com/en/ranking)
- [SELECT Queries Order of Execution](https://sqlbolt.com/lesson/select_queries_order_of_execution)
- [SQL Joins Visualizer](https://sql-joins.leopard.in.ua/)
