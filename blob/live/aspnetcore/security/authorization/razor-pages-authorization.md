---
title: "ASP.NET Core に razor ページの承認規則"
author: guardrex
description: "起動時にユーザーを承認して、個々 のページまたはページのフォルダーにアクセスする匿名ユーザーを許可する規則にページへのアクセスを制御する方法を説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/27/2017
ms.topic: article
ms.assetid: f65ad22d-9472-478a-856c-c59c8681fa71
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 36acf3c06a462882972c5f389d544d98cadc35f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="55f0f-103">ASP.NET Core に razor ページの承認規則</span><span class="sxs-lookup"><span data-stu-id="55f0f-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="55f0f-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="55f0f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="55f0f-105">Razor ページ アプリのアクセスを制御する方法の 1 つは起動時に承認規則を使用することです。</span><span class="sxs-lookup"><span data-stu-id="55f0f-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="55f0f-106">これらの規則を使用すると、ユーザーを承認して、個々 のページまたはページのフォルダーにアクセスする匿名ユーザーを許可できます。</span><span class="sxs-lookup"><span data-stu-id="55f0f-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="55f0f-107">自動的にこのトピックで説明する規則を適用[承認フィルター](xref:mvc/controllers/filters#authorization-filters)のアクセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="55f0f-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="55f0f-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="55f0f-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="55f0f-109">ページにアクセスするための承認が必要</span><span class="sxs-lookup"><span data-stu-id="55f0f-109">Require authorization to access a page</span></span>

<span data-ttu-id="55f0f-110">使用して、 [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)指定したパスにページに。</span><span class="sxs-lookup"><span data-stu-id="55f0f-110">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="55f0f-111">指定されたパスは、ビュー エンジンのパス、これは、拡張機能とスラッシュのみを含むせず Razor ページ ルートの相対パスです。</span><span class="sxs-lookup"><span data-stu-id="55f0f-111">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="55f0f-112">[AuthorizePage オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="55f0f-112">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="55f0f-113">ページのフォルダーにアクセスするための承認が必要</span><span class="sxs-lookup"><span data-stu-id="55f0f-113">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="55f0f-114">使用して、 [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)のすべての指定したパスにフォルダー内のページに。</span><span class="sxs-lookup"><span data-stu-id="55f0f-114">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="55f0f-115">指定されたパスは、Razor ページのルートの相対パスは、ビュー エンジンのパスです。</span><span class="sxs-lookup"><span data-stu-id="55f0f-115">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="55f0f-116">[AuthorizeFolder オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="55f0f-116">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="55f0f-117">ページへの匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="55f0f-117">Allow anonymous access to a page</span></span>

<span data-ttu-id="55f0f-118">使用して、 [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)指定したパスにページに。</span><span class="sxs-lookup"><span data-stu-id="55f0f-118">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="55f0f-119">指定されたパスは、ビュー エンジンのパス、これは、拡張機能とスラッシュのみを含むせず Razor ページ ルートの相対パスです。</span><span class="sxs-lookup"><span data-stu-id="55f0f-119">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="55f0f-120">ページのフォルダーへの匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="55f0f-120">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="55f0f-121">使用して、 [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)のすべての指定したパスにフォルダー内のページに。</span><span class="sxs-lookup"><span data-stu-id="55f0f-121">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="55f0f-122">指定されたパスは、Razor ページのルートの相対パスは、ビュー エンジンのパスです。</span><span class="sxs-lookup"><span data-stu-id="55f0f-122">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="55f0f-123">承認済みと匿名アクセスを組み合わせることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="55f0f-123">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="55f0f-124">ページのフォルダーが承認を必要とし、そのフォルダー内のページが匿名アクセスを許可するように指定を指定することは完全に。</span><span class="sxs-lookup"><span data-stu-id="55f0f-124">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="55f0f-125">ただし、逆の場合は、true は。</span><span class="sxs-lookup"><span data-stu-id="55f0f-125">The reverse, however, isn't true.</span></span> <span data-ttu-id="55f0f-126">匿名アクセスのページのフォルダーを宣言し、承認の内のページを指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="55f0f-126">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="55f0f-127">[プライベート] ページの承認を必要とするため、機能しないときに両方、`AllowAnonymousFilter`と`AuthorizeFilter`、ページにフィルターを適用、 `AllowAnonymousFilter` wins し、アクセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="55f0f-127">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="see-also"></a><span data-ttu-id="55f0f-128">関連項目</span><span class="sxs-lookup"><span data-stu-id="55f0f-128">See also</span></span>

* [<span data-ttu-id="55f0f-129">Razor ページ カスタム ルートしてページ モデル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="55f0f-129">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* <span data-ttu-id="55f0f-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection)クラス</span><span class="sxs-lookup"><span data-stu-id="55f0f-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
