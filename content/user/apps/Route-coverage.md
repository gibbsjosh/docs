<!--
title: "Route Intelligence"
description: "Understand how vulnerabilities map to your app's attack surface"
tags: "user UI applications route coverage exercised vulnerabilities"
-->

## Background

Web requests are the primary interface of web applications. A request may be handled by one function with many subsequent functions coordinating interactions with other services, databases, or files. During the request handling process, Contrast monitors data flows across the application to identify vulnerabilities. It is not unusual for a single web request to be vulnerable to multiple types of attacks. Contrast can associate these vulnerabilities with the original request using a feature called Route Intelligence.

Let's look at an example request.

```
GET /users?active=true
Host: example.com
Accept: application/json
```

This request could be handled by a function such as:

```
@Controller
public class UserController {
    @GetMapping("/users")
    public String users(@RequestParam(name="active", required=false, defaultValue=true) Bool active) {
        ...
    }
}
```

An application route is a combination of three parts: an HTTP verb (`GET` in this example), the resource path (`/users`), and the method signature of the controller (`UserController.users(Bool active)`). With Contrast's route coverage, you can see detailed information on the components of your application such as which routes have been exercised versus which ones have not. This insight can help guide decisions like where to focus your testing efforts.

## How Contrast identifies routes in your application

When the Contrast agent starts, it instruments functions in the application so that web requests can be assessed for vulnerabilities while the application is running. If a function implements a framework to handle web requests, Contrast can identify the route before a request is handled. These routes are labeled **discovered** within Contrast.

When your application is handling a request, Contrast tracks the activity as an **observed** route.

Contrast supports route discovery for these frameworks: 

* **Java:** Jersey 2, Spring MVC 4, Struts 1 and Struts 2
* **.Net Core:** ASP.NET Core MVC (versions 2.1, 2.2, 3.0 and 3.1) and ASP.NET Core Razor Pages (versions 2.1, 2.2, 3.0 and 3.1)
* **.Net Framework:** ASP.NET MVC (versions 4 and 5), WebForms, WebAPI and WCF
* **Node:** Express, Hapi 17+, Koa and Kraken
* **Python:** Django, Pyramid and Flask
* **Ruby:** Rails and Sinatra


> **Note:** The Java and Node agents only report routes from supported frameworks. The Java agent also requires the option `-Dcontrast.agent.java.standalone_app_name=<example_name>` be defined in the agent configuration.

### What if the framework I'm using isn't supported?

Let Contrast Support know! Your feeback makes Contrast better. Contrast will attempt to infer the routes based on observed requests, but you will not see any routes discovered within Contrast.

### What if I add or remove a route from my application?

Discuss agent sessions here.

## View Route Details 

To see Contrast findings in the UI, select an application from the **Applications** grid. In your application's **Overview** tab, view the number of **Routes Exercised** compared to the number of total routes in your application. Click on the figure or select the **Route Coverage** tab to view details for each route that Contrast has identified in the application. 

<a href="assets/images/App-overview.png" rel="lightbox" title="View routes in your application Overview page"><img class="thumbnail" src="assets/images/App-overview.png"/></a>

Each layer of the chart represents routes that have been **discovered** by Contrast (but never exercised with the agent), **exercised** with the Contrast agent, and exercised and found to be **vulnerable**. Click on each layer to see how Contrast's findings have been updated each day. 

<a href="assets/images/App-route-coverage.png" rel="lightbox" title="View detailed coverage information for each route"><img class="thumbnail" src="assets/images/App-route-coverage.png"/></a>

View details on each route - including the servers on which it exists and the number of vulnerabilities found - in the **Route** grid. Click on the route signature to view the HTTP verb and URL, or click on the name of a server to go to the server's **Overview** page. Click on the vulnerability count in a grid row to view more information about each vulnerability in the application's **Vulnerabilities** page. (The number of critical vulnerabilities are noted with a red warning mark.)  

Use the dropdown menu to filter routes, or the search field to find specific routes in the grid. The date range (calendar) filter simultaneously updates your view in the grid and the chart. Users with administrator-level permissions can also click the **reset** icon to remove all routes listed in the grid. 

### Export route details 

To view and share route details outside of the Contrast UI, use the download icon above the grid to **Export Routes to CSV**. The spreadsheet includes a list of the application's routes, details about the server on which they were found and when the routes were last exercised as well as a list of vulnerabilities, the severity and status of each, and details about when the vulnerabilities were detected. 


