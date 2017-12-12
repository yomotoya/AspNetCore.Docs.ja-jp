---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: "OWIN OAuth 2.0 承認サーバー |Microsoft ドキュメント"
author: hongyes
description: "このチュートリアルでは、OAuth の OWIN ミドルウェアを使用して OAuth 2.0 承認サーバーを実装する方法について説明します。 これは高度なチュートリアル、その唯一の outlin しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/20/2014
ms.topic: article
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: 8842f57df84d841df77b34e9645dbf4909f82d85
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="owin-oauth-20-authorization-server"></a>OWIN OAuth 2.0 承認サーバー
====================
によって[Hongye Sun](https://github.com/hongyes)、 [Praburaj Thiagarajan](https://github.com/Praburaj)、 [Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、OAuth の OWIN ミドルウェアを使用して OAuth 2.0 承認サーバーを実装する方法について説明します。 これは、のみ OWIN OAuth 2.0 承認サーバーを作成する手順が説明されている高度なチュートリアルです。 これは、ステップ バイ ステップ チュートリアルではありません。 [サンプル コードをダウンロード](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip)です。
> 
> > [!NOTE]
> > このアウトラインは、セキュリティで保護された実稼働アプリケーションの作成に使用するものでありません必要があります。 このチュートリアルは、OAuth の OWIN ミドルウェアを使用して OAuth 2.0 承認サーバーを実装する方法について概要を提供するためのものです。
> 
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> | **このチュートリアルで示す** | **でも動作します。** |
> | --- | --- |
> | Windows 8.1 | Windows 8、Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [デスクトップの visual Studio 2013 Express](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)です。 Visual Studio 2012 と最新の更新プログラムの作業する必要がありますが、チュートリアルはこれを使ってテストされていないおよび一部のメニューやダイアログ ボックスが異なる。 |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> チュートリアルに直接関連付けられていない質問がある場合でそれらを投稿[GitHub の Katana プロジェクト](https://github.com/aspnet/AspNetKatana/)です。 に関する質問やコメント自体、チュートリアル、ページの下部にあるコメント セクションを参照してください。


[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) HTTP サービスへのアクセス制限を取得するサード パーティ製アプリを有効にします。 保護されたリソースにアクセスするリソース所有者の資格情報を使用する代わりに、クライアントはアクセス トークンを取得 (文字列は、特定のスコープ、有効期間、およびその他のアクセス属性を示す) です。 アクセス トークンは、リソースの所有者の承認をサード パーティ製のクライアントに、承認サーバーによって発行されます。

このチュートリアルを取り上げます。

- 次の 4 つの承認をサポートする承認サーバーを作成する方法は、付与タイプと更新トークン。
    - 認証コード付与
    - Implicit Grant
    - リソース所有者のパスワード資格情報が付与
    - クライアント資格情報が付与
- アクセス トークンによって保護されているリソース サーバーを作成します。
- OAuth 2.0 クライアントを作成します。

<a id="prerequisites"></a>
## <a name="prerequisites"></a>必須コンポーネント

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) 、無料または[Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express)に示されているように、**ソフトウェア バージョン**ページの上部にあります。
- OWIN 熟知します。 参照してください[Katana プロジェクトの使用を開始する](https://msdn.microsoft.com/en-us/magazine/dn451439.aspx)と[OWIN および Katana 新](index.md)です。
- 十分に理解[OAuth](http://tools.ietf.org/html/rfc6749)用語では、含む[ロール](http://tools.ietf.org/html/rfc6749#section-1.1)、[プロトコル フロー](http://tools.ietf.org/html/rfc6749#section-1.2)、および[Authorization Grant](http://tools.ietf.org/html/rfc6749#section-1.3)です。 [OAuth 2.0 の概要](http://tools.ietf.org/html/rfc6749#section-1)の概要を提供します。

## <a name="create-an-authorization-server"></a>承認サーバーを作成します。

このチュートリアルではおはほぼスケッチを使用する方法を[OWIN](https://msdn.microsoft.com/en-us/magazine/dn451439.aspx)と ASP.NET MVC を承認サーバーを作成します。 このチュートリアルでは、各手順は含まれません、すぐに、完成したサンプルのダウンロードを提供する予定です。 という名前の空の web アプリを最初に、作成*AuthorizationServer*し、次のパッケージをインストールします。

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (その他のソーシャル ログインのパッケージまたは Microsoft.Owin.Security.Facebook など)

追加、 [OWIN スタートアップ クラス](owin-startup-class-detection.md)という名前のプロジェクト ルート フォルダーの下にある*スタートアップ*です。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

作成、*アプリ\_開始*フォルダーです。 選択、*アプリ\_開始*フォルダーと、ダウンロードしたバージョンを追加するには Shift + Alt + A を使用して、 *AuthorizationServer\App\_Start\Startup.Auth.cs*ファイル。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

上記のコードは、アプリケーション/外部サインインは cookie と Google の認証、承認サーバー自体によってアカウントを管理するために使用できます。

`UseOAuthAuthorizationServer`拡張メソッドは、承認サーバーをセットアップします。 セットアップのオプションは次のとおりです。

- `AuthorizeEndpointPath`: クライアント アプリケーションがユーザーを取得するために、ユーザー エージェントをリダイレクト場所の要求パス トークンまたはコードを発行することに同意します。 たとえば、先頭のスラッシュで始める必要があります"`/Authorize`"です。
- `TokenEndpointPath`要求パスのクライアント アプリケーションは、アクセス トークンを取得する直接通信します。 これは、"/token"のように、先頭にスラッシュで始めなければなりません。 クライアントが発行された場合は、[クライアント\_シークレット](http://tools.ietf.org/html/rfc6749#appendix-A.2)、このエンドポイントを指定する必要があります。
- `ApplicationCanDisplayErrors`: に設定します。 `true` web アプリケーションが上のクライアント検証エラーのカスタム エラー ページを生成する場合は`/Authorize`エンドポイント。 たとえばバックアップ、クライアント アプリケーションに、ブラウザーがリダイレクトでない場合にのみ必要です、ときに、`client_id`または`redirect_uri`が正しくありません。 `/Authorize`エンドポイントはず"oauth です。エラー"、"oauth です。ErrorDescription"、および"oauth です。ErrorUri"プロパティは、OWIN 環境に追加されます。 

    > [!NOTE]
    > 以外の場合は true、承認サーバーは返しますエラーの詳細に既定のエラー ページ。
- `AllowInsecureHttp`: True を承認し、HTTP URI アドレスに到着して、受信を許可するトークンが要求を許可する`redirect_uri`HTTP URI アドレスを持つ要求パラメーターを承認します。 

    > [!WARNING]
    > セキュリティ - これは開発専用です。
- `Provider`: 認証サーバー ミドルウェアによって発生したイベントを処理するアプリケーションによって提供されるオブジェクト。 アプリケーションが完全に実装するインターフェイス、またはのインスタンスを作成することがあります、 `OAuthAuthorizationServerProvider` OAuth フローがこのサーバーをサポートしているために必要なデリゲートを割り当てます。
- `AuthorizationCodeProvider`: クライアント アプリケーションに戻るには 1 回だけ認証コードを生成します。 OAuth するサーバーには、アプリケーションをセキュリティで保護**必要があります**のインスタンスを提供`AuthorizationCodeProvider`によってトークンが生成される、`OnCreate/OnCreateAsync`イベントは 1 つだけの呼び出しの有効と見なされます`OnReceive/OnReceiveAsync`です。
- `RefreshTokenProvider`: 必要なときに、新しいアクセス トークンを生成するために使用できる更新トークンを生成します。 かどうかは指定されていない、承認サーバーは返されませんから更新トークン、`/Token`エンドポイント。

## <a name="account-management"></a>アカウント管理

OAuth はしないユーザー アカウント情報を管理する方法や場所を注意してください。 [ASP.NET Identity](../../../identity/index.md)はそれを担当します。 このチュートリアルではおをアカウント管理のコードを簡略化し、ユーザーがログインできる cookie の OWIN ミドルウェアを使用することを確認しておいてください。 簡単なサンプル コードをここでは、 `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri`登録されたリダイレクト URL を使用してクライアントの検証に使用されます。 `ValidateClientAuthentication`基本的なスキームのヘッダーとクライアントの資格情報を取得するフォームの本文を確認します。

ログイン ページは、次に示します。

![](owin-oauth-20-authorization-server/_static/image1.png)

確認、IETF の OAuth 2[認証コード付与](http://tools.ietf.org/html/rfc6749#section-4.1)セクションのようになりました。 

**プロバイダー** (次の表では) は[OAuthAuthorizationServerOptions](https://msdn.microsoft.com/en-us/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx)です。型プロバイダー `OAuthAuthorizationServerProvider`、OAuth サーバーのすべてのイベントが含まれています。 

| 認証コード付与セクションからフロー手順 | ダウンロードしたサンプルで上記の手順を実行します。 |
| --- | --- |
|  |  |
| (A)、クライアントは、リソースの所有者のユーザー エージェントを承認エンドポイントにダイレクトすることで、フローを開始します。 クライアントには、クライアント識別子、要求のスコープ、ローカルの状態、および承認サーバーが送信するユーザー エージェント アクセスを許可 (または拒否) 後のリダイレクト URI が含まれています。 | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B)、承認サーバーは、(ユーザー エージェント) 経由でリソースの所有者を認証し、リソースの所有者が許可されたり、クライアントのアクセス要求を拒否するかどうかを確立します。 | **&lt;ユーザー アクセスを許可する場合&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (承認サーバーは、リダイレクト URI を使用して、クライアントにユーザー エージェントをリダイレクト、アクセスを許可 C) リソースの所有者と仮定した場合 (要求またはクライアントの登録中に) 以前です。 ... |  |
|  |  |
| (D)、クライアントは、前の手順で受信した認証コードを含めることによって、承認サーバーのトークン エンドポイントからアクセス トークンを要求します。 要求を行うときに、クライアントは承認サーバーに対して認証します。 クライアントには、URI 検証のための認証コードを取得するために使用するリダイレクトが含まれています。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsyncRefreshTokenProvider.CreateAsync |

実装サンプル`AuthorizationCodeProvider.CreateAsync`と`ReceiveAsync`を作成し、認証コードの検証の制御を次に示します。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

上記のコードは、コードと id のチケットを格納およびコードが表示されたら、id を復元するメモリ内の同時実行ディクショナリを使用します。 実際のアプリケーションで、永続的なデータ ストアで置き換えられますなります。 承認エンドポイントでは、クライアントへのアクセス許可をリソース所有者のです。 通常、ユーザーがボタンをクリックし、許可を確認できるようにするユーザー インターフェイスが必要です。 OAuth の OWIN ミドルウェアは、承認エンドポイントを処理するアプリケーション コードを許可します。 呼ばれる MVC コント ローラーを使用するアプリでは、サンプル、お`OAuthController`それを処理します。 次に、サンプルの実装を示します。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

`Authorize`アクションのかどうか、ユーザーがログイン承認サーバーはまずチェックします。 ない場合、認証ミドルウェアが"Application"の cookie を使用して認証を呼び出し元の課題をし、ログイン ページにリダイレクトします。 (上記の強調表示されたコードを参照してください)。ユーザー ログインしている場合は、次のように、Authorize ビューを表示します。

![](owin-oauth-20-authorization-server/_static/image2.png)

場合、 **Grant**ボタンが選択されている、`Authorize`アクションは、新しい"Bearer"の id とそれを使用してサインインを作成します。 ベアラー トークンを生成し、JSON ペイロードでクライアントに送信する承認サーバーがトリガーされます。 

### <a name="implicit-grant"></a>Implicit Grant

IETF の OAuth 2 を参照してください[Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2)セクションのようになりました。

 [Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2)図 4 に示すフローは次のフローとマッピングする OWIN OAuth ミドルウェアに依存します。  

| Implicit Grant セクションからフロー手順 | ダウンロードしたサンプルで上記の手順を実行します。 |
| --- | --- |
|  |  |
| (A)、クライアントは、リソースの所有者のユーザー エージェントを承認エンドポイントにダイレクトすることで、フローを開始します。 クライアントには、クライアント識別子、要求のスコープ、ローカルの状態、および承認サーバーが送信するユーザー エージェント アクセスを許可 (または拒否) 後のリダイレクト URI が含まれています。 | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B)、承認サーバーは、(ユーザー エージェント) 経由でリソースの所有者を認証し、リソースの所有者が許可されたり、クライアントのアクセス要求を拒否するかどうかを確立します。 | **&lt;ユーザー アクセスを許可する場合&gt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (承認サーバーは、リダイレクト URI を使用して、クライアントにユーザー エージェントをリダイレクト、アクセスを許可 C) リソースの所有者と仮定した場合 (要求またはクライアントの登録中に) 以前です。 ... |  |
|  |  |
| (D)、クライアントは、前の手順で受信した認証コードを含めることによって、承認サーバーのトークン エンドポイントからアクセス トークンを要求します。 要求を行うときに、クライアントは承認サーバーに対して認証します。 クライアントには、URI 検証のための認証コードを取得するために使用するリダイレクトが含まれています。 |  |

承認エンドポイントは既に実装されているため (`OAuthController.Authorize`アクション) の認証コード付与が自動的に有効に暗黙のフローもです。 注: `Provider.ValidateClientRedirectUri` 、リダイレクト URL は、送信、アクセス トークンを悪意のあるクライアントからの暗黙的な付与フローを保護すると、クライアント ID の検証に使用されます ([仲介で攻撃](https://www.owasp.org/index.php/Man-in-the-middle_attack))。

### <a name="resource-owner-password-credentials-grant"></a>リソース所有者のパスワード資格情報が付与

IETF の OAuth 2 を参照してください[リソース所有者のパスワード資格情報の付与](http://tools.ietf.org/html/rfc6749#section-4.3)セクションのようになりました。

 [リソース所有者のパスワード資格情報の付与](http://tools.ietf.org/html/rfc6749#section-4.3)フロー図 5 に示すように、フローは、ミドルウェアに依存する OWIN OAuth をマッピングします。  

| リソース所有者のパスワード資格情報の付与 セクションからフロー手順 | ダウンロードしたサンプルで上記の手順を実行します。 |
| --- | --- |
|  |  |
| (A) リソースの所有者のユーザー名とパスワードをクライアントに提供します。 |  |
|  |  |
| (B)、クライアントは、リソースの所有者から受信した資格情報を含めることによって、承認サーバーのトークン エンドポイントからアクセス トークンを要求します。 要求を行うときに、クライアントは承認サーバーに対して認証します。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C)、承認サーバーは、クライアントを認証およびリソース所有者の資格情報を検証し、有効な場合は、アクセス トークンを発行します。 |  |

実装のサンプルを次に示します`Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> 上記のコードは、チュートリアルのこのセクションで説明するためのものし、安全では使用できませんまたは運用アプリ。 リソース所有者の資格情報はチェックされません。 すべての資格情報は有効であり、そのため、新しい id の作成と見なします。 新しい id がアクセス トークンを生成し、更新トークンに使用されます。 セキュリティで保護されたアカウント管理コードには、コードに置き換えてください。


### <a name="client-credentials-grant"></a>クライアント資格情報が付与

IETF の OAuth 2 を参照してください[Client Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.4)セクションのようになりました。

 [Client Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.4)フロー図 6 に示すように、フローは、OWIN OAuth をマッピングするミドルウェアに依存します。  

| クライアント資格情報の付与 セクションからフロー手順 | ダウンロードしたサンプルで上記の手順を実行します。 |
| --- | --- |
|  |  |
| (A)、クライアントは承認サーバーに対して認証し、トークン エンドポイントからアクセス トークンを要求します。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B)、承認サーバーは、クライアントを認証し、有効な場合は、アクセス トークンを発行します。 |  |

実装のサンプルを次に示します`Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> 上記のコードは、チュートリアルのこのセクションで説明するためのものし、安全では使用できませんまたは運用アプリ。 セキュリティで保護されたクライアント管理コードには、コードに置き換えてください。


### <a name="refresh-token"></a>更新トークン

IETF の OAuth 2 を参照してください[更新トークン](http://tools.ietf.org/html/rfc6749#section-1.5)セクションのようになりました。

 [更新トークン](http://tools.ietf.org/html/rfc6749#section-1.5)フロー図 2 に示すように、フローは、OWIN OAuth をマッピングするミドルウェアに依存します。  

| クライアント資格情報の付与 セクションからフロー手順 | ダウンロードしたサンプルで上記の手順を実行します。 |
| --- | --- |
|  |  |
| (G)、クライアントは、承認サーバーによる認証方法と更新トークンを提示して、新しいアクセス トークンを要求します。 クライアントの認証要件は、サーバーの承認ポリシーとクライアントの種類に基づいています。 | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsyncRefreshTokenProvider.CreateAsync |
|  |  |
| (H)、承認サーバーは、クライアントを認証し更新トークンを検証し、有効な場合が、新しいアクセス トークン (および、必要に応じて、新しい更新トークン) を発行します。 |  |

実装のサンプルを次に示します`Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>アクセス トークンで保護されているリソース サーバーを作成します。

空の web アプリ プロジェクトを作成し、次のプロジェクト内のパッケージをインストールします。

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

スタートアップ クラスを作成し、認証および Web API を構成します。 参照してください*AuthorizationServer\ResourceServer\Startup.cs*サンプル ダウンロードにします。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

参照してください*AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs*サンプル ダウンロードにします。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

参照してください*AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs*サンプル ダウンロードにします。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors`メソッドは、すべてのドメインに対して CORS を使用します。
- `UseOAuthBearerAuthentication`メソッドは、OAuth ベアラー トークンの認証ミドルウェアを受信し、要求の承認ヘッダーからベアラー トークンを検証できます。
- `Config.SuppressDefaultHostAuthenticaiton`既定値を非表示のアプリからの認証されたプリンシパルをホストするため、すべての要求がされる匿名この呼び出しの後です。
- `HostAuthenticationFilter`だけ、指定した認証の種類の認証を有効にします。 この例では、ベアラー認証の種類を勧めします。

認証済み id を示すためには、するためには、現在のユーザーの信頼性情報を出力する、ApiController を作成します。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

承認サーバーと、リソース サーバーが同じコンピューター上にない場合は、OAuth ミドルウェア キーを使用して、別のコンピューターの暗号化し、ベアラー アクセス トークンの暗号化を解除します。 同じ両方のプロジェクト間での同じ秘密キーを共有するために追加して`machinekey`両方で設定*web.config*ファイル。

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>OAuth 2.0 クライアントを作成されます。

 使用して、 [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet パッケージをクライアント コードを簡略化します。

### <a name="authorization-code-grant-client"></a>認証コード Grant クライアント

 このクライアントは、MVC アプリケーションです。 バックエンドからアクセス トークンを取得するには、認証コード付与フローがトリガーされます。 次のように 1 つのページがあります。

![](owin-oauth-20-authorization-server/_static/image3.png)

- **Authorize**ボタンがブラウザーをクライアントへのアクセス許可をリソース所有者に通知する承認サーバーにリダイレクトされます。
- **更新**ボタンはトークンを取得する、新しいアクセス トークンと更新トークンが現在の更新を使用します。
- **アクセス保護されたリソースの API**ボタンは、リソース サーバーを現在のユーザーの信頼性情報データを取得し、ページに表示することを呼び出します。

サンプル コードをここでは、`HomeController`クライアントのです。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth`既定では SSL が必要です。 追加する必要がありますので、このデモは、HTTP を使用して、次の構成ファイルに設定します。

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> セキュリティの運用アプリケーションで無効 SSL ことはありません。 ネットワーク経由で、クリア テキストで、ログイン資格情報を送信されているようになりましたされます。 上記のコードでは、ローカルのサンプルのデバッグと探索についてのみです。


### <a name="implicit-grant-client"></a>暗黙の Grant クライアント

このクライアントは、JavaScript を使用してください。

1. 新しいウィンドウを開き、承認サーバーの承認エンドポイントにリダイレクトします。
2. アクセス トークンを取得 URL フラグメントから戻るリダイレクト時にします。

次の図は、このプロセスを示しています。

![](owin-oauth-20-authorization-server/_static/image4.png)

クライアントが 2 つのページにある必要があります。 ホーム ページで、コールバックのもう 1 つです。サンプルの JavaScript コードをここでは、 *Index.cshtml*ファイル。

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

ここでは、処理コードでコールバック*SignIn.cshtml*ファイル。

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> JavaScript を外部ファイルに移動し、Razor のマークアップを組み込まないことをお勧めします。 このサンプルをシンプルにするには、統合されました。


### <a name="resource-owner-password-credentials-grant-client"></a>リソース所有者のパスワード許可クライアントの資格情報します。

このクライアントをデモするコンソール アプリを使用しています。 コードを次に示します。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>クライアント資格情報の付与クライアント

リソース所有者のパスワード資格情報の付与と同様の場合、次に、コンソール アプリのコードを示します。

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
