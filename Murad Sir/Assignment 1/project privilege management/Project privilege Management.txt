1.	Create a user named Person2, give password 1234, give privilege CREATE SESSION to Person2; Create another user named Person1, give password 5678, give privilege CREATE SESSION, CREATE TABLE, CREATE VIEW, CREATE SYNONYM to Person1; create a table named NEWSPAPER under user Person1, insert data into NEWSPAPER table as user Person1, give privileges SELECT, insert ON NEWSPAPER table TO Person2 from user Person1; Show NEWSPAPER table and insert data into NEWSPAPER table from user Person2. 


SHOW USER;
CONNECT system/1234;


SQL> CREATE USER C##Person2 IDENTIFIED BY 1234;

User created.

SQL> GRANT CREATE SESSION TO C##Person2;

Grant succeeded.



SQL> CREATE USER C##Person1 IDENTIFIED BY 5678;

User created.

SQL> GRANT CREATE SESSION, CREATE TABLE, CREATE VIEW, CREATE SYNONYM TO C##Person1;

Grant succeeded.


SQL> CONNECT C##Person1/5678;
Connected.


SQL> CREATE TABLE NEWSPAPER (
  2      ID NUMBER PRIMARY KEY,
  3      NAME VARCHAR2(100),
  4      PRICE NUMBER(5,2)
  5  );

Table created.



SQL> connect system/1234;
Connected.



SQL> connect system/1234;
Connected.
SQL> ALTER USER C##Person1 QUOTA UNLIMITED ON USERS;

User altered.

SQL> ALTER USER C##Person2 QUOTA UNLIMITED ON USERS;

User altered.




SQL> CONNECT C##Person1/5678;
Connected.





SQL> INSERT INTO NEWSPAPER (ID, NAME, PRICE) VALUES (1, 'Daily News', 10.50);

1 row created.

SQL> INSERT INTO NEWSPAPER (ID, NAME, PRICE) VALUES (2, 'Morning Times', 12.00);

1 row created.

SQL> INSERT INTO NEWSPAPER (ID, NAME, PRICE) VALUES (3, 'Evening Star', 15.75);

1 row created.




SQL> GRANT SELECT, INSERT ON C##Person1.NEWSPAPER TO C##Person2;

Grant succeeded.


SQL> CONNECT C##Person2/1234;
Connected.


SQL> SELECT * FROM C##Person1.NEWSPAPER;

        ID
----------
NAME
--------------------------------------------------------------------------------
     PRICE
----------
         1
Daily News
      10.5

         2
Morning Times
        12

        ID
----------
NAME
--------------------------------------------------------------------------------
     PRICE
----------

         3
Evening Star
     15.75




SQL> INSERT INTO C##Person1.NEWSPAPER (ID, NAME, PRICE) VALUES (4, 'Weekly Journal', 20.00);

1 row created.

SQL> COMMIT;

Commit complete.



SELECT * FROM C##Person1.NEWSPAPER; 



SQL> SELECT * FROM C##Person1.NEWSPAPER;

        ID
----------
NAME
--------------------------------------------------------------------------------
     PRICE
----------
         1
Daily News
      10.5

         2
Morning Times
        12

        ID
----------
NAME
--------------------------------------------------------------------------------
     PRICE
----------

         3
Evening Star
     15.75

         4
Weekly Journal

        ID
----------
NAME
--------------------------------------------------------------------------------
     PRICE
----------
        20