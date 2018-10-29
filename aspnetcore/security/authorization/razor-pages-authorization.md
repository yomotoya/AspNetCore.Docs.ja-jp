---
title: ASP.NET Core で razor ページの承認規則
author: guardrex
description: ユーザーを承認して、匿名ユーザーがページまたはページのフォルダーにアクセスできるようにする規則を含むページへのアクセスを制御する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 675dc8aa4bf00bb21981cc892a09a4acd0d53c15
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207265"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>ASP.NET Core で razor ページの承認規則

作成者: [Luke Latham](https://github.com/guardrex)

Razor ページ アプリでのアクセスを制御する方法の 1 つでは、起動時に承認規則を使用します。 これらの規則を使用すると、ユーザーを承認して、匿名ユーザーが個々 のページまたはページのフォルダーにアクセスできるようにできます。 自動的にこのトピックで説明されている規則が適用されます[承認フィルター](xref:mvc/controllers/filters#authorization-filters)のアクセスを制御します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

サンプル アプリでは[ASP.NET Core Identity なしでの Cookie 認証](xref:security/authentication/cookie)します。 Maria Rodriguez、架空のユーザーのユーザー アカウント、アプリにハードコードしています。 電子メールのユーザー名を使用して、"maria.rodriguez@contoso.com"と、ユーザーがサインインするすべてのパスワード。 における、ユーザーの認証、`AuthenticateUser`メソッドで、 *Pages/Account/Login.cshtml.cs*ファイル。 実際の例では、ユーザーは、データベースに対して認証は。 ASP.NET Core Identity を使用するのガイダンスに従って、[の ASP.NET core Identity の概要](xref:security/authentication/identity)トピック。 概念とこのトピックで示す例については、ASP.NET Core Identity を使用するアプリに等しく適用されます。

## <a name="require-authorization-to-access-a-page"></a>ページにアクセスするための承認が必要です。

使用して、 [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)規則を使用して[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)のページに、指定されたパス。

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

指定されたパスが、せず、拡張機能とスラッシュのみを含む Razor ページ ルートの相対パスは、ビュー エンジンのパス。

[AuthorizePage オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> `AuthorizeFilter`とページ モデル クラスに適用できる、`[Authorize]`フィルター属性。 詳細については、次を参照してください。 [Authorize フィルター属性](xref:razor-pages/filter#authorize-filter-attribute)します。

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>ページのフォルダーにアクセスするための承認が必要です。

使用して、 [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)規則を使用して[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)のすべての指定したパスにフォルダー内のページに。

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

指定されたパスが、Razor ページのルートの相対パスは、ビュー エンジンのパスです。

[AuthorizeFolder オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a>領域をページにアクセスする承認を要求します。

使用して、 [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage)規則を使用して[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)指定されたパスの領域のページに。

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

ページ名は、指定された領域のページのルート ディレクトリに対する相対拡張子のないファイルのパスです。 たとえば、ファイルのページ名*Areas/Identity/Pages/Manage/Accounts.cshtml*は */管理/アカウント*します。

[AuthorizeAreaPage オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。

## <a name="require-authorization-to-access-a-folder-of-areas"></a>領域のフォルダーにアクセスするための承認が必要です。

使用して、 [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder)規則を使用して[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)のすべての指定したパスにフォルダー内の領域に。

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

フォルダーのパスは、指定された領域のページのルート ディレクトリに対する相対フォルダーのパスです。 ファイルのフォルダー パスではたとえば、*領域/ユーザー/ページ/管理/* は */管理*します。

[AuthorizeAreaFolder オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a>ページへの匿名アクセスを許可します。

使用して、 [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)規則を使用して[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)指定されたパスにページ。

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

指定されたパスが、せず、拡張機能とスラッシュのみを含む Razor ページ ルートの相対パスは、ビュー エンジンのパス。

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>ページのフォルダーへの匿名アクセスを許可します。

使用して、 [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder)規則を使用して[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)のすべての指定したパスにフォルダー内のページに。

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

指定されたパスが、Razor ページのルートの相対パスは、ビュー エンジンのパスです。

## <a name="note-on-combining-authorized-and-anonymous-access"></a>承認済みおよび匿名アクセスを組み合わせることに注意してください。

ページのフォルダーが承認を要求して、そのフォルダー内のページが匿名アクセスを許可することを指定することを指定する完全に有効です。

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

ただし、逆の場合は、true は。 匿名アクセスのページのフォルダーを宣言し、承認の内のページを指定することはできません。

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

プライベート ページでの承認を必要とするため、機能せず両方、`AllowAnonymousFilter`と`AuthorizeFilter`フィルター、ページに適用されます、 `AllowAnonymousFilter` wins し、アクセスを制御します。

## <a name="additional-resources"></a>その他の技術情報

* [Razor ページのカスタム ルートとページ モデル プロバイダー](xref:razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection)クラス
