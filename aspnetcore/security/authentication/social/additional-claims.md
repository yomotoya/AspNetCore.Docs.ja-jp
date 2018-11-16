---
title: その他の要求と ASP.NET Core での外部プロバイダーからのトークンを保存します。
author: guardrex
description: その他の要求と外部プロバイダーからトークンを確立する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 9a24ac138950ef2bedac48f506655d06520137cf
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708362"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>その他の要求と ASP.NET Core での外部プロバイダーからのトークンを保存します。

作成者: [Luke Latham](https://github.com/guardrex)

ASP.NET Core アプリは、追加の要求および Facebook、Google、Microsoft、Twitter などの外部認証プロバイダーからのトークンを確立できます。 各プロバイダーが、そのプラットフォームでユーザーに関するさまざまな情報が表示されますが、受信し、その他の要求にユーザー データを変換するためのパターンは同じです。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

## <a name="prerequisites"></a>必須コンポーネント

アプリでサポートするには、どの外部認証プロバイダーを決定します。 プロバイダーごとにアプリを登録して、クライアント ID とクライアント シークレットを取得します。 詳細については、「 <xref:security/authentication/social/index> 」を参照してください。 [サンプル アプリ](#sample-app-instructions)を使用して、 [Google の認証プロバイダー](xref:security/authentication/google-logins)します。

## <a name="set-the-client-id-and-client-secret"></a>クライアント ID とクライアント シークレットを設定します。

OAuth 認証プロバイダーは、クライアント ID とクライアント シークレットを使用するアプリとの信頼関係を確立します。 クライアント ID とクライアント シークレットの値はアプリ用に作成、外部認証プロバイダーによって、アプリが、プロバイダーに登録されている場合。 各外部プロバイダー、アプリが使用するは、プロバイダーのクライアント ID とクライアント シークレット個別に構成する必要があります。 詳細については、実際のシナリオに適用される、外部認証プロバイダーについてのトピックを参照してください。

* [Facebook での認証](xref:security/authentication/facebook-logins)
* [Google での認証](xref:security/authentication/google-logins)
* [Microsoft での認証](xref:security/authentication/microsoft-logins)
* [Twitter での認証](xref:security/authentication/twitter-logins)
* [他の認証プロバイダー](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

サンプル アプリは、クライアント ID とクライアント シークレットが Google から提供されると、Google の認証プロバイダーを構成します。

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

## <a name="establish-the-authentication-scope"></a>認証のスコープを確立します。

指定することで、プロバイダーから取得するアクセス許可の一覧を指定、<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>します。 一般的な外部プロバイダーの認証のスコープは、次の表に表示されます。

| プロバイダー  | スコープ                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/plus.login`                     |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

サンプル アプリの追加、Google`plus.login`スコープ アクセス許可で Google + 記号を要求します。

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

## <a name="map-user-data-keys-and-create-claims"></a>ユーザー データのキーをマップし、要求の作成

プロバイダーのオプションでは、指定、<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*>外部プロバイダーのユーザー データを JSON のサインイン時の読み取りをアプリ id を内の各キー。 要求の種類の詳細については、次を参照してください。<xref:System.Security.Claims.ClaimTypes>します。

サンプル アプリを作成、<xref:System.Security.Claims.ClaimTypes.Gender>からの要求を`gender`Google ユーザーのデータ キー。

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

<xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>、 <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) が使用して、アプリにサインイン<xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>します。 プロセスでは、サインアップ時に、<xref:Microsoft.AspNetCore.Identity.UserManager`1>格納できる、`ApplicationUser`から使用可能なユーザー データの要求、<xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>します。

サンプル アプリで`OnPostConfirmationAsync`(*Account/ExternalLogin.cshtml.cs*) を確立、 <xref:System.Security.Claims.ClaimTypes.Gender> 、符号付きの要求で`ApplicationUser`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

## <a name="save-the-access-token"></a>アクセス トークンを保存します。

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> アクセスと更新トークンを保存するかどうかを定義、<xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties>成功した認証が完了後します。 `SaveTokens` 設定されている`false`既定では、最終的な認証 cookie のサイズを小さくします。

サンプル アプリの値を設定する`SaveTokens`に`true`で<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

ときに`OnPostConfirmationAsync`を実行するアクセス トークンを保存 ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) で外部プロバイダーから、`ApplicationUser`の`AuthenticationProperties`します。

サンプル アプリでアクセス トークンを保存します。

* `OnPostConfirmationAsync` &ndash; 新規ユーザー登録を実行します。
* `OnGetCallbackAsync` &ndash; 以前に登録されたユーザーがアプリにサインインを実行します。

*Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

## <a name="how-to-add-additional-custom-tokens"></a>追加のカスタム トークンを追加する方法

一部として格納されているカスタムのトークンを追加する方法を説明するために`SaveTokens`、サンプル アプリを追加、<xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>を現在<xref:System.DateTime>の[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*)の`TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a>サンプル アプリの手順

サンプル アプリについて説明する方法。

* Google からユーザーの性別を取得し、値は、性別のクレームを格納します。
* ユーザーの Google アクセス トークンは保存`AuthenticationProperties`します。

サンプル アプリを使用します。

1. アプリを登録し、有効なクライアント ID と Google 認証用のクライアント シークレットを取得します。 詳細については、「 <xref:security/authentication/google-logins> 」を参照してください。
1. クライアント ID とクライアント シークレットをアプリに提供、<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>の`Startup.ConfigureServices`します。
1. アプリを実行し、My 要求ページを要求します。 ユーザーが署名されていないときに、アプリを Google にリダイレクトします。 Google でサインインします。 Google は、アプリにユーザーをリダイレクト (`/Home/MyClaims`)。 ユーザーが認証されると、および My 要求ページが読み込まれます。 性別の要求は下**ユーザーの信頼性情報**Google から取得した値を使用します。 表示されます、アクセス トークン、**認証プロパティ**します。

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]
