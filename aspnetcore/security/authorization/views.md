---
title: "ビュー ベースの承認"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 3b7fa6025d766da80ba92ee27af20bf9bfe0dcf4
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="view-based-authorization"></a><span data-ttu-id="27e85-103">ビュー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="27e85-103">View Based Authorization</span></span>

<a name=security-authorization-views></a>

<span data-ttu-id="27e85-104">多くの場合、開発者は、それ以外の場合、現在のユーザー id に基づいて UI を変更または非表示にすることができます。</span><span class="sxs-lookup"><span data-stu-id="27e85-104">Often a developer will want to show, hide or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="27e85-105">使用して MVC ビューの中で承認サービスにアクセスすることができます[依存性の注入](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="27e85-105">You can access the authorization service within MVC views via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection).</span></span> <span data-ttu-id="27e85-106">Razor ビューの使用に承認サービスを挿入する、`@inject`例については、ディレクティブ`@inject IAuthorizationService AuthorizationService`(必要`@using Microsoft.AspNetCore.Authorization`)。</span><span class="sxs-lookup"><span data-stu-id="27e85-106">To inject the authorization service into a Razor view use the `@inject` directive, for example `@inject IAuthorizationService AuthorizationService` (requires `@using Microsoft.AspNetCore.Authorization`).</span></span> <span data-ttu-id="27e85-107">場合はすべてのビューで、承認サービスを配置し、`@inject`にディレクティブ、`_ViewImports.cshtml`ファイルで、`Views`ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="27e85-107">If you want the authorization service in every view then place the `@inject` directive into the `_ViewImports.cshtml` file in the `Views` directory.</span></span> <span data-ttu-id="27e85-108">ビューに依存関係の挿入の詳細については、次を参照してください。[ビューに依存性の注入](../../mvc/views/dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="27e85-108">For more information on dependency injection into views see [Dependency injection into views](../../mvc/views/dependency-injection.md).</span></span>

<span data-ttu-id="27e85-109">呼び出して使用する認証サービスを挿入した後、`AuthorizeAsync`メソッド中にチェックインする方法とまったく同じで[リソース ベースの承認](resourcebased.md#security-authorization-resource-based-imperative)です。</span><span class="sxs-lookup"><span data-stu-id="27e85-109">Once you have injected the authorization service you use it by calling the `AuthorizeAsync` method in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative).</span></span>

```csharp
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
   {
       <p>This paragraph is displayed because you fulfilled PolicyName.</p>
   }
   ```

<span data-ttu-id="27e85-110">場合によっては、されるリソースは、モデルを表示し、呼び出すことができます`AuthorizeAsync`中にチェックインする方法とまったく同じで[リソース ベースの承認](resourcebased.md#security-authorization-resource-based-imperative)です。</span><span class="sxs-lookup"><span data-stu-id="27e85-110">In some cases the resource will be your view model, and you can call `AuthorizeAsync` in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative);</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="27e85-111">ASP.NET 2.x のコア</span><span class="sxs-lookup"><span data-stu-id="27e85-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
   @if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="27e85-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="27e85-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
   @if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```
---

<span data-ttu-id="27e85-113">ここで、リソースの承認を考慮に入れておく必要があります、モデルに渡されますを表示できます。</span><span class="sxs-lookup"><span data-stu-id="27e85-113">Here you can see the model is passed as the resource authorization should take into consideration.</span></span>

>[!WARNING]
><span data-ttu-id="27e85-114">表示または非表示、唯一の認証方法として、UI の部分に依存しません。</span><span class="sxs-lookup"><span data-stu-id="27e85-114">Do not rely on showing or hiding parts of your UI as your only authorization method.</span></span> <span data-ttu-id="27e85-115">UI を非表示にする要素ないわけでは、ユーザーがアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="27e85-115">Hiding a UI element does not mean a user cannot access it.</span></span> <span data-ttu-id="27e85-116">コント ローラー コード内でユーザーを許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="27e85-116">You must also authorize the user within your controller code.</span></span>
