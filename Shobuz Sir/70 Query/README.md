
---




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
SELECT DISTINCT i.name, t.course_id FROM instructor i JOIN teaches t ON i.ID = t.ID;
```

### 6. Find the names of all instructors whose salary is greater than at least one instructor in the Biology department.

Explanation: This query finds the instructors whose salary is greater than the salary of at least one instructor in the 'Biology' department.

```sql
SELECT i.name FROM instructor i WHERE salary > ANY ( SELECT salary FROM instructor WHERE dept_name = 'Biology');
```

### 7. Find the advisor of the student with ID 12345.

Explanation: This query joins the `instructor` and `advisor` tables to find the advisor of the student with ID 12345.

```sql
SELECT i.name FROM instructor i JOIN advisor a ON i.ID = a.i_ID WHERE a.s_ID = '12345';
```

### 8. Find the average salary of all instructors.

Explanation: This query calculates the average salary of all instructors.

```sql
SELECT AVG(salary) FROM instructor;
```

### 9. Find the names of all departments whose building name includes the substring Watson.

Explanation: This query finds department names where the building name includes 'Watson'.

```sql
SELECT dept_name FROM department WHERE building LIKE '%Watson%';
```

### 10. Find the names of instructors with salary amounts between \$90,000 and \$100,000.

Explanation: This query selects the `name` of instructors whose salary is between 90,000 and 100,000.

```sql
SELECT name FROM instructor WHERE salary BETWEEN 90000 AND 100000;
```

### 11. Find the instructor names and the courses they taught for all instructors in the Biology department who have taught some course.

Explanation: This query finds the `name` of instructors in the 'Biology' department and the courses they taught.

```sql
SELECT i.name, t.course_id FROM instructor i JOIN teaches t ON i.ID = t.ID WHERE i.dept_name = 'Biology';
```

### 12. Find the courses taught in Fall-2009 semester.

Explanation: This query selects the `course_id` of all courses taught in the Fall-2009 semester.

```sql
SELECT course_id FROM section WHERE semester = 'Fall' AND year = 2009;
```

### 13. Find the set of all courses taught either in Fall-2009 or in Spring-2010.

Explanation: This query selects all `course_id`s taught either in the Fall-2009 or Spring-2010 semesters.

```sql
SELECT DISTINCT course_id FROM section WHERE (semester = 'Fall' AND year = 2009) OR (semester = 'Spring' AND year = 2010);
```

### 14. Find the set of all courses taught in the Fall-2009 as well as in Spring-2010.

Explanation: This query finds all `course_id`s taught in both Fall-2009 and Spring-2010.

```sql
SELECT course_id FROM section WHERE (semester = 'Fall' AND year = 2009) AND (semester = 'Spring' AND year = 2010);
```

### 15. Find all courses taught in the Fall-2009 semester but not in the Spring-2010 semester.

Explanation: This query finds all `course_id`s taught in Fall-2009 but not in Spring-2010.

```sql
SELECT course_id FROM section WHERE (semester = 'Fall' AND year = 2009) AND NOT course_id IN (SELECT course_id FROM section WHERE semester = 'Spring' AND year = 2010);
```

### 16. Find all instructors who appear in the instructor relation with null values for salary.

Explanation: This query selects all rows in the `instructor` table where the salary is NULL.

```sql
SELECT * FROM instructor WHERE salary IS NULL;
```

### 17. Find the average salary of instructors in the Finance department.

Explanation: This query calculates the average salary of instructors in the 'Finance' department.

```sql
SELECT AVG(salary) FROM instructor WHERE dept_name = 'Finance';
```

### 18. Find the total number of instructors who teach a course in the Spring-2010 semester.

Explanation: This query counts the distinct instructors teaching a course in the Spring-2010 semester.

```sql
SELECT COUNT(DISTINCT i.ID) FROM instructor i JOIN teaches t ON i.ID = t.ID JOIN section s ON t.course_id = s.course_id AND t.sec_id = s.sec_id WHERE s.semester = 'Spring' AND s.year = 2010;
```

### 19. Find the average salary in each department.

Explanation: This query calculates the average salary of instructors in each department.

```sql
SELECT dept_name, AVG(salary) FROM instructor GROUP BY dept_name;
```

### 20. Find the number of instructors in each department who teach a course in the Spring-2010 semester.

Explanation: This query finds the number of instructors in each department who taught a course in Spring-2010.

```sql
SELECT d.dept_name, COUNT(DISTINCT i.ID) FROM department d LEFT JOIN instructor i ON d.dept_name = i.dept_name LEFT JOIN teaches t ON i.ID = t.ID LEFT JOIN section s ON t.course_id = s.course_id AND t.sec_id = s.sec_id AND s.semester = 'Spring' AND s.year = 2010 GROUP BY d.dept_name;
```

### 21. List out the departments where the average salary of the instructors is more than \$42,000.

Explanation: This query finds departments where the average salary of instructors is greater than 42,000.

```sql
SELECT dept_name FROM instructor GROUP BY dept_name HAVING AVG(salary) > 42000;
```

### 22. For each course section offered in 2009, find the average total credits (tot cred) of all students enrolled in the section, if the section had at least 2 students.

Explanation: This query finds the average total credits of students in each course section offered in 2009 with at least two students.

```sql
SELECT s.course_id, s.sec_id, AVG(t.tot_cred) as avg_tot_cred FROM section s JOIN takes t ON s.course_id = t.course_id AND s.sec_id = t.sec_id AND s.year = 2009 GROUP BY s.course_id, s.sec_id HAVING COUNT(DISTINCT t.ID) >= 2;
```

### 23. Find all the courses taught in both the Fall-2009 and Spring-2010 semesters.

Explanation: This query selects all `course_id`s taught in both Fall-2009 and Spring-2010.

```sql
SELECT course_id FROM section WHERE (semester = 'Fall' AND year = 2009) AND course_id IN (SELECT course_id FROM section WHERE semester = 'Spring' AND year = 2010);
```

### 24. Find all the courses taught in the Fall-2009 semester but not in the Spring-2010 semester.

Explanation: This query selects all `course_id`s taught in Fall-2009 but not in Spring-2010.

```sql
SELECT course_id FROM section WHERE (semester = 'Fall' AND year = 2009) AND NOT course_id IN (SELECT course_id FROM section WHERE semester = 'Spring' AND year = 2010);
```

### 25. Select the names of instructors whose names are neither 'Mozart' nor 'Einstein'.

Explanation: This query selects the `name` of instructors whose names are not 'Mozart' or 'Einstein'.

```sql
SELECT name FROM instructor WHERE name NOT IN ('Mozart', 'Einstein');
```

### 26. Find the total number of (distinct) students who have taken course sections taught by the instructor with ID 110011.

Explanation: This query counts the distinct students who have taken courses taught by the instructor with ID 110011.

```sql
SELECT COUNT(DISTINCT t.ID) FROM teaches te JOIN takes t ON te.course_id = t.course_id AND te.sec_id = t.sec_id WHERE te.ID = '110011';
```

### 27. Find the ID and names of all instructors whose salary is greater than at least one instructor in the History department.
Explanation: This query finds the ID and name of instructors whose salary is greater than any instructor in the 'History' department.
```sql
SELECT ID, name FROM instructor WHERE salary > ANY (SELECT salary FROM instructor WHERE dept_name = 'History');
```


### 28. Find the names of the students who have enrolled in at least one course taught by the instructor with ID 110011.
Explanation: This query selects the names of students who have enrolled in at least one course taught by the instructor with ID 110011.
```sql
SELECT DISTINCT s.name FROM student s JOIN takes t ON s.ID = t.ID JOIN section sec ON t.course_id = sec.course_id AND t.sec_id = sec.sec_id JOIN teaches te ON sec.course_id = te.course_id AND sec.sec_id = te.sec_id WHERE te.ID = '110011';
````

### 29. Find all the courses that have been taught by instructors from either the Physics or Chemistry department.

Explanation: This query finds all `course_id`s taught by instructors in the 'Physics' or 'Chemistry' departments.

```sql
SELECT DISTINCT t.course_id FROM teaches t JOIN instructor i ON t.ID = i.ID WHERE i.dept_name IN ('Physics', 'Chemistry');
```

### 30. Find the number of courses offered by each department.

Explanation: This query finds the number of courses offered by each department by counting `course_id` in each department.

```sql
SELECT dept_name, COUNT(DISTINCT course_id) FROM section GROUP BY dept_name;
```

### 31. List all students who have enrolled in all courses offered in the Spring-2010 semester.

Explanation: This query selects the students who have enrolled in every course offered in Spring-2010.

```sql
SELECT s.name FROM student s WHERE NOT EXISTS (SELECT course_id FROM section WHERE semester = 'Spring' AND year = 2010 EXCEPT SELECT t.course_id FROM takes t WHERE t.ID = s.ID);
```

### 32. Find the names of instructors who have taught the course 'CS101' in both Fall-2009 and Spring-2010.

Explanation: This query selects the names of instructors who taught 'CS101' in both Fall-2009 and Spring-2010.

```sql
SELECT DISTINCT i.name FROM instructor i JOIN teaches t ON i.ID = t.ID JOIN section s ON t.course_id = s.course_id AND t.sec_id = s.sec_id WHERE s.course_id = 'CS101' AND ((s.semester = 'Fall' AND s.year = 2009) OR (s.semester = 'Spring' AND s.year = 2010));
```

### 33. Find the names of all instructors in the 'Mathematics' department who have taught at least one course with a course ID of 'MATH101'.

Explanation: This query finds instructors in the 'Mathematics' department who have taught at least one 'MATH101' course.

```sql
SELECT i.name FROM instructor i JOIN teaches t ON i.ID = t.ID WHERE i.dept_name = 'Mathematics' AND t.course_id = 'MATH101';
```

### 34. Find the total number of students who have enrolled in courses taught by instructor with ID '12345'.

Explanation: This query finds the total number of students who have enrolled in courses taught by the instructor with ID '12345'.

```sql
SELECT COUNT(DISTINCT t.ID) FROM takes t JOIN section s ON t.course_id = s.course_id AND t.sec_id = s.sec_id JOIN teaches te ON s.course_id = te.course_id AND s.sec_id = te.sec_id WHERE te.ID = '12345';
```

### 35. List the departments where the average salary is greater than \$60,000.

Explanation: This query selects the departments where the average salary of instructors is greater than 60,000.

```sql
SELECT dept_name FROM instructor GROUP BY dept_name HAVING AVG(salary) > 60000;
```

### 36. List the names of the students who have enrolled in 'CS101' in Fall-2009.

Explanation: This query selects the names of students who enrolled in 'CS101' in Fall-2009.

```sql
SELECT DISTINCT s.name FROM student s JOIN takes t ON s.ID = t.ID JOIN section sec ON t.course_id = sec.course_id AND t.sec_id = sec.sec_id WHERE sec.course_id = 'CS101' AND sec.semester = 'Fall' AND sec.year = 2009;
```

### 37. Find the instructors who are teaching courses in both the Fall-2009 and Spring-2010 semesters.

Explanation: This query selects the names of instructors teaching courses in both Fall-2009 and Spring-2010.

```sql
SELECT DISTINCT i.name FROM instructor i JOIN teaches t ON i.ID = t.ID JOIN section sec ON t.course_id = sec.course_id AND t.sec_id = sec.sec_id WHERE (sec.semester = 'Fall' AND sec.year = 2009) AND EXISTS (SELECT 1 FROM section sec2 WHERE sec2.semester = 'Spring' AND sec2.year = 2010 AND sec2.course_id = sec.course_id AND sec2.sec_id = sec.sec_id);
```

### 38. Find the students who have not enrolled in any courses taught by 'Prof. Smith'.

Explanation: This query selects the names of students who have not enrolled in any courses taught by 'Prof. Smith'.

```sql
SELECT DISTINCT s.name FROM student s WHERE NOT EXISTS (SELECT 1 FROM takes t JOIN section sec ON t.course_id = sec.course_id AND t.sec_id = sec.sec_id JOIN teaches te ON sec.course_id = te.course_id AND sec.sec_id = te.sec_id WHERE te.name = 'Prof. Smith' AND t.ID = s.ID);
```

### 39. List the department names that have a budget greater than \$500,000.

Explanation: This query selects department names that have a budget greater than 500,000.

```sql
SELECT dept_name FROM department WHERE budget > 500000;
```

### 40. Find the courses that were taught by 'Prof. Green' in the 'History' department.

Explanation: This query selects the courses taught by 'Prof. Green' in the 'History' department.

```sql
SELECT DISTINCT t.course_id FROM teaches t JOIN instructor i ON t.ID = i.ID WHERE i.name = 'Prof. Green' AND i.dept_name = 'History';
```

### 41. Find the names of instructors who have taught courses in more than one department.

Explanation: This query finds the names of instructors who have taught courses in multiple departments.

```sql
SELECT DISTINCT i.name FROM instructor i JOIN teaches t ON i.ID = t.ID JOIN section s ON t.course_id = s.course_id WHERE i.dept_name IN (SELECT dept_name FROM instructor WHERE ID = i.ID GROUP BY dept_name HAVING COUNT(DISTINCT dept_name) > 1);
```

### 42. Find the names of students who have taken exactly 3 courses.

Explanation: This query selects the names of students who have taken exactly 3 courses.

```sql
SELECT s.name FROM student s JOIN takes t ON s.ID = t.ID GROUP BY s.ID HAVING COUNT(DISTINCT t.course_id) = 3;
```

### 43. Find the total number of students in the university.

Explanation: This query counts the total number of students in the university.

```sql
SELECT COUNT(*) FROM student;
```

### 44. Find the departments that have at least one instructor with a salary greater than \$100,000.

Explanation: This query finds departments where at least one instructor has a salary greater than 100,000.

```sql
SELECT DISTINCT dept_name FROM instructor WHERE salary > 100000;
```

### 45. Find the total number of courses offered in Spring-2010.

Explanation: This query counts the number of courses offered in Spring-2010.

```sql
SELECT COUNT(DISTINCT course_id) FROM section WHERE semester = 'Spring' AND year = 2010;
```

### 46. List the courses that have been taught by at least one instructor with the name 'Dr. Brown'.

Explanation: This query finds the courses taught by at least one instructor named 'Dr. Brown'.

```sql
SELECT DISTINCT course_id FROM teaches t JOIN instructor i ON t.ID = i.ID WHERE i.name = 'Dr. Brown';
```

### 47. Find all instructors who taught a course in the 'Engineering' department in Fall-2009.

Explanation: This query finds all instructors who taught a course in the 'Engineering' department in Fall-2009.

```sql
SELECT DISTINCT i.name FROM instructor i JOIN teaches t ON i.ID = t.ID JOIN section sec ON t.course_id = sec.course_id WHERE i.dept_name = 'Engineering' AND sec.semester = 'Fall' AND sec.year = 2009;
```

### 48. Find all students who have enrolled in 'CS101' and 'CS102'.

Explanation: This query finds the names of students who have enrolled in both 'CS101' and 'CS102'.

```sql
SELECT s.name FROM student s JOIN takes t ON s.ID = t.ID WHERE t.course_id = 'CS101' AND s.ID IN (SELECT t2.ID FROM takes t2 WHERE t2.course_id = 'CS102');
```

### 49. List the instructors who taught courses in the Spring-2010 semester and have a salary greater than \$90,000.

Explanation: This query finds the names of instructors who taught in Spring-2010 and have a salary greater than 90,000.

```sql
SELECT DISTINCT i.name FROM instructor i JOIN teaches t ON i.ID = t.ID JOIN section s ON t.course_id = s.course_id AND t.sec_id = s.sec_id WHERE s.semester = 'Spring' AND s.year = 2010 AND i.salary > 90000;
```

### 50. Find the total number of instructors in the 'Computer Science' department.

Explanation: This query counts the total number of instructors in the 'Computer Science' department.

```sql
SELECT COUNT(*) FROM instructor WHERE dept_name = 'Computer Science';
```

### 51. Find all students who have taken courses in both the Fall-2009 and Spring-2010 semesters.

Explanation: This query finds the students who have enrolled in courses in both Fall-2009 and Spring-2010.

```sql
SELECT DISTINCT s.name FROM student s WHERE EXISTS (SELECT 1 FROM takes t JOIN section sec ON t.course_id = sec.course_id AND t.sec_id = sec.sec_id WHERE sec.semester = 'Fall' AND sec.year = 2009 AND t.ID = s.ID) AND EXISTS (SELECT 1 FROM takes t JOIN section sec ON t.course_id = sec.course_id AND t.sec_id = sec.sec_id WHERE sec.semester = 'Spring' AND sec.year = 2010 AND t.ID = s.ID);
```

### 52. Find the names of all instructors who have a salary between \$75,000 and \$95,000.

Explanation: This query selects the names of instructors whose salary is between 75,000 and 95,000.

```sql
SELECT name FROM instructor WHERE salary BETWEEN 75000 AND 95000;
```

### 53. Find all departments where no instructor's salary exceeds \$120,000.

Explanation: This query finds departments where no instructor has a salary exceeding 120,000.

```sql
SELECT dept_name FROM instructor GROUP BY dept_name HAVING MAX(salary) <= 120000;
```

### 54. Find all students who have not taken a course in the Spring-2010 semester.

Explanation: This query selects all students who have not enrolled in any course in Spring-2010.

```sql
SELECT DISTINCT s.name FROM student s WHERE NOT EXISTS (SELECT 1 FROM takes t JOIN section sec ON t.course_id = sec.course_id AND t.sec_id = sec.sec_id WHERE sec.semester = 'Spring' AND sec.year = 2010 AND t.ID = s.ID);
```

### 55. Find all courses taught by 'Prof. Johnson' in 2009.

Explanation: This query finds all courses taught by 'Prof. Johnson' in 2009.

```sql
SELECT DISTINCT t.course_id FROM teaches t JOIN instructor i ON t.ID = i.ID WHERE i.name = 'Prof. Johnson' AND t.year = 2009;
```

### 56. Find the students who have enrolled in a course where the instructor is named 'Dr. Harris'.

Explanation: This query selects the students who have enrolled in a course taught by 'Dr. Harris'.

```sql
SELECT DISTINCT s.name FROM student s JOIN takes t ON s.ID = t.ID JOIN section sec ON t.course_id = sec.course_id AND t.sec_id = sec.sec_id JOIN teaches te ON sec.course_id = te.course_id AND sec.sec_id = te.sec_id WHERE te.name = 'Dr. Harris';
```

### 57. Find the instructors who taught 'CS102' in both Fall-2009 and Spring-2010.

Explanation: This query finds instructors who taught 'CS102' in both Fall-2009 and Spring-2010.

```sql
SELECT DISTINCT i.name FROM instructor i JOIN teaches t ON i.ID = t.ID JOIN section sec ON t.course_id = sec.course_id WHERE sec.course_id = 'CS102' AND ((sec.semester = 'Fall' AND sec.year = 2009) OR (sec.semester = 'Spring' AND sec.year = 2010));
```

### 58. Find all students who have not enrolled in any course in 2009.

Explanation: This query finds students who have not enrolled in any course in 2009.

```sql
SELECT s.name FROM student s WHERE NOT EXISTS (SELECT 1 FROM takes t JOIN section sec ON t.course_id = sec.course_id AND t.sec_id = sec.sec_id WHERE sec.year = 2009 AND t.ID = s.ID);
```

### 59. List the departments that have a budget greater than the average budget across all departments.

Explanation: This query selects departments with a budget greater than the average budget of all departments.

```sql
SELECT dept_name FROM department WHERE budget > (SELECT
```


AVG(budget) FROM department);

````

### 60. Find the instructors who have taught 'MATH101' and 'CS102'.
Explanation: This query finds the instructors who have taught both 'MATH101' and 'CS102'.
```sql
SELECT DISTINCT i.name FROM instructor i JOIN teaches t ON i.ID = t.ID WHERE t.course_id IN ('MATH101', 'CS102') GROUP BY i.name HAVING COUNT(DISTINCT t.course_id) = 2;
````

### 61. Find the students who have taken a course in the 'Computer Science' department.

Explanation: This query finds students who have enrolled in courses from the 'Computer Science' department.

```sql
SELECT DISTINCT s.name FROM student s JOIN takes t ON s.ID = t.ID JOIN section sec ON t.course_id = sec.course_id WHERE sec.dept_name = 'Computer Science';
```

### 62. Find the instructors who have a salary less than the average salary of instructors in their department.

Explanation: This query finds instructors whose salary is lower than the average salary in their department.

```sql
SELECT name FROM instructor i WHERE salary < (SELECT AVG(salary) FROM instructor WHERE dept_name = i.dept_name);
```

### 63. Find the names of students who have taken a course in the 'Engineering' department.

Explanation: This query finds students who have enrolled in courses in the 'Engineering' department.

```sql
SELECT DISTINCT s.name FROM student s JOIN takes t ON s.ID = t.ID JOIN section sec ON t.course_id = sec.course_id WHERE sec.dept_name = 'Engineering';
```

### 64. Find all instructors who have taught in both Fall and Spring semesters of the same year.

Explanation: This query finds instructors who have taught courses in both Fall and Spring semesters of the same year.

```sql
SELECT DISTINCT i.name FROM instructor i JOIN teaches t ON i.ID = t.ID JOIN section sec ON t.course_id = sec.course_id AND t.sec_id = sec.sec_id WHERE EXISTS (SELECT 1 FROM section sec2 WHERE sec2.semester = 'Spring' AND sec2.year = sec.year AND sec2.course_id = sec.course_id AND sec2.sec_id = sec.sec_id);
```

### 65. List the courses that have no students enrolled.

Explanation: This query lists courses that have no students enrolled in any section.

```sql
SELECT course_id FROM section WHERE course_id NOT IN (SELECT DISTINCT course_id FROM takes);
```

### 66. List the students who are enrolled in more than one course from the same department.

Explanation: This query finds students enrolled in multiple courses from the same department.

```sql
SELECT DISTINCT s.name FROM student s JOIN takes t ON s.ID = t.ID JOIN section sec ON t.course_id = sec.course_id WHERE s.ID IN (SELECT t2.ID FROM takes t2 JOIN section sec2 ON t2.course_id = sec2.course_id WHERE sec.dept_name = sec2.dept_name GROUP BY t2.ID HAVING COUNT(DISTINCT sec2.course_id) > 1);
```

### 67. Find all courses that have been taught by instructors from more than one department.

Explanation: This query finds courses that have been taught by instructors from multiple departments.

```sql
SELECT DISTINCT course_id FROM teaches t JOIN instructor i ON t.ID = i.ID GROUP BY t.course_id HAVING COUNT(DISTINCT i.dept_name) > 1;
```

### 68. List the students who have not enrolled in any course during the Spring-2010 semester.

Explanation: This query lists students who did not enroll in any course in Spring-2010.

```sql
SELECT DISTINCT s.name FROM student s WHERE NOT EXISTS (SELECT 1 FROM takes t JOIN section sec ON t.course_id = sec.course_id AND t.sec_id = sec.sec_id WHERE sec.semester = 'Spring' AND sec.year = 2010 AND t.ID = s.ID);
```

### 69. Find the courses taught by instructors with a salary greater than \$100,000.

Explanation: This query finds the courses taught by instructors with a salary greater than 100,000.

```sql
SELECT DISTINCT course_id FROM teaches t JOIN instructor i ON t.ID = i.ID WHERE i.salary > 100000;
```

### 70. List the names of students who have enrolled in the 'MATH101' course but not in 'CS101'.

Explanation: This query finds the names of students who have enrolled in 'MATH101' but not in 'CS101'.

```sql
SELECT DISTINCT s.name FROM student s JOIN takes t ON s.ID = t.ID WHERE t.course_id = 'MATH101' AND s.ID NOT IN (SELECT t2.ID FROM takes t2 WHERE t2.course_id = 'CS101');
```

---



