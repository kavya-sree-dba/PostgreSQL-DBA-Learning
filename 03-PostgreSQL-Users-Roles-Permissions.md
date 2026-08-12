# PostgreSQL Users, Roles and Permissions

This section covers PostgreSQL users, roles, authentication, ownership, and permissions. These are important areas of PostgreSQL database administration because they control who can access databases and what actions they are allowed to perform.

## PostgreSQL Roles

PostgreSQL uses the concept of **roles** to manage database access.

A role can act as:

* A login user
* A group of users
* An owner of database objects
* A role that receives specific privileges

In PostgreSQL, users are essentially roles that have the `LOGIN` privilege.

## Viewing Existing Roles

To list all PostgreSQL roles using `psql`:

```text
\du
```

Another option is:

```sql
SELECT rolname, rolsuper, rolcanlogin
FROM pg_roles;
```

## Creating a Role

Create a role without login access:

```sql
CREATE ROLE developer;
```

Create a role that can log in:

```sql
CREATE ROLE app_user
LOGIN
PASSWORD 'StrongPasswordHere';
```

For real production systems, passwords should be managed securely and should not be stored directly in scripts or public repositories.

## Creating a User

PostgreSQL also supports the `CREATE USER` command.

Example:

```sql
CREATE USER report_user
WITH PASSWORD 'StrongPasswordHere';
```

A user created with `CREATE USER` can log in by default.

## Role Attributes

PostgreSQL roles can have different attributes.

Some important attributes include:

* `LOGIN`
* `SUPERUSER`
* `CREATEDB`
* `CREATEROLE`
* `REPLICATION`
* `BYPASSRLS`

View role attributes:

```sql
SELECT rolname,
       rolsuper,
       rolcreatedb,
       rolcreaterole,
       rolreplication,
       rolcanlogin
FROM pg_roles;
```

## LOGIN Privilege

A role needs the `LOGIN` attribute to connect to PostgreSQL.

Create a login role:

```sql
CREATE ROLE analyst
LOGIN
PASSWORD 'StrongPasswordHere';
```

Enable login for an existing role:

```sql
ALTER ROLE analyst LOGIN;
```

Disable login:

```sql
ALTER ROLE analyst NOLOGIN;
```

## Password Management

Set or change a role password:

```sql
ALTER ROLE analyst
PASSWORD 'NewStrongPasswordHere';
```

For production environments, strong passwords and secure credential management should always be used.

## Creating a Group Role

A group role can be used to manage privileges for multiple users.

Create a group role:

```sql
CREATE ROLE reporting_group;
```

Create a user:

```sql
CREATE ROLE analyst1 LOGIN
PASSWORD 'StrongPasswordHere';
```

Add the user to the group:

```sql
GRANT reporting_group TO analyst1;
```

This approach makes permission management easier when multiple users need the same access.

## GRANT

The `GRANT` command is used to provide privileges on database objects.

For example:

```sql
GRANT CONNECT ON DATABASE dba_lab TO analyst;
```

Grant schema usage:

```sql
GRANT USAGE ON SCHEMA lab TO analyst;
```

Grant table privileges:

```sql
GRANT SELECT ON TABLE lab.employees TO analyst;
```

Multiple privileges can be granted together:

```sql
GRANT SELECT, INSERT, UPDATE
ON TABLE lab.employees
TO analyst;
```

## REVOKE

The `REVOKE` command removes previously granted privileges.

Example:

```sql
REVOKE UPDATE
ON TABLE lab.employees
FROM analyst;
```

Remove all table privileges:

```sql
REVOKE ALL
ON TABLE lab.employees
FROM analyst;
```

## Database Privileges

Common database privileges include:

* `CONNECT`
* `CREATE`
* `TEMPORARY`

Example:

```sql
GRANT CONNECT ON DATABASE dba_lab TO analyst;
```

## Schema Privileges

Common schema privileges include:

* `USAGE`
* `CREATE`

Example:

```sql
GRANT USAGE ON SCHEMA lab TO analyst;
```

Allow a role to create objects in a schema:

```sql
GRANT CREATE ON SCHEMA lab TO developer;
```

## Table Privileges

Common table privileges include:

* `SELECT`
* `INSERT`
* `UPDATE`
* `DELETE`
* `TRUNCATE`
* `REFERENCES`
* `TRIGGER`

Example:

```sql
GRANT SELECT, INSERT, UPDATE
ON lab.employees
TO developer;
```

## Sequence Privileges

Sequences are commonly used with columns such as `SERIAL`.

A role may need sequence privileges when inserting data into tables that use sequences.

Example:

```sql
GRANT USAGE, SELECT
ON SEQUENCE lab.employees_id_seq
TO developer;
```

## Default Privileges

Default privileges can automatically grant permissions on objects created in the future.

Example:

```sql
ALTER DEFAULT PRIVILEGES
IN SCHEMA lab
GRANT SELECT ON TABLES TO analyst;
```

This is useful when a role should automatically receive access to newly created tables.

## Role Membership

A role can be granted to another role.

Example:

```sql
GRANT reporting_group TO analyst;
```

Remove the role membership:

```sql
REVOKE reporting_group FROM analyst;
```

This allows database administrators to manage permissions through group roles instead of granting every privilege separately to every user.

## Role Ownership

Database objects have owners.

Check table ownership:

```sql
SELECT schemaname,
       tablename,
       tableowner
FROM pg_tables
WHERE schemaname = 'lab';
```

Change the owner of a table:

```sql
ALTER TABLE lab.employees
OWNER TO developer;
```

Ownership is important because the owner normally has full control over the object.

## Superuser

A PostgreSQL superuser has very high-level privileges and can bypass many permission checks.

Check whether a role is a superuser:

```sql
SELECT rolname, rolsuper
FROM pg_roles;
```

Create a superuser:

```sql
CREATE ROLE admin_user
LOGIN
SUPERUSER
PASSWORD 'StrongPasswordHere';
```

Superuser access should be limited because a superuser can perform almost any database operation.

## Authentication

PostgreSQL uses the `pg_hba.conf` file to control client authentication.

Find the location of the file:

```sql
SHOW hba_file;
```

The file contains rules that define:

* Database
* User or role
* Client address
* Authentication method

Common authentication methods include:

* `scram-sha-256`
* `md5`
* `peer`
* `trust`
* `cert`

Authentication settings should be configured carefully, especially when PostgreSQL accepts remote connections.

## Checking User Connections

The `pg_stat_activity` view can be used to check current database sessions.

```sql
SELECT pid,
       usename,
       datname,
       client_addr,
       state
FROM pg_stat_activity;
```

This can help identify:

* Connected users
* Active sessions
* Client addresses
* Current session states

## Terminating a Session

A DBA may need to terminate a database session in certain troubleshooting situations.

First identify the process ID:

```sql
SELECT pid,
       usename,
       datname,
       state
FROM pg_stat_activity;
```

Then terminate the session:

```sql
SELECT pg_terminate_backend(pid);
```

This should be used carefully, especially in production environments, because terminating a session can roll back an active transaction.

## Practical Permission Example

Create a reporting role:

```sql
CREATE ROLE reporting_group;
```

Create a reporting user:

```sql
CREATE ROLE report_user
LOGIN
PASSWORD 'StrongPasswordHere';
```

Add the user to the reporting role:

```sql
GRANT reporting_group TO report_user;
```

Allow the role to connect to the database:

```sql
GRANT CONNECT
ON DATABASE dba_lab
TO reporting_group;
```

Allow access to the schema:

```sql
GRANT USAGE
ON SCHEMA lab
TO reporting_group;
```

Allow read-only access to the employees table:

```sql
GRANT SELECT
ON TABLE lab.employees
TO reporting_group;
```

The `report_user` can now receive these permissions through membership in `reporting_group`.

## Checking Privileges

In `psql`, table privileges can be viewed using:

```text
\dp lab.employees
```

Role information can be viewed using:

```text
\du
```

Database information can be viewed using:

```text
\l
```

## Security Best Practices

Some basic PostgreSQL security practices I am learning are:

* Avoid unnecessary superuser access
* Follow the principle of least privilege
* Use separate roles for different applications and users
* Use strong authentication methods
* Avoid using `trust` authentication unless there is a specific reason
* Review `pg_hba.conf` carefully
* Grant only the permissions required for a task
* Remove unused roles and permissions
* Avoid storing passwords in public repositories
* Monitor active database sessions
* Review permissions regularly

## Hands-on Practice

The following tasks are part of my PostgreSQL DBA practice:

* Create PostgreSQL roles
* Create login users
* Change role attributes
* Manage passwords
* Create group roles
* Grant database privileges
* Grant schema privileges
* Grant table privileges
* Revoke privileges
* Manage role membership
* Check role information
* Check table ownership
* Review active sessions
* Understand `pg_hba.conf`
* Practice least-privilege access

## Commands Practiced

| Task                   | Command                                            |
| ---------------------- | -------------------------------------------------- |
| List roles             | \du                                                |
| List databases         |  \l                                                |
| Create role            |  CREATE ROLE role_name;                            |
| Create login role      |  CREATE ROLE user_name LOGIN PASSWORD 'password';  |
| Change role            |  ALTER ROLE role_name ...;                         |
| Grant privilege        |  GRANT ... TO role_name;                           |
| Remove privilege       |  REVOKE ... FROM role_name;                        |
| Grant role membership  |  GRANT group_role TO user_name;                    |
| Remove role membership |  REVOKE group_role FROM user_name;                 |
| Check HBA file         |  SHOW hba_file;                                    |
| Check active sessions  |  SELECT * FROM pg_stat_activity;                   |
| Terminate session      |  SELECT pg_terminate_backend(pid);                 |

## Learning Progress

* [x] PostgreSQL roles
* [x] Login users
* [x] Role attributes
* [x] GRANT and REVOKE
* [x] Database privileges
* [x] Schema privileges
* [x] Table privileges
* [x] Role membership
* [x] Basic authentication concepts
* [ ] Advanced pg_hba.conf configuration
* [ ] Row-level security
* [ ] Advanced role management
* [ ] Production security practices

## Next Steps

My next focus area is database, schema, and table management. I will practice creating and altering database objects, managing ownership, working with constraints, and organizing PostgreSQL database objects effectively.
