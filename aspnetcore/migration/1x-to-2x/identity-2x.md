---
title: "移行の認証および ASP.NET Core 2.0 を Id"
author: scottaddie
description: "この記事では、ASP.NET Core 2.0 に移行する ASP.NET Core 1.x 認証と Id の最も一般的な手順について説明します。"
keywords: "ASP.NET Core、Id 認証"
ms.author: scaddie
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 6505378c718d2ec676b534ddeb141231faf3fca3
ms.sourcegitcommit: 8cafdd1dd409d5070d227100ba0e094c779ac47b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="04c32-104">移行の認証および ASP.NET Core 2.0 を Id</span><span class="sxs-lookup"><span data-stu-id="04c32-104">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="04c32-105">によって[Scott Addie](https://github.com/scottaddie)と[ハオ最後にトリム](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="04c32-105">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="04c32-106">ASP.NET Core 2.0 が認証用の新しいモデルと[Identity](xref:security/authentication/identity) services を使用して構成を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="04c32-106">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="04c32-107">認証または Id を使用する ASP.NET Core 1.x アプリケーションは、下記の手順に従って、新しいモデルを使用して更新できます。</span><span class="sxs-lookup"><span data-stu-id="04c32-107">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="04c32-108">認証ミドルウェアとサービス</span><span class="sxs-lookup"><span data-stu-id="04c32-108">Authentication Middleware and Services</span></span>
<span data-ttu-id="04c32-109">1.x プロジェクトでは、ミドルウェアを介して認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="04c32-109">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="04c32-110">各認証スキームをサポートするために、ミドルウェア メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="04c32-110">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="04c32-111">次の 1.x 例では、Facebook 認証を構成で Id を持つ*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="04c32-111">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="04c32-112">2.0 プロジェクトでは、サービスを使用して認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="04c32-112">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="04c32-113">各認証スキームが登録されている、`ConfigureServices`メソッドの*Startup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="04c32-113">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="04c32-114">`UseIdentity`メソッドが置き換え`UseAuthentication`です。</span><span class="sxs-lookup"><span data-stu-id="04c32-114">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="04c32-115">次の 2.0 の例では、Facebook 認証を構成で Id を持つ*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="04c32-115">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options => {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

<span data-ttu-id="04c32-116">`UseAuthentication`メソッドは、自動認証およびリモート認証要求の処理を担当する 1 つの認証ミドルウェア コンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="04c32-116">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="04c32-117">単一の一般的なミドルウェア コンポーネントをすべての個別のミドルウェア コンポーネントに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="04c32-117">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="04c32-118">各主要な認証スキーム用 2.0 の移行手順のとおりです。</span><span class="sxs-lookup"><span data-stu-id="04c32-118">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="04c32-119">Cookie ベースの認証</span><span class="sxs-lookup"><span data-stu-id="04c32-119">Cookie-based Authentication</span></span>
<span data-ttu-id="04c32-120">次の 2 つのオプションのいずれかを選択しに必要な変更を加える*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="04c32-120">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="04c32-121">Id を持つ cookie を使用します。</span><span class="sxs-lookup"><span data-stu-id="04c32-121">Use cookies with Identity</span></span>
    - <span data-ttu-id="04c32-122">置き換える`UseIdentity`で`UseAuthentication`で、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="04c32-122">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="04c32-123">呼び出す、`AddIdentity`メソッドで、 `ConfigureServices` cookie 認証サービスを追加するメソッド。</span><span class="sxs-lookup"><span data-stu-id="04c32-123">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="04c32-124">必要に応じて、呼び出し、`ConfigureApplicationCookie`または`ConfigureExternalCookie`メソッドで、 `ConfigureServices` Id cookie の設定を調整する方法です。</span><span class="sxs-lookup"><span data-stu-id="04c32-124">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="04c32-125">Identity せず cookie を使用します。</span><span class="sxs-lookup"><span data-stu-id="04c32-125">Use cookies without Identity</span></span>
    - <span data-ttu-id="04c32-126">置換、`UseCookieAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="04c32-126">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="04c32-127">呼び出す、`AddAuthentication`と`AddCookie`内のメソッド、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="04c32-127">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User, 
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options => {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="04c32-128">JWT ベアラ認証</span><span class="sxs-lookup"><span data-stu-id="04c32-128">JWT Bearer Authentication</span></span>
<span data-ttu-id="04c32-129">次の変更を加え*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="04c32-129">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="04c32-130">置換、`UseJwtBearerAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="04c32-130">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="04c32-131">呼び出す、`AddJwtBearer`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="04c32-131">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="04c32-132">渡すことによって、既定のスキームを設定する必要がありますので、このコード スニペットは Id を使用しない`JwtBearerDefaults.AuthenticationScheme`を`AddAuthentication`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="04c32-132">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="04c32-133">OpenID Connect (OIDC) 認証</span><span class="sxs-lookup"><span data-stu-id="04c32-133">OpenID Connect (OIDC) Authentication</span></span>
<span data-ttu-id="04c32-134">次の変更を加え*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="04c32-134">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="04c32-135">置換、`UseOpenIdConnectAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="04c32-135">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="04c32-136">呼び出す、`AddOpenIdConnect`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="04c32-136">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(options => {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options => {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a><span data-ttu-id="04c32-137">Facebook の認証</span><span class="sxs-lookup"><span data-stu-id="04c32-137">Facebook Authentication</span></span>
<span data-ttu-id="04c32-138">次の変更を加え*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="04c32-138">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="04c32-139">置換、`UseFacebookAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="04c32-139">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="04c32-140">呼び出す、`AddFacebook`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="04c32-140">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="04c32-141">Google 認証</span><span class="sxs-lookup"><span data-stu-id="04c32-141">Google Authentication</span></span>
<span data-ttu-id="04c32-142">次の変更を加え*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="04c32-142">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="04c32-143">置換、`UseGoogleAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="04c32-143">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="04c32-144">呼び出す、`AddGoogle`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="04c32-144">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="04c32-145">Microsoft アカウントの認証</span><span class="sxs-lookup"><span data-stu-id="04c32-145">Microsoft Account Authentication</span></span>
<span data-ttu-id="04c32-146">次の変更を加え*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="04c32-146">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="04c32-147">置換、`UseMicrosoftAccountAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="04c32-147">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="04c32-148">呼び出す、`AddMicrosoftAccount`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="04c32-148">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="04c32-149">Twitter 認証</span><span class="sxs-lookup"><span data-stu-id="04c32-149">Twitter Authentication</span></span>
<span data-ttu-id="04c32-150">次の変更を加え*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="04c32-150">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="04c32-151">置換、`UseTwitterAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="04c32-151">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="04c32-152">呼び出す、`AddTwitter`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="04c32-152">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="04c32-153">既定の認証スキームの設定</span><span class="sxs-lookup"><span data-stu-id="04c32-153">Setting Default Authentication Schemes</span></span>
<span data-ttu-id="04c32-154">1.x で、`AutomaticAuthenticate`と`AutomaticChallenge`プロパティが 1 つの認証スキームに設定するためのものです。</span><span class="sxs-lookup"><span data-stu-id="04c32-154">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="04c32-155">これを適用することをお勧めはありませんでした。</span><span class="sxs-lookup"><span data-stu-id="04c32-155">There was no good way to enforce this.</span></span>

<span data-ttu-id="04c32-156">個々 のフラグとして 2.0 では、これら 2 つのプロパティが削除されて`AuthenticationOptions`インスタンスし、はベースに移動した[AuthenticationOptions](/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions)クラスです。</span><span class="sxs-lookup"><span data-stu-id="04c32-156">In 2.0, these two properties have been removed as flags on the individual `AuthenticationOptions` instance and have moved into the base [AuthenticationOptions](/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) class.</span></span> <span data-ttu-id="04c32-157">プロパティを構成することができます、`AddAuthentication`内でメソッドの呼び出し、`ConfigureServices`メソッドの*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="04c32-157">The properties can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="04c32-158">または、オーバー ロードされたバージョンを使用して、 `AddAuthentication` 1 つ以上のプロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="04c32-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="04c32-159">次のオーバー ロードされたメソッドの例では、既定のスキームが に設定されている`CookieAuthenticationDefaults.AuthenticationScheme`です。</span><span class="sxs-lookup"><span data-stu-id="04c32-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="04c32-160">認証スキームまたはを指定できますが、個人内`[Authorize]`属性または承認ポリシー。</span><span class="sxs-lookup"><span data-stu-id="04c32-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => {
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="04c32-161">次の条件のいずれかが true の場合は、2.0 では、既定のスキームを定義します。</span><span class="sxs-lookup"><span data-stu-id="04c32-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="04c32-162">ユーザーが自動的にサインインします。</span><span class="sxs-lookup"><span data-stu-id="04c32-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="04c32-163">使用する、`[Authorize]`スキームを指定せずに属性または承認のポリシー</span><span class="sxs-lookup"><span data-stu-id="04c32-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="04c32-164">ただし、このルールは、`AddIdentity`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="04c32-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="04c32-165">このメソッドでは、cookie を追加すると、既定の認証およびパターンをアプリケーションの cookie にチャレンジ セット`IdentityConstants.ApplicationScheme`です。</span><span class="sxs-lookup"><span data-stu-id="04c32-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="04c32-166">さらに、外部の cookie を既定のサインイン スキームに設定`IdentityConstants.ExternalScheme`です。</span><span class="sxs-lookup"><span data-stu-id="04c32-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="04c32-167">HttpContext 認証の拡張機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="04c32-167">Use HttpContext Authentication Extensions</span></span>
<span data-ttu-id="04c32-168">`IAuthenticationManager`インターフェイスは、メイン エントリ ポイントを 1.x の認証システムにします。</span><span class="sxs-lookup"><span data-stu-id="04c32-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="04c32-169">新しいセットに置き換わりました`HttpContext`の拡張メソッドにおいて、`Microsoft.AspNetCore.Authentication`名前空間。</span><span class="sxs-lookup"><span data-stu-id="04c32-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="04c32-170">たとえば、1.x が参照をプロジェクトに`Authentication`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="04c32-170">For example, 1.x projects reference an `Authentication` property:</span></span>

<span data-ttu-id="04c32-171">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]</span><span class="sxs-lookup"><span data-stu-id="04c32-171">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]</span></span>

<span data-ttu-id="04c32-172">プロジェクトでは 2.0、インポート、`Microsoft.AspNetCore.Authentication`名前空間、および削除、`Authentication`プロパティ参照。</span><span class="sxs-lookup"><span data-stu-id="04c32-172">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

<span data-ttu-id="04c32-173">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]</span><span class="sxs-lookup"><span data-stu-id="04c32-173">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]</span></span>

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="04c32-174">Windows 認証 (HTTP.sys/IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="04c32-174">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="04c32-175">Windows 認証の 2 つのバリエーションがあります。</span><span class="sxs-lookup"><span data-stu-id="04c32-175">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="04c32-176">ホストは認証されたユーザーだけを許可します。</span><span class="sxs-lookup"><span data-stu-id="04c32-176">The host only allows authenticated users</span></span>
2. <span data-ttu-id="04c32-177">により、ホストし、認証されたユーザー</span><span class="sxs-lookup"><span data-stu-id="04c32-177">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="04c32-178">上記で説明した最初のコマンドは、2.0 の変更による影響はありません。</span><span class="sxs-lookup"><span data-stu-id="04c32-178">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="04c32-179">上記で説明した 2 番目のコマンドは 2.0 の変更の影響を受けます。</span><span class="sxs-lookup"><span data-stu-id="04c32-179">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="04c32-180">例として、する可能性があることの匿名ユーザーを IIS でアプリケーションにまたは[HTTP.sys](xref:fundamentals/servers/weblistener)コント ローラー レベルの認証がユーザーのレイヤーします。</span><span class="sxs-lookup"><span data-stu-id="04c32-180">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="04c32-181">このシナリオでは、既定のスキームを設定`IISDefaults.AuthenticationScheme`で、`ConfigureServices`メソッドの*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="04c32-181">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="04c32-182">それに応じてにより、作業からチャレンジに承認要求が、既定のスキームの設定に失敗しました。</span><span class="sxs-lookup"><span data-stu-id="04c32-182">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="04c32-183">IdentityCookieOptions インスタンス</span><span class="sxs-lookup"><span data-stu-id="04c32-183">IdentityCookieOptions Instances</span></span>
<span data-ttu-id="04c32-184">2.0 の変更の副作用は、cookie のオプションのインスタンスではなくオプションをという名前を使用するスイッチです。</span><span class="sxs-lookup"><span data-stu-id="04c32-184">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="04c32-185">Id cookie のスキーム名をカスタマイズする機能が削除されます。</span><span class="sxs-lookup"><span data-stu-id="04c32-185">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="04c32-186">1.x での使用のプロジェクトなど、[コンス トラクター インジェクション](xref:mvc/controllers/dependency-injection#constructor-injection)に渡す、`IdentityCookieOptions`にパラメーター *AccountController.cs*です。</span><span class="sxs-lookup"><span data-stu-id="04c32-186">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="04c32-187">外部の cookie 認証スキームは指定されたインスタンスからアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="04c32-187">The external cookie authentication scheme is accessed from the provided instance:</span></span>

<span data-ttu-id="04c32-188">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]</span><span class="sxs-lookup"><span data-stu-id="04c32-188">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]</span></span>

<span data-ttu-id="04c32-189">ここに挙げたコンス トラクター インジェクションがプロジェクトでは 2.0、不要になると`_externalCookieScheme`フィールドを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="04c32-189">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

<span data-ttu-id="04c32-190">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]</span><span class="sxs-lookup"><span data-stu-id="04c32-190">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]</span></span>

<span data-ttu-id="04c32-191">`IdentityConstants.ExternalScheme`定数を直接使用することができます。</span><span class="sxs-lookup"><span data-stu-id="04c32-191">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

<span data-ttu-id="04c32-192">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]</span><span class="sxs-lookup"><span data-stu-id="04c32-192">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]</span></span>

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="04c32-193">IdentityUser POCO ナビゲーション プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="04c32-193">Add IdentityUser POCO Navigation Properties</span></span>
<span data-ttu-id="04c32-194">ベースの Entity Framework (EF) 中核となるナビゲーション プロパティ`IdentityUser`POCO (Plain Old CLR Object) が削除されました。</span><span class="sxs-lookup"><span data-stu-id="04c32-194">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="04c32-195">1.x プロジェクトでは、これらのプロパティを使用する場合は、この 2.0 のプロジェクトに追加手動でします。</span><span class="sxs-lookup"><span data-stu-id="04c32-195">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="04c32-196">外部キーの重複を防ぐためには、EF コア移行を実行するときに、追加、次のように、`IdentityDbContext`クラス`OnModelCreating`メソッド (後、`base.OnModelCreating();`を呼び出す)。</span><span class="sxs-lookup"><span data-stu-id="04c32-196">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="04c32-197">GetExternalAuthenticationSchemes を置き換えます</span><span class="sxs-lookup"><span data-stu-id="04c32-197">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="04c32-198">同期メソッド`GetExternalAuthenticationSchemes`非同期バージョンを優先するために削除されました。</span><span class="sxs-lookup"><span data-stu-id="04c32-198">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="04c32-199">1.x プロジェクトに次のコードがある*ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="04c32-199">1.x projects have the following code in *ManageController.cs*:</span></span>

<span data-ttu-id="04c32-200">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]</span><span class="sxs-lookup"><span data-stu-id="04c32-200">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]</span></span>

<span data-ttu-id="04c32-201">このメソッドは、 *Login.cshtml*すぎます。</span><span class="sxs-lookup"><span data-stu-id="04c32-201">This method appears in *Login.cshtml* too:</span></span>

<span data-ttu-id="04c32-202">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Account/Login.cshtml?range=62,75-84)]</span><span class="sxs-lookup"><span data-stu-id="04c32-202">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Account/Login.cshtml?range=62,75-84)]</span></span>

<span data-ttu-id="04c32-203">2.0 のプロジェクトで使用して、`GetExternalAuthenticationSchemesAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="04c32-203">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

<span data-ttu-id="04c32-204">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]</span><span class="sxs-lookup"><span data-stu-id="04c32-204">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]</span></span>

<span data-ttu-id="04c32-205">*Login.cshtml*、`AuthenticationScheme`でアクセスされるプロパティ、`foreach`ループに変更`Name`:</span><span class="sxs-lookup"><span data-stu-id="04c32-205">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

<span data-ttu-id="04c32-206">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Views/Account/Login.cshtml?range=62,75-84)]</span><span class="sxs-lookup"><span data-stu-id="04c32-206">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Views/Account/Login.cshtml?range=62,75-84)]</span></span>

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="04c32-207">ManageLoginsViewModel プロパティの変更</span><span class="sxs-lookup"><span data-stu-id="04c32-207">ManageLoginsViewModel Property Change</span></span>
<span data-ttu-id="04c32-208">A`ManageLoginsViewModel`でオブジェクトを使用して、`ManageLogins`のアクション*ManageController.cs*です。</span><span class="sxs-lookup"><span data-stu-id="04c32-208">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="04c32-209">1.x のプロジェクトで、オブジェクトの`OtherLogins`プロパティの戻り型が`IList<AuthenticationDescription>`です。</span><span class="sxs-lookup"><span data-stu-id="04c32-209">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="04c32-210">この戻り値の型のインポートを必要と`Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="04c32-210">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

<span data-ttu-id="04c32-211">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]</span><span class="sxs-lookup"><span data-stu-id="04c32-211">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]</span></span>

<span data-ttu-id="04c32-212">2.0 プロジェクトでは、戻り値の型の変更`IList<AuthenticationScheme>`です。</span><span class="sxs-lookup"><span data-stu-id="04c32-212">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="04c32-213">この新しいの戻り値の型は、置き換える必要があります、`Microsoft.AspNetCore.Http.Authentication`と共にインポート、`Microsoft.AspNetCore.Authentication`をインポートします。</span><span class="sxs-lookup"><span data-stu-id="04c32-213">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

<span data-ttu-id="04c32-214">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]</span><span class="sxs-lookup"><span data-stu-id="04c32-214">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]</span></span>

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="04c32-215">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="04c32-215">Additional Resources</span></span>
<span data-ttu-id="04c32-216">追加の詳細についてを参照してください、 [Auth 2.0 のディスカッション](https://github.com/aspnet/Security/issues/1338)GitHub の問題です。</span><span class="sxs-lookup"><span data-stu-id="04c32-216">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
