---
title: "スキームで識別情報を制限します。"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 2483c441da317a5c29b611b3a4910eae3c01fd7a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-identity-by-scheme"></a>スキームで識別情報を制限します。

<a name=security-authorization-limiting-by-scheme></a>

一部のシナリオで単一ページ アプリケーションなど可能であればを複数の認証方法終了します。 たとえば、アプリケーションおよび使用できます cookie ベースの認証のログインにベアラ認証の JavaScript 要求。 場合によっては、認証ミドルウェアの複数のインスタンスがあります。 たとえば、次の 2 つ cookie middlewares 基本的な id が含まれています、ユーザーは、追加のセキュリティを必要とする操作を要求したため、multi-factor authentication がトリガーされたときに 1 つ作成します。

認証ミドルウェアが認証時に、たとえば構成されている場合に認証スキームがという名前します。

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AuthenticationScheme = "Cookie",
    LoginPath = new PathString("/Account/Unauthorized/"),
    AccessDeniedPath = new PathString("/Account/Forbidden/"),
    AutomaticAuthenticate = false
});

app.UseBearerAuthentication(options =>
{
    options.AuthenticationScheme = "Bearer";
    options.AutomaticAuthenticate = false;
});
```

この構成では 2 つの認証 middlewares が追加されました、cookie をベアラーの 1 つ。

>[!NOTE]
>複数の認証ミドルウェアを追加するときに、自動的に実行するミドルウェアが構成されていないことを確認する必要があります。 設定して、これを行う、`AutomaticAuthenticate`オプションのプロパティを false にします。 これに失敗したこのスキームによってフィルタ リングは機能しません。

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Authorize attribute のスキームを選択します。

自動的に実行を作成する必要がある、id の認証ミドルウェアが構成されていないと承認の時点でミドルウェアを使用するを選択します。 承認するミドルウェアを選択する最も簡単な方法が使用するには、`ActiveAuthenticationSchemes`プロパティです。 このプロパティは、使用する認証スキームのコンマ区切りのリストを指定できます。 例を示します。

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

Cookie とベアラー上記の例で middlewares は実行し、作成して、現在のユーザーの id を追加することはします。 1 つのスキームを指定することによって、指定したミドルウェアのみが実行されます。

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

ここではベアラー スキームとミドルウェアのみ、し、クッキー ベース id は無視されます。

## <a name="selecting-the-scheme-with-policies"></a>ポリシーと設定の選択

必要な方式を指定する場合[ポリシー](policies.md#security-authorization-policies-based)設定することができます、`AuthenticationSchemes`コレクション、ポリシーを追加するときにします。

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

この例で、Over18 ポリシーはに対してのみ実行によって作成された、id、`Bearer`ミドルウェア。
