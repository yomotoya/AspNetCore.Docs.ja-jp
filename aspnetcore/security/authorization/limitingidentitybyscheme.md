---
title: ASP.NET Core での特定のスキームで承認します。
author: rick-anderson
description: この記事では、複数の認証方法を使用する場合は、id を特定のスキームを制限する方法について説明します。
ms.author: riande
ms.date: 10/22/2018
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 778bb61f472ab2e76f85da5999d3c79238188f19
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248200"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="b5d59-103">ASP.NET Core での特定のスキームで承認します。</span><span class="sxs-lookup"><span data-stu-id="b5d59-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="b5d59-104">シングル ページ アプリケーション (Spa) など、いくつかのシナリオでは、複数の認証方法を使用する一般的なです。</span><span class="sxs-lookup"><span data-stu-id="b5d59-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="b5d59-105">など、アプリのログインに cookie ベースの認証および使用 JWT ベアラー認証 JavaScript 要求。</span><span class="sxs-lookup"><span data-stu-id="b5d59-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="b5d59-106">場合によっては、アプリは、認証ハンドラーの複数のインスタンスがあります。</span><span class="sxs-lookup"><span data-stu-id="b5d59-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="b5d59-107">たとえば、id 基本的なには 1 つが含まれている 2 つの cookie ハンドラーと 1 つは多要素認証 (MFA) がトリガーされたときに作成されます。</span><span class="sxs-lookup"><span data-stu-id="b5d59-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="b5d59-108">MFA は、ユーザーは、追加のセキュリティを必要とする操作を要求したため、トリガーされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b5d59-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b5d59-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b5d59-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b5d59-110">認証サービスが認証時に構成されている場合は、認証方式がという名前です。</span><span class="sxs-lookup"><span data-stu-id="b5d59-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="b5d59-111">例:</span><span class="sxs-lookup"><span data-stu-id="b5d59-111">For example:</span></span>

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

<span data-ttu-id="b5d59-112">上記のコードでは、2 つの認証ハンドラーが追加されています。 cookie とベアラーのいずれかのいずれか。</span><span class="sxs-lookup"><span data-stu-id="b5d59-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="b5d59-113">既定のスキームを指定する、`HttpContext.User`プロパティがその id に設定されています。</span><span class="sxs-lookup"><span data-stu-id="b5d59-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="b5d59-114">その動作が必要ない場合は無効のパラメーターなしのフォームを呼び出すことによって`AddAuthentication`します。</span><span class="sxs-lookup"><span data-stu-id="b5d59-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b5d59-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b5d59-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b5d59-116">認証方式は、認証ミドルウェアが認証時に構成されている場合に名前です。</span><span class="sxs-lookup"><span data-stu-id="b5d59-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="b5d59-117">例:</span><span class="sxs-lookup"><span data-stu-id="b5d59-117">For example:</span></span>

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

<span data-ttu-id="b5d59-118">上記のコードでは、2 つの認証ミドルウェアが追加されています。 cookie とベアラーのいずれかのいずれか。</span><span class="sxs-lookup"><span data-stu-id="b5d59-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="b5d59-119">既定のスキームを指定する、`HttpContext.User`プロパティがその id に設定されています。</span><span class="sxs-lookup"><span data-stu-id="b5d59-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="b5d59-120">その動作が必要ない場合は無効を設定して、`AuthenticationOptions.AutomaticAuthenticate`プロパティを`false`します。</span><span class="sxs-lookup"><span data-stu-id="b5d59-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="b5d59-121">Authorize 属性を持つスキームを選択します。</span><span class="sxs-lookup"><span data-stu-id="b5d59-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="b5d59-122">承認、時点では、アプリは、使用するハンドラーを示します。</span><span class="sxs-lookup"><span data-stu-id="b5d59-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="b5d59-123">使用する認証スキームのコンマ区切りの一覧を渡すことによって、アプリは承認ハンドラーを選択して`[Authorize]`します。</span><span class="sxs-lookup"><span data-stu-id="b5d59-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="b5d59-124">`[Authorize]`属性は、認証方式、または、既定値が構成されているかどうかに関係なく使用するスキームを指定します。</span><span class="sxs-lookup"><span data-stu-id="b5d59-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="b5d59-125">例:</span><span class="sxs-lookup"><span data-stu-id="b5d59-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b5d59-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b5d59-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b5d59-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b5d59-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

<span data-ttu-id="b5d59-128">前の例では、ベアラーとクッキー ハンドラーは実行し、作成して、現在のユーザーの id を追加すること。</span><span class="sxs-lookup"><span data-stu-id="b5d59-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="b5d59-129">1 つのスキームのみを指定すると、対応するハンドラーが実行されます。</span><span class="sxs-lookup"><span data-stu-id="b5d59-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b5d59-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b5d59-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b5d59-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b5d59-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="b5d59-132">上記のコードでは、"Bearer"スキームでハンドラーのみが実行されます。</span><span class="sxs-lookup"><span data-stu-id="b5d59-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="b5d59-133">Cookie ベース id は無視されます。</span><span class="sxs-lookup"><span data-stu-id="b5d59-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="b5d59-134">ポリシーの設定を選択します。</span><span class="sxs-lookup"><span data-stu-id="b5d59-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="b5d59-135">必要な方式を指定する場合[ポリシー](xref:security/authorization/policies)を設定することができます、`AuthenticationSchemes`ポリシーを追加するときに、コレクション。</span><span class="sxs-lookup"><span data-stu-id="b5d59-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

<span data-ttu-id="b5d59-136">前の例では、"over18 という"ポリシーは、"Bearer"ハンドラーによって作成された id に対してのみ実行されます。</span><span class="sxs-lookup"><span data-stu-id="b5d59-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="b5d59-137">設定して、ポリシーを使用して、`[Authorize]`属性の`Policy`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="b5d59-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a><span data-ttu-id="b5d59-138">複数の認証方式を使用して、</span><span class="sxs-lookup"><span data-stu-id="b5d59-138">Use multiple authentication schemes</span></span>

<span data-ttu-id="b5d59-139">一部のアプリは、複数の種類の認証をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b5d59-139">Some apps may need to support multiple types of authentication.</span></span> <span data-ttu-id="b5d59-140">など、アプリがユーザー データベースと Azure Active Directory からユーザーを認証する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b5d59-140">For example, your app might authenticate users from Azure Active Directory and from a users database.</span></span> <span data-ttu-id="b5d59-141">別の例は、Active Directory フェデレーション サービスと Azure Active Directory B2C の両方からユーザーを認証するアプリです。</span><span class="sxs-lookup"><span data-stu-id="b5d59-141">Another example is an app that authenticates users from both Active Directory Federation Services and Azure Active Directory B2C.</span></span> <span data-ttu-id="b5d59-142">この場合、アプリでは、いくつかの発行者から JWT ベアラー トークンを受け入れる必要があります。</span><span class="sxs-lookup"><span data-stu-id="b5d59-142">In this case, the app should accept a JWT bearer token from several issuers.</span></span>

<span data-ttu-id="b5d59-143">そのまま使用したいすべての認証スキームを追加します。</span><span class="sxs-lookup"><span data-stu-id="b5d59-143">Add all authentication schemes you'd like to accept.</span></span> <span data-ttu-id="b5d59-144">次のコード例では、`Startup.ConfigureServices`さまざまな発行者で 2 つの JWT ベアラー認証スキームを追加します。</span><span class="sxs-lookup"><span data-stu-id="b5d59-144">For example, the following code in `Startup.ConfigureServices` adds two JWT bearer authentication schemes with different issuers:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://localhost:5000/identity/";
        })
        .AddJwtBearer("AzureAD", options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://login.microsoftonline.com/eb971100-6f99-4bdc-8611-1bc8edd7f436/";
        });
}
```

> [!NOTE]
> <span data-ttu-id="b5d59-145">既定の認証スキームは 1 つだけの JWT ベアラー認証に登録されて`JwtBearerDefaults.AuthenticationScheme`します。</span><span class="sxs-lookup"><span data-stu-id="b5d59-145">Only one JWT bearer authentication is registered with the default authentication scheme `JwtBearerDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="b5d59-146">追加の認証は、一意の認証スキームを登録することができます。</span><span class="sxs-lookup"><span data-stu-id="b5d59-146">Additional authentication has to be registered with a unique authentication scheme.</span></span>

<span data-ttu-id="b5d59-147">次の手順では、両方の認証方式を受け入れるように既定の承認ポリシーを更新します。</span><span class="sxs-lookup"><span data-stu-id="b5d59-147">The next step is to update the default authorization policy to accept both authentication schemes.</span></span> <span data-ttu-id="b5d59-148">例:</span><span class="sxs-lookup"><span data-stu-id="b5d59-148">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthorization(options =>
    {
        var defaultAuthorizationPolicyBuilder = new AuthorizationPolicyBuilder(
            JwtBearerDefaults.AuthenticationScheme,
            "AzureAD");
        defaultAuthorizationPolicyBuilder = 
            defaultAuthorizationPolicyBuilder.RequireAuthenticatedUser();
        options.DefaultPolicy = defaultAuthorizationPolicyBuilder.Build();
    });
}
```

<span data-ttu-id="b5d59-149">使用することは、既定の承認ポリシーがオーバーライドされると、`[Authorize]`コント ローラー内の属性。</span><span class="sxs-lookup"><span data-stu-id="b5d59-149">As the default authorization policy is overridden, it's possible to use the `[Authorize]` attribute in controllers.</span></span> <span data-ttu-id="b5d59-150">コント ローラーは、最初または 2 番目の発行者によって発行された JWT に、要求を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="b5d59-150">The controller then accepts requests with JWT issued by the first or second issuer.</span></span>

::: moniker-end
