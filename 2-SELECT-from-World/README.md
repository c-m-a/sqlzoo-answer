name | continent | area | population | gdp
Afghanistan | Asia | 652230 | 25500100 | 20343000000
Albania | Europe | 28748  | 2831741 | 12960000000
Algeria | Africa | 2381741  | 37100000 | 188681000000
Andorra | Europe | 468 | 78115  | 3712000000
Angola | Africa | 1246700  | 20609294 | 100990000000
...

1. [Read the notes](http://sqlzoo.net/wiki/Read_the_notes_about_this_table.)
 about this table. Observe the result of running this SQL command to show the
 name, continent and population of all countries.

```sql
SELECT name, continent, population FROM world
```

2. [How to use WHERE to filter records.](http://sqlzoo.net/wiki/WHERE_filters)
 Show the name for the countries that have a population of at least 200 million.
 200 million is 200000000, there are eight zeros.

```sql
SELECT name
FROM world
WHERE population >= 200000000
```

3. Give the name and the per capita GDP for those countries with a population
 of at least 200 million.

```sql
SELECT name, gdp/population as per_cap_GDP FROM world
WHERE population >= 200000000
```

4. Show the name and population in millions for the countries of the continent
 'South America'. Divide the population by 1000000 to get population in millions.

```sql
SELECT name, population/1000000 as pop_millions FROM world
WHERE continent = 'South America'
```

5. Show the name and population for France, Germany, Italy.

```sql
SELECT name, population FROM world
WHERE name in ('France', 'Germany', 'Italy')
```

6. Show the countries which have a name that includes the word 'United'

```sql
SELECT name
FROM world
WHERE name LIKE '%United%'
```

7. Show the countries that are big by area or big by population. Show name, population and area.

```sql
SELECT name, population, area FROM world
WHERE area >= 3000000 OR
population > 250000000
```

8. Exclusive OR (XOR). Show the countries that are big by area (more than 3
 million) or big by population (more than 250 million) but not both. Show name,
 population and area.

```sql
SELECT name, population, area FROM world
WHERE area >= 3000000 XOR
population > 250000000
```

9. For South America show population in millions and GDP in billions both to 2
 decimal places.

[ROUND Instruction](http://sqlzoo.net/wiki/ROUND)

```sql
SELECT
  name,
  ROUND(population/1000000, 2) pop_millions,
  ROUND(gdp/1000000000, 2) gdp_billions
FROM world
WHERE continent = 'South America'
```

10. Show the name and per-capita GDP for those countries with a GDP of at least
 one trillion (1000000000000; that is 12 zeros). Round this value to the nearest
 1000.

Show per-capita GDP for the trillion dollar countries to the nearest $1000.

```sql
SELECT
  name,
  ROUND((gdp/population), -3) AS per_cap_GDP
FROM world
WHERE gdp >= 1000000000000
```

11. Show the name and capital where the name and the capital have the same
 number of characters.

```sql
SELECT name, capital
  FROM world
WHERE LENGTH(name) = LENGTH(capital)
```

12. Show the name and the capital where the first letters of each match. Don't
 include countries where the name and the capital are the same word.

```sql
SELECT name, capital
FROM world
WHERE LEFT(name,1) = LEFT(capital,1) AND
  name <> capital
```

13. Find the country that has all the vowels and no spaces in its name.

```sql
SELECT name
   FROM world
WHERE name LIKE '%a%'
AND name LIKE '%e%'
AND name LIKE '%i%'
AND name LIKE '%o%'
AND name LIKE '%u%'
AND name NOT LIKE '% %'
```
