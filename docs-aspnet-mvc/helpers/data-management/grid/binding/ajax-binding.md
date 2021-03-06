---
title: Ajax Binding
page_title: Ajax Binding | Telerik UI Grid HtmlHelper for ASP.NET MVC
description: "Learn how to implement Ajax binding with Telerik UI Grid HtmlHelper for ASP.NET MVC."
previous_url: /helpers/grid/ajax-binding
slug: ajaxbinding_grid_aspnetmvc
position: 2
---

# Ajax Binding

You can configure the Grid HtmlHelper extension for Ajax binding.

When configured for Ajax binding, the Grid for ASP.NET MVC makes Ajax requests when doing paging, sorting, filtering, grouping, or when saving data. For runnable examples, refer to the [demo page on binding of the Grid](https://demos.telerik.com/aspnet-mvc/grid/local-data-binding).  

The Ajax-bound mode has the following features:
- The Grid retrieves only the data (in JSON format) representing the current page. As a result, only the Grid is updated.
- All Grid templates (column, detail) are executed client-side. They follow the [Kendo UI for jQuery template](http://docs.telerik.com/kendo-ui/framework/templates/overview) definition rules and may contain embedded JavaScript code.

## Setting Up the Project

To configure the Grid for ASP.NET MVC to do Ajax binding to the **Products** table of the Northwind database:

1. Create a new ASP.NET MVC 4 application. If you have installed the [Telerik UI for ASP.NET MVC Visual Studio Extensions]({% slug overview_aspnetmvc %}#requirements), create a Telerik UI for ASP.NET MVC application. Name the application `KendoGridAjaxBinding`. If you decided not to use the Telerik UI for ASP.NET MVC Visual Studio Extensions, follow the steps from the [introductory article]({% slug overview_aspnetmvc %}) to add Telerik UI for ASP.NET MVC to the application.
1. Add a new `Entity Framework Data Model`. Right-click the `~/Models` folder in the solution explorer and pick **Add new item**. Choose **Data** > **ADO.NET Entity Data Model** in the **Add New Item** dialog. Name the model `Northwind.edmx` and click **Next**. This starts the **Entity Data Model Wizard**.

    ![A new entity data model](../images/grid-entity-data-model.png)

1.  Pick the **Generate from database** option and click **Next**. Configure a connection to the Northwind database. Click **Next**.

    ![Choosing the connection](../images/grid-entity-data-model.png)

1. Choose the **Products** table from the **Which database objects do you want to include in your model?**. Leave all other options as they are set by default. Click **Finish**.

    ![Choosing the Products table](../images/grid-database-objects.png)

1. Open the `HomeController.cs` and add a new action method which will return the Products as JSON. The Grid makes Ajax requests to this action.

        public ActionResult Products_Read()
        {
        }

1. Add a new parameter of type `Kendo.Mvc.UI.DataSourceRequest` to the action. It will contain the current Grid request information&mdash;page, sort, group, and filter. Decorate that parameter with the `Kendo.Mvc.UI.DataSourceRequestAttribute`. This attribute will populate the `DataSourceRequest` object from the posted data. Now import the `Kendo.Mvc.UI` namespace.

        public ActionResult Products_Read([DataSourceRequest]DataSourceRequest request)
        {
        }

1. Use the `ToDataSourceResult` extension method to convert the Products to a `Kendo.Mvc.UI.DataSourceResult` object. This extension method will page, filter, sort, or group your data using the information provided by the `DataSourceRequest` object. To use the `ToDataSourceResult` extension method, import the `Kendo.Mvc.Extensions` namespace.

        public ActionResult Products_Read([DataSourceRequest]DataSourceRequest request)
        {
            using (var northwind = new NorthwindEntities())
            {
                IQueryable<Product> products = northwind.Products;
                DataSourceResult result = products.ToDataSourceResult(request);
            }
        }


1. Return the `DataSourceResult` as JSON. Configure the Grid for Ajax binding.

        public ActionResult Products_Read([DataSourceRequest]DataSourceRequest request)
        {
            using (var northwind = new NorthwindEntities())
            {
                IQueryable<Product> products = northwind.Products;
                DataSourceResult result = products.ToDataSourceResult(request);
                return Json(result);
            }
        }

    The same thing applies when you use the asynchronous `ToDataSourceResultAsync` counterpart.

        public async Task<ActionResult> Products_Read([DataSourceRequest]DataSourceRequest request)
        {
            using (var northwind = new NorthwindEntities())
            {
                IQueryable<Product> products = northwind.Products;
                DataSourceResult result = await products.ToDataSourceResultAsync(request);
                return Json(result);
            }
        }

1. In the view, configure the Grid to use the action method created in the previous steps.

    ```ASPX
        <%: Html.Kendo().Grid<KendoGridAjaxBinding.Models.Product>()
            .Name("grid")
            .DataSource(dataSource => dataSource // Configure the Grid data source.
                .Ajax() // Specify that Ajax binding is used.
                .Read(read => read.Action("Products_Read", "Home")) // Set the action method which will return the data in JSON format
            )
            .Columns(columns =>
            {
                // Create a column bound to the ProductID property.
                columns.Bound(product => product.ProductID);
                // Create a column bound to the ProductName property.
                columns.Bound(product => product.ProductName);
                // Create a column bound to the UnitsInStock property.
                columns.Bound(product => product.UnitsInStock);
            })
            .Pageable() // Enable paging
            .Sortable() // Enable sorting
        %>
    ```
    ```Razor
        @(Html.Kendo().Grid<KendoGridAjaxBinding.Models.Product>()
            .Name("grid")
            .DataSource(dataSource => dataSource // Configure the Grid data source.
                .Ajax() // Specify that Ajax binding is used.
                .Read(read => read.Action("Products_Read", "Home")) // Set the action method which will return the data in JSON format.
            )
            .Columns(columns =>
            {
                // Create a column bound to the ProductID property.
                columns.Bound(product => product.ProductID);
                // Create a column bound to the ProductName property.
                columns.Bound(product => product.ProductName);
                // Create a column bound to the UnitsInStock property.
                columns.Bound(product => product.UnitsInStock);
            })
            .Pageable() // Enable paging
            .Sortable() // Enable sorting
        )
    ```

1. Build and run the application.

    ![The final result](../images/grid-bound-grid.png)

To download the Visual Studio Project, refer to [this GitHub repository](https://github.com/telerik/ui-for-aspnet-mvc-examples/tree/master/grid/ajax-binding).

The `ToDataSourceResult` method uses the `DataSourceRequest` parameter and LINQ expressions to page, sort, filter, and group your data. The JSON response of the action method will contain only a single page of data and the Grid will be bound to that data.

> * If your data is `IQueryable<T>` returned by a LINQ-enabled provider&mdash;Entity Framework, LINQ to SQL, Telerik OpenAccess, NHibernate or other&mdash;the LINQ expressions, created by the `ToDataSourceResult` method, are converted to SQL and executed by the database server.
> * The `ToDataSourceResult()` method will page, sort, filter, and group the collection that is passed to it. If this collection is already paged, the method returns an empty result.
> * As of the R1 2017 SP1 release, you can use the `ToDataSourceResultAsync` extension method to provide the asynchronous functionality of `ToDataSourceResult` by leveraging the `async` and `await` features of the .NET Framework.
> * If you impersonation is enabled, use the `ToDataSourceResultAsync` extension method with only one thread in your ASP.NET application. If you create a new thread, the impersonation in the newly created child thread decreases because, by default, all newly created child threads in ASP.NET run under the ASP.NET identity of the worker process. To change this behavior, explicitly impersonate the current identity within the code of the child thread.

The following example demonstrates how to implement the `ToDataSourceResultAsync` extension method in your project.

    public async Task<ActionResult> Products_Read([DataSourceRequest]DataSourceRequest request)
    {
        using (var northwind = new NorthwindEntities())
        {
            IQueryable<Product> products = northwind.Products;
            DataSourceResult result = await products.ToDataSourceResultAsync(request);
        }
    }

## Using the View Models

Sometimes it is convenient to use view model objects instead of entities returned by Entity Framework. For example, you may want to avoid serializing all Entity Framework properties as JSON or prevent serialization exceptions caused by circular references.

To use the view models and the Telerik UI Grid for ASP.NET MVC:

1. Perform all steps from the previous **Setting Up the Project** section.
1. Add a new class to the `~/Models` folder. Name it `ProductViewModel`.

        public class ProductViewModel
        {
            public int ProductID { get; set; }
            public string ProductName { get; set; }
            public short? UnitsInStock { get; set; }
        }

1. Modify the Grid declaration and make it use `ProductViewModel` instead of `Product`.

    ```ASPX
        <%: Html.Kendo().Grid<KendoGridAjaxBinding.Models.ProductViewModel>()
            .Name("grid")
            .DataSource(dataSource => dataSource
                .Ajax()
                .Read(read => read.Action("Products_Read", "Home"))
            )
            .Columns(columns =>
            {
                columns.Bound(product => product.ProductID);
                columns.Bound(product => product.ProductName);
                columns.Bound(product => product.UnitsInStock);
            })
            .Pageable()
            .Sortable()
        %>
    ```
    ```Razor
        @(Html.Kendo().Grid<KendoGridAjaxBinding.Models.ProductViewModel>()
            .Name("grid")
            .DataSource(dataSource => dataSource
                .Ajax()
                .Read(read => read.Action("Products_Read", "Home"))
            )
            .Columns(columns =>
            {
                columns.Bound(product => product.ProductID);
                columns.Bound(product => product.ProductName);
                columns.Bound(product => product.UnitsInStock);
            })
            .Pageable()
            .Sortable()
        )
    ```

1. Modify the `Products_Read` action method and use the `ToDataSourceResult` method overload which accepts a mapping lambda.

        public ActionResult Products_Read([DataSourceRequest]DataSourceRequest request)
        {
            using (var northwind = new NorthwindEntities())
            {
                IQueryable<Product> products = northwind.Products;
                // Convert the Product entities to ProductViewModel instances.
                DataSourceResult result = products.ToDataSourceResult(request, product => new ProductViewModel
                        {
                        ProductID = product.ProductID,
                        ProductName = product.ProductName,
                        UnitsInStock = product.UnitsInStock
                        });
                return Json(result);
            }
        }

To download the Visual Studio Project, refer to [this GitHub repository](https://github.com/telerik/ui-for-aspnet-mvc-examples/tree/master/grid/ajax-binding).

## Passing Additional Data to Action Methods

To pass additional parameters to the action, use the `Data` method. Provide the name of a JavaScript function which will return a JavaScript object with the additional data.

The custom parameter names must not match reserved words, which are used by the Kendo UI DataSource for jQuery for [sorting](http://docs.telerik.com/kendo-ui/api/javascript/data/datasource#configuration-serverSorting), [filtering](http://docs.telerik.com/kendo-ui/api/javascript/data/datasource#configuration-serverFiltering), [paging](http://docs.telerik.com/kendo-ui/api/javascript/data/datasource#configuration-serverPaging), and [grouping](http://docs.telerik.com/kendo-ui/api/javascript/data/datasource#configuration-serverGrouping).

The following example demonstrates how to add the additional parameters to the action method.

    public ActionResult Products_Read([DataSourceRequest]DataSourceRequest request, string firstName, string lastName)
    {
        // The implementation is omitted.
    }

The following example demonstrates how to specify the JavaScript function which returns additional data.

```ASPX
    <%: Html.Kendo().Grid<KendoGridAjaxBinding.Models.Product>()
        .Name("grid")
        .DataSource(dataSource => dataSource
            .Ajax()
            .Read(read => read
                .Action("Products_Read", "Home") // Set the action method which will return the data in JSON format.
                .Data("productsReadData") // Specify the JavaScript function which will return the data.
            )
        )
        .Columns(columns =>
        {
            columns.Bound(product => product.ProductID);
            columns.Bound(product => product.ProductName);
            columns.Bound(product => product.UnitsInStock);
        })
        .Pageable()
        .Sortable()
    %>
    <script>
        function productsReadData() {
            return {
                firstName: "John",
                lastName: "Doe"
            };
        }
    </script>
```
```Razor
    @(Html.Kendo().Grid<KendoGridAjaxBinding.Models.Product>()
        .Name("grid")
        .DataSource(dataSource => dataSource
            .Ajax()
            .Read(read => read
                .Action("Products_Read", "Home") // Set the action method which will return the data in JSON format.
                .Data("productsReadData") // Specify the JavaScript function which will return the data.
            )
        )
        .Columns(columns =>
        {
            columns.Bound(product => product.ProductID);
            columns.Bound(product => product.ProductName);
            columns.Bound(product => product.UnitsInStock);
        })
        .Pageable()
        .Sortable()
    )
    <script>
        function productsReadData() {
            return {
                firstName: "John",
                lastName: "Doe"
            };
        }
    </script>
```

## Enabling Client Data Processing

By default, the Telerik UI Grid for ASP.NET MVC makes an Ajax request to the `action` method every time the user changes the page, sorts, filters, or groups. Change this behavior by disabling the `ServerOperation`.

```ASPX
    <%: Html.Kendo().Grid<KendoGridAjaxBinding.Models.Product>()
        .Name("grid")
        .DataSource(dataSource => dataSource
            .Ajax()
            .ServerOperation(false) //Paging, sorting, filtering, and grouping will be done client-side.
            .Read(read => read
                .Action("Products_Read", "Home") // Set the action method which will return the data in JSON format.
                .Data("productsReadData")
            )
        )
        .Columns(columns =>
        {
            columns.Bound(product => product.ProductID);
            columns.Bound(product => product.ProductName);
            columns.Bound(product => product.UnitsInStock);
        })
        .Pageable()
        .Sortable()
    %>
```
```Razor
    @(Html.Kendo().Grid<KendoGridAjaxBinding.Models.Product>()
        .Name("grid")
        .DataSource(dataSource => dataSource
            .Ajax()
            .ServerOperation(false) // Paging, sorting, filtering, and grouping will be done client-side.
            .Read(read => read
                .Action("Products_Read", "Home") // Set the action method which will return the data in JSON format.
                .Data("productsReadData")
            )
        )
        .Columns(columns =>
        {
            columns.Bound(product => product.ProductID);
            columns.Bound(product => product.ProductName);
            columns.Bound(product => product.UnitsInStock);
        })
        .Pageable()
        .Sortable()
    )
```

## Customizing Content and Attaching Event Handlers on the Fly

In addition to using [server]({% slug configuration_gridhelper_aspnetmvc %}#template) and [client]({% slug configuration_gridhelper_aspnetmvc %}#clienttemplate) column templates, you may need to customize the appearance or content of the Grid data rows by using JavaScript&mdash;hide, show, or modify content, or attach custom event handlers.

When you use client-side data binding for the Grid, perform all these customizations in the [`dataBound`]({% slug overview_gridhelper_aspnetmvc %}#event-handling) event of the Grid. If the custom code is executed earlier&mdash;for example, in `document.ready`&mdash;it is very likely it has no effect, because the table rows are still not rendered at that time.

The following exception exists: delegated event handlers will work because they are attached to an ancestor element of the data rows&mdash;for example, the Grid [`table`](http://docs.telerik.com/kendo-ui/api/javascript/ui/grid#fields-table), [`tbody`](http://docs.telerik.com/kendo-ui/api/javascript/ui/grid#fields-tbody), or [`wrapper`](http://docs.telerik.com/kendo-ui/intro/widget-basics/wrapper-element), and the event handler code has to check what the event target is.

## Preventing Ajax Response Caching

To prevent Ajax response caching, refer to [this section from the Frequently Asked Questions article]({% slug freqaskedquestions_gridhelper_aspnetmvc %}#how-to-prevent-ajax-response-caching)

## See Also

* [Binding the Grid HtmlHelper for ASP.NET MVC to Data (Demos)](https://demos.telerik.com/aspnet-mvc/grid/local-data-binding)
* [Server-Side API](/api/grid)
