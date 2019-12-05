# The JOIN operation

![BD Model](http://sqlzoo.net/wiki/File:FootballERD.png)

id | mdate | stadium | team1 | team2
1001 | 8 June 2012 | National Stadium, Warsaw  | POL | GRE
1002 | 8 June 2012 | Stadion Miejski (Wroclaw) | RUS | CZE
1003 | 12 June 2012  | Stadion Miejski (Wroclaw) | GRE | CZE
1004 | 12 June 2012  | National Stadium, Warsaw  | POL | RUS
...

1. Modify it to show the matchid and player name for all goals scored by Germany.
 To identify German players, check for: teamid = 'GER'

```sql
SELECT matchid, player name FROM goal
WHERE teamid = 'GER'
```

2. Show id, stadium, team1, team2 for just game 1012

```sql
SELECT id,stadium,team1,team2
  FROM game
WHERE id = 1012
```

3. You can combine the two steps into a single query with a JOIN.

```sql
SELECT *
  FROM game JOIN goal ON (id=matchid)
```

The FROM clause says to merge data from the goal table with that from the game
 table. The ON says how to figure out which rows in game go with which rows in
 goal - the matchid from goal must match id from game. (If we wanted to be more
 clear/specific we could say ON (game.id=goal.matchid)

The code below shows the player (from the goal) and stadium name (from the game
 table) for every goal scored.

Modify it to show the player, teamid, stadium and mdate for every German goal.

```sql
SELECT player, teamid, stadium, mdate
  FROM game JOIN goal ON (id=matchid)
WHERE teamid = 'GER'
```

4. Show the team1, team2 and player for every goal scored by a player called
Mario player LIKE 'Mario%'

```sql
SELECT team1, team2, player
  FROM game JOIN goal ON (id=matchid)
WHERE player LIKE 'Mario%'
```

5. The table eteam gives details of every national team including the coach. You
 can JOIN goal to eteam using the phrase goal JOIN eteam on teamid=id

Show player, teamid, coach, gtime for all goals scored in the first 10 minutes
 gtime<=10

```sql
SELECT player, teamid, coach, gtime
  FROM goal JOIN eteam ON (teamid=id)
WHERE gtime<=10
```

6. To JOIN game with eteam you could use either
game JOIN eteam ON (team1=eteam.id) or game JOIN eteam ON (team2=eteam.id)

Notice that because id is a column name in both game and eteam you must specify
 eteam.id instead of just id

List the the dates of the matches and the name of the team in which
 'Fernando Santos' was the team1 coach.

```sql
select mdate, teamname
from game JOIN eteam ON (team1=eteam.id)
where coach = 'Fernando Santos'
```

7. List the player for every goal scored in a game where the stadium was
 'National Stadium, Warsaw'

```sql
SELECT player
FROM game JOIN goal ON (id=matchid)
WHERE stadium = 'National Stadium, Warsaw'
```

8. The example query shows all goals scored in the Germany-Greece quarterfinal.

Instead show the name of all players who scored a goal against Germany.

HINT

Select goals scored only by non-German players in matches where GER was the id
 of either team1 or team2.

You can use teamid!='GER' to prevent listing German players.

You can use DISTINCT to stop players being listed twice.


```sql
SELECT DISTINCT player
  FROM game JOIN goal ON matchid = id
    WHERE (team1 = 'GER' or team2 = 'GER')
AND teamid <> 'GER'
```

9. Show teamname and the total number of goals scored.

COUNT and GROUP BY

You should `COUNT(\*)` in the SELECT line and GROUP BY teamname

```sql
SELECT teamname, count(gtime)
  FROM eteam JOIN goal ON id=teamid
GROUP by teamname
```

10. Show the stadium and the number of goals scored in each stadium.

```sql
select distinct stadium, count(gtime) from game JOIN goal ON (id=matchid)
group by stadium
```

11. For every match involving 'POL', show the matchid, date and the number of
 goals scored.

```sql
SELECT matchid, mdate, count(gtime)
  FROM game JOIN goal ON matchid = id
 WHERE (team1 = 'POL' OR team2 = 'POL')
group by matchid, mdate
```

12. For every match where 'GER' scored, show matchid, match date and the number
 of goals scored by 'GER'

```sql
SELECT id, mdate, COUNT(teamid)
FROM game JOIN goal ON (id=matchid)
WHERE teamid = 'GER'
GROUP BY id, mdate
```

13. List every match with the goals scored by each team as shown. This will use
 ["CASE WHEN"](http://sqlzoo.net/wiki/CASE) which has not been explained in any
 previous exercises.

mdate | team1 | score1 | team2 | score2
1 July 2012 | ESP | 4 | ITA  | 0
10 June 2012 | ESP | 1 | ITA | 1
10 June 2012 | IRL | 1 | CRO | 3
...

Notice in the query given every goal is listed. If it was a team1 goal then a 1
appears in score1, otherwise there is a 0. You could SUM this column to get a
 count of the goals scored by team1. Sort your result by mdate, matchid, team1
 and team2.

```sql
SELECT mdate,
  team1,
  SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) score1,
  team2,
  SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) score2
  FROM game LEFT JOIN goal ON id = matchid
GROUP BY mdate, team1, team2
ORDER BY mdate, id, team1, team2
```
