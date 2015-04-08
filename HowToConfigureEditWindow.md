## Question ##

I have a List controller and I want to set properties on the Form controller that is automatically created to edit and view single records. Where do I set these properties?

## Answer ##

By default, the List controller hosts a GridPanel which creates and displays the Form controller at appropriate times. So, your view definition should look like

```
Type: Data
Controller: List
  ...add AutoOpen, Filters and so on here...
  GridPanelController:
    FormController:
      ConfirmButton:
        Caption: Accept
```

The definition above will change the caption of the Form's confirmation button to 'Accept'. The `Controller/GridPanelController/FormController` node represents the Form controller's configuration. Put all your Form-specific setting there.

Please note that you can also use custom classes instead of the default GridPanel and Form by simply defining them in your code and specifying their names as node values:

```
Type: Data
Controller: List
  ...add AutoOpen, Filters and so on here...
  GridPanelController: MyGridPanel
    FormController: MyForm
      ConfirmButton:
        Caption: Accept
```