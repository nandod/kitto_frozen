### When using a predefined controller such as `ChangePassword` or `Logout`, how do I customize display labels and icons? ###

There are several ways. A controller defines its default display labels and icon by defining the public methods `GetDefaultDisplayLabel` and `GetDefaultImageName` (see the unit `Kitto.Ext.ChangePassword` for an example). These methods, if existing, are called if no alternative options are used. You can override these by specifying the keys `DisplayLabel` and `ImageName` at the tree view node level:

```
Folder: User options
  View:
    Controller: ChangePassword
    ImageName: my_icon
    DisplayLabel: My text
```

or at the view level (that is, directly in the view definition file) if the view is persistent.

You can also override any predefined Kitto icon by placing a file with the same name in your application's `Resources` directory.