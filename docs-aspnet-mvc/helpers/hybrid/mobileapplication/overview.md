---
title: Overview
page_title: Overview | Hybrid UI Application HtmlHelper
description: "Get started with the server-side wrapper for the hybrid Kendo UI Application widget for ASP.NET MVC."
previous_url: /helpers/mobileapplication/overview
slug: overview_hybridapplication_aspnetmvc
position: 1
---

# Hybrid Application HtmlHelper Overview

The hybrid Application HtmlHelper extension is a server-side wrapper for the [hybrid Kendo UI Application](http://demos.telerik.com/kendo-ui/m/index#application/loadingpopup) widget.

It allows you to configure the hybrid Kendo UI Application from server-side code.

## Getting Started

### The Basics

The Hybrid UI Application for ASP.NET MVC provides the necessary tools for building native-looking web-based mobile applications. When initialized, the mobile Application modifies the behavior of the hybrid Kendo UI widgets, so that they navigate between the mobile views when the user taps them.

There are two ways of navigation:

* Server navigation.
* Ajax navigation.

### Server Navigation

1. Create a new ASP.NET MVC 4 application. If you have installed the [Telerik UI for ASP.NET MVC Visual Studio Extensions]({% slug overview_aspnetmvc %}#kendo-ui-for-asp.net-mvc-visual-studio-extensions), create a Telerik UI for ASP.NET MVC application. If you decide not to use the Telerik UI for ASP.NET MVC Visual Studio Extensions, follow the steps from the [introductory article]({% slug overview_aspnetmvc %}) to add Telerik UI for ASP.NET MVC to the application.
1. Open `HomeController.cs` and modify the `Index` action method.

        public ActionResult Index()
        {
            return View();
        }

1. Add an action method for the `detail` view.

        public ActionResult Details()
        {
            return View();
        }

1. Add the default Hybrid UI View for ASP.NET MVC. The mobile application expects that the immediate child of the application element is a `MobileView`.

    ```ASPX
        <% Html.Kendo().MobileView()
            .Name("Index")
            .Title("Index")
            .Content(() =>
            {
                %>
                    View Content Template
                    <!--Add button that will `server navigate` the application-->
                    <%: Html.Kendo().MobileButton()
                            .Text("Navigate to Details")
                            .Url("Details", "Home")
                    %>
                <%
            })
            .Render();
        %>
    ```
    ```Razor
        @(Html.Kendo().MobileView()
            .Name("Index")
            .Title("Index")
            .Content(
                @<text>
                    View Content Template

                    <!--Add button that will `server navigate` the application-->
                    @(Html.Kendo().MobileButton()
                        .Text("Navigate to Details")
                        .Url("Details", "Home")
                    )

                </text>
            )
        )
    ```

1. Create a new `Details` ASP.NET MVC View file under the `/Views/Home/` folder.

    ```ASPX
        <%Html.Kendo().MobileView()
            .Title("Details")
            .Name("Details")
            .Content(() =>
            {
                %>
                View Details Template
                <!--Add back button that will `server navigate` the application to `Index`-->
                <%: Html.Kendo().MobileButton()
                        .Text("Go Back")
                        .Url("./")
                %>
                <%
            })
        %>
    ```
    ```Razor
        @(Html.Kendo().MobileView()
            .Title("Details")
            .Name("Details")
            .Content(
                @<text>
                View Details Template
                <!--Add back button that will `server navigate` the application to `Index`-->
                @(Html.Kendo().MobileButton()
                    .Text("Go Back")
                    .Url("./")
                )
                </text>
            )
        )
    ```

1. Initialize the Application inside the `Master/Layout` page and enable the server navigation.

    ```ASPX
        <%: Html.Kendo().MobileApplication()
                .ServerNavigation(true)
        %>
    ```
    ```Razor
        @(Html.Kendo().MobileApplication()
            .ServerNavigation(true)
        )
    ```

1. Build and run the Application.

### Ajax Navigation

1. Create a new ASP.NET MVC 4 application. If you have installed the [Telerik UI for ASP.NET MVC Visual Studio Extensions]({% slug overview_aspnetmvc %}#kendo-ui-for-asp.net-mvc-visual-studio-extensions), create a Telerik UI for ASP.NET MVC application. If you decide not to use the Telerik UI for ASP.NET MVC Visual Studio Extensions, follow the steps from the [introductory article]({% slug overview_aspnetmvc %}) to add Telerik UI for ASP.NET MVC to the application.

1. Open `HomeController.cs` and modify the `Index` action method.

        public ActionResult Index()
        {
            return View();
        }

1. Add an action method for the `detail` view.

        public ActionResult Details()
        {
            return PartialView();
        }

    > Notice that the view is returned as partial.

1. Add the default Hybrid UI View for ASP.NET MVC. The mobile application expects that the immediate child of the application element is a `MobileView`.

    ```ASPX
        <% Html.Kendo().MobileView()
            .Name("Index")
            .Title("Index")
            .Content(() =>
            {
                %>
                    View Content Template
                    <!--Add button that will `server navigate` the application-->
                    <%: Html.Kendo().MobileButton()
                            .Text("Navigate to Details")
                            .Url("Details", "Home")
                    %>
                <%
            })
            .Render();
        %>
    ```
    ```Razor
        @(Html.Kendo().MobileView()
            .Name("Index")
            .Title("Index")
            .Content(
                @<text>
                    View Content Template

                    <!--Add button that will `server navigate` the application-->
                    @(Html.Kendo().MobileButton()
                        .Text("Navigate to Details")
                        .Url("Details", "Home")
                    )

                </text>
            )
        )
    ```

1. Create a new `Details` ASP.NET MVC View file under the `/Views/Home/` folder.

    ```ASPX
        <%Html.Kendo().MobileView()
            .Title("Details")
            .Name("Details")
            .Content(() =>
            {
                %>
                View Details Template
                <!--Add back button that will `server navigate` the application to `Index`-->
                <%: Html.Kendo().MobileButton()
                        .Text("Go Back")
                        .Url("#:back")
                %>
                <%
            })
        %>
    ```
    ```Razor
        @(Html.Kendo().MobileView()
            .Title("Details")
            .Name("Details")
            .Content(
                @<text>
                View Details Template
                <!--Add back button that will `server navigate` the application to `Index`-->
                @(Html.Kendo().MobileButton()
                    .Text("Go Back")
                    .Url("#:back")
                )
                </text>
            )
        )
    ```

1. Initialize the mobile application inside the `Master/Layout` page.

    ```ASPX
        <%: Html.Kendo().MobileApplication()
            .PushState(true)
            .ServerNavigation(false)
        %>
    ```
    ```Razor
        @(Html.Kendo().MobileApplication()
            .PushState(true)
            .ServerNavigation(false)
        )
    ```

1. Build and run the application.

## Reference

### Instances

You can reference a hybrid Application instance by using the code from the following example. Once a reference is established, use the [hybrid Application API](https://docs.telerik.com/kendo-ui/api/javascript/mobile/application#methods) to control its behavior.

    @(Html.Kendo().MobileApplication()
            .ServerNavigation(true)
    )
    <script>
        $(function() {
            //Notice that the casing is important.
            var application = kendo.mobile.application;
        });
    </script>

## See Also

* [Telerik UI for ASP.NET MVC API Reference: ApplicationBuilder](http://docs.telerik.com/aspnet-mvc/api/Kendo.Mvc.UI.Fluent/MobileApplicationBuilder)
* [Overview of the Hybrid UI Application Widget](http://docs.telerik.com/kendo-ui/controls/hybrid/application)
* [Overview of Telerik UI for ASP.NET MVC]({% slug overview_aspnetmvc %})
* [Fundamentals of Telerik UI for ASP.NET MVC]({% slug fundamentals_aspnetmvc %})
* [Scaffolding in Telerik UI for ASP.NET MVC]({% slug scaffolding_aspnetmvc %})
* [Telerik UI for ASP.NET MVC API Reference Folder](http://docs.telerik.com/aspnet-mvc/api/Kendo.Mvc/AggregateFunction)
* [Telerik UI for ASP.NET MVC HtmlHelpers Folder]({% slug overview_barcodehelper_aspnetmvc %})
* [Tutorials on Telerik UI for ASP.NET MVC]({% slug overview_timeefficiencyapp_aspnetmvc6 %})
* [Telerik UI for ASP.NET MVC Troubleshooting]({% slug troubleshooting_aspnetmvc %})
