# Introduction #

This documents explains how to deploy a Kitto application on a production system.

# Application directory #

When deploying a Kitto application, you need to provide its Application Home directory with the executable file and the `Metadata` and `Resources` subdirectories.

Plus, you need to provide the System Home directory. While during development these two directories are separated, in deployment it is recommended that you merge them together, keeping the files from the Application Home rather than those from the System Home in case of duplicates. It is easy to set up an installation script to do that, for example with [InnoSetup](http://www.jrsoftware.org/isinfo.php). This requires to define a single Apache alias instead of two.

# Configuring the web server #

Then you need to configure the Apache web server following the steps detailed [here](KittoConfigurationApache.md). Remember to take care of:

  * Installing ExtJS and CodePress.
  * Installing and configuring mod\_fastcgi.so.
  * Setup file permissions on the Application Home.
  * Create the application alias(es).

# Additional steps #

There are additional steps you can take in deployment, such as enabling HTTP compression or SSL cryptography in the web server. As Kitto uses [http://code.google.com/p/extpascal/](ExtPascal.md) for its FastCGI implementation, you can refer to [ExtPascal's documentation for advanced configuration topics](http://extpascal.googlecode.com/files/ExtPascal-Advanced-Configuration-eng-v6.pdf), keeping in mind Kitto's special aspects.