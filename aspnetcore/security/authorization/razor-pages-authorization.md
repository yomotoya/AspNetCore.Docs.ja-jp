---
title: ASP.NET Core で razor ページの承認規則
author: guardrex
description: ユーザーを承認して、匿名ユーザーがページまたはページのフォルダーにアクセスできるようにする規則を含むページへのアクセスを制御する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: d3ecb41765da912df68aeb829350d27e4d087e3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829923"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="8b3c3-103">ASP.NET Core で razor ページの承認規則</span><span class="sxs-lookup"><span data-stu-id="8b3c3-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="8b3c3-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8b3c3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8b3c3-105">Razor ページ アプリでのアクセスを制御する方法の 1 つでは、起動時に承認規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="8b3c3-106">これらの規則を使用すると、ユーザーを承認して、匿名ユーザーが個々 のページまたはページのフォルダーにアクセスできるようにできます。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="8b3c3-107">自動的にこのトピックで説明されている規則が適用されます[承認フィルター](xref:mvc/controllers/filters#authorization-filters)のアクセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="8b3c3-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8b3c3-109">サンプル アプリでは[ASP.NET Core Identity なしでの Cookie 認証](xref:security/authentication/cookie)します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="8b3c3-110">Maria Rodriguez、架空のユーザーのユーザー アカウント、アプリにハードコードしています。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="8b3c3-111">電子メールのユーザー名を使用して、"maria.rodriguez@contoso.com"と、ユーザーがサインインするすべてのパスワード。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="8b3c3-112">における、ユーザーの認証、`AuthenticateUser`メソッドで、 *Pages/Account/Login.cshtml.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="8b3c3-113">実際の例では、ユーザーは、データベースに対して認証は。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="8b3c3-114">ASP.NET Core Identity を使用するのガイダンスに従って、[の ASP.NET core Identity の概要](xref:security/authentication/identity)トピック。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="8b3c3-115">概念とこのトピックで示す例については、ASP.NET Core Identity を使用するアプリに等しく適用されます。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="8b3c3-116">ページにアクセスするための承認が必要です。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-116">Require authorization to access a page</span></span>

<span data-ttu-id="8b3c3-117">使用して、 [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)規則を使用して[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)のページに、指定されたパス。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="8b3c3-118">指定されたパスが、せず、拡張機能とスラッシュのみを含む Razor ページ ルートの相対パスは、ビュー エンジンのパス。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="8b3c3-119">[AuthorizePage オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="8b3c3-120">`AuthorizeFilter`とページ モデル クラスに適用できる、`[Authorize]`フィルター属性。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="8b3c3-121">詳細については、次を参照してください。 [Authorize フィルター属性](xref:razor-pages/filter#authorize-filter-attribute)します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="8b3c3-122">ページのフォルダーにアクセスするための承認が必要です。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="8b3c3-123">使用して、 [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)規則を使用して[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)のすべての指定したパスにフォルダー内のページに。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="8b3c3-124">指定されたパスが、Razor ページのルートの相対パスは、ビュー エンジンのパスです。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="8b3c3-125">[AuthorizeFolder オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="8b3c3-126">領域をページにアクセスする承認を要求します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-126">Require authorization to access an area page</span></span>

<span data-ttu-id="8b3c3-127">使用して、 [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage)規則を使用して[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)指定されたパスの領域のページに。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-127">Use the [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="8b3c3-128">ページ名は、指定された領域のページのルート ディレクトリに対する相対拡張子のないファイルのパスです。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-128">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="8b3c3-129">たとえば、ファイルのページ名*Areas/Identity/Pages/Manage/Accounts.cshtml*は */管理/アカウント*します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-129">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="8b3c3-130">[AuthorizeAreaPage オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-130">An [AuthorizeAreaPage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="8b3c3-131">領域のフォルダーにアクセスするための承認が必要です。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-131">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="8b3c3-132">使用して、 [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder)規則を使用して[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)のすべての指定したパスにフォルダー内の領域に。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-132">Use the [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="8b3c3-133">フォルダーのパスは、指定された領域のページのルート ディレクトリに対する相対フォルダーのパスです。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-133">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="8b3c3-134">ファイルのフォルダー パスではたとえば、*領域/ユーザー/ページ/管理/* は */管理*します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-134">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="8b3c3-135">[AuthorizeAreaFolder オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-135">An [AuthorizeAreaFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="8b3c3-136">ページへの匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-136">Allow anonymous access to a page</span></span>

<span data-ttu-id="8b3c3-137">使用して、 [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)規則を使用して[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)指定されたパスにページ。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-137">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="8b3c3-138">指定されたパスが、せず、拡張機能とスラッシュのみを含む Razor ページ ルートの相対パスは、ビュー エンジンのパス。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-138">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="8b3c3-139">ページのフォルダーへの匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-139">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="8b3c3-140">使用して、 [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder)規則を使用して[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)のすべての指定したパスにフォルダー内のページに。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-140">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="8b3c3-141">指定されたパスが、Razor ページのルートの相対パスは、ビュー エンジンのパスです。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-141">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="8b3c3-142">承認済みおよび匿名アクセスを組み合わせることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-142">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="8b3c3-143">ページのフォルダーが承認を要求して、そのフォルダー内のページが匿名アクセスを許可することを指定することを指定する完全に有効です。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-143">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="8b3c3-144">ただし、逆の場合は、true は。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-144">The reverse, however, isn't true.</span></span> <span data-ttu-id="8b3c3-145">匿名アクセスのページのフォルダーを宣言し、承認の内のページを指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-145">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="8b3c3-146">プライベート ページでの承認を必要とするため、機能せず両方、`AllowAnonymousFilter`と`AuthorizeFilter`フィルター、ページに適用されます、 `AllowAnonymousFilter` wins し、アクセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="8b3c3-146">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8b3c3-147">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="8b3c3-147">Additional resources</span></span>

* [<span data-ttu-id="8b3c3-148">Razor ページのカスタム ルートとページ モデル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="8b3c3-148">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="8b3c3-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection)クラス</span><span class="sxs-lookup"><span data-stu-id="8b3c3-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
