# TreeView #

a special kind of view that is a tree of subviews. It can be rendered by a ToolBar or a TreePanel.

This is an example:

```
Type: Tree
Folder: Options
  View: Parties
Folder: Lookup tables
  View: Build AutoList
    Model: HAIR
    MainTable:
      Controller:
        PopupWindow:
          Width: 320
          Height: 160
Folder: User
  View:
    Controller: ChangePassword
  View:
    Controller: Logout
```


This is the result:

![http://wiki.kitto.googlecode.com/git/ToolBar.png](http://wiki.kitto.googlecode.com/git/ToolBar.png)

As you can see into the image, the same structure can be rendered as a ToolBar (red highlighted) or a TreePanel (blue highlighted).