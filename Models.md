# Models #

Models in Kitto represent data. The most common case is a model representing a database table, although other types of models based on database views or more abstract models are possible.

A model is a yaml file stored under Metadata\Models, and is identified by its name. Here is an example:

```
ModelName: Party
Fields:
  Party_Id: String(32) not null primary key
    IsVisible: False
    DefaultValue: %COMPACT_GUID%
  Party_Name: String(40) not null
    Hint: Enter a catchy name
    Rules:
      ForceCamelCaps:
  Party_Date: Date not null
    DefaultValue: {date}
  Party_Time: Time not null
  Address: String(256) not null
    DisplayWidth: 80
  Party_Period: String(20)
    DisplayLabel: Period
    DisplayWidth: 10
    Expression: |
      case
        when %DB.CURRENT_DATE% - PARTY.PARTY_DATE < 0 then cast('Future' as varchar(10))
        when %DB.CURRENT_DATE% - PARTY.PARTY_DATE = 0 then cast('Today' as varchar(10))
        when %DB.CURRENT_DATE% - PARTY.PARTY_DATE = 1 then cast('Yesterday' as varchar(10))
        when %DB.CURRENT_DATE% - PARTY.PARTY_DATE between 2 and 7 then cast('Last Week' as varchar(10))
        when %DB.CURRENT_DATE% - PARTY.PARTY_DATE between 8 and 30 then cast('Last Month' as varchar(10))
        else cast('Older' as varchar(10))
      end
DetailReferences:
  Invitation: Invitation
```

## Model properties ##

As you can see in the example above, a model is identified by its `ModelName`. In this case, it is simply the name of the underlying database table. You can set the `IsLarge` node to `True` to mark a model with a large number of records, which will affect the user interface (for example by disallowing unfiltered queries or paginating the result sets).

A model can also have data validation [`Rules`] that you declare in the model and that are enforced at various times. Kitto includes a lot of predefined rule types you can use, and you can add your own in Delphi or Javascript.

## Model fields and their properties ##

Fields are defined under the `Fields` node, using a syntax similar to SQL. Every field has a data type, optionally a size (for some data types only), and can be a required field (`not null`) or not. One or more fields can also be marked as the model's `primary key` (this is required to make the model data editable).

Kitto includes a set of predefined data types, but since a data type is just a Delphi class inherited from [TEFDataType](http://www.ethea.it/docs/kitto/en/ref/TEFDataType.html), you can add your own. Current data types are String, Memo, Integer, Date, Time, DateTime, Boolean, Float, Decimal plus the special Reference type which represents a reference to another model (the equivalent of a database foreign key).

A field can also have subnodes with additional attributes. For example:

  * IsVisible is True by default, and if set to False makes the field invisible in the user interface. In this case, we make the primary key invisible as it's auto-generated and not user-readable.
  * DefaultValue is the value written to the field when a new record is created in the user interface. In this case, the value is a compact GUID (a GUID with stripped curly braces and dashes, encoded as a 32-char string) generated through a [macro](Macros.md). Default values can also be special values of types other than String, such as `{date}` which expands to the current date.
  * Hint is a string that the user interface displays as a hint in the field when the value is empty (such as when inserting a new record).
  * [Rules](Rules.md) is a subnode containing field validation rules that you can set and that will be enforced on the client or on the server, according to the type of rule, during data editing.
  * DisplayLabel is a user-readable description for the field. If you use meaningful field names, you are not going to need to specify a display label for all of them, as Kitto will try to build readable labels from the field names. We suggest that you only add display labels after seeing the user interface, or if you have special needs such as localization or to shorten field names.
  * DisplayWidth is a width in characters for the field in the user interface (be it a grid column or an editor in a form). If this property is missing, Kitto tries to determine a default sensible width for each field based on its data type. As with display labels, we suggest you don't add display widths in advance as they might not be needed.
  * IsReadOnly marks the field as not editable in the user interface. Note that some fields are implicitly read-only, such as expression fields.
  * Expression: use this node to define a field as the result of a SQL expression calculated on the fly. These are fields that do not exist in the underlying database table but are needed in the application. Expressions may contain [macros](Macros.md), and particularly database-scoped macros (those with a DB. prefix) to help writing database-independent code.
  * AllowedValues: a set of values to which the input in the field is limited. Each value can have a display label; subnodes are in the format `DisplayLabel: Value`.
  * Images: a set of subnodes in the format `ImageName: RegExp` used to display images together with field values. The first image matching the field value is displayed in a grid column (or no image if the value doesn't match). Each image may have a `DisplayTemplate` subnode that, if present, overrides the field's own `DisplayTemplate`.
  * BlankValue: set this to True to hide the field value when an image is displayed (otherwise both image and value are shown). If no image is displayed, this property is ignored. Please note that the image will always have a tooltip regardless of this property.
  * DisplayTemplate: a string format that will be used to render the field's value. The template text may include the `{value}` placeholder that will be replaced with the field's value in read-only GUIs, and any  `{FieldName}` placeholders for other fields' values. A template of `'{value}'` acts the same as no template.

Generally, all model-level properties can be overridden or set at the view level. According to the [DRY principle](http://en.wikipedia.org/wiki/Don't_repeat_yourself), you want to set as much properties as possible at the lowest possible levels, and optionally tweak them at the higher levels as required.

## Relationships ##

A model can have two types of relationships with other models:

### "Has one" references ###

A field of the `Reference` type represents a "has one" relationship, and is loosely mapped on a database foreign key.

Example of Reference field:
```
  Hair: Reference(Hair)
    Fields:
      Hair_Id: 
```

Fields of this type have a `Fields` subnode containing all fields that make up the key to the other model. These fields only need names, as their data types and other properties are taken from the corresponding fields in the referenced model's primary key.

### "Has many" references ###

A node under `DetailReferences` represents a "has many" relationship, and usually corresponds to a reference field in the referenced model that points back to this model.

Examples:

```
# Party.yaml
ModelName: Party
...
DetailReferences:
  Invitation: Invitation

# Invitation.yaml
ModelName: Invitation
Fields:
...
  Party: Reference(Party) not null
    Fields:
      Party_Id:
```

The detail reference is specified through a node with the reference name as the name and the referenced model's name as the value.

### Database routing ###

By default, a model is linked to the default database (`'Main'`) defined in `Config.yaml`. You can add the property:

```
DatabaseRouter: Static
  DatabaseName: Other
```

to link it to the `'Other'` database instead. This means that all read and write operations for that model are performed against the `'Other'` database (which must be defined in `Config.yaml` as well).