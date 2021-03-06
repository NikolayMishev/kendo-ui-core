---
title: Performance Issues
page_title: Performance Issues | Kendo UI Charts
description: "Learn the tips and tricks for improving the performance of the Kendo UI widgets rendering data visualization."
previous_url: /dataviz/performance-tips
slug: tipsandtricks_kendouistyling
position: 2
---

# Performance Issues

This page provides tips and tricks on how to handle and improve the performance of the [Kendo UI Gauges, Charts, Barcodes, Diagrams, and Maps](http://demos.telerik.com/kendo-ui/).

## Tips and Tricks

### Use Canvas Rendering

Switching to Canvas rendering improves the performance of the widgets, especially on mobile devices. It facilitates the visualization of data by its fast update and lower resource usage.

> **Important**
>
> Versions of Kendo UI prior to 2016.2 do not support interactivity
> and do not fire events when rendering in Canvas.

The example below demonstrates how to configure Canvas rendering.

###### Example

```dojo
    <div id="chart"></div>
    <script>
        $(function() {
            $("#chart").kendoChart({
                renderAs: "canvas",
                series: [{
                    type: "column",
                    data: [1, 2, 3]
                }]
            });
        });
    </script>
```

For more information on configuration settings, refer to the [`renderAs` API article](/api/dataviz/chart#configuration-renderAs).

### Use Inline Binding

When using a DataSource binding, all data items are be wrapped in [`Observable`](/api/javascript/data/observableobject) instances to track changes.

This is generally unnecessary for the Chart. It might become an issue if you have a large number of data points&mdash;5,000 and more.

In this case you can use [inline binding]({% slug databinding_charts_widget %}), as demonstrated in the example below.

###### Example

```dojo
    <div id="chart"></div>
    <script>
    $("#chart").kendoChart({
        series: [{
            name: "Series 1",
            type: "column",
            data: [200, 450, 300, 125, 100]
        }, {
            name: "Series 2",
            type: "column",
            data: [210, null, 200, 100, null]
        }],
        categoryAxis: {
            categories: ["Mon", "Tue", "Wed", "Thu", "Fri"]
        }
    });
    </script>
```

The example below demonstrates how to do inline binding with objects.

###### Example

```dojo
    <div id="chart"></div>
    <script>
    var seriesData = [
        { day: "Mon", sales1: 200, sales2: 210 },
        { day: "Tue", sales1: 450 },
        { day: "Wed", sales1: 300, sales2: 200 },
        { day: "Thu", sales1: 125, sales2: 100 },
        { day: "Fri", sales1: 100 }
    ];

    $("#chart").kendoChart({
        series: [{
            name: "Series 1",
            type: "column",
            data: seriesData,
            field: "sales1"
        }, {
            name: "Series 2",
            type: "column",
            data: seriesData,
            field: "sales2"
        }],
        categoryAxis: {
            categoryField: "day"
        }
    });
    </script>
```

### Turn Off Animated Transitions

Animated transitions might slow the browser down, especially if the page displays many active charts.

The example below demonstrates how to implement this approach.

###### Example

```dojo
    <div id="chart"></div>
    <script>
        $(function() {
            $("#chart").kendoChart({
                series: [{
                    type: "column",
                    data: [1, 2, 3]
                }],
                transitions: false
            });
        });
    </script>
```

The example below demonstrates how to turn off the initial animation only.

###### Example

```dojo
    <div id="chart"></div>
    <script>
      $(function() {
        var src = new kendo.data.ObservableArray([
            { value: 1 }, { value: 2 }
        ]);

        $("#chart").kendoChart({
          dataSource: {
               data: src
          },
          series: [{
              type: "column",
              field: "value"
          }]
        });

        var chart = $("#chart").data("kendoChart");
        chart.options.transitions = false;

        // Subsequent updates won't be animated
        setTimeout(function() {
          src.push({ value: 3 });
        }, 2000);
      });
    </script>
```

### Disable Gradients

Using solid fills instead of gradients noticeably improves performance.

The example below demonstrates how to disable gradients.

###### Example

```dojo
    <div id="chart"></div>
    <script>
        $(function() {
            $("#chart").kendoChart({
                seriesDefaults: {
                  overlay: {
                    gradient: null
                  }
                },
                series: [{
                    type: "column",
                    data: [1, 2, 3]
                }],
                transitions: false
            });
        });
    </script>
```

### Reduce the Number of Rendered Elements

When you have a lot of data points and categories, having all of them render will not only slow the chart down, but will also make the chart unreadable for the end user.

You can hide certain elements from the chart to improve both aspects:

* hide minor and major grid lines on the x-axis where many categories exist
* set a step for the category axis labels so only ever n-th label renders
* use a shorter format string for date axis labels
* hide labels for series and/or axes entirely

###### Example

```dojo
    <div id="chart"></div>
    <script>
        $(function() {
            $("#chart").kendoChart({
               categoryAxis: {
                  minorGridLines: {visible: false},//hide unnecessary elements
                  majorGridLines: {visible: false},//hide unnecessary elements
                  labels: {
                    step: 60,//every hourly label so they don't overlap
                    rotation: 90,//rotate so they take up less horizontal space and also reduce overlap
                    //visible: false,//hide labels altogether, you can set that for the series/seriesDefaults as well
                    dateFormats: {
                      days: "HH:mm" //use shorter format for the labels
                    }
                  },
                  baseUnit: "minutes" //set up a date axis, choose an appropriate range for your data
                },
                transitions: false
            });
        });
    </script>
```

## See Also

* [Themes and Appearance of the Kendo UI Widgets]({% slug themesandappearnce_kendoui_desktopwidgets %})
* [Rendering Modes for Data Visualization]({% slug renderingmodesfor_datavisualization_kendouistyling %})
* [Common Issues in Kendo UI Charts]({% slug troubleshooting_chart_widget %})
* [Common Issues in Kendo UI]({% slug troubleshooting_common_issues_kendoui %})
* [Kendo UI JavaScript Errors]({% slug troubleshooting_javascript_errors_kendoui %})
* [Kendo UI Performance Issues]({% slug troubleshooting_system_memory_symptoms_kendoui %})
* [Kendo UI Content Security Policy]({% slug troubleshooting_content_security_policy_kendoui %})
* [Common Issues in Kendo UI Excel Export]({% slug troubleshooting_excel_export_kendoui %})
* [Common Issues in Kendo UI ComboBox]({% slug troubleshooting_common_issues_combobox_kendoui %})
* [Common Issues in Kendo UI Diagram]({% slug troubleshooting_diagram_widget %})
* [Common Issues in Kendo UI DropDownList]({% slug troubleshooting_common_issues_dropdownlist_kendoui %})
* [Common Issues in Kendo UI Editor]({% slug troubleshooting_editor_widget %})
* [Common Issues in Kendo UI MultiSelect]({% slug troubleshooting_common_issues_multiselect_kendoui %})
* [Common Issues in Kendo UI Scheduler]({% slug troubleshooting_scheduler_widget %})
* [Common Issues in Kendo UI Upload]({% slug troubleshooting_upload_widget %})
* [Common Issues in Telerik UI for ASP.NET MVC](http://docs.telerik.com/aspnet-mvc/troubleshoot/troubleshooting)
* [Validation Issues in Telerik UI for ASP.NET MVC](http://docs.telerik.com/aspnet-mvc/troubleshoot/troubleshooting-validation)
* [Scaffolding Issues in Telerik UI for ASP.NET MVC](http://docs.telerik.com/aspnet-mvc/troubleshoot/troubleshooting-scaffolding)
* [Common Issues in the Grid ASP.NET MVC HtmlHelper Extension](http://docs.telerik.com/aspnet-mvc/helpers/grid/troubleshoot/troubleshooting)
* [Excel Export with the Grid ASP.NET MVC HtmlHelper Extension](http://docs.telerik.com/aspnet-mvc/helpers/grid/troubleshoot/excel-export-issues)
* [Common Issues in the Spreadsheet ASP.NET MVC HtmlHelper Extension](http://docs.telerik.com/aspnet-mvc/helpers/spreadsheet/troubleshoot/troubleshooting)
* [Common Issues in the Upload ASP.NET MVC HtmlHelper Extension](http://docs.telerik.com/aspnet-mvc/helpers/upload/troubleshoot/troubleshooting)
