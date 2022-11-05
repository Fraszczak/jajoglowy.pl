---
title: API Versioning
description: Article about how to versioning api
date: 2022-10-08
aliases: [api-versioning, api, versioning]
author: Piotr Fraszczak
url: blog/api-versioning
---

### First things first

Before we read what API versioning is, we should know [What API is](/blog/api-c#). Once we know how it's works, we also known it's exist to enable data exchange, communicate.
When the user interface changes, at best, users have to get used to it again.
When the API changes and the client program is not prepared for the changes, throws error.
When the API is public, it likely has more than one client application. We can version changes to the code, so why not version the entire API?

### Guidelines
All API compliant with the [Microsoft REST API Guidelines](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md) MUST support explicit versioning. It's critical that clients can count on services to be stable over time, and it's critical that services can add features and make changes.

Services are versioned using a Major.Minor versioning scheme. Services MAY opt for a "Major" only version scheme in which case the ".0" is implied and all other rules in this section apply
### Two options for specifying the version of a REST API request are supported:

{{< tabgroup align="right" style="code" >}}
  {{< tab name="Embedded in the path of the request URL" >}}

```Embedded in the path of the request URL
https://api.twoja-sciezka.pl/v1.0/products/users
```

  {{< /tab >}}
  {{< tab name="As a query string parameter of the URL" >}}

```As a query string parameter of the URL
https://api.twoja-sciezka.pl/products/users?api-version=1.0
```

  {{< /tab >}}
{{< /tabgroup >}}