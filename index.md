---
layout: index
---

# Metrics matcher

## About
Metrics matcher is a data matching tool.

![Screenshot](/assets/screenshot.png)

TODO more description here

## Releases

These versions of the application were released and available for downloading:

- **v1.0.1** (2019-03-17) [metrics-matcher-1.0.1.zip](/releases/metrics-matcher-1.0.1.zip)

## User interface
- TODO screenshot
- TODO menu
- TODO link to naming convention
- TODO table, colors, status time, columns description
- TODO status bar

## Configuration

### Data sources

Data source is a set of connection parameters to your database.
You can define multiple data sources, but only one at a time can be selected as an active in the application.

In theory, the application supports any databases that have JDBC interface.
Like: PostgreSQL, MySQL, MariaDB, Oracle, H2, etc.
You just need to provide the appropriate driver (see [Drivers](#drivers) section).

Data sources are defined in the `configs/datasources.json` file.
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
    "timeout": 300
  }
]
```

Optional connection timeout parameter can be specified in seconds (300 seconds by default).


### Drivers

Application supports all databases having JDBC interface.

Driver is a jar file with JDBC interface implementation to specific database.

- [H2](http://www.h2database.com/html/download.html) 
- [MySQL](https://dev.mysql.com/downloads/connector/j)
- [Oracle](https://www.oracle.com/technetwork/database/application-development/jdbc/downloads/index.html)
- [PostgreSQL](https://jdbc.postgresql.org/download.html)

Drivers samples:

| Database   | JDBC JAR                        | JDBC URL               |
|------------|---------------------------------|------------------------|
| H2         | h2-1.4.199.jar                  | jdbc:h2:/data/test     |
| MySQL      | mysql-connector-java-8.0.15.jar | TODO                   |
| Oracle     | ojdbc8.jar                      | TODO                   |
| PostgreSQL | postgresql-42.2.5.jar           | TODO                   |
  
Drivers should be placed into `drivers` directory to be available in the app.

### Queries

- TODO queries directory, queries file
- TODO name, naming convention
- TODO valid SQL
- TODO vaiables

### Metrics profiles

- TODO about metrics
- TODO JSON sample
- TODO naming convention
- TODO params, variables substitution


## Naming convention

- TODO datasource name
- TODO query file name, id (without sql), query title
- TODO variables mapping
- TODO screenshot, highlight menu, statusbar 


## TODO backlog
### Data sources

### Query files

Application makes SQL query in order to select metric from the specified datasource.
List of
todo variable substitution
todo reference to naming convention

### Profiles
todo order is important
todo label
todo ref to naming convention
todo params

## UI
### Menu
- File
  - Save report
  - Exit
- Datasource
  - Submenu of radio buttons with a list of data sources. Only one profile can checked at a time.
- Profile
    - Submenu of radio buttons with a list of profiles. Multiple profiles can checked at a time.
- Run
    - Run
    - Stop
    - Stop on error checkbox
    - Stop on mismatch checkbox
    
Menu is blocked at the time of execution of the matcher.

### Report
Result of 
Report is a table with

Table
- Metric name
- Expected value
- Actual value
- Execution time (in milliseconds)

### Status bar

todo metrics order
todo highlight mismatch
todo highlight error
todo summary
todo stop button
todo status bar with start-stop buttons
todo block menu at execution time
todo naming convention

todo inform about readonly sessions
todo status bar connection status
todo status bar selected datasource  
todo status bar selected profile
