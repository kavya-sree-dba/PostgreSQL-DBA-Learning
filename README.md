# PostgreSQL DBA Learning

Welcome to my PostgreSQL DBA learning and hands-on practice repository.

This repository documents my journey in **PostgreSQL Database Administration, SQL, Linux administration, backup and recovery, performance tuning, WAL, replication, monitoring, troubleshooting, and database security**.

The goal of this repository is to build practical PostgreSQL DBA skills through hands-on exercises, commands, SQL queries, troubleshooting scenarios, and projects.

## 🛠️ Technologies & Tools

* PostgreSQL 17
* SQL
* pgAdmin 4
* psql
* Linux
* Git & GitHub
* pg_dump
* pg_dumpall
* pg_restore
* pg_basebackup
* EXPLAIN
* EXPLAIN ANALYZE

## 📚 PostgreSQL DBA Topics

### 1. PostgreSQL Architecture

* PostgreSQL Architecture
* Backend Processes
* PostgreSQL Server Process
* Background Writer
* WAL Writer
* Checkpointer
* Autovacuum
* Shared Buffers
* work_mem
* maintenance_work_mem
* Connections and Processes

### 2. Installation, Configuration & Upgrades

* initdb
* pg_ctl
* systemd
* postgresql.conf
* pg_hba.conf
* pg_ident.conf
* Minor vs Major Upgrades
* pg_upgrade
* Logical Replication for Upgrades

### 3. Backup & Recovery

* Logical Backups
* pg_dump
* pg_dumpall
* Physical Backups
* pg_basebackup
* Point-in-Time Recovery (PITR)
* WAL Archiving
* Restore Strategies

### 4. Replication & High Availability

* Physical Streaming Replication
* Logical Replication
* Synchronous Replication
* Asynchronous Replication
* Read Replicas
* Failover
* Patroni
* repmgr

### 5. Performance Tuning & Optimization

* Query Planning
* EXPLAIN
* EXPLAIN ANALYZE
* B-tree Indexes
* GIN Indexes
* GiST Indexes
* BRIN Indexes
* VACUUM
* Autovacuum Tuning
* Memory Tuning
* I/O Tuning

### 6. Monitoring & Troubleshooting

* pg_stat_* Views
* Slow Query Identification
* Locks
* Blocking Sessions
* Deadlocks
* PostgreSQL Logs
* Log Analysis
* Dead Tuples
* Table and Index Bloat

### 7. Security & Access Control

* Users and Roles
* GRANT and REVOKE
* Authentication Methods
* md5
* SCRAM-SHA-256
* Certificate Authentication
* SSL/TLS
* Row-Level Security (RLS)

### 8. Maintenance & Housekeeping

* VACUUM
* VACUUM FULL
* ANALYZE
* REINDEX
* Autovacuum
* Table Bloat
* Index Bloat

### 9. Data Modeling & Storage Internals

* Tablespaces
* MVCC
* TOAST
* Partitioning
* Range Partitioning
* List Partitioning
* Hash Partitioning

### 10. DBA Tools & Automation

* pgAdmin 4
* psql
* pgBouncer
* Cron Jobs
* Shell Scripting
* pg_stat_statements
* pg_repack
* PgBadger

### 11. SQL

* DBMS
* RDBMS vs Non-RDBMS
* SQL Fundamentals
* DDL, DML, DQL, DCL and TCL
* ALTER
* DELETE vs TRUNCATE vs DROP
* Operators
* Aggregate Functions
* WHERE
* GROUP BY
* ORDER BY
* HAVING
* Constraints
* Primary Key
* Foreign Key
* UNIQUE
* NOT NULL
* CHECK
* DEFAULT
* Primary Key vs Foreign Key
* Primary Key vs UNIQUE Key
* Joins
* Subqueries
* CTE
* ACID Properties

## 🔍 SQL Query Processing

I will also document the basic stages involved in processing SQL statements:

```text
SQL Query
    ↓
Syntax Check
    ↓
Semantic Analysis
    ↓
Query Rewrite
    ↓
Query Planning / Optimization
    ↓
Query Execution
```

## 📂 Repository Structure

```text
PostgreSQL-DBA-Learning/
│
├── 01-PostgreSQL-Architecture/
├── 02-Installation-Configuration-Upgrades/
├── 03-Backup-Recovery/
├── 04-Replication-High-Availability/
├── 05-Performance-Tuning/
├── 06-Monitoring-Troubleshooting/
├── 07-Security/
├── 08-Maintenance/
├── 09-Storage-Internals/
├── 10-DBA-Tools-Automation/
├── 11-SQL/
└── 12-Projects/
```

## 🧪 Hands-on Practice

Each topic will contain:

* Concept explanation
* Important DBA commands
* SQL examples
* Practical exercises
* Expected results
* Troubleshooting scenarios
* Interview questions
* Lessons learned

## 🚀 Projects

Planned hands-on projects include:

1. Company Database Administration
2. PostgreSQL Backup & Recovery
3. PostgreSQL Performance Tuning
4. PostgreSQL Replication
5. PostgreSQL Monitoring & Troubleshooting

## 🎯 Goal

My goal is to develop strong practical skills in **PostgreSQL Database Administration, SQL, and Linux** and build hands-on experience through real-world practice projects.

This repository will be continuously updated as I learn and practice new PostgreSQL DBA concepts.

## 👩‍💻 Author

**Kavya Sree**

Aspiring PostgreSQL DBA
