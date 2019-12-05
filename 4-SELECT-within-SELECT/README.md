name | continent | area  | population | gdp
Afghanistan | Asia | 652230 | 25500100 | 20343000000
Albania | Europe | 28748 | 2831741 | 12960000000
Algeria | Africa | 2381741 | 37100000  | 188681000000
Andorra | Europe | 468 | 78115 | 3712000000
Angola | Africa | 1246700 | 20609294  | 100990000000
...

1. List each country name where the population is larger than that of 'Russia'.

```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```

2. Show the countries in Europe with a per capita GDP greater than
 'United Kingdom'.

```sql
SELECT name
FROM world x
WHERE continent = 'Europe'
AND gdp / population > (SELECT gdp / population
                        FROM world
                        WHERE name = 'United Kingdom')
```

3. List the name and continent of countries in the continents containing either
 Argentina or Australia. Order by name of the country.

```sql
SELECT name, continent
FROM world
WHERE continent IN (SELECT continent
                    FROM world
                    WHERE name IN ('Argentina', 'Australia'))
ORDER BY name
```

4. Which country has a population that is more than Canada but less than Poland?
 Show the name and the population.

```sql
SELECT name, population FROM world
WHERE population > (SELECT population FROM world WHERE name = 'Canada')
AND population < (SELECT population FROM world WHERE name = 'Poland')
```

5. Show the name and the population of each country in Europe. Show the population
 as a percentage of the population of Germany.

```sql
SELECT name,
CONCAT(
  ROUND(100 * population / (SELECT population FROM world WHERE name = 'Germany'), 0),
  '%')
FROM world
WHERE continent = 'Europe'
```

6. [To get a well rounded view of the important
 features of SQL you should move on to the next
 tutorial concerning aggregates.](http://sqlzoo.net/wiki/SUM_and_COUNT)

To gain an absurdly detailed view of one insignificant feature of the language, read on.

We can use the word ALL to allow >= or > or < or <=to act over a list. For
 example, you can find the largest country in the world, by population with this query:

SELECT name
  FROM world
 WHERE population >= ALL(SELECT population
                           FROM world
                          WHERE population>0)

You need the condition population>0 in the sub-query as some countries have null
 for population.

```sql
SELECT name
  FROM world
WHERE gdp >= ALL(SELECT gdp
                  FROM world
                  WHERE continent = 'Europe' AND gdp>0)
AND continent <> 'Europe'
```

7. Find the largest country (by area) in each continent, show the continent, the
 name and the area:

```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND population>0)
```

8. List each continent and the name of the country that comes first
 alphabetically.

```sql
SELECT continent, name
FROM world x
WHERE name = (SELECT name FROM world
              WHERE continent = x.continent
              ORDER BY continent, name
              LIMIT 1)
```

9. Find the continents where all countries have a population <= 25000000. Then
 find the names of the countries associated with these continents. Show name,
 continent and population.

```sql
SELECT name, continent, population
FROM world x
WHERE 25000000 >= ALL (SELECT population FROM world
    WHERE continent = y.continent
    AND population > 0)
```

10. List each continent and the name of the country that comes first alphabetically.

```sql
SELECT name, continent
FROM world x
WHERE population / 3 >= all (SELECT MAX(population) FROM world
                              WHERE continent = x.continent
                              AND name <> x.name)
```
