# Introduction #

Kitto uses [Apache](http://httpd.apache.org/) as its reference web server. It is recommended that you install a local copy of Apache on your development machine in order to ease development and testing.

Since Kitto uses ExtPascal as the underlying web engine, you might find the instructions [here](http://code.google.com/p/extpascal/wiki/GettingStarted) useful as well as what's on this page.

# Installing and configuring Apache on Windows #

First, make sure you have **at least Apache 2.2**. At the time of this writing, Apache 2.4.12 is the latest stable release and can be downloaded from [here](http://httpd.apache.org/download.cgi).

---

Make sure you don't install Apache in a path with spaces in it. `C:\Apache24\` is fine.

---


## Installing ExtJS ##

Currently, Kitto expects a copy of ExtJS 3.4.0 inside Apache's `/ext` path. This means that you should download the archive from [here](http://cdn.sencha.com/ext/gpl/ext-3.4.1.1-gpl.zip) and unpack it under `C:\Apache24\htdocs\`. This should get you a directory called `ext-3.4.1` that you need to rename to just `ext`: `C:\Apache24\htdocs\ext`.

You can check that installation went well by starting Apache and navigating to `localhost/ext`. By the way, ExtJS documentation is conveniently available under `localhost/ext/docs`, and examples are under `localhost/ext/examples`.

---

You **can** have different versions of ExtJS (as you can with versions of Kitto), if you indicate the wanted directory name in each Kitto application's config file.

---


## Installing CodePress ##

CodePress is a javascript library that features pretty display of Javascript code. It is used by ExtPascal to display error messages in debug mode. Download it from [here](http://sourceforge.net/projects/codepress/) and unpack it under `C:\Apache24\htdocs\`. That's all.

## Enabling FastCGI support for Kitto applications ##

To enable FastCGI support in Apache, install [the relevant module](http://www.fastcgi.com/mod_fastcgi/docs/mod_fastcgi.html). You can get it from [here](http://www.fastcgi.com/dist/mod_fastcgi-2.4.6-AP22.dll) and save it inside Apache's `modules` directory (`C:\Apache24\modules` in our example). Remember to rename it to `mod_fastcgi.so`.

Then you must install the module in Apache's configuration file, `C:\Apache24\conf\httpd.conf`. Make sure the line
```
LoadModule fastcgi_module modules/mod_fastcgi.so
```
is present and not commented out (that is, not starting with a #).

Add the following sections to the conf file:

Inside the `<IfModule alias_module>` section, locate the directive `ScriptAlias /cgi-bin/` and add an equivalent one below it called `/kitto/`. Example:

```
ScriptAlias /kitto/ "C:/Apache24/kitto/"
```

You should also **create** an empty directory inside `C:\Apache24` and call it `kitto`.

Then, configure permissions for the new directory by adding the following section:

```
<Directory "C:/Apache24/kitto">
    AllowOverride None
    Options None
    Order allow,deny
    Allow from all
</Directory>
```

Last step: create the application slots. The following example creates three slots for the examples included in kitto: they all use different TCP ports):

```
<IfModule fastcgi_module>
  # HelloKitto
  FastCgiExternalServer C:/Apache24/kitto/hellokitto -host localhost:2201 -idle-timeout 3000
  # TasKitto
  FastCgiExternalServer C:/Apache24/kitto/taskitto -host localhost:2202 -idle-timeout 3000
  # KEmployee
  FastCgiExternalServer C:/Apache24/kitto/kemployee -host localhost:2203 -idle-timeout 3000
</IfModule> 
```

The `C:/Apache24/kitto/` part in the slot name is mandatory, and depends on the settings made before. The last part reads as the TCP port number by convention, but it can be changed.

---

You must change the `C:/Apache24/` part to something else if you have installed Apache in a different path.

---

Always use forward slashes (`/`) in paths contained in httpd.conf, not backslashes (`\`).

---

You can add or remove slots as needed.

You are going to use these names when running web applications in the browser. For example:
```
http://localhost/kitto/hellokitto
http://localhost/kitto/taskitto
```
The `-host` parameter indicates the machine where your Kitto application is running and the TCP port it is listening on, so that Apache can call it. `localhost` is fine for development and single-server setups. Make sure the port number is the same as specified in the Kitto application's `Config.yaml` file (the default setting is `2014`).

---

Remember that you **always** need to restart Apache every time you edit `httpd.conf` in order for the changes to take effect. If you make mistakes, Apache will not start and write error messages in `C:\Apache24\logs\error.log` instead. Look there for clues.

---

Before you leave Apache's configuration files, there are a few more settings you need to make.

## Setting up file permissions ##

Create a `Directory` section for each of your Kitto applications, or just one that encloses all them. In this section you allow Apache to access the folders where your Kitto applications store resources. For example, if you had an application called HelloKitto under `C:\Dev\KittoApps\HelloKitto`, you would need:

```
<Directory "C:/Dev/Kitto/Examples/HelloKitto/">
    AllowOverride None
    Options None
    Order allow,deny
    Allow from all
</Directory>
```

You would need to create a `Directory` section for each application. If you have a folder under which all Kitto applications are stored (recommended setup), you just create a single Directory section for all of them. For example:

```
<Directory "C:/Dev/Kitto/">
  AllowOverride None
  Options None
  Order allow,deny
  Allow from all
</Directory>
```

## Creating application aliases ##

Finally, you need to create a pair of Apache aliases for each Kitto application you want to use. These are needed to enable Apache to find and serve resources from Kitto's System Home and Application Home folder. Locate the section in httpd.conf that reads <IfModule alias\_module>, and add the following lines inside it:

```
Alias /HelloKitto/ "C:/Dev/Kitto/Examples/HelloKitto/Home/Resources/"
Alias /HelloKitto-Kitto/ "C:/Dev/Kitto/Home/Resources/"
```

The above assumes that the application's name is `HelloKitto` (which is the name of the application's executable file, unless otherwise specified in its config file), that it is installed in the specified folder and that uses the standard layout (with Kitto under the `Externals` directory). If you are using a custom setup, change the lines above accordingly.

---

If your application doesn't show in the browser window when you launch it, or you notice missing resources (such as missing images), chances are that one or both aliases are wrong. Use a web debugging tool such as [Firebug](http://getfirebug.com/) to inspect the failed HTTP requests.

---

You must add two aliases for each of your Kitto applications (in production, usually a single alias is enough, as explained [here](KittoDeployment.md).

Your Apache configuration is done. Save your changes and restart Apache.

# Testing Apache, FastCGI and Kitto #

Once you have successfully restarted Apache, you can verify it is running correctly by opening a browser and going to

```
http://localhost
```

You should see the writing "It works!" on the screen. If your Apache instance is not using the standard TCP port (80), then you're going to add the port to the server name, separated by a colon.

Now start a Kitto application, for example HelloKitto, and make sure the status bar in the main form says "Started". Navigate to

```
http://localhost/kitto/hellokitto/
```

This assumes a default setup as shown above. You should see the application login form or main form depening on whether it requires authentication or not. If you don't see it, check the ports in Apache's configuration, look into Apache's error logs, and look at Firebug's Console errors.