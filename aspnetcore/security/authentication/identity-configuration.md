---
title: "ASP.NET Core Id を構成します。"
author: AdrienTorris
description: "ASP.NET Core Id の既定値を理解し、カスタム値を使用する Id プロパティを構成する方法について説明します。"
manager: wpickett
ms.author: scaddie
ms.date: 02/21/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: 6aeb85063b4b6f97822062b523a0c1f7ee6b595c
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="configure-identity"></a>Id を構成します。

ASP.NET Core の Id は、パスワード ポリシー、ロックアウト時間、および cookie の設定などの設定を既定の構成を使用します。 アプリのではこれらの設定をオーバーライドできます`Startup`クラスです。

## <a name="identity-options"></a>Id オプション

[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)クラス Id システムを構成するために使用するオプションを表します。

### <a name="claims-identity"></a>要求の Id

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity)を指定します、 [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions)の表に示すプロパティを持つ。

| プロパティ | 説明 | 既定値 |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | 取得または使用ロール クレームのクレームの種類を設定します。 | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | 取得またはセキュリティ スタンプの要求に対して使用される要求の種類を設定します。 | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | 取得または使用ユーザー識別子要求の要求の種類を設定します。 | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | 取得またはユーザー名要求のために使用するクレームの種類を設定します。 | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>ロックアウト

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout)を指定します、 [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions)の表に示すプロパティを持つ。

| プロパティ | 説明 | 既定値 |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | 新しいユーザーをロックアウトできるかどうかを判断します。 | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | 時間数、ユーザーがロックアウト ロックアウトが発生するとします。 | 5 分 |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | ロックアウトが有効になっている場合、ユーザーがロックされるまで、失敗したアクセス試行の数。 | 5 |

### <a name="password"></a>パスワード

既定では、Id は、パスワードに、大文字、小文字、数字、および英数字以外の文字を含めることが必要です。 パスワードは、少なくとも 6 文字以上にする必要があります。 [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)で変更できます。`Startup.ConfigureServices`です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 追加 2.0、 [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars)プロパティです。 それ以外の場合、オプションは、ASP.NET Core として同じ 1.x です。

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password)を指定します、 [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)の表に示すプロパティを持つ。

| プロパティ | 説明 | 既定値 |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | 0 ~ 9 では、パスワードに数字が必要です。 | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | パスワードの最小長。 | 6 |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | ASP.NET Core 2.0 またはそれ以降にのみ適用されます。<br><br> パスワードに個別の文字数が必要です。 | 1 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | パスワードに小文字が必要です。 | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | パスワードに英数字以外の文字が必要です。 | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | パスワードに大文字が必要です。 | `true` |

### <a name="sign-in"></a>サインイン

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin)を指定します、 [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions)の表に示すプロパティを持つ。

| プロパティ | 説明 | 既定値 |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | 確認の電子メールにサインインする必要があります。 | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | 確認の電話番号にサインインする必要があります。 | `false` |

### <a name="tokens"></a>トークン

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens)を指定します、 [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions)の表に示すプロパティを持つ。

| プロパティ | 説明 |
| -------- | ----------- |
| [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider) | 取得または設定、`AuthenticatorTokenProvider`認証によるサインインを 2 つの要素を検証するために使用します。 |
| [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider) | 取得または設定、`ChangeEmailTokenProvider`電子メール変更の確認メールで使用されるトークンを生成するために使用します。 |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) | 取得または設定、`ChangePhoneNumberTokenProvider`電話番号を変更するときに使用されるトークンを生成するために使用します。 |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) | 取得またはアカウントの確認メールで使用されるトークンの生成に使用されるトークン プロバイダーを設定します。 |
| [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider) | 取得または設定、 [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1)パスワード リセット電子メールで使用されるトークンを生成するために使用します。 |
| [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap) | 構築するために使用される、[ユーザー トークン プロバイダー](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor)プロバイダーの名前として使用されるキーを使用します。 |

### <a name="user"></a>ユーザー

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user)を指定します、 [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions)の表に示すプロパティを持つ。

| プロパティ | 説明 | 既定値 |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | ユーザー名に許容される文字。 | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | 各ユーザーを一意の電子メールを持っている必要があります。 | `false` |

## <a name="cookie-settings"></a>Cookie の設定

アプリの cookie を構成する`Startup.ConfigureServices`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions)は次のプロパティがあります。

| プロパティ | 説明 |
| -------- | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath) | 送信変更する必要があります、ハンドラーに通知*403 アクセス不可*にステータス コード、 *302 リダイレクト*指定されたパスにします。<br><br>既定値は `/Account/AccessDenied` です。 |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme) | ASP.NET Core にのみ適用されます 1.x です。<br><br> 特定の認証スキームの論理名。 |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate) | ASP.NET Core にのみ適用されます 1.x です。<br><br> True の場合、cookie 認証を要求ごとに実行しを検証し、作成された任意のシリアル化されたプリンシパルを再構築しようとします。 |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge) | ASP.NET Core にのみ適用されます 1.x です。<br><br> True の場合、認証ミドルウェアは自動の課題を処理します。 かどうかは false の場合、認証ミドルウェアが変更されるだけ応答によって明示的に示されたとき、`AuthenticationScheme`です。 |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer) | 取得または作成された任意のクレームを使用する発行者を設定 (から継承された[AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions))。 |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) | Cookie を関連付けるドメイン。 |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration) | 取得または cookie の有効期間を設定します。 |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) | Cookie がクライアント側スクリプトからアクセスできるかどうかを示します。<br><br>既定値は `true` です。 |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) | Cookie の名前。<br><br>既定値は `.AspNetCore.Cookies` です。 |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) | Cookie のパス。 |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) | `SameSite` Cookie の属性です。<br><br>既定値は[SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode)です。 |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy) | [CookieSecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy)構成します。<br><br>既定値は[CookieSecurePolicy.SameAsRequest](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy)です。 |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain) | ASP.NET Core にのみ適用されます 1.x です。<br><br> Cookie が提供されたドメイン名です。 |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly) | ASP.NET Core にのみ適用されます 1.x です。<br><br> クッキーはサーバーにのみアクセスできるかどうかを示すフラグ。<br><br>既定値は `true` です。 |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath) | ASP.NET Core にのみ適用されます 1.x です。<br><br> 同じホスト名で実行されているアプリを分離するために使用します。 |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure) | ASP.NET Core にのみ適用されます 1.x です。<br><br> 作成された cookie を HTTPS に限られますかどうかを示すフラグ (`CookieSecurePolicy.Always`)、HTTP または HTTPS (`CookieSecurePolicy.None`)、または要求と同じプロトコル (`CookieSecurePolicy.SameAsRequest`)。<br><br>既定値は `CookieSecurePolicy.SameAsRequest` です。 |
| [CookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.cookiemanager) | 要求の cookie を取得または応答には設定を使用するコンポーネントです。 | [ChunkingCookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.chunkingcookiemanager) |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider) | かどうか設定すると、プロバイダーが使用して、 [CookieAuthenticationHandler](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationhandler)データ保護のためです。 |
| [説明](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description) | ASP.NET Core にのみ適用されます 1.x です。<br><br> アプリに使用できる認証の種類に関する追加情報。 |
| [イベント](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events) | ハンドラーは、処理が発生している特定の時点でのアプリの制御がプロバイダーでメソッドを呼び出します。 |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype) | かどうか設定すると、サービスに取得する型、`Events`プロパティではなくインスタンス (から継承された[AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions))。 |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan) | どの程度時間、cookie は有効なまま作成された時点からに格納されている認証チケットを制御します。<br><br>既定値は、14 日間です。 |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath) | ユーザーが承認されていない、ときに、ログインにこのパスにリダイレクトしています。<br><br>既定値は `/Account/Login` です。 |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath) | ユーザーがログアウトするときは、このパスにリダイレクトしています。<br><br>既定値は `/Account/Logout` です。 |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter) | ミドルウェアによって付加されるクエリ文字列パラメーターの名前を指定ときに、 *401 Unauthorized*にステータス コードを変更、 *302 リダイレクト*ログイン パスにします。<br><br>既定値は `ReturnUrl` です。 |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore) | 要求間で id を格納するための省略可能なコンテナーです。 |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration) | True の場合、現在 cookie が有効期限 ウィンドウから複数のちょうど中間にあるときに新しい有効期限時刻に新しい cookie が発行されます。<br><br>既定値は `true` です。 |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat) | `TicketDataFormat`を保護し、id および cookie の値に格納されているその他のプロパティの保護を解除するために使用します。 |
