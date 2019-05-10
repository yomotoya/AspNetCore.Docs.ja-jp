---
title: ASP.NET Core MVC でビュー ベースの承認
author: rick-anderson
description: このドキュメントを挿入して、ASP.NET Core の Razor ビューの内部で承認サービスを使用する方法を示します。
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: e497c41d4dca29fed8733f18cf727804e3f06d8c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892059"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="72e6e-103">ASP.NET Core MVC でビュー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="72e6e-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="72e6e-104">開発者は、表示、非表示にする、またはそれ以外の場合、現在のユーザー id に基づく UI を変更する多くの場合は。</span><span class="sxs-lookup"><span data-stu-id="72e6e-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="72e6e-105">使用して MVC ビューの中で承認サービスにアクセスできる[依存関係の注入](xref:fundamentals/dependency-injection)します。</span><span class="sxs-lookup"><span data-stu-id="72e6e-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="72e6e-106">承認サービスを Razor ビューに挿入を使用して、`@inject`ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="72e6e-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="72e6e-107">すべてのビューで承認サービスを実行する場合に、配置、`@inject`にディレクティブ、 *_ViewImports.cshtml*のファイル、*ビュー*ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="72e6e-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="72e6e-108">詳しくは、「[ビューへの依存関係の挿入](xref:mvc/views/dependency-injection)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="72e6e-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="72e6e-109">挿入された承認サービスを使用して呼び出す`AuthorizeAsync`ことを確認中にまったく同じ方法で[リソース ベースの承認](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="72e6e-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="72e6e-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="72e6e-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="72e6e-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="72e6e-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="72e6e-112">場合によっては、リソースは、ビュー モデルになります。</span><span class="sxs-lookup"><span data-stu-id="72e6e-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="72e6e-113">呼び出す`AuthorizeAsync`ことを確認中にまったく同じ方法で[リソース ベースの承認](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="72e6e-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="72e6e-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="72e6e-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="72e6e-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="72e6e-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="72e6e-116">上記のコードで考慮のポリシーの評価を実行する必要がありますをリソースとしてモデルに渡されます。</span><span class="sxs-lookup"><span data-stu-id="72e6e-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="72e6e-117">唯一の承認チェックとして、アプリの UI 要素の切り替えの可視性に依存しないようにします。</span><span class="sxs-lookup"><span data-stu-id="72e6e-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="72e6e-118">UI 要素を非表示できない可能性がありますいない完全にアクセス、関連付けられたコント ローラー アクションにします。</span><span class="sxs-lookup"><span data-stu-id="72e6e-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="72e6e-119">たとえば、上記のコード スニペットで、ボタンがあるとします。</span><span class="sxs-lookup"><span data-stu-id="72e6e-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="72e6e-120">ユーザーが呼び出すことができます、`Edit`相対リソースを知っている場合、アクション メソッドの URL が */Document/Edit/1*します。</span><span class="sxs-lookup"><span data-stu-id="72e6e-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="72e6e-121">このため、`Edit`アクション メソッドは、独自の承認チェックを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="72e6e-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
