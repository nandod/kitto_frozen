# FOP (Formatted Object Processor) Tool controller #

This is a specific [DownloadFile](DownloadFile.md) Tool controller that builds a PDF or RTF file using the data actually visible in the [Panel](Panel.md) based controller, and send it to the client browser.

**Prerequisites**:

Add Kitto.Ext.FOPTools, to UseKitto.pas.

The FOP Tool uses FOP Engine provided by [Apache FOP Project](http://xmlgraphics.apache.org/fop) that must be present in a Folder of the Application Server. The Path of the FOP Engine must be configured into Config.Yaml:
```
FOPEnginePath: C:\FOP\FOPEngine1.1
```

It's mandatory to specify TransformFileName, the xsl file used for the FO transformation. The ClientFileName extension determines the type of the output: PDF or RTF.

Here's an example:

```
# Export a text file with fixed column length.
ToolViews:
  FOPReport:
    DisplayLabel: Activity by FOP
      Controller: FOPTool
        ClientFileName: ActivityByFOP.pdf
        TransformFileName: %APP_PATH%ReportTemplates\ActivityReport.xsl
        RequireSelection: False
```