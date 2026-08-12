# Database, Schema and Table Management

This section covers the basic management of PostgreSQL databases, schemas, tables, constraints, sequences, and views. These are important concepts for a PostgreSQL DBA because database objects need to be created, maintained, secured, and organized properly.

## PostgreSQL Database

A database is a collection of related data and database objects.

A PostgreSQL server can contain multiple databases.

List databases:

```text
\l
```

Create a database:

```sql
CREATE DATABASE dba_lab;
```

Connect to a database:

```text
\c dba_lab
```

Check the current database:

```sql
SELECT current_database();
```

## Database Information

PostgreSQL provides system catalogs and views that can be used to understand database information.

List databases with SQL:

```sql
SELECT datname
FROM pg_database
ORDER BY datname;
```

Check database size:

```sql
SELECT pg_size_pretty(pg_database_size(current_database()));
```

Check the size of a specific database:

```sql
SELECT pg_size_pretty(pg_database_size('dba_lab'));
```

## Altering a Database

Change the database name:

```sql
ALTER DATABASE dba_lab
RENAME TO postgres_lab;
```

Change the database owner:

```sql
ALTER DATABASE postgres_lab
OWNER TO postgres;
```

Changing database properties should be done carefully, especially when working with a production database.

## Dropping a Database

A database can be removed using:

```sql
DROP DATABASE postgres_lab;
```

A database should only be dropped when it is no longer required.

Before dropping a database, a DBA should verify that:

* The database is no longer required
* Required backups are available
* No important application is using it
* The correct database has been selected

## PostgreSQL Schemas

A schema is a logical namespace used to organize database objects.

A database can contain multiple schemas.

List schemas:

```text
\dn
```

Create a schema:

```sql
CREATE SCHEMA company;
```

Create a table inside a schema:

```sql
CREATE TABLE company.employees (
    employee_id SERIAL PRIMARY KEY,
    employee_name VARCHAR(100),
    department VARCHAR(100)
);
```

## Schema Ownership

Create a schema with a specific owner:

```sql
CREATE SCHEMA reporting
AUTHORIZATION postgres;
```

Change schema ownership:

```sql
ALTER SCHEMA reporting
OWNER TO postgres;
```

Check schemas:

```sql
SELECT schema_name
FROM information_schema.schemata
ORDER BY schema_name;
```

## Search Path

PostgreSQL uses the `search_path` setting to determine which schemas are searched when an object name is not fully qualified.

Check the current search path:

```sql
SHOW search_path;
```

Set a session-level search path:

```sql
SET search_path TO company, public;
```

Using schema-qualified names such as:

```sql
SELECT *
FROM company.employees;
```

can make SQL statements clearer and reduce ambiguity.

## PostgreSQL Tables

Tables are used to store structured data.

Create a simple table:

```sql
CREATE TABLE company.departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);
```

Create an employee table:

```sql
CREATE TABLE company.employees (
    employee_id SERIAL PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    email VARCHAR(150),
    department_id INTEGER,
    salary NUMERIC(10,2)
);
```

List tables:

```text
\dt company.*
```

View table structure:

```text
\d company.employees
```

## Common PostgreSQL Data Types

Some commonly used PostgreSQL data types are:

| Data Type   | Use                                  |
| ----------- | ------------------------------------ |
| INTEGER     | Whole numbers                        |
| BIGINT      | Large whole numbers                  |
| NUMERIC     | Exact numeric values                 |
| VARCHAR     | Variable-length text                 |
| TEXT        | Text without a specific length limit |
| BOOLEAN     | True or false values                 |
| DATE        | Date values                          |
| TIMESTAMP   | Date and time                        |
| TIMESTAMPTZ | Date and time with time zone         |
| JSON        | JSON data                            |
| JSONB       | Binary JSON data                     |
| UUID        | Universally unique identifiers       |

The data type should be selected based on the type of information being stored and the application requirements.

## Primary Key

A primary key uniquely identifies each row in a table.

Example:

```sql
CREATE TABLE company.departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);
```

The `department_id` column is the primary key.

A table can have only one primary key constraint, although the primary key can contain multiple columns.

## Foreign Key

A foreign key creates a relationship between tables.

Example:

```sql
CREATE TABLE company.employees (
    employee_id SERIAL PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    department_id INTEGER,
    CONSTRAINT fk_department
        FOREIGN KEY (department_id)
        REFERENCES company.departments(department_id)
);
```

The `department_id` column references the department table.

## NOT NULL Constraint

The `NOT NULL` constraint prevents a column from storing null values.

Example:

```sql
CREATE TABLE company.users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(100) NOT NULL
);
```

## UNIQUE Constraint

A `UNIQUE` constraint prevents duplicate values.

Example:

```sql
CREATE TABLE company.users (
    user_id SERIAL PRIMARY KEY,
    username VARCHAR(100) NOT NULL UNIQUE
);
```

## CHECK Constraint

A `CHECK` constraint ensures that data meets a specified condition.

Example:

```sql
CREATE TABLE company.employees (
    employee_id SERIAL PRIMARY KEY,
    employee_name VARCHAR(100),
    salary NUMERIC(10,2)
        CHECK (salary >= 0)
);
```

## DEFAULT Constraint

A default value can be automatically inserted when a value is not provided.

Example:

```sql
CREATE TABLE company.employees (
    employee_id SERIAL PRIMARY KEY,
    employee_name VARCHAR(100),
    active BOOLEAN DEFAULT TRUE
);
```

## Adding a Constraint

A constraint can also be added after creating a table.

Example:

```sql
ALTER TABLE company.employees
ADD CONSTRAINT salary_check
CHECK (salary >= 0);
```

## Altering a Table

Add a column:

```sql
ALTER TABLE company.employees
ADD COLUMN joining_date DATE;
```

Change a column data type:

```sql
ALTER TABLE company.employees
ALTER COLUMN employee_name TYPE VARCHAR(150);
```

Rename a column:

```sql
ALTER TABLE company.employees
RENAME COLUMN employee_name TO full_name;
```

Rename a table:

```sql
ALTER TABLE company.employees
RENAME TO staff;
```

Drop a column:

```sql
ALTER TABLE company.staff
DROP COLUMN joining_date;
```

Table changes should be planned carefully because they can affect applications and existing data.

## Inserting Data

Insert a department:

```sql
INSERT INTO company.departments
(department_name)
VALUES
('Database');
```

Insert an employee:

```sql
INSERT INTO company.employees
(employee_name, department_id, salary)
VALUES
('Kavya', 1, 45000);
```

## Updating Data

Update an employee salary:

```sql
UPDATE company.employees
SET salary = 50000
WHERE employee_id = 1;
```

Always use an appropriate `WHERE` condition when updating specific records.

## Deleting Data

Delete a specific employee:

```sql
DELETE FROM company.employees
WHERE employee_id = 1;
```

Deleting data should be performed carefully because the operation can permanently remove records unless they are restored from a backup or transaction is rolled back.

## Truncate

`TRUNCATE` removes all rows from a table quickly.

Example:

```sql
TRUNCATE TABLE company.employees;
```

It is different from `DELETE` because it is designed to remove all rows from the table.

A DBA should understand the impact before using `TRUNCATE`, especially on production tables.

## PostgreSQL Sequences

Sequences generate numeric values, commonly for primary key columns.

List sequences:

```text
\ds
```

Create a sequence:

```sql
CREATE SEQUENCE company.employee_id_seq;
```

Get the next sequence value:

```sql
SELECT nextval('company.employee_id_seq');
```

Check the current sequence value:

```sql
SELECT currval('company.employee_id_seq');
```

## Identity Columns

PostgreSQL also supports identity columns for automatically generated values.

Example:

```sql
CREATE TABLE company.projects (
    project_id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    project_name VARCHAR(100) NOT NULL
);
```

Identity columns are a modern alternative to using `SERIAL` for automatically generated values.

## Views

A view is a stored query that can be used like a table.

Create a view:

```sql
CREATE VIEW company.employee_summary AS
SELECT employee_id,
       employee_name,
       salary
FROM company.employees;
```

Query the view:

```sql
SELECT *
FROM company.employee_summary;
```

List views:

```text
\dv company.*
```

## Materialized Views

A materialized view stores the result of a query physically.

Create one:

```sql
CREATE MATERIALIZED VIEW company.employee_summary_mv AS
SELECT department_id,
       COUNT(*) AS employee_count
FROM company.employees
GROUP BY department_id;
```

Refresh it:

```sql
REFRESH MATERIALIZED VIEW company.employee_summary_mv;
```

Materialized views can be useful when a query is expensive and the result does not need to be generated every time.

## Table Size

Check the size of a table:

```sql
SELECT pg_size_pretty(
    pg_total_relation_size('company.employees')
);
```

Check table size including indexes:

```sql
SELECT pg_size_pretty(
    pg_total_relation_size('company.employees')
);
```

Check the size of the table data only:

```sql
SELECT pg_size_pretty(
    pg_relation_size('company.employees')
);
```

## Finding Large Tables

A DBA may need to identify tables that consume significant storage.

Example:

```sql
SELECT
    schemaname,
    relname AS table_name,
    pg_size_pretty(
        pg_total_relation_size(relid)
    ) AS total_size
FROM pg_catalog.pg_statio_user_tables
ORDER BY pg_total_relation_size(relid) DESC;
```

This can help with storage monitoring and database maintenance.

## Table Ownership

Check table ownership:

```sql
SELECT schemaname,
       tablename,
       tableowner
FROM pg_tables
WHERE schemaname = 'company';
```

Change table ownership:

```sql
ALTER TABLE company.employees
OWNER TO postgres;
```

Ownership is important because object owners have significant control over their database objects.

## Dropping Database Objects

Drop a view:

```sql
DROP VIEW company.employee_summary;
```

Drop a table:

```sql
DROP TABLE company.employees;
```

Drop a schema:

```sql
DROP SCHEMA company;
```

Using `CASCADE` can also remove dependent objects:

```sql
DROP SCHEMA company CASCADE;
```

`CASCADE` should be used carefully because it can remove dependent objects.

## Practical Database Management Example

Create a database:

```sql
CREATE DATABASE company_db;
```

Connect to it:

```text
\c company_db
```

Create a schema:

```sql
CREATE SCHEMA company;
```

Create a departments table:

```sql
CREATE TABLE company.departments (
    department_id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL UNIQUE
);
```

Create an employees table:

```sql
CREATE TABLE company.employees (
    employee_id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE,
    department_id INTEGER,
    salary NUMERIC(10,2) CHECK (salary >= 0),
    joining_date DATE,
    CONSTRAINT fk_department
        FOREIGN KEY (department_id)
        REFERENCES company.departments(department_id)
);
```

Insert departments:

```sql
INSERT INTO company.departments
(department_name)
VALUES
('Database'),
('IT'),
('Support');
```

Insert employees:

```sql
INSERT INTO company.employees
(employee_name, email, department_id, salary, joining_date)
VALUES
('Kavya', 'kavya@example.com', 1, 45000, '2026-01-10'),
('Anitha', 'anitha@example.com', 2, 50000, '2026-02-15'),
('Rahul', 'rahul@example.com', 3, 40000, '2026-03-20');
```

Check the data:

```sql
SELECT *
FROM company.employees;
```

## Basic DBA Checks

List databases:

```text
\l
```

List schemas:

```text
\dn
```

List tables:

```text
\dt company.*
```

Describe a table:

```text
\d company.employees
```

List sequences:

```text
\ds
```

List views:

```text
\dv company.*
```

Check database size:

```sql
SELECT pg_size_pretty(pg_database_size(current_database()));
```

Check table size:

```sql
SELECT pg_size_pretty(
    pg_total_relation_size('company.employees')
);
```

## Common Issues

### Table does not exist

Check the current database:

```sql
SELECT current_database();
```

Check the schema:

```text
\dn
```

List tables:

```text
\dt *.*
```

Use the full schema-qualified table name:

```sql
SELECT *
FROM company.employees;
```

### Permission denied

Check the table privileges:

```text
\dp company.employees
```

The user may need appropriate privileges such as:

```sql
GRANT SELECT
ON company.employees
TO analyst;
```

### Duplicate key error

A duplicate primary key or unique value can cause an error.

Check the table constraints:

```text
\d company.employees
```

Review the data before inserting or updating records.

## Hands-on Practice

The following tasks are part of my PostgreSQL DBA practice:

* Create and manage databases
* Create schemas
* Create and modify tables
* Work with PostgreSQL data types
* Create primary keys
* Create foreign keys
* Use NOT NULL, UNIQUE, CHECK, and DEFAULT constraints
* Insert, update, and delete records
* Understand DELETE and TRUNCATE
* Work with sequences and identity columns
* Create and query views
* Check table and database sizes
* Manage object ownership
* Practice basic database troubleshooting

## Commands Practiced

| Task                | Command                          |
| ------------------- | -------------------------------- |
| List databases      | \l                               |
| Connect to database | \c database_name                 |
| List schemas        | \dn                              |
| List tables         |  \dt                             |
| Describe table      |  \d table_name                   |
| List sequences      |  \ds                             |
| List views          |  \dv                             |
| Create database     |  CREATE DATABASE database_name;  |
| Create schema       |  CREATE SCHEMA schema_name;      |
| Create table        |  CREATE TABLE table_name (...);  |
| Alter table         |  ALTER TABLE table_name ...;     |
| Drop table          |  DROP TABLE table_name;          |
| Check database size |  pg_database_size()              |
| Check table size    |  pg_total_relation_size()        |

## Learning Progress

* [x] Database creation and management
* [x] Schema management
* [x] Table creation and modification
* [x] PostgreSQL data types
* [x] Primary and foreign keys
* [x] Table constraints
* [x] Basic data manipulation
* [x] Sequences and identity columns
* [x] Views
* [x] Database and table size checks
* [ ] Partitioning
* [ ] Advanced indexing
* [ ] Advanced database object management

## Next Steps

My next focus area is SQL queries and transaction management. I will practice joins, subqueries, aggregate functions, transactions, isolation levels, locks, and query handling in PostgreSQL.
