# Newspaper Database Management

This repository contains SQL scripts for managing and querying a table named `NEWSPAPER`. The table stores information about features, sections, and page numbers of a newspaper. Below is a detailed guide for creating and interacting with this database.

## Table Creation

```sql
CREATE TABLE NEWSPAPER (
    Feature      VARCHAR2(15) NOT NULL,
    Section      CHAR(1),
    Page         NUMBER
);
```

## Data Insertion

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

## Basic Queries

### Display the Table
```sql
SELECT * FROM NEWSPAPER;
```

### Describe Table Structure
```sql
DESCRIBE NEWSPAPER;
```

## Feedback Settings

### Turn Feedback Off
```sql
SET FEEDBACK OFF;
```

### Set Feedback Limit
```sql
SET FEEDBACK 25;
```

### Show Feedback Status
```sql
SHOW FEEDBACK;
```

## Filtering and Sorting Queries

### Select Rows for Section 'F'
```sql
SELECT * FROM NEWSPAPER
WHERE Section = 'F';
```

### Select Rows for Section 'F' Ordered by Feature
```sql
SELECT * FROM NEWSPAPER
WHERE Section = 'F'
ORDER BY Feature;
```

### Select Rows for Section 'F' Ordered by Page and Feature
```sql
SELECT * FROM NEWSPAPER
WHERE Section = 'F'
ORDER BY Page, Feature;
```

### Select Rows for Section 'F' Ordered by Page (Descending) and Feature
```sql
SELECT * FROM NEWSPAPER
WHERE Section = 'F'
ORDER BY Page DESC, Feature;
```

## Conditional Queries

### Select Rows Where Page = 6
```sql
SELECT Feature, Section, Page
FROM NEWSPAPER
WHERE Page = 6;
```

### Select Rows Where Page > 6
```sql
SELECT Feature, Section, Page
FROM NEWSPAPER
WHERE Page > 6;
```

### Select Rows Where Page < 6
```sql
SELECT Feature, Section, Page
FROM NEWSPAPER
WHERE Page < 6;
```

### Select Rows Where Page != 6
```sql
SELECT Feature, Section, Page
FROM NEWSPAPER
WHERE Page != 6;
```

### Select Rows Where Section > 'B'
```sql
SELECT Feature, Section, Page
FROM NEWSPAPER
WHERE Section > 'B';
```

## Pattern Matching Queries

### Features Starting with 'Mo'
```sql
SELECT * FROM NEWSPAPER
WHERE Feature LIKE 'Mo%';
```

### Features with Second Letter 'i'
```sql
SELECT * FROM NEWSPAPER
WHERE Feature LIKE '__i%';
```

### Features Containing 'o' Twice
```sql
SELECT * FROM NEWSPAPER
WHERE Feature LIKE '%o%o%';
```

### Features Containing 'i' Twice
```sql
SELECT * FROM NEWSPAPER
WHERE Feature LIKE '%i%i%';
```

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## Author

Sadman Kabir

