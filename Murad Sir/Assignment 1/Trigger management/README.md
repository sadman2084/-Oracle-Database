README - BOOKSHELF Rating Audit Trigger
----------------------------------------

📌 Objective:
To maintain an audit trail for rating updates in the BOOKSHELF table using a BEFORE UPDATE row-level trigger.

✅ Step-by-Step Process:

1️⃣ Create the Audit Table:
   Table Name: BOOKSHELF_AUDIT

```sql
   CREATE TABLE BOOKSHELF_AUDIT (
       Audit_ID NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
       Book_ID NUMBER,
       Old_Rating NUMBER(2,1),
       New_Rating NUMBER(2,1),
       Updated_By VARCHAR2(100),
       Update_Time TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );
```

2️⃣ Create the Main Table:
   Table Name: BOOKSHELF

  ```sql
   CREATE TABLE BOOKSHELF (
       Book_ID NUMBER PRIMARY KEY,
       Title VARCHAR2(200),
       Author VARCHAR2(100),
       Rating NUMBER(2,1) NULL
   );
   ```

3️⃣ Insert Initial Data:

   ```sql
   INSERT INTO BOOKSHELF (Book_ID, Title, Author, Rating)
   VALUES (1, 'Bilai Zindabaad', 'Paulo Coelho', 4.5);

   INSERT INTO BOOKSHELF (Book_ID, Title, Author, Rating)
   VALUES (2, 'Mehedi Mota', 'George Orwell', 4.8);

   INSERT INTO BOOKSHELF (Book_ID, Title, Author, Rating)
   VALUES (3, 'Bilai', 'Harper Lee', 4.9);

   COMMIT;
   ```

4️⃣ Create the Trigger:

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

5️⃣ Test the Trigger with UPDATEs:

   ```sql
   UPDATE BOOKSHELF SET Rating = 4.7 WHERE Book_ID = 1;
   UPDATE BOOKSHELF SET Rating = 4.6 WHERE Book_ID = 2;
   COMMIT;
   ```

6️⃣ Verify the Audit Entries:

   ```sql
   SELECT * FROM BOOKSHELF_AUDIT;
   ```

📊 Sample Output:

| AUDIT_ID | BOOK_ID | OLD_RATING | NEW_RATING | UPDATED_BY | UPDATE_TIME                  |
|----------|---------|------------|------------|------------|------------------------------|
| 1        | 1       | 4.5        | 4.7        | SYSTEM     | 09-FEB-25 12.41.15.842 PM    |
| 2        | 2       | 4.8        | 4.6        | SYSTEM     | 09-FEB-25 12.41.15.844 PM    |

📝 Note:
- Trigger ensures audit entry is only made if the **Rating** actually changes.
- Timestamp is generated using `SYSTIMESTAMP`.
- User who performed the update is captured via `USER`.

```

---

