---
title: その他の要求と ASP.NET Core での外部プロバイダーからのトークンを保存します。
author: guardrex
description: その他の要求と外部プロバイダーからトークンを確立する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: e18287e5a4171b3f7a6daa122111448b8447c1da
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874846"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>その他の要求と ASP.NET Core での外部プロバイダーからのトークンを保存します。

作成者: [Luke Latham](https://github.com/guardrex)

ASP.NET Core アプリは、追加の要求および Facebook、Google、Microsoft、Twitter などの外部認証プロバイダーからのトークンを確立できます。 各プロバイダーが、そのプラットフォームでユーザーに関するさまざまな情報が表示されますが、受信し、その他の要求にユーザー データを変換するためのパターンは同じです。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

## <a name="prerequisites"></a>必須コンポーネント

アプリでサポートするには、どの外部認証プロバイダーを決定します。 プロバイダーごとにアプリを登録して、クライアント ID とクライアント シークレットを取得します。 詳細については、「 <xref:security/authentication/social/index> 」を参照してください。 サンプル アプリでは、 [Google の認証プロバイダー](xref:security/authentication/google-logins)します。

## <a name="set-the-client-id-and-client-secret"></a>クライアント ID とクライアント シークレットを設定します。

OAuth 認証プロバイダーは、クライアント ID とクライアント シークレットを使用するアプリとの信頼関係を確立します。 クライアント ID とクライアント シークレットの値はアプリ用に作成、外部認証プロバイダーによって、アプリが、プロバイダーに登録されている場合。 各外部プロバイダー、アプリが使用するは、プロバイダーのクライアント ID とクライアント シークレット個別に構成する必要があります。 詳細については、実際のシナリオに適用される、外部認証プロバイダーについてのトピックを参照してください。

* [Facebook での認証](xref:security/authentication/facebook-logins)
* [Google での認証](xref:security/authentication/google-logins)
* [Microsoft での認証](xref:security/authentication/microsoft-logins)
* [Twitter での認証](xref:security/authentication/twitter-logins)
* [他の認証プロバイダー](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

サンプル アプリは、クライアント ID とクライアント シークレットが Google から提供されると、Google の認証プロバイダーを構成します。

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>認証のスコープを確立します。

指定することで、プロバイダーから取得するアクセス許可の一覧を指定、<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>します。 一般的な外部プロバイダーの認証のスコープは、次の表に表示されます。

| プロバイダー  | スコープ                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

サンプル アプリで、Google の`userinfo.profile`スコープは、フレームワークによって自動的に追加時に<xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>で呼び出される、<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>します。 アプリは、追加のスコープを必要とする場合は、オプションを追加します。 次の例では、Google`https://www.googleapis.com/auth/user.birthday.read`スコープは、ユーザーの誕生日を取得するために追加されます。

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>ユーザー データのキーをマップし、要求の作成

プロバイダーのオプションでは、指定、<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*>または<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*>の外部プロバイダーのユーザー データを JSON のサインイン時の読み取りをアプリ id の各キー サブキー/。 要求の種類の詳細については、次を参照してください。<xref:System.Security.Claims.ClaimTypes>します。

サンプル アプリは、ロケールを作成します (`urn:google:locale`) と画像 (`urn:google:picture`) からの要求、`locale`と`picture`Google ユーザー データ内のキー。

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>、 <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) が使用して、アプリにサインイン<xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>します。 プロセスでは、サインアップ時に、<xref:Microsoft.AspNetCore.Identity.UserManager%601>格納できる、`ApplicationUser`から使用可能なユーザー データの要求、<xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>します。

サンプル アプリで`OnPostConfirmationAsync`(*Account/ExternalLogin.cshtml.cs*)、ロケールの確立 (`urn:google:locale`) と画像 (`urn:google:picture`)、署名付きの要求で`ApplicationUser`、クレームを含む<xref:System.Security.Claims.ClaimTypes.GivenName>:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

既定では、ユーザーの要求は認証 cookie に格納されます。 認証 cookie が大きすぎる場合、アプリケーションのために失敗する可能性します。

* ブラウザーでは、cookie のヘッダーが長すぎることを検出します。
* 要求の全体的なサイズが大きすぎます。

大量のユーザー データがユーザーの要求を処理するために必要な場合。

* 要求にのみ、アプリが必要な処理のユーザー要求のサイズと数を制限します。
* 使用して、カスタム<xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore>の Cookie 認証ミドルウェアの<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore>要求間で id を格納します。 大量ののみ、小規模のセッション識別子のキーをクライアントに送信中に、サーバー上の id 情報が保持されます。

## <a name="save-the-access-token"></a>アクセス トークンを保存します。

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> アクセスと更新トークンを保存するかどうかを定義、<xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties>成功した認証が完了後します。 `SaveTokens` 設定されている`false`既定では、最終的な認証 cookie のサイズを小さくします。

サンプル アプリの値を設定する`SaveTokens`に`true`で<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

ときに`OnPostConfirmationAsync`を実行するアクセス トークンを保存 ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) で外部プロバイダーから、`ApplicationUser`の`AuthenticationProperties`します。

サンプル アプリでアクセス トークンを保存します`OnPostConfirmationAsync`(新しいユーザーの登録) と`OnGetCallbackAsync`(以前に登録したユーザー) で*Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>追加のカスタム トークンを追加する方法

一部として格納されているカスタムのトークンを追加する方法を説明するために`SaveTokens`、サンプル アプリを追加、<xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>を現在<xref:System.DateTime>の[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*)の`TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a>作成して、要求の追加

フレームワークは、一般的な操作と、コレクションにクレームを追加する拡張メソッドを提供します。 詳細については、「 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 」および「 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>」を参照してください。

派生することによって、ユーザーがカスタム アクションを定義できます<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction>抽象の実装と<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>メソッド。

詳細については、「 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims> 」を参照してください。

## <a name="removal-of-claim-actions-and-claims"></a>要求アクションと要求の削除

[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)すべて削除要求のアクションを指定された<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType>コレクションから。 [ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)のクレームを削除する、指定された<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType>id からです。 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> 主に併用[OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc)プロトコルによって生成されたクレームを削除します。

## <a name="sample-app-output"></a>アプリのサンプル出力

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="additional-resources"></a>その他の技術情報

* [aspnet/AspNetCore エンジニア リング SocialSample アプリ](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample)&ndash;リンクされているサンプル アプリが上に、 [aspnet/AspNetCore GitHub リポジトリの](https://github.com/aspnet/AspNetCore)`master`エンジニア リングの分岐。 `master`ブランチには、次のリリースの ASP.NET Core の開発中のコードが含まれています。 ASP.NET Core のリリース バージョンのサンプル アプリのバージョンを表示する、**ブランチ**ドロップダウン リストをリリース ブランチを選択します (たとえば`release/2.2`)。
