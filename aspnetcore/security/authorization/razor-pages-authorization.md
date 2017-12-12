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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>ASP.NET Core に razor ページの承認規則

作成者: [Luke Latham](https://github.com/guardrex)

Razor ページ アプリのアクセスを制御する方法の 1 つは起動時に承認規則を使用することです。 これらの規則を使用すると、ユーザーを承認して、個々 のページまたはページのフォルダーにアクセスする匿名ユーザーを許可できます。 自動的にこのトピックで説明する規則を適用[承認フィルター](xref:mvc/controllers/filters#authorization-filters)のアクセスを制御します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="require-authorization-to-access-a-page"></a>ページにアクセスするための承認が必要

使用して、 [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)指定したパスにページに。

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

指定されたパスは、ビュー エンジンのパス、これは、拡張機能とスラッシュのみを含むせず Razor ページ ルートの相対パスです。

[AuthorizePage オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。

## <a name="require-authorization-to-access-a-folder-of-pages"></a>ページのフォルダーにアクセスするための承認が必要

使用して、 [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)のすべての指定したパスにフォルダー内のページに。

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

指定されたパスは、Razor ページのルートの相対パスは、ビュー エンジンのパスです。

[AuthorizeFolder オーバー ロード](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)は承認ポリシーを指定する必要がある場合に使用します。

## <a name="allow-anonymous-access-to-a-page"></a>ページへの匿名アクセスを許可します。

使用して、 [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)指定したパスにページに。

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

指定されたパスは、ビュー エンジンのパス、これは、拡張機能とスラッシュのみを含むせず Razor ページ ルートの相対パスです。

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>ページのフォルダーへの匿名アクセスを許可します。

使用して、 [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder)を介して規約[AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions)を追加する、 [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter)のすべての指定したパスにフォルダー内のページに。

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

指定されたパスは、Razor ページのルートの相対パスは、ビュー エンジンのパスです。

## <a name="note-on-combining-authorized-and-anonymous-access"></a>承認済みと匿名アクセスを組み合わせることに注意してください。

ページのフォルダーが承認を必要とし、そのフォルダー内のページが匿名アクセスを許可するように指定を指定することは完全に。

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

ただし、逆の場合は、true は。 匿名アクセスのページのフォルダーを宣言し、承認の内のページを指定することはできません。

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

[プライベート] ページの承認を必要とするため、機能しないときに両方、`AllowAnonymousFilter`と`AuthorizeFilter`、ページにフィルターを適用、 `AllowAnonymousFilter` wins し、アクセスを制御します。

## <a name="see-also"></a>関連項目

* [Razor ページ カスタム ルートしてページ モデル プロバイダー](xref:mvc/razor-pages/razor-pages-convention-features)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection)クラス
