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
    SELECT dept_name, avg_salary
    FROM (
        SELECT dept_name, AVG(salary) AS avg_salary
        FROM instructor
        GROUP BY dept_name
    ) AS subquery
    WHERE avg_salary > 42000;
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

---

## Notes
- Ensure the `VERSITY1` database is selected before running these queries.
- Replace any placeholders (e.g., `'Comp. Sci.'`, `'Biology'`) with actual values if needed.
- If any query fails, check for syntax errors or missing tables/columns.

---

