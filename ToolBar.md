# ToolBar #

Renders a TreeView as a set of buttons with potentially multi-level menus.

Here's an example:

```
NorthView:
  Controller: ToolBar
  TreeView: MainMenu
```

As you can see the structure of the menu is defined into a separate file "MainMenu.yaml", so it can be used also by a TreePanel controller.

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

As you can see into the image, the same MainMenu is rendered as a ToolBar (red highlighted) and a TreePanel (blue highlighted).