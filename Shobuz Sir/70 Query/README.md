# Here all the questions and query for university database.
---




# SQL Queries and Explanations



### 1. Find out the ID and salary of the instructors.
Explanation: This query selects the `ID` and `salary` columns from the `instructor` table.
```sql
SELECT ID, salary FROM instructor;
```

### 2. Find out the ID and salary of the instructor who gets more than \$85,000.

Explanation: This query selects the `ID` and `salary` of instructors where the salary is greater than 85,000.

```sql
SELECT ID, SALARY FROM INSTRUCTOR WHERE SALARY > 85000;
```

### 3. Find out the department names and their budget at the university.

Explanation: This query selects the department names (`dept_name`) and their budget (`budget`) from the `department` table.

```sql
SELECT dept_name, budget FROM department;
```

### 4. List out the names of the instructors from Computer Science who have more than \$70,000.

Explanation: This query selects the `name` of instructors in the 'Computer Science' department who have a salary greater than 70,000.

```sql
SELECT name FROM instructor WHERE dept_name = 'Computer Science' AND salary > 70000;
```

### 5. For all instructors in the university who have taught some course, find their names and the course ID of all courses they taught.

Explanation: This query joins the `instructor` and `teaches` tables to find instructors and the courses they taught.

```sql
SELECT DISTINCT i.name, t.course_id
FROM instructor i
JOIN teaches t
ON i.ID = t.ID;
```

### 6. Find the names of all instructors whose salary is greater than at least one instructor in the Biology department.

Explanation: This query finds the instructors whose salary is greater than the salary of at least one instructor in the 'Biology' department.

```sql
SELECT i.name
FROM instructor i
WHERE salary > ANY (
  SELECT salary
  FROM instructor
  WHERE dept_name = 'Biology'
);
```

### 7. Find the advisor of the student with ID 12345.

Explanation: This query joins the `instructor` and `advisor` tables to find the advisor of the student with ID 12345.

```sql
SELECT i.name
FROM instructor i
JOIN advisor a
ON i.ID = a.i_ID
WHERE a.s_ID = '12345';
```

### 8. Find the average salary of all instructors.

Explanation: This query calculates the average salary of all instructors.

```sql
SELECT AVG(salary)
FROM instructor;
```

### 9. Find the names of all departments whose building name includes the substring Watson.

Explanation: This query finds department names where the building name includes 'Watson'.

```sql
SELECT dept_name
FROM department
WHERE building LIKE '%Watson%';
```

### 10. Find the names of instructors with salary amounts between \$90,000 and \$100,000.

Explanation: This query selects the `name` of instructors whose salary is between 90,000 and 100,000.

```sql
SELECT name
FROM instructor
WHERE salary BETWEEN 90000 AND 100000;
```

### 11. Find the instructor names and the courses they taught for all instructors in the Biology department who have taught some course.

Explanation: This query finds the `name` of instructors in the 'Biology' department and the courses they taught.

```sql
SELECT i.name, t.course_id
FROM instructor i
JOIN teaches t
ON i.ID = t.ID
WHERE i.dept_name = 'Biology';

```

### 12. Find the courses taught in Fall-2009 semester.

Explanation: This query selects the `course_id` of all courses taught in the Fall-2009 semester.

```sql
SELECT course_id
FROM section
WHERE semester = 'Fall' AND year = 2009;
```

### 13. Find the set of all courses taught either in Fall-2009 or in Spring-2010.

Explanation: This query selects all `course_id`s taught either in the Fall-2009 or Spring-2010 semesters.

```sql
SELECT DISTINCT course_id
FROM section
WHERE (semester = 'Fall' AND year = 2009)
OR
(semester = 'Spring' AND year = 2010);
```

### 14. Find the set of all courses taught in the Fall-2009 as well as in Spring-2010.

Explanation: This query finds all `course_id`s taught in both Fall-2009 and Spring-2010.

```sql
SELECT course_id
FROM section
WHERE (semester = 'Fall' AND year = 2009)
AND
(semester = 'Spring' AND year = 2010);
```

### 15. Find all courses taught in the Fall-2009 semester but not in the Spring-2010 semester.

Explanation: This query finds all `course_id`s taught in Fall-2009 but not in Spring-2010.

```sql
SELECT course_id
FROM section
WHERE (semester = 'Fall' AND year = 2009)
AND NOT course_id IN (
  SELECT course_id
  FROM section
  WHERE semester = 'Spring' AND year = 2010
);
```

### 16. Find all instructors who appear in the instructor relation with null values for salary.

Explanation: This query selects all rows in the `instructor` table where the salary is NULL.

```sql
SELECT * FROM instructor WHERE salary IS NULL;
```

### 17. Find the average salary of instructors in the Finance department.

Explanation: This query calculates the average salary of instructors in the 'Finance' department.

```sql
SELECT AVG(salary)
FROM instructor
WHERE dept_name = 'Finance';
```

### 18. Find the total number of instructors who teach a course in the Spring-2010 semester.

Explanation: This query counts the distinct instructors teaching a course in the Spring-2010 semester.

```sql
SELECT COUNT(DISTINCT i.ID)
FROM instructor i
JOIN teaches t
ON i.ID = t.ID
JOIN section s
ON t.course_id = s.course_id AND t.sec_id = s.sec_id
WHERE s.semester = 'Spring' AND s.year = 2010;

```

### 19. Find the average salary in each department.

Explanation: This query calculates the average salary of instructors in each department.

```sql
SELECT dept_name, AVG(salary)
FROM instructor
GROUP BY dept_name;
```

### 20. Find the number of instructors in each department who teach a course in the Spring-2010 semester.

Explanation: This query finds the number of instructors in each department who taught a course in Spring-2010.

```sql
SELECT d.dept_name, COUNT(DISTINCT i.ID)
FROM department d
LEFT JOIN instructor i
ON d.dept_name = i.dept_name
LEFT JOIN teaches t
ON i.ID = t.ID
LEFT JOIN section s
ON t.course_id = s.course_id AND t.sec_id = s.sec_id
AND s.semester = 'Spring' AND s.year = 2010
GROUP BY d.dept_name;

```

### 21. List out the departments where the average salary of the instructors is more than \$42,000.

Explanation: This query finds departments where the average salary of instructors is greater than 42,000.

```sql
SELECT dept_name
FROM instructor
GROUP BY dept_name
HAVING AVG(salary) > 42000;
```

### 22. For each course section offered in 2009, find the average total credits (tot cred) of all students enrolled in the section, if the section had at least 2 students.

Explanation: This query finds the average total credits of students in each course section offered in 2009 with at least two students.

```sql
select s.course_id, s.sec_id, avg(p.tot_cred) as avg_tot_cred
from section s 
join takes t join student p 
on s.course_id = t.course_id and t.sec_id = s.sec_id and p.ID = t.ID and s.year = 2009
group by s.course_id, s.sec_id 
having count(distinct t.ID)>=2

```

### 23. Find all the courses taught in both the Fall-2009 and Spring-2010 semesters.

Explanation: This query selects all `course_id`s taught in both Fall-2009 and Spring-2010.

```sql
SELECT course_id
FROM section
WHERE (semester = 'Fall' AND year = 2009)
AND course_id IN (
  SELECT course_id
  FROM section
  WHERE semester = 'Spring' AND year = 2010
);
```

### 24. Find all the courses taught in the Fall-2009 semester but not in the Spring-2010 semester.

Explanation: This query selects all `course_id`s taught in Fall-2009 but not in Spring-2010.

```sql
SELECT course_id
FROM section
WHERE (semester = 'Fall' AND year = 2009)
AND NOT course_id IN (
  SELECT course_id
  FROM section
  WHERE semester = 'Spring' AND year = 2010
);
```

### 25. Select the names of instructors whose names are neither 'Mozart' nor 'Einstein'.

Explanation: This query selects the `name` of instructors whose names are not 'Mozart' or 'Einstein'.

```sql
SELECT name
FROM instructor
WHERE name NOT IN ('Mozart', 'Einstein');
```

### 26. Find the total number of (distinct) students who have taken course sections taught by the instructor with ID 110011.

Explanation: This query counts the distinct students who have taken courses taught by the instructor with ID 110011.

```sql
SELECT COUNT(DISTINCT t.ID)
FROM teaches te
JOIN takes t
ON te.course_id = t.course_id 
AND te.sec_id = t.sec_id
WHERE te.ID = '110011';
```

### 27. Find the ID and names of all instructors whose salary is greater than at least one instructor in the History department.
Explanation: This query finds the ID and name of instructors whose salary is greater than any instructor in the 'History' department.
```sql
SELECT ID, name
FROM instructor
WHERE salary > ANY (
  SELECT salary
  FROM instructor
  WHERE dept_name = 'History'
);

```
### 28. Find the names of all instructors that have a salary value greater than that of each instructor in the Biology department.
```sql
SELECT i.name
FROM instructor i
WHERE salary > ALL (
  SELECT salary
  FROM instructor
  WHERE dept_name = 'Biology'
);
```
### 29. Find the departments that have the highest average salary.
```sql
SELECT dept_name, AVG(salary) AS avg_salary
FROM instructor
GROUP BY dept_name
ORDER BY avg_salary DESC
LIMIT 1;
```
### 30. Find all courses taught in both the Fall 2009 semester and in the Spring-2010 semester. 
```sql
SELECT course_id
FROM section
WHERE (semester = 'Fall' AND year = 2009)
AND course_id IN (
  SELECT course_id
  FROM section
  WHERE semester = 'Spring' AND year = 2010
);
```
### 31. Find all students who have taken all the courses offered in the Biology department.
```sql
SELECT s.ID, s.name
FROM student s
WHERE NOT EXISTS (
  SELECT c.course_id
  FROM course c
  WHERE c.dept_name = 'Biology'
  AND NOT EXISTS (
    SELECT t.course_id
    FROM takes t
    WHERE t.ID = s.ID AND t.course_id = c.course_id
  )
);
```
### 32. Find all courses that were offered at most once in 2009.
```sql
SELECT course_id
FROM section
WHERE year = 2009
GROUP BY course_id
HAVING COUNT(DISTINCT semester) <= 1;
```
### 33. Find all courses that were offered at least twice in 2009.
```sql
SELECT course_id
FROM section
WHERE year = 2009
GROUP BY course_id
HAVING COUNT(DISTINCT semester) >= 2;

```
### 34. Find the average instructors9 salaries of those departments where the average salary is
greater than $42,000.
```sql
SELECT dept_name, AVG(salary) as avg_salary
FROM instructor
GROUP BY dept_name
HAVING AVG(salary) > 42000;
```
### 35. Find the maximum across all departments of the total salary at each department. 
```sql
SELECT dept_name, MAX(total_salary) AS max_total_salary
FROM (
  SELECT dept_name, SUM(salary) AS total_salary
  FROM instructor
  GROUP BY dept_name
) AS department_salary;

```
### 36.  List all departments along with the number of instructors in each department.
```sql
SELECT d.dept_name, COUNT(i.ID) as num_instructors
FROM department d
LEFT JOIN instructor i
ON d.dept_name = i.dept_name
GROUP BY d.dept_name;

```
### 37. Find the titles of courses in the Comp. Sci. department that has 3 credits.
```sql
SELECT title
FROM course
WHERE dept_name = 'Comp. Sci.' AND credits = 3;
```
### 38. Find the IDs of all students who were taught by an instructor named Einstein; make sure there are no duplicates in the result.
```sql
SELECT DISTINCT t.ID
FROM takes t
JOIN teaches te
ON t.course_id = te.course_id AND t.sec_id = te.sec_id
JOIN instructor i
ON te.ID = i.ID
WHERE i.name = 'Einstein';
```
### 39. Find the highest salary of any instructor.
```sql
SELECT MAX(salary) as max_salary
FROM instructor;
```
### 40. Find all instructors earning the highest salary (there may be more than one with the same salary).
```sql
SELECT ID, name, salary
FROM instructor
WHERE salary = (
  SELECT MAX(salary)
  FROM instructor
);
```
### 41. Find the enrollment of each section that was offered in Autumn-2009. 
```sql
SELECT course_id, sec_id, COUNT(ID) AS enrollment
FROM takes
WHERE semester = 'Autumn' AND year = 2009
GROUP BY course_id, sec_id;
```

### 42.  Find the maximum enrollment, across all sections, in Autumn-2009.
```sql
SELECT MAX(enrollment) AS max_enrollment
FROM (
    SELECT course_id, sec_id, COUNT(ID) AS enrollment
    FROM takes
    WHERE semester = 'Autumn' AND year = 2009
    GROUP BY course_id, sec_id
) AS section_enrollments;
```
### 43.  Find the salaries after the following operation: Increase the salary of each instructor in the Comp. Sci. department by 10%.
```sql
SELECT ID, name, dept_name, salary * 1.10 AS increased_salary
FROM instructor
WHERE dept_name = 'Comp. Sci.';
```
### 44.  Find all students who have not taken a course.
```sql
SELECT *
FROM student
WHERE ID NOT IN (
    SELECT ID FROM takes
);
```
### 45. List all course sections offered by the Physics department in the Fall-2009 semester, with the building and room number of each section.
```sql
SELECT course_id, sec_id, building, room_number
FROM section
WHERE semester = 'Fall' AND year = 2009
  AND course_id IN (
      SELECT course_id
      FROM course
      WHERE dept_name = 'Physics'
  );
```
### 46. Find the student names who take courses in Spring-2010 semester at Watson Building.
```sql
SELECT DISTINCT student.name
FROM student
JOIN takes ON student.ID = takes.ID
JOIN section ON takes.course_id = section.course_id 
            AND takes.sec_id = section.sec_id 
            AND takes.semester = section.semester 
            AND takes.year = section.year
WHERE section.semester = 'Spring' 
  AND section.year = 2010 
  AND section.building = 'Watson';
```
### 47. List the students who take courses teaches by Brandt.
```sql
SELECT DISTINCT student.name
FROM student
JOIN takes ON student.ID = takes.ID
JOIN teaches ON takes.course_id = teaches.course_id 
JOIN instructor ON teaches.ID = instructor.ID
WHERE instructor.name = 'Brandt';
```
### 48. Find out the average salary of the instructor in each department.
```sql
SELECT dept_name, AVG(salary) AS avg_salary
FROM instructor
GROUP BY dept_name;
```
### 49. Find the number of students who take the course titled Intro. To Computer Science.
```sql
SELECT COUNT(DISTINCT ID) AS num_students
FROM takes
WHERE course_id = 'Intro. To Computer Science';
```
### 50. Find out the total salary of the instructors of the Computer Science department who take a course(s) in Watson building.
```sql
SELECT SUM(salary) AS total_salary
FROM instructor
WHERE dept_name = 'Computer Science'
  AND ID IN (
      SELECT teaches.ID
      FROM teaches
      JOIN section ON teaches.course_id = section.course_id
      WHERE section.building = 'Watson'
  );
```
### 51. Find out the course titles which starts between 10:00 to 12:00.
```sql
SELECT title
FROM course
WHERE course_id IN (
  SELECT course_id
  FROM section
  JOIN time_slot USING(time_slot_id)
  WHERE start_time BETWEEN '10:00:00' AND '12:00:00'
);
```
### 52. List the course names where CS-1019 is the pre-requisite course.
```sql
SELECT course.course_name
FROM course
JOIN prereq ON course.course_id = prereq.course_id
WHERE prereq.prereq_id = 'CS-1019';

```
### 53. List the student names who get more than B+ grades in their respective courses.
```sql
SELECT name
FROM student
WHERE ID IN (
  SELECT ID
  FROM takes
  WHERE grade > 'B+'
);
```
### 54. Find the student who takes the maximum credit from each department.
```sql
SELECT s.ID, s.name, s.dept_name, MAX(tot_cred) as max_credits
FROM student s
GROUP BY s.dept_name;
```
### 55. Find out the student ID and grades who take a course(s) in Spring-2009 semester.
```sql
SELECT ID, grade
FROM takes
WHERE semester = 'Spring' AND year = 2009;
```
### 56. Find the building(s) where the student takes the course titled Image Processing.
```sql
SELECT DISTINCT building
FROM section
WHERE course_id = (
  SELECT course_id FROM course WHERE title = 'Image Processing'
);
```
### 57. Find the room no. and the building where the student from Fall-2009 semester can take a course(s)
```sql
SELECT DISTINCT building, room_number
FROM section
WHERE semester = 'Fall' AND year = 2009;
```
### 58. Find the names of those departments whose budget is higher than that of Astronomy. List them in alphabetic order
```sql
SELECT dept_name
FROM department
WHERE budget > (
  SELECT budget FROM department WHERE dept_name = 'Astronomy'
)
ORDER BY dept_name;
```
### 59. Display a list of all instructors, showing each instructor's ID and the number of sections taught. Make sure to show the number of sections as 0 for instructors who have not taught any section
```sql
SELECT instructor.id, COUNT(teaches.course_id) AS num_sections
FROM instructor
LEFT JOIN teaches ON instructor.id = teaches.id
GROUP BY instructor.id
order by num_sections asc;
```
### 60. For each student who has retaken a course at least twice (i.e., the student has taken the courseat least three times), show the course ID and the student's ID. Please display your results in order of course ID and do not display duplicate rows
```sql
SELECT DISTINCT id, course_id
FROM takes
GROUP BY id, course_id
HAVING COUNT(*) >= 3
ORDER BY course_id;
```
### 61. Find the names of Biology students who have taken at least 3 Accounting courses
```sql
SELECT s.name
FROM student s
WHERE s.dept_name = 'Biology' AND s.ID IN (
  SELECT t.ID
  FROM takes t
  JOIN course c ON t.course_id = c.course_id
  WHERE c.dept_name = 'Accounting'
  GROUP BY t.ID
  HAVING COUNT(*) >= 3
);
```
### 62. Find the sections that had maximum enrollment in Fall 2010
```sql
SELECT course_id, sec_id, semester, year
FROM takes
WHERE semester = 'Fall' AND year = 2010
GROUP BY course_id, sec_id, semester, year
HAVING COUNT(id) = (
    SELECT MAX(cnt)
    FROM (
        SELECT COUNT(id) AS cnt
        FROM takes
        WHERE semester = 'Fall' AND year = 2010
        GROUP BY course_id, sec_id, semester, year
    ) AS counts
);
```
### 63. Find student names and the number of law courses taken for students who have taken at least half of the available law courses. (These courses are named things like 'Tort Law' or Environmental Law'
```sql
SELECT s.name, COUNT(t.course_id) AS law_courses_taken
FROM student s
JOIN takes t ON s.ID = t.ID
JOIN course c ON t.course_id = c.course_id
WHERE c.dept_name = 'Law'
GROUP BY s.ID
HAVING COUNT(t.course_id) >= (
  SELECT COUNT(*) / 2
  FROM course
  WHERE dept_name = 'Law'
);
```
### 64. Find the rank and name of the 10 students who earned the most A grades (A-, A, A+). Use alphabetical order by name to break ties.
```sql
SELECT s.name, RANK() OVER (ORDER BY COUNT(t.grade) DESC, s.name) AS rank
FROM student s
JOIN takes t ON s.ID = t.ID
WHERE t.grade IN ('A-', 'A', 'A+')
GROUP BY s.ID
LIMIT 10;
```
### 65. Find the titles of courses in the Comp. Sci. department that have 3 credits.
```sql
SELECT title
FROM course
WHERE dept_name = 'Comp. Sci.' AND credits = 3;
```
### 66. Find the IDs of all students who were taught by an instructor named Einstein; make sure there are no duplicates in the result.
```sql
SELECT DISTINCT t.ID
FROM takes t
JOIN teaches te ON t.course_id = te.course_id
JOIN instructor i ON te.ID = i.ID
WHERE i.name = 'Einstein';
```
### 67. Find the ID and name of each student who has taken at least one Comp. Sci. course; make sure there are no duplicate names in the result.
```sql
SELECT DISTINCT s.ID, s.name
FROM student s
JOIN takes t ON s.ID = t.ID
JOIN course c ON t.course_id = c.course_id
WHERE c.dept_name = 'Comp. Sci.';
```
### 68. Find the course id, section id, and building for each section of a Biology course.
```sql
SELECT s.course_id, s.sec_id, s.building
FROM section s
JOIN course c ON s.course_id = c.course_id
WHERE c.dept_name = 'Biology';
```
### 69. Output instructor names sorted by the ratio of their salary to their department's budget (in ascending order).
```sql
SELECT i.name
FROM instructor i
JOIN department d ON i.dept_name = d.dept_name
ORDER BY i.salary / d.budget ASC;
```
### 70. Output instructor names and buildings for each building an instructor has taught in. Include instructor names who have not taught any classes (the building name should be NULL in this case).
```sql
SELECT i.name, s.building
FROM instructor i
LEFT JOIN teaches t ON i.ID = t.ID
LEFT JOIN section s ON t.course_id = s.course_id AND t.sec_id = s.sec_id
ORDER BY i.name;
```
