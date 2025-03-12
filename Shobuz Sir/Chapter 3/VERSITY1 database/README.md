## SQL Queries to Execute After Importing `drop.sql` and `largeRelationsInsertFile.sql`

Below is a list of SQL queries to execute in the `VERSITY1` database. These queries are designed to retrieve and manipulate data from the imported tables.

---

### Basic Queries

1. **Retrieve Department Names from Instructor Table**
   ```sql
   SELECT dept_name FROM instructor;
   ```

2. **Calculate 10% Salary Increase for Instructors**
   ```sql
   SELECT ID, name, dept_name, salary * 1.1 FROM instructor;
   ```

3. **Retrieve Instructors in Computer Science with Salary > 70000**
   ```sql
   SELECT name FROM instructor WHERE dept_name = 'Comp. Sci.' AND salary > 70000;
   ```

4. **Join Instructor and Department Tables**
   ```sql
   SELECT instructor.name, instructor.dept_name, department.building
   FROM instructor, department
   WHERE instructor.dept_name = department.dept_name;
   ```

5. **Join Instructor and Teaches Tables**
   ```sql
   SELECT instructor.name, teaches.course_id
   FROM instructor, teaches
   WHERE instructor.ID = teaches.ID;
   ```

6. **Retrieve Instructors in Computer Science with Their Course IDs**
   ```sql
   SELECT instructor.name, teaches.course_id
   FROM instructor, teaches
   WHERE instructor.ID = teaches.ID AND instructor.dept_name = 'Comp. Sci.';
   ```

7. **Alias Usage in Queries**
   ```sql
   SELECT name AS instructor_name, course_id AS cor
   FROM instructor, teaches
   WHERE instructor.ID = teaches.ID;
   ```

8. **Alias Usage with Table Names**
   ```sql
   SELECT T.name, S.course_id
   FROM instructor AS T, teaches AS S
   WHERE T.ID = S.ID;
   ```

9. **Retrieve Instructors with Salaries Higher Than Biology Department**
   ```sql
   SELECT DISTINCT T.name
   FROM instructor AS T, instructor AS S
   WHERE T.salary > S.salary AND S.dept_name = 'Biology';
   ```

10. **Retrieve Departments in Buildings Containing "Whit"**
    ```sql
    SELECT dept_name FROM department WHERE building LIKE '%Whit%';
    ```

11. **Retrieve Physics Instructors Ordered by Name**
    ```sql
    SELECT name FROM instructor WHERE dept_name = 'Physics' ORDER BY name;
    ```

12. **Retrieve All Instructors Ordered by Salary (Descending) and Name (Ascending)**
    ```sql
    SELECT * FROM instructor ORDER BY salary DESC, name ASC;
    ```

13. **Retrieve Instructors with Salaries Between 90000 and 100000**
    ```sql
    SELECT name FROM instructor WHERE salary BETWEEN 90000 AND 100000;
    ```

14. **Retrieve Instructors with Salaries Between 90000 and 100000 (Alternative)**
    ```sql
    SELECT name FROM instructor WHERE salary <= 100000 AND salary >= 90000;
    ```

15. **Retrieve Biology Instructors and Their Course IDs**
    ```sql
    SELECT instructor.name, teaches.course_id
    FROM instructor, teaches
    WHERE instructor.ID = teaches.ID AND dept_name = 'Biology';
    ```

16. **Retrieve Biology Instructors and Their Course IDs (Alternative)**
    ```sql
    SELECT instructor.name, teaches.course_id
    FROM instructor, teaches
    WHERE (instructor.ID, dept_name) = (teaches.ID, 'Biology');
    ```

---

### Set Operations

17. **Union of Courses in Fall 2017 and Spring 2018**
    ```sql
    (SELECT course_id FROM section WHERE semester = 'Fall' AND year = 2017)
    UNION
    (SELECT course_id FROM section WHERE semester = 'Spring' AND year = 2018);
    ```

18. **Intersection of Courses in Fall 2017 and Spring 2018**
    ```sql
    (SELECT course_id FROM section WHERE semester = 'Fall' AND year = 2017)
    INTERSECT
    (SELECT course_id FROM section WHERE semester = 'Spring' AND year = 2018);
    ```

19. **Intersection of Courses in Fall 2017 and Spring 2018 (Including Duplicates)**
    ```sql
    (SELECT course_id FROM section WHERE semester = 'Fall' AND year = 2017)
    INTERSECT ALL
    (SELECT course_id FROM section WHERE semester = 'Spring' AND year = 2018);
    ```

20. **Courses in Fall 2017 but Not in Spring 2018**
    ```sql
    (SELECT course_id FROM section WHERE semester = 'Fall' AND year = 2017)
    EXCEPT
    (SELECT course_id FROM section WHERE semester = 'Spring' AND year = 2018);
    ```

21. **Courses in Fall 2017 but Not in Spring 2018 (Including Duplicates)**
    ```sql
    (SELECT course_id FROM section WHERE semester = 'Fall' AND year = 2017)
    EXCEPT ALL
    (SELECT course_id FROM section WHERE semester = 'Spring' AND year = 2018);
    ```

---

### Advanced Queries

22. **Retrieve Instructors with Null Salary**
    ```sql
    SELECT name FROM instructor WHERE salary IS NULL;
    ```

23. **Retrieve Instructors with Unknown Salary Status**
    ```sql
    SELECT name FROM instructor WHERE salary > 10000 IS UNKNOWN;
    ```

24. **Calculate Average Salary in Computer Science Department**
    ```sql
    SELECT AVG(salary) FROM instructor WHERE dept_name = 'Comp. Sci.';
    ```

25. **Count Distinct Instructors Teaching in Spring 2018**
    ```sql
    SELECT COUNT(DISTINCT ID) FROM teaches WHERE semester = 'Spring' AND year = 2018;
    ```

26. **Retrieve Departments with Average Salary > 42000**
    ```sql
      SELECT dept_name 
      FROM instructor 
      GROUP BY dept_name 
      HAVING AVG(salary) > 42000;
    ```

27. **Count Total Courses**
    ```sql
    SELECT COUNT(*) FROM course;
    ```

28. **Group Instructors by Department and Calculate Average Salary**
    ```sql
    SELECT dept_name, AVG(salary) AS avg_salary
    FROM instructor
    GROUP BY dept_name;
    ```

29. **Count Instructors Teaching in Spring 2018 by Department**
    ```sql
    SELECT instructor.dept_name, COUNT(DISTINCT teaches.ID) AS instr_count
    FROM instructor, teaches
    WHERE instructor.ID = teaches.ID AND semester = 'Spring' AND year = 2018
    GROUP BY dept_name;
    ```

30. **Retrieve Departments with Average Salary > 42000 (Using HAVING)**
    ```sql
    SELECT dept_name, AVG(salary) AS avg_salary
    FROM instructor
    GROUP BY dept_name
    HAVING AVG(salary) > 42000;
    ```

31. **Retrieve Course Sections with Average Student Credits >= 2 in 2017**
    ```sql
    SELECT course_id, semester, year, sec_id, AVG(tot_cred)
    FROM student, takes
    WHERE student.ID = takes.ID AND year = 2017
    GROUP BY course_id, semester, year, sec_id
    HAVING COUNT(student.ID) >= 2;
    ```

32. **Calculate Total Salary of All Instructors**
    ```sql
    SELECT SUM(salary) FROM instructor;
    ```

33. **Retrieve Courses in Spring 2018**
    ```sql
    SELECT course_id FROM section WHERE semester = 'Spring' AND year = 2018;
    ```

34. **Retrieve Courses in Fall 2017 Also Offered in Spring 2018**
    ```sql
    SELECT DISTINCT course_id
    FROM section
    WHERE semester = 'Fall' AND year = 2017
    AND course_id IN (
        SELECT course_id
        FROM section
        WHERE semester = 'Spring' AND year = 2018
    );
    ```

35. **Count Students Taught by Instructor 10101**
    ```sql
    SELECT COUNT(DISTINCT ID)
    FROM takes
    WHERE (course_id, sec_id, semester, year) IN (
        SELECT course_id, sec_id, semester, year
        FROM teaches
        WHERE teaches.ID = '10101'
    );
    ```

36. **Retrieve Instructors with Salaries Higher Than Some Biology Instructor**
    ```sql
    SELECT name
    FROM instructor
    WHERE salary > SOME (
        SELECT salary
        FROM instructor
        WHERE dept_name = 'Biology'
    );
    ```

37. **Retrieve Instructors with Salaries Higher Than All Biology Instructors**
    ```sql
    SELECT name
    FROM instructor
    WHERE salary > ALL (
        SELECT salary
        FROM instructor
        WHERE dept_name = 'Biology'
    );
    ```

38. Count of Distinct IDs

```sql
SELECT COUNT(DISTINCT ID)
FROM takes
WHERE EXISTS (
    SELECT 1
    FROM teaches
    WHERE teaches.ID = '10101'
      AND takes.course_id = teaches.course_id
      AND takes.sec_id = teaches.sec_id
      AND takes.semester = teaches.semester
      AND takes.year = teaches.year
);
```

This query counts the number of distinct student IDs in the `takes` table where the student is enrolled in the same course and section taught by instructor `10101`.

---

39. Courses with Single Section in 2017

```sql
SELECT T.course_id
FROM course AS T
WHERE T.course_id IN (
    SELECT R.course_id
    FROM section AS R
    WHERE R.year = 2017
    GROUP BY R.course_id
    HAVING COUNT(R.course_id) = 1
);
```

This query retrieves the list of courses that had only one section offered in the year 2017.

---

40. Courses with Sections in 2017

```sql
SELECT T.course_id
FROM course AS T
WHERE 1 >= (
    SELECT COUNT(*)
    FROM section AS R
    WHERE T.course_id = R.course_id AND R.year = 2017
);
```

This query returns courses that had one or fewer sections in the year 2017.

---

41. Courses without Sections in 2017

```sql
SELECT T.course_id
FROM course AS T
WHERE NOT EXISTS (
    SELECT R.course_id
    FROM section AS R
    WHERE T.course_id = R.course_id AND R.year = 2017
    GROUP BY R.course_id
    HAVING COUNT(*) = 1
);
```

This query selects courses that did not have exactly one section in 2017.

---

42. Max Total Salary per Department

```sql
SELECT MAX(tot_salary)
FROM (
    SELECT dept_name, SUM(salary) AS tot_salary
    FROM instructor
    GROUP BY dept_name
) AS dept_total;
```

This query finds the maximum total salary by department.

---

43. Max Budget in Department

```sql
WITH max_budget AS (
    SELECT MAX(budget) AS value
    FROM department
)
SELECT budget
FROM department, max_budget
WHERE department.budget = max_budget.value;
```

This query retrieves the department with the highest budget.

---

44. Departments Above Average Salary

```sql
WITH dept_total AS (
    SELECT dept_name, SUM(salary) AS value
    FROM instructor
    GROUP BY dept_name
),
dept_total_avg AS (
    SELECT AVG(value) AS avg_value
    FROM dept_total
)
SELECT dept_name
FROM dept_total, dept_total_avg
WHERE dept_total.value > dept_total_avg.avg_value;
```

This query finds departments whose total salary exceeds the average salary of all departments.

---

45. Count of Instructors per Department

```sql
SELECT dept_name,
(
    SELECT COUNT(*)
    FROM instructor
    WHERE department.dept_name = instructor.dept_name
) AS num_instructors
FROM department;
```

This query counts the number of instructors in each department.

---

46. Delete Instructors by Department

```sql
DELETE FROM instructor
WHERE dept_name = 'Finance';
```

This query deletes all instructors from the `Finance` department.

---

47. Delete Instructors by Salary

```sql
DELETE FROM instructor
WHERE salary BETWEEN 13000 AND 15000;
```

This query deletes instructors with salaries between $13,000 and $15,000.

---

48. Insert Courses

```sql
INSERT INTO course
VALUES ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
```

This query inserts a new course into the `course` table.

---

49. Insert Instructors from Students

```sql
INSERT INTO instructor
SELECT ID, name, dept_name, 18000
FROM student
WHERE dept_name = 'Music' AND tot_cred > 144;
```

This query inserts instructors into the `instructor` table based on students from the `Music` department with more than 144 credits.

---

50. Update Instructor Salaries

```sql
UPDATE instructor
SET salary = salary * 1.05
WHERE salary < 70000;
```

This query increases the salary of instructors who earn less than $70,000 by 5%.

---

51. Update Student Credits

```sql
UPDATE student
SET tot_cred = (
    SELECT SUM(credits)
    FROM takes, course
    WHERE student.ID = takes.ID
      AND takes.course_id = course.course_id
      AND takes.grade <> 'F'
      AND takes.grade IS NOT NULL
);
```

This query updates the total credits for each student, excluding failed grades.

---

52. Conditional Credit Sum

```sql
SELECT CASE
    WHEN SUM(credits) IS NOT NULL THEN SUM(credits)
    ELSE 0
END;
```

This query computes the sum of credits for students, returning `0` if the sum is `NULL`.



