---
title: "移行の認証および ASP.NET Core 2.0 を Id"
author: scottaddie
description: "この記事では、ASP.NET Core 2.0 に移行する ASP.NET Core 1.x 認証と Id の最も一般的な手順について説明します。"
ms.author: scaddie
manager: wpickett
ms.date: 10/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 72ad31438a344fb5fa2b357c709b923b8077e742
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a>移行の認証および ASP.NET Core 2.0 を Id

によって[Scott Addie](https://github.com/scottaddie)と[ハオ最後にトリム](https://github.com/HaoK)

ASP.NET Core 2.0 が認証用の新しいモデルと[Identity](xref:security/authentication/identity) services を使用して構成を簡略化します。 認証または Id を使用する ASP.NET Core 1.x アプリケーションは、下記の手順に従って、新しいモデルを使用して更新できます。

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>認証ミドルウェアとサービス
1.x プロジェクトでは、ミドルウェアを介して認証を構成します。 各認証スキームをサポートするために、ミドルウェア メソッドが呼び出されます。

次の 1.x 例では、Facebook 認証を構成で Id を持つ*Startup.cs*:

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

2.0 プロジェクトでは、サービスを使用して認証を構成します。 各認証スキームが登録されている、`ConfigureServices`メソッドの*Startup.cs*です。 `UseIdentity`メソッドが置き換え`UseAuthentication`です。

次の 2.0 の例では、Facebook 認証を構成で Id を持つ*Startup.cs*:

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

`UseAuthentication`メソッドは、自動認証およびリモート認証要求の処理を担当する 1 つの認証ミドルウェア コンポーネントを追加します。 単一の一般的なミドルウェア コンポーネントをすべての個別のミドルウェア コンポーネントに置き換えます。

各主要な認証スキーム用 2.0 の移行手順のとおりです。

### <a name="cookie-based-authentication"></a>Cookie ベースの認証
次の 2 つのオプションのいずれかを選択しに必要な変更を加える*Startup.cs*:

1. Id を持つ cookie を使用します。
    - 置き換える`UseIdentity`で`UseAuthentication`で、`Configure`メソッド。

        ```csharp
        app.UseAuthentication();
        ```

    - 呼び出す、`AddIdentity`メソッドで、 `ConfigureServices` cookie 認証サービスを追加するメソッド。
    - 必要に応じて、呼び出し、`ConfigureApplicationCookie`または`ConfigureExternalCookie`メソッドで、 `ConfigureServices` Id cookie の設定を調整する方法です。

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Identity せず cookie を使用します。
    - 置換、`UseCookieAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - 呼び出す、`AddAuthentication`と`AddCookie`内のメソッド、`ConfigureServices`メソッド。

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

### <a name="jwt-bearer-authentication"></a>JWT ベアラ認証
次の変更を加え*Startup.cs*:
- 置換、`UseJwtBearerAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- 呼び出す、`AddJwtBearer`メソッドで、`ConfigureServices`メソッド。

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    渡すことによって、既定のスキームを設定する必要がありますので、このコード スニペットは Id を使用しない`JwtBearerDefaults.AuthenticationScheme`を`AddAuthentication`メソッドです。

### <a name="openid-connect-oidc-authentication"></a>OpenID Connect (OIDC) 認証
次の変更を加え*Startup.cs*:

- 置換、`UseOpenIdConnectAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- 呼び出す、`AddOpenIdConnect`メソッドで、`ConfigureServices`メソッド。

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

### <a name="facebook-authentication"></a>Facebook の認証
次の変更を加え*Startup.cs*:
- 置換、`UseFacebookAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- 呼び出す、`AddFacebook`メソッドで、`ConfigureServices`メソッド。
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Google 認証
次の変更を加え*Startup.cs*:
- 置換、`UseGoogleAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- 呼び出す、`AddGoogle`メソッドで、`ConfigureServices`メソッド。

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a>Microsoft アカウントの認証
次の変更を加え*Startup.cs*:
- 置換、`UseMicrosoftAccountAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- 呼び出す、`AddMicrosoftAccount`メソッドで、`ConfigureServices`メソッド。

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a>Twitter 認証
次の変更を加え*Startup.cs*:
- 置換、`UseTwitterAuthentication`メソッドの呼び出し、`Configure`メソッドを`UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- 呼び出す、`AddTwitter`メソッドで、`ConfigureServices`メソッド。

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>既定の認証スキームの設定
1.x で、`AutomaticAuthenticate`と`AutomaticChallenge`のプロパティ、 [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1)基底クラスが 1 つの認証スキームに設定するためのものです。 これを適用することをお勧めはありませんでした。

個々 のプロパティとして 2.0 では、これら 2 つのプロパティが削除されて`AuthenticationOptions`インスタンス。 構成することができます、`AddAuthentication`内でメソッドの呼び出し、`ConfigureServices`メソッドの*Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

上記のコード スニペットで、既定のスキームが に設定されている`CookieAuthenticationDefaults.AuthenticationScheme`("Cookie") です。

または、オーバー ロードされたバージョンを使用して、 `AddAuthentication` 1 つ以上のプロパティを設定します。 次のオーバー ロードされたメソッドの例では、既定のスキームが に設定されている`CookieAuthenticationDefaults.AuthenticationScheme`です。 認証スキームまたはを指定できますが、個人内`[Authorize]`属性または承認ポリシー。

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

次の条件のいずれかが true の場合は、2.0 では、既定のスキームを定義します。
- ユーザーが自動的にサインインします。
- 使用する、`[Authorize]`スキームを指定せずに属性または承認のポリシー

ただし、このルールは、`AddIdentity`メソッドです。 このメソッドでは、cookie を追加すると、既定の認証およびパターンをアプリケーションの cookie にチャレンジ セット`IdentityConstants.ApplicationScheme`です。 さらに、外部の cookie を既定のサインイン スキームに設定`IdentityConstants.ExternalScheme`です。

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>HttpContext 認証の拡張機能を使用します。
`IAuthenticationManager`インターフェイスは、メイン エントリ ポイントを 1.x の認証システムにします。 新しいセットに置き換わりました`HttpContext`の拡張メソッドにおいて、`Microsoft.AspNetCore.Authentication`名前空間。

たとえば、1.x が参照をプロジェクトに`Authentication`プロパティ。

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

プロジェクトでは 2.0、インポート、`Microsoft.AspNetCore.Authentication`名前空間、および削除、`Authentication`プロパティ参照。

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Windows 認証 (HTTP.sys/IISIntegration)
Windows 認証の 2 つのバリエーションがあります。
1. ホストは認証されたユーザーだけを許可します。
2. により、ホストし、認証されたユーザー

上記で説明した最初のコマンドは、2.0 の変更による影響はありません。

上記で説明した 2 番目のコマンドは 2.0 の変更の影響を受けます。 例として、する可能性があることの匿名ユーザーを IIS でアプリケーションにまたは[HTTP.sys](xref:fundamentals/servers/weblistener)コント ローラー レベルの認証がユーザーのレイヤーします。 このシナリオでは、既定のスキームを設定`IISDefaults.AuthenticationScheme`で、`ConfigureServices`メソッドの*Startup.cs*:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

それに応じてにより、作業からチャレンジに承認要求が、既定のスキームの設定に失敗しました。

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>IdentityCookieOptions Instances
2.0 の変更の副作用は、cookie のオプションのインスタンスではなくオプションをという名前を使用するスイッチです。 Id cookie のスキーム名をカスタマイズする機能が削除されます。

1.x での使用のプロジェクトなど、[コンス トラクター インジェクション](xref:mvc/controllers/dependency-injection#constructor-injection)に渡す、`IdentityCookieOptions`にパラメーター *AccountController.cs*です。 外部の cookie 認証スキームは指定されたインスタンスからアクセスできます。

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

ここに挙げたコンス トラクター インジェクションがプロジェクトでは 2.0、不要になると`_externalCookieScheme`フィールドを削除することができます。

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

`IdentityConstants.ExternalScheme`定数を直接使用することができます。

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>IdentityUser POCO ナビゲーション プロパティを追加します。
ベースの Entity Framework (EF) 中核となるナビゲーション プロパティ`IdentityUser`POCO (Plain Old CLR Object) が削除されました。 1.x プロジェクトでは、これらのプロパティを使用する場合は、この 2.0 のプロジェクトに追加手動でします。

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

外部キーの重複を防ぐためには、EF コア移行を実行するときに、追加、次のように、`IdentityDbContext`クラス`OnModelCreating`メソッド (後、`base.OnModelCreating();`を呼び出す)。

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

## <a name="replace-getexternalauthenticationschemes"></a>GetExternalAuthenticationSchemes を置き換えます
同期メソッド`GetExternalAuthenticationSchemes`非同期バージョンを優先するために削除されました。 1.x プロジェクトに次のコードがある*ManageController.cs*:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

このメソッドは、 *Login.cshtml*すぎます。

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

2.0 のプロジェクトで使用して、`GetExternalAuthenticationSchemesAsync`メソッド。

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

*Login.cshtml*、`AuthenticationScheme`でアクセスされるプロパティ、`foreach`ループに変更`Name`:

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>ManageLoginsViewModel プロパティの変更
A`ManageLoginsViewModel`でオブジェクトを使用して、`ManageLogins`のアクション*ManageController.cs*です。 1.x のプロジェクトで、オブジェクトの`OtherLogins`プロパティの戻り型が`IList<AuthenticationDescription>`です。 この戻り値の型のインポートを必要と`Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

2.0 プロジェクトでは、戻り値の型の変更`IList<AuthenticationScheme>`です。 この新しいの戻り値の型は、置き換える必要があります、`Microsoft.AspNetCore.Http.Authentication`と共にインポート、`Microsoft.AspNetCore.Authentication`をインポートします。

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>その他のリソース
追加の詳細についてを参照してください、 [Auth 2.0 のディスカッション](https://github.com/aspnet/Security/issues/1338)GitHub の問題です。
