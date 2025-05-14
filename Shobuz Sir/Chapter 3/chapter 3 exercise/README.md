# SQL Practice Exercises with Answers

---

## 3.1 Basic SQL Queries Using the University Schema

### a. Find the titles of courses in the Comp. Sci. department that have 3 credits.

```sql
SELECT title
FROM course
WHERE dept_name = 'Comp. Sci.' AND credits = 3;
```

### b. Find the IDs of all students who were taught by an instructor named Einstein; no duplicates.

```sql
SELECT DISTINCT takes.ID
FROM takes
JOIN teaches USING (course_id, sec_id, semester, year)
JOIN instructor ON teaches.ID = instructor.ID
WHERE instructor.name = 'Einstein';
```

### c. Find the highest salary of any instructor.

```sql
SELECT MAX(salary) AS highest_salary
FROM instructor;
```

### d. Find all instructors earning the highest salary.

```sql
SELECT name
FROM instructor
WHERE salary = (SELECT MAX(salary) FROM instructor);
```

### e. Find the enrollment of each section that was offered in Fall 2017.

```sql
SELECT course_id, sec_id, COUNT(ID) AS enrollment
FROM takes
WHERE semester = 'Fall' AND year = 2017
GROUP BY course_id, sec_id;
```

### f. Find the maximum enrollment in Fall 2017.

```sql
SELECT MAX(enrollment) AS max_enrollment
FROM (
  SELECT COUNT(ID) AS enrollment
  FROM takes
  WHERE semester = 'Fall' AND year = 2017
  GROUP BY course_id, sec_id
) AS enrollments;
```

### g. Find sections with maximum enrollment in Fall 2017.

```sql
SELECT course_id, sec_id
FROM takes
WHERE semester = 'Fall' AND year = 2017
GROUP BY course_id, sec_id
HAVING COUNT(ID) = (
  SELECT MAX(enrollment)
  FROM (
    SELECT COUNT(ID) AS enrollment
    FROM takes
    WHERE semester = 'Fall' AND year = 2017
    GROUP BY course_id, sec_id
  ) AS max_enroll
);
```

---

## 3.2 GPA Calculations Using grade\_points(grade, points)

### a. Total grade points earned by student '12345'

```sql
SELECT SUM(course.credits * grade_points.points) AS total_grade_points
FROM takes
JOIN course USING (course_id)
JOIN grade_points ON takes.grade = grade_points.grade
WHERE takes.ID = '12345';
```

### b. GPA for student '12345'

```sql
SELECT SUM(course.credits * grade_points.points) / SUM(course.credits) AS GPA
FROM takes
JOIN course USING (course_id)
JOIN grade_points ON takes.grade = grade_points.grade
WHERE takes.ID = '12345';
```

### c. ID and GPA of each student

```sql
SELECT takes.ID, SUM(course.credits * grade_points.points) / SUM(course.credits) AS GPA
FROM takes
JOIN course USING (course_id)
JOIN grade_points ON takes.grade = grade_points.grade
GROUP BY takes.ID;
```

### d. Handling null grades

```sql
SELECT takes.ID, SUM(course.credits * grade_points.points) / SUM(course.credits) AS GPA
FROM takes
JOIN course USING (course_id)
LEFT JOIN grade_points ON takes.grade = grade_points.grade
WHERE grade_points.points IS NOT NULL
GROUP BY takes.ID;
```

---

## 3.3 SQL Data Modification Queries

### a. Increase salary of Comp. Sci. instructors by 10%

```sql
UPDATE instructor
SET salary = salary * 1.10
WHERE dept_name = 'Comp. Sci.';
```

### b. Delete courses never offered

```sql
DELETE FROM course
WHERE course_id NOT IN (SELECT DISTINCT course_id FROM section);
```

### c. Insert students with >100 credits as instructors

```sql
INSERT INTO instructor (ID, name, dept_name, salary)
SELECT ID, name, dept_name, 10000
FROM student
WHERE tot_cred > 100;
```

---

## 3.4 Insurance Database Queries

### a. Total people who owned cars in 2017 accidents

```sql
SELECT COUNT(DISTINCT owns.driver_id) AS total_people
FROM owns
JOIN participated USING (license_plate)
JOIN accident USING (report_number)
WHERE accident.year = 2017;
```

### b. Delete all year-2010 cars of person '12345'

```sql
DELETE FROM car
WHERE license_plate IN (
  SELECT license_plate FROM owns
  WHERE driver_id = '12345'
) AND year = 2010;
```

---

## 3.5 Marks to Grade Conversion

### a. Display grade for each student

```sql
SELECT ID,
       score,
       CASE
         WHEN score < 40 THEN 'F'
         WHEN score < 60 THEN 'C'
         WHEN score < 80 THEN 'B'
         ELSE 'A'
       END AS grade
FROM marks;
```

### b. Number of students with each grade

```sql
SELECT
  CASE
    WHEN score < 40 THEN 'F'
    WHEN score < 60 THEN 'C'
    WHEN score < 80 THEN 'B'
    ELSE 'A'
  END AS grade,
  COUNT(*) AS total_students
FROM marks
GROUP BY grade;
```

---

## 3.6 Case-Insensitive LIKE

### Find departments containing 'sci' (case-insensitive)

```sql
SELECT dept_name
FROM department
WHERE LOWER(dept_name) LIKE '%sci%';
```

---

## 3.7 Query Evaluation with Empty Relations

### Correct version of query when r1 or r2 might be empty

```sql
SELECT DISTINCT p.a1
FROM p
LEFT JOIN r1 ON p.a1 = r1.a1
LEFT JOIN r2 ON p.a1 = r2.a1
WHERE r1.a1 IS NOT NULL OR r2.a1 IS NOT NULL;
```

---

## 3.8 Bank Database Queries

### a. Customers with account but no loan

```sql
SELECT DISTINCT d.ID
FROM depositor d
LEFT JOIN borrower b ON d.ID = b.ID
WHERE b.ID IS NULL;
```

### b. Customers in same street & city as '12345'

```sql
SELECT c2.ID
FROM customer c1
JOIN customer c2 ON c1.customer_street = c2.customer_street AND c1.customer_city = c2.customer_city
WHERE c1.ID = '12345' AND c2.ID <> '12345';
```

### c. Branches with customers in "Harrison"

```sql
SELECT DISTINCT a.branch_name
FROM account a
JOIN depositor d ON a.account_number = d.account_number
JOIN customer c ON d.ID = c.ID
WHERE c.customer_city = 'Harrison';
```

---

## 3.9 Employee and Company Relations

### a. Employees of "First Bank Corporation"

```sql
SELECT e.ID, e.name, e.city
FROM employee e
JOIN works w ON e.ID = w.ID
WHERE w.company_name = 'First Bank Corporation';
```

### b. Employees with salary > 10000

```sql
SELECT e.ID, e.name, e.city
FROM employee e
JOIN works w ON e.ID = w.ID
WHERE w.company_name = 'First Bank Corporation' AND w.salary > 10000;
```

### c. Employees not working for "First Bank Corporation"

```sql
SELECT DISTINCT ID
FROM works
WHERE company_name <> 'First Bank Corporation';
```

### d. Employees earning more than every employee of "Small Bank Corporation"

```sql
SELECT DISTINCT w1.ID
FROM works w1
WHERE w1.salary > ALL (
  SELECT w2.salary
  FROM works w2
  WHERE w2.company_name = 'Small Bank Corporation'
);
```

### e. Companies in every city where "Small Bank Corporation" is located

```sql
SELECT DISTINCT c1.company_name
FROM company c1
WHERE NOT EXISTS (
  SELECT c2.city
  FROM company c2
  WHERE c2.company_name = 'Small Bank Corporation'
  AND NOT EXISTS (
    SELECT *
    FROM company c3
    WHERE c3.company_name = c1.company_name AND c3.city = c2.city
  )
);
```

### f. Company with most employees

```sql
SELECT company_name
FROM works
GROUP BY company_name
HAVING COUNT(ID) = (
  SELECT MAX(emp_count)
  FROM (
    SELECT company_name, COUNT(ID) AS emp_count
    FROM works
    GROUP BY company_name
  ) AS company_counts
);
```

### g. Companies with average salary > First Bank Corporation

```sql
SELECT w1.company_name
FROM works w1
GROUP BY w1.company_name
HAVING AVG(w1.salary) > (
  SELECT AVG(w2.salary)
  FROM works w2
  WHERE w2.company_name = 'First Bank Corporation'
);
```

---

## 3.10 Employee and Company Updates

### a. Update city of employee '12345'

```sql
UPDATE employee
SET city = 'Newtown'
WHERE ID = '12345';
```

### b. Raise salary for managers at "First Bank Corporation"

```sql
UPDATE works
SET salary =
  CASE
    WHEN salary * 1.10 <= 100000 THEN salary * 1.10
    ELSE salary * 1.03
  END
WHERE ID IN (SELECT ID FROM manages)
AND company_name = 'First Bank Corporation';
```

---

---

## ✅ 3.11 Queries on University Schema

**a. Find the ID and name of each student who has taken at least one Comp. Sci. course**

```sql
SELECT DISTINCT student.ID, student.name
FROM student
JOIN takes ON student.ID = takes.ID
JOIN course ON takes.course_id = course.course_id
WHERE course.dept_name = 'Comp. Sci.';
```

**b. Find the ID and name of each student who has not taken any course offered before 2017**

```sql
SELECT student.ID, student.name
FROM student
WHERE student.ID NOT IN (
    SELECT DISTINCT takes.ID
    FROM takes
    JOIN section ON takes.course_id = section.course_id AND takes.sec_id = section.sec_id
    WHERE section.year < 2017
);
```

**c. For each department, find the maximum salary of instructors in that department**

```sql
SELECT dept_name, MAX(salary) AS max_salary
FROM instructor
GROUP BY dept_name;
```

**d. Find the lowest, across all departments, of the per-department maximum salary**

```sql
SELECT MIN(max_salary)
FROM (
    SELECT dept_name, MAX(salary) AS max_salary
    FROM instructor
    GROUP BY dept_name
) AS dept_max_salaries;
```

---

## ✅ 3.12 Data Manipulation on University Schema

**a. Create a new course “CS-001”, titled “Weekly Seminar”, with 0 credits**

```sql
INSERT INTO course (course_id, title, dept_name, credits)
VALUES ('CS-001', 'Weekly Seminar', 'Comp. Sci.', 0);
```

**b. Create a section of this course in Fall 2017**

```sql
INSERT INTO section (course_id, sec_id, semester, year, building, room_number)
VALUES ('CS-001', 1, 'Fall', 2017, NULL, NULL);
```

**c. Enroll every Comp. Sci. student in the section**

```sql
INSERT INTO takes (ID, course_id, sec_id, semester, year)
SELECT ID, 'CS-001', 1, 'Fall', 2017
FROM student
WHERE dept_name = 'Comp. Sci.';
```

**d. Delete enrollments in the section where student ID is 12345**

```sql
DELETE FROM takes
WHERE ID = '12345' AND course_id = 'CS-001' AND sec_id = 1 AND semester = 'Fall' AND year = 2017;
```

**e. Delete the course CS-001**

```sql
DELETE FROM course WHERE course_id = 'CS-001';
```

> ⚠️ Deleting without removing dependent sections may cause a foreign key constraint violation.

**f. Delete enrollments for sections with course titles containing “advanced”**

```sql
DELETE FROM takes
WHERE course_id IN (
    SELECT course_id
    FROM course
    WHERE LOWER(title) LIKE '%advanced%'
);
```

---

## ✅ 3.13 DDL for Insurance Schema

```sql
CREATE TABLE person (
    driver_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    address VARCHAR(255) NOT NULL
);

CREATE TABLE car (
    license_plate VARCHAR(20) PRIMARY KEY,
    model VARCHAR(50) NOT NULL,
    year INT CHECK (year >= 1886)
);

CREATE TABLE accident (
    report_number INT PRIMARY KEY,
    year INT CHECK (year >= 1900),
    location VARCHAR(255) NOT NULL
);

CREATE TABLE owns (
    driver_id INT,
    license_plate VARCHAR(20),
    PRIMARY KEY (driver_id, license_plate),
    FOREIGN KEY (driver_id) REFERENCES person(driver_id) ON DELETE CASCADE,
    FOREIGN KEY (license_plate) REFERENCES car(license_plate) ON DELETE CASCADE
);

CREATE TABLE participated (
    report_number INT,
    license_plate VARCHAR(20),
    driver_id INT,
    damage_amount DECIMAL(10,2) CHECK (damage_amount >= 0),
    PRIMARY KEY (report_number, license_plate, driver_id),
    FOREIGN KEY (report_number) REFERENCES accident(report_number) ON DELETE CASCADE,
    FOREIGN KEY (license_plate) REFERENCES car(license_plate) ON DELETE CASCADE,
    FOREIGN KEY (driver_id) REFERENCES person(driver_id) ON DELETE CASCADE
);
```

---

## ✅ 3.14 Queries on Insurance Database

**a. Count accidents involving cars owned by "John Smith"**

```sql
SELECT COUNT(DISTINCT participated.report_number)
FROM participated
JOIN owns ON participated.license_plate = owns.license_plate
JOIN person ON owns.driver_id = person.driver_id
WHERE person.name = 'John Smith';
```

**b. Update damage amount**

```sql
UPDATE participated
SET damage_amount = 3000
WHERE report_number = 'AR2197' AND license_plate = 'AABB2000';
```

---

## ✅ 3.15 Queries on Bank Schema

**a. Customers with accounts at every “Brooklyn” branch**

```sql
SELECT d.ID
FROM depositor d
JOIN account a ON d.account_number = a.account_number
JOIN branch b ON a.branch_name = b.branch_name
WHERE b.branch_city = 'Brooklyn'
GROUP BY d.ID
HAVING COUNT(DISTINCT a.branch_name) = (
    SELECT COUNT(branch_name) FROM branch WHERE branch_city = 'Brooklyn'
);
```

**b. Total loan amount**

```sql
SELECT SUM(amount) AS total_loan_amount FROM loan;
```

**c. Branches with more assets than at least one Brooklyn branch**

```sql
SELECT DISTINCT b1.branch_name
FROM branch b1
WHERE b1.assets > (
    SELECT MIN(b2.assets)
    FROM branch b2
    WHERE b2.branch_city = 'Brooklyn'
);
```

---

## ✅ 3.16 Queries on Employee Schema

**a. Employees who live in the same city as their company**

```sql
SELECT e.ID, e.person_name
FROM employee e
JOIN works w ON e.ID = w.ID
JOIN company c ON w.company_name = c.company_name
WHERE e.city = c.city;
```

**b. Employees who live on same street and city as manager**

```sql
SELECT e1.ID, e1.person_name
FROM employee e1
JOIN manages m ON e1.ID = m.ID
JOIN employee e2 ON m.manager_id = e2.ID
WHERE e1.city = e2.city AND e1.street = e2.street;
```

**c. Employees who earn more than company average**

```sql
SELECT e.ID, e.person_name
FROM employee e
JOIN works w1 ON e.ID = w1.ID
WHERE w1.salary > (
    SELECT AVG(w2.salary)
    FROM works w2
    WHERE w2.company_name = w1.company_name
);
```

**d. Company with the smallest payroll**

```sql
SELECT company_name
FROM works
GROUP BY company_name
ORDER BY SUM(salary) ASC
LIMIT 1;
```

---

## ✅ 3.17 Data Updates and Deletes on Employee Schema

**a. 10% raise for First Bank Corporation employees**

```sql
UPDATE works
SET salary = salary * 1.10
WHERE company_name = 'First Bank Corporation';
```

**b. 10% raise for managers of First Bank Corporation**

```sql
UPDATE works
SET salary = salary * 1.10
WHERE ID IN (
    SELECT ID FROM manages
) AND company_name = 'First Bank Corporation';
```

**c. Delete all employees of Small Bank Corporation**

```sql
DELETE FROM works
WHERE company_name = 'Small Bank Corporation';
```

---

## ✅ 3.18 DDL for Employee Database

```sql
CREATE TABLE employee (
    ID INT PRIMARY KEY,
    person_name VARCHAR(100),
    street VARCHAR(255),
    city VARCHAR(100)
);

CREATE TABLE company (
    company_name VARCHAR(100) PRIMARY KEY,
    city VARCHAR(100)
);

CREATE TABLE works (
    ID INT,
    company_name VARCHAR(100),
    salary DECIMAL(10,2),
    PRIMARY KEY (ID, company_name),
    FOREIGN KEY (ID) REFERENCES employee(ID),
    FOREIGN KEY (company_name) REFERENCES company(company_name)
);

CREATE TABLE manages (
    ID INT PRIMARY KEY,
    manager_id INT,
    FOREIGN KEY (ID) REFERENCES employee(ID),
    FOREIGN KEY (manager_id) REFERENCES employee(ID)
);
```

---

## ✅ 3.19 Reasons for Null Values

1. **Missing Information** – Example: customer’s phone number not known yet.
2. **Not Applicable** – Example: `manager_id` is NULL for employees without a manager.

---

## ✅ 3.20 Equivalence of `<> ALL` and `NOT IN`

```sql
SELECT ID FROM employee
WHERE salary <> ALL (
    SELECT salary FROM works WHERE company_name = 'Small Bank Corporation'
);
```

This is **equivalent** to:

```sql
SELECT ID FROM employee
WHERE salary NOT IN (
    SELECT salary FROM works WHERE company_name = 'Small Bank Corporation'
);
```

---


#### **3.21**

**(a)** এমন সদস্যদের সদস্য নম্বর ও নাম বের করো যারা "McGraw-Hill" দ্বারা প্রকাশিত অন্তত একটি বই ধার নিয়েছে।

```sql
SELECT DISTINCT member.memb_no, member.name
FROM member
JOIN borrowed ON member.memb_no = borrowed.memb_no
JOIN book ON borrowed.isbn = book.isbn
WHERE book.publisher = 'McGraw-Hill';
```

**(b)** এমন সদস্যদের বের করো যারা "McGraw-Hill" দ্বারা প্রকাশিত প্রতিটি বই ধার নিয়েছে।

```sql
SELECT m.memb_no, m.name
FROM member m
WHERE NOT EXISTS (
    SELECT b.isbn
    FROM book b
    WHERE b.publisher = 'McGraw-Hill'
    EXCEPT
    SELECT br.isbn
    FROM borrowed br
    WHERE br.memb_no = m.memb_no
);
```

**(c)** প্রতিটি প্রকাশকের জন্য, এমন সদস্যদের বের করো যারা ঐ প্রকাশকের ৫টির বেশি বই ধার নিয়েছে।

```sql
SELECT book.publisher, borrowed.memb_no, member.name
FROM borrowed
JOIN book ON borrowed.isbn = book.isbn
JOIN member ON borrowed.memb_no = member.memb_no
GROUP BY book.publisher, borrowed.memb_no, member.name
HAVING COUNT(borrowed.isbn) > 5;
```

**(d)** প্রত্যেক সদস্য গড়ে কতটি বই ধার নিয়েছে তা বের করো। এমন সদস্যরাও গণনায় থাকবে যারা কোনও বই ধার নেয়নি।

```sql
SELECT COUNT(borrowed.isbn) * 1.0 / COUNT(DISTINCT member.memb_no) AS avg_books_per_member
FROM member
LEFT JOIN borrowed ON member.memb_no = borrowed.memb_no;
```

---

#### **3.22**

`UNIQUE` ব্যবহার না করে এই WHERE ক্লজটি লেখো:

```sql
WHERE (SELECT COUNT(DISTINCT title) FROM course) = 1;
```

---

#### **3.23**

`WITH` ব্যবহার না করে নিচের কোড রিরাইট করো:

```sql
SELECT dept_name
FROM (
    SELECT dept_name, SUM(salary) AS value
    FROM instructor
    GROUP BY dept_name
) AS dept_total
JOIN (
    SELECT AVG(value) AS avg_value
    FROM (
        SELECT dept_name, SUM(salary) AS value
        FROM instructor
        GROUP BY dept_name
    ) AS dept_total_avg
) AS overall_avg
ON dept_total.value >= overall_avg.avg_value;
```

---

#### **3.24**

Physics ডিপার্টমেন্টের ইন্সট্রাক্টরের দ্বারা পরামর্শপ্রাপ্ত Accounting ছাত্রদের নাম ও ID বের করো:

```sql
SELECT student.ID, student.name
FROM student
JOIN advisor ON student.ID = advisor.s_id
JOIN instructor ON advisor.i_id = instructor.ID
WHERE student.dept_name = 'Accounting' AND instructor.dept_name = 'Physics';
```

---

#### **3.25**

যেসব বিভাগের বাজেট 'Philosophy' বিভাগের চেয়ে বেশি, সেগুলোর নাম বের করো (বর্ণানুক্রমে):

```sql
SELECT dept_name
FROM department
WHERE budget > (SELECT budget FROM department WHERE dept_name = 'Philosophy')
ORDER BY dept_name;
```

---

#### **3.26**

যেসব ছাত্র কোনও কোর্স কমপক্ষে তিনবার নিয়েছে (রিটেক করেছে), তাদের আইডি ও কোর্স আইডি বের করো:

```sql
SELECT course_id, ID
FROM takes
GROUP BY course_id, ID
HAVING COUNT(*) >= 3
ORDER BY course_id, ID;
```

---

#### **3.27**

যেসব ছাত্র অন্তত তিনটি ভিন্ন কোর্স অন্তত দুইবার করে নিয়েছে, তাদের ID বের করো:

```sql
SELECT ID
FROM (
  SELECT ID, course_id, COUNT(*) AS cnt
  FROM takes
  GROUP BY ID, course_id
  HAVING COUNT(*) >= 2
) AS repeated
GROUP BY ID
HAVING COUNT(DISTINCT course_id) >= 3;
```

---

#### **3.28**

যেসব ইন্সট্রাক্টর তার বিভাগের সব কোর্স পড়িয়েছেন, তাদের নাম ও ID বের করো (নাম অনুযায়ী সাজানো):

```sql
SELECT instructor.ID, instructor.name
FROM instructor
WHERE NOT EXISTS (
    SELECT course.course_id
    FROM course
    WHERE course.dept_name = instructor.dept_name
    EXCEPT
    SELECT teaches.course_id
    FROM teaches
    WHERE teaches.ID = instructor.ID
)
ORDER BY instructor.name;
```

---

#### **3.29**

যেসব History ছাত্রের নাম 'D' দিয়ে শুরু এবং যারা কমপক্ষে ৫টি Music কোর্স নেয়নি, তাদের নাম ও ID:

```sql
SELECT student.ID, student.name
FROM student
WHERE student.dept_name = 'History'
AND student.name LIKE 'D%'
AND (
    SELECT COUNT(*)
    FROM takes
    JOIN course ON takes.course_id = course.course_id
    WHERE course.dept_name = 'Music'
    AND takes.ID = student.ID
) < 5;
```

---

#### **3.30**

নিচের query-র ফলাফল সবসময় শূন্য হয় না কেন, একটি উদাহরণসহ ব্যাখ্যা করো:

```sql
SELECT AVG(salary) - (SUM(salary) / COUNT(*)) FROM instructor;
```

**Answer:**
যদি `salary` কলামে NULL থাকে, তবে `AVG()` ফাংশন NULL উপেক্ষা করে, কিন্তু `COUNT(*)` সব সারি গোনে। তাই পার্থক্য হয়।

**Example:**

| ID | Salary |
| -- | ------ |
| 1  | 50000  |
| 2  | NULL   |
| 3  | 70000  |

* AVG = (50000 + 70000) / 2 = 60000
* SUM / COUNT = (50000 + 70000) / 3 = 40000
* Difference = 20000

---

#### **3.31**

যেসব ইন্সট্রাক্টর কখনো কোনও কোর্সে 'A' গ্রেড দেয়নি, তাদের ID ও নাম বের করো:

```sql
SELECT DISTINCT instructor.ID, instructor.name
FROM instructor
WHERE instructor.ID NOT IN (
    SELECT teaches.ID
    FROM teaches
    JOIN takes ON teaches.course_id = takes.course_id AND teaches.sec_id = takes.sec_id
    WHERE takes.grade = 'A'
);
```

---

#### **3.32**

উপরে দেওয়া প্রশ্নেই আরেকটা শর্ত যোগ করো: যিনি অন্তত একবার অন্য কোনও (null নয় এমন) গ্রেড দিয়েছেন:

```sql
SELECT DISTINCT instructor.ID, instructor.name
FROM instructor
WHERE instructor.ID NOT IN (
    SELECT teaches.ID
    FROM teaches
    JOIN takes ON teaches.course_id = takes.course_id AND teaches.sec_id = takes.sec_id
    WHERE takes.grade = 'A'
)
AND instructor.ID IN (
    SELECT teaches.ID
    FROM teaches
    JOIN takes ON teaches.course_id = takes.course_id AND teaches.sec_id = takes.sec_id
    WHERE takes.grade IS NOT NULL
);
```

---

#### **3.33**

যেসব Comp. Sci. কোর্সের অন্তত একটি সেকশন দুপুর ১২টার পরে শেষ হয়, সেই কোর্স ID ও নাম বের করো:

```sql
SELECT DISTINCT course.course_id, course.title
FROM course
JOIN section ON course.course_id = section.course_id
WHERE course.dept_name = 'Comp. Sci.'
AND section.end_time >= '12:00';
```

---

#### **3.34**

প্রত্যেক সেকশনে কতজন ছাত্র আছে তা বের করো, format: courseid, secid, year, semester, num

```sql
SELECT takes.course_id, takes.sec_id, takes.year, takes.semester, COUNT(*) AS num
FROM takes
GROUP BY takes.course_id, takes.sec_id, takes.year, takes.semester;
```

---

#### **3.35**

সর্বোচ্চ ভর্তি আছে এমন section গুলো বের করো:

```sql
WITH section_counts AS (
    SELECT takes.course_id, takes.sec_id, takes.year, takes.semester, COUNT(*) AS num
    FROM takes
    GROUP BY takes.course_id, takes.sec_id, takes.year, takes.semester
),
max_count AS (
    SELECT MAX(num) AS max_enrollment FROM section_counts
)
SELECT sc.course_id, sc.sec_id, sc.year, sc.semester, sc.num
FROM section_counts sc
JOIN max_count mc ON sc.num = mc.max_enrollment;
```


