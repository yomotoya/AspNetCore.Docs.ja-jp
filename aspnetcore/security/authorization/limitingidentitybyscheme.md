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
# <a name="limiting-identity-by-scheme"></a><span data-ttu-id="85bbb-103">スキームで識別情報を制限します。</span><span class="sxs-lookup"><span data-stu-id="85bbb-103">Limiting identity by scheme</span></span>

<a name=security-authorization-limiting-by-scheme></a>

<span data-ttu-id="85bbb-104">一部のシナリオで単一ページ アプリケーションなど可能であればを複数の認証方法終了します。</span><span class="sxs-lookup"><span data-stu-id="85bbb-104">In some scenarios, such as Single Page Applications it is possible to end up with multiple authentication methods.</span></span> <span data-ttu-id="85bbb-105">たとえば、アプリケーションおよび使用できます cookie ベースの認証のログインにベアラ認証の JavaScript 要求。</span><span class="sxs-lookup"><span data-stu-id="85bbb-105">For example, your application may use cookie-based authentication to log in and bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="85bbb-106">場合によっては、認証ミドルウェアの複数のインスタンスがあります。</span><span class="sxs-lookup"><span data-stu-id="85bbb-106">In some cases you may have multiple instances of an authentication middleware.</span></span> <span data-ttu-id="85bbb-107">たとえば、次の 2 つ cookie middlewares 基本的な id が含まれています、ユーザーは、追加のセキュリティを必要とする操作を要求したため、multi-factor authentication がトリガーされたときに 1 つ作成します。</span><span class="sxs-lookup"><span data-stu-id="85bbb-107">For example, two cookie middlewares where one contains a basic identity and one is created when a multi-factor authentication has triggered because the user requested an operation that requires extra security.</span></span>

<span data-ttu-id="85bbb-108">認証ミドルウェアが認証時に、たとえば構成されている場合に認証スキームがという名前します。</span><span class="sxs-lookup"><span data-stu-id="85bbb-108">Authentication schemes are named when authentication middleware is configured during authentication, for example</span></span>

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

<span data-ttu-id="85bbb-109">この構成では 2 つの認証 middlewares が追加されました、cookie をベアラーの 1 つ。</span><span class="sxs-lookup"><span data-stu-id="85bbb-109">In this configuration two authentication middlewares have been added, one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="85bbb-110">複数の認証ミドルウェアを追加するときに、自動的に実行するミドルウェアが構成されていないことを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="85bbb-110">When adding multiple authentication middleware you should ensure that no middleware is configured to run automatically.</span></span> <span data-ttu-id="85bbb-111">設定して、これを行う、`AutomaticAuthenticate`オプションのプロパティを false にします。</span><span class="sxs-lookup"><span data-stu-id="85bbb-111">You do this by setting the `AutomaticAuthenticate` options property to false.</span></span> <span data-ttu-id="85bbb-112">これに失敗したこのスキームによってフィルタ リングは機能しません。</span><span class="sxs-lookup"><span data-stu-id="85bbb-112">If you fail to do this filtering by scheme will not work.</span></span>

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="85bbb-113">Authorize attribute のスキームを選択します。</span><span class="sxs-lookup"><span data-stu-id="85bbb-113">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="85bbb-114">自動的に実行を作成する必要がある、id の認証ミドルウェアが構成されていないと承認の時点でミドルウェアを使用するを選択します。</span><span class="sxs-lookup"><span data-stu-id="85bbb-114">As no authentication middleware is configured to automatically run and create an identity you must, at the point of authorization choose which middleware will be used.</span></span> <span data-ttu-id="85bbb-115">承認するミドルウェアを選択する最も簡単な方法が使用するには、`ActiveAuthenticationSchemes`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="85bbb-115">The simplest way to select the middleware you wish to authorize with is to use the `ActiveAuthenticationSchemes` property.</span></span> <span data-ttu-id="85bbb-116">このプロパティは、使用する認証スキームのコンマ区切りのリストを指定できます。</span><span class="sxs-lookup"><span data-stu-id="85bbb-116">This property accepts a comma delimited list of Authentication Schemes to use.</span></span> <span data-ttu-id="85bbb-117">例を示します。</span><span class="sxs-lookup"><span data-stu-id="85bbb-117">For example;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

<span data-ttu-id="85bbb-118">Cookie とベアラー上記の例で middlewares は実行し、作成して、現在のユーザーの id を追加することはします。</span><span class="sxs-lookup"><span data-stu-id="85bbb-118">In the example above both the cookie and bearer middlewares will run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="85bbb-119">1 つのスキームを指定することによって、指定したミドルウェアのみが実行されます。</span><span class="sxs-lookup"><span data-stu-id="85bbb-119">By specifying a single scheme only the specified middleware will run;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

<span data-ttu-id="85bbb-120">ここではベアラー スキームとミドルウェアのみ、し、クッキー ベース id は無視されます。</span><span class="sxs-lookup"><span data-stu-id="85bbb-120">In this case only the middleware with the Bearer scheme would run, and any cookie based identities would be ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="85bbb-121">ポリシーと設定の選択</span><span class="sxs-lookup"><span data-stu-id="85bbb-121">Selecting the scheme with policies</span></span>

<span data-ttu-id="85bbb-122">必要な方式を指定する場合[ポリシー](policies.md#security-authorization-policies-based)設定することができます、`AuthenticationSchemes`コレクション、ポリシーを追加するときにします。</span><span class="sxs-lookup"><span data-stu-id="85bbb-122">If you prefer to specify the desired schemes in [policy](policies.md#security-authorization-policies-based) you can set the `AuthenticationSchemes` collection when adding your policy.</span></span>

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

<span data-ttu-id="85bbb-123">この例で、Over18 ポリシーはに対してのみ実行によって作成された、id、`Bearer`ミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="85bbb-123">In this example the Over18 policy will only run against the identity created by the `Bearer` middleware.</span></span>
