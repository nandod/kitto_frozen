# TreePanel #

Renders a TreeView (a special kind of view that is a tree of subviews) as a visual tree. Useful for menus.

Here's an example:

```
  Controller: TreePanel
    TreeView: MainMenu
    Width: 150
    Collapsible: True
    Border: True
```

As you can see the structure of the menu is defined into a separate file "MainMenu.yaml", so it can be used also by a ToolBar controller.

```
Type: Tree
Folder: Tables
  View: Employees
  View: Roles
  View: ActivityTypes
Folder: Customers
  View: Customers
  View: Projects
Folder: Activities
  View: ActivityInput
  View: ActivityReport
Folder: Maintenance
  View: Users
    ImageName: user
  View:
    DisplayLabel: Logout
    ImageName: login
```


This is the result:

![http://wiki.kitto.googlecode.com/git/Treepanel.png](http://wiki.kitto.googlecode.com/git/Treepanel.png)