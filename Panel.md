# Panel-based controllers #

Many controllers in Kitto are panel-based. These controllers use the ExtJS Panel control as basic rendering solutions. This page is meant to document the features of all panel-based controllers.

Among the controllers to which the information presented here applies are:

  * [TabPanel](TabPanel.md), [BorderPanel](BorderPanel.md), [HTMLPanel](HTMLPanel.md), [AccordionPanel](AccordionPanel.md) and so on.
  * [List](List.md), [Form](Form.md), [GridPanel](GridPanel.md), [ChartPanel](ChartPanel.md) and all data panels.

Any custom controller that you might want to build, as long as it ultimately inherits from `TKExtPanelControllerBase`, will get the same features. Another way to say it is that apart from the [Window](Window.md), [Viewport](Viewport.md) and the tool controllers, everything else is a panel-based controller.

## Tools ##

Tools are buttons, rendered on the upper tool bar of the view, that launch controllers (either predefined or custom). They are defined as children of the `ToolViews` node.

Example:

```
ToolViews:
  TestDownload:
    DisplayLabel: Download a file
    ImageName: download
    Controller: DownloadFile
      ConfirmationMessage: Are you sure you want to download the file?
      # Always enable the button. Use False to require selection of a
      # specific row (whose data is then passed as context info to the
      # controller). This only applies to data panels that support
      # row selection, such as the GridPanel.
      RequireSelection: False
      # Serve a static file (with expansion rules). 
      # Inherit from the controller if you want
      # to provide a file on demand (like a custom report).
      FileName: %APP_PATH%\temp\test.pdf
      # Provide a default file name for the user (which can save the
      # file under a different name if needed).
      ClientFileName: somefile.pdf
      # Empty means infer the content type from the file name.
      # In this case, it is going to be application/pdf.
      ContentType:
```

The example above will render a button with the text (or tooltip) "Download a file" and the "download" icon (one of Kitto's predefined icons, located under `Home\Resources`) that, when clicked, serves the specified file. By inheriting from the DownloadFile controller you can serve a file or stream built on-demand.

See the documentation for [Tool](Tool.md)s controllers, for more details.