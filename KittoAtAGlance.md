# Kitto at a glance #

![http://wiki.kitto.googlecode.com/git/Kitto_architecture.png](http://wiki.kitto.googlecode.com/git/Kitto_architecture.png)

Right now, Kitto is only at the beginning of its life, but...

  * If you fancy about creating a web front-end for that 200-table database you have;
  * If you want it with a sophisticated Ajax-powered rich user interface, with features such as search, filtering, grouping, charts, etc.;
  * If you'd very much like to write your server-side business rules in Delphi;
  * If you need to do it rapidly;
  * If you feel like doing it with Open Source tools.

If some or all of the above conditions are met, then a closer look at Kitto is in order.

# A closer look #

Kitto is an engine for creating data-driven web applications. This means that Kitto offers tools to ease the creation of applications that display and allow to manipulate data stored in a database. It is made in Delphi, and it is mainly aimed at Delphi developers not wanting to learn the intricacies of HTML, CSS and Javascript but wanting to get the job done quickly.

As such, Kitto is not a general-purpose tool for creating web sites.

![http://wiki.kitto.googlecode.com/git/Kitto_detailed.png](http://wiki.kitto.googlecode.com/git/Kitto_detailed.png)

A Kitto application is based on a set of text files in yaml format called the Metadata. Metadata contains the mapped database objects (Models), the user interface descriptions (Views and Layouts) and the general configuration parameters such as database connection. A Kitto application may as well contain business rules and controllers (custom user interface elements) written in Delphi.

Kitto roughly follows the MVC paradigm for data presentation.

Kitto offers the following features for application development:

  * Scalable 3-tier architecture.
  * Extensible metadata catalog based on yaml files.
  * Database-agnostic data access.
  * Modeling of database objects through Models.
  * Pluggable authentication mechanisms.
  * Pluggable access control/user permission management mechanisms.
  * Modeling of presentation (user interface) elements through Views and Layouts.
  * Pre-defined and pluggable Controllers.
  * Pervasive, pluggable macros throughout metadata.
  * Built-in localization for multi-language applications.

Kitto integrates with other tools to implement some of its features:

  * [ExtJS](http://www.sencha.com/products/extjs/) for the client-side user interface.
  * [ExtPascal](http://code.google.com/p/extpascal/) for generating the ExtJS code and managing the communication with the web server.
  * [Apache](http://httpd.apache.org/) as a web server, and [FastCGI](http://www.fastcgi.com/) as the communication protocol.
  * [dxgettext](http://dxgettext.po.dk/) for localization.

For more information, please read the [Getting started](KittoGettingStarted.md) guide, download the source code and the examples, and if you have doubts just ask!