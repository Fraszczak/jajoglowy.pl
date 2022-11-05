---
title: Wersjonowanie API
description: Artykuł poruszający temat wersjonowania API
date: 2022-10-08
aliases: [api-versioning, api, wersjonowanie API]
author: Piotr Fraszczak
url: blog/api-versioning
---

Spis treści: <!--more-->
* [Wstęp]({{<           ref "api-versioning.pl.md#First things first" >}})
  * [Wytyczne]({{<      ref "api-versioning.pl.md#Guidelines" >}})
  * [Paczki NuGet]({{<  ref "api-versioning.pl.md#NuGet" >}})
  * [Konfiguracja]({{<  ref "api-versioning.pl.md#configuration" >}})
* [Atrybuty]({{<        ref "api-versioning.pl.md#attributes" >}})
* [Przykład użycia]({{< ref "api-versioning.pl.md#example" >}})
* [Podsumowanie]({{<    ref "api-versioning.pl.md#summary" >}})




# Wstęp {id="First things first"}

  Temat wersjonowania API warto zacząć od tego [Czym jest API](/pl/blog/api-c#). Gdy już wiem jak działa, wiemy też, że istnieje żeby umożliwić wymianę danych, komunikację.
  Gdy interfejs użytkownika się zmieni, w najlepszym razie użytkownicy muszą się do niego ponownie przyzwyczaić. Gdy zmienia się API, a program klienta nie jest przygotowany na zmiany, kończy działanie błędem. <!--more-->

  Gdy API jest publiczne, prawdopodobnie ma więcej niż jedną aplikacje kliencką. Zmiany w kodzie możemy wersjonować, dlaczego by więc nie wersjonować całego API?

  ## Wytyczne {id="Guidelines"}

  Wszystkie interfejsy API zgodne z wytycznymi [Microsoft REST API ](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md) **MUSZĄ** obsługiwać jawne przechowywanie wersji. Niezwykle ważne jest, aby klienci mogli liczyć na to, że usługi będą stabilne w czasie, a usługi mogą dodawać funkcje i wprowadzać zmiany.

  Usługi są wersjonowane przy użyciu schematu  Major.Minor. Usługi **MOGĄ** wybrać schemat wersji tylko „główny”, w którym to przypadku dorozumiane jest „.0” i obowiązują wszystkie inne zasady opisane w tej sekcji.


  ## Instalacja - NuGet Packages {id="NuGet"}

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


# Konfiguracja {id="configuration"}

Gdy już zainstalujesz odpowiednie paczki, zostaje zaktualizować plik konfiguracyjny w Twoim projekcie.

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


# Atrybuty {id="attributes"}

W ramach pakietu mamy do dyspozycji szereg atrybutów, poprzez które wersjonujemy API. Tabelka niżej zawiera je wraz z opisem znaczenia.

######

| Atrybut              | Znaczenie |
| :---                 |      ---: |
| ApiVersion           | Atrybut którego można użyć z poziomu kontrolera lub metody. Ustawia wersję interfejsu API, którą kontroler lub metoda działania zaakceptuje. Może być użyty więcej niż raz, aby wskazać więcej niż jedną akceptowalną wersje. Wersje w atrybucie ApiVersion są wykrywalne. |
| ApiVersionNeutral    | Atrybut który działa na poziomie kontrolera, sprawia że ten rezygnuje z wersjonowania. Zapytanie o wersję ciągu znaków są ignorowane. Wszelkie poprawnie sformułowane adresy URL podczas korzystania z wersjonowania adresów URL będą akceptowane. |
| MapToApiVersion      | Atrybut który działa na poziome metody, odwzorowuje działanie na określoną wersję, gdy jest wiele wersje są określane na poziomie kontrolera. Wersje w atrybucie MapToApiVersion nie są wykrywalne. |
| AdvertiseApiVersions | Ogłasza dodatkowe wersje poza tym, co jest zawarte w wystąpieniu aplikacji. Używane, gdy wersje API są podzielone na wdrożenia, a informacje API nie mogą być agregowane za pomocą Eksploratora wersji interfejsu API |

#
## Przykład użycia {id="example"}

Jak już wiemy, API wersjonujemy poprzez atrybuty, te natomiast mają się odnosić do kontrolera lub bezpośrednio do metody. Niżej przedstawiam kawałek kodu z wykorzystaniem przykładowych atrybutów.

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

Klient chcąc korzystać z funkcjonalności wersjonowanego API, musi uwzględnić to w swoich rządaniach.

#### Obsługiwane są dwie opcje określania wersji żądania REST API:

  {{< tabgroup align="right" style="code" >}}
    {{< tab name="Osadzone w ścieżce adresu URL" >}}

  ```Osadzone w ścieżce adresu URL
  https://api.some-url.pl/v1.0/products/users
  ```

    {{< /tab >}}
    {{< tab name="Jako parametr ciągu zapytania adresu URL" >}}

  ```Jako parametr ciągu zapytania adresu URL
  https://api.some-url.pl/products/users?api-version=1.0
  ```

    {{< /tab >}}
  {{< /tabgroup >}}


######
## Podsumowanie {id="summary"}

Tu kończy się temat wersjonowania API, jednak po przeczytaniu artykułu zastanawiać się możesz czy jeżeli wizualizujemy zasoby, np za pomocą Swagger'a, to czy jego też możemy wersjonować? <!--more--> 
Zerknij w [Wersjonowanie Swagger'a](/pl/blog/swagger-versioning), pokaże Ci jak można to zrobić.
