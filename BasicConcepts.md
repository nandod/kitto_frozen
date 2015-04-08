# Introduction #

This document explains some basic concepts common to all Kitto applications. Start here if you want to explore the examples in some depth and start creating your own applications.

# Structure of project files #

A Kitto application is made up of

  * a Delphi project;
  * a set of metadata files;
  * a set of resources.

Resources are just additional files that your application will need to send to the web browser, such as images and javascript files, while the Delphi project contains any server-side business logic you might need to implement. Most of a Kitto application, however, lies in the metadata files.

## Yaml 101 ##

All metadata files are in [yaml](http://www.yaml.org) format. Yaml is a human-readable markup language which is easier to read and less noisy and error-prone to write than XML or JSON, meaning that you can just open and edit the files in your preferred text editor. Just remember:

  * A yaml file is a tree of nodes, each on its own text line with a name followed by a colon followed by a value.
  * Valid data types for values include numbers, strings, dates but also space-separated lists and special values or macros. Multi-line values are supported as well.
  * Indentation in yaml is paramount to define structure. Kitto, by convention, uses two spaces for each level of indentation.
  * Don't use tabs, only spaces. Kitto refuses to parse files containing tabs.

Example:
```
# This is a comment.

# Address is a value-less node.
Address:
  # StreetName is a string.
  StreetName: Rue Bellevue
  # Number is an Integer values.
  Number: 102
```

As you can see in the example above, # characters denote comments, while blank lines are ignored. StreetName and Number are child nodes of Address, which is a note with no value, just the children. Each value's data type is inferred by parsing it, and node names are case insensitive.


---

In the documentation and, as you'll see, in Delphi code, you can access nodes by path. A path is a string with hierarchy represented by slashes. In the example above, the `Number` node would be referred to as `Address/Number`.

---


As mentioned above, you can break up long string values into multiple lines, by using a special introducer character as the node value and putting the text, indented, in the following lines:

```
Verse: |
  I wandered lonely as a cloud:
  That floats on high o'er vales and hills
```

The multi-lihe values continue until the text is outdented again. You can also use the > introducer. the difference between the two is as follows:

  * The pipe (`|`) preserves formatting (line breaks and spaces).
  * The `>` symbol does not preserve formatting, putting everything together.

## Macros ##

Kitto supports macros, in the form of strings surrounded by %s, just about everywhere, and particularly in yaml values. See [this page](Macros.md) for a list of available macros and instructions on how to add your own.

## Metadata directory structure ##

The Metadata directory contains one or more config files (by default, one called Config.yaml) and some subdirectories with more yaml files. Currently you have:

  * Models containing one yaml file for each model, which is basically the representation of a database table (although it can be more abstract than that).
  * Views containing one yaml file for each view, which is a piece of user interface, often linked to an underlying model or family of models.


---

Whenever you change a metadata file while the application is running, the change is visible to all new web sessions but not currently running session. You can restart the Kitto server to close all sessions and force the change to be seen by everyone. This is done particularly often during development, as it's a button click in Kitto's control panel.

You only need to restart the Kitto server when you recompile it, that is if you change any Delphi code inside it.

---


Now is a good time to mention that Kitto favours convention over configuration (CoC) whenever possible, meaning that each setting can be done at different levels and if you don't specify a value at one level, Kitto tries to synthesize a sensible default by inspecting higher levels. For example, if you don't specify a DisplayLabel for a field in a View, the DisplayLabel for the underlying model field is used, and if the specification is missing there as well, a label is created by beautifying the field name.

This means that in Kitto you generally only specify variations from the common standard way, and that if you use sensible conventions you are going to save time and bloat. In a way, Kitto tries to apply the DRY (Don't Repeat Yourself) principle, by never writing the same information in two places.

# The config file #

It is called Config.yaml by default, and it is the one mandatory file in a Kitto application. Think of it as the main entry point. Normally the config file mentions a HomeView from which the entire application is served. The config file contains settings regarding database connections, language and localization, user authentication, access control and so on.

List of first-level config nodes (click on node names for details):

| **Name** | **Type** | **Description** |
|:---------|:---------|:----------------|
| AppTitle | String | Application title |
| [Databases](Config_Databases.md) | Subnode | Database connections |
| [Auth](Config_Auth.md) | Subnode | User authentication settings |
| [AccessControl](Config_AccessControl.md) | Subnode | Access control (user privileges) settings |
| [HomeView](Views.md) | String/Subnode | Specifies the view to display at application startup (or after login if user authentication is enabled) |
| [Ext](Config_Ext.md) | Subnode | ExtPascal- and ExtJS-related settings |
| [Log](Config_Log.md) | Subnode | Logging-related settings |
| [FastCGI](Config_FastCGI.md) | Subnode | Settings such as the TCP port to use and the session timeout |
| LanguageId | String | Default user interface language (such as en) |
| Charset | String | Default charset (normally utf-8) |
| JavaScriptLibraries | List | Optional libreries to include |

A Config.yaml example:
```
AppTitle: My First Kitto Application

Databases:
  Main: ADO
    Connection:
      ConnectionString: >
        Provider=SQLOLEDB.1;User ID=sa;Password=xxx;
        Initial Catalog=MyDatabase;Data Source=%COMPUTERNAME%

  Other: DBX
    Connection:
      DriverName: DevartSQLServer
      HostName: %COMPUTERNAME%
      DataBase: MyDatabase
      User_Name: sa
      Password: xxx
      GetDriverFunc: getSQLDriverSQLServer
      LibraryName: dbexpsda40.dll
      VendorLib: sqloledb.dll
  
Auth: DB
  IsClearPassword: True
  IsPassepartoutEnabled: True
  PassepartoutPassword: hfjry%%tebd!qywha$£nò
  Defaults:
    UserName: administrator
    Password: hfjry%%tebd!qywha$£nò

AccessControl: Null
    
# Home is also the default.
HomeView: Home

Ext:
  Theme: gray
  URL: /ext

# Default: en
LanguageId: it
Charset: utf-8
```

Next step: Understanding [Views](Views.md).