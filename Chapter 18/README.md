# User and Profile Management in Oracle Database

This document provides instructions for managing users and profiles in Oracle Database, including creating users, assigning privileges, locking/unlocking accounts, and modifying profiles.

---

## Show Current User
```sql
SHOW USER;
```

## Connect to System User
```sql
CONNECT system/1234;
```

## Create a Profile
```sql
CREATE PROFILE C##limited_profile
  LIMIT
        PASSWORD_LOCK_TIME 1
        FAILED_LOGIN_ATTEMPTS 2;
```

## Create a User and Assign a Profile
```sql
CREATE USER C##jane IDENTIFIED BY eyre PROFILE C##LIMITED_PROFILE;
```

## Grant Privileges to a User
```sql
GRANT CREATE SESSION TO C##mehedi;
```

## Connect as a User
```sql
CONNECT C##jane/eyre;
```

## Change Password
### Option 1:
```sql
PASSWORD;
```
### Option 2:
```sql
ALTER USER C##jane IDENTIFIED BY test;
```

## Account Lock After Failed Login Attempts
After 2 failed login attempts, the account will be locked. To unlock it:
```sql
ALTER USER C##JANE ACCOUNT UNLOCK;
```

## Manually Lock a User Account
```sql
ALTER USER C##JANE ACCOUNT LOCK;
```

## Expire the Password and Set a New Password
```sql
ALTER USER C##jane PASSWORD EXPIRE;
```

## Update a Profile with New Privileges
```sql
ALTER PROFILE C##LIMITED_PROFILE LIMIT
        PASSWORD_LIFE_TIME 10
        PASSWORD_GRACE_TIME 8
        PASSWORD_REUSE_MAX 3
        PASSWORD_LOCK_TIME 1
        FAILED_LOGIN_ATTEMPTS 2
        PASSWORD_REUSE_TIME UNLIMITED;
```

## View All Profiles
```sql
SELECT * FROM DBA_PROFILES;
```

## Grant Permissions to Create Table, View, and Synonym
```sql
GRANT CREATE VIEW, CREATE TABLE, CREATE SYNONYM TO C##jane;
```

## Allow User to Create Tables
### Unlimited Quota:
```sql
ALTER USER C##jane QUOTA UNLIMITED ON USERS;
```
### Limit Quota to 100MB:
```sql
ALTER USER C##jane QUOTA 100M ON USERS;
```

## Delete a User
```sql
DROP USER C##jane;
```

## Delete a Profile
```sql
DROP PROFILE C##limited_profile;
```

---

## Notes
1. Profiles allow administrators to define resource limits and password policies for users.
2. Use the `QUOTA` clause to manage the storage available to a user.
3. Always test changes in a non-production environment before applying them in production.

---

## Contributing
Feel free to fork this repository and submit pull requests with improvements or additional features.

