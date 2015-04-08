# Tool controllers #

Tools are buttons, rendered on the upper tool bar of a panel-based controller (such as the GridPanel or the Form controller), that define views which are displayed on click. A tool controller does not necessarily show anything on screen when displayed: for example, it can perform a server-side operation on data, it can build and serve a document or a report, or it can send an e-mail, etc.

Tools are defined as children of the ToolViews node of a panel-based controller.

Kitto sports many predefined tool controllers, but you can define your own that will execute your custom server-side code. Just inherit from `TKExtDataToolController` to have context information (such as the data store, data record, view table and view objects) readily available as properties.
If your controller needs to generate and serve a file or stream of data, you can inherit from `TKExtDownloadFileController`. Just look at how `TKExtDataToolController` and `TKExtDownloadFileController` are implemented.

# Predefined controllers #

  * Logout controller: it is a tool for the logout function (useful when authentication is enabled).

  * URL and FilteredURL controllers: provide a clickable hyperlink.
For example:

```
ToolViews:
  FilteredURL:
    DisplayLabel: Go!
    ImageName: www_page
    Controller: FilteredURL
      RequireSelection: False
      Filters:
        Filter:
          Header: REMOTE_ADDR
          Pattern: 127.0.0.1
          TargetURL: http://www.ethea.it
      DefaultURL: http://code.google.com/p/kitto/
```

The example above renders a button that, when clicked, navigates to a different URL based on the user's IP address (a local connection vs a remote one). You can filter on any HTTP header, and use wildcards and regular expressions in pattern, so you can navigate to different URLs based on the user's subnet and so on. If no filters are matched, you navigate to the default URL, if provided.

  * DownloadFile controller: is a generic tool for server-side download of a single file. The file can be a static file existing on the web server.

  * Other standard controllers include ExportExcelTool, ChangePassword, MergePDFTool, FOPTool, SendEmail, ExportCSVTool, ExportTextTool, ExportXMLTool. The list is growing.


# Chaining tools #

You can use the `BeforeExecute` property to chain tools. Let's say you want to have a button that downloads a pdf file generated on the fly, and another button that instead of downloading it sends it to a customer. Here's what you would do:
```
ToolViews:
  PrintOrderByFOP:
    DisplayLabel: Print order
    Controller: FOPTool
      ClientFileName: Order_{Id}.pdf
      TransformFileName: %APP_PATH%ReportTemplates\Order.xsl
      RequireSelection: True
      RequireDetails: True
      PersistentFileName: %APP_PATH%Resources\Orders\Order_{Id}.pdf
  SendEmail:
    BeforeExecute: 
      ToolView: PrintOrderByFOP
    DisplayLabel: Send order
    Controller: SendEmail
      RequireSelection: True
      #SMTP: Default
      Message:
        From: Demo application <demo@demo.com>
        To: {CustomerName} <{CustomerEmail}>
        #CC:
        #BCC:
        Subject: [DEMO] Order # {Id}
        Body: |
          Dear {CustomerName},
          please find your order # {Id} attached.
          Best regards,
          -- 
          Demo application
        Attachments:
          Order_{Id}.pdf: %APP_PATH%Resources\Orders\Order_{Id}.pdf
```

When a tool is launched through `BeforeExecute`, its client-side effects are suppressed, which in this example means that the pdf file is generated but not sent to the client. The email tool then picks up the file from the conventional location and attaches it to the message.