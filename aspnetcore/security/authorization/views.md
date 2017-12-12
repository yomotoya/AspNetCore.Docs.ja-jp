---
title: "ASP.NET Core MVC でのビューに基づく承認"
author: rick-anderson
description: "このドキュメントでは、挿入および ASP.NET Core Razor ビュー内で承認サービスを利用する方法を示します。"
keywords: "ASP.NET Core、承認、IAuthorizationService、Razor の承認"
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 756431f398c29376ab0ecd6c4f4d1db4f8022b0b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="view-based-authorization"></a><span data-ttu-id="b6ad2-104">ビューに基づく承認</span><span class="sxs-lookup"><span data-stu-id="b6ad2-104">View-based authorization</span></span>

<span data-ttu-id="b6ad2-105">多くの場合、開発者を表示、非表示、またはそれ以外の場合、現在のユーザー id に基づいて UI を変更したいです。</span><span class="sxs-lookup"><span data-stu-id="b6ad2-105">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="b6ad2-106">使用して MVC ビューの中で承認サービスにアクセスすることができます[依存性の注入](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="b6ad2-106">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="b6ad2-107">Razor ビューには、承認サービスを挿入を使用して、`@inject`ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="b6ad2-107">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="b6ad2-108">すべてのビューで、承認サービスを実行する場合に、配置、`@inject`にディレクティブ、 *_ViewImports.cshtml*のファイル、*ビュー*ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="b6ad2-108">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="b6ad2-109">詳細については、次を参照してください。[ビューに依存性の注入](xref:mvc/views/dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="b6ad2-109">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="b6ad2-110">挿入された承認サービスを使用して呼び出す`AuthorizeAsync`ことを確認中にまったく同じ方法で[リソース ベースの承認](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="b6ad2-110">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6ad2-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6ad2-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6ad2-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6ad2-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="b6ad2-113">場合によっては、リソースは、モデルの表示になります。</span><span class="sxs-lookup"><span data-stu-id="b6ad2-113">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="b6ad2-114">呼び出す`AuthorizeAsync`ことを確認中にまったく同じ方法で[リソース ベースの承認](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="b6ad2-114">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6ad2-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6ad2-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6ad2-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6ad2-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="b6ad2-117">上記のコードで、モデルは、考慮に入れてポリシーの評価が実行をリソースとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="b6ad2-117">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="b6ad2-118">アプリの UI 要素の唯一の承認チェックとの切り替えの可視性に依存しないようにします。</span><span class="sxs-lookup"><span data-stu-id="b6ad2-118">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="b6ad2-119">UI 要素を非表示にする可能性がありますいないを完全に防ぐアクセスその関連付けられたコント ローラー アクションにします。</span><span class="sxs-lookup"><span data-stu-id="b6ad2-119">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="b6ad2-120">たとえば、上記のコード スニペットのボタンがあるとします。</span><span class="sxs-lookup"><span data-stu-id="b6ad2-120">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="b6ad2-121">ユーザーが呼び出すことができます、`Edit`相対的なリソースを知っている場合、アクション メソッドの URL は*/Document/Edit/1*です。</span><span class="sxs-lookup"><span data-stu-id="b6ad2-121">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="b6ad2-122">このため、`Edit`アクション メソッドが独自の承認チェックを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b6ad2-122">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
