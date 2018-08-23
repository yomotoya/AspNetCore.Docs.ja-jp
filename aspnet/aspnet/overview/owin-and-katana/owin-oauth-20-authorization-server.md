---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: OWIN OAuth 2.0 承認サーバー |Microsoft Docs
author: hongyes
description: このチュートリアルでは、OAuth の OWIN ミドルウェアを使用して OAuth 2.0 承認サーバーを実装する方法について説明します。 これは、高度なチュートリアルをそののみ outlin です.
ms.author: riande
ms.date: 03/20/2014
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: 7b9ba2ca0cd0269ebb3e0e4ae056d4597a198a03
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838018"
---
<a name="owin-oauth-20-authorization-server"></a>OWIN OAuth 2.0 承認サーバー
====================
によって[Hongye Sun](https://github.com/hongyes)、 [Praburaj Thiagarajan](https://github.com/Praburaj)、 [Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、OAuth の OWIN ミドルウェアを使用して OAuth 2.0 承認サーバーを実装する方法について説明します。 これは、のみ OWIN OAuth 2.0 承認サーバーを作成する手順が説明されている、高度なチュートリアルです。 これは、ステップ バイ ステップ チュートリアルではありません。 [サンプル コードをダウンロード](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip)します。
> 
> > [!NOTE]
> > このアウトラインは、セキュリティで保護された実稼働アプリを作成するために使用されるされないものする必要があります。 このチュートリアルの目的は、OAuth の OWIN ミドルウェアを使用して OAuth 2.0 承認サーバーを実装する方法のアウトラインのみを提供します。
> 
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> | **このチュートリアルで示すように** | **でも使用できます。** |
> | --- | --- |
> | Windows 8.1 | Windows 8、Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Visual Studio 2013 Express for Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)します。 Visual Studio 2012 と最新の更新プログラムが動作する必要がありますが、このチュートリアルは、テストされていないといくつかのメニューとダイアログ ボックスが異なる。 |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>意見やご質問
> 
> チュートリアルに直接関連付けられていない質問がある場合でこれらを投稿[Katana プロジェクト github](https://github.com/aspnet/AspNetKatana/)します。 チュートリアルに関する意見やご質問は、ページの下部にあるコメント セクションを参照してください。


[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) HTTP サービスへのアクセス制限を取得するサード パーティ製アプリができるようにします。 保護されたリソースにアクセスするリソース所有者の資格情報を使用する代わりに、クライアントがアクセス トークンを取得します (これは、文字列、特定のスコープ、有効期間、およびその他のアクセス属性を示す)。 アクセス トークンは、リソース所有者の承認をサード パーティ製のクライアントに、承認サーバーによって発行されます。

このチュートリアルを取り上げます。

- 次の 4 つの承認をサポートする承認サーバーを作成する方法と更新トークンを付与タイプ。
    - 認証コード付与
    - 暗黙的な許可
    - リソース所有者のパスワード資格情報の付与
    - クライアントの資格情報の付与
- アクセス トークンで保護されたリソース サーバーを作成します。
- OAuth 2.0 クライアントを作成します。

<a id="prerequisites"></a>
## <a name="prerequisites"></a>必須コンポーネント

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions)または無料[Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express)に記載されている、**ソフトウェア バージョン**ページの上部にあります。
- OWIN の知識。 参照してください[Katana プロジェクトの概要](https://msdn.microsoft.com/magazine/dn451439.aspx)と[OWIN と Katana の新](index.md)します。
- 知識[OAuth](http://tools.ietf.org/html/rfc6749)用語では、含む[ロール](http://tools.ietf.org/html/rfc6749#section-1.1)、[プロトコル フロー](http://tools.ietf.org/html/rfc6749#section-1.2)と[承認付与](http://tools.ietf.org/html/rfc6749#section-1.3)します。 [OAuth 2.0 の概要](http://tools.ietf.org/html/rfc6749#section-1)の紹介を提供します。

## <a name="create-an-authorization-server"></a>承認サーバーを作成します。

このチュートリアルでは使用する方法についてはスケッチほぼ[OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx)と ASP.NET MVC、承認サーバーを作成します。 このチュートリアルには、各ステップが含まれていないとすぐに完全なサンプルのダウンロードを提供することと思います。 という名前の空の web アプリを最初に、作成*AuthorizationServer*し、次のパッケージをインストールします。

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (または Microsoft.Owin.Security.Facebook などその他の任意のソーシャル ログインをパッケージ化)

追加、 [OWIN Startup クラス](owin-startup-class-detection.md)という名前のプロジェクト ルート フォルダーの下*スタートアップ*します。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

作成、*アプリ\_開始*フォルダー。 選択、*アプリ\_開始*フォルダーとのダウンロードしたバージョンを追加するには Shift + Alt + A を使用して、 *AuthorizationServer\App\_Start\Startup.Auth.cs*ファイル。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

上記のコードでは、アプリケーション/外部サインイン cookie および Google の認証、承認サーバー自体でアカウントを管理するために使用でを使用できます。

`UseOAuthAuthorizationServer`拡張メソッドは、承認サーバーをセットアップします。 セットアップのオプションがあります。

- `AuthorizeEndpointPath`: クライアント アプリケーションがユーザーを取得するために、ユーザー エージェントをリダイレクト場所の要求パスは、コードまたはトークン発行に同意します。 たとえば、先頭のスラッシュで始める必要があります"`/Authorize`"。
- `TokenEndpointPath`要求パスのクライアント アプリケーションは、アクセス トークンを取得する直接通信します。 "/Token"のように、先頭にスラッシュが始まる必要があります。 クライアントが発行されている場合、[クライアント\_シークレット](http://tools.ietf.org/html/rfc6749#appendix-A.2)、このエンドポイントに指定する必要があります。
- `ApplicationCanDisplayErrors`: に設定します。 `true` web アプリケーションがクライアントの検証エラーのカスタム エラー ページを生成する必要がある場合`/Authorize`エンドポイント。 たとえばバックアップ、クライアント アプリケーションに、ブラウザーがリダイレクトしない場合にのみ必要です、ときに、`client_id`または`redirect_uri`が正しくありません。 `/Authorize`エンドポイントが"oauth を表示することが予想されます。エラー"、"oauth します。ErrorDescription"、および"oauth します。ErrorUri"プロパティは、OWIN 環境に追加されます。 

    > [!NOTE]
    > 指定しない場合、true の場合、承認サーバーは既定のエラー ページとエラーの詳細が返すされます。
- `AllowInsecureHttp`: 承認し、HTTP URI アドレスに到着して、受信を許可するトークンが要求を許可する True を`redirect_uri`要求パラメーターが HTTP URI アドレスを承認します。 

    > [!WARNING]
    > セキュリティ - これは開発専用の。
- `Provider`: 認証サーバー ミドルウェアによって発生したイベントを処理するアプリケーションによって提供されるオブジェクト。 アプリケーションが完全には、インターフェイスを実装することがありますまたはのインスタンスを作成することがあります`OAuthAuthorizationServerProvider`OAuth フローがこのサーバーをサポートするために必要なデリゲートを割り当てます。
- `AuthorizationCodeProvider`: クライアント アプリケーションに返される単一使用承認コードを生成します。 OAuth サーバーは、アプリケーションをセキュリティで保護**する必要があります**のインスタンスを指定`AuthorizationCodeProvider`によってトークンが生成される、`OnCreate/OnCreateAsync`イベントが 1 つだけの呼び出しの有効と見なさ`OnReceive/OnReceiveAsync`します。
- `RefreshTokenProvider`: 必要なときに、新しいアクセス トークンを生成するために使用できる更新トークンを生成します。 かどうかは指定されていない、承認サーバーは返されませんから更新トークン、`/Token`エンドポイント。

## <a name="account-management"></a>アカウントの管理

OAuth は、ユーザー アカウント情報の管理や場所、方法に関係ありません。 [ASP.NET Identity](../../../identity/index.md)はその責任を負います。 このチュートリアルでは、アカウント管理コードを簡略化し、だけそのユーザーがログインする OWIN クッキー ミドルウェアを使用していることを確認します。 次の簡略化されたサンプル コードに示します、 `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri` その登録されたリダイレクト URL を使用してクライアントの検証に使用されます。 `ValidateClientAuthentication` 基本的なスキームのヘッダーとクライアントの資格情報を取得するフォームの本文を確認します。

ログイン ページは、以下に示します。

![](owin-oauth-20-authorization-server/_static/image1.png)

IETF の OAuth 2 の確認[認証コード付与](http://tools.ietf.org/html/rfc6749#section-4.1)セクションのようになりました。 

**プロバイダー** (次の表では) 機能は[OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx)します。型のプロバイダー、 `OAuthAuthorizationServerProvider`、OAuth サーバーのすべてのイベントが含まれています。 

| 認証コード付与セクションからフローのステップ | サンプルのダウンロードでは、これらの手順を実行します。 |
| --- | --- |
|  |  |
| (A)、クライアントは、リソース所有者のユーザー エージェントを承認エンドポイントにダイレクトすることで、フローを開始します。 クライアントには、クライアント識別子、要求されたスコープ、ローカルの状態、および承認サーバーが送信するユーザー エージェントへのアクセスが許可 (または拒否された) 後のリダイレクト URI が含まれています。 | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B)、承認サーバーは、(ユーザー エージェント) を使用して、リソースの所有者を認証し、リソース所有者が許可されたり、クライアントのアクセス要求を拒否するかどうかを確立します。 | **&lt;ユーザー アクセスを許可する場合&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (承認サーバーは、リダイレクト URI を使用してクライアントにユーザー エージェントをリダイレクトのアクセスを許可 C)、リソースの所有者と仮定した場合 (要求またはクライアント登録時に) 以前。 ... |  |
|  |  |
| (D)、クライアントは、前の手順で受け取った承認コードを含めることによって、承認サーバーのトークン エンドポイントからアクセス トークンを要求します。 要求を行うときに、クライアントは承認サーバーで認証されます。 クライアントには、URI 検証のための認証コードを取得するために使用するリダイレクトが含まれています。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsyncRefreshTokenProvider.CreateAsync |

実装サンプル`AuthorizationCodeProvider.CreateAsync`と`ReceiveAsync`を作成し、認証コードの検証の制御を以下に示します。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

上記のコードでは、コードと id のチケットを格納して、コードを受信した後は、id を復元するメモリ内の同時実行ディクショナリを使用します。 実際のアプリケーションで永続的なデータ ストアによって置き換えが。 承認エンドポイントは、クライアントへのアクセス許可リソースの所有者です。 通常、ユーザー インターフェイスにボタンをクリックし、許可を確認します。 ユーザーを許可する必要があります。 OWIN OAuth のミドルウェアは、承認エンドポイントを処理するためにアプリケーション コードを利用します。 このサンプル アプリでと呼ばれる MVC コント ローラーを使用して`OAuthController`を処理します。 サンプル実装を次に示します。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

`Authorize`アクションでは、ユーザーがログインに承認サーバーへのかどうかはまず調べます。 存在しない場合、認証ミドルウェアの課題を呼び出し元「アプリケーション」の cookie を使用して認証をログイン ページにリダイレクトされます。 (上記の強調表示されたコードを参照してください)。ユーザーがログインに場合、次に示すよう、承認のビューがレンダリングされます。

![](owin-oauth-20-authorization-server/_static/image2.png)

場合、 **Grant**ボタンが選択されている、`Authorize`アクションは、新しい"Bearer"の id と、サインインを作成します。 ベアラー トークンを生成し、JSON ペイロードをクライアントに送信する承認サーバーのトリガーとなります。 

### <a name="implicit-grant"></a>暗黙的な許可

IETF の OAuth 2 を参照してください[Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2)セクションのようになりました。

 [Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2)フロー図 4 は、フローとマッピングする OWIN OAuth ミドルウェアに依存します。  

| 暗黙的な許可 セクションからフローのステップ | サンプルのダウンロードでは、これらの手順を実行します。 |
| --- | --- |
|  |  |
| (A)、クライアントは、リソース所有者のユーザー エージェントを承認エンドポイントにダイレクトすることで、フローを開始します。 クライアントには、クライアント識別子、要求されたスコープ、ローカルの状態、および承認サーバーが送信するユーザー エージェントへのアクセスが許可 (または拒否された) 後のリダイレクト URI が含まれています。 | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B)、承認サーバーは、(ユーザー エージェント) を使用して、リソースの所有者を認証し、リソース所有者が許可されたり、クライアントのアクセス要求を拒否するかどうかを確立します。 | **&lt;ユーザー アクセスを許可する場合&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (承認サーバーは、リダイレクト URI を使用してクライアントにユーザー エージェントをリダイレクトのアクセスを許可 C)、リソースの所有者と仮定した場合 (要求またはクライアント登録時に) 以前。 ... |  |
|  |  |
| (D)、クライアントは、前の手順で受け取った承認コードを含めることによって、承認サーバーのトークン エンドポイントからアクセス トークンを要求します。 要求を行うときに、クライアントは承認サーバーで認証されます。 クライアントには、URI 検証のための認証コードを取得するために使用するリダイレクトが含まれています。 |  |

承認エンドポイントを既に実装されていますので (`OAuthController.Authorize`アクション) の認証コード付与では、それを自動的に有効暗黙的フローもします。 注:`Provider.ValidateClientRedirectUri`送信アクセス トークンを悪意のあるクライアントからの暗黙的な許可フローは、保護、そのリダイレクト URL にクライアント ID を検証するために使用 ([で中間者攻撃](https://www.owasp.org/index.php/Man-in-the-middle_attack))。

### <a name="resource-owner-password-credentials-grant"></a>リソース所有者のパスワード資格情報の付与

IETF の OAuth 2 を参照してください[リソース所有者パスワード資格情報付与](http://tools.ietf.org/html/rfc6749#section-4.3)セクションのようになりました。

 [リソース所有者パスワード資格情報付与](http://tools.ietf.org/html/rfc6749#section-4.3)フロー図 5 に示すは、フローとマッピングする OWIN OAuth ミドルウェアに依存します。  

| リソース所有者パスワード資格情報の付与 セクションからフローのステップ | サンプルのダウンロードでは、これらの手順を実行します。 |
| --- | --- |
|  |  |
| (A) リソースの所有者では、そのユーザー名とパスワード、クライアントを提供します。 |  |
|  |  |
| (B)、クライアントは、リソース所有者から受け取った資格情報を含めることによって、承認サーバーのトークン エンドポイントからアクセス トークンを要求します。 要求を行うときに、クライアントは承認サーバーで認証されます。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C)、承認サーバーは、クライアントを認証し、リソース所有者の資格情報を検証し、有効な場合は、アクセス トークンを発行します。 |  |

サンプルの実装を次に示します`Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> 上記のコード、チュートリアルのこのセクションで説明するものでは、安全では使用する必要がありますまたは実稼働アプリ。 リソース所有者の資格情報は確認しません。 すべての資格情報が有効でありの新しい id を作成します。 前提としています。 新しい id は、アクセス トークンを生成し、更新トークンに使用されます。 セキュリティで保護されたアカウント管理コードには、コードに置き換えてください。


### <a name="client-credentials-grant"></a>クライアントの資格情報の付与

IETF の OAuth 2 を参照してください[クライアント資格情報付与](http://tools.ietf.org/html/rfc6749#section-4.4)セクションのようになりました。

 [クライアント資格情報付与](http://tools.ietf.org/html/rfc6749#section-4.4)フロー図 6 に示すようには、フローとマッピングする OWIN OAuth ミドルウェアに依存します。  

| クライアント資格情報の付与 セクションからフローのステップ | サンプルのダウンロードでは、これらの手順を実行します。 |
| --- | --- |
|  |  |
| (A)、クライアントは、承認サーバーで認証し、トークン エンドポイントからアクセス トークンを要求します。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B)、承認サーバーは、クライアントを認証し、有効な場合は、アクセス トークンを発行します。 |  |

サンプルの実装を次に示します`Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> 上記のコード、チュートリアルのこのセクションで説明するものでは、安全では使用する必要がありますまたは実稼働アプリ。 セキュリティで保護されたクライアント管理コードには、コードに置き換えてください。


### <a name="refresh-token"></a>更新トークン

IETF の OAuth 2 を参照してください[更新トークン](http://tools.ietf.org/html/rfc6749#section-1.5)セクションのようになりました。

 [更新トークン](http://tools.ietf.org/html/rfc6749#section-1.5)フロー図 2 は、フローとマッピングする OWIN OAuth ミドルウェアに依存します。  

| クライアント資格情報の付与 セクションからフローのステップ | サンプルのダウンロードでは、これらの手順を実行します。 |
| --- | --- |
|  |  |
| (G)、クライアントは、承認サーバーによる認証方法と更新トークンを提示して新しいアクセス トークンを要求します。 クライアントの認証要件は、クライアントの種類とサーバーの承認ポリシーに基づいています。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsyncRefreshTokenProvider.CreateAsync |
|  |  |
| (H)、承認サーバーは、クライアントを認証し、更新トークンを検証しますと、有効な場合は、新しいアクセス トークン (および、必要に応じて、新しい更新トークン) を発行します。 |  |

サンプルの実装を次に示します`Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>アクセス トークンで保護されたリソース サーバーを作成します。

空の web アプリ プロジェクトを作成し、次のプロジェクト内のパッケージをインストールします。

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Startup クラスを作成し、認証と Web API を構成します。 参照してください*AuthorizationServer\ResourceServer\Startup.cs*サンプル ダウンロードにします。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

参照してください*AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs*サンプル ダウンロードにします。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

参照してください*AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs*サンプル ダウンロードにします。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors` メソッドは、すべてのドメインに対して CORS を使用します。
- `UseOAuthBearerAuthentication` メソッドは、OAuth ベアラー トークンの認証ミドルウェアが受信し、要求の承認ヘッダーからのベアラー トークンを検証できます。
- `Config.SuppressDefaultHostAuthenticaiton` 既定の抑制、アプリからの認証済みプリンシパルのホスト、この呼び出しの後に匿名してすべての要求があるためです。
- `HostAuthenticationFilter` だけ、指定した認証の種類の認証を有効にします。 この場合は、ベアラー認証の種類を勧めします。

認証済み id を示すためには、現在のユーザーの要求を出力する、ApiController を作成します。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

承認サーバーと、リソース サーバーが同じコンピューター上にない場合は、OAuth ミドルウェアは暗号化ベアラー アクセス トークンを復号化し、別のマシン キーを使用します。 同じ両方のプロジェクト間で同じ秘密キーを共有するには追加`machinekey`両方で設定*web.config*ファイル。

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>OAuth 2.0 クライアントを作成します。

 使用して、 [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet パッケージをクライアント コードを簡略化します。

### <a name="authorization-code-grant-client"></a>承認コード付与クライアント

 このクライアントは、MVC アプリケーションです。 バックエンドからアクセス トークンを取得するには、認証コード付与フローのトリガーとなります。 次に示す 1 つのページがあります。

![](owin-oauth-20-authorization-server/_static/image3.png)

- **Authorize**ボタンは、このクライアントにアクセスを許可するリソースの所有者に通知する承認サーバーへのブラウザーをリダイレクトします。
- **更新**ボタンはトークンを取得、新しいアクセス トークンと更新トークンが現在の更新を使用します。
- **アクセスの保護されたリソースの API**ボタンは現在のユーザーの信頼性情報データを取得し、それらをページに表示するリソース サーバーを呼び出します。

次のサンプル コードに示します、`HomeController`クライアント。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth` 既定で SSL が必要です。 追加する必要があるため、このデモは、HTTP を使用して、次の構成ファイルの設定。

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> セキュリティの運用アプリで無効 SSL ことはありません。 ログイン資格情報は、ネットワーク経由でクリア テキストで送信されるようになりました。 上記のコードは、ローカル サンプルのデバッグと探索のためだけです。


### <a name="implicit-grant-client"></a>暗黙的な許可クライアント

このクライアントは、JavaScript を使用してください。

1. 新しいウィンドウを開き、承認サーバーの承認エンドポイントにリダイレクトします。
2. アクセス トークンを取得 URL フラグメントからリダイレクト戻るときにします。

次の図は、このプロセスを示しています。

![](owin-oauth-20-authorization-server/_static/image4.png)

クライアントが 2 つのページにある必要があります。 ホーム ページで、コールバックのもう 1 つ。JavaScript のコード サンプルを次に示します、 *Index.cshtml*ファイル。

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

処理コードにコールバックを次に示します*SignIn.cshtml*ファイル。

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> JavaScript を外部ファイルに移動し、Razor マークアップを埋め込まないことをお勧めします。 このサンプルをシンプルにするが統合されました。


### <a name="resource-owner-password-credentials-grant-client"></a>リソース所有者のパスワードの資格情報付与クライアント

このクライアントをデモするため、コンソール アプリを使用しています。 次のコードに示します。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>クライアント資格情報付与

リソース所有者パスワード資格情報の付与と同様の場合、ここで、コンソール アプリのコードを示します。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
