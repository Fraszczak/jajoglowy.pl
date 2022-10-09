---
title: Wersjonowanie API
description: Artykuł poruszający temat wersjonowania API
date: 2022-10-08
aliases: [api-versioning, api, wersjonowanie API]
author: Piotr Fraszczak
url: blog/api-versioning
---

### First things first

Temat wersjonowania API warto zacząć od tego [Czym jest API](/blog/api-c#). Gdy już wiem jak działa, wiemy też, że istnieje żeby umożliwić wymianę danych, komunikację.
Gdy interfejs użytkownika się zmieni, w najlepszym razie użytkownicy muszą się do niego ponownie przyzwyczaić. Gdy zmienia się API, a program klienta nie jest przygotowany na zmiany, kończy działanie błędem. <!--more-->
Gdy API jest publiczne, prawdopodobnie ma więcej niż jedną aplikacje kliencką. Zmiany w kodzie możemy wersjonować, dlaczego by więc nie wersjonować całego API?

### Wytyczne

Wszystkie interfejsy API zgodne z wytycznymi [Microsoft REST API ](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md) MUSZĄ obsługiwać jawne przechowywanie wersji. Niezwykle ważne jest, aby klienci mogli liczyć na to, że usługi będą stabilne w czasie, a usługi mogą dodawać funkcje i wprowadzać zmiany.

Usługi są wersjonowane przy użyciu schematu wersjonowania Major.Minor. Usługi MOGĄ wybrać schemat wersji tylko „główny”, w którym to przypadku dorozumiane jest „.0” i obowiązują wszystkie inne zasady opisane w tej sekcji.

### Obsługiwane są dwie opcje określania wersji żądania REST API:

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
