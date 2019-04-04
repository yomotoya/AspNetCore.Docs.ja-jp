---
title: ASP.NET Core で razor ページの承認規則
author: guardrex
description: ユーザーを承認して、匿名ユーザーがページまたはページのフォルダーにアクセスできるようにする規則を含むページへのアクセスを制御する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 040d33eba7eaf7a3aece2eedcdef7343e52972af
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345503"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="acdde-103">ASP.NET Core で razor ページの承認規則</span><span class="sxs-lookup"><span data-stu-id="acdde-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="acdde-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="acdde-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="acdde-105">Razor ページ アプリでのアクセスを制御する方法の 1 つでは、起動時に承認規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="acdde-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="acdde-106">これらの規則を使用すると、ユーザーを承認して、匿名ユーザーが個々 のページまたはページのフォルダーにアクセスできるようにできます。</span><span class="sxs-lookup"><span data-stu-id="acdde-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="acdde-107">自動的にこのトピックで説明されている規則が適用されます[承認フィルター](xref:mvc/controllers/filters#authorization-filters)のアクセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="acdde-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="acdde-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="acdde-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="acdde-109">サンプル アプリでは[ASP.NET Core Identity なしでの cookie 認証](xref:security/authentication/cookie)します。</span><span class="sxs-lookup"><span data-stu-id="acdde-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="acdde-110">概念とこのトピックで示す例については、ASP.NET Core Identity を使用するアプリに等しく適用されます。</span><span class="sxs-lookup"><span data-stu-id="acdde-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="acdde-111">ASP.NET Core Identity を使用するのガイダンスに従って<xref:security/authentication/identity>します。</span><span class="sxs-lookup"><span data-stu-id="acdde-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="acdde-112">ページにアクセスするための承認が必要です。</span><span class="sxs-lookup"><span data-stu-id="acdde-112">Require authorization to access a page</span></span>

<span data-ttu-id="acdde-113">使用して、<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*>規則を使用して<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>を追加する、<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>のページに、指定されたパス。</span><span class="sxs-lookup"><span data-stu-id="acdde-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="acdde-114">指定されたパスが、せず、拡張機能とスラッシュのみを含む Razor ページ ルートの相対パスは、ビュー エンジンのパス。</span><span class="sxs-lookup"><span data-stu-id="acdde-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="acdde-115">指定する、[承認ポリシー](xref:security/authorization/policies)を使用して、 [AuthorizePage オーバー ロード](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="acdde-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="acdde-116"><xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>とページ モデル クラスに適用できる、`[Authorize]`フィルター属性。</span><span class="sxs-lookup"><span data-stu-id="acdde-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="acdde-117">詳細については、[Authorize フィルター属性](xref:razor-pages/filter#authorize-filter-attribute)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="acdde-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="acdde-118">ページのフォルダーにアクセスするための承認が必要です。</span><span class="sxs-lookup"><span data-stu-id="acdde-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="acdde-119">使用して、<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*>規則を使用して<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>を追加する、<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>のすべての指定したパスにフォルダー内のページに。</span><span class="sxs-lookup"><span data-stu-id="acdde-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="acdde-120">指定されたパスが、Razor ページのルートの相対パスは、ビュー エンジンのパスです。</span><span class="sxs-lookup"><span data-stu-id="acdde-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="acdde-121">指定する、[承認ポリシー](xref:security/authorization/policies)を使用して、 [AuthorizeFolder オーバー ロード](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="acdde-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="acdde-122">領域をページにアクセスする承認を要求します。</span><span class="sxs-lookup"><span data-stu-id="acdde-122">Require authorization to access an area page</span></span>

<span data-ttu-id="acdde-123">使用して、<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*>規則を使用して<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>を追加する、<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>指定されたパスの領域のページに。</span><span class="sxs-lookup"><span data-stu-id="acdde-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="acdde-124">ページ名は、指定された領域のページのルート ディレクトリに対する相対拡張子のないファイルのパスです。</span><span class="sxs-lookup"><span data-stu-id="acdde-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="acdde-125">たとえば、ファイルのページ名*Areas/Identity/Pages/Manage/Accounts.cshtml*は */管理/アカウント*します。</span><span class="sxs-lookup"><span data-stu-id="acdde-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="acdde-126">指定する、[承認ポリシー](xref:security/authorization/policies)を使用して、 [AuthorizeAreaPage オーバー ロード](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="acdde-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="acdde-127">領域のフォルダーにアクセスするための承認が必要です。</span><span class="sxs-lookup"><span data-stu-id="acdde-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="acdde-128">使用して、<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*>規則を使用して<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>を追加する、<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>のすべての指定したパスにフォルダー内の領域に。</span><span class="sxs-lookup"><span data-stu-id="acdde-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="acdde-129">フォルダーのパスは、指定された領域のページのルート ディレクトリに対する相対フォルダーのパスです。</span><span class="sxs-lookup"><span data-stu-id="acdde-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="acdde-130">ファイルのフォルダー パスではたとえば、*領域/ユーザー/ページ/管理/* は */管理*します。</span><span class="sxs-lookup"><span data-stu-id="acdde-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="acdde-131">指定する、[承認ポリシー](xref:security/authorization/policies)を使用して、 [AuthorizeAreaFolder オーバー ロード](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="acdde-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="acdde-132">ページへの匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="acdde-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="acdde-133">使用して、<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*>規則を使用して<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>を追加する、<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>指定されたパスにページ。</span><span class="sxs-lookup"><span data-stu-id="acdde-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="acdde-134">指定されたパスが、せず、拡張機能とスラッシュのみを含む Razor ページ ルートの相対パスは、ビュー エンジンのパス。</span><span class="sxs-lookup"><span data-stu-id="acdde-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="acdde-135">ページのフォルダーへの匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="acdde-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="acdde-136">使用して、<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*>規則を使用して<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>を追加する、<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>のすべての指定したパスにフォルダー内のページに。</span><span class="sxs-lookup"><span data-stu-id="acdde-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="acdde-137">指定されたパスが、Razor ページのルートの相対パスは、ビュー エンジンのパスです。</span><span class="sxs-lookup"><span data-stu-id="acdde-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="acdde-138">承認済みおよび匿名アクセスを組み合わせることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="acdde-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="acdde-139">指定することは承認を必要とするページのフォルダーよりも、そのフォルダー内のページが匿名アクセスを許可することを指定するとします。</span><span class="sxs-lookup"><span data-stu-id="acdde-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="acdde-140">ただし、逆の場合が無効です。</span><span class="sxs-lookup"><span data-stu-id="acdde-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="acdde-141">匿名アクセスのページのフォルダーを宣言し、承認を必要とするそのフォルダー内のページを指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="acdde-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="acdde-142">[プライベート] ページが必要な承認が失敗します。</span><span class="sxs-lookup"><span data-stu-id="acdde-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="acdde-143">ときに両方の<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>と<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>、ページに適用されます、<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>が優先され、アクセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="acdde-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="acdde-144">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="acdde-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
