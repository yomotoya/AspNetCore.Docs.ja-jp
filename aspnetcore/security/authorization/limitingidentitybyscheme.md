---
title: "特定のスキームの ASP.NET Core を承認します。"
author: rick-anderson
description: "この記事では、複数の認証方法を使用する場合は、id、特定のスキームを制限する方法について説明します。"
keywords: "ASP.NET Core、id、認証スキーム"
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: cf3259f206b8d970cc6f2b0b9e52e233c30d6df3
ms.sourcegitcommit: e3b1726cc04e80dc28464c35259edbd3bc39a438
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/12/2017
---
# <a name="authorize-with-a-specific-scheme"></a><span data-ttu-id="98bc6-104">特定のスキームを承認します。</span><span class="sxs-lookup"><span data-stu-id="98bc6-104">Authorize with a specific scheme</span></span>

<span data-ttu-id="98bc6-105">単一ページ アプリケーション (SPAs) など、一部のシナリオでは、複数の認証方法を使用する一般的なです。</span><span class="sxs-lookup"><span data-stu-id="98bc6-105">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="98bc6-106">たとえば、アプリおよび使用できます cookie ベースの認証のログインに JWT ベアラ認証の JavaScript 要求。</span><span class="sxs-lookup"><span data-stu-id="98bc6-106">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="98bc6-107">アプリの認証ハンドラーの複数のインスタンスことがあります。</span><span class="sxs-lookup"><span data-stu-id="98bc6-107">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="98bc6-108">たとえば、基本的な id を含む 1 つ、2 つのクッキー ハンドラーと 1 つは多要素認証 (MFA) がトリガーされたときに作成されます。</span><span class="sxs-lookup"><span data-stu-id="98bc6-108">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="98bc6-109">ユーザーは、追加のセキュリティを必要とする操作を要求したため、MFA をトリガー可能性があります。</span><span class="sxs-lookup"><span data-stu-id="98bc6-109">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="98bc6-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="98bc6-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="98bc6-111">認証サービスが認証時に構成されている場合は、認証スキームがという名前です。</span><span class="sxs-lookup"><span data-stu-id="98bc6-111">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="98bc6-112">例:</span><span class="sxs-lookup"><span data-stu-id="98bc6-112">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

<span data-ttu-id="98bc6-113">上記のコードでは、2 つの認証ハンドラーが追加されました。 cookie とベアラーのいずれかのいずれか。</span><span class="sxs-lookup"><span data-stu-id="98bc6-113">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="98bc6-114">既定のスキームを指定する、`HttpContext.User`その id に設定されているプロパティ。</span><span class="sxs-lookup"><span data-stu-id="98bc6-114">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="98bc6-115">その動作が必要ない場合は無効のパラメーターなしのフォームを呼び出すことによって`AddAuthentication`です。</span><span class="sxs-lookup"><span data-stu-id="98bc6-115">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="98bc6-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="98bc6-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="98bc6-117">認証スキームには、認証 middlewares が認証時に構成されている場合は、という名前です。</span><span class="sxs-lookup"><span data-stu-id="98bc6-117">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="98bc6-118">例:</span><span class="sxs-lookup"><span data-stu-id="98bc6-118">For example:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

<span data-ttu-id="98bc6-119">上記のコードでは、次の 2 つの認証 middlewares が追加されました。 cookie とベアラーのいずれかのいずれか。</span><span class="sxs-lookup"><span data-stu-id="98bc6-119">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="98bc6-120">既定のスキームを指定する、`HttpContext.User`その id に設定されているプロパティ。</span><span class="sxs-lookup"><span data-stu-id="98bc6-120">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="98bc6-121">その動作が必要ない場合は、無効に設定して、`AuthenticationOptions.AutomaticAuthenticate`プロパティを`false`です。</span><span class="sxs-lookup"><span data-stu-id="98bc6-121">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="98bc6-122">Authorize attribute のスキームを選択します。</span><span class="sxs-lookup"><span data-stu-id="98bc6-122">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="98bc6-123">承認、時点では、アプリは、使用するハンドラーを示します。</span><span class="sxs-lookup"><span data-stu-id="98bc6-123">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="98bc6-124">使用するアプリを認証方式のコンマ区切りの一覧を渡すことによって承認は、ハンドラーを選択して`[Authorize]`です。</span><span class="sxs-lookup"><span data-stu-id="98bc6-124">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="98bc6-125">`[Authorize]`属性は、認証方式、または、既定値が構成されているかどうかに関係なく使用するスキームを指定します。</span><span class="sxs-lookup"><span data-stu-id="98bc6-125">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="98bc6-126">例:</span><span class="sxs-lookup"><span data-stu-id="98bc6-126">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="98bc6-127">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="98bc6-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="98bc6-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="98bc6-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

---

<span data-ttu-id="98bc6-129">前の例では、cookie とベアラーの両方のハンドラーは実行し、作成して、現在のユーザーの id を追加することはします。</span><span class="sxs-lookup"><span data-stu-id="98bc6-129">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="98bc6-130">1 つのスキームのみを指定することで、対応するハンドラーが実行されます。</span><span class="sxs-lookup"><span data-stu-id="98bc6-130">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="98bc6-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="98bc6-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = "Bearer")]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="98bc6-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="98bc6-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
public class MixedController : Controller
```

---

<span data-ttu-id="98bc6-133">上記のコードでは、"Bearer"スキームでハンドラーのみが実行されます。</span><span class="sxs-lookup"><span data-stu-id="98bc6-133">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="98bc6-134">Cookie ベース id は無視されます。</span><span class="sxs-lookup"><span data-stu-id="98bc6-134">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="98bc6-135">ポリシーと設定の選択</span><span class="sxs-lookup"><span data-stu-id="98bc6-135">Selecting the scheme with policies</span></span>

<span data-ttu-id="98bc6-136">必要な方式を指定する場合[ポリシー](xref:security/authorization/policies#security-authorization-policies-based)、設定することができます、`AuthenticationSchemes`ポリシーを追加するときにコレクション。</span><span class="sxs-lookup"><span data-stu-id="98bc6-136">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies#security-authorization-policies-based), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add("Bearer");
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

<span data-ttu-id="98bc6-137">前の例では、"over18 という"ポリシーは、"Bearer"ハンドラーによって作成された id に対してのみ実行されます。</span><span class="sxs-lookup"><span data-stu-id="98bc6-137">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="98bc6-138">設定して、ポリシーを使用して、`[Authorize]`属性の`Policy`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="98bc6-138">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
