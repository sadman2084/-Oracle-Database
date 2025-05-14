## SQL Queries and Explanations

### 1. Find the names of those departments whose budget is higher than that of Astronomy. List them in alphabetical order.
```sql
SELECT dept_name FROM department
WHERE budget > (SELECT budget FROM department WHERE dept_name = 'Astronomy')
ORDER BY dept_name ASC;
```

### 2. Display a list of all instructors, showing each instructor's ID and the number of sections taught. Make sure to show the number of sections as 0 for instructors who have not taught any section.
```sql
SELECT instructor.id, COUNT(teaches.course_id) AS num_sections
FROM instructor
LEFT JOIN teaches ON instructor.id = teaches.id
GROUP BY instructor.id
order by num_sections asc;
```

### 3. For each student who has retaken a course at least twice (i.e., the student has taken the course at least three times), show the course ID and the student's ID. Please display your results in order of course ID and do not display duplicate rows.
```sql
SELECT DISTINCT course_id, ID
FROM takes
GROUP BY course_id, ID
HAVING COUNT(*) >= 3
ORDER BY ID, course_id;
```
### 4.Find the names of Biology students who have taken at least 3 Accounting courses.
```sql
SELECT s.name 
FROM student s, takes t, course c 
WHERE s.ID = t.ID AND t.course_id = c.course_id 
AND s.dept_name = 'Biology' AND c.dept_name = 'Accounting' 
GROUP BY s.ID, s.name 
HAVING COUNT(*) >= 3;

```
### 5. Find the sections that had maximum enrollment in Fall 2010.
```sql
select top 1
 course_id, sec_id 
from takes
WHERE semester = 'Fall' AND year = 2010
group by course_id, sec_id
order by count(*) desc;

);
```
### 6. Find student names and the number of law courses taken for students who have taken at least half of the available law courses. (These courses are named things like or Environmental Law.
```sql
SELECT student.name, COUNT(takes.course_id) AS law_courses_taken
FROM student
JOIN takes ON student.ID = takes.ID
JOIN course ON takes.course_id = course.course_id
WHERE course.title LIKE '%Law%'
GROUP BY student.ID, student.name
HAVING COUNT(takes.course_id) >= (
SELECT COUNT(course_id) / 2
FROM course
WHERE title LIKE '%Law%'
);
```

### 7. Find the rank and name of the 10 students who earned the most A grades (A-, A, A+). Use alphabetical order by name to break ties. Note: the browser SQLite does not support window functions.
```sql
SELECT student.name, COUNT(*) AS a_count
FROM student
JOIN takes ON student.ID = takes.ID
WHERE takes.grade IN ('A-', 'A', 'A+')
GROUP BY student.ID, student.name
ORDER BY a_count DESC, student.name ASC
LIMIT 10;
```
### G1.Find out the ID and salary of the instructors.
```sql
SELECT ID, salary
FROM instructor;
;
```
### G2. Find out the ID and salary of the instructor who gets more than $85,000.
```sql
SELECT ID, salary
FROM instructor
WHERE salary > 85000;
```

### G3. Find out the department names and their budget at the university.
```sql
SELECT dept_name, budget
FROM department;
```
### G4. List out the names of the instructors from Computer Science who have more than $70,000.
```sql
SELECT name
FROM instructor
WHERE dept_name = 'Comp. Sci.'
AND salary > 70000;
```

### G5. For all instructors in the university who have taught some course, find their names and the course ID of all courses they taught.
```sql
SELECT i.name, t.course_id
FROM instructor i
JOIN teaches t ON i.ID = t.ID;
```
### G6. Find the names of all instructors whose salary is greater than at least one instructor in the Biology department.
```sql
SELECT name
FROM instructor
WHERE salary >  (
SELECT max(salary)
FROM instructor
WHERE dept_name = 'Biology'
);
```
### G7. Find the advisor of the student with ID 12345
```sql
SELECT i_id
FROM advisor
WHERE s_id = 12345;
```
### G8. Find the average salary of all instructors.
```sql
SELECT AVG(salary) AS average_salary
FROM instructor;
```
### G9. Find the names of all departments whose building name includes the substring.
```sql
SELECT dept_name
FROM department
WHERE building LIKE '%Watson%';
```

### G10. Find the names of instructors with salary amounts between $90,000 and $100,000.
```sql
SELECT name FROM instructor
WHERE salary >= 90000 AND salary <= 100000;
```

### G11. Find the instructor names and the courses they taught for all instructors in the Biology department who have taught some course.
```sql
SELECT instructor.name, teaches.course_id
FROM instructor
JOIN teaches ON instructor.ID = teaches.ID
WHERE instructor.dept_name = 'Biology';
```

### G12. Find the courses taught in the Fall-2009 semester.
```sql
SELECT course_id FROM teaches
WHERE semester='Fall' AND year=2009;
```

### G13. Find the set of all courses taught either in Fall-2009 or in Spring-2010.
```sql
SELECT course_id FROM teaches
WHERE (semester='Fall' AND year=2009) OR (semester = 'Spring' AND year = 2010);
```

### G14. Find the set of all courses taught in the Fall-2009 as well as in Spring-2010.
```sql
SELECT course_id
FROM teaches
WHERE semester = 'Fall' AND year = 2009
AND course_id IN (
    SELECT course_id
    FROM teaches
    WHERE semester = 'Spring' AND year = 2010
);
```

### G15. Find all courses taught in the Fall-2009 semester but not in the Spring-2010 semester.
```sql
SELECT DISTINCT course_id
FROM teaches
WHERE semester = 'Fall' AND year = 2009
AND course_id NOT IN (
    SELECT course_id
    FROM teaches
    WHERE semester = 'Spring' AND year = 2010
);
```

### G16. Find all instructors who appear in the instructor relation with null values for salary.
```sql
SELECT * FROM instructor
WHERE salary IS NULL;
```

### G17. Find the average salary of instructors in the Finance department.
```sql
SELECT AVG(salary) AS average_salary
FROM instructor
WHERE dept_name = 'Finance';
```

### G19. Find the average salary in each department.
```sql
SELECT dept_name, AVG(salary)
FROM instructor
GROUP BY dept_name;
```

### G20. Find the number of instructors in each department who teach a course in the Spring-2010 semester.
```sql
SELECT dept_name, COUNT(*) AS number_of_instructor
FROM instructor
JOIN teaches ON instructor.ID = teaches.ID
WHERE teaches.semester='Spring' AND teaches.year=2010
GROUP BY dept_name;
```

### G21. List out the departments where the average salary of the instructors is more than $42,000.
```sql
  SELECT dept_name 
   FROM instructor 
   GROUP BY dept_name 
   HAVING AVG(salary) > 42000;;
```

### G24. Find all the courses taught in the Fall-2009 semester but not in the Spring-2010 semester.
```sql
SELECT DISTINCT course_id
FROM teaches
WHERE semester = 'Fall' AND year = 2009
AND course_id NOT IN (
    SELECT course_id
    FROM teaches
    WHERE semester = 'Spring' AND year = 2010
);
```

### G25. Select the names of instructors whose names are neither Mozart nor Einstein.
```sql
SELECT name
FROM instructor
WHERE name NOT IN ('Mozart', 'Einstein');
```

### G26. Find the total number of (distinct) students who have taken course sections taught by the instructor with ID 110011.
```sql
SELECT COUNT(DISTINCT takes.ID) AS total_students
FROM takes
JOIN teaches USING (course_id, sec_id, semester, year)
WHERE teaches.ID = 110011;
```

### G27. Find the ID and names of all instructors whose salary is greater than at least one instructor in the History department.
```sql
SELECT ID, name
FROM instructor
WHERE salary > (
    SELECT MIN(salary)
    FROM instructor
    WHERE dept_name = 'History'
);
```

### G28. Find the names of all instructors that have a salary value greater than that of each instructor in the Biology department.
```sql
SELECT name
FROM instructor
WHERE salary > (
    SELECT MAX(salary)
    FROM instructor
    WHERE dept_name = 'Biology'
);
```

