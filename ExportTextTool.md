# Export Text Tool controller #

This is a specific [DownloadFile](DownloadFile.md) Tool controller that builds a text file using the data actually visible in the [Panel](Panel.md) based controller, and send it to the client browser.
It's possibile to specify some parameters to format the output.

Here's an example:

```
# Export a text file with fixed column length.
ToolViews:
  DownloadText:
    DisplayLabel: Download in TXT
    Controller: ExportTextTool
      RequireSelection: False
      IncludeHeader: False
      ClientFileName: ActivityList.txt

```