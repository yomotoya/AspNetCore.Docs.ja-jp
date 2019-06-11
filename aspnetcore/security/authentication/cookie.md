---
title: ASP.NET Core Identity なしでの cookie 認証を使用します。
author: rick-anderson
description: ASP.NET Core Identity なしでの cookie 認証を使用しての説明
ms.author: riande
ms.date: 02/25/2019
uid: security/authentication/cookie
ms.openlocfilehash: f3e02b357a83cf5fc4b9fcdc79b2fbe80da98507
ms.sourcegitcommit: 9691b742134563b662948b0ed63f54ef7186801e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/10/2019
ms.locfileid: "66824761"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>ASP.NET Core Identity なしでの cookie 認証を使用します。

によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[Luke Latham](https://github.com/guardrex)

以前の認証に関するトピックで説明したように[ASP.NET Core Identity](xref:security/authentication/identity)の作成とログインの管理、フル機能の完全な認証プロバイダーは、します。 ただし、cookie ベースの認証時に、独自のカスタム認証ロジックを使用する場合があります。 ASP.NET Core Identity なしのスタンドアロンの認証プロバイダーとしては、cookie ベースの認証を使用できます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

サンプル アプリでは、デモンストレーションのための Maria Rodriguez、架空のユーザーのユーザー アカウント、アプリにハードコードしています。 電子メールのユーザー名を使用して、"maria.rodriguez@contoso.com"と、ユーザーがサインインするすべてのパスワード。 における、ユーザーの認証、`AuthenticateUser`メソッドで、 *Pages/Account/Login.cshtml.cs*ファイル。 実際の例では、ユーザーは、データベースに対して認証は。

ASP.NET Core から移行する cookie ベースの認証の詳細について 1.x から 2.0 への参照[認証の移行と ASP.NET Core 2.0 のトピック (Cookie ベースの認証) を Id](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication)します。

ASP.NET Core Identity を使用する、次を参照してください。、 [Id の概要](xref:security/authentication/identity)トピック。

## <a name="configuration"></a>構成

::: moniker range=">= aspnetcore-2.0"

アプリを使用しない場合、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)、プロジェクト ファイルでパッケージの参照を作成、 [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)パッケージ (バージョン 2.1.0 または後で)。

`ConfigureServices`メソッドを使用して、認証ミドルウェア サービスを作成、`AddAuthentication`と`AddCookie`メソッド。

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

`AuthenticationScheme` 渡される`AddAuthentication`アプリの既定の認証スキームを設定します。 `AuthenticationScheme` cookie 認証の複数のインスタンスがあるし、する場合に便利です[特定のスキームで承認](xref:security/authorization/limitingidentitybyscheme)します。 設定、`AuthenticationScheme`に`CookieAuthenticationDefaults.AuthenticationScheme`スキームの「クッキー」の値を指定します。 スキームを識別する任意の文字列値を指定することができます。

アプリの認証スキームは、アプリの cookie 認証スキームと異なります。 Cookie の認証スキームがときに指定されていません<xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>を使用して[CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) (「クッキー」)。

認証 cookie の<xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential>プロパティに設定されて`true`既定。 認証 cookie は、サイト訪問者がデータの収集に同意していない場合に許可されます。 詳細については、「 <xref:security/gdpr#essential-cookies> 」を参照してください。

`Configure`メソッドを使用して、`UseAuthentication`を設定する、認証ミドルウェアを呼び出すメソッドを`HttpContext.User`プロパティ。 呼び出す、`UseAuthentication`メソッドを呼び出す前に`UseMvcWithDefaultRoute`または`UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

**AddCookie オプション**

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0)認証プロバイダーのオプションを構成するクラスを使用します。

| オプション | 説明 |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | 302 検出 (URL リダイレクト) を指定するパスを提供してトリガーされたときに`HttpContext.ForbidAsync`します。 既定値は `/Account/AccessDenied` です。 |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | 使用する発行者、[発行者](/dotnet/api/system.security.claims.claim.issuer)cookie 認証サービスによって作成されたすべての要求のプロパティ。 |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | ドメイン名は、cookie が処理される場所です。 既定では、要求のホスト名です。 ブラウザーはのみ、一致するホスト名に、要求で cookie を送信します。 このドメインの任意のホストに使用できる cookie を調整することがあります。 たとえば、クッキーのドメインに設定`.contoso.com`で使用できるように`contoso.com`、 `www.contoso.com`、および`staging.www.contoso.com`します。 |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | クッキーをサーバーにのみアクセスできる必要があるかどうかを示すフラグです。 この値を変更する`false`クッキーにアクセスするクライアント側スクリプトを許可し、アプリが必要 cookie の盗難に、アプリを開くことがあります、[クロス サイト スクリプティング (XSS)](xref:security/cross-site-scripting)脆弱性。 既定値は `true` です。 |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | クッキーの名前を設定します。 |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | 同じホスト名で実行されているアプリを分離するために使用します。 実行中のアプリがあれば`/app1`cookie をそのアプリに制限を設定して、`CookiePath`プロパティを`/app1`します。 これにより、クッキーが要求に使用可能なだけ`/app1`とその下にあるすべてのアプリ。 |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | ブラウザーが同じサイトの要求のみに接続する cookie を許可するかどうかを指定 (`SameSiteMode.Strict`) または安全な HTTP メソッドと同じサイトの要求を使用してサイト間の要求 (`SameSiteMode.Lax`)。 設定すると`SameSiteMode.None`cookie のヘッダーの値が設定されていません。 なお[Cookie ポリシー ミドルウェア](#cookie-policy-middleware)指定した値を上書きする可能性があります。 既定値は、OAuth 認証をサポートする`SameSiteMode.Lax`します。 詳細については、次を参照してください。 [SameSite cookie のポリシーにより分割 OAuth 認証](https://github.com/aspnet/Security/issues/1231)します。 |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | 作成された cookie を HTTPS に制限するかどうかを示すフラグ (`CookieSecurePolicy.Always`)、HTTP または HTTPS (`CookieSecurePolicy.None`)、または、要求と同じプロトコル (`CookieSecurePolicy.SameAsRequest`)。 既定値は `CookieSecurePolicy.SameAsRequest` です。 |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | セット、`DataProtectionProvider`の既定の作成に使用される`TicketDataFormat`します。 場合、`TicketDataFormat`プロパティが設定されて、`DataProtectionProvider`オプションは使用されません。 指定されていない場合は、アプリの既定のデータ保護プロバイダーが使用されます。 |
| [イベント](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | ハンドラーは、特定の処理ポイントでアプリの制御を提供するプロバイダーのメソッドを呼び出します。 場合`Events`メソッドが呼び出されたときに何もしない既定のインスタンスが指定されて、指定されていません。 |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | サービスの種類として取得するために使用、`Events`インスタンス プロパティの代わりにします。 |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | `TimeSpan` Cookie 内に格納された認証チケットが期限切れ後。 `ExpireTimeSpan` チケットの有効期限を作成するには、現在の時刻に追加されます。 `ExpiredTimeSpan`値が常に検証サーバーで暗号化された認証チケットに移動します。 少し可能性があります、 [Set-cookie](https://tools.ietf.org/html/rfc6265#section-4.1)ヘッダーが場合にのみ、`IsPersistent`設定されます。 設定する`IsPersistent`に`true`、構成、 [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties)に渡される`SignInAsync`します。 既定値の`ExpireTimeSpan`は 14 日間です。 |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | 302 検出 (URL リダイレクト) を指定するパスを提供してトリガーされたときに`HttpContext.ChallengeAsync`します。 401 を生成した現在の URL を追加、`LoginPath`によってという名前のクエリ文字列パラメーターとして、`ReturnUrlParameter`します。 要求を 1 回、`LoginPath`新しいサインイン id、付与、`ReturnUrlParameter`値を使用して、元の unauthorized ステータス コードの原因となった URL にブラウザーをリダイレクトします。 既定値は `/Account/Login` です。 |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | 場合、 `LogoutPath` 、そのパスに対する要求をリダイレクトし、ハンドラーに提供の値に基づいて、`ReturnUrlParameter`します。 既定値は `/Account/Logout` です。 |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | 302 Found (URL リダイレクト) 応答のハンドラーによって追加されるクエリ文字列パラメーターの名前を指定します。 `ReturnUrlParameter` 要求を受信したときに使用されます、`LoginPath`または`LogoutPath`ログインまたはログアウト アクションが実行された後、元の URL にブラウザーに戻ります。 既定値は `ReturnUrl` です。 |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | 要求間で id を格納するために使用するオプションのコンテナー。 使用すると、セッション識別子のみがクライアントに送信されます。 `SessionStore` 大規模な id を持つ潜在的な問題を軽減するために使用できます。 |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | 更新された期限で新しい cookie を動的に発行するかどうかを示すフラグです。 これは、現在 cookie の有効期間が 50% 以上の有効期限が切れたすべての要求に対して発生します。 新しい有効期限は順方向に移動は、現在の日付を指定するだけでなく、`ExpireTimespan`します。 [絶対クッキーの有効期限](xref:security/authentication/cookie#absolute-cookie-expiration)を使用して設定することができます、`AuthenticationProperties`クラスを呼び出すときに`SignInAsync`します。 絶対有効期限は、認証 cookie が有効な時間を制限することで、アプリのセキュリティを強化できます。 既定値は `true` です。 |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | `TicketDataFormat`を保護し、id と cookie の値に格納されているその他のプロパティの保護を解除するために使用します。 指定しない場合、`TicketDataFormat`を使用して作成されて、 [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0)します。 |
| [検証](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | オプションが有効であるかをチェックするメソッド。 |

設定`CookieAuthenticationOptions`での認証サービスの構成で、`ConfigureServices`メソッド。

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

ASP.NET Core 1.x は cookie[ミドルウェア](xref:fundamentals/middleware/index)ユーザー プリンシパルに暗号化された cookie をシリアル化します。 プリンシパルが再作成されに割り当てられているし、後続の要求で cookie が有効な`HttpContext.User`プロパティ。

インストール、 [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)プロジェクトに NuGet パッケージ。 このパッケージには、cookie ミドルウェアが含まれています。

使用して、`UseCookieAuthentication`メソッドで、`Configure`メソッドで、 *Startup.cs*する前にファイル`UseMvc`または`UseMvcWithDefaultRoute`:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

**CookieAuthenticationOptions オプション**

[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1)認証プロバイダーのオプションを構成するクラスを使用します。

| オプション | 説明 |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | 認証スキームを設定します。 `AuthenticationScheme` 認証の複数のインスタンスがあるし、する場合に便利です[特定のスキームで承認](xref:security/authorization/limitingidentitybyscheme)します。 設定、`AuthenticationScheme`に`CookieAuthenticationDefaults.AuthenticationScheme`スキームの「クッキー」の値を指定します。 スキームを識別する任意の文字列値を指定することができます。 |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Cookie 認証が要求ごとに実行、検証し、作成された任意のシリアル化されたプリンシパルを再構築することを示す値を設定します。 |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | True の場合、認証ミドルウェアは、自動の課題を処理します。 かどうかは false の場合、認証ミドルウェアが変更されるだけ応答によって明示的に示されたとき、`AuthenticationScheme`します。 |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | 使用する発行者、[発行者](/dotnet/api/system.security.claims.claim.issuer)cookie 認証ミドルウェアによって作成されたすべての要求のプロパティ。 |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | ドメイン名は、cookie が処理される場所です。 既定では、要求のホスト名です。 ブラウザーでは、一致するホスト名のクッキーのみ機能します。 このドメインの任意のホストに使用できる cookie を調整することがあります。 たとえば、クッキーのドメインに設定`.contoso.com`で使用できるように`contoso.com`、 `www.contoso.com`、および`staging.www.contoso.com`します。 |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | クッキーをサーバーにのみアクセスできる必要があるかどうかを示すフラグです。 この値を変更する`false`クッキーにアクセスするクライアント側スクリプトを許可し、アプリが必要 cookie の盗難に、アプリを開くことがあります、[クロス サイト スクリプティング (XSS)](xref:security/cross-site-scripting)脆弱性。 既定値は `true` です。 |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | 同じホスト名で実行されているアプリを分離するために使用します。 実行中のアプリがあれば`/app1`cookie をそのアプリに制限を設定して、`CookiePath`プロパティを`/app1`します。 これにより、クッキーが要求に使用可能なだけ`/app1`とその下にあるすべてのアプリ。 |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | 作成された cookie を HTTPS に制限するかどうかを示すフラグ (`CookieSecurePolicy.Always`)、HTTP または HTTPS (`CookieSecurePolicy.None`)、または、要求と同じプロトコル (`CookieSecurePolicy.SameAsRequest`)。 既定値は `CookieSecurePolicy.SameAsRequest` です。 |
| [説明](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | アプリに利用可能になっている認証の種類に関する追加情報。 |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | `TimeSpan`認証チケットが期限切れ後。 チケットの有効期限を作成するには、現在の時刻に追加されます。 使用する`ExpireTimeSpan`を設定する必要があります`IsPersistent`に`true`で、`AuthenticationProperties`に渡される`SignInAsync`します。 既定値は 14 日間です。 |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | 複数の半分のクッキーの有効期限の日時をリセットするかどうかを示すフラグ、`ExpireTimeSpan`間隔が渡されます。 新しい有効期限は順方向に移動は、現在の日付を指定するだけでなく、`ExpireTimespan`します。 [絶対クッキーの有効期限](xref:security/authentication/cookie#absolute-cookie-expiration)を使用して設定することができます、`AuthenticationProperties`クラスを呼び出すときに`SignInAsync`します。 絶対有効期限は、認証 cookie が有効な時間を制限することで、アプリのセキュリティを強化できます。 既定値は `true` です。 |

設定`CookieAuthenticationOptions`で Cookie 認証ミドルウェアの`Configure`メソッド。

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a>Cookie のポリシーのミドルウェア

[Cookie のポリシーのミドルウェア](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware)アプリでの cookie ポリシー機能を使用します。 機密性の高い; 順序は、アプリの処理パイプラインにミドルウェアを追加します。その後に、パイプラインに登録されたコンポーネントのみに影響します。

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) Cookie のポリシーのミドルウェアに提供される cookie を追加または削除されたときに、cookie 処理ハンドラーにクッキーの処理とフックのグローバルの特性を制御することです。

| プロパティ | 説明 |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Cookie は HttpOnly である必要があります、かどうかをフラグを示すです、クッキーをサーバーにのみアクセスできる必要があるかどうかに影響します。 既定値は `HttpOnlyPolicy.None` です。 |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Cookie の同じサイト属性 (下記参照) に影響します。 既定値は `SameSiteMode.Lax` です。 このオプションでは、ASP.NET Core 2.0 以降を使用します。 |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Cookie を追加するときに呼び出されます。 |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Cookie が削除されたときに呼び出されます。 |
| [セキュリティで保護します。](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | Cookie はセキュリティで保護する必要があるかどうかに影響します。 既定値は `CookieSecurePolicy.None` です。 |

**MinimumSameSitePolicy** (ASP.NET Core 2.0 以降のみ)

既定の`MinimumSameSitePolicy`値は`SameSiteMode.Lax`OAuth2 認証を許可するようにします。 厳密にポリシーを適用する同じサイトの`SameSiteMode.Strict`、設定、`MinimumSameSitePolicy`します。 ただし、この設定は、OAuth2 やその他のクロス オリジンの認証スキームを区切り、クロス オリジン要求の処理に依存しないようにするアプリの他の種類の cookie のセキュリティのレベルを高めます。

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Cookie のポリシーのミドルウェアの設定`MinimumSameSitePolicy`の設定に影響を与える`Cookie.SameSite`で`CookieAuthenticationOptions`次の表に従って設定します。

| MinimumSameSitePolicy | Cookie.SameSite | 最終的な Cookie.SameSite 設定 |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>認証 cookie を作成します。

ユーザー情報を保持するクッキーを作成するには、構築する必要があります、 [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)します。 ユーザー情報がシリアル化され、cookie に格納されています。 

::: moniker range=">= aspnetcore-2.0"

作成、 [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity)必須[要求](/dotnet/api/system.security.claims.claim)s と呼び出し[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0)ユーザーがサインインします。

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

呼び出す[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1)ユーザーがサインインします。

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

`SignInAsync` 暗号化された cookie を作成し、それを現在の応答に追加します。 指定しない場合、`AuthenticationScheme`既定のスキームを使用します。

実際には、使用される暗号化とは、ASP.NET Core の[データ保護](xref:security/data-protection/using-data-protection)システム。 複数のマシン、アプリでの負荷分散または web ファームを使用してアプリをホストしているかどうかは、する必要があります[データ保護を構成する](xref:security/data-protection/configuration/overview)アプリ id と同じキー リングを使用します。

## <a name="sign-out"></a>サインアウト

::: moniker range=">= aspnetcore-2.0"

現在のユーザーをサインアウトし、その cookie を削除、 [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

現在のユーザーをサインアウトし、その cookie を削除、 [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

使用していない場合`CookieAuthenticationDefaults.AuthenticationScheme`(または「クッキー」)、スキーム (たとえば、"ContosoCookie") として、認証プロバイダーを構成するときに使用するスキームを指定します。 それ以外の場合、既定のスキームが使用されます。

## <a name="react-to-back-end-changes"></a>バックエンドの変更に対応します。

Cookie が作成されると、id の 1 つのソースになります。 バックエンド システムでユーザーを無効にした場合でも、cookie 認証システムは、この知識を持たないし、ユーザーはまだその cookie が有効な限り、ログインしています。

[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal)イベントでは、ASP.NET Core 2.x または[ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) 1.x をインターセプトし、cookie id の検証のオーバーライドに使用できる ASP.NET Core でのメソッド。 このアプローチでは、失効したユーザーがアプリにアクセスするリスクを軽減します。

Cookie を検証する方法の 1 つは、ユーザー データベースが変更されたときの追跡に基づいています。 ユーザーのクッキーが発行されたので、データベースが変更されていない場合、その cookie が有効である場合、ユーザーを再認証する必要はありません。 このシナリオに実装されていると、データベースを実装する`IUserRepository`この例では、格納、`LastChanged`値。 すべてのユーザーが、データベースで更新されたときに、`LastChanged`値が現在の時刻に設定します。

データベースの変更に基づいている場合は、cookie を無効にするには、`LastChanged`値には、cookie を作成、`LastChanged`現在を含む要求`LastChanged`データベースから値。

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker range=">= aspnetcore-2.0"

オーバーライドを実装するために、`ValidatePrincipal`から派生したクラスでは、次のシグネチャを持つメソッドを書き込み、イベント[CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

例では、次のようになります。

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

クッキーにサービスを登録中に、イベント インスタンスを登録、`ConfigureServices`メソッド。 スコープを持つサービスの登録を提供、`CustomCookieAuthenticationEvents`クラス。

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

オーバーライドを実装するために、`ValidateAsync`イベントでは、次のシグネチャを持つメソッドを作成します。

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET Core Identity では、このチェックを実装の一部としてその[SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync)します。 例では、次のようになります。

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Cookie 認証の構成時にイベントを登録、`Configure`メソッド。

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

::: moniker-end

ユーザーの名前が更新される場合を考えてみましょう&mdash;判断を任意の方法でセキュリティには影響しません。 非破壊的ユーザー プリンシパルを更新する場合は、呼び出す`context.ReplacePrincipal`設定と、`context.ShouldRenew`プロパティを`true`します。

> [!WARNING]
> ここで説明したアプローチは、要求ごとにトリガーされます。 これにより、アプリのパフォーマンスが大幅に低下します。

## <a name="persistent-cookies"></a>永続的な cookie

Cookie がブラウザー セッション間で永続化することができます。 この永続化は、明示的なユーザーによる同意を「記憶」チェック ボックスが付いたログインまたは同様のメカニズムでのみ有効にする必要があります。 

次のコード スニペットでは、id およびブラウザー クロージャでは存続する対応する cookie を作成します。 以前に構成された、スライド式有効期限の設定が受け入れられます。 ブラウザーに cookie の期限切れのブラウザーが閉じられたときに、再起動の後、cookie をクリアします。

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0)でクラスが存在する、`Microsoft.AspNetCore.Authentication`名前空間。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1)でクラスが存在する、`Microsoft.AspNetCore.Http.Authentication`名前空間。

::: moniker-end

## <a name="absolute-cookie-expiration"></a>絶対クッキーの有効期限

絶対有効期限を設定する`ExpiresUtc`します。 永続的なクッキーを作成する必要がありますも設定する`IsPersistent`; それ以外の場合、cookie はセッション ベースの有効期間で作成され、期限前にいずれかまたはチケットの認証後を保持します。 ときに`ExpiresUtc`が設定されて`SignInAsync`の値よりも優先、`ExpireTimeSpan`オプションの`CookieAuthenticationOptions`場合は、設定します。

次のコード スニペットは、id と対応するクッキーを 20 分間継続を作成します。 これには、以前に構成された、スライド式有効期限の設定が無視されます。

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

::: moniker-end

## <a name="additional-resources"></a>その他の技術情報

* [Auth 2.0 の変更/移行のお知らせ](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [ポリシー ベースのロールのチェック](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
