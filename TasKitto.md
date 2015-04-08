# TasKitto #

![http://wiki.kitto.googlecode.com/git/TasKitto.png](http://wiki.kitto.googlecode.com/git/TasKitto.png)

TasKitto is a sample application that manages a database of a few tables modeling a simple task tracking application. Use it together with the other example applications to explore Kitto and its features, or as a starting point for your own applications.

Copying and renaming the application directory and the project within is the fastest way to create a new application.

# Features #

The features of Kitto used by TasKitto are:

  * Multiple config files:
    * Config.yaml is the TasKitto application itself.
    * ConfigMini.yaml uses a different menu and strips down some elements to create a GUI usable on smartphones.
  * DBExpress-based database connection to Firebird.
  * ADO-based database connection to SQL Server - no changes needed.
  * DB authentication using a standard database table and clear passwords.
    * With passepartout option enabled.
  * ExtJS theming.
  * Models and model relationships (references and details).
  * Tree-based or Toolbar-based menu.
  * Custom logo through the use of an HtmlPanel controller.
  * Tabbed views (TabPanel controller).
  * StatusBar controller.
  * Data display and editing with rules.
  * List grouping with a custom javascript template.
  * List filtering with multiple user filters.
  * Custom form layouts.
  * Lookups on dynamic and fixed lists of values.
  * Master/Detail display and edit forms.

![http://wiki.kitto.googlecode.com/git/TasKitto_2.png](http://wiki.kitto.googlecode.com/git/TasKitto_2.png)

# Testing #

In order to use TasKitto you need to create the database using the scripts or backup files provided in the `DB` directory, and edit `Config.yaml` so it points to your database and uses the correct authentication credentials.

Use the metadata of TasKitto as both example and study material. You can find more details about the features in this wiki or in the Kitto Reference.