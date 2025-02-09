# Password Management Profile in Oracle

## Overview
This document describes the creation and testing of an Oracle profile named `Password_Management` that enforces password policies such as expiration time, lock time, reuse limits, and failed login attempts.

## Profile Creation
The following SQL command creates a profile `C##Password_Management` with the specified password policies:

```sql
CREATE PROFILE C##Password_Management
LIMIT
    PASSWORD_LIFE_TIME 10
    PASSWORD_GRACE_TIME 8
    PASSWORD_REUSE_MAX 3
    PASSWORD_LOCK_TIME 1
    FAILED_LOGIN_ATTEMPTS 2
    PASSWORD_REUSE_TIME 10;
```

### Profile Verification
To check the created profile, use:
```sql
SELECT * FROM DBA_PROFILES WHERE UPPER(PROFILE) = 'C##PASSWORD_MANAGEMENT';
```

## User Creation and Profile Assignment
```sql
CREATE USER C##test IDENTIFIED BY test PROFILE C##Password_Management;
GRANT CREATE SESSION TO C##test;
```

## Testing the Profile

### Password Reuse Limit
Testing `PASSWORD_REUSE_MAX 3` by changing the password multiple times:
```sql
CONNECT C##test/test;
PASSWORD;
PASSWORD;
PASSWORD;
PASSWORD;
```
The fourth attempt results in:
```
ERROR:
ORA-28007: the password cannot be reused
```

### Account Locking on Failed Login Attempts
```sql
CONNECT C##test/sad;
CONNECT C##test/sad;
CONNECT C##test/sad;
```
After two failed attempts, the third attempt results in:
```
ERROR:
ORA-28000: The account is locked.
```

### Grace Period and Password Expiration
```sql
CONNECT C##test/sadman;
```
Displays:
```
ORA-28002: the password will expire within 8 days
```
After 10 days:
```
ORA-28001: the password has expired
```
The user is then forced to reset their password.

## Justification: PASSWORD_REUSE_MAX vs. PASSWORD_REUSE_TIME
Oracle enforces password security using two mutually exclusive parameters:
- **PASSWORD_REUSE_MAX**: Defines the number of different passwords a user must use before reusing an old one.
- **PASSWORD_REUSE_TIME**: Defines the number of days that must pass before reusing an old password.

Since these parameters serve different purposes, setting both simultaneously could create conflicts. For example, if `PASSWORD_REUSE_MAX` allows reuse after 3 changes but `PASSWORD_REUSE_TIME` enforces a 10-day restriction, it may cause inconsistency in password enforcement. To prevent this, Oracle does not allow `PASSWORD_REUSE_MAX` to be set if `PASSWORD_REUSE_TIME` is set to `UNLIMITED`, and vice versa.

