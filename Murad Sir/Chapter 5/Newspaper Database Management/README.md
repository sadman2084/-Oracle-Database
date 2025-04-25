# Newspaper Table Query Examples

This repository contains SQL queries to work with a sample table named `NEWSPAPER`. The table stores information about newspaper features, their sections, and the page numbers where they appear. Below is a detailed description of the table schema and the queries provided in this project.

## Table Schema
```sql
CREATE TABLE NEWSPAPER (
    Feature      VARCHAR2(15) NOT NULL,
    Section      CHAR(1),
    Page         NUMBER
);
```

### Sample Data
The following data is inserted into the `NEWSPAPER` table:
```sql
INSERT INTO NEWSPAPER VALUES ('National News', 'A', 1);
INSERT INTO NEWSPAPER VALUES ('Sports', 'D', 1);
INSERT INTO NEWSPAPER VALUES ('Editorials', 'A', 12);
INSERT INTO NEWSPAPER VALUES ('Business', 'E', 1);
INSERT INTO NEWSPAPER VALUES ('Weather', 'C', 2);
INSERT INTO NEWSPAPER VALUES ('Television', 'B', 7);
INSERT INTO NEWSPAPER VALUES ('Births', 'F', 7);
INSERT INTO NEWSPAPER VALUES ('Classified', 'F', 8);
INSERT INTO NEWSPAPER VALUES ('Modern Life', 'B', 1);
INSERT INTO NEWSPAPER VALUES ('Comics', 'C', 4);
INSERT INTO NEWSPAPER VALUES ('Movies', 'B', 4);
INSERT INTO NEWSPAPER VALUES ('Bridge', 'B', 2);
INSERT INTO NEWSPAPER VALUES ('Obituaries', 'F', 6);
INSERT INTO NEWSPAPER VALUES ('Doctor Is In', 'F', 6);
```

## Queries

### Display Table Contents
```sql
SELECT * FROM NEWSPAPER;
```

### Display Table Schema
```sql
DESCRIBE NEWSPAPER;
```

### Feedback Management
- Turn off feedback:
  ```sql
  SET FEEDBACK OFF;
  ```
- Set feedback to display 25 rows:
  ```sql
  SET FEEDBACK 25;
  ```
- Show feedback status:
  ```sql
  SHOW FEEDBACK;
  ```

### Conditional Queries
- Show all records where section is 'F':
  ```sql
  SELECT * FROM NEWSPAPER WHERE Section = 'F';
  ```
- Order records by feature and display section 'F' only:
  ```sql
  SELECT * FROM NEWSPAPER WHERE Section = 'F' ORDER BY Feature;
  ```
- Order records by page (ascending) and feature for section 'F':
  ```sql
  SELECT * FROM NEWSPAPER WHERE Section = 'F' ORDER BY Page, Feature;
  ```
- Order records by page (descending) and feature for section 'F':
  ```sql
  SELECT * FROM NEWSPAPER WHERE Section = 'F' ORDER BY Page DESC, Feature;
  ```

### Page Queries
- Find records on a specific page:
  ```sql
  SELECT Feature, Section, Page FROM NEWSPAPER WHERE Page = 6;
  ```
- Find records where the page number is greater than 6:
  ```sql
  SELECT Feature, Section, Page FROM NEWSPAPER WHERE Page > 6;
  ```
- Find records where the page number is less than 6:
  ```sql
  SELECT Feature, Section, Page FROM NEWSPAPER WHERE Page < 6;
  ```
- Find records where the page number is not 6:
  ```sql
  SELECT Feature, Section, Page FROM NEWSPAPER WHERE Page != 6;
  ```

### Section Queries
- Find records where section is greater than 'B':
  ```sql
  SELECT Feature, Section, Page FROM NEWSPAPER WHERE Section > 'B';
  ```
- Use `LIKE` for pattern matching:
  - Features starting with 'Mo':
    ```sql
    SELECT * FROM NEWSPAPER WHERE Feature LIKE 'Mo%';
    ```
  - Features with the second letter as 'i':
    ```sql
    SELECT * FROM NEWSPAPER WHERE Feature LIKE '__i%';
    ```
  - Features containing 'o' twice:
    ```sql
    SELECT * FROM NEWSPAPER WHERE Feature LIKE '%o%o%';
    ```
  - Features containing 'i' twice:
    ```sql
    SELECT * FROM NEWSPAPER WHERE Feature LIKE '%i%i%';
    ```

### Inclusion and Range Queries
- Select records where section is in ('A', 'B', 'F'):
  ```sql
  SELECT Feature, Section, Page FROM NEWSPAPER WHERE Section IN ('A', 'B', 'F');
  ```
- Select records where section is not in ('A', 'B', 'F'):
  ```sql
  SELECT Feature, Section, Page FROM NEWSPAPER WHERE Section NOT IN ('A', 'B', 'F');
  ```
- Find records where the page is between 7 and 10:
  ```sql
  SELECT Feature, Section, Page FROM NEWSPAPER WHERE Page BETWEEN 7 AND 10;
  ```

### Compound Conditions
- Records where section is 'F' and page is greater than 7:
  ```sql
  SELECT Feature, Section, Page FROM NEWSPAPER WHERE Section = 'F' AND Page > 7;
  ```
- Records where section is 'F' or page is greater than 7:
  ```sql
  SELECT Feature, Section, Page FROM NEWSPAPER WHERE Section = 'F' OR Page > 7;
  ```
- Records where section is 'A' or 'B' and page is greater than 2:
  ```sql
  SELECT Feature, Section, Page FROM NEWSPAPER WHERE Page > 2 AND (Section = 'A' OR Section = 'B');
  ```

### Subqueries
- Find the section of a specific feature:
  ```sql
  SELECT Section FROM NEWSPAPER WHERE Feature = 'Doctor Is In';
  ```
- Find features in the same section as 'Doctor Is In':
  ```sql
  SELECT FEATURE FROM NEWSPAPER WHERE Section = (SELECT Section FROM NEWSPAPER WHERE Feature = 'Doctor Is In');
  ```
- Find all records where the section matches those on page 1:
  ```sql
  SELECT * FROM NEWSPAPER WHERE Section = (SELECT Section FROM NEWSPAPER WHERE Page = 1);
  ```
- Find all records where the section is lexicographically less than 'Doctor Is In':
  ```sql
  SELECT * FROM NEWSPAPER WHERE Section < (SELECT Section FROM NEWSPAPER WHERE Feature = 'Doctor Is In');
  ```

## How to Use
1. Clone this repository:
   ```bash
   [[git clone https://github.com/your-username/newspaper-sql-queries.git](https://github.com/sadman2084/-Oracle-Database.git)](https://github.com/sadman2084/-Oracle-Database.git)
   ```
2. Import the SQL script into your database.
3. Run the queries as needed.

## License
This project is licensed under the MIT License. Feel free to use and modify it as needed.

