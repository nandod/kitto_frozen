# Databases #

A single Kitto application can work on multiple databases, of different kinds. The `Databases` node in `Config.yaml` contains one or more subnodes, each containing the connection params to a database. The subnode called `Main` is the one used by default for database access.

## Adapters ##

An adapter is a wrapper that allows Kitto to connect to a particular database (or a family of databases) through a particular data access library. Currently Kitto has three adapters:
  * DBX to use Delphi's DBExpress data access layer.
  * ADO to use Microsoft's ADO.
  * FD to use Delphi's FireDac data access layer.

More adapters can be developed and plugged in as needed.

The adapter is specified by name as the value of each database's subnode.

## Database types ##

Each adapter detects the type of database it is using and uses this information to affect the behaviour of Kitto, for example to generate slightly different SQL statements depending on the particular dialect the database understands.

## Connection subnode ##

You specify each database's settings inside a `Connection` subnode whose structure depends on the adapter. For example, the ADO adapter accepts a single `ConnectionString` subnode with all params in the typical ADO format:

Example:
```
Databases:
  Main: ADO
    Connection:
      ConnectionString: >
        Provider=SQLOLEDB.1;User ID=sa;Password=xxx;
        Initial Catalog=MyDatabase;Data Source=%COMPUTERNAME%
```

DBX allows for specifying params as direct subnodes:

```
Databases:
  Main: DBX
    Connection:
      DriverName: DevartSQLServer
      HostName: %COMPUTERNAME%
      DataBase: MyDatabase
      User_Name: sa
      Password: xxx
      GetDriverFunc: getSQLDriverSQLServer
      LibraryName: dbexpsda40.dll
      VendorLib: sqloledb.dll
```

FD allows for specifying params as direct subnodes, specific to DriverID:

```
Databases:
  FDMSSQL: FD
    Connection:
      DriverID: MSSQL
      Server: %COMPUTERNAME%\MSSQL2K8R2
      ApplicationName: %APPTITLE%
      Database: HelloKitto
      OSAuthent: Yes
      Isolation: RepeatableRead
    
  FDFb: FD
    Connection:
      DriverID: FB
      # Alias of database defined in Aliases.conf
      Database: HELLOKITTO
      User_Name: SYSDBA
      Password: masterkey
      Server: localhost
      CharacterSet: UTF8
      Protocol: TCPIP
```

### AfterOpenCommandText ###

If specified, the value of `Connection/!AfterOpenCommandText` is executed as a SQL command each time a connection is made. Useful if special connection setup is needed. The command text may contain [macros](Macros.md).