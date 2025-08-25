SQL Commands:

`GroupBy` && `Count` && SUBQUERIES
```SQL
SELECT name
FROM Employee
WHERE id IN (
	SELECT managerId
	FROM Employee
	GROUP BY managerId
	HAVING COUNT(*) >= 5)
```


## JOIN

`cross join` and `left join` and `group by on pairs`

``` SQL
SELECT
	s.student_id,
	s.student_name,
	sub.subject_name,
	COUNT(e.subject_name) AS attended_exams
FROM Students AS s
CROSS JOIN Subjects AS sub
LEFT JOIN Examinations AS e
	ON e.student_id = s.student_id
	AND e.subject_name = sub.subject_name
GROUP BY s.student_id, s.student_name, sub.subject_name
```