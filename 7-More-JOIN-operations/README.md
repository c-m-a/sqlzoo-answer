# More JOIN operations


movie
id
title
yr
director
budget
gross

actor
id
name

casting
movieid
actorid
ord

![DB model](http://sqlzoo.net/w/images/1/10/Movie-er.png)

1. List the films where the yr is 1962 [Show id, title]

```sql
SELECT id, title
FROM movie
WHERE yr=1962
```

2. Give year of 'Citizen Kane'.

```sql
SELECT yr FROM movie WHERE title = 'Citizen Kane'
```

3. List all of the Star Trek movies, include the id, title and yr (all of these
 movies include the words Star Trek in the title). Order results by year.

```sql
SELECT id, title, yr FROM movie WHERE title LIKE '%Star Trek%'
```

4. What id number does the actor 'Glenn Close' have?

```sql
SELECT id FROM actor WHERE name = 'Glenn Close'
```

5. What is the id of the film 'Casablanca'

```sql
SELECT id FROM movie WHERE title = 'Casablanca'
```

6. Obtain the cast list for 'Casablanca'.

what is a cast list?
The cast list is the names of the actors who were in the movie.

Use movieid=11768, (or whatever value you got from the previous question)

```sql
SELECT name
FROM actor JOIN casting ON (id=actorid)
WHERE movieid = 11768
```

7. Obtain the cast list for the film 'Alien'

Solution 1 but it's not the right one. Alien movieid = 10522

```sql
SELECT a.name FROM casting c JOIN actor a ON (a.id=c.actorid)
WHERE movieid = 10522
```

Correct one

```sql
select a.name from casting c JOIN actor a ON (a.id=c.actorid)
JOIN movie m ON (m.id=c.movieid)
where m.title = 'Alien'
```

8. List the films in which 'Harrison Ford' has appeared

```sql
SELECT m.title FROM casting c JOIN actor a ON (a.id=c.actorid)
JOIN movie m ON (m.id=c.movieid)
WHERE a.name = 'Harrison Ford'
```

9. List the films where 'Harrison Ford' has appeared - but not in the starring
 role. [Note: the ord field of casting gives the position of the actor. If ord=1
 then this actor is in the starring role]

```sql
SELECT m.title FROM casting c JOIN actor a ON (a.id=c.actorid)
JOIN movie m ON (m.id=c.movieid)
WHERE a.name = 'Harrison Ford'
AND c.ord <> 1
```

10. List the films together with the leading star for all 1962 films.

```sql
SELECT m.title, a.name FROM casting c JOIN actor a ON (a.id=c.actorid)
JOIN movie m ON (m.id=c.movieid)
WHERE c.ord = 1
AND m.yr = 1962
```

11. Which were the busiest years for 'Rock Hudson', show the year and the number
 of movies he made each year for any year in which he made more than 2 movies.

```sql
SELECT yr,COUNT(title) FROM
  movie JOIN casting ON movie.id=movieid
        JOIN actor   ON actorid=actor.id
WHERE name='Rock Hudson'
GROUP BY yr
HAVING COUNT(title) > 2
```

12. List the film title and the leading actor for all of the films 'Julie Andrews' played in.
Did you get "Little Miss Marker twice"?

Julie Andrews starred in the 1980 remake of Little Miss Marker and not the original(1934).

Title is not a unique field, create a table of IDs in your subquery

![Video solution](https://youtu.be/BcNIDK5qYx)

SELECT m.title, a.name FROM casting c
                            JOIN movie m ON (m.id = c.movieid AND c.ord = 1)
                            JOIN actor a ON (a.id = c.actorid)
WHERE m.id in (SELECT movieid FROM casting
                  WHERE actorid IN (
                       SELECT id FROM actor
                       WHERE name='Julie Andrews'
                  )
              )

13. Obtain a list, in alphabetical order, of actors who've had at least 30
 starring roles.

```sql
SELECT name FROM actor
WHERE id IN (SELECT c.actorid from casting c
              WHERE c.ord = 1
              GROUP BY c.actorid
              HAVING SUM(c.ord) >= 30)
ORDER BY name
```

14. List the films released in the year 1978 ordered by the number of actors in
 the cast, then by title.

```sql
SELECT m.title, count(a.id) n_actors
FROM casting c JOIN movie m ON m.id = c.movieid
               JOIN actor a ON a.id = c.actorid
WHERE m.yr = 1978
GROUP BY m.title
ORDER BY n_actors DESC, m.title
```

15. List all the people who have worked with 'Art Garfunkel'.

```sql
SELECT a.name FROM casting c JOIN actor a on (a.id = c.actorid)
  WHERE movieid IN (SELECT movieid FROM casting c
                      JOIN actor a ON (a.id = c.actorid)
                    WHERE a.name = 'Art Garfunkel')
AND a.name <> 'Art Garfunkel'
```
