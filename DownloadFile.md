# Download File Tool Controller #

The DownloadFile tool, is a generic tool for server-side download of a single file. The file can be a static file existing on the web server.

For example:
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
      # Provide a persistent file name to save it server-side.
      # Anothar action can use it (eg.to send it by Email).
      PersistentFileName: %APP_PATH%OutputFiles\Somefile_{Id}.pdf
      # Empty means infer the content type from the file name.
      # In this case, it is going to be application/pdf.
      ContentType:
```

Other Download Tool controllers inherits from DownloadFile and can build on the fly a specific file to download:
  * [ExportTextTool](ExportTextTool.md) To generate a Text file
  * [ExportCSVTool](ExportCSVTool.md) To generate a CSV file
  * [ExportExcelTool](ExportExcelTool.md) To generate an Excel file via ADO
  * [MergePDFTool](MergePDFTool.md) To generate a PDF file with merge capabilities
  * [FOPTool](FOPTool.md) To generate a PDF file using Apache FOP
  * [ReportBuilderTool](ReportBuilderTool.md) To generate a PDF file using ReportBuilder engine