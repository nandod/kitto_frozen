# Introduction #

This document describes Kitto's standard conventions for setting up Delphi, and how to change them for custom setups.

# Kickstart a new application #

For a quick startup of a new application, you can just:

  1. Copy the directory structure of [HelloKitto](HelloKitto.md) or another example application;
  1. Open the project file and save it under a new name;
  1. Edit Config.yaml and start creating your own metadata files.

---

When running the compiled application, beware that it will try to run as a service if under an administrator account. To force the application to run with a visible GUI, pass **`-a`** as a command line argument.

---

If instead you want a detailed description of what's where and what you can customize, read on.

# Details #

A Kitto application is made up of a Delphi project that you create and maintain, a Home folder with metadata and resources (such as images or javascript files), and a link to the Kitto sources or precompiled files.

## Standard directory layout ##

As you can see in the HelloKitto example, a typical folder structure for Delphi XE3 goes like this:

  * HelloKitto
    * Home
      * Metadata
        * Models
        * Views
        * Config.yaml
      * Resources
        * js
          * application.js
      * Example.exe (the compiled application)
    * Source
    * Projects
      * DXE3
        * HelloKitto.dproj/dpr (project files)
    * Lib
    * Externals
      * Kitto
        * Source
        * Home

Your application code goes into `Source` and `Projects`, and the project file should be configured to output the compiled dcu files into subdirectories of `Lib`:

<pre>Unit output directory = ..\..\Lib\DXE3\$(Config)\$(Platform)</pre>

and the exe file into `Home`:

<pre>Output directory = ..\..\Home</pre>

This folder contains your yaml metadata and support files as well, and is called the _Application Home_.

## The Application Home path ##

The Application Home is assumed to the your executable's directory. You can change this (for example when using a single Kitto executable to serve different applications) in the following ways:

  * By passing a `-home` argument on the command line.
  * By setting the property `TKConfig.AppHomePath` at application startup.

## Finding Kitto: The System Home path ##

`Externals\Kitto\Home` contains the predefined metadata and support files, such as Kitto's default style sheet and images, which are used by your Kitto application at run time and you shouldn't need to touch. This is called the _System Home_.

As you can see, the application needs to have Kitto inside its directory structure. This is done to allow different applications to use different versions of the library without stepping on each other's toes, and to make the application as much self-contained as possible.

The project should be configured to find Kitto, either in source format:

<pre>Search path = ..\..\Externals\Kitto\Source;<br>
..\..\Externals\Kitto\Source\EF;<br>
..\..\Externals\Kitto\Source\!ThirdParty;<br>
..\..\Externals\Kitto\Source\!ThirdParty\TPerlRegEx;<br>
..\..\Externals\Kitto\Source\!ThirdParty\!ExtPascal;<br>
..\..\Externals\Kitto\Source\!ThirdParty\!ExtPascal\ExtJSWrapper</pre>

or in pre-compiled format:

<pre>Search path = ..\..\Externals\Kitto\Lib\DXE3\$(Config)\$(Platform)</pre>

---

TPerlregEx is not used since Delphi XE.

---

For the latter version to work, you need to build `Externals\Kitto\Projects\DXE3\Kitto.dproj` at least once.

---


By default, the System Home is the first existing path in the following list (relative to the executable directory):

  1. ..\Externals\Kitto\Home\
  1. ..\..\Externals\Kitto\Home\
  1. ..\..\..\Home\
  1. %KITTO%\Home\

If none of the paths above exists, then the Application Home is assumed. This allows you to prepare a zero-configuration setup in which the System and Application Home paths are merged in the default Application Home. As explained [here](KittoDeployment.md), a typical deployment directory looks like this:

  * HelloKitto
> > HelloKitto.exe
    * Metadata
    * Resources

## Setting up a custom System Home ##

If you'd like a centralized setup better, you can change that easily, in one of the following ways:

  1. You can make Kitto a junction/link so it points to a directory elsewhere. Windows has been supporting file system links for a good while now; you can find a utility to manage them [here](http://schinagl.priv.at/nt/hardlinkshellext/hardlinkshellext.html).
  1. If you use Subversion, you can define `Kitto` as an external folder that points to a different svn repository.
  1. You can put Kitto elsewhere and change the search path accordingly in the project options. Be warned that this takes care of the `Kitto\Source` directory, but your application will still need `Kitto/Home` at run time. If you want to move `Kitto\Home` elsewhere as well, you will need to do the following at application startup (for example, in a unit's initialization section:

```

TKConfig.SystemHomePath := 'Path:\To\Kitto\Home\';
```

That's it. You should now be ready to develop and test your Kitto applications. We suggest that you stick to the standard conventions unless you have good reasons to do otherwise, because they are designed to save you time and effort in the long run.