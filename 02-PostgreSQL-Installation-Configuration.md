# PostgreSQL Installation and Configuration

This section covers the basic PostgreSQL installation and configuration tasks I am learning and practicing for PostgreSQL database administration.

## Installation Overview

PostgreSQL can be installed on Linux, Windows, and other supported operating systems.

For DBA work, I am focusing mainly on PostgreSQL administration in a Linux environment because many PostgreSQL servers are managed from Linux systems.

The main steps are:

1. Install PostgreSQL
2. Start the PostgreSQL service
3. Check the PostgreSQL version
4. Check the service status
5. Connect using `psql`
6. Check the server port
7. Identify PostgreSQL configuration files
8. Make basic configuration changes
9. Restart or reload the PostgreSQL service when required

## PostgreSQL Packages

On a Debian or Ubuntu-based Linux system, PostgreSQL can be installed using:

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

After installation, check the PostgreSQL version:

```bash
psql --version
```

Example:

```text
psql (PostgreSQL 17.x)
```

The exact version depends on the PostgreSQL package installed on the system.

## PostgreSQL Service

PostgreSQL normally runs as a system service on Linux.

Check the service status:

```bash
sudo systemctl status postgresql
```

Start PostgreSQL:

```bash
sudo systemctl start postgresql
```

Stop PostgreSQL:

```bash
sudo systemctl stop postgresql
```

Restart PostgreSQL:

```bash
sudo systemctl restart postgresql
```

Reload PostgreSQL configuration:

```bash
sudo systemctl reload postgresql
```

Enable PostgreSQL to start automatically after a system reboot:

```bash
sudo systemctl enable postgresql
```

## Checking the PostgreSQL Process

To check whether PostgreSQL processes are running:

```bash
ps aux | grep postgres
```

Another useful command is:

```bash
pg_isready
```

A successful response normally indicates that PostgreSQL is accepting connections.

## Connecting to PostgreSQL

On Linux, the PostgreSQL operating system user is commonly named `postgres`.

Switch to the postgres user:

```bash
sudo -u postgres -i
```

Open the PostgreSQL command-line interface:

```bash
psql
```

A specific database can also be accessed using:

```bash
psql -U postgres -d postgres
```

Exit the PostgreSQL shell:

```text
\q
```

## Checking Server Information

Check the PostgreSQL version:

```sql
SELECT version();
```

Check the current database:

```sql
SELECT current_database();
```

Check the current user:

```sql
SELECT current_user;
```

Check the server start time:

```sql
SELECT pg_postmaster_start_time();
```

## PostgreSQL Port

The default PostgreSQL TCP port is:

```text
5432
```

To check the configured port from PostgreSQL:

```sql
SHOW port;
```

To check whether PostgreSQL is listening on the port from Linux:

```bash
ss -ltnp | grep 5432
```

The actual port can be changed in the PostgreSQL configuration.

## PostgreSQL Configuration Files

PostgreSQL uses configuration files to control server behavior.

The three important configuration files are:

### postgresql.conf

This file contains PostgreSQL server configuration settings such as:

* Port
* Memory settings
* Connection settings
* Logging settings
* WAL settings
* Checkpoint settings
* Autovacuum settings

### pg_hba.conf

This file controls client authentication and access to PostgreSQL databases.

It can define:

* Which databases can be accessed
* Which users can connect
* Which client addresses are allowed
* Which authentication method is used

### pg_ident.conf

This file can be used for user name mapping when certain authentication methods require it.

## Finding Configuration File Locations

PostgreSQL can show the location of its configuration files.

Check `postgresql.conf`:

```sql
SHOW config_file;
```

Check `pg_hba.conf`:

```sql
SHOW hba_file;
```

Check the data directory:

```sql
SHOW data_directory;
```

These commands are useful when troubleshooting configuration-related issues.

## Viewing Configuration Parameters

To display a specific parameter:

```sql
SHOW shared_buffers;
```

Check the maximum number of connections:

```sql
SHOW max_connections;
```

Check the listening address:

```sql
SHOW listen_addresses;
```

Check the port:

```sql
SHOW port;
```

To view all configuration parameters:

```sql
SELECT name, setting
FROM pg_settings
ORDER BY name;
```

## Basic Configuration Example

For example, the PostgreSQL port can be checked with:

```sql
SHOW port;
```

If the port needs to be changed, the `port` parameter can be updated in `postgresql.conf`.

Example:

```text
port = 5433
```

After changing a setting, PostgreSQL may require a reload or restart depending on the parameter.

For a restart:

```bash
sudo systemctl restart postgresql
```

After restarting, verify the port:

```sql
SHOW port;
```

Configuration changes should always be planned carefully on production systems because an incorrect setting can affect database availability.

## Reload vs Restart

A configuration reload tells PostgreSQL to reread configuration files without completely stopping the server.

```bash
sudo systemctl reload postgresql
```

A restart stops and starts the PostgreSQL server:

```bash
sudo systemctl restart postgresql
```

A restart may be required for configuration parameters that cannot be changed by a reload.

## Checking Configuration Changes

PostgreSQL provides information about configuration parameters through the `pg_settings` view.

For example:

```sql
SELECT name, setting, unit, context
FROM pg_settings
WHERE name IN ('max_connections', 'shared_buffers', 'work_mem', 'maintenance_work_mem');
```

This helps a DBA understand the current configuration and whether a parameter requires a restart.

## Basic Memory Parameters

Some important PostgreSQL memory settings are:

### shared_buffers

`shared_buffers` controls the amount of memory PostgreSQL uses for shared buffer caching.

Check the current value:

```sql
SHOW shared_buffers;
```

### work_mem

`work_mem` controls the amount of memory available for individual query operations such as sorting and hashing before PostgreSQL may use temporary files.

Check the current value:

```sql
SHOW work_mem;
```

### maintenance_work_mem

`maintenance_work_mem` controls memory available for maintenance operations such as `VACUUM`, `CREATE INDEX`, and some `ALTER TABLE` operations.

Check the current value:

```sql
SHOW maintenance_work_mem;
```

These parameters should be configured based on available system resources and workload. They should not be changed blindly.

## Logging Basics

Logging is important for monitoring and troubleshooting PostgreSQL.

Useful logging-related parameters include:

```sql
SHOW logging_collector;
```

```sql
SHOW log_directory;
```

```sql
SHOW log_filename;
```

Logging can help identify:

* Connection problems
* Authentication failures
* SQL errors
* Server errors
* Long-running queries
* Configuration problems

## Checking Database Connections

To view current PostgreSQL sessions:

```sql
SELECT pid,
       usename,
       datname,
       client_addr,
       state
FROM pg_stat_activity;
```

This is useful when checking active connections and troubleshooting database activity.

## Basic Installation Verification

After installing PostgreSQL, I can verify the installation using the following checks:

```bash
psql --version
```

```bash
sudo systemctl status postgresql
```

```bash
pg_isready
```

Then connect to PostgreSQL:

```bash
sudo -u postgres psql
```

And run:

```sql
SELECT version();
```

If these checks work correctly, the PostgreSQL server is installed and accepting connections.

## Common Installation and Configuration Issues

### PostgreSQL service is not running

Check the service:

```bash
sudo systemctl status postgresql
```

Start it:

```bash
sudo systemctl start postgresql
```

### Cannot connect to PostgreSQL

Check whether PostgreSQL is accepting connections:

```bash
pg_isready
```

Check the service:

```bash
sudo systemctl status postgresql
```

Check the configured port:

```sql
SHOW port;
```

### Authentication failure

Check the `pg_hba.conf` file:

```sql
SHOW hba_file;
```

Review the authentication rules and confirm that the required user, database, client address, and authentication method are configured correctly.

### Port connection problem

Check the PostgreSQL port:

```sql
SHOW port;
```

Check whether the server is listening:

```bash
ss -ltnp | grep 5432
```

Also check firewall rules and the `listen_addresses` configuration when remote connections are required.

## Hands-on Practice

The following tasks are part of my PostgreSQL DBA practice:

* Install PostgreSQL on Linux
* Check the installed PostgreSQL version
* Start and stop the PostgreSQL service
* Check PostgreSQL service status
* Connect using `psql`
* Check the PostgreSQL port
* Find PostgreSQL configuration files
* Read basic configuration parameters
* Understand `postgresql.conf`
* Understand `pg_hba.conf`
* Check active database connections
* Practice reload and restart operations
* Troubleshoot basic connection problems

## Commands Practiced

| Task                      | Command                             |
| ------------------------- | ----------------------------------- |
| Check version             | psql --version                      |
| Check service             | sudo systemctl status postgresql    |
| Start service             | sudo systemctl start postgresql     |
| Stop service              | sudo systemctl stop postgresql      |
| Restart service           | sudo systemctl restart postgresql   |
| Reload configuration      | sudo systemctl reload postgresql    |
| Check server availability | pg_isready                          |
| Connect as postgres       | sudo -u postgres psql               |
| Check PostgreSQL port     | SHOW port;                          |
| Find config file          | SHOW config_file;                   |
| Find HBA file             | SHOW hba_file;                      |
| Find data directory       | SHOW data_directory;                |
| Check active sessions     | SELECT * FROM pg_stat_activity;     |

## Learning Progress

* [x] PostgreSQL installation basics
* [x] PostgreSQL service management
* [x] psql connection
* [x] PostgreSQL version and server checks
* [x] Basic configuration files
* [x] Basic configuration parameters
* [ ] Advanced PostgreSQL configuration
* [ ] PostgreSQL authentication configuration
* [ ] Remote client connections
* [ ] PostgreSQL logging configuration
* [ ] Memory and resource tuning

## Next Steps

My next focus area is PostgreSQL users, roles, authentication, and permissions. 
I will practice creating roles, managing passwords, granting privileges, and controlling access to databases and schemas.
