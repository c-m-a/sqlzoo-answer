# Self join

[Details of the database](https://sqlzoo.net/wiki/Edinburgh_Buses) Looking at
the data

stop
|
id
name

route
num
company
pos
stop


1. How many stops are in the database.

```sql
select count(*) from stops
```

2. Find the id value for the stop 'Craiglockhart'.

```sql
select id from stops where name = 'Craiglockhart'
```

3. Give the id and the name for the stops on the '4' 'LRT' service.

```sql
SELECT id, name
FROM stops s left JOIN route r ON (s.id = r.stop)
WHERE r.num = 4
    AND r.company = 'LRT'
order by name
```

4. The query shown gives the number of routes that visit either London Road (149)
 or Craiglockhart (53). Run the query and notice the two services that link
 these stops have a count of 2. Add a HAVING clause to restrict the output to
 these two routes.

```sql
SELECT company, num, COUNT(*)
FROM route WHERE stop=149 OR stop=53
GROUP BY company, num
HAVING COUNT(*) = 2
```

5. Execute the self join shown and observe that b.stop gives all the places you
 can get to from Craiglockhart, without changing routes. Change the query so that
 it shows the services from Craiglockhart to London Road.

```sql
SELECT a.company, a.num, a.stop, b.stop
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
WHERE a.stop=53
AND b.stop = 149
```

6. The query shown is similar to the previous one, however by joining two copies
 of the stops table we can refer to stops by name rather than by number. Change
 the query so that the services between 'Craiglockhart' and 'London Road' are
 shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross'

```sql
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
  JOIN stops stopa ON (a.stop=stopa.id)
  JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart'
AND stopb.name='London Road'
```

7. Give a list of all the services which connect stops 115 and 137
 ('Haymarket' and 'Leith')

```sql
SELECT DISTINCT a.company, a.num
FROM route a JOIN route b ON a.company=b.company AND a.num=b.num
WHERE a.stop = 115 AND b.stop = 137
``

8. Give a list of the services which connect the stops 'Craiglockhart' and
 'Tollcross'

```sql
SELECT a.company, a.num
FROM route a JOIN route b JOIN stops sa JOIN stops sb
ON a.company=b.company
AND a.num=b.num
AND a.stop=sa.id
AND b.stop=sb.id
WHERE sa.name='Craiglockhart' AND sb.name='Tollcross'
```

9. Give a distinct list of the stops which may be reached from 'Craiglockhart'
 by taking one bus, including 'Craiglockhart' itself, offered by the LRT company.
 Include the company and bus no. of the relevant services.

```sql
SELECT DISTINCT sb.name, a.company, a.num
FROM route a JOIN route b JOIN stops sa JOIN stops sb
ON a.company=b.company
AND a.num=b.num
AND a.stop=sa.id
AND b.stop=sb.id
WHERE sa.name='Craiglockhart'
```

10. Find the routes involving two buses that can go from Craiglockhart to Lochend.
Show the bus no. and company for the first bus, the name of the stop for the transfer,
and the bus no. and company for the second bus.

Hint
Self-join twice to find buses that visit Craiglockhart and Lochend, then join
 those on matching stops.

```sql
SELECT DISTINCT an, ac, stops.name, dn, dc
FROM
  (SELECT a.num as an, a.company as ac, b.stop as bstop
   FROM
   route a JOIN route b JOIN stops s
   ON a.num=b.num AND s.id=a.stop
   WHERE s.name='Craiglockhart') T1
 JOIN
  (SELECT d.num as dn, d.company as dc, c.stop as cstop
   FROM
   route c JOIN route d JOIN stops s
   ON c.num=d.num AND c.company=d.company AND s.id=d.stop
   WHERE s.name='Sighthill') T2
 JOIN stops
 ON bstop=cstop AND bstop=stops.id
```
