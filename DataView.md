# DataView #

The DataView (a view with `Type: Data`) is a special kind of view that is data-bound. In a data-driven Kitto application, it's the most common type of view.

Data in kitto applications is conveyed through [Models](Models.md). You might want to have a look at what a model is and how model relationships work before going on with data views.

## Data views and controllers ##

A data view can currently use two kinds of controllers:
  * [List](List.md) displays a list of records in a grid that can be paginated, filtered and grouped.
  * [Form](Form.md) displays a form for viewing or editing a single record, using a customizable field layout.

Other controllers for data views, such as the Chart controller, are being designed and developed.

These controllers share some properties, which are specified under the Controller subnode of the data view and its data tables.

## Structure of a data view ##

A data view embeds a `MainTable`, which is a _view table_ object. The main table may have a set of detail tables (also view table objects), each of which may have a set of subdetail tables, and so on, forming a tree of data, depending on the relationships among models. A view table has an underlying `Model`, and represents a set of record fetched according to the model specification. By default, this means all the existing records, but they can be limited in various ways: fixed criteria, access-control filters, user filters, master/detail relationships. A view table has a set of _view fields_, which are view-level equivalents of model fields. You can override just about any model or model field property at the view table or view field level.

Among the typical properties of a view table:

  * `DisplayLabel`: Used as a label when displaying a single record. If missing, it is synthesized.
  * `PluralDisplayLabel`: Used as a label when displaying multiple records. If missing, the singular display label is "pluralized".
  * `IsReadOnly`: Overrides the model's read-only state. Determines whether to display update tools (new/edit/delete) or not.
  * `Fields`: List of model fields to display. Fields from referenced models can be included in the form `<Reference Name>.<Field Name>`. Each subnode has the field name as its name and optionally the display label as the value. If you don't specify a display label, it is inherited from the underlying model field. You can override any model field property (such as `IsVisible`, `IsRequired`, `Rules`, etc.) by adding subnodes under each view field. In the case of rules, the new rules are added rather than replaced. You cannot replace a model-level rule at the view level.
  * `DefaultFilter`: An optional SQL predicate that will be used as fixed filter for displayed records. May contain macros, which allows to create filters based on the authenticated user or other variables.
  * `Controller`: Contains all controller-related properties; see the documentation of each controller for details.

Other things you can configure in a view table are grouping and filtering, used by the [List controller](List.md).