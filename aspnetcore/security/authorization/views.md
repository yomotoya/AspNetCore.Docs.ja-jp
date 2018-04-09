---
title: ASP.NET Core MVC でのビューに基づく承認
author: rick-anderson
description: このドキュメントでは、挿入および ASP.NET Core Razor ビュー内で承認サービスを利用する方法を示します。
manager: wpickett
ms.author: riande
ms.date: 10/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/views
ms.openlocfilehash: dad59a297efb4648755436fbd07742f95af97fb2
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>ASP.NET Core MVC でのビューに基づく承認

多くの場合、開発者を表示、非表示、またはそれ以外の場合、現在のユーザー id に基づいて UI を変更したいです。 使用して MVC ビューの中で承認サービスにアクセスすることができます[依存性の注入](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)です。 Razor ビューには、承認サービスを挿入を使用して、`@inject`ディレクティブ。

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

すべてのビューで、承認サービスを実行する場合に、配置、`@inject`にディレクティブ、 *_ViewImports.cshtml*のファイル、*ビュー*ディレクトリ。 詳しくは、「[ビューへの依存関係の挿入](xref:mvc/views/dependency-injection)」をご覧ください。

挿入された承認サービスを使用して呼び出す`AuthorizeAsync`ことを確認中にまったく同じ方法で[リソース ベースの承認](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

場合によっては、リソースは、モデルの表示になります。 呼び出す`AuthorizeAsync`ことを確認中にまったく同じ方法で[リソース ベースの承認](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

上記のコードで、モデルは、考慮に入れてポリシーの評価が実行をリソースとして渡されます。

> [!WARNING]
> アプリの UI 要素の唯一の承認チェックとの切り替えの可視性に依存しないようにします。 UI 要素を非表示にする可能性がありますいないを完全に防ぐアクセスその関連付けられたコント ローラー アクションにします。 たとえば、上記のコード スニペットのボタンがあるとします。 ユーザーが呼び出すことができます、`Edit`相対的なリソースを知っている場合、アクション メソッドの URL は*/Document/Edit/1*です。 このため、`Edit`アクション メソッドが独自の承認チェックを実行する必要があります。
