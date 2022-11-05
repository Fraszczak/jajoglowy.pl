---
title: API Versioning
description: Article about how to versioning api
date: 2022-10-08
aliases: [api-versioning, api, versioning]
author: Piotr Fraszczak
url: blog/api-versioning
---

Spis treści: <!--more-->
* [Introduction]({{<     ref "api-versioning.md#First things first" >}})
  * [Guidelines]({{<     ref "api-versioning.md#Guidelines" >}})
  * [NuGet packages]({{< ref "api-versioning.md#NuGet" >}})
  * [Configuration]({{<  ref "api-versioning.md#configuration" >}})
* [Attributes]({{<         ref "api-versioning.md#attributes" >}})
* [Przykład użycia]({{<  ref "api-versioning.md#example" >}})
* [Podsumowanie]({{<     ref "api-versioning.md#summary" >}})


# Introduction {id="First things first"}

The topic of API versioning should start with [What is API](/blog/api-c#). Once we know how it works, we also know that it exists to enable data exchange, communication.
When the user interface changes, at best, users have to get used to it again. When the API changes and the client program is not prepared for the changes, it exits with an error. <!-- more -->

When the API is public, it likely has more than one client application. We can version changes to the code, so why not version the entire API?

## Guidelines {id="Guidelines"}

All API compliant with the [Microsoft REST API Guidelines](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md) **MUST** support explicit versioning. It's critical that clients can count on services to be stable over time, and it's critical that services can add features and make changes.

Services are versioned using a Major.Minor versioning scheme. Services **MAY** opt for a "Major" only version scheme in which case the ".0" is implied and all other rules in this section apply

 ## Installation - NuGet Packages {id="NuGet"}

 If you want to version your API, you need to install the [Microsoft.Aspnetcore.Mvc.Versioning](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Versioning/).

  {{< tabgroup align="right" style="code" >}}
  {{< tab name="Package Manager" >}}

  ```Package Manager
  NuGet\Install-Package Microsoft.AspNetCore.Mvc.Versioning -Version 5.0.0
  ```

  {{< /tab >}}
  {{< tab name=".Net CLI" >}}

  ```.Net CLI
  dotnet add package Microsoft.AspNetCore.Mvc.Versioning --version 5.0.0
  ```

  {{< /tab >}}
  {{< tab name="Package reference" >}}

  ```Package reference
  <PackageReference Include="Microsoft.AspNetCore.Mvc.Versioning" Version="5.0.0" />
  ```

  {{< /tab >}}
  {{< tab name="Paket CLI" >}}

  ```Paket CLI
  paket add Microsoft.AspNetCore.Mvc.Versioning --version 5.0.0
  ```

  {{< /tab >}}
  {{< tab name="Script & Interactive" >}}

  ```Script & Interactive
  #r "nuget: Microsoft.AspNetCore.Mvc.Versioning, 5.0.0"
  ```

  {{< /tab >}}
  {{< tab name="Cake" >}}

  ```Cake
  // Install Microsoft.AspNetCore.Mvc.Versioning as a Cake Addin
  #addin nuget:?package=Microsoft.AspNetCore.Mvc.Versioning&version=5.0.0

  // Install Microsoft.AspNetCore.Mvc.Versioning as a Cake Tool
  #tool nuget:?package=Microsoft.AspNetCore.Mvc.Versioning&version=5.0.0
  ```

  {{< /tab >}}
  {{< /tabgroup >}}

# Configuration {id="configuration"}

Once you have installed the appropriate packages, you need to update configuration file in your project.

```cs
builder.Services.AddApiVersioning(o =>
{
    o.AssumeDefaultVersionWhenUnspecified = true;
    o.DefaultApiVersion = new Microsoft.AspNetCore.Mvc.ApiVersion(1, 0);
    o.ReportApiVersions = true;
    o.ApiVersionReader = ApiVersionReader.Combine(
        new QueryStringApiVersionReader("api-version"),
        new HeaderApiVersionReader("X-Version"),
        new MediaTypeApiVersionReader("ver"));
});
```

# Attributes {id="attributes"}

As part of the package, we have a number of attributes at our disposal through which we version the API.
The table below lists them together with a description of their meaning.


######

| Attributes              | Meaning |
| :---                 |      ---: |
| ApiVersion           | An attribute that can be used from a controller or method. Sets the API version that the controller or action method will accept. May be used more than once to indicate more than one acceptable version. The versions in the ApiVersion attribute are discoverable. |
| ApiVersionNeutral    | The attribute that works at the controller level makes it resign from versioning. String version query is ignored. Any well-formed URLs when using URL versioning will be accepted. |
| MapToApiVersion      | An attribute that runs on the level of the method maps the hit to a specific version when multiple versions are specified at the controller level. The versions in the MapToApiVersion attribute are not discoverable. |
| AdvertiseApiVersions | Announces additional versions beyond what is contained in the application instance. Used when API versions are broken down into Deployments and API information cannot be aggregated with the API Version Explorer |

#
## Usage examples {id="example"}

```cs
  [ApiVersion("1.0")]
  [ApiVersion("2.0")]
  [ApiController]
  [Route("api/[controller]")]
  public class ValuesController : ControllerBase
  {
  //omitted for brevity
  }
```

The client who wants to use the functionality of the versioned API must take it into account in his requests.

#### Obsługiwane są dwie opcje określania wersji żądania REST API:

  {{< tabgroup align="right" style="code" >}}
    {{< tab name="Embedded in the URL path" >}}

  ```Embedded in the URL path
  https://api.some-url.pl/v1.0/products/users
  ```

    {{< /tab >}}
    {{< tab name="As the URL query string parameter" >}}

  ```As the URL query string parameter
  https://api.some-url.pl/products/users?api-version=1.0
  ```

    {{< /tab >}}
  {{< /tabgroup >}}

######
## Summary {id="summary"}

This is where the topic of API versioning ends, but after reading the article you may wonder if we visualize resources, e.g. with Swagger, can we version it too? <!--more--> 
Check [Swagger versioning](/pl/blog/swagger-versioning), I'll show you how to do it.
