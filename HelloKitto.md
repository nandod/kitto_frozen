# Hello Kitto #

![http://wiki.kitto.googlecode.com/git/HelloKitto.png](http://wiki.kitto.googlecode.com/git/HelloKitto.png)

HelloKitto is a sample application that you can use to explore the Kitto way of doing things, understand the basic concepts behind the framework, get ideas and copy-paste snippets for your own experimentations.

Also, copying and renaming the application directory and the project within is the fastest way to create a new application.

# Features #

The features of KItto used by HelloKitto are:

  * Multiple config files:
    * ConfigHelloWorld.yaml defines the smallest possible Kitto application, the classic "Hello World".
    * Config.yaml is the HelloKitto application itself.
    * ConfigEmbedded.yaml shows how to embed (part of) a Kitto application in an existing web page through an iframe.
  * DBExpress-based database connection to Firebird.
  * DB authentication using a standard database table and hashed passwords.
    * With defaults.
  * ExtJS theming.
  * Localization.
  * Models and model relationships (references and details).
  * Tree-based or Toolbar-based menu.
  * Custom logo through the use of an HtmlPanel controller.
  * Tabbed views (TabPanel controller).
  * StatusBar controller.
  * Data display and editing with rules.
  * List grouping with a custom javascript template.
  * List filtering with multiple user filters.
  * A custom form layout.
  * Master/Detail display and edit forms.

# Testing #

In order to use HelloKitto you need to create the database using the scripts or backup files provided in the `DB` directory, and edit `Config.yaml` so it points to your database and uses the correct authentication credentials.

Use the metadata of HelloKitto as both example and study material. You can find more details about the features in this wiki or in the Kitto Reference.