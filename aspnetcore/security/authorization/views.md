---
title: ASP.NET Core MVC でビュー ベースの承認
author: rick-anderson
description: このドキュメントを挿入して、ASP.NET Core の Razor ビューの内部で承認サービスを使用する方法を示します。
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: e497c41d4dca29fed8733f18cf727804e3f06d8c
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342537"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>ASP.NET Core MVC でビュー ベースの承認

開発者は、表示、非表示にする、またはそれ以外の場合、現在のユーザー id に基づく UI を変更する多くの場合は。 使用して MVC ビューの中で承認サービスにアクセスできる[依存関係の注入](xref:fundamentals/dependency-injection)します。 承認サービスを Razor ビューに挿入を使用して、`@inject`ディレクティブ。

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

すべてのビューで承認サービスを実行する場合に、配置、`@inject`にディレクティブ、 *_ViewImports.cshtml*のファイル、*ビュー*ディレクトリ。 詳しくは、「[ビューへの依存関係の挿入](xref:mvc/views/dependency-injection)」をご覧ください。

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

場合によっては、リソースは、ビュー モデルになります。 呼び出す`AuthorizeAsync`ことを確認中にまったく同じ方法で[リソース ベースの承認](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

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

上記のコードで考慮のポリシーの評価を実行する必要がありますをリソースとしてモデルに渡されます。

> [!WARNING]
> 唯一の承認チェックとして、アプリの UI 要素の切り替えの可視性に依存しないようにします。 UI 要素を非表示できない可能性がありますいない完全にアクセス、関連付けられたコント ローラー アクションにします。 たとえば、上記のコード スニペットで、ボタンがあるとします。 ユーザーが呼び出すことができます、`Edit`相対リソースを知っている場合、アクション メソッドの URL が */Document/Edit/1*します。 このため、`Edit`アクション メソッドは、独自の承認チェックを実行する必要があります。
