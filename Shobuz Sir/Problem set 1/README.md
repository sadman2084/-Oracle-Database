## SQL Queries and Explanations

### 1. Find the names of those departments whose budget is higher than that of Astronomy. List them in alphabetical order.
```sql
SELECT dept_name FROM department
WHERE budget > (SELECT budget FROM department WHERE dept_name = 'Astronomy')
ORDER BY dept_name ASC;
```

### 2. Display a list of all instructors, showing each instructor's ID and the number of sections taught. Make sure to show the number of sections as 0 for instructors who have not taught any section.
```sql
SELECT instructor.ID,
       COALESCE(COUNT(teaches.sec_id), 0) AS number_of_sections
FROM instructor
LEFT JOIN teaches ON instructor.ID = teaches.ID
GROUP BY instructor.ID
ORDER BY number_of_sections ASC, instructor.ID ASC;
```

### 3. For each student who has retaken a course at least twice (i.e., the student has taken the course at least three times), show the course ID and the student's ID. Please display your results in order of course ID and do not display duplicate rows.
```sql
SELECT DISTINCT course_id, ID
FROM takes
GROUP BY course_id, ID
HAVING COUNT(*) >= 3
ORDER BY ID, course_id;
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
SELECT department.dept_name
FROM department
JOIN instructor ON department.dept_name = instructor.dept_name
GROUP BY department.dept_name
HAVING AVG(instructor.salary) > 42000;
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

