# Support Files #

By default, Kitto adds a few javascript and css files to the home page that is built and served when accessing the application. Each support file (either .js or .css) may be loaded either from the application's `Resources/js` directory, or from the framework's `Resources/js` directory (the former takes precedence over the latter).

Standard files loaded include:
## `kitto-core.js` and `kitto-core.css` ##
Contain fundamental styles and javascript code used throughout Kitto. These files should not be overridden unless you know exactly what you are doing.
## `application.js` and `application.css` ##
Optional files that you put inside your application's `Resources/js` directory and contain extra styles and javascript code used by your application. You can omit these if you don't need extra styles or javascript code.
## `<Id>.js/css` where `<Id>` is the Id of a controller ##
These files contain extra styles and code used by a controller (for example `TilePanel.css`). For standard controllers, these files are bundled with Kitto as necessary and you should only override them if you know what you are doing. For your custom controller, you can add them to your resource directory if needed.
## `<ViewName>.js/css` ##
Same as above but related to views. You can use these files to attach custom styles and code to single views. If a view does not have a name, you can still attach it to a css and/or js file by adding a `SupportBaseName` node to it with the base name (for example, `SupportBaseName: myview` maps to `myview.js` and `myview.css`.