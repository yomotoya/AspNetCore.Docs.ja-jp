---
title: ASP.NET Core に razor ページの承認規則
author: guardrex
description: ユーザーを承認して、匿名ユーザーがページやページのフォルダーにアクセスできるようにする規則を含むページへのアクセスを制御する方法を説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 35a21156c001d8703e09e604129c4c2c500fe25f
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734654"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="7d07b-103">ASP.NET Core に razor ページの承認規則</span><span class="sxs-lookup"><span data-stu-id="7d07b-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="7d07b-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7d07b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7d07b-105">Razor ページ アプリのアクセスを制御する方法の 1 つは起動時に承認規則を使用することです。</span><span class="sxs-lookup"><span data-stu-id="7d07b-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="7d07b-106">これらの規則を使用すると、ユーザーを承認して、個々 のページまたはページのフォルダーにアクセスする匿名ユーザーを許可できます。</span><span class="sxs-lookup"><span data-stu-id="7d07b-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="7d07b-107">自動的にこのトピックで説明する規則を適用[承認フィルター](xref:mvc/controllers/filters#authorization-filters)のアクセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="7d07b-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="7d07b-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="7d07b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7d07b-109">サンプル アプリは[ASP.NET Core Id なしの Cookie 認証](xref:security/authentication/cookie)です。</span><span class="sxs-lookup"><span data-stu-id="7d07b-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="7d07b-110">Maria ロドリゲス、仮想ユーザーのユーザー アカウントは、アプリにハードコードされたです。</span><span class="sxs-lookup"><span data-stu-id="7d07b-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="7d07b-111">電子メールのユーザー名を使用して"maria.rodriguez@contoso.com"とユーザーのサインインに任意のパスワード。</span><span class="sxs-lookup"><span data-stu-id="7d07b-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="7d07b-112">ユーザーが認証されて、`AuthenticateUser`メソッドで、 *Pages/Account/Login.cshtml.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7d07b-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="7d07b-113">実際の例では、ユーザーは、データベースに対する認証は。</span><span class="sxs-lookup"><span data-stu-id="7d07b-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="7d07b-114">ASP.NET Core の Id を使用するのガイダンスに従って、 [ASP.NET Core での Id の概要](xref:security/authentication/identity)トピックです。</span><span class="sxs-lookup"><span data-stu-id="7d07b-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="7d07b-115">概念と例をここに示すように、ASP.NET Core の Id を使用するアプリに同じように適用します。</span><span class="sxs-lookup"><span data-stu-id="7d07b-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="7d07b-116">ページにアクセスするための承認が必要</span><span class="sxs-lookup"><span data-stu-id="7d07b-116">Require authorization to access a page</span></span>

<span data-ttu-id="7d07b-117">使用して、 [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)指定したパスにページに。</span><span class="sxs-lookup"><span data-stu-id="7d07b-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="7d07b-118">指定されたパスは、ビュー エンジンのパス、これは、拡張機能とスラッシュのみを含むせず Razor ページ ルートの相対パスです。</span><span class="sxs-lookup"><span data-stu-id="7d07b-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="7d07b-119">[AuthorizePage オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="7d07b-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="7d07b-120">`AuthorizeFilter`ページ モデル クラスに適用できる、`[Authorize]`フィルター属性。</span><span class="sxs-lookup"><span data-stu-id="7d07b-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="7d07b-121">詳細については、次を参照してください。[承認フィルター属性](xref:mvc/razor-pages/filter#authorize-filter-attribute)です。</span><span class="sxs-lookup"><span data-stu-id="7d07b-121">For more information, see [Authorize filter attribute](xref:mvc/razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="7d07b-122">ページのフォルダーにアクセスするための承認が必要</span><span class="sxs-lookup"><span data-stu-id="7d07b-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="7d07b-123">使用して、 [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)のすべての指定したパスにフォルダー内のページに。</span><span class="sxs-lookup"><span data-stu-id="7d07b-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="7d07b-124">指定されたパスは、Razor ページのルートの相対パスは、ビュー エンジンのパスです。</span><span class="sxs-lookup"><span data-stu-id="7d07b-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="7d07b-125">[AuthorizeFolder オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="7d07b-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="7d07b-126">ページへの匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="7d07b-126">Allow anonymous access to a page</span></span>

<span data-ttu-id="7d07b-127">使用して、 [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)指定したパスにページに。</span><span class="sxs-lookup"><span data-stu-id="7d07b-127">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="7d07b-128">指定されたパスは、ビュー エンジンのパス、これは、拡張機能とスラッシュのみを含むせず Razor ページ ルートの相対パスです。</span><span class="sxs-lookup"><span data-stu-id="7d07b-128">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="7d07b-129">ページのフォルダーへの匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="7d07b-129">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="7d07b-130">使用して、 [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)のすべての指定したパスにフォルダー内のページに。</span><span class="sxs-lookup"><span data-stu-id="7d07b-130">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="7d07b-131">指定されたパスは、Razor ページのルートの相対パスは、ビュー エンジンのパスです。</span><span class="sxs-lookup"><span data-stu-id="7d07b-131">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="7d07b-132">承認済みと匿名アクセスを組み合わせることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="7d07b-132">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="7d07b-133">ページのフォルダーが承認を必要とし、そのフォルダー内のページが匿名アクセスを許可するように指定を指定することは完全に。</span><span class="sxs-lookup"><span data-stu-id="7d07b-133">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="7d07b-134">ただし、逆の場合は、true は。</span><span class="sxs-lookup"><span data-stu-id="7d07b-134">The reverse, however, isn't true.</span></span> <span data-ttu-id="7d07b-135">匿名アクセスのページのフォルダーを宣言し、承認の内のページを指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="7d07b-135">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="7d07b-136">[プライベート] ページの承認を必要とするため、機能しないときに両方、`AllowAnonymousFilter`と`AuthorizeFilter`、ページにフィルターを適用、 `AllowAnonymousFilter` wins し、アクセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="7d07b-136">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7d07b-137">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="7d07b-137">Additional resources</span></span>

* [<span data-ttu-id="7d07b-138">Razor ページのカスタム ルートとページ モデル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="7d07b-138">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-conventions)
* <span data-ttu-id="7d07b-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection)クラス</span><span class="sxs-lookup"><span data-stu-id="7d07b-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
