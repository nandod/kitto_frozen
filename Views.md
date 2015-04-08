# Views and Controllers #

In Kitto, a View is a user interface unit; it might be a page or panel, a tabbed panel, a tree, a toolbar, a list or grid panel, and so on. Often, a view is linked to underlying data from the database, in which case it is a more specific concept, a [DataView](DataView.md).

In all cases, a View uses a Controller that renders it. While a View is a pure metadata object (implemented by a Delphi class that encapsulates the tree of nodes), a Controller is a Delphi class that generates Javascript and JSON code to render the user interface and manage user interaction through AJAX calls. Kitto controllers use ExtPascal and ExtJS to perform their job.

Controller-level properties are specified under the view's Controller subnode, and the set of supported properties depends on the controller type.

All controllers support an `AllowClose` Boolean parameter that defaults to True and determines whether the view can be closed by the user or not. You usually set this to False for views that are auto-opened on startup as opposed to views opened interactively through a menu.

## Defining views ##

### Yaml file View ###
A view can be defined in its own yaml file in the Views directory, and referenced where it's used. You can use a view in several places.

### Inline View ###
You can also define a view inline, which is convenient for very simple views you don't need to reference more than once.

Example:

```
# Refers to Views\MyHomeView.yaml
HomeView: MyHomeView

# Defines the view inline
HomeView:
  Controller: Viewport
    SubView:
      Controller: HtmlPanel
        Html: <h1>Hello World!</h1>
```


---

The latter example actually defines two inline views, one inside the other, each using its own controller, and also shows the smallest possible self-contained Kitto application.

---


### Using the ViewBuilder ###
The ViewBuilder is a convenient way to build a simple Data View with search support, without using a Yaml file. It is most useful for management of data like parameters or configuration tables.

Example:

```
# Refers to Taskitto example: Views\MainMenu.yaml
Type: Tree
Folder: Tables
  View: Employees
  View: Build AutoList
    Model: OPERATOR_ROLE
    DisplayLabel: Operator Roles
...
```

---

The latter example request to the Builder to generate an DataView based on the model OPERATOR\_ROLE with a free search on it. The only required node is Model.

---



Regardless of how you specify it, a view has some important subnodes. Special types of views have their own special nodes, but all views have a `DisplayLabel` (user-visible short description) and a `Controller`.

## Controllers ##

The value of the Controller node specifies its name, while the subnodes configure it. Often the most part of a view definition consists of controller configuration.

Here is a list of currently available controllers. Keep in mind that new controllers are going to be added frequently, and that you can easily create custom controllers and use them to render your views.

| **Name** | **Description** |
|:---------|:----------------|
| [Window](Window.md) | Displays a window with a border and specified size that will host other views |
| [Viewport](Viewport.md) | Hosts other views in a region that fills the entire browser surface and resizes with it |

The above are top-level controllers, that is controllers that can rendere autonomously. All other types of controllers need to be (ultimately) nested inside one of these two. You must use one of these to render the Home view.

Other predefined controllers:

| **Name** | **Description** |
|:---------|:----------------|
| BorderPanel | Hosts from 1 to 5 subviews in a border layout (NorthView, SouthView, WestView, EastView, CenterView). |
| TreePanel | Renders a [TreeView](TreeView.md) (a special kind of view that is a tree of subviews) as a visual tree. Useful for menus. |
| TabPanel | Shows subviews as tabbed panels. |
| ToolBar | Renders a [TreeView](TreeView.md) as a set of buttons with potentially multi-level menus. |
| StatusBar | The classic status bar for messages and info. |

Data-bound controllers (all need to render a [DataView](DataView.md)):

| **Name** | **Description** |
|:---------|:----------------|
| [List](List.md) | Renders a list of records. |
| [Form](Form.md) | Renders a single record (optionally allowing used editing). |

Action controllers are meant to perform operations rather than render something; views using these controllers are usually rendered as buttons or other clickable elements that launch the server-side code embedded inside the controller. Often you create custom controllers to be used this way, but Kitto includes several predefined controllers of this type that you can use or inherit from:

| **Name** | **Description** |
|:---------|:----------------|
| URL | Navigates to a given URL (supports macros). |
| FilteredURL | Navigates to a URL chosen from a set according to filter expressions. |
| DownloadFile| Downloads an already existing file or one prepared on-demand. |

You can see most of these controllers in action in the examples bundled with Kitto.