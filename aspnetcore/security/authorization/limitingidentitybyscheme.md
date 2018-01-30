---
title: "特定のスキームの ASP.NET Core を承認します。"
author: rick-anderson
description: "この記事では、複数の認証方法を使用する場合は、id、特定のスキームを制限する方法について説明します。"
manager: wpickett
ms.author: riande
ms.date: 10/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: dd044a0829382f9f7f0c3256c6e669340f2d5240
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="authorize-with-a-specific-scheme"></a>特定のスキームを承認します。

単一ページ アプリケーション (SPAs) など、一部のシナリオでは、複数の認証方法を使用する一般的なです。 たとえば、アプリおよび使用できます cookie ベースの認証のログインに JWT ベアラ認証の JavaScript 要求。 アプリの認証ハンドラーの複数のインスタンスことがあります。 たとえば、基本的な id を含む 1 つ、2 つのクッキー ハンドラーと 1 つは多要素認証 (MFA) がトリガーされたときに作成されます。 ユーザーは、追加のセキュリティを必要とする操作を要求したため、MFA をトリガー可能性があります。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

認証サービスが認証時に構成されている場合は、認証スキームがという名前です。 例:

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

上記のコードでは、2 つの認証ハンドラーが追加されました。 cookie とベアラーのいずれかのいずれか。

>[!NOTE]
>既定のスキームを指定する、`HttpContext.User`その id に設定されているプロパティ。 その動作が必要ない場合は無効のパラメーターなしのフォームを呼び出すことによって`AddAuthentication`です。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

認証スキームには、認証 middlewares が認証時に構成されている場合は、という名前です。 例:

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

上記のコードでは、次の 2 つの認証 middlewares が追加されました。 cookie とベアラーのいずれかのいずれか。

>[!NOTE]
>既定のスキームを指定する、`HttpContext.User`その id に設定されているプロパティ。 その動作が必要ない場合は、無効に設定して、`AuthenticationOptions.AutomaticAuthenticate`プロパティを`false`です。

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Authorize attribute のスキームを選択します。

承認、時点では、アプリは、使用するハンドラーを示します。 使用するアプリを認証方式のコンマ区切りの一覧を渡すことによって承認は、ハンドラーを選択して`[Authorize]`です。 `[Authorize]`属性は、認証方式、または、既定値が構成されているかどうかに関係なく使用するスキームを指定します。 例:

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

前の例では、cookie とベアラーの両方のハンドラーは実行し、作成して、現在のユーザーの id を追加することはします。 1 つのスキームのみを指定することで、対応するハンドラーが実行されます。

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

## <a name="selecting-the-scheme-with-policies"></a>ポリシーと設定の選択

必要な方式を指定する場合[ポリシー](xref:security/authorization/policies)、設定することができます、`AuthenticationSchemes`ポリシーを追加するときにコレクション。

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
