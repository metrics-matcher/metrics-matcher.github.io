---
layout: index
---

# Metrics matcher

Metrics matcher is a data matching tool.
![Screenshot](/assets/screenshot.png)

TODO more description here

## Releases

These versions of the application were released and can be downloaded:
[-] 2019-03-7 [v1.0.1](/releases/metrics-matcher-1.0.1.zip)

## User interface
- TODO screenshot
- TODO menu
- TODO link to naming convention
- TODO table, colors, status time, columns description
- TODO status bar

## Configuration

### Data sources

- TODO what is datasource
- TODO list of datasources
- TODO datasource sample JSON
- TODO JDBC ULR samples
- TODO "optional timeout"


### Drivers

Application supports all databases having JDBC interface.

- TODO what is driver, jdbc jar
- TODO drivers location
- TODO how driver related to datasoure URL 
- TODO mysql, postgres, h2, oracle, 


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
Data source (or multiple data sources) can be defined in the `datasources.json` file.

Sample file:
```json

```

Default connection parameters are:
- `type`: "JDBC"
- `timeout`: 300 seconds
- `driver`: ?

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
