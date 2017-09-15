---
title: "検索の追加"
author: rick-anderson
description: "単純な ASP.NET Core MVC アプリに検索を追加する方法を紹介します"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-ffff-4628-855d-200206d96269
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: 57419c697aea80a054e906c75002f5a39c512fc6
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="b112d-104">注: SQLlite では、大文字と小文字が区別されます。そのため、"ghost" ではなく、"Ghost" で検索する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b112d-104">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="b112d-105">*Views\movie\Index.cshtml* Razor ビューの `<form>` タグを変更し、`method="get"` を指定します。</span><span class="sxs-lookup"><span data-stu-id="b112d-105">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="b112d-106">[前 - コントローラーのメソッドとビュー](controller-methods-views.md)
[次 - フィールドの追加](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="b112d-106">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>  
