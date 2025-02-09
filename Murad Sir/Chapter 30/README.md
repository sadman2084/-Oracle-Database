# Salary Difference Trigger in Oracle

## Overview
This project demonstrates the use of an Oracle database trigger to track salary changes in an `emp` table. The trigger `salary_difference` calculates the difference between the old and new salary values whenever an `INSERT`, `UPDATE`, or `DELETE` operation is performed on the table.

## Table Structure
A table named `emp` is created with the following columns:
- `emp_id` (NUMBER, PRIMARY KEY) - Unique identifier for employees.
- `emp_name` (VARCHAR2(100)) - Name of the employee.
- `salary` (NUMBER) - Employee's salary.

## Trigger Details
The `salary_difference` trigger is executed **before any INSERT, UPDATE, or DELETE operation** on the `emp` table. It computes the salary difference and prints the old and new salary values using `DBMS_OUTPUT.PUT_LINE`.

### Trigger Code:
```sql
CREATE TRIGGER salary_difference
BEFORE INSERT OR DELETE OR UPDATE ON emp
FOR EACH ROW
DECLARE salary_difference NUMBER;
BEGIN
    salary_difference := :NEW.salary - :OLD.salary;
    DBMS_OUTPUT.PUT_LINE('Old salary: ' || :OLD.salary);
    DBMS_OUTPUT.PUT_LINE('New salary: ' || :NEW.salary);
    DBMS_OUTPUT.PUT_LINE('Salary difference: ' || salary_difference);
END;
/
```

## Operations Performed

### Creating the `emp` Table:
```sql
CREATE TABLE emp (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(100),
    salary NUMBER
);
```

### Inserting Initial Data:
```sql
INSERT INTO emp (emp_id, emp_name, salary) VALUES (1, 'Rahim', 50000);
INSERT INTO emp (emp_id, emp_name, salary) VALUES (2, 'Karim', 60000);
INSERT INTO emp (emp_id, emp_name, salary) VALUES (3, 'Selim', 55000);
COMMIT;
```

### Additional Inserts:
```sql
INSERT INTO emp (emp_id, emp_name, salary) VALUES (4, 'Hasan', 70000);
INSERT INTO emp (emp_id, emp_name, salary) VALUES (5, 'Sumon', 75000);
COMMIT;
```

### Updating a Salary:
```sql
UPDATE emp SET salary = 80000 WHERE emp_id = 4;
COMMIT;
```

### Deleting an Employee:
```sql
DELETE FROM emp WHERE emp_id = 5;
COMMIT;
```

## Expected Output
Whenever an `INSERT`, `UPDATE`, or `DELETE` operation is performed, the trigger should print the following details:
- **Old Salary** (Before the operation)
- **New Salary** (After the operation)
- **Salary Difference**

However, there is a potential issue: when inserting a new row, `:OLD.salary` does not exist, which may cause an error. Similarly, when deleting a row, `:NEW.salary` does not exist. Proper handling using `IF` conditions should be added to avoid runtime errors.

## Notes
- Make sure to enable `DBMS_OUTPUT` in SQL*Plus or SQL Developer to see the trigger output.
- Modify the trigger logic to handle `INSERT` and `DELETE` cases properly.

## Improvements
To prevent errors during `INSERT` and `DELETE` operations, modify the trigger like this:
```sql
CREATE OR REPLACE TRIGGER salary_difference
BEFORE INSERT OR DELETE OR UPDATE ON emp
FOR EACH ROW
DECLARE
    salary_difference NUMBER;
BEGIN
    IF INSERTING THEN
        DBMS_OUTPUT.PUT_LINE('New Employee Salary: ' || :NEW.salary);
    ELSIF UPDATING THEN
        salary_difference := :NEW.salary - :OLD.salary;
        DBMS_OUTPUT.PUT_LINE('Old salary: ' || :OLD.salary);
        DBMS_OUTPUT.PUT_LINE('New salary: ' || :NEW.salary);
        DBMS_OUTPUT.PUT_LINE('Salary difference: ' || salary_difference);
    ELSIF DELETING THEN
        DBMS_OUTPUT.PUT_LINE('Deleted Employee Salary: ' || :OLD.salary);
    END IF;
END;
/
```

This ensures that the trigger runs without errors during all operations.

