### How to color grid rows in the List controller based on custom conditions? ###

You can customize the background color of grid rows in the [List](List.md) controller in several ways, depending on the complexity of your needs.

The easiest way is to define a `Colors` subnode at the model field or view field level (view field level, as usual, has precedence). The following example defines yellow as a color for rows in which the EMailAddress field matches the given regular expression, and red for all other rows.

```
EMailAddress:
  Colors:
    FFFF00: ^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,4}$
    FF0000: .*
```

If the ViewTable (or underlying Model) has more than one field with a `Colors` subnode, then the first one is used, unless you also specify `RowColorField` at the controller level:

```
Controller:
  RowColorField: EMailAddress
```

The above settings allow you to color rows based on pattern matching of single field values. If your row-coloring conditions are more complex (such as coloring based on numeric ranges, or multi-field conditions) and you don't mind writing a bit of javascript code, then you can use the `RowClassProvider` property:

```
Controller:
  RowClassProvider: |
    function (record, index) {
      var age = record.get('Age');
      if (age >= 15) {
        return 'important-row';
      } else if (age >= 12) {
        return 'semi-important-row';
      } else
        return '';
    }
```

In the example above, an anonymous javascript function is defined with a given prototype and return value. The function gets the data record and the row index as arguments (it can have more - see the ExtJS documentation for GridView.getRowClass for more details) and should return the name of a CSS class that will be applied to the entire row. The example applies a certain class for records with Age >= 15 and a different class for other record. The CSS classes can be defined statically in application.css, for example:

```
.important-row
{
  background-color: #FFB2B2;
}

.semi-important-row
{
  background-color: #FFEB99;
}
```

or they can be added dynamically by calling Kitto's predefined addStyleRule routine. Here's the revised example:

```
Controller:
  RowClassProvider: |
    function (record, index) {
      var age = record.get('Age');
      if (age >= 15) {
        addStyleRule('.important-row { background-color: #FFB2B2; }');
        return 'important-row';
      } else if (age >= 12) {
        addStyleRule('.semi-important-row { background-color: #FFEB99; ');
        return 'semi-important-row';
      } else
        return '';
    }
```

Generally you want to use static CSS classes, unless you need to build their contents based on data values.

---

As a future enhancement, Kitto will allow you to express your row-coloring condition in Delphi code, executed on the server side during data-retrieving Ajax calls.