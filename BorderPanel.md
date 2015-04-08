# BorderPanel #

A controller that hosts up to 5 other controllers in a "border" layout.

![http://wiki.kitto.googlecode.com/git/BorderPanel.png](http://wiki.kitto.googlecode.com/git/BorderPanel.png)

Here's an example:

```
SubView:
  Controller: BorderPanel
      
    WestView:
      DisplayLabel: Navigation
      Controller: BorderPanel
        Width: 155
        Collapsible: True
        Border: True
        Split: True
        NorthView:
          Controller: HtmlPanel
            Split: False
            Html: <p><img src="%IMAGE(kitto_logo_150)%" width="150" height="47"></img></p>
        CenterView:
          Controller: TreePanel
            TreeView: MainMenu
      
    CenterView:
      Controller: TabPanel
        Border: True
        SubViews:
          View: Girls
          View: Dolls

    NorthView:
      Controller: ToolBar
        TreeView: MainMenu

    SouthView:
      Controller: StatusBar
        Text: <p>User: %Auth:UserName%</p>
      ImageName: user
```

And here's the result:

![http://wiki.kitto.googlecode.com/git/BorderPanelExample.png](http://wiki.kitto.googlecode.com/git/BorderPanelExample.png)

As you can see in the `WestView` above, each sub-controller can in turn host other controllers, and so on. You can try to "comment" (just add a leading dot or otherwise alter the name) the `NorthView` and see what happens.