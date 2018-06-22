---
title: ASP.NET Core に razor ページの承認規則
author: guardrex
description: ユーザーを承認して、匿名ユーザーがページやページのフォルダーにアクセスできるようにする規則を含むページへのアクセスを制御する方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 8856520bf43f2f62cc12c7e883485babdb43fb3e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272676"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="a85f0-103">ASP.NET Core に razor ページの承認規則</span><span class="sxs-lookup"><span data-stu-id="a85f0-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="a85f0-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a85f0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a85f0-105">Razor ページ アプリのアクセスを制御する方法の 1 つは起動時に承認規則を使用することです。</span><span class="sxs-lookup"><span data-stu-id="a85f0-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="a85f0-106">これらの規則を使用すると、ユーザーを承認して、個々 のページまたはページのフォルダーにアクセスする匿名ユーザーを許可できます。</span><span class="sxs-lookup"><span data-stu-id="a85f0-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="a85f0-107">自動的にこのトピックで説明する規則を適用[承認フィルター](xref:mvc/controllers/filters#authorization-filters)のアクセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="a85f0-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="a85f0-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="a85f0-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a85f0-109">サンプル アプリは[ASP.NET Core Id なしの Cookie 認証](xref:security/authentication/cookie)です。</span><span class="sxs-lookup"><span data-stu-id="a85f0-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="a85f0-110">Maria ロドリゲス、仮想ユーザーのユーザー アカウントは、アプリにハードコードされたです。</span><span class="sxs-lookup"><span data-stu-id="a85f0-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="a85f0-111">電子メールのユーザー名を使用して"maria.rodriguez@contoso.com"とユーザーのサインインに任意のパスワード。</span><span class="sxs-lookup"><span data-stu-id="a85f0-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="a85f0-112">ユーザーが認証されて、`AuthenticateUser`メソッドで、 *Pages/Account/Login.cshtml.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a85f0-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="a85f0-113">実際の例では、ユーザーは、データベースに対する認証は。</span><span class="sxs-lookup"><span data-stu-id="a85f0-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="a85f0-114">ASP.NET Core の Id を使用するのガイダンスに従って、 [ASP.NET Core での Id の概要](xref:security/authentication/identity)トピックです。</span><span class="sxs-lookup"><span data-stu-id="a85f0-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="a85f0-115">概念と例をここに示すように、ASP.NET Core の Id を使用するアプリに同じように適用します。</span><span class="sxs-lookup"><span data-stu-id="a85f0-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="a85f0-116">ページにアクセスするための承認が必要</span><span class="sxs-lookup"><span data-stu-id="a85f0-116">Require authorization to access a page</span></span>

<span data-ttu-id="a85f0-117">使用して、 [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)指定したパスにページに。</span><span class="sxs-lookup"><span data-stu-id="a85f0-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="a85f0-118">指定されたパスは、ビュー エンジンのパス、これは、拡張機能とスラッシュのみを含むせず Razor ページ ルートの相対パスです。</span><span class="sxs-lookup"><span data-stu-id="a85f0-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="a85f0-119">[AuthorizePage オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="a85f0-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="a85f0-120">`AuthorizeFilter`ページ モデル クラスに適用できる、`[Authorize]`フィルター属性。</span><span class="sxs-lookup"><span data-stu-id="a85f0-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="a85f0-121">詳細については、次を参照してください。[承認フィルター属性](xref:razor-pages/filter#authorize-filter-attribute)です。</span><span class="sxs-lookup"><span data-stu-id="a85f0-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="a85f0-122">ページのフォルダーにアクセスするための承認が必要</span><span class="sxs-lookup"><span data-stu-id="a85f0-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="a85f0-123">使用して、 [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)のすべての指定したパスにフォルダー内のページに。</span><span class="sxs-lookup"><span data-stu-id="a85f0-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="a85f0-124">指定されたパスは、Razor ページのルートの相対パスは、ビュー エンジンのパスです。</span><span class="sxs-lookup"><span data-stu-id="a85f0-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="a85f0-125">[AuthorizeFolder オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="a85f0-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="a85f0-126">ページへの匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="a85f0-126">Allow anonymous access to a page</span></span>

<span data-ttu-id="a85f0-127">使用して、 [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)指定したパスにページに。</span><span class="sxs-lookup"><span data-stu-id="a85f0-127">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="a85f0-128">指定されたパスは、ビュー エンジンのパス、これは、拡張機能とスラッシュのみを含むせず Razor ページ ルートの相対パスです。</span><span class="sxs-lookup"><span data-stu-id="a85f0-128">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="a85f0-129">ページのフォルダーへの匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="a85f0-129">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="a85f0-130">使用して、 [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)のすべての指定したパスにフォルダー内のページに。</span><span class="sxs-lookup"><span data-stu-id="a85f0-130">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="a85f0-131">指定されたパスは、Razor ページのルートの相対パスは、ビュー エンジンのパスです。</span><span class="sxs-lookup"><span data-stu-id="a85f0-131">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="a85f0-132">承認済みと匿名アクセスを組み合わせることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a85f0-132">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="a85f0-133">ページのフォルダーが承認を必要とし、そのフォルダー内のページが匿名アクセスを許可するように指定を指定することは完全に。</span><span class="sxs-lookup"><span data-stu-id="a85f0-133">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="a85f0-134">ただし、逆の場合は、true は。</span><span class="sxs-lookup"><span data-stu-id="a85f0-134">The reverse, however, isn't true.</span></span> <span data-ttu-id="a85f0-135">匿名アクセスのページのフォルダーを宣言し、承認の内のページを指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="a85f0-135">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="a85f0-136">[プライベート] ページの承認を必要とするため、機能しないときに両方、`AllowAnonymousFilter`と`AuthorizeFilter`、ページにフィルターを適用、 `AllowAnonymousFilter` wins し、アクセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="a85f0-136">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a85f0-137">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="a85f0-137">Additional resources</span></span>

* [<span data-ttu-id="a85f0-138">Razor ページのカスタム ルートとページ モデル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="a85f0-138">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="a85f0-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection)クラス</span><span class="sxs-lookup"><span data-stu-id="a85f0-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
