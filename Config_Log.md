# Configuring logging #

You can have Kitto log information about what happens during the application's execution by enabling logging. Use the `Log` node in the config file to do that. Example:

```
Log:
  # Create a new log file every time the service starts.
  FileName: %HOME_PATH%Logs\%PROCESS_ID%.log
  # 1 = Low, 5 = Debug. CAUTION: Level 5 generates huge
  # amounts of data.
  Level: 1
```


---

Currently, logging is only enabled when the Kitto application runs as a service, and not in GUI mode.

---


# Custom logging #

If you need to log information from your server-side code, include the unit `EF.Logger` and call the methods of `TEFLogger.Instance`. Kitto takes care of thread synchronization.

Also don't forget to enable `EF.Logger.TextFile` in your UseKitto.pas unit (or otherwise reference the unit in your project). You can also use `EF.Logger.CodeSite` if you have [CodeSite](http://www.raize.com/devtools/codesite/), or you can define a custom logging endpoint. You can combine endpoints (meaning that you can log to file and CodeSite at the same time).