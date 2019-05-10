---
title: ASP.NET Core で razor ページの承認規則
author: guardrex
description: ユーザーを承認して、匿名ユーザーがページまたはページのフォルダーにアクセスできるようにする規則を含むページへのアクセスを制御する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: ff061f96f30cd893b903403de760a172c924cf06
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895419"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>ASP.NET Core で razor ページの承認規則

作成者: [Luke Latham](https://github.com/guardrex)

Razor ページ アプリでのアクセスを制御する方法の 1 つでは、起動時に承認規則を使用します。 これらの規則を使用すると、ユーザーを承認して、匿名ユーザーが個々 のページまたはページのフォルダーにアクセスできるようにできます。 自動的にこのトピックで説明されている規則が適用されます[承認フィルター](xref:mvc/controllers/filters#authorization-filters)のアクセスを制御します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

サンプル アプリでは[ASP.NET Core Identity なしでの cookie 認証](xref:security/authentication/cookie)します。 概念とこのトピックで示す例については、ASP.NET Core Identity を使用するアプリに等しく適用されます。 ASP.NET Core Identity を使用するのガイダンスに従って<xref:security/authentication/identity>します。

## <a name="require-authorization-to-access-a-page"></a>ページにアクセスするための承認が必要です。

使用して、<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*>規則を使用して<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>を追加する、<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>のページに、指定されたパス。

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

指定されたパスが、せず、拡張機能とスラッシュのみを含む Razor ページ ルートの相対パスは、ビュー エンジンのパス。

指定する、[承認ポリシー](xref:security/authorization/policies)を使用して、 [AuthorizePage オーバー ロード](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>とページ モデル クラスに適用できる、`[Authorize]`フィルター属性。 詳細については、次を参照してください。 [Authorize フィルター属性](xref:razor-pages/filter#authorize-filter-attribute)します。

## <a name="require-authorization-to-access-a-folder-of-pages"></a>ページのフォルダーにアクセスするための承認が必要です。

使用して、<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*>規則を使用して<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>を追加する、<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>のすべての指定したパスにフォルダー内のページに。

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

指定されたパスが、Razor ページのルートの相対パスは、ビュー エンジンのパスです。

指定する、[承認ポリシー](xref:security/authorization/policies)を使用して、 [AuthorizeFolder オーバー ロード](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>領域をページにアクセスする承認を要求します。

使用して、<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*>規則を使用して<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>を追加する、<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>指定されたパスの領域のページに。

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

ページ名は、指定された領域のページのルート ディレクトリに対する相対拡張子のないファイルのパスです。 たとえば、ファイルのページ名*Areas/Identity/Pages/Manage/Accounts.cshtml*は */管理/アカウント*します。

指定する、[承認ポリシー](xref:security/authorization/policies)を使用して、 [AuthorizeAreaPage オーバー ロード](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>領域のフォルダーにアクセスするための承認が必要です。

使用して、<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*>規則を使用して<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>を追加する、<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>のすべての指定したパスにフォルダー内の領域に。

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

フォルダーのパスは、指定された領域のページのルート ディレクトリに対する相対フォルダーのパスです。 ファイルのフォルダー パスではたとえば、*領域/ユーザー/ページ/管理/* は */管理*します。

指定する、[承認ポリシー](xref:security/authorization/policies)を使用して、 [AuthorizeAreaFolder オーバー ロード](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>ページへの匿名アクセスを許可します。

使用して、<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*>規則を使用して<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>を追加する、<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>指定されたパスにページ。

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

指定されたパスが、せず、拡張機能とスラッシュのみを含む Razor ページ ルートの相対パスは、ビュー エンジンのパス。

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>ページのフォルダーへの匿名アクセスを許可します。

使用して、<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*>規則を使用して<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>を追加する、<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>のすべての指定したパスにフォルダー内のページに。

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

指定されたパスが、Razor ページのルートの相対パスは、ビュー エンジンのパスです。

## <a name="note-on-combining-authorized-and-anonymous-access"></a>承認済みおよび匿名アクセスを組み合わせることに注意してください。

指定することは承認を必要とするページのフォルダーよりも、そのフォルダー内のページが匿名アクセスを許可することを指定するとします。

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

ただし、逆の場合が無効です。 匿名アクセスのページのフォルダーを宣言し、承認を必要とするそのフォルダー内のページを指定することはできません。

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

[プライベート] ページが必要な承認が失敗します。 ときに両方の<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>と<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>、ページに適用されます、<xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>が優先され、アクセスを制御します。

## <a name="additional-resources"></a>その他の技術情報

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
