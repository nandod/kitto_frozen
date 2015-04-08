### How to set config and metadata values from external files ###

When working in a team, it is customary that database names, user names and passwords and other data are different for each developer or development machine. Since these data is usually stores in an application's `Config.yaml` file, this prevents to keep the file under version control unless each developer has it own different copy, which is a burden to maintain.

You can do that through environment variables, which in Kitto you can embed into any metadata files just by surrounding them with `%` characters, or you can do in a more local way by moving the data to separate files.

### The `%FILE()%` macro ###

One of the uses of Kitto's `%FILE()%` macro (there can be many others) is to allow you to push personal data outside `Config.yaml` and into local files that are never kept under version control. Example:

```
Databases:
  Main: DBX
    Connection:
      DriverName: MSSQL
      HostName: %FILE(%HOME_PATH%.dbhostname.local)%
      DataBase: %FILE(%HOME_PATH%.dbname.local)%
      OS Authentication: True             
```

Just create local files named `.dbhostname.local` and `.dbname.local`, put the host/instance name in the former and the catalog name in the latter, and make sure those files are ignored by your version control system. When processing the `%FILE()%` macro, Kitto will expand the file name, load and expand the file contents (meaning that both name and contents may contain other macros).

Another example:

```
Auth: DB
  Defaults:
    UserName: %FILE(%HOME_PATH%.username.local)%
    Password: %FILE(%HOME_PATH%.password.local)%
```

For more details, see the [Macros](Macros.md) page.