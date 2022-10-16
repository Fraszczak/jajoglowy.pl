---
title: Wersjonowanie API
description: Artykuł poruszający temat wersjonowania API
date: 2022-10-08
aliases: [api-versioning, api, wersjonowanie API]
author: Piotr Fraszczak
url: blog/api-versioning
---

Spis treści: <!--more-->
* [First things firts]({{< ref "api-versioning.pl.md#First things first" >}})
  * [Wytyczne]({{< ref "api-versioning.pl.md#Wytyczne" >}})
  * [Paczki NuGet]({{< ref "api-versioning.pl.md#NuGet" >}})
* [Konfiguracja wsparcia dla wersjonowania]({{< ref "api-versioning.pl.md#configuration" >}})
* [Atrybuty]({{< ref "api-versioning.pl.md#atrybutes" >}})
* [Przykład użycia]({{< ref "api-versioning.pl.md#example" >}})
* [Podsumowanie]({{< ref "api-versioning.pl.md#summary" >}})




## First things first {id="First things first"}

  Temat wersjonowania API warto zacząć od tego [Czym jest API](/blog/api-c#). Gdy już wiem jak działa, wiemy też, że istnieje żeby umożliwić wymianę danych, komunikację.
  Gdy interfejs użytkownika się zmieni, w najlepszym razie użytkownicy muszą się do niego ponownie przyzwyczaić. Gdy zmienia się API, a program klienta nie jest przygotowany na zmiany, kończy działanie błędem. <!--more-->
  Gdy API jest publiczne, prawdopodobnie ma więcej niż jedną aplikacje kliencką. Zmiany w kodzie możemy wersjonować, dlaczego by więc nie wersjonować całego API?

  ### Wytyczne {id="Wytyczne"}

  Wszystkie interfejsy API zgodne z wytycznymi [Microsoft REST API ](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md) MUSZĄ obsługiwać jawne przechowywanie wersji. Niezwykle ważne jest, aby klienci mogli liczyć na to, że usługi będą stabilne w czasie, a usługi mogą dodawać funkcje i wprowadzać zmiany.

  Usługi są wersjonowane przy użyciu schematu wersjonowania Major.Minor. Usługi MOGĄ wybrać schemat wersji tylko „główny”, w którym to przypadku dorozumiane jest „.0” i obowiązują wszystkie inne zasady opisane w tej sekcji.

  #### Obsługiwane są dwie opcje określania wersji żądania REST API:

  {{< tabgroup align="right" style="code" >}}
    {{< tab name="Osadzone w ścieżce adresu URL" >}}

  ```Osadzone w ścieżce adresu URL
  https://api.twoja-sciezka.pl/v1.0/products/users
  ```

    {{< /tab >}}
    {{< tab name="Jako parametr ciągu zapytania adresu URL" >}}

  ```Jako parametr ciągu zapytania adresu URL
  https://api.twoja-sciezka.pl/products/users?api-version=1.0
  ```

    {{< /tab >}}
  {{< /tabgroup >}}


  ### NuGet Packages {id="NuGet"}
  Chcąc wersjonować swoje API, musimy w ramach swojego projektu doinstalować sobie bibliotekę [Microsoft.Aspnetcore.Mvc.Versioning](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Versioning/).

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

## Konfiguracja wsparcia dla wersjonowania {id="configuration"}

tu o konfiguracji 


## Atrybuty API Versioning {id="atrybutes"}

tu o atrybutach i przykłady użycia


## Przykład użycia {id="example"}

tu już kawałki kodu i wyniki wywołań


## Podsumowanie {id="summary"}

tu podsumowanie i przekierwoanie to części o swagger versionig