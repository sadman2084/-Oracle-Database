# Oracle User Privileges and Triggers

## 1. Creating Users and Assigning Privileges

### Creating Users
- Create `Person2` with password `1234` and grant `CREATE SESSION` privilege.
- Create `Person1` with password `5678` and grant `CREATE SESSION`, `CREATE TABLE`, `CREATE VIEW`, and `CREATE SYNONYM` privileges.

```sql
CREATE USER C##Person2 IDENTIFIED BY 1234;
GRANT CREATE SESSION TO C##Person2;

CREATE USER C##Person1 IDENTIFIED BY 5678;
GRANT CREATE SESSION, CREATE TABLE, CREATE VIEW, CREATE SYNONYM TO C##Person1;
```

### Creating and Managing Tables
- `Person1` creates a table `NEWSPAPER` and inserts data.
- `Person1` grants `SELECT` and `INSERT` privileges on `NEWSPAPER` to `Person2`.

```sql
CREATE TABLE NEWSPAPER (
    ID NUMBER PRIMARY KEY,
    NAME VARCHAR2(100),
    PRICE NUMBER(5,2)
);

INSERT INTO NEWSPAPER (ID, NAME, PRICE) VALUES (1, 'Daily News', 10.50);
INSERT INTO NEWSPAPER (ID, NAME, PRICE) VALUES (2, 'Morning Times', 12.00);
INSERT INTO NEWSPAPER (ID, NAME, PRICE) VALUES (3, 'Evening Star', 15.75);

GRANT SELECT, INSERT ON C##Person1.NEWSPAPER TO C##Person2;
```

### Accessing and Modifying Data
- `Person2` retrieves and inserts data into `NEWSPAPER`.

```sql
SELECT * FROM C##Person1.NEWSPAPER;
INSERT INTO C##Person1.NEWSPAPER (ID, NAME, PRICE) VALUES (4, 'Weekly Journal', 20.00);
COMMIT;
```

---

## 2. Implementing a Row-Level Trigger for Rating Updates

### Creating Tables
- Create `BOOKSHELF_AUDIT` to track rating changes.
- Create `BOOKSHELF` and insert initial data.

```sql
CREATE TABLE BOOKSHELF_AUDIT (
    Audit_ID NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    Book_ID NUMBER,
    Old_Rating NUMBER(2,1),
    New_Rating NUMBER(2,1),
    Updated_By VARCHAR2(100),
    Update_Time TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE BOOKSHELF (
    Book_ID NUMBER PRIMARY KEY,
    Title VARCHAR2(200),
    Author VARCHAR2(100),
    Rating NUMBER(2,1) NULL
);

INSERT INTO BOOKSHELF (Book_ID, Title, Author, Rating) VALUES (1, 'The Alchemist', 'Paulo Coelho', 4.5);
INSERT INTO BOOKSHELF (Book_ID, Title, Author, Rating) VALUES (2, '1984', 'George Orwell', 4.8);
INSERT INTO BOOKSHELF (Book_ID, Title, Author, Rating) VALUES (3, 'To Kill a Mockingbird', 'Harper Lee', 4.9);

COMMIT;
```

### Creating a Trigger for Rating Updates
- A `BEFORE UPDATE` trigger logs changes in `BOOKSHELF_AUDIT` when the `Rating` column is updated.

```sql
CREATE OR REPLACE TRIGGER trg_bookshelf_rating_audit
BEFORE UPDATE OF Rating ON BOOKSHELF
FOR EACH ROW
BEGIN
    IF :OLD.Rating != :NEW.Rating THEN
        INSERT INTO BOOKSHELF_AUDIT (Book_ID, Old_Rating, New_Rating, Updated_By, Update_Time)
        VALUES (:OLD.Book_ID, :OLD.Rating, :NEW.Rating, USER, SYSTIMESTAMP);
    END IF;
END;
/
```

### Updating Ratings and Viewing Audit Logs
- Modify book ratings and verify updates in `BOOKSHELF_AUDIT`.

```sql
UPDATE BOOKSHELF SET Rating = 4.7 WHERE Book_ID = 1;
UPDATE BOOKSHELF SET Rating = 4.6 WHERE Book_ID = 2;

COMMIT;

SELECT * FROM BOOKSHELF_AUDIT;
```

This README provides step-by-step SQL commands to create users, assign privileges, create tables, and implement a row-level trigger for monitoring rating changes in a bookshelf database.

