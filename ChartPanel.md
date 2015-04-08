# ChartPanel (Kitto Enterprise only) #

The ChartPanel is a [DataPanelLeaf](DataPanelLeaf.md) controller and as such inherits all its base class' capabilities. For example, you can put ToolViews in a ChartPanel.

The ChartPanel is usually contained in a List controller (which provides filtering capability) and displays its ViewTable's records as a chart.

The available chart types are: Line, Bar, Column, StackedBar, StackedColumn, Pie. All but the latter are carthesian charts, with an X and an Y axes, while the pie chart has a `DataField` and a `CategoryField` instead.

Here is a simple example that showcases a few features:

```
Controller: ChartPanel
  Chart:
    Type: Line
    Axes:
      X:
        Field: Date
        Title: _(Day)
      Y:
        Field: Weight
        Title: _(Kgs)
        Min: 50
        Max: 80
      ChartStyle: |
        {
          xAxis: {
            labelRotation: -45
          }
        }
      TipRenderer: |
        return Ext.util.Format.number(record.data.Weight, '0.0')
          + ' kilograms on '
          + Ext.util.Format.date(record.data.Date);
```

The example above displays a simple line chart. The chart has a date field in the X axis and a numeric field in the Y axis (Kitto creates the correct type of axes based on the data types). There is no legend but each axis has a custom (localizable) title.

The Y axis has a custom min-max range defined. If you omit that, the engine will calculate one automatically based on the data set.

The ChartStyle param is very powerful, as it allows you to access the underlying chart implementation and style it. In this case it is used to rotate the X axis' labels. See http://docs.sencha.com/ext-js/3-4/#!/api/Ext.chart.Chart for details about this param (look for both chartStyle and extraStyle). Please note that the param's value is a block of plain JSON code instead of the usual YAML tree. This is because Kitto passes it as-is (except macro expansion) to the underlying chart. You can copy-paste ExtJS- or YUI-based chart styling examples right in your view definition, but with great power comes great responsibility: Since you are going low-level here, a change in the chart implementation in future Kitto versions might require checking/rewriting this kind of definitions.

The TipRenderer is a bit of Javascript code that you can use to custom-formatting the tooltip that pops up for each data point when the mouse pointer hovers over it. The code should be written as a function that receives the data record as the `record` argument and the chart object as the `chart` argument. By default, the tooltip simply displays the values of the axes with a line break between them. In this example we build a description string with formatted data values in it.

### Charts with multiple series ###

Here is a slightly more complex example with a legend and multiple series:

```
Controller: ChartPanel
  Chart:
    Type: Line
    Axes:
      X:
        Field: Date
        Title: _(Day)
    Series:
      Series:
        Type: Line
        YField: Weight
        DisplayName: _(Kgs)
      Series:
        Type: Column
        YField: PercFat
        DisplayName: _(Fat %)
      Series:
        Type: Line
        YField: PercWater
        DisplayName: _(Water %)
      Series:
        Type: Line
        YField: CMI
        DisplayName: _(C.M.I.)
    ChartStyle: |
      {
        xAxis: {
          labelRotation: -45
        },
        legend: {
          display: 'bottom'
        }
      }
    TipRenderer: |
      return series.displayName + ' = '
        + Ext.util.Format.number(record.get(series.yField), '0.0')
        + ' al ' + Ext.util.Format.date(record.data.Data);
```

A few highlights in the example above:

  * You don't need to explicitly configure an axis (but you can do that, if you need to specify custom parameters) when you have one or more series using it.
  * You can mix series of different types (you cannot mix pie and carthesian series, though). If you don't need to do that, you can omit the series' `Type` parameters altogether. Please note that if you are using a StackedBar or StackedColumn chart you **must** omit the `Type`.
  * Adding a default legend is as simple as specifying where it should be displayed in the `ChartStyle` JSON code. You can style it in the same way (see ExtJS docs).
  * In multiple-series charts, the TipRenderer also receives `series` and `index` arguments, so you can build different tooltips for different series. This example keeps it simple while still showing you how to use the `series` argument. You could inspect series.yField and build a completely different tooltip for each different series (or even data value, should you need to do that). Please be aware at all times that this is javascript, not Kitto, so mind the character case of everything, the display format string syntax, etc.

### Pie charts ###

Example of a pie chart:

```
Controller: ChartPanel
  Chart:
    Type: Pie
    DataField: Weight
    CategoryField: YearPlusMonth
    ChartStyle: |
      {
        legend: {
          display: 'bottom'
        }
      }
    TipRenderer: |
      return Ext.util.Format.number(record.data.Weight, '0.0')
        + ' kilograms in month '
        + record.data.YearPlusMonth.replace("_", "/");
```

The chart has a slice for each distinct value in the `CategoryField`, whose area is proportional to the value in the same record's `DataField`.

In this example, the YearPlusMonth field is a string in the format 'YYYY\_MM', which the `TipRenderer` slightly massages before displaying.

### Stacked charts ###

Stacked bar or column charts display all series (all stackable ones, to be precise) in the same bar or column. This is useful for comparing performances, for example. Here is how you define a stacked bar chart:

```
CenterController: ChartPanel
  Chart:
    Type: StackedBar
    Axes:
      Y:
        Field: YearPlusMonth
    Series:
      Series:
        XField: FatPerc
        DisplayName: _(Fat %)
      Series:
        XField: WaterPerc
        DisplayName: _(H2O %)
    ChartStyle: |
      {
        animationEnabled: true,
        legend: {
          display: 'bottom'
        }
      }
```

Please note the missing `Type` specification for the series.

---

Displaying charts currently requires the Flash plugin to be installed in the browser.