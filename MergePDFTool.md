# A Tool controller to merge data into PDF document #

This is a specific [DownloadFile](DownloadFile.md) Tool controller that creates a PDF file starting from a predefined PDF file, using the data of the selected record visible in the [Panel](Panel.md) based controller, and send it to the client browser.

**Prerequisites**:

Add Kitto.Ext.DebenuQuickPDFTools, to UseKitto.pas.

The Merge PDF Tool uses [DebenuQuickPDF library](http://www.debenu.com/products/development/debenu-pdf-library/) that must be installed and registered in the Web Server.
Kitto uses version 11 of the library (DebenuPDFLibraryLite1112.dll).

It's mandatory to specify:

_RequireSelection: True_ - this tool can work only on a single record.

_BaseFileName_ - the template pdf file used to merge the data).

_LayoutFileName_ - the node that points to a yaml file that contains the elements of data to put into the file.

Here's an example of the definition of the tool. Note that the coords of the elements starts from the bottom page of the document.

```
    ToolViews:
      PDFReport:
        DisplayLabel: Activity report PDF
        Controller: MergePDFTool
          ClientFileName: Activity_{Description}.pdf
          BaseFileName: %APP_PATH%ReportTemplates\ActivityReport.pdf
          LayoutFileName: %APP_PATH%ReportTemplates\ActivityPDFMerge.yaml
          RequireSelection: True
```

And here's an example of the LayoutFileName content:
```
DefaultFont: Courier
  Color: Red
  Size: 15.5

Information:
  Author: %Auth:UserName%
  Title: Activity Form
  Subject: {Description}
  Creator: Taskitto
  Keywords: Activity, Taskitto, {Description}
  CreationDate: %DATETIME%
  
Items:
  Image: 
    FileName: taskitto_logo_150.png
    Left: 10
    Top: 100
    Width: 440
    Height: 94
  Image: 
    FileName: kitto_logo_100.png
    Left: 10
    Top: 200
    Width: 100
    Height: 31
  Text: 
    Expression: Activity: {Description} on %DATETIME%
    XPos: 10
    YPos: 600
    Font: Helvetica
      Size: 12
      Color: Green
  Text: 
    Expression: {Employee}
    XPos: 10
    YPos: 550
    Font: HelveticaBold
      Size: 18.5
      Color: Blue
  QRCode: 
    Expression: %ActivityId%
    XPos: 10
    YPos: 700
    Size: 45.5
    Rotation: 90
  TextBox: 
    Expression: |
      Lorem ipsum dolor sit amet, consectetuer adipiscing elit. 
      Aenean commodo ligula eget dolor. Aenean massa. Cum sociis natoque...
    Left: 10
    Top: 500
    Width: 440
    Height: 200
    Align: Justified
    VertAlign: top
    Font: TimesRoman
      Size: 12
      Color: #0000FF
```