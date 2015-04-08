# ReportBuilder Tool controller #

This is a specific [DownloadFile](DownloadFile.md) Tool controller that builds a PDF file thorought ReportBuilder engine, using the data actually visible in the [Panel](Panel.md) based controller, and send it to the client browser.
It's necessary to specify some parameters to format the output, like the ReportBuilder File Template (rtm).

**Prerequisites**:
Add Kitto.Ext.ReportBuilderTools, to UseKitto.pas.
The ReportBuilder Tool is included only in the [Kitto Enterprise](KittoEntComponents.md) version. ReportBuilder must be the [Enterprise Version](https://www.digital-metaphors.com/products/enterprise.html) because Kitto uses DADE and RAP technologies of ReportBuilder.

**How to build reports**

Ethea offers a ReportBuilder designer tool, an advanced IDE for design reports with full support of "Data" Tab and "RAP" Tab. The tool share the Kitto connections to the Database and pass it to ReportBuilder engine.

**1st method: AutoSearch Parameters**

The 1st method uses parameters. They must be defined into the rtm template as an AutoSearch mandatory parameter named TABLE.FIELDID (eg. DOLL.DOLL\_ID). The tool searches for Autosearch parameters and put the field value of the actual record into them, then build the report that read data directly from the database.

This method is used when UsePipeLines is not specified or set to False (the default).

Here's an example:

```
    ToolViews:
      ReportBuilderSQL:
        DisplayLabel: _(Doll Card by RB)
        Controller: ReportBuilderTool
          ClientFileName: DollCard_{Doll_Id}.pdf
          TemplateFileName: %APP_PATH%ReportTemplates\DollCard.rtm
          RequireSelection: True
```

Note that ClientFileName can contains the key of the record printed.

**2nd method: KittoStore pipeline**

The 2st method uses pipelines that reads data from KittoStore structure. The pipelines are provided directly to the report designer (available when Design parameter of tool is set to True).

This method is used when UsePipeLines is set to True. In combination with RequireSelection to False you obtain a pipeline for the master viewtable.

Here's the example:

```
    ToolViews:
      ReportBuilderPipe:
        DisplayLabel: _(Dolls by RB)
        Controller: ReportBuilderTool
          ClientFileName: Dolls.pdf
          TemplateFileName: %APP_PATH%ReportTemplates\DollsListPipe.rtm
          RequireSelection: False
          UsePipeLines: True
          Design: True
```

In combination with RequireSelection to True and RequireDetails to True you obtain a pipeline for the master viewtable of the current record and the pipelines of every Details ViewTables.

Here's the example:

```
    ToolViews:
      ReportBuilderPipe:
        DisplayLabel: _(Parties by RB)
        Controller: ReportBuilderTool
          ClientFileName: Parties.pdf
          TemplateFileName: %APP_PATH%ReportTemplates\PartyListPipe.rtm
          RequireSelection: True
          RequireDetails: True
          UsePipeLines: True
          Design: True
```

The node **Design: True** is used to inform Kitto to open the ReportBuilder designer tool when the Report is called. When the report is ready, the Design attribute must be removed from yaml file, so Kitto launch the ReportBuilder engine with PDF output and download to the client the generated PDF file.