---
title: "ビュー ベースの承認"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 82c0c7282de34e496f529d964f99121ae2805c5a
ms.sourcegitcommit: e3b1726cc04e80dc28464c35259edbd3bc39a438
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/12/2017
---
# <a name="view-based-authorization"></a>ビュー ベースの承認

<a name=security-authorization-views></a>

多くの場合、開発者は、それ以外の場合、現在のユーザー id に基づいて UI を変更または非表示にすることができます。 使用して MVC ビューの中で承認サービスにアクセスすることができます[依存性の注入](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)です。 Razor ビューの使用に承認サービスを挿入する、`@inject`例については、ディレクティブ`@inject IAuthorizationService AuthorizationService`(必要`@using Microsoft.AspNetCore.Authorization`)。 場合はすべてのビューで、承認サービスを配置し、`@inject`にディレクティブ、`_ViewImports.cshtml`ファイルで、`Views`ディレクトリ。 ビューに依存関係の挿入の詳細については、次を参照してください。[ビューに依存性の注入](../../mvc/views/dependency-injection.md)です。

呼び出して使用する認証サービスを挿入した後、`AuthorizeAsync`メソッド中にチェックインする方法とまったく同じで[リソース ベースの承認](resourcebased.md#security-authorization-resource-based-imperative)です。

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
   {
       <p>This paragraph is displayed because you fulfilled PolicyName.</p>
   }
   ```

場合によっては、されるリソースは、モデルを表示し、呼び出すことができます`AuthorizeAsync`中にチェックインする方法とまったく同じで[リソース ベースの承認](resourcebased.md#security-authorization-resource-based-imperative)です。

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

ここで、リソースの承認を考慮に入れておく必要があります、モデルに渡されますを表示できます。

>[!WARNING]
>表示または非表示、唯一の認証方法として、UI の部分に依存しません。 UI を非表示にする要素ないわけでは、ユーザーがアクセスできません。 コント ローラー コード内でユーザーを許可する必要があります。
