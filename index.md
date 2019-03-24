---
layout: index
---

## About
Metrics matcher is a data testing tool.

It executes SQL queries against your data in a batch, 
compares expected and actual results and highlights mismatches and errors.

This is similar to running suites of test cases with assertions, but for databases.

This is how it looks on Windows:

![Metrics matcher screenshot](/images/screenshot.png)

Metrics matcher is a Java-based desktop application. It requires **Java 8** to be installed.

It is open and free, licensed under MIT. With sources hosted on the GitHub in the 
[metrics-matcher](https://github.com/metrics-matcher/metrics-matcher) repository.

In order to start using of the application your need to:

1. Download and unpack the latest version (see [Releases](#releases))
2. Configure database connections (see [Data sources](#data-sources))
3. Download JDBC driver for your database if need (see [Drivers](#drivers))
4. Write your SQL queries (see [Queries](#queries))
5. Link data sources, SQL queries and expected results (see [Metrics profiles](#metrics-profiles))
6. Run it
7. See how the actual result matches the expected.

## Releases

These versions of the application were released and available for downloading:

- **v1.0.2** (2019-03-24) 
[demo zip](/releases/metrics-matcher-1.0.2-demo.zip)
[mini zip](/releases/metrics-matcher-1.0.2-mini.zip)
[oracle zip](/releases/metrics-matcher-1.0.2-oracle.zip)
  - Separate app builds
  - Optimized app size

- **v1.0.1** (2019-03-17) [metrics-matcher-1.0.1.zip](/releases/metrics-matcher-1.0.1.zip)
  - Initial implementation
  
Since v1.0.2 build procedure produces different variants of the app.

Latest release contains these:

- **demo** - includes H2 driver, different types of SQL queries and profiles to demonstrate how it works
- **mini** - minimal without drivers and configs
- **oracle** - includes Oracle driver, data source template for Oracle database and dummy metrics profile   

Full list of downloadable files is available at 
[Releases](https://github.com/metrics-matcher/metrics-matcher.github.io/tree/master/releases)

## Configuration

### Data sources

Data source is a set of connection parameters to your database.
You can define multiple data sources, but only one at a time can be selected as an active in the application.

In theory, the application supports any databases that have JDBC interface.
Like: PostgreSQL, MySQL, MariaDB, Oracle, H2, etc.
You just need to provide the appropriate driver (see [Drivers](#drivers) section).

Data sources are defined in the ***configs/datasources.json*** file.

Sample file:

```json
[
  {
    "name": "My H2",
    "url": "jdbc:h2:mem:dev",
    "username": "me",
    "password": "passwd"
  },
  {
    "name": "My Oracle",
    "url": "jdbc:oracle:thin:@//myhost:1521/orcl",
    "username": "me",
    "password": "passwd",
    "timeout": 60
  }
]
```

Optional connection timeout parameter can be specified in seconds (300 seconds by default).

Application establishes connection in **READ-ONLY** mode.
So, the app does not allow queries to modify data in the database.

### Drivers

Application interacts with databases via JDBC interface using JDBC driver.

Typically, JDBC driver is a Java library packed into the ***.jar*** file
and supplied by the database vendor.

You may find appropriate drivers at [Maven repository](https://mvnrepository.com), 
like these:

- [H2](https://mvnrepository.com/artifact/com.h2database/h2/1.4.199) 
- [Microsoft SQL Server](https://mvnrepository.com/artifact/com.microsoft.sqlserver/mssql-jdbc/7.2.1.jre8)
- [MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.15)
- [Oracle](https://www.oracle.com/technetwork/database/application-development/jdbc/downloads/index.html)
- [PostgreSQL](https://mvnrepository.com/artifact/org.postgresql/postgresql/42.2.5)

Binary is available at the driver's page in the **Files** section.

Here is a list of well-known JDBC drivers:

| Database | JDBC JAR | JDBC URL Template |
|----------------------|---------------------------------|---------------------------------------------|
| H2 | h2-1.4.199.jar | See [Database URLs](http://www.h2database.com/html/features.html) |
| Microsoft SQL Server | mssql-jdbc-7.2.1.jre8.jar | jdbc:sqlserver://HOST;DatabaseName=DATABASE |
| MySQL | mysql-connector-java-8.0.15.jar | jdbc:mysql://HOST/DATABASE |
| Oracle | ojdbc8.jar | jdbc:oracle:thin:@HOST:1521:SID |
| PostgreSQL | postgresql-42.2.5.jar | jdbc:postgresql://HOST/DATABASE |

Drivers should be placed into ***drivers*** directory to be available in the app.
You can put the drivers you need.

The application most likely should work with any database.

### Queries

Application works with small portions of data extracted from the database using SQL queries.

**Each query should return metrics - a single value in a single row**.

In most cases it is a counter or result of aggregation function.

Empty result or result set of multiple rows will be treated  as an error.

SQL can be simple like:
```sql
-- Count of users
SELECT COUNT(1) FROM users
```

You can also use parameters:
```sql
-- Max age in the group ${groupName}
SELECT MAX(user_age)
FROM users
WHERE user_group = '${groupName}'
```

Query may have optional comment (started with `--`) on the first line.
In the runtime such parameters will be substituted with values specified in the Metrics Profiles.

Files with SQL queries should be placed into the ***queries*** directory.
One query per file.


### Metrics profiles

Metrics profile is a way to link queries with expected results and organize them into groups. 

Metrics profiles can be defined in ***metrics-profiles.json***, like this:

```json
[
  {
    "name": "Portal users",
    "metrics": {
      "total-count-of-users": 125,
      "average-user-age": 35.5,
      "last-registration-date": "2019-02-28",
      "last-event-timestamp": "2019-03-23 15:25:59",
      "most-popular-user": "Neo"
    }
  },
  {
    "name": "Diablo gamers",
    "metrics": {
      "count-of-users": 55
    },
    "params": {
      "groupName": "diablo"
    }
  }
]
```

Here `metrics` is a map of query ids and expected results.

Imagine you have a file ***queries/total-count-of-users.sql***.

Then `total-count-of-users` (without `.sql`) is  a **Query Id**.

And you expect that this query should return 123 users.

Queries can be parameterized.

For example, if out have a query like:

```sql 
SELECT COUNT(1) FROM users WHERE user_group = '${groupName}'
```

Then you can use the same query in different profiles:

```json
[
  {
    "name": "Diablo gamers",
    "metrics": {
      "count-of-users": 55
    },
    "params": {
      "groupName": "diablo"
    }
  },
  {
    "name": "Minecraft gamers",
    "metrics": {
      "count-of-users": 10
    },
    "params": {
      "groupName": "minecraft"
    }
  }
]
```

For the first profile query will be formed as:
```sql
SELECT COUNT(1) FROM users WHERE user_group = 'diablo'
```
and for the second as:
```sql
SELECT COUNT(1) FROM users WHERE user_group = 'minecraft'
```

## Naming convention

Application follows pretty simple naming convention between file names, metrics and...
 
- TODO datasource name
- TODO query file name, id (without sql), query title
- TODO variables mapping
- TODO screenshot, highlight menu, statusbar

## SQL tips and tricks

TODO
