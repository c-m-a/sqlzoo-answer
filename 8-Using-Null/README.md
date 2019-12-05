# Using Null

## Teacher

id | dept | name | phone | mobile
101  | 1 | Shrivell | 2753 | 07986 555 1234
102  | 1 | Throd | 2754 | 07122 555 1920
103 | 1 | Splint | 2293 |
104 | | Spiregrain | 3287 |
105 | 2 | Cutflower | 3212 | 07996 555 6574
106 | | Deadyawn | 3345 |
...

## Dept
id | name
1  | Computing
2  | Design
3  | Engineering
...

1. List the teachers who have NULL for their department.

Why we cannot use =

You might think that the phrase dept=NULL would work here but it doesn't - you
 can use the phrase dept IS NULL

That's not a proper explanation.

No it's not, but you can read a better explanation at
 [Wikipedia:NULL](http://en.wikipedia.org/wiki/Null_%28SQL%29).

```sql
SELECT name FROM teacher WHERE dept is NULL
```

2. Note the INNER JOIN misses the teachers with no department and the departments
 with no teacher.

```sql
SELECT teacher.name, dept.name
  FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id)
```
**Alternative**

```sql
SELECT teacher.name, dept.name
  FROM teacher JOIN dept
           ON (teacher.dept=dept.id)
```

3. Use a different JOIN so that all teachers are listed.

```sql
SELECT teacher.name, dept.name
  FROM teacher LEFT JOIN dept
           ON (teacher.dept=dept.id)
```

4. Use a different JOIN so that all departments are listed.

```sql
SELECT teacher.name, dept.name
 FROM teacher RIGHT JOIN dept
           ON (teacher.dept=dept.id)
```

5. Use COALESCE to print the mobile number. Use the number '07986 444 2266' if
 there is no number given. Show teacher name and mobile number or '07986 444 2266'

```sql
SELECT name, COALESCE(mobile, '07986 444 2266') FROM teacher
```

6. Use the COALESCE function and a LEFT JOIN to print the teacher name and
 department name. Use the string 'None' where there is no department.

```sql
SELECT teacher.name, COALESCE(dept.name, 'None')
 FROM teacher LEFT JOIN dept
           ON (teacher.dept=dept.id)
```

7. Use COUNT to show the number of teachers and the number of mobile phones.

```sql
select count(name), count(mobile) from teacher
```

8. Use COUNT and GROUP BY dept.name to show each department and the number of
 staff. Use a RIGHT JOIN to ensure that the Engineering department is listed.

```sql
SELECT d.name, COUNT(t.id)
FROM teacher t RIGHT JOIN dept d ON (t.dept = d.id)
GROUP BY d.name
```

**Alternative**

```sql
SELECT d.name, COUNT(t.id)
FROM dept d LEFT JOIN teacher t ON (d.id = t.dept)
GROUP BY d.name
```

9. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is
 in dept 1 or 2 and 'Art' otherwise.

```sql
SELECT name, case WHEN dept <= 2 THEN 'Sci'
  ELSE 'Art'
  END
from teacher
```

10. Use CASE to show the name of each teacher followed by 'Sci' if the teacher
 is in dept 1 or 2, show 'Art' if the teacher's dept is 3 and 'None' otherwise.

```
select name, case when dept <= 2 then 'Sci'
  when dept = 3 then 'Art'
  ELSE 'None'
  end
from teacher
```
