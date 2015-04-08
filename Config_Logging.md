# Logging #

You can enable server side logging in Kitto, which will allow you to see trace data useful for debugging or log your own data from your server side code (such as custom controllers or rules, for example).

## Logging to file ##

In order to enable logging to file, you write something like this in your Config.yaml:

```
Log:
  # 1 = Minimal, 5 = Debug.
  Level: 5
  TextFile:
    # Set this to false to disable this logger without
    # deleting its configuration.
    IsEnabled: True
    FileName: %APP_PATH%log.txt
```

The code above uses the `TextFile` logger, which you also need to enable  in your application by referencing the unit `EF.Logger.TextFile` (usually in your `UseKitto.pas` file). If you fail to include the relevant unit, your logging configuration will be ineffective.

## Logging to CodeSite ##

If you have CodeSite (any edition), you can include `EF.Logger.CodeSite` and have your log strings sent to the CodeSite viewer. Multiple loggers can be active at once: just add the relevant sections under the `Log` node in `Config.yaml`. The section name for the CodeSite logger is called `CodeSite` and must contain at least the `IsEnabled: True` line.

## Custom loggers ##

You can add your own custom loggers; just have a look at the predefined ones and use them as examples and starting points.

Basically you need to create a _log endpoint_. Just create a new class inherited from `TEFLogEndpoint` and override the `DoLog` method (don't forget the `initialization` and `finalization` sections in your unit, which will make your new class available to the system).