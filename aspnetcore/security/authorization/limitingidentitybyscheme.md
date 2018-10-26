---
title: ASP.NET Core での特定のスキームで承認します。
author: rick-anderson
description: この記事では、複数の認証方法を使用する場合は、id を特定のスキームを制限する方法について説明します。
ms.author: riande
ms.date: 10/22/2018
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: fbe9f32e01a214f41b5a6e9f43e8fdee5fc612df
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089397"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>ASP.NET Core での特定のスキームで承認します。

シングル ページ アプリケーション (Spa) など、いくつかのシナリオでは、複数の認証方法を使用する一般的なです。 など、アプリのログインに cookie ベースの認証および使用 JWT ベアラー認証 JavaScript 要求。 場合によっては、アプリは、認証ハンドラーの複数のインスタンスがあります。 たとえば、id 基本的なには 1 つが含まれている 2 つの cookie ハンドラーと 1 つは多要素認証 (MFA) がトリガーされたときに作成されます。 MFA は、ユーザーは、追加のセキュリティを必要とする操作を要求したため、トリガーされる可能性があります。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

認証サービスが認証時に構成されている場合は、認証方式がという名前です。 例えば:

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

上記のコードでは、2 つの認証ハンドラーが追加されています。 cookie とベアラーのいずれかのいずれか。

>[!NOTE]
>既定のスキームを指定する、`HttpContext.User`プロパティがその id に設定されています。 その動作が必要ない場合は無効のパラメーターなしのフォームを呼び出すことによって`AddAuthentication`します。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

認証方式は、認証ミドルウェアが認証時に構成されている場合に名前です。 例えば:

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

上記のコードでは、2 つの認証ミドルウェアが追加されています。 cookie とベアラーのいずれかのいずれか。

>[!NOTE]
>既定のスキームを指定する、`HttpContext.User`プロパティがその id に設定されています。 その動作が必要ない場合は無効を設定して、`AuthenticationOptions.AutomaticAuthenticate`プロパティを`false`します。

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Authorize 属性を持つスキームを選択します。

承認、時点では、アプリは、使用するハンドラーを示します。 使用する認証スキームのコンマ区切りの一覧を渡すことによって、アプリは承認ハンドラーを選択して`[Authorize]`します。 `[Authorize]`属性は、認証方式、または、既定値が構成されているかどうかに関係なく使用するスキームを指定します。 例えば:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

前の例では、ベアラーとクッキー ハンドラーは実行し、作成して、現在のユーザーの id を追加すること。 1 つのスキームのみを指定すると、対応するハンドラーが実行されます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

上記のコードでは、"Bearer"スキームでハンドラーのみが実行されます。 Cookie ベース id は無視されます。

## <a name="selecting-the-scheme-with-policies"></a>ポリシーの設定を選択します。

必要な方式を指定する場合[ポリシー](xref:security/authorization/policies)を設定することができます、`AuthenticationSchemes`ポリシーを追加するときに、コレクション。

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

前の例では、"over18 という"ポリシーは、"Bearer"ハンドラーによって作成された id に対してのみ実行されます。 設定して、ポリシーを使用して、`[Authorize]`属性の`Policy`プロパティ。

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a>複数の認証方式を使用して、

一部のアプリは、複数の種類の認証をサポートする必要があります。 など、アプリがユーザー データベースと Azure Active Directory からユーザーを認証する可能性があります。 別の例は、Active Directory フェデレーション サービスと Azure Active Directory B2C の両方からユーザーを認証するアプリです。 この場合、アプリでは、いくつかの発行者から JWT ベアラー トークンを受け入れる必要があります。

そのまま使用したいすべての認証スキームを追加します。 次のコード例では、`Startup.ConfigureServices`さまざまな発行者で 2 つの JWT ベアラー認証スキームを追加します。

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
> 既定の認証スキームは 1 つだけの JWT ベアラー認証に登録されて`JwtBearerDefaults.AuthenticationScheme`します。 追加の認証は、一意の認証スキームを登録することができます。

次の手順では、両方の認証方式を受け入れるように既定の承認ポリシーを更新します。 例えば:

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

既定の承認ポリシーがオーバーライドされると、単純なを使用することは`[Authorize]`コント ローラー内の属性。 コント ローラーは、最初または 2 番目の発行者によって発行された JWT に、要求を受け入れます。

::: moniker-end
