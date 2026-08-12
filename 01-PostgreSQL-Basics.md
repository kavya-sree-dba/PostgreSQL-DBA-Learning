#postgresql basics
# PostgreSQL Basics

This section covers the PostgreSQL fundamentals I am learning and practicing as part of my PostgreSQL DBA learning.

## What is PostgreSQL?

PostgreSQL is an open-source relational database management system used to store, manage, and retrieve data.

It supports SQL, transactions, indexing, views, functions, JSON and JSONB, backup and recovery, replication, and other features used in database administration.

## PostgreSQL DBA Fundamentals

The main areas I am learning include:

* PostgreSQL installation and configuration
* Database and schema management
* Users, roles, and permissions
* SQL commands and queries
* Tables and indexes
* Transactions and locks
* MVCC
* WAL (Write-Ahead Logging)
* Backup and recovery
* Replication
* Performance monitoring and tuning
* Troubleshooting
* Database security
* Linux administration for PostgreSQL

## PostgreSQL Architecture

A PostgreSQL environment includes the PostgreSQL server, databases, schemas, tables, indexes, users, processes, and configuration files.

Understanding these components is important for managing PostgreSQL databases and troubleshooting database issues.

## Checking PostgreSQL Version

To check the PostgreSQL version:

```sql
SELECT version();
```

From the Linux command line:

```bash
psql --version
```

## Connecting to PostgreSQL

Connect using the psql command-line tool:

```bash
psql -U postgres
```

Connect to a specific database:

```bash
psql -U postgres -d postgres
```

## Useful psql Commands

List all databases:

```text
\l
```

Connect to a database:

```text
\c database_name
```

List tables:

```text
\dt
```

View the structure of a table:

```text
\d table_name
```

List PostgreSQL users and roles:

```text
\du
```

Exit the PostgreSQL shell:

```text
\q
```

Check the current user:

```sql
SELECT current_user;
```

Check the current database:

```sql
SELECT current_database();
```

## Creating a Database

```sql
CREATE DATABASE dba_practice;
```

Connect to the database:

```text
\c dba_practice
```

## Creating a Schema

A schema helps organize database objects such as tables, views, and functions.

```sql
CREATE SCHEMA practice;
```

## Creating a Table

```sql
CREATE TABLE practice.employees (
    employee_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(100),
    salary NUMERIC(10,2)
);
```

## Inserting Data

```sql
INSERT INTO practice.employees
(name, department, salary)
VALUES
('Kavya', 'Database', 45000),
('Anitha', 'IT', 50000),
('Rahul', 'Support', 40000);
```

## Reading Data

Display all records:

```sql
SELECT * FROM practice.employees;
```

Select specific columns:

```sql
SELECT name, salary
FROM practice.employees;
```

## Updating Data

```sql
UPDATE practice.employees
SET salary = 55000
WHERE name = 'Anitha';
```

## Deleting Data

```sql
DELETE FROM practice.employees
WHERE name = 'Rahul';
```

## Important PostgreSQL DBA Concepts

### Users and Roles

PostgreSQL uses roles to manage database access and permissions. Understanding role creation, privileges, authentication, and ownership is important for database security.

### Transactions

Transactions group database operations into a single unit of work. PostgreSQL provides commands such as BEGIN, COMMIT, and ROLLBACK to manage transactions.

### MVCC

MVCC stands for Multi-Version Concurrency Control. PostgreSQL uses MVCC to manage concurrent transactions and maintain data consistency.

### WAL

WAL stands for Write-Ahead Logging. PostgreSQL records changes in the WAL before the corresponding changes are written to the main data files.

WAL is important for:

* Crash recovery
* Backup and recovery
* Point-in-Time Recovery
* Streaming replication

### Vacuum and Autovacuum

Vacuum helps PostgreSQL clean up old row versions created by updates and deletes.

Autovacuum automatically performs maintenance tasks in the background and helps keep database tables healthy.

### Indexes

Indexes help PostgreSQL find rows more efficiently and can improve query performance.

However, unnecessary indexes can increase storage usage and affect write performance, so indexes should be created based on query requirements.

## Hands-on Practice

I am using practical exercises to improve my PostgreSQL DBA skills.

### Practice Database

Create a database:

```sql
CREATE DATABASE dba_lab;
```

Connect to the database:

```text
\c dba_lab
```

Create a schema:

```sql
CREATE SCHEMA lab;
```

Create an employee table:

```sql
CREATE TABLE lab.employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary NUMERIC(10,2)
);
```

### Practice Tasks

* Create and manage databases
* Create schemas and tables
* Insert sample records
* Retrieve data using SQL queries
* Update records
* Delete records
* Create and understand indexes
* Manage PostgreSQL users and roles
* Grant and revoke permissions
* Check database and table information
* Practice basic troubleshooting

## PostgreSQL Commands Practiced

| Task                     | Command                      |
| ------------------------ | ---------------------------- |
| Check PostgreSQL version | `SELECT version();`          |
| List databases           | `\l`                         |
| Connect to database      | `\c database_name`           |
| List tables              | `\dt`                        |
| Describe table           | `\d table_name`              |
| List roles               | `\du`                        |
| Show current user        | `SELECT current_user;`       |
| Show current database    | `SELECT current_database();` |
| Exit psql                | `\q`                         |

## Learning Progress

* [x] PostgreSQL fundamentals
* [x] Basic SQL commands
* [x] Database and schema basics
* [x] Basic table management
* [ ] PostgreSQL installation and configuration
* [ ] Users, roles, and permissions
* [ ] Backup and restore
* [ ] WAL and Point-in-Time Recovery
* [ ] PostgreSQL replication
* [ ] Performance tuning
* [ ] Monitoring
* [ ] Troubleshooting
* [ ] Database security
* [ ] PostgreSQL DBA projects

## Next Steps

My next focus areas are PostgreSQL installation and configuration, user and role management, backup and recovery, WAL, replication, monitoring, troubleshooting, and performance tuning.
