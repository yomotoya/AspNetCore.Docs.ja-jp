---
title: "ASP.NET Core Identity なしで認証に Cookie を使用します。"
author: rick-anderson
description: "ASP.NET Core Id なしの cookie 認証を使用しての説明"
keywords: "ASP.NET Core、cookie"
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: ea9c93e34a3242b5b3716404228edb8902baf625
ms.sourcegitcommit: e3b1726cc04e80dc28464c35259edbd3bc39a438
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/12/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>ASP.NET Core Identity なしで認証に Cookie を使用します。

<a name="security-authentication-cookie-middleware"></a>

ASP.NET Core 1.x 提供 cookie[ミドルウェア](../../fundamentals/middleware.md#fundamentals-middleware)暗号化された cookie にユーザー プリンシパルをシリアル化し、次に、後続の要求の cookie を検証するプリンシパルを再作成し、それを`HttpContext.User`プロパティ. 独自のログイン画面とユーザー データベースを提供する場合は、スタンドアロン機能として cookie ミドルウェアを使用できます。

重大な変更点、ASP.NET Core 2.x は cookie ミドルウェアが存在しないことです。 代わりに、`UseAuthentication`メソッドを呼び出し、`Configure`メソッドの*Startup.cs* AuthenticationMiddleware 設定を追加、`HttpContext.User`プロパティです。

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a>追加と構成

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

次の手順を実行します。

- 使用していない場合、 `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage)、バージョン 2.0 以降のインストール、`Microsoft.AspNetCore.Authentication.Cookies`プロジェクトでの NuGet パッケージです。

- 呼び出す、`UseAuthentication`メソッドで、`Configure`のメソッド、 *Startup.cs*ファイル。

    ```csharp
    app.UseAuthentication();
    ```

- 呼び出す、`AddAuthentication`と`AddCookie`内のメソッド、`ConfigureServices`のメソッド、 *Startup.cs*ファイル。

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie("MyCookieAuthenticationScheme", options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

次の手順を実行します。

- インストール、`Microsoft.AspNetCore.Authentication.Cookies`プロジェクトでの NuGet パッケージです。 このパッケージには、cookie ミドルウェアが含まれています。

- 次の行を追加、`Configure`メソッドで、 *Startup.cs*ファイルの前に、`app.UseMvc()`ステートメント。

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AccessDeniedPath = "/Account/Forbidden/",
        AuthenticationScheme = "MyCookieAuthenticationScheme",
        AutomaticAuthenticate = true,
        AutomaticChallenge = true,
        LoginPath = "/Account/Unauthorized/"
    });
    ```

---

上記のコード スニペットは、次のオプションの一部またはすべてを構成します。

* `AccessDeniedPath`-これは、ユーザーがリソースにアクセスしようとしています。 いずれかを渡さない場合に要求がリダイレクトする相対パス[承認ポリシー](xref:security/authorization/policies#security-authorization-policies-based)をそのリソース用です。

* `AuthenticationScheme`-これは、特定の cookie 認証スキームを認識する値です。 これは、cookie 認証とアプリの必要性を複数のインスタンスがある場合に便利です[の承認を 1 つのインスタンスを制限する](xref:security/authorization/limitingidentitybyscheme)です。

* `AutomaticAuthenticate`-このフラグは ASP.NET Core のみに関連 1.x です。 Cookie 認証の要求ごとに実行、および検証し、作成された任意のシリアル化されたプリンシパルを再構築しようとしています。 ことを示します。

* `AutomaticChallenge`-このフラグは ASP.NET Core のみに関連 1.x です。 1.x cookie 認証がブラウザーをリダイレクトすることを示します、`LoginPath`または`AccessDeniedPath`承認に失敗するとします。

* `LoginPath`-これは、ユーザーがリソースにアクセスしようとしていますが、認証されていないときに要求がリダイレクトする相対パスです。

[その他のオプション](xref:security/authentication/cookie#security-authentication-cookie-options)cookie 認証を作成する任意のクレームの発行者を設定する機能は、認証 cookie の名前になると、ドメイン、cookie と、cookie でさまざまなセキュリティのプロパティをします。 既定では、cookie 認証は、クッキーを作成などの適切なセキュリティ オプションを使用します。
- クライアント側 JavaScript でクッキーのアクセスを防ぐため HttpOnly フラグの設定
- 要求が HTTPS 経由で旅行の場合、cookie を HTTPS を制限します。

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a>Id cookie を作成します。

構築する必要があります、ユーザー情報を保持する cookie を作成するには[ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)クッキーにシリアル化したい情報を保持します。 作成したら、適切な`ClaimsPrincipal`オブジェクトを次の呼び出し、コント ローラー メソッドの内部。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

これにより、暗号化された cookie を作成し、現在の応答に追加します。 `AuthenticationScheme`中に指定された[構成](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring)を呼び出すときに使用する必要があります`SignInAsync`です。

背後で使用される暗号化は ASP.NET Core[データ保護](xref:security/data-protection/using-data-protection#security-data-protection-getting-started)システムです。 負荷分散、または web ファームを使用して、複数のコンピューターでホストしているかどうかは、する必要があります[データ保護を構成する](xref:security/data-protection/configuration/overview#data-protection-configuring)同じキーのリングとのアプリケーション識別子を使用します。

## <a name="signing-out"></a>サインアウト

現在のユーザーからサインアウトするか、クッキーを削除して、次を呼び出す内部コント ローラー。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a>バックエンドの変更に反応します。

>[!WARNING]
> プリンシパルの cookie が作成されたら、id の 1 つのソースになります。 バックエンド システムでユーザーを無効にする場合でも cookie 認証は、この情報を持たないし、状態を維持でログに記録されたその cookie が有効な限り、します。

Cookie 認証では、一連のオプション クラスでイベントを提供します。 `ValidateAsync()`イベントをインターセプトし、cookie id の検証のオーバーライドに使用できます。

"LastChanged"列を持たない可能性のあるバック エンド ユーザー データベースを検討してください。 データベースが変更されたときに、cookie を無効にするためにする必要があります最初、[クッキーを作成する](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie)、現在の値を含む"LastChanged"クレームを追加します。 データベースが変更されたときに、"LastChanged"値を更新する必要があります。

オーバーライドを実装する、`ValidateAsync()`イベントでは、次のシグネチャを持つメソッドを記述する必要があります。

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

ASP.NET Core Id では、このチェックを実装の一部としてその`SecurityStampValidator`です。 例は、次のようになります。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

これは、するワイヤード (有線) を cookie にサービスを登録中に、`ConfigureServices`メソッドの*Startup.cs*:

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

これは、するワイヤード (有線) の cookie 認証の構成時に、`Configure`メソッドの*Startup.cs*:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

その名前が更新された例を考えてみます&mdash;任意の方法でセキュリティが低下する可能性が決定します。 非破壊的ユーザー プリンシパルを更新する場合は、呼び出す`context.ReplacePrincipal()`設定と、`context.ShouldRenew`プロパティを`true`です。

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a>Cookie のオプションを制御します。

[CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions)クラスの作成中の cookie を微調整する各種の構成オプションが付属します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.x が cookie を構成するために使用される Api を統一します。 Api は、不使用とマークされている 1.x され、新しい`Cookie`型のプロパティ`CookieBuilder`が導入されました、`CookieAuthenticationOptions`クラスです。 2.x Api に移行することをお勧めします。

* `ClaimsIssuer`使用する発行者には、[発行者](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer)cookie 認証で作成された任意の信頼性情報のプロパティです。

* `CookieBuilder.Domain`cookie を提供するドメイン名です。 既定では、これは、要求に送信されました、ホスト名です。 ブラウザーでは、cookie、一致するホスト名にのみ機能します。 任意のホストに使用できる cookie は、ドメイン内にこれを調整することもできます。 たとえば、cookie のドメインに設定`.contoso.com`で使用できるように`contoso.com`、 `www.contoso.com`、 `staging.www.contoso.com`, などです。

* `CookieBuilder.HttpOnly`クッキーはサーバーにのみアクセスできるかどうかを示すフラグです。 既定値は`true`します。 この値を変更する可能性がありますアプリケーションを開く、cookie の盗難にクロスサイト スクリプティングのバグがある、アプリケーションをする必要があります。

* `CookieBuilder.Path`同じホスト名で実行されているアプリケーションを分離するために使用します。 実行されているアプリがあれば`/app1`だけそのアプリケーションに送信される発行された cookie を制限するし、設定する必要があります、`CookiePath`プロパティを`/app1`です。 これにより、cookie が要求に使用可能なのみ`/app1`または下にあるものです。

* `CookieBuilder.SameSite`ブラウザーが同じサイト内またはサイト間の要求にアタッチされている cookie を許可するかどうかを指定します。 既定値は`SameSiteMode.Lax`します。

* `CookieBuilder.SecurePolicy`作成された cookie を HTTPS、HTTP または HTTPS、または要求と同じプロトコルに限られますかどうかを示すフラグです。 既定値は`SameAsRequest`します。

* `ExpireTimeSpan``TimeSpan` cookie が期限切れの後にします。 Cookie の有効期限日を作成するには、現在の日付と時刻に追加されます。

* `SlidingExpiration`複数の半分の cookie の有効期限の日付をリセットするかどうかを示すフラグです、`ExpireTimeSpan`間隔が経過します。 新しい有効期限日が前方に移動する、現在の日付と`ExpireTimespan`です。 [絶対有効期限](xref:security/authentication/cookie#security-authentication-absolute-expiry)を使用して設定することができます、`AuthenticationProperties`クラスを呼び出すときに`SignInAsync`です。 絶対有効期限は、認証 cookie の有効期限の量を制限することによって、アプリケーションのセキュリティを強化できます。

使用する例`CookieAuthenticationOptions`で、`ConfigureServices`のメソッド*Startup.cs*に従います。

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Cookie.Name = "AuthCookie";
            options.Cookie.Domain = "contoso.com";
            options.Cookie.Path = "/";
            options.Cookie.HttpOnly = true;
            options.Cookie.SameSite = SameSiteMode.Lax;
            options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `ClaimsIssuer`使用する発行者には、[発行者](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer)ミドルウェアによって作成された任意の信頼性情報のプロパティです。

* `CookieDomain`cookie を提供するドメイン名です。 既定では、これは、要求に送信されました、ホスト名です。 ブラウザーでは、cookie、一致するホスト名にのみ機能します。 任意のホストに使用できる cookie は、ドメイン内にこれを調整することもできます。 たとえば、cookie のドメインに設定`.contoso.com`で使用できるように`contoso.com`、 `www.contoso.com`、 `staging.www.contoso.com`, などです。

* `CookieHttpOnly`クッキーはサーバーにのみアクセスできるかどうかを示すフラグです。 既定値は`true`します。 この値を変更する可能性がありますアプリケーションを開く、cookie の盗難にクロスサイト スクリプティングのバグがある、アプリケーションをする必要があります。

* `CookiePath`同じホスト名で実行されているアプリケーションを分離するために使用します。 実行されているアプリがあれば`/app1`だけそのアプリケーションに送信される発行された cookie を制限するし、設定する必要があります、`CookiePath`プロパティを`/app1`です。 これにより、cookie が要求に使用可能なのみ`/app1`または下にあるものです。

* `CookieSecure`作成された cookie を HTTPS、HTTP または HTTPS、または要求と同じプロトコルに限られますかどうかを示すフラグです。 既定値は`SameAsRequest`します。

* `ExpireTimeSpan``TimeSpan` cookie が期限切れの後にします。 Cookie の有効期限日を作成するには、現在の日付と時刻に追加されます。

* `SlidingExpiration`複数の半分の cookie の有効期限の日付をリセットするかどうかを示すフラグです、`ExpireTimeSpan`間隔が経過します。 新しい有効期限日が前方に移動する、現在の日付と`ExpireTimespan`です。 [絶対有効期限](xref:security/authentication/cookie#security-authentication-absolute-expiry)を使用して設定することができます、`AuthenticationProperties`クラスを呼び出すときに`SignInAsync`です。 絶対有効期限は、認証 cookie の有効期限の量を制限することによって、アプリケーションのセキュリティを強化できます。

使用する例`CookieAuthenticationOptions`で、`Configure`のメソッド*Startup.cs*に従います。

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    CookieName = "AuthCookie",
    CookieDomain = "contoso.com",
    CookiePath = "/",
    CookieHttpOnly = true,
    CookieSecure = CookieSecurePolicy.Always
});
```

---

## <a name="persistent-cookies-and-absolute-expiry-times"></a>永続的な cookie および絶対有効期限の時間

ブラウザー セッション間で永続化し、id とそれを転送する cookie を絶対有効期限をクッキーの有効期限をする可能性があります。 この永続化は、「次回」にあるチェック ボックス ログインまたは同様のメカニズムを使用して、明示的なユーザーの同意でのみ有効にする必要があります。 使用してこれらの操作を行うことができます、`AuthenticationProperties`上のパラメーター、`SignInAsync`メソッドを呼び出すときに[id でサインインし、cookie を作成する](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie)です。 例:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

`AuthenticationProperties`に上記のコード スニペットで使用されるクラスが存在する、`Microsoft.AspNetCore.Authentication`名前空間。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

`AuthenticationProperties`に上記のコード スニペットで使用されるクラスが存在する、`Microsoft.AspNetCore.Http.Authentication`名前空間。

---

上記のコード スニペットは、id およびブラウザー クロージャで回収されなかったを対応する cookie を作成します。 使用して以前構成スライディング有効期限設定[cookie オプション](xref:security/authentication/cookie#security-authentication-cookie-options)がまだ受け入れられます。 Cookie には、ブラウザーを閉じる中期限が切れると、ブラウザーをクリアが再起動したらします。

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

上記のコード スニペットは、id と対応する cookie が 20 分間は継続を作成します。 これは、àavhd スライディング有効期限を使用して以前構成[cookie オプション](xref:security/authentication/cookie#security-authentication-cookie-options)です。

`ExpiresUtc`と`IsPersistent`プロパティが相互に排他的です。
