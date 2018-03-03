---
title: "移行の認証および ASP.NET Core 2.0 を Id"
author: scottaddie
description: "この記事では、ASP.NET Core 2.0 に移行する ASP.NET Core 1.x 認証と Id の最も一般的な手順について説明します。"
manager: wpickett
ms.author: scaddie
ms.date: 10/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: f0c29e6b4faa5c9d574726fc960f0c7c60092757
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="9ebf6-103">移行の認証および ASP.NET Core 2.0 を Id</span><span class="sxs-lookup"><span data-stu-id="9ebf6-103">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="9ebf6-104">によって[Scott Addie](https://github.com/scottaddie)と[ハオ最後にトリム](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="9ebf6-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="9ebf6-105">ASP.NET Core 2.0 が認証用の新しいモデルと[Identity](xref:security/authentication/identity) services を使用して構成を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="9ebf6-106">認証または Id を使用する ASP.NET Core 1.x アプリケーションは、下記の手順に従って、新しいモデルを使用して更新できます。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="9ebf6-107">認証ミドルウェアとサービス</span><span class="sxs-lookup"><span data-stu-id="9ebf6-107">Authentication Middleware and services</span></span>
<span data-ttu-id="9ebf6-108">1.x プロジェクトでは、ミドルウェアを介して認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="9ebf6-109">各認証スキームをサポートするために、ミドルウェア メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="9ebf6-110">次の 1.x 例では、Facebook 認証を構成で Id を持つ*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions { 
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
} 
```

<span data-ttu-id="9ebf6-111">2.0 プロジェクトでは、サービスを使用して認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="9ebf6-112">各認証スキームが登録されている、`ConfigureServices`メソッドの*Startup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="9ebf6-113">`UseIdentity`メソッドが置き換え`UseAuthentication`です。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="9ebf6-114">次の 2.0 の例では、Facebook 認証を構成で Id を持つ*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

<span data-ttu-id="9ebf6-115">`UseAuthentication`メソッドは、自動認証およびリモート認証要求の処理を担当する 1 つの認証ミドルウェア コンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="9ebf6-116">単一の一般的なミドルウェア コンポーネントをすべての個別のミドルウェア コンポーネントに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="9ebf6-117">各主要な認証スキーム用 2.0 の移行手順のとおりです。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="9ebf6-118">Cookie ベースの認証</span><span class="sxs-lookup"><span data-stu-id="9ebf6-118">Cookie-based authentication</span></span>
<span data-ttu-id="9ebf6-119">次の 2 つのオプションのいずれかを選択しに必要な変更を加える*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="9ebf6-120">Id を持つ cookie を使用します。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="9ebf6-121">置き換える`UseIdentity`で`UseAuthentication`で、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="9ebf6-122">呼び出す、`AddIdentity`メソッドで、 `ConfigureServices` cookie 認証サービスを追加するメソッド。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="9ebf6-123">必要に応じて、呼び出し、`ConfigureApplicationCookie`または`ConfigureExternalCookie`メソッドで、 `ConfigureServices` Id cookie の設定を調整する方法です。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="9ebf6-124">Identity せず cookie を使用します。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="9ebf6-125">置換、`UseCookieAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="9ebf6-126">呼び出す、`AddAuthentication`と`AddCookie`内のメソッド、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User, 
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options => 
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="9ebf6-127">JWT ベアラ認証</span><span class="sxs-lookup"><span data-stu-id="9ebf6-127">JWT Bearer Authentication</span></span>
<span data-ttu-id="9ebf6-128">次の変更を加え*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="9ebf6-129">置換、`UseJwtBearerAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="9ebf6-130">呼び出す、`AddJwtBearer`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="9ebf6-131">渡すことによって、既定のスキームを設定する必要がありますので、このコード スニペットは Id を使用しない`JwtBearerDefaults.AuthenticationScheme`を`AddAuthentication`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="9ebf6-132">OpenID 接続 (OIDC) 認証</span><span class="sxs-lookup"><span data-stu-id="9ebf6-132">OpenID Connect (OIDC) authentication</span></span>
<span data-ttu-id="9ebf6-133">次の変更を加え*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="9ebf6-134">置換、`UseOpenIdConnectAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="9ebf6-135">呼び出す、`AddOpenIdConnect`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(options => 
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options => 
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a><span data-ttu-id="9ebf6-136">facebook の認証</span><span class="sxs-lookup"><span data-stu-id="9ebf6-136">Facebook authentication</span></span>
<span data-ttu-id="9ebf6-137">次の変更を加え*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-137">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="9ebf6-138">置換、`UseFacebookAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-138">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="9ebf6-139">呼び出す、`AddFacebook`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-139">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="9ebf6-140">Google 認証</span><span class="sxs-lookup"><span data-stu-id="9ebf6-140">Google authentication</span></span>
<span data-ttu-id="9ebf6-141">次の変更を加え*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-141">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="9ebf6-142">置換、`UseGoogleAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-142">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="9ebf6-143">呼び出す、`AddGoogle`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-143">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="9ebf6-144">Microsoft アカウントの認証</span><span class="sxs-lookup"><span data-stu-id="9ebf6-144">Microsoft Account authentication</span></span>
<span data-ttu-id="9ebf6-145">次の変更を加え*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-145">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="9ebf6-146">置換、`UseMicrosoftAccountAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-146">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="9ebf6-147">呼び出す、`AddMicrosoftAccount`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-147">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="9ebf6-148">Twitter 認証</span><span class="sxs-lookup"><span data-stu-id="9ebf6-148">Twitter authentication</span></span>
<span data-ttu-id="9ebf6-149">次の変更を加え*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-149">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="9ebf6-150">置換、`UseTwitterAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-150">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="9ebf6-151">呼び出す、`AddTwitter`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-151">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="9ebf6-152">既定の認証スキームの設定</span><span class="sxs-lookup"><span data-stu-id="9ebf6-152">Setting default authentication schemes</span></span>
<span data-ttu-id="9ebf6-153">1.x で、`AutomaticAuthenticate`と`AutomaticChallenge`のプロパティ、 [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1)基底クラスが 1 つの認証スキームに設定するためのものです。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-153">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="9ebf6-154">これを適用することをお勧めはありませんでした。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-154">There was no good way to enforce this.</span></span>

<span data-ttu-id="9ebf6-155">個々 のプロパティとして 2.0 では、これら 2 つのプロパティが削除されて`AuthenticationOptions`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-155">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="9ebf6-156">構成することができます、`AddAuthentication`内でメソッドの呼び出し、`ConfigureServices`メソッドの*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-156">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="9ebf6-157">上記のコード スニペットで、既定のスキームが に設定されている`CookieAuthenticationDefaults.AuthenticationScheme`("Cookie") です。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-157">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="9ebf6-158">または、オーバー ロードされたバージョンを使用して、 `AddAuthentication` 1 つ以上のプロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="9ebf6-159">次のオーバー ロードされたメソッドの例では、既定のスキームが に設定されている`CookieAuthenticationDefaults.AuthenticationScheme`です。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="9ebf6-160">認証スキームまたはを指定できますが、個人内`[Authorize]`属性または承認ポリシー。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="9ebf6-161">次の条件のいずれかが true の場合は、2.0 では、既定のスキームを定義します。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="9ebf6-162">ユーザーが自動的にサインインします。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="9ebf6-163">使用する、`[Authorize]`スキームを指定せずに属性または承認のポリシー</span><span class="sxs-lookup"><span data-stu-id="9ebf6-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="9ebf6-164">ただし、このルールは、`AddIdentity`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="9ebf6-165">このメソッドでは、cookie を追加すると、既定の認証およびパターンをアプリケーションの cookie にチャレンジ セット`IdentityConstants.ApplicationScheme`です。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="9ebf6-166">さらに、外部の cookie を既定のサインイン スキームに設定`IdentityConstants.ExternalScheme`です。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="9ebf6-167">HttpContext 認証の拡張機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-167">Use HttpContext authentication extensions</span></span>
<span data-ttu-id="9ebf6-168">`IAuthenticationManager`インターフェイスは、メイン エントリ ポイントを 1.x の認証システムにします。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="9ebf6-169">新しいセットに置き換わりました`HttpContext`の拡張メソッドにおいて、`Microsoft.AspNetCore.Authentication`名前空間。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="9ebf6-170">たとえば、1.x が参照をプロジェクトに`Authentication`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-170">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="9ebf6-171">プロジェクトでは 2.0、インポート、`Microsoft.AspNetCore.Authentication`名前空間、および削除、`Authentication`プロパティ参照。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-171">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="9ebf6-172">Windows 認証 (HTTP.sys/IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="9ebf6-172">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="9ebf6-173">Windows 認証の 2 つのバリエーションがあります。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-173">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="9ebf6-174">ホストは認証されたユーザーだけを許可します。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-174">The host only allows authenticated users</span></span>
2. <span data-ttu-id="9ebf6-175">により、ホストし、認証されたユーザー</span><span class="sxs-lookup"><span data-stu-id="9ebf6-175">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="9ebf6-176">上記で説明した最初のコマンドは、2.0 の変更による影響はありません。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-176">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="9ebf6-177">上記で説明した 2 番目のコマンドは 2.0 の変更の影響を受けます。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-177">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="9ebf6-178">例として、する可能性があることの匿名ユーザーを IIS でアプリケーションにまたは[HTTP.sys](xref:fundamentals/servers/weblistener)コント ローラー レベルの認証がユーザーのレイヤーします。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-178">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="9ebf6-179">このシナリオでは、既定のスキームを設定`IISDefaults.AuthenticationScheme`で、`ConfigureServices`メソッドの*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-179">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="9ebf6-180">それに応じてにより、作業からチャレンジに承認要求が、既定のスキームの設定に失敗しました。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-180">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="9ebf6-181">IdentityCookieOptions インスタンス</span><span class="sxs-lookup"><span data-stu-id="9ebf6-181">IdentityCookieOptions instances</span></span>
<span data-ttu-id="9ebf6-182">2.0 の変更の副作用は、cookie のオプションのインスタンスではなくオプションをという名前を使用するスイッチです。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-182">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="9ebf6-183">Id cookie のスキーム名をカスタマイズする機能が削除されます。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-183">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="9ebf6-184">1.x での使用のプロジェクトなど、[コンス トラクター インジェクション](xref:mvc/controllers/dependency-injection#constructor-injection)に渡す、`IdentityCookieOptions`にパラメーター *AccountController.cs*です。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-184">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="9ebf6-185">外部の cookie 認証スキームは指定されたインスタンスからアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-185">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="9ebf6-186">ここに挙げたコンス トラクター インジェクションがプロジェクトでは 2.0、不要になると`_externalCookieScheme`フィールドを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-186">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="9ebf6-187">`IdentityConstants.ExternalScheme`定数を直接使用することができます。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-187">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="9ebf6-188">IdentityUser POCO のナビゲーション プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-188">Add IdentityUser POCO navigation properties</span></span>
<span data-ttu-id="9ebf6-189">ベースの Entity Framework (EF) 中核となるナビゲーション プロパティ`IdentityUser`POCO (Plain Old CLR Object) が削除されました。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-189">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="9ebf6-190">1.x プロジェクトでは、これらのプロパティを使用する場合は、この 2.0 のプロジェクトに追加手動でします。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-190">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

<span data-ttu-id="9ebf6-191">外部キーの重複を防ぐためには、EF コア移行を実行するときに、追加、次のように、`IdentityDbContext`クラス`OnModelCreating`メソッド (後、`base.OnModelCreating();`を呼び出す)。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-191">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="9ebf6-192">GetExternalAuthenticationSchemes を置き換えます</span><span class="sxs-lookup"><span data-stu-id="9ebf6-192">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="9ebf6-193">同期メソッド`GetExternalAuthenticationSchemes`非同期バージョンを優先するために削除されました。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-193">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="9ebf6-194">1.x プロジェクトに次のコードがある*ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-194">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="9ebf6-195">このメソッドは、 *Login.cshtml*すぎます。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-195">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="9ebf6-196">2.0 のプロジェクトで使用して、`GetExternalAuthenticationSchemesAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-196">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="9ebf6-197">*Login.cshtml*、`AuthenticationScheme`でアクセスされるプロパティ、`foreach`ループに変更`Name`:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-197">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="9ebf6-198">ManageLoginsViewModel プロパティの変更</span><span class="sxs-lookup"><span data-stu-id="9ebf6-198">ManageLoginsViewModel property change</span></span>
<span data-ttu-id="9ebf6-199">A`ManageLoginsViewModel`でオブジェクトを使用して、`ManageLogins`のアクション*ManageController.cs*です。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-199">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="9ebf6-200">1.x のプロジェクトで、オブジェクトの`OtherLogins`プロパティの戻り型が`IList<AuthenticationDescription>`です。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-200">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="9ebf6-201">この戻り値の型のインポートを必要と`Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="9ebf6-201">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="9ebf6-202">2.0 プロジェクトでは、戻り値の型の変更`IList<AuthenticationScheme>`です。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-202">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="9ebf6-203">この新しいの戻り値の型は、置き換える必要があります、`Microsoft.AspNetCore.Http.Authentication`と共にインポート、`Microsoft.AspNetCore.Authentication`をインポートします。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-203">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="9ebf6-204">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="9ebf6-204">Additional resources</span></span>
<span data-ttu-id="9ebf6-205">追加の詳細についてを参照してください、 [Auth 2.0 のディスカッション](https://github.com/aspnet/Security/issues/1338)GitHub の問題です。</span><span class="sxs-lookup"><span data-stu-id="9ebf6-205">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
