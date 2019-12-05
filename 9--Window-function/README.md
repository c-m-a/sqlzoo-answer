# Window functions

1. Show the lastName, party and votes for the constituency 'S14000024' in 2017.

```sql
SELECT lastName, party, votes
  FROM ge
 WHERE constituency = 'S14000024' AND yr = 2017
ORDER BY votes DESC
```

2. You can use the RANK function to see the order of the candidates. If you RANK
 using (ORDER BY votes DESC) then the candidate with the most votes has rank 1.
Show the party and RANK for constituency S14000024 in 2017. List the output by party

```sql
SELECT party, votes,
       RANK() OVER (ORDER BY votes) as posn
  FROM ge
 WHERE constituency = 'S14000024' AND yr = 2017
GROUP by party, votes
ORDER BY votes
```

3. The 2015 election is a different PARTITION to the 2017 election. We only care
 about the order of votes for each year.

Use PARTITION to show the ranking of each party in S14000021 in each year.

```sql
SELECT yr,party, votes,
      RANK() OVER (PARTITION BY yr ORDER BY votes DESC) as posn
  FROM ge
 WHERE constituency = 'S14000021'
GROUP BY yr, party, votes
ORDER BY party,yr
```

4. 
