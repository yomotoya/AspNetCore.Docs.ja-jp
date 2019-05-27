---
title: ASP.NET Core 2.0 への認証と Id を移行します。
author: scottaddie
description: この記事では、ASP.NET Core 2.0 に移行する ASP.NET Core 1.x の認証と Id の最も一般的な手順について説明します。
ms.author: scaddie
ms.date: 12/18/2018
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 086deac51af186012315d5b6a1236c92c8980037
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/23/2019
ms.locfileid: "66039238"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="32f63-103">ASP.NET Core 2.0 への認証と Id を移行します。</span><span class="sxs-lookup"><span data-stu-id="32f63-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="32f63-104">によって[Scott Addie](https://github.com/scottaddie)と[Hao 力](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="32f63-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="32f63-105">ASP.NET Core 2.0 が認証用の新しいモデルおよび[Identity](xref:security/authentication/identity)サービスを使用して構成を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="32f63-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="32f63-106">認証または Id を使用する ASP.NET Core 1.x アプリケーションは、以下に示すとおり、新しいモデルを使用して更新できます。</span><span class="sxs-lookup"><span data-stu-id="32f63-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="32f63-107">認証ミドルウェアとサービス</span><span class="sxs-lookup"><span data-stu-id="32f63-107">Authentication Middleware and services</span></span>

<span data-ttu-id="32f63-108">1.x プロジェクトでは、認証はミドルウェアを介して構成されます。</span><span class="sxs-lookup"><span data-stu-id="32f63-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="32f63-109">サポートする各認証スキーム ミドルウェア メソッドは呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="32f63-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="32f63-110">次の 1.x の例では、Facebook 認証を構成で Id を持つ*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="32f63-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="32f63-111">2.0 プロジェクトでサービスを使用して認証が構成されています。</span><span class="sxs-lookup"><span data-stu-id="32f63-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="32f63-112">各認証スキームが登録されている、`ConfigureServices`メソッドの*Startup.cs*します。</span><span class="sxs-lookup"><span data-stu-id="32f63-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="32f63-113">`UseIdentity`メソッドが置き換え`UseAuthentication`します。</span><span class="sxs-lookup"><span data-stu-id="32f63-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="32f63-114">次の 2.0 の例では、Facebook 認証を構成で Id を持つ*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="32f63-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="32f63-115">`UseAuthentication`メソッドは、自動認証と、リモート認証要求の処理を担当する 1 つの認証ミドルウェア コンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="32f63-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="32f63-116">1 つの一般的なミドルウェア コンポーネントをすべての個々 のミドルウェア コンポーネントを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="32f63-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="32f63-117">各主要な認証スキーム用の 2.0 の移行手順を次に示します。</span><span class="sxs-lookup"><span data-stu-id="32f63-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="32f63-118">Cookie ベースの認証</span><span class="sxs-lookup"><span data-stu-id="32f63-118">Cookie-based authentication</span></span>

<span data-ttu-id="32f63-119">次の 2 つのオプションのいずれかを選択しに必要な変更を加えます*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="32f63-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="32f63-120">Id で cookie を使用します。</span><span class="sxs-lookup"><span data-stu-id="32f63-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="32f63-121">置換`UseIdentity`で`UseAuthentication`で、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="32f63-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="32f63-122">呼び出す、`AddIdentity`メソッド、 `ConfigureServices` cookie 認証サービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="32f63-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="32f63-123">必要に応じて、呼び出し、`ConfigureApplicationCookie`または`ConfigureExternalCookie`メソッドで、 `ConfigureServices` Id cookie の設定を調整する方法。</span><span class="sxs-lookup"><span data-stu-id="32f63-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="32f63-124">Identity なしの cookie を使用します。</span><span class="sxs-lookup"><span data-stu-id="32f63-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="32f63-125">置換、`UseCookieAuthentication`でメソッドを呼び出す、`Configure`メソッド`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="32f63-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="32f63-126">呼び出す、`AddAuthentication`と`AddCookie`メソッド、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="32f63-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="32f63-127">JWT ベアラー認証</span><span class="sxs-lookup"><span data-stu-id="32f63-127">JWT Bearer Authentication</span></span>

<span data-ttu-id="32f63-128">次に変更を加えます*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="32f63-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="32f63-129">置換、`UseJwtBearerAuthentication`でメソッドを呼び出す、`Configure`メソッド`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="32f63-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="32f63-130">呼び出す、`AddJwtBearer`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="32f63-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="32f63-131">このコード スニペットが Id を使用しないのは、渡すことによって、既定のスキームを設定する必要があるため`JwtBearerDefaults.AuthenticationScheme`を`AddAuthentication`メソッド。</span><span class="sxs-lookup"><span data-stu-id="32f63-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="32f63-132">OpenID Connect (OIDC) 認証</span><span class="sxs-lookup"><span data-stu-id="32f63-132">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="32f63-133">次に変更を加えます*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="32f63-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="32f63-134">置換、`UseOpenIdConnectAuthentication`でメソッドを呼び出す、`Configure`メソッド`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="32f63-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="32f63-135">呼び出す、`AddOpenIdConnect`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="32f63-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

- <span data-ttu-id="32f63-136">置換、`PostLogoutRedirectUri`プロパティ、`OpenIdConnectOptions`ェマル`SignedOutRedirectUri`:</span><span class="sxs-lookup"><span data-stu-id="32f63-136">Replace the `PostLogoutRedirectUri` property in the `OpenIdConnectOptions` action with `SignedOutRedirectUri`:</span></span>

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a><span data-ttu-id="32f63-137">Facebook での認証</span><span class="sxs-lookup"><span data-stu-id="32f63-137">Facebook authentication</span></span>

<span data-ttu-id="32f63-138">次に変更を加えます*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="32f63-138">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="32f63-139">置換、`UseFacebookAuthentication`でメソッドを呼び出す、`Configure`メソッド`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="32f63-139">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="32f63-140">呼び出す、`AddFacebook`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="32f63-140">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="32f63-141">Google での認証</span><span class="sxs-lookup"><span data-stu-id="32f63-141">Google authentication</span></span>

<span data-ttu-id="32f63-142">次に変更を加えます*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="32f63-142">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="32f63-143">置換、`UseGoogleAuthentication`でメソッドを呼び出す、`Configure`メソッド`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="32f63-143">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="32f63-144">呼び出す、`AddGoogle`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="32f63-144">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="32f63-145">Microsoft アカウント認証</span><span class="sxs-lookup"><span data-stu-id="32f63-145">Microsoft Account authentication</span></span>

<span data-ttu-id="32f63-146">次に変更を加えます*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="32f63-146">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="32f63-147">置換、`UseMicrosoftAccountAuthentication`でメソッドを呼び出す、`Configure`メソッド`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="32f63-147">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="32f63-148">呼び出す、`AddMicrosoftAccount`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="32f63-148">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="32f63-149">Twitter での認証</span><span class="sxs-lookup"><span data-stu-id="32f63-149">Twitter authentication</span></span>

<span data-ttu-id="32f63-150">次に変更を加えます*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="32f63-150">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="32f63-151">置換、`UseTwitterAuthentication`でメソッドを呼び出す、`Configure`メソッド`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="32f63-151">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="32f63-152">呼び出す、`AddTwitter`メソッドで、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="32f63-152">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="32f63-153">認証スキームの既定の設定</span><span class="sxs-lookup"><span data-stu-id="32f63-153">Setting default authentication schemes</span></span>

<span data-ttu-id="32f63-154">1.x で、`AutomaticAuthenticate`と`AutomaticChallenge`のプロパティ、 [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1)基底クラスが 1 つの認証スキームに設定する対象としています。</span><span class="sxs-lookup"><span data-stu-id="32f63-154">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="32f63-155">これを強制する良い方法はありませんでした。</span><span class="sxs-lookup"><span data-stu-id="32f63-155">There was no good way to enforce this.</span></span>

<span data-ttu-id="32f63-156">個々 のプロパティとして 2.0 では、これら 2 つのプロパティが削除されました`AuthenticationOptions`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="32f63-156">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="32f63-157">構成することができますが、`AddAuthentication`内でメソッドの呼び出し、`ConfigureServices`メソッドの*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="32f63-157">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="32f63-158">上記のコード スニペットでは、既定のスキームに設定されて`CookieAuthenticationDefaults.AuthenticationScheme`(「クッキー」)。</span><span class="sxs-lookup"><span data-stu-id="32f63-158">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="32f63-159">またはのオーバー ロード バージョンを使用して、`AddAuthentication`メソッドは、複数のプロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="32f63-159">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="32f63-160">次のオーバー ロードされたメソッドの例では、既定のスキームに設定されて`CookieAuthenticationDefaults.AuthenticationScheme`します。</span><span class="sxs-lookup"><span data-stu-id="32f63-160">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="32f63-161">認証スキームは、個別にまたは指定できます`[Authorize]`属性または承認ポリシー。</span><span class="sxs-lookup"><span data-stu-id="32f63-161">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="32f63-162">次の条件のいずれかが true の場合は、2.0 で既定のスキームを定義します。</span><span class="sxs-lookup"><span data-stu-id="32f63-162">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="32f63-163">ユーザーが自動的にサインインします。</span><span class="sxs-lookup"><span data-stu-id="32f63-163">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="32f63-164">使用する、`[Authorize]`スキームを指定せずに属性または承認のポリシー</span><span class="sxs-lookup"><span data-stu-id="32f63-164">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="32f63-165">このルールの例外は、`AddIdentity`メソッド。</span><span class="sxs-lookup"><span data-stu-id="32f63-165">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="32f63-166">このメソッドでは、cookie を追加して、既定の認証し、アプリケーション cookie にスキームの課題のセットの`IdentityConstants.ApplicationScheme`します。</span><span class="sxs-lookup"><span data-stu-id="32f63-166">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="32f63-167">さらに、外部の cookie を既定のサインイン方式を設定`IdentityConstants.ExternalScheme`します。</span><span class="sxs-lookup"><span data-stu-id="32f63-167">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="32f63-168">HttpContext の認証拡張機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="32f63-168">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="32f63-169">`IAuthenticationManager`インターフェイスは 1.x の認証システムにメイン エントリ ポイントです。</span><span class="sxs-lookup"><span data-stu-id="32f63-169">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="32f63-170">新しいセットで置き換え`HttpContext`の拡張メソッド、`Microsoft.AspNetCore.Authentication`名前空間。</span><span class="sxs-lookup"><span data-stu-id="32f63-170">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="32f63-171">たとえば、1.x プロジェクトを参照する`Authentication`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="32f63-171">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="32f63-172">2.0 プロジェクトでは、インポート、`Microsoft.AspNetCore.Authentication`名前空間、および削除、`Authentication`プロパティの参照。</span><span class="sxs-lookup"><span data-stu-id="32f63-172">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="32f63-173">Windows 認証 (HTTP.sys/IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="32f63-173">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="32f63-174">Windows 認証の 2 つのバリエーションがあります。</span><span class="sxs-lookup"><span data-stu-id="32f63-174">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="32f63-175">ホストでは、認証されたユーザーのみで許可します。</span><span class="sxs-lookup"><span data-stu-id="32f63-175">The host only allows authenticated users</span></span>
2. <span data-ttu-id="32f63-176">により、ホストし、認証されたユーザー</span><span class="sxs-lookup"><span data-stu-id="32f63-176">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="32f63-177">上記で説明した最初のコマンドは、2.0 の変更による影響はありません。</span><span class="sxs-lookup"><span data-stu-id="32f63-177">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="32f63-178">上記で説明した 2 つ目のバリエーションは 2.0 の変更の影響を受けます。</span><span class="sxs-lookup"><span data-stu-id="32f63-178">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="32f63-179">例として、する可能性があること匿名ユーザーに、IIS でアプリまたは[HTTP.sys](xref:fundamentals/servers/httpsys)レイヤーのコント ローラー レベルで、承認ユーザー。</span><span class="sxs-lookup"><span data-stu-id="32f63-179">As an example, you may be allowing anonymous users into your app at the IIS or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="32f63-180">このシナリオで、既定のスキームを設定`IISDefaults.AuthenticationScheme`で、`Startup.ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="32f63-180">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="32f63-181">それに応じてにより、作業からチャレンジを承認要求を既定のスキームを設定できません。</span><span class="sxs-lookup"><span data-stu-id="32f63-181">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="32f63-182">IdentityCookieOptions インスタンス</span><span class="sxs-lookup"><span data-stu-id="32f63-182">IdentityCookieOptions instances</span></span>

<span data-ttu-id="32f63-183">2.0 の変更の副作用は、という名前の cookie のオプションのインスタンスではなくオプションを使用するスイッチです。</span><span class="sxs-lookup"><span data-stu-id="32f63-183">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="32f63-184">Id cookie のスキーム名をカスタマイズする機能が削除されます。</span><span class="sxs-lookup"><span data-stu-id="32f63-184">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="32f63-185">たとえば、1.x プロジェクトを使用[コンス トラクターの挿入](xref:mvc/controllers/dependency-injection#constructor-injection)を渡す、`IdentityCookieOptions`パラメーターに*AccountController.cs*します。</span><span class="sxs-lookup"><span data-stu-id="32f63-185">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="32f63-186">外部の cookie 認証スキームは、指定されたインスタンスからアクセスされます。</span><span class="sxs-lookup"><span data-stu-id="32f63-186">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="32f63-187">前述のコンス トラクターの挿入が 2.0 プロジェクトで不要になると、`_externalCookieScheme`フィールドを削除できます。</span><span class="sxs-lookup"><span data-stu-id="32f63-187">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="32f63-188">`IdentityConstants.ExternalScheme`定数を直接使用することができます。</span><span class="sxs-lookup"><span data-stu-id="32f63-188">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="32f63-189">IdentityUser POCO ナビゲーション プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="32f63-189">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="32f63-190">Entity Framework (EF) Core ナビゲーションのプロパティ ベースの`IdentityUser`POCO (Plain Old CLR Object) が削除されました。</span><span class="sxs-lookup"><span data-stu-id="32f63-190">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="32f63-191">1.x プロジェクトでは、これらのプロパティを使用する場合は、この 2.0 プロジェクトに追加手動でします。</span><span class="sxs-lookup"><span data-stu-id="32f63-191">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="32f63-192">外部キーの重複を防ぐためには、EF Core の移行を実行するときに、追加するには、次の`IdentityDbContext`クラス`OnModelCreating`メソッド (後に、`base.OnModelCreating();`呼び出す)。</span><span class="sxs-lookup"><span data-stu-id="32f63-192">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Core Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Core Identity table names and more.
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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="32f63-193">GetExternalAuthenticationSchemes を置き換えます</span><span class="sxs-lookup"><span data-stu-id="32f63-193">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="32f63-194">同期メソッド`GetExternalAuthenticationSchemes`非同期バージョンを優先して削除されました。</span><span class="sxs-lookup"><span data-stu-id="32f63-194">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="32f63-195">1.x プロジェクトに次のコードがある*ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="32f63-195">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="32f63-196">このメソッドでは、 *Login.cshtml*すぎます。</span><span class="sxs-lookup"><span data-stu-id="32f63-196">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="32f63-197">2.0 プロジェクトで使用して、`GetExternalAuthenticationSchemesAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="32f63-197">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="32f63-198">*Login.cshtml*、`AuthenticationScheme`でアクセスされるプロパティ、`foreach`ループに変更`Name`:</span><span class="sxs-lookup"><span data-stu-id="32f63-198">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="32f63-199">ManageLoginsViewModel プロパティの変更</span><span class="sxs-lookup"><span data-stu-id="32f63-199">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="32f63-200">A`ManageLoginsViewModel`でオブジェクトを使用して、`ManageLogins`のアクション*ManageController.cs*します。</span><span class="sxs-lookup"><span data-stu-id="32f63-200">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="32f63-201">1.x プロジェクトで、オブジェクトの`OtherLogins`プロパティの戻り型が`IList<AuthenticationDescription>`します。</span><span class="sxs-lookup"><span data-stu-id="32f63-201">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="32f63-202">この戻り値の型がインポートする必要があります`Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="32f63-202">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="32f63-203">2.0 プロジェクトでは、戻り値の型の変更`IList<AuthenticationScheme>`します。</span><span class="sxs-lookup"><span data-stu-id="32f63-203">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="32f63-204">この新しい戻り値の型は、置き換える必要があります、`Microsoft.AspNetCore.Http.Authentication`とインポートを`Microsoft.AspNetCore.Authentication`をインポートします。</span><span class="sxs-lookup"><span data-stu-id="32f63-204">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="32f63-205">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="32f63-205">Additional resources</span></span>

<span data-ttu-id="32f63-206">追加の詳細情報とディスカッションでは、次を参照してください。、 [Auth 2.0 のディスカッション](https://github.com/aspnet/Security/issues/1338)GitHub 上の問題。</span><span class="sxs-lookup"><span data-stu-id="32f63-206">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
