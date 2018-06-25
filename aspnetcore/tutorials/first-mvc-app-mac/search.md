---
title: ASP.NET Core MVC アプリへの検索の追加
author: rick-anderson
description: 単純な ASP.NET Core MVC アプリに検索を追加する方法を紹介します
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 4175d4dfd03d173f7025aff3b51d255bb1c213ee
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274483"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

注: SQLlite では、大文字と小文字が区別されます。そのため、"ghost" ではなく、"Ghost" で検索する必要があります。

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

*Views\movie\Index.cshtml* Razor ビューの `<form>` タグを変更し、`method="get"` を指定します。

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [前 - コントローラーのメソッドとビュー](controller-methods-views.md)
> [次 - フィールドの追加](new-field.md)
