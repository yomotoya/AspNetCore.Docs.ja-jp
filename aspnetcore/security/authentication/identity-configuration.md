---
title: "ASP.NET Core Id を構成します。"
author: AdrienTorris
description: "ASP.NET Core Id で、既定値を把握し、カスタム値を使用するさまざまな Id プロパティを構成します。"
manager: wpickett
ms.author: scaddie
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: cf7dcdb80f5edf9e10960cb08957793c36829a69
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="configure-identity"></a>Id を構成します。

ASP.NET Core Id は、パスワード ポリシー、ロックアウト時間、およびアプリケーションの簡単にオーバーライドできる cookie の設定などのアプリケーションで共通の動作を持つ`Startup`クラスです。

## <a name="passwords-policy"></a>パスワード ポリシー

既定では、Id は、パスワードに、大文字、小文字、数字、および英数字以外の文字を含めることが必要です。 その他のいくつかの制限もあります。 パスワードの制限を簡略化の変更、`ConfigureServices`のメソッド、`Startup`アプリケーションのクラスです。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 追加 2.0、`RequiredUniqueChars`プロパティです。 それ以外の場合、オプションは、ASP.NET Core から同じ 1.x です。

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`次のプロパティがあります。

| プロパティ                | 説明                       | 既定値 |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | 0 ~ 9 では、パスワードに数字が必要です。 | true |
| `RequiredLength`        | パスワードの最小長。 | 6 |
| `RequireNonAlphanumeric`| パスワードに英数字以外の文字が必要です。 | true |
| `RequireUppercase`      | パスワードに大文字が必要です。 | true |
| `RequireLowercase`      | パスワードに小文字が必要です。 | true |
| `RequiredUniqueChars`   | パスワードに個別の文字数が必要です。 | 1 |


## <a name="users-lockout"></a>ユーザーのロックアウト

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`次のプロパティがあります。

| プロパティ                | 説明                       | 既定値 |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | 時間数、ユーザーがロックアウト ロックアウトが発生するとします。  | 5 分  |
| `MaxFailedAccessAttempts` | ロックアウトが有効になっている場合、ユーザーがロックされるまで、失敗したアクセス試行の数。  | 5 |
| `AllowedForNewUsers` | 新しいユーザーをロックアウトできるかどうかを判断します。  | true |

## <a name="sign-in-settings"></a>サインイン設定

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`次のプロパティがあります。

| プロパティ                | 説明                       | 既定値 |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | 確認の電子メールにサインインする必要があります。 | False  |
| `RequireConfirmedPhoneNumber` |  確認の電話番号にサインインする必要があります。 | False  |

## <a name="user-validation-settings"></a>ユーザーの検証の設定

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`次のプロパティがあります。

| プロパティ                | 説明                       | 既定値 |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | 各ユーザーを一意の電子メールを持っている必要があります。 | False  |
| `AllowedUserNameCharacters`  | ユーザー名に許容される文字。 | abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+ |


## <a name="applications-cookie-settings"></a>アプリケーションの cookie の設定

パスワード ポリシーと同様に、アプリケーションの cookie のすべての設定を変更できます、`Startup`クラスです。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`ConfigureServices`で、`Startup`クラス、アプリケーションの cookie を構成することができます。

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

`CookieAuthenticationOptions`次のプロパティがあります。

| プロパティ                | 説明                       | 既定値 |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | Cookie の名前。  | .AspNetCore.Cookies.  |
| `Cookie.HttpOnly`  | True の場合、cookie はクライアント側スクリプトからアクセスできません。  |  true |
| `ExpireTimeSpan`  | 認証チケットがクッキーに格納されている時間は作成された時点から有効になるかを制御します。  | 14 日間  |
| `LoginPath`  | ユーザーが承認されていない、ときに、ログインにこのパスにリダイレクトされます。 | /Account/Login  |
| `LogoutPath`  | ユーザーがログアウトするときは、このパスにリダイレクトされます。  | /Account/Logout  |
| `AccessDeniedPath`  | ユーザーには、承認チェックが失敗した場合は、このパスにリダイレクトされます。  |   |
| `SlidingExpiration`  | True の場合、現在 cookie が有効期限 ウィンドウから複数のちょうど中間にあるときに新しい有効期限時刻に新しい cookie が発行されます。  | /Account/AccessDenied |
| `ReturnUrlParameter`  | 401 Unauthorized ステータス コードがログイン パスへの 302 リダイレクトに変更されたときに、ミドルウェアによって付加されるクエリ文字列パラメーターの名前を決定します。  |  true |
| `AuthenticationScheme`  | これにのみ関連 ASP.NET Core 1.x です。 特定の認証スキームの論理名。 |  |
| `AutomaticAuthenticate`  | このフラグにのみ関連 ASP.NET Core 1.x です。 True の場合、cookie 認証を要求ごとに実行しを検証し、作成された任意のシリアル化されたプリンシパルを再構築しようとします。  |  |
