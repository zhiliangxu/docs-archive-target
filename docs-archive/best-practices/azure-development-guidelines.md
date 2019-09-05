---
title: Azure Development Guidelines
description: Rules and suggestions for developing services on Azure for APEX
---
# Azure Development Guidelines

This document covers the guidelines for how to use Azure to build services for APEX.

## Service Choice

When possible, use the highest level service that provides the flexibility needed to do the work required. By default, this should be Azure Web Apps for services where possible. Using docker containers to build and deploy services is on the radar but approval is still required to use these for service development.

Azure Functions can be used for simple services but all code needs to be checked in to source control and able to be run and tested locally. A full VSTS build and release is also needed as is the ability to have multiple instances for CI, staging, and production.

## Data Storage

**CosmosDB** should be the default choice for most large, scalable applications that need data storage. Large assets should be stored in **Blob storage**. Free form, full text query should be provided by **Azure Search Service**. **Event Hub** is the default choice for pub/sub and event based systems.

The following services should be avoided and require approval to use:
* SQL Azure DB
* Table Storage
* Azure Queues

## App Service Guidelines

### Use Local Caching

When creating an **App Service**, always turn on locale caching in your production slot only. This will prevent an entire class of LSIs that have to do with intermittent datacenter issues. You can find information about this setting on [Docs](https://docs.microsoft.com/en-us/azure/app-service/app-service-local-cache-overview)

### Hide Server Headers

For security best practices, we need to hide response headers such as ```Server```, ```X-Powered-By```.

#### Server

* If you are using ASP.Net Core, use the [KestrelServerOptions.AddServerHeader](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions.addserverheader) property to hide ```Server``` header. Sample code:  
  ```csharp
  new WebHostBuilder()
  ///...
  .UseKestrel(opt => opt.AddServerHeader = false)
  ///...
  ;
  ```

#### X-Powered-By

Add the below snippet to ```web.config``` to hide ```X-Powered-By``` header.
```xml
<system.webServer>
  <httpProtocol>
    <customHeaders>
      <remove name="X-Powered-By" />
    </customHeaders>
  </httpProtocol>
</system.webServer>
```
