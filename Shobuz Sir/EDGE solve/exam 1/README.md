# README - SQL Queries Explanation


## 1. Employee Table Creation and Data Insertion
```sql
CREATE TABLE employee (
    employee_id INT,
    employee_name VARCHAR(50),
    department VARCHAR(50),
    salary INT,
    month VARCHAR(50),
    PRIMARY KEY (employee_id, month)
);

INSERT INTO employee (employee_id, employee_name, department, salary, month)
VALUES
(1, 'Alice', 'HR', 5555, 'January'),
(1, 'Alice', 'HR', 5555, 'February'),
(1, 'Alice', 'HR', 5555, 'March'),
(2, 'BOB', 'IT', 5555, 'January'),
(2, 'BOB', 'IT', 5555, 'February'),
(2, 'BOB', 'IT', 5555, 'March');
```

##  Salary Data Pivot Table
```sql
SELECT 
    Employee_ID, 
    Employee_Name, 
    Department,
    MAX(CASE WHEN Month = 'January' THEN Salary END) AS January,
    MAX(CASE WHEN Month = 'February' THEN Salary END) AS February,
    MAX(CASE WHEN Month = 'March' THEN Salary END) AS March
FROM Employee
GROUP BY Employee_ID, Employee_Name, Department;
```
**Explanation:** "এই query তে **salary** তথ্যকে **pivot** করা হয়েছে যাতে মাস অনুযায়ী দেখা যায়। অর্থাৎ, প্রতি মাসে কত টাকা বেতন দেওয়া হচ্ছে, সেটি আলাদা করে প্রদর্শিত হবে।"
## 2. Password Management
```sql
create profile C##combine
limit
password_life_time 10
password_grace_time 8
password_reuse_max 3
password_lock_time 1
failed_login_attempts 2
password_reuse_time 10

create user C##jane identified by test profile C##combine;

grant create session to C##jane;
```
## 3. SQL Joins Explanation

### Inner Join
```sql
SELECT e.Employee_ID, e.Employee_Name, e.Department, s.Salary, s.Month
FROM Employee e
INNER JOIN Employee s ON e.Employee_ID = s.Employee_ID;
```
**Explanation:** যেহেতু **JOIN** সাধারণত দুটি আলাদা টেবিলের মধ্যে হয়, কিন্তু এখানে শুধুমাত্র একটি টেবিলকেই দুইবার নেওয়া হয়েছে, যেখানে একবার 'e' এবং আরেকবার 's' হিসেবে ধরা হয়েছে, এটি একই টেবিলকে নিজের সাথেই **JOIN** করার মতো।  

যখন একই টেবিলের মধ্যে **JOIN** করা হয়, তখন সব রো একই থাকবে, কারণ কোনো রো বাদ যাবে না। ফলে **OUTPUT হবে n × n**।  

এখানে, **Alice** ৩ বার এবং **Bob** ৩ বার রয়েছে, তাই **আউটপুট হবে ৩ × ৩ = ৯টি রো**।  

আরেকটি সম্ভাব্য ক্ষেত্র হলো, যদি **দুটি ভিন্ন টেবিল থাকে**, কিন্তু **উভয়ের প্রাইমারি কি (Primary Key) একই হয়**, তাহলে রো-এর সংখ্যা একই থাকবে।

### Left Join
```sql
SELECT e.employee_id, e.employee_name, e.department, s.salary, s.month 
FROM employee e
LEFT JOIN employee s ON e.employee_id = s.employee_id;
```

### Right Join
```sql
SELECT e.employee_id, e.employee_name, e.department, s.salary, s.month 
FROM employee e
RIGHT JOIN employee s ON e.employee_id = s.employee_id;
```

### Full Join (Union Join)
```sql
SELECT e.employee_id, e.employee_name, e.department, s.salary, s.month 
FROM employee e
LEFT JOIN employee s ON e.employee_id = s.employee_id

UNION

SELECT e.employee_id, e.employee_name, e.department, s.salary, s.month 
FROM employee e
RIGHT JOIN employee s ON e.employee_id = s.employee_id;
```
**Explanation:** "বাংলা কথা বামে এবং ডানে এক সাথে যোগ করা"
### Cross Join
```sql
SELECT e.employee_id, e.employee_name, e.department, s.salary, s.month 
FROM employee e
CROSS JOIN employee s;
```
**Explanation:** "Many to many relation মানে হলো বাম এন্টিটির সাথে ডান এন্টিটির সম্পর্ক, মানে সবাই সবার সাথে সম্পর্কিত।এখানে যেহেতু সবার সাথে সবার সম্পর্ক, তাই কোন শর্ত (condition) দরকার নেই।"

## Duplicate Data Delete
```sql
DELETE FROM Employee
WHERE Employee_ID NOT IN (
    SELECT MIN(Employee_ID) FROM Employee 
    GROUP BY Employee_ID, Month
);
```
**Inner query explanation**: এখানে "age" সম্পর্কিত যতগুলো query থাকবে, যেগুলো পুনরাবৃত্তি (repeated) হবে, তাদের মধ্যে থেকে কমপক্ষে একটি চয়ন করা হবে (যেমন, মিনিমাম একটাকে রাখবে)।

**Outer query explanation**: একটি query বাদে বাকী গুলিকে মুছে ফেলবে। এটির জন্য `NOT IN` ব্যবহার করা হয়, যাতে inner query এর ফলাফল থেকে প্রাপ্ত মান গুলোর সাথে মেলানো হয় না এবং সেগুলি মুছে ফেলা হয়।

## 4. University Database Queries

### (i) Course Table Creation
```sql
CREATE TABLE Course (
    Course_ID INT PRIMARY KEY,
    TITLE VARCHAR(50),
    DEPT_NAME VARCHAR(50),
    CREDITS INT
);
```

### (ii) Instructor er Salary Astronomy Dept er theke beshi kina dekha
```sql
SELECT COURSE.DEPT_NAME, INSTRUCTOR.SALARY 
FROM Course, INSTRUCTOR 
WHERE SALARY > (SELECT SALARY FROM INSTRUCTOR WHERE DEPT_NAME = 'Astronomy') 
ORDER BY DEPT_NAME;
```
**Explanation:** "এখানে আমি largerelational database ব্যবহার করেছি, তাই বাজেটের পরিবর্তে বেতন (salary) গণনা করছি।"
### (iii) Comp. Sci. er instructor der salary check
```sql
SELECT NAME FROM instructor
WHERE DEPT_NAME='Comp. Sci.' AND SALARY <90000;
```

### (iv) Spring 2010 e kon department er koyjon instructor course poraise
```sql
SELECT instructor.dept_name, COUNT(DISTINCT instructor.ID)
FROM instructor
JOIN teaches ON instructor.ID = teaches.ID
WHERE teaches.semester = 'Spring' AND teaches.year = 2010
GROUP BY instructor.dept_name;
```
**Explanation:**"এখানে **semester** এবং **year** এর তথ্য **teaches** টেবিলে আছে, তাই **instructor** এবং **teaches** টেবিলকে যোগ করেছি। এবং **instructor** ও **teaches** টেবিলে **ID** একই হওয়ায়, **instructor.ID = teaches.ID** এই শর্তে যোগ করেছি। তারপর একটি নতুন শর্ত যোগ করেছি **semester** এবং **year** এর জন্য যাতে **semester** অনুযায়ী ডেটা ফিল্টার করা যায়।"
### (v) Salary Increment Logic
```sql
UPDATE instructor
SET salary = 
    CASE 
        WHEN salary > 100000 THEN salary * 1.0303  
        ELSE salary * 1.045 
    END;
```

## 5. PASSWORD_REUSE_MAX vs PASSWORD_REUSE_TIME
**Explanation:**
PASSWORD_REUSE_MAX vs PASSWORD_REUSE_TIME
Justification:
PASSWORD_REUSE_MAX: Specifies how many times a password can be reused before it is blocked.
PASSWORD_REUSE_TIME: Specifies a time duration (in days) before a password can be reused.
These are mutually exclusive because setting both can create conflicts. If one is set, the other must not be UNLIMITED to avoid security loopholes.

**Bangla Explanation:**


- **PASSWORD_REUSE_MAX**: এটি নির্দিষ্ট করে কতবার একটি পাসওয়ার্ড পুনরায় ব্যবহার করা যেতে পারে তার আগে সেটি ব্লক হয়ে যাবে।
  
- **PASSWORD_REUSE_TIME**: এটি একটি সময়সীমা (দিনে) নির্দিষ্ট করে, যার মধ্যে পাসওয়ার্ডটি পুনরায় ব্যবহার করা যাবে না।

এগুলি **mutually exclusive** (একত্রে ব্যবহৃত হতে পারে না) কারণ, যদি দুটি শর্ত একসাথে সেট করা হয়, তবে এটি কনফ্লিক্ট সৃষ্টি করতে পারে। যদি একটি সেট করা থাকে, তবে অন্যটির মান **UNLIMITED** না হওয়া উচিত, অন্যথায় সুরক্ষা ঝুঁকি সৃষ্টি হতে পারে।
