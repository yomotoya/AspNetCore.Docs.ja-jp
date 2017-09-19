---
title: "ASP.NET Core Id を構成します。"
author: AdrienTorris
description: "ASP.NET Core Id で、既定値を把握し、カスタム値を使用するさまざまな Id プロパティを構成します。"
keywords: "ASP.NET Core、Identity、認証、セキュリティ"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: adf577ae1e1c752c3b1a332ec94a7a7627a7a4b4
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity"></a>Id を構成します。

ASP.NET Core Id が、アプリケーションで簡単にオーバーライドできる既定の動作によって`Startup`クラスです。

## <a name="passwords-policy"></a>パスワード ポリシー

既定では、Id は、パスワードに、大文字、小文字、数字、および英数字 1 文字を含めることが必要です。 その他のいくつかの制限もあります。 パスワードの制限を簡略化する場合するには、`Startup`アプリケーションのクラスです。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 追加 2.0、`RequiredUniqueChars`プロパティです。 それ以外の場合、オプションは、ASP.NET Core から同じ 1.x です。

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`次のプロパティがあります。
* `RequireDigit`: 0 ~ 9 では、パスワードに数字が必要です。 既定値は true です。
* `RequiredLength`: パスワードの最小長。 既定値は 6 です。
* `RequireNonAlphanumeric`: パスワードの英数字以外の文字が必要です。 既定値は true です。
* `RequireUppercase`: パスワードに大文字が必要です。 既定値は true です。
* `RequireLowercase`: パスワードに小文字が必要です。 既定値は true です。
* `RequiredUniqueChars`: パスワードの個別の文字数が必要です。 既定値は 1 です。


## <a name="users-lockout"></a>ユーザーのロックアウト

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`次のプロパティがあります。
* `DefaultLockoutTimeSpan`: 時間数、ユーザーがロックアウト ロックアウトが発生するとします。 既定値は 5 分です。
* `MaxFailedAccessAttempts`: 失敗したアクセス試行回数を、ユーザーがロックアウトされるまでロックアウトが有効になっている場合。 既定値は 5 です。
* `AllowedForNewUsers`: 新しいユーザーをロックアウトできるかどうかを判断します。既定値は true です。


## <a name="sign-in-settings"></a>サインイン設定

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`次のプロパティがあります。
* `RequireConfirmedEmail`: 未確認の電子メールにサインインする必要があります。 既定値は false。
* `RequireConfirmedPhoneNumber`: サインインに確認の電話番号が必要です。 既定値は false。


## <a name="user-validation-settings"></a>ユーザーの検証の設定

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`次のプロパティがあります。
* `RequireUniqueEmail`: 各ユーザーを一意の電子メールを持っている必要があります。 既定値は false。
* `AllowedUserNameCharacters`: ユーザー名に文字を指定できます。 既定値は abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+ です。

## <a name="applications-cookie-settings"></a>アプリケーションの cookie の設定

パスワード ポリシーと同様に、アプリケーションの cookie のすべての設定を変更できます、`Startup`クラスです。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`ConfigureServices`で、`Startup`クラス、アプリケーションの cookie を構成することができます。

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

`CookieAuthenticationOptions`次のプロパティがあります。
* `Cookie.Name`: Cookie の名前。 既定値です。AspNetCore.Cookies です。
* `Cookie.HttpOnly`: True の場合、cookie はクライアント側スクリプトからアクセスできません。 既定値は true です。
* `ExpireTimeSpan`: 認証チケットがクッキーに格納されている時間は作成された時点から有効になるを制御します。 既定値は 14 日間です。
* `LoginPath`: ユーザーが承認されていない、ときに、ログインにこのパスにリダイレクトされます。 ログイン/アカウント/既定値です。
* `LogoutPath`: ユーザーがログアウトされるときは、このパスにリダイレクトされます。 既定値は、ログアウト/アカウント/です。
* `AccessDeniedPath`: ユーザーには、承認チェックが失敗すると、ときに、このパスにリダイレクトされます。 既定値は/アカウント/AccessDenied です。
* `SlidingExpiration`: True の場合、現在 cookie が有効期限 ウィンドウから複数のちょうど中間にあるときに新しい有効期限時刻に新しい cookie が発行されます。 既定値は true です。
* `ReturnUrlParameter`:、ReturnUrlParameter は、は 401 Unauthorized ステータス コードがログイン パスへの 302 リダイレクトに変更されたときに、ミドルウェアによって付加されるクエリ文字列パラメーターの名前を決定します。
* `AuthenticationScheme`: ASP.NET Core これはのみ 1.x です。 特定の認証スキームの論理名。
* `AutomaticAuthenticate`: このフラグにのみ関連 ASP.NET Core 1.x です。 True の場合、cookie 認証を要求ごとに実行しを検証し、作成された任意のシリアル化されたプリンシパルを再構築しようとします。

