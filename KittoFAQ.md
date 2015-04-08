# Kitto Q&A #

This page links to a growing collection of small articles about Kitto techniques, how-tos, tips & tricks. If you need an article covering a particular subject, or if you think you have something to contribute, let us know by [posting in the forum](https://groups.google.com/forum/#!forum/kitto-discuss).

## Basic Questions ##

**Does Kitto work with FreePascal/Lazarus?**

_Currently Kitto supports only Delphi from XE3 to latest version. We have no own interest in supporting earlier versions or Delphi nor Lazarus/FPC, but contributions in this regard are welcome._

**Why are my Kitto applications (even the examples) not showing any GUI when launched?**

_They are probably trying to run as a service, which Kitto does when started under an administrative account. In such cases, you can force them to run in GUI mode by passing `-a` on the command line (or in Delphi's debugger parameters)._

## Models and Data Access ##

  * [Can I use more than one database at a time?](MultipleDatabases.md)
  * [How to disable editing for single fields](HowToDisableFields.md)
  * [How to upload files to the database](HowToUploadFilesToTheDatabase.md)

## User Interface ##

  * [How to customize view labels and icons](HowToCustomizeLabelsAndIcons.md)
  * [How to customize grid row colors](HowToCustomizeGridRowColors.md)
  * [How to display blob fields and external files as images](HowToDisplayFieldsAsImages.md)
  * [Kitto demos are very desktop-like. How can you make it look and feel more webby?](DesktopLikeLookAndFeel.md)
  * [Can I style my applications through CSS?](CanIUseCss.md)
  * [How do I configure a List controller's edit window?](HowToConfigureEditWindow.md)

## Authentication and Access Control ##

  * [How to use a custom table for user names and passwords ](CustomUserTable.md)

## Miscellaneous features ##

  * [How to set config and metadata values from external files](HowToSetConfigValuesFromTheOutside.md)
  * [Can I open a view in a new tab?](OpenViewInNewTab.md)