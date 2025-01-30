## Overview
This script creates two database users (`C##BOB` and `C##JUDY`), manages privileges, and performs various operations on a `NEWSPAPER` table in an Oracle database.

## Prerequisites
- Oracle Database with Container Database (CDB) enabled
- SYSDBA access to execute `DROP USER` and `CREATE USER` commands

---
## Script Breakdown

### 1. **Dropping Existing Users**
```sql
DROP USER C##BOB CASCADE;
DROP USER C##JUDY CASCADE;
```
- Deletes the users `C##BOB` and `C##JUDY`, along with all their objects.

### 2. **Creating Users(From System)**
```sql
CREATE USER C##BOB IDENTIFIED BY pizza;
GRANT CREATE SESSION, CREATE TABLE, CREATE SYNONYM TO C##BOB;
```
- Creates user `C##BOB` with password `pizza`
- Grants session creation, table creation, and synonym creation privileges

```sql
CREATE USER C##JUDY IDENTIFIED BY burger;
GRANT CREATE SESSION TO C##JUDY;
```
- Creates user `C##JUDY` with password `burger`
- Grants only session creation privilege

### 3. **Creating and Populating the `NEWSPAPER` Table**
```sql
CONNECT C##BOB/pizza;

CREATE TABLE NEWSPAPER (
    Feature VARCHAR2(15) NOT NULL,
    Section CHAR(1),
    Page NUMBER
);
```
- Logs into `C##BOB`
- Creates the `NEWSPAPER` table with three columns

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
- Inserts sample records into the `NEWSPAPER` table

### 4. **Granting Permissions to `C##JUDY`**
```sql
GRANT SELECT ON NEWSPAPER TO C##JUDY;
GRANT INSERT, UPDATE, DELETE ON NEWSPAPER TO C##JUDY;
```
- Allows `C##JUDY` to read, insert, update, and delete records in `NEWSPAPER`

### 5. **Performing CRUD Operations as `C##JUDY`**
```sql
CONNECT C##JUDY/burger;

SELECT * FROM C##BOB.NEWSPAPER;
```
- Logs in as `C##JUDY` and selects all records from `C##BOB.NEWSPAPER`

```sql
INSERT INTO NEWSPAPER VALUES ('Sadman is alive', 'F', 6);
```
- Inserts a new record into `NEWSPAPER`

```sql
UPDATE C##BOB.NEWSPAPER
SET FEATURE = 'cat'
WHERE PAGE = 1;
```
- Updates the `FEATURE` column to `'cat'` for rows where `PAGE = 1`

```sql
DELETE FROM C##BOB.NEWSPAPER
WHERE SECTION = 'D';
```
- Deletes records where `SECTION = 'D'`

---
## Expected Output
After execution, `C##BOB` will have a `NEWSPAPER` table with initial data. `C##JUDY` can:
1. View the table (`SELECT` query)
2. Add new records (`INSERT` query)
3. Modify existing records (`UPDATE` query)
4. Remove records (`DELETE` query)

---
## Notes
- Ensure you have `SYSDBA` privileges before running the script.
- `CASCADE` in `DROP USER` ensures all objects owned by the user are also removed.
- Run each section in order to avoid permission errors.

Happy coding! 🚀

