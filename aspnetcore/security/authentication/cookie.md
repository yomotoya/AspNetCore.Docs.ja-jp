---
title: ASP.NET Core Id なしの cookie 認証を使用します。
author: rick-anderson
description: ASP.NET Core Id なしの cookie 認証を使用しての説明
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: b251aa3ff0b4d0c08f9885cd73a111b7c2008766
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>ASP.NET Core Id なしの cookie 認証を使用します。

によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[Luke Latham](https://github.com/guardrex)

以前の認証のトピックで説明したように[ASP.NET Core Id](xref:security/authentication/identity) 、フル機能の完全な認証プロバイダーを作成してログインを維持するため、します。 ただし、cookie ベースの認証時に、独自のカスタム認証ロジックを使用することがあります。 ASP.NET Core Id せず、スタンドアロンの認証プロバイダーとしては、クッキー ベースの認証を使用できます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

ASP.NET Core から移行する cookie ベースの認証の詳細について 1.x 2.0 を参照してください[移行認証および ASP.NET Core 2.0 トピック (Cookie ベースの認証) を Id](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication)です。

## <a name="configuration"></a>構成

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
使用しない場合、 [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)、バージョン 2.0 以降のインストール、 [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet パッケージです。

`ConfigureServices`メソッドを使用して、認証ミドルウェア サービスを作成、`AddAuthentication`と`AddCookie`メソッド。

[!code-csharp[](cookie/sample/Startup.cs?name=snippet1)]

`AuthenticationScheme` 渡される`AddAuthentication`アプリの既定の認証スキームを設定します。 `AuthenticationScheme` cookie 認証の複数のインスタンスがあるし、する場合に便利です[、特定のスキームの承認](xref:security/authorization/limitingidentitybyscheme)です。 設定、`AuthenticationScheme`に`CookieAuthenticationDefaults.AuthenticationScheme`スキームの"Cookie"の値を提供します。 スキームを区別する任意の文字列値を指定することができます。

`Configure`メソッドを使用して、`UseAuthentication`設定認証ミドルウェアを呼び出すメソッドを`HttpContext.User`プロパティです。 呼び出す、`UseAuthentication`メソッドを呼び出す前に`UseMvcWithDefaultRoute`または`UseMvc`:

[!code-csharp[](cookie/sample/Startup.cs?name=snippet2)]

**AddCookie オプション**

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0)認証プロバイダー オプションを構成するクラスを使用します。

| オプション | 説明 |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | 302 検出 (URL リダイレクト) を指定するパスを提供によってトリガーされたときに`HttpContext.ForbidAsync`です。 既定値は `/Account/AccessDenied` です。 |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | 使用する発行者、[発行者](/dotnet/api/system.security.claims.claim.issuer)cookie 認証サービスによって作成された任意の信頼性情報のプロパティです。 |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | Cookie が提供されたドメイン名です。 既定では、これは、要求のホスト名です。 ブラウザーはのみ、一致するホスト名に、要求で cookie を送信します。 任意のホストに使用できる cookie は、ドメイン内にこれを調整することもできます。 たとえば、cookie のドメインに設定`.contoso.com`で使用できるように`contoso.com`、 `www.contoso.com`、および`staging.www.contoso.com`です。 |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | 取得または cookie の有効期間を設定します。 現時点では、このキャッシュなしでのオプションし、ASP.NET Core 2.1 以降で廃止になります。 使用して、 `ExpireTimeSpan` cookie の有効期限を設定するオプションです。 詳細については、次を参照してください。 [CookieAuthenticationOptions.Cookie.Expiration の動作が明確](https://github.com/aspnet/Security/issues/1293)です。 |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | クッキーはサーバーにのみアクセスできるかどうかを示すフラグ。 この値を変更する`false`cookie にアクセスするクライアント側のスクリプトを許可し、アプリが必要 cookie の盗難にアプリを開きます可能性があります、[クロス サイト スクリプト (XSS)](xref:security/cross-site-scripting)脆弱性。 既定値は `true` です。 |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | Cookie の名前を設定します。 |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | 同じホスト名で実行されているアプリを分離するために使用します。 実行されているアプリがあれば`/app1`cookie をそのアプリに制限を設定して、`CookiePath`プロパティを`/app1`です。 これにより、cookie が要求に使用可能なのみ`/app1`とその下のすべてのアプリです。 |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | ブラウザーが同じサイトの要求のみに接続する cookie を許可するかどうかを指定 (`SameSiteMode.Strict`) またはセーフである HTTP メソッドと同じサイトの要求を使用してクロスサイト リクエスト (`SameSiteMode.Lax`)。 設定すると`SameSiteMode.None`cookie のヘッダーの値が設定されていません。 なお[Cookie ポリシー ミドルウェア](#cookie-policy-middleware)指定した値を上書きする可能性があります。 既定値は、OAuth 認証をサポートする`SameSiteMode.Lax`です。 詳細については、次を参照してください。 [SameSite クッキーのポリシーのための別の OAuth 認証](https://github.com/aspnet/Security/issues/1231)です。 |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | 作成された cookie を HTTPS に限られますかどうかを示すフラグ (`CookieSecurePolicy.Always`)、HTTP または HTTPS (`CookieSecurePolicy.None`)、または要求と同じプロトコル (`CookieSecurePolicy.SameAsRequest`)。 既定値は `CookieSecurePolicy.SameAsRequest` です。 |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | セット、`DataProtectionProvider`の既定の作成に使用される`TicketDataFormat`です。 場合、`TicketDataFormat`プロパティが設定されて、`DataProtectionProvider`オプションは使用されません。 指定しないと、アプリの既定のデータ保護プロバイダーが使用されます。 |
| [イベント](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | ハンドラーは、特定の処理時点で、アプリのコントロールを提供するプロバイダーのメソッドを呼び出します。 場合`Events`は提供された場合、既定のインスタンスが指定されたメソッドが呼び出されたときに何も実行しません。 |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | 取得するサービスの種類として使用される、`Events`プロパティではなくインスタンス。 |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | `TimeSpan` Cookie 内に格納する認証チケットが期限切れの後にします。 `ExpireTimeSpan` チケットの有効期限を作成するには、現在の時刻に追加されます。 `ExpiredTimeSpan`値は常に暗号化された認証チケットがサーバーで検証に入ります。 入りますが、 [Set-cookie](https://tools.ietf.org/html/rfc6265#section-4.1)場合にのみ、ヘッダー、`IsPersistent`が設定されています。 設定する`IsPersistent`に`true`、構成、 [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties)に渡される`SignInAsync`です。 既定値の`ExpireTimeSpan`は 14 日間です。 |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | 302 検出 (URL リダイレクト) を指定するパスを提供によってトリガーされたときに`HttpContext.ChallengeAsync`です。 401 を生成した現在の URL に追加、`LoginPath`によってという名前のクエリ文字列パラメーターとして、`ReturnUrlParameter`です。 要求を 1 回、`LoginPath`新しいサインイン ユーザーの許可、`ReturnUrlParameter`値を使用して、元の unauthorized ステータス コードの原因となった URL にブラウザーをリダイレクトします。 既定値は `/Account/Login` です。 |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | 場合、`LogoutPath`が、そのパスへの要求をリダイレクトし、ハンドラーに提供されるの値に基づいて、`ReturnUrlParameter`です。 既定値は `/Account/Logout` です。 |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | 302 Found (URL リダイレクト) 応答のハンドラーで追加されるクエリ文字列パラメーターの名前を決定します。 `ReturnUrlParameter` 要求を受信したときに使用、`LoginPath`または`LogoutPath`ログインまたはログアウト アクションが実行された後、元の URL をブラウザーに戻ります。 既定値は `ReturnUrl` です。 |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | 要求間で id を格納するために使用、省略可能なコンテナーです。 使用すると、セッション識別子のみがクライアントに送信されます。 `SessionStore` 大規模な id を持つ潜在的な問題を軽減するために使用できます。 |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | かどうかは、更新された期限で新しい cookie を動的に発行する必要がありますを示すフラグ。 ここで、現在 cookie の有効期間が 50% を超える有効期限が切れてすべての要求の可能性があります。 新しい有効期限が前方に移動する、現在の日付と`ExpireTimespan`です。 [絶対 cookie の有効期限](xref:security/authentication/cookie#absolute-cookie-expiration)を使用して設定することができます、`AuthenticationProperties`クラスを呼び出すときに`SignInAsync`です。 絶対有効期限は、認証 cookie が有効な時間を制限することによって、アプリのセキュリティを強化できます。 既定値は `true` です。 |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | `TicketDataFormat`を保護し、id および cookie の値に格納されているその他のプロパティの保護を解除するために使用します。 指定されていない場合、`TicketDataFormat`を使用して作成されて、 [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0)です。 |
| [検証](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | オプションが有効なことを確認するメソッド。 |

設定`CookieAuthenticationOptions`での認証サービスの構成で、`ConfigureServices`メソッド。

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
ASP.NET Core 1.x 使用 cookie[ミドルウェア](xref:fundamentals/middleware/index)暗号化された cookie にユーザー プリンシパルをシリアル化します。 以降の要求でクッキーが検証され、プリンシパルが再作成されに割り当てられている、`HttpContext.User`プロパティです。

インストール、 [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)プロジェクトでの NuGet パッケージです。 このパッケージには、cookie ミドルウェアが含まれています。

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

[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1)認証プロバイダー オプションを構成するクラスを使用します。

| オプション | 説明 |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | 認証方式を設定します。 `AuthenticationScheme` 認証の複数のインスタンスがあるし、する場合に便利です[、特定のスキームの承認](xref:security/authorization/limitingidentitybyscheme)です。 設定、`AuthenticationScheme`に`CookieAuthenticationDefaults.AuthenticationScheme`スキームの"Cookie"の値を提供します。 スキームを区別する任意の文字列値を指定することができます。 |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Cookie 認証の要求ごとに実行、および検証し、作成された任意のシリアル化されたプリンシパルを再構築しようとしています。 ことを示す値を設定します。 |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | True の場合、認証ミドルウェアは自動の課題を処理します。 かどうかは false の場合、認証ミドルウェアが変更されるだけ応答によって明示的に示されたとき、`AuthenticationScheme`です。 |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | 使用する発行者、[発行者](/dotnet/api/system.security.claims.claim.issuer)cookie 認証ミドルウェアによって作成された任意の信頼性情報のプロパティです。 |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | Cookie が提供されたドメイン名です。 既定では、これは、要求のホスト名です。 ブラウザーでは、cookie、一致するホスト名にのみ機能します。 任意のホストに使用できる cookie は、ドメイン内にこれを調整することもできます。 たとえば、cookie のドメインに設定`.contoso.com`で使用できるように`contoso.com`、 `www.contoso.com`、および`staging.www.contoso.com`です。 |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | クッキーはサーバーにのみアクセスできるかどうかを示すフラグ。 この値を変更する`false`cookie にアクセスするクライアント側のスクリプトを許可し、アプリが必要 cookie の盗難にアプリを開きます可能性があります、[クロス サイト スクリプト (XSS)](xref:security/cross-site-scripting)脆弱性。 既定値は `true` です。 |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | 同じホスト名で実行されているアプリを分離するために使用します。 実行されているアプリがあれば`/app1`cookie をそのアプリに制限を設定して、`CookiePath`プロパティを`/app1`です。 これにより、cookie が要求に使用可能なのみ`/app1`とその下のすべてのアプリです。 |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | 作成された cookie を HTTPS に限られますかどうかを示すフラグ (`CookieSecurePolicy.Always`)、HTTP または HTTPS (`CookieSecurePolicy.None`)、または要求と同じプロトコル (`CookieSecurePolicy.SameAsRequest`)。 既定値は `CookieSecurePolicy.SameAsRequest` です。 |
| [説明](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | アプリに使用できる認証の種類に関する追加情報。 |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | `TimeSpan`認証チケットが期限切れの後にします。 チケットの有効期限を作成するのには、現在の時刻に追加されます。 使用する`ExpireTimeSpan`、設定する必要があります`IsPersistent`に`true`で、`AuthenticationProperties`に渡される`SignInAsync`です。 既定値は、14 日間です。 |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | 複数の半分の cookie の有効期限の日付をリセットするかどうかを示すフラグ、`ExpireTimeSpan`間隔が経過します。 新しい exipiration 時刻が前方に移動する、現在の日付と`ExpireTimespan`です。 [絶対 cookie の有効期限](xref:security/authentication/cookie#absolute-cookie-expiration)を使用して設定することができます、`AuthenticationProperties`クラスを呼び出すときに`SignInAsync`です。 絶対有効期限は、認証 cookie が有効な時間を制限することによって、アプリのセキュリティを強化できます。 既定値は `true` です。 |

設定`CookieAuthenticationOptions`で Cookie 認証ミドルウェアの`Configure`メソッド。

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

* * *
## <a name="cookie-policy-middleware"></a>Cookie ポリシー ミドルウェア

[Cookie ポリシー ミドルウェア](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware)アプリ内で cookie ポリシーの機能を有効にします。 機密性の高い; 順序は、アプリの処理パイプラインにミドルウェアを追加します。パイプラインの後に登録されているコンポーネントのみに影響します。

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) Cookie ポリシー ミドルウェアに渡された cookie を追加または削除されたときに、cookie 処理ハンドラーに cookie の処理とフックのグローバルの特性を制御することができます。

| プロパティ | 説明 |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Cookie が HttpOnly であるかどうかをフラグを示すですクッキーはサーバーにのみアクセスできるかどうかに影響します。 既定値は `HttpOnlyPolicy.None` です。 |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Cookie の同じサイト属性 (下記参照) に影響します。 既定値は `SameSiteMode.Lax` です。 このオプションは、ASP.NET Core 2.0 以降を使用します。 |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Cookie を追加するときに呼び出されます。 |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Cookie が削除されたときに呼び出されます。 |
| [セキュリティで保護されました。](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | クッキーはセキュリティで保護する必要があるかどうかに影響します。 既定値は `CookieSecurePolicy.None` です。 |

**MinimumSameSitePolicy** (ASP.NET Core 2.0 以降のみ)

既定値`MinimumSameSitePolicy`値は`SameSiteMode.Lax`OAuth2 認証を許可するようにします。 厳密にポリシーを適用する、同じサイトの`SameSiteMode.Strict`、設定、`MinimumSameSitePolicy`です。 この設定は、OAuth2 と他のクロス オリジン認証スキームを破損が他の種類のクロス オリジン要求の処理に依存しないようにするアプリの cookie のセキュリティのレベルを高めます。

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

## <a name="creating-an-authentication-cookie"></a>認証 cookie を作成します。

ユーザー情報を保持する cookie を作成、構築する必要があります、 [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)です。 ユーザー情報がシリアル化され、cookie に格納されています。 

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
作成、 [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity)で必要な[クレーム](/dotnet/api/system.security.claims.claim)s と呼び出し[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0)ユーザーにサインインします。

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet1)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
呼び出す[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1)ユーザーにサインインします。

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

* * *
`SignInAsync` 暗号化された cookie を作成し、現在の応答に追加します。 指定しない場合は、`AuthenticationScheme`既定のスキームを使用します。

背後で使用される暗号化は ASP.NET Core[データ保護](xref:security/data-protection/using-data-protection#security-data-protection-getting-started)システムです。 複数のコンピューター、アプリでは、間における負荷分散または web ファーム上のアプリをホストしているかどうかは、する必要があります[データ保護を構成する](xref:security/data-protection/configuration/overview)同じキーのリングとアプリ id を使用します。

## <a name="signing-out"></a>サインアウト

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
現在のユーザーをサインアウトし、その cookie を削除、 [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet2)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
現在のユーザーをサインアウトし、その cookie を削除、 [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

* * *
使用しない場合`CookieAuthenticationDefaults.AuthenticationScheme`(または"Cookie")、スキーム (たとえば、"ContosoCookie") として認証プロバイダーを構成するときに使用するスキームを指定します。 それ以外の場合、既定のスキームが使用されます。

## <a name="reacting-to-back-end-changes"></a>バックエンドの変更に反応します。

Cookie が作成されると、id の 1 つのソースになります。 バックエンド システムでユーザーを無効にする場合でも cookie 認証システムは、この情報を持たないし、状態を維持でログに記録されたその cookie が有効な限り、します。

[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) ASP.NET Core でのイベント 2.x または[ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) 1.x をインターセプトし、cookie id の検証のオーバーライドに使用できる ASP.NET Core のメソッドです。 この方法は、失効したユーザーがアプリにアクセスするリスクを軽減します。

Cookie 検証方法の 1 つは、ユーザー データベースが変更されたときの追跡に基づいています。 ユーザーの cookie が発行されてから、データベースが変更されていない場合、cookie が有効である場合、ユーザーを再認証する必要はありません。 このシナリオで実装されると、データベースを実装する`IUserRepository`この例では、格納、`LastChanged`値。 すべてのユーザーが、データベースで更新されたときに、`LastChanged`値は、現在の時刻に設定します。

に基づいて、データベースの変更時に、cookie を無効にするために、`LastChanged`値、cookie を作成、`LastChanged`を含む、現在の要求`LastChanged`データベースから値。

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

オーバーライドを実装する、`ValidatePrincipal`イベントから派生したクラスでは、次のシグネチャを持つメソッドを作成します[CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

例は、次のようになります。

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

Cookie にサービスを登録中に、イベント インスタンスを登録する、`ConfigureServices`メソッドです。 スコープを持つサービスの登録を提供、`CustomCookieAuthenticationEvents`クラス。

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

オーバーライドを実装する、`ValidateAsync`イベント、次のシグネチャを持つメソッドを作成します。

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET Core Id では、このチェックを実装の一部としてその[SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync)です。 例は、次のようになります。

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

---

ユーザーの名前を更新する場合を検討&mdash;任意の方法でセキュリティに影響しないする意思決定します。 非破壊的ユーザー プリンシパルを更新する場合は、呼び出す`context.ReplacePrincipal`設定と、`context.ShouldRenew`プロパティを`true`です。

> [!WARNING]
> ここで説明されているアプローチは、要求ごとにトリガーされます。 これにより、アプリのパフォーマンスを大幅に低下。

## <a name="persistent-cookies"></a>永続的な cookie

Cookie をブラウザー セッション間で保持することができます。 この永続化は、「次回」にあるチェック ボックス ログインまたは同様のメカニズムを使用してユーザーの明示的な同意でのみ有効にする必要があります。 

次のコード スニペットは、id およびブラウザー クロージャで回収されなかったを対応する cookie を作成します。 以前に構成された、スライド式有効期限の設定が無視されます。 Cookie には、ブラウザーを閉じている間に期限が切れるが再起動したら、ブラウザーの cookie を消去します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0)に存在するクラス、`Microsoft.AspNetCore.Authentication`名前空間。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1)に存在するクラス、`Microsoft.AspNetCore.Http.Authentication`名前空間。

---

## <a name="absolute-cookie-expiration"></a>絶対 cookie の有効期限

絶対有効期限を設定することができます`ExpiresUtc`です。 設定する必要もあります`IsPersistent`、それ以外の`ExpiresUtc`は無視されますと、単一セッションの cookie を作成します。 ときに`ExpiresUtc`に設定されている`SignInAsync`の値をオーバーライドし、`ExpireTimeSpan`オプション`CookieAuthenticationOptions`場合は、設定します。

次のコード スニペットは、id と 20 分間存続する対応する cookie を作成します。 これには、以前に構成された、スライド式有効期限の設定は無視されます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

---

## <a name="see-also"></a>関連項目

* [Auth 2.0 変更/移行アナウンス](https://github.com/aspnet/Announcements/issues/262)
* [スキームによる ID の制限](xref:security/authorization/limitingidentitybyscheme)
* [クレーム ベースの承認](xref:security/authorization/claims)
* [ポリシー ベースのロールのチェック](xref:security/authorization/roles#policy-based-role-checks)
