## Question ##

Can I style my applications using CSS?

## Answer ##

Of course you can. The most comprehensive way would be to get a theme for ExtJS. Currently Kitto uses ExtJS 3.4, but will be ported to ExtJS 4.x quite soon. Once a theme is installed in your ExtJS directory, you can have a Kitto application use it by adding

```
Ext:
  Theme: my-theme
```

to your config file.

You can also customize individual CSS styles by adding code to your `Home/Resources/js/application.css` file (which is included in every Kitto application by default). For example, if you wanted to have bigger text in buttons, you would do something like this:

```
.x-btn button {
    font-size: 125%;
}
```

In order to learn the structure and naming of ExtJS styles, we suggest you explore the generated pages with a debugger such as [Firebug](http://getfirebug.com/).

There are also ways to include additional CSS files and assign individual CSS classes to your GUI objects if you create custom controllers.