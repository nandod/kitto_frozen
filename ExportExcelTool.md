# Export Excel Tool controller #

**Prerequisites**:
Add Kitto.Ext.ADOTools, to UseKitto.pas. On the Application sever use the [Microsoft Access Database Engine 2010 Redistributable](http://www.microsoft.com/it-it/download/details.aspx?id=13255).

This is a specific [DownloadFile](DownloadFile.md) Tool controller that builds an Excel file (by ADO) using the data actually visible in the [Panel](Panel.md) based controller, and send it to the client browser.
It's possibile to specify some parameters to format the output, like an Excel File Template.

Here's an example:

```
    ToolViews:
      DownloadExcel:
        DisplayLabel: Download in Excel
          Controller: ExportExcelTool
            ClientFileName: ActivityList.xls
            TemplateFileName: %APP_PATH%ReportTemplates\ActivityList.xlt
            RequireSelection: False
```