---
uid: web-api/overview/security/individual-accounts-in-web-api
title: 個々 のアカウントと ASP.NET Web API 2.2 でのローカル ログインを使用して Web API をセキュリティ保護 |Microsoft Docs
author: MikeWasson
description: このトピックでは、メンバーシップ データベースに対する認証に OAuth2 を使用して、web API をセキュリティで保護する方法を示します。 チュートリアルの Visual Studio 201 で使用されるソフトウェアのバージョン.
ms.author: riande
ms.date: 10/15/2014
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 01d117260ef458453bee79285a37a8977221998c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826488"
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>個々 のアカウントと ASP.NET Web API 2.2 でのローカル ログインを使用して Web API をセキュリティで保護します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[サンプル アプリをダウンロードします。](https://github.com/MikeWasson/LocalAccountsApp)

> このトピックでは、メンバーシップ データベースに対する認証に OAuth2 を使用して、web API をセキュリティで保護する方法を示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Web API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


Visual Studio 2013 で、Web API プロジェクト テンプレートを使用して認証用の 3 つのオプション。

- **個々 のアカウント。** アプリでは、メンバーシップ データベースを使用します。
- **組織のアカウント。** ユーザーが Azure Active Directory、Office 365、またはオンプレミスの Active Directory の資格情報でサインインします。
- **Windows 認証。** このオプションは、イントラネット アプリケーションのためのものでは、Windows 認証の IIS モジュールを使用しています。

これらのオプションの詳細については、次を参照してください。 [Visual Studio 2013 で ASP.NET Web プロジェクトの作成](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth)です。

個々 のアカウントは、ユーザーにログインするための 2 つの方法を提供します。

- **ローカル ログイン**します。 ユーザー登録サイトでは、ユーザー名とパスワードを入力します。 アプリは、メンバーシップ データベースにパスワード ハッシュを格納します。 ユーザーがログインすると、ASP.NET Identity システムは、パスワードを検証します。
- **ソーシャル ログイン**します。 ユーザーが Facebook、Microsoft、Google などの外部サービスを使ってサインインします。 アプリは引き続き、メンバーシップ データベースで、ユーザーのエントリを作成しますが、すべての資格情報を格納しません。 外部サービスにサインインして、ユーザーを認証します。

この記事では、ローカル ログイン シナリオ。 ローカルおよびソーシャル ログインは、Web API は、要求の認証に OAuth2 を使用します。 ただし、資格情報フローでは、ローカルおよびソーシャル ログイン異なります。

この記事では、ユーザーがログインし、web API への認証済みの AJAX 呼び出しを送信できる簡単なアプリを紹介します。 サンプル コードをダウンロードする[ここ](https://github.com/MikeWasson/LocalAccountsApp)します。 リリース ノートでは、Visual Studio で最初からサンプルを作成する方法について説明します。

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

サンプル アプリは、データ バインディングと jQuery AJAX 要求を送信するための Knockout.js を使用します。 アドレスは、この記事の Knockout.js を把握する必要はありませんに、AJAX 呼び出しに注目しましょう。

その過程を説明します。

- どのようなアプリケーションのクライアント側で実行します。
- サーバーで何が起きています。
- 中央の HTTP トラフィック。

まず、OAuth2 の用語を定義する必要があります。

- *リソース*します。 一部のデータを保護できます。
- *リソース サーバー*します。 リソースをホストするサーバー。
- *リソース所有者*します。 このエンティティは、リソースにアクセスする権限を許可できます。 (通常はユーザーです。)
- *クライアント*: リソースにアクセスするアプリケーション。 この記事では、クライアントは、web ブラウザーが。
- *アクセス トークン*します。 リソースへのアクセスを許可するトークンです。
- *ベアラー トークン*します。 特定の種類のすべてのユーザーはことに、トークンを使用してプロパティを使用して、アクセス トークン。 つまり、クライアントには、暗号化キーまたはベアラー トークンを使用するには、その他のシークレットが不要です。 そのため、ベアラー トークンは、HTTPS 経由でのみ使用する必要があり、比較的短い有効期限があります。
- *承認サーバー*します。 アクセス トークンを提供するサーバー。

アプリケーションは、承認サーバーとリソース サーバーの両方として機能できます。 Web API プロジェクト テンプレートでは、このパターンに従います。

## <a name="local-login-credential-flow"></a>ローカルのログイン資格情報フロー

Web API を使用して、ローカル ログインの[リソース所有者パスワードのフロー](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) OAuth2 で定義されています。

1. ユーザーは、クライアント名とパスワードを入力します。
2. クライアントは、承認サーバーにこれらの資格情報を送信します。
3. 承認サーバーは、資格情報を認証し、アクセス トークンを返します。
4. クライアントには保護されたリソースにアクセスするには、HTTP 要求の Authorization ヘッダーにアクセス トークンが含まれています。

![](individual-accounts-in-web-api/_static/image3.png)

選択すると**個々 のアカウント**Web API プロジェクト テンプレートで、プロジェクトにはユーザーの資格情報を検証し、トークンを発行する承認サーバーが含まれます。 次の図は、Web API のコンポーネントの観点から、同じ資格情報フローを示します。

![](individual-accounts-in-web-api/_static/image4.png)

このシナリオでは、Web API コント ローラーは、リソース サーバーとして機能します。 認証フィルター、アクセス トークンを検証して、 **[Authorize]** 属性を使用してリソースを保護します。 コント ローラーまたはアクションが持っている場合、 **[Authorize]** 属性は、すべての要求をそのコント ローラーまたはアクションを認証する必要があります。 それ以外の場合、承認が拒否されていると、Web API は 401 (未承認) のエラーを返します。

承認サーバーと両方への呼び出しの認証フィルター、 [OWIN ミドルウェア](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)OAuth2 の詳細を処理するコンポーネント。 このチュートリアルの後半で詳しくデザインを説明します。

## <a name="sending-an-unauthorized-request"></a>未承認の要求を送信します。

最初に、アプリを実行し、をクリックして、 **API の呼び出し**ボタンをクリックします。 要求が完了したら、エラー メッセージが表示されます、**結果**ボックス。 要求が承認されていないため、要求は、アクセス トークンを含まないためにです。

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

**API の呼び出し**ボタン、AJAX 要求を送信します ~/api/値、Web API コント ローラーのアクションを呼び出す。 AJAX 要求を送信する JavaScript コードのセクションを次に示します。 サンプル アプリですべての JavaScript アプリのコードは Scripts\app.js ファイルにあります。

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

ユーザーがログインするまでは、ベアラー トークンがありません、要求に Authorization ヘッダーしたがってがありません。 これにより、要求が 401 エラーが返されます。

HTTP 要求を次に示します。 (使用して[Fiddler](http://www.telerik.com/fiddler) HTTP トラフィックをキャプチャします)。

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

HTTP 応答:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

ベアラーに設定するという課題に Www 認証ヘッダーが応答が含まれることに注意してください。 サーバーは、ベアラー トークンを示すです。

## <a name="register-a-user"></a>ユーザーを登録します。

**登録**セクションで、アプリの電子メールとパスワードを入力し、クリックして、**登録**ボタンをクリックします。

このサンプルでは、有効な電子メール アドレスを使用する必要はありませんが、実際のアプリは、アドレスを確認します。 (を参照してください[ログイン、電子メールの確認とパスワードのリセットをセキュリティで保護された ASP.NET MVC 5 web アプリを作成](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md))。パスワードを大文字、小文字、番号、および英数字以外の文字を"Password1!"、ようなものを使用します。 クライアント側の検証を残しましたをアプリをシンプルにするため、パスワードの形式に問題がある場合は、400 (Bad Request) のエラーが表示されます。

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

**登録**ボタン ~/api/Account/Register に POST 要求を送信する/。 要求本文は、ユーザー名とパスワードを保持する JSON オブジェクトです。 要求を送信する JavaScript コードを次に示します。

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

HTTP 要求:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

HTTP 応答:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

この要求が処理、`AccountController`クラス。 内部的には、 `AccountController` ASP.NET Identity を使用して、メンバーシップ データベースを管理します。

Visual Studio からアプリをローカルで実行する場合、ユーザー アカウントが AspNetUsers テーブルに、LocalDB に格納されます。 Visual Studio でのテーブルを表示する] をクリックして、**ビュー**メニューの [**サーバー エクスプ ローラー**の順に展開**データ接続**します。

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>アクセス トークンを取得します。

これまでに実行していない任意の OAuth でも、これで今回はアクションで、OAuth 承認サーバー アクセス トークンを要求するとします。 **ログで**サンプル アプリでの領域は電子メール アドレスとパスワードを入力しをクリックして**ログで**します。

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

**ログで**ボタンは、トークン エンドポイントに要求を送信します。 要求の本文には、次の形式で url でエンコードされたデータが含まれています。

- grant\_type: "password"
- ユーザー名:&lt;ユーザーの電子メール アドレス&gt;
- パスワード:&lt;パスワード&gt;

AJAX 要求を送信する JavaScript コードを次に示します。

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

要求が成功すると、承認サーバーは応答本文でアクセス トークンを返します。 API に要求を送信するときに後で使用する、セッションの記憶域にトークンを保存したことに注意してください。 (Cookie ベースの認証の場合) などの認証の一部のフォームとは異なり、ブラウザーは自動的に含まれないアクセス トークン後続の要求で。 アプリケーションする必要があります明示的に行います。 制限するためには良いことだいる[CSRF の脆弱性](preventing-cross-site-request-forgery-csrf-attacks.md)します。

HTTP 要求:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

ユーザーの資格情報が要求に含まれることを確認できます。 *する必要があります*トランスポート層セキュリティを提供する HTTPS を使用します。

HTTP 応答:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

読みやすくするためは、JSON をインデントされ、これは非常に長くアクセス トークンが切り捨てられます。

`access_token`、 `token_type`、および`expires_in`プロパティは、OAuth2 仕様によって定義されます。その他のプロパティ (`userName`、 `.issued`、および`.expires`) は情報提供を目的用だけです。 これらのプロパティが追加されているコードを検索することができます、 `TokenEndpoint` /Providers/ApplicationOAuthProvider.cs ファイル内のメソッド。

## <a name="send-an-authenticated-request"></a>認証された要求を送信します。

ベアラー トークンをしたら、API に認証された要求を実行できます。 これは、要求に Authorization ヘッダーを設定して行います。 をクリックして、 **API の呼び出し**ボタンをもう一度参照してください。

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

HTTP 要求:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

HTTP 応答:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>ログアウトします。

ブラウザーでは、資格情報またはアクセス トークンをキャッシュしません、ためには、単にセッションの記憶域から削除することで、トークンを「忘れる」のログアウトです。

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>個々 のアカウントのプロジェクト テンプレートを理解します。

選択すると**個々 のアカウント**ASP.NET Web アプリケーション プロジェクト テンプレートで、プロジェクトが含まれます。

- OAuth2 承認サーバーの場合は。
- ユーザー アカウントを管理するための Web API エンドポイント
- ユーザー アカウントを格納するための EF モデル。

これらの機能を実装するアプリケーションのメイン クラスを次に示します。

- `AccountController`。 ユーザー アカウントを管理するためには、Web API エンドポイントを提供します。 `Register`アクションは、このチュートリアルで使用した 1 つだけです。 クラスの他のメソッドは、パスワードのリセット、ソーシャル ログイン、およびその他の機能をサポートします。
- `ApplicationUser`、/Models/IdentityModels.cs で定義されています。 このクラスは、メンバーシップ データベース内のユーザー アカウントの EF モデルです。
- `ApplicationUserManager`、/App で定義されている\_からこのクラスの派生 Start/IdentityConfig.cs [UserManager](https://msdn.microsoft.com/library/dn613290.aspx)パスワード、およびなどを確認する、新しいユーザーの作成などのユーザー アカウントに対して操作を実行し、自動的に保持データベースに変更します。
- `ApplicationOAuthProvider`。 このオブジェクトは、OWIN ミドルウェアに差し込むし、ミドルウェアによって発生したイベントを処理します。 派生した[OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx)します。

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>承認サーバーを構成します。

StartupAuth.cs では、次のコードは、OAuth2 承認サーバーを構成します。

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

`TokenEndpointPath`プロパティは、承認サーバーのエンドポイントへの URL パス。 URL はそのアプリを使用してベアラー トークンを取得します。

`Provider`プロパティは、OWIN ミドルウェアに接続して、ミドルウェアによって生成されるイベントを処理するプロバイダーを指定します。

アプリがトークンを取得するときに次の基本的な流れに示します。

1. アプリが要求を送信するアクセス トークンを取得するには、~/トークンです。
2. OAuth のミドルウェア呼び出し`GrantResourceOwnerCredentials`プロバイダー。
3. プロバイダーの呼び出し、`ApplicationUserManager`資格情報を検証し、要求 id を作成します。
4. 成功すると、プロバイダーは、トークンの生成に使用される認証チケットを作成します。

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

OAuth のミドルウェアのユーザー アカウントについて何も認識します。 プロバイダーは、ミドルウェア、ASP.NET Identity と通信します。 承認サーバーの実装の詳細については、次を参照してください。 [OWIN OAuth 2.0 承認サーバー](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md)します。

### <a name="configuring-web-api-to-use-bearer-tokens"></a>ベアラー トークンを使用する Web API を構成します。

`WebApiConfig.Register`メソッドでは、次のコードは、Web API パイプラインの認証を設定します。

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

**HostAuthenticationFilter**クラスは、ベアラー トークンを使用して認証を使用できます。

**しないように SuppressDefaultHostAuthentication**メソッドが Web API、Web API パイプラインは、IIS または OWIN ミドルウェアのいずれかに到達する前に発生するすべての認証を無視するように指示します。 これにより、Web API のみベアラー トークンを使用して認証を制限できることです。

> [!NOTE]
> 具体的には、アプリの MVC 部分は、cookie に資格情報を格納するフォーム認証を使用する可能性があります。 Cookie ベースの認証には、CSRF 攻撃を防ぐため、偽造防止トークンの使用が必要です。 Web Api、問題の 1、偽造防止トークンをクライアントに送信する web API の便利な方法がないためにです。 (この問題の詳細については、次を参照してください[Web API での CSRF 攻撃の防止](preventing-cross-site-request-forgery-csrf-attacks.md)。)。呼び出す**しないように SuppressDefaultHostAuthentication** Web API が cookie に格納されている資格情報から CSRF 攻撃に対して脆弱でないことを確認します。


クライアントが保護されたリソースを要求すると次 Web API パイプラインでの動作に示します。

1. **HostAuthentication**フィルターは、トークンを検証する OAuth ミドルウェアを呼び出します。
2. ミドルウェアは、トークンを要求の id に変換します。
3. 要求は、この時点では、*認証*なく*承認*します。
4. 承認フィルターは、要求の id を検証します。 クレームは、そのリソースに対してユーザーを承認する場合、要求を承認します。 既定で、 **[Authorize]** 属性では、認証されたすべての要求を承認します。 ただし、ロールまたはその他の要求を承認できます。 詳細については、次を参照してください。 [Web API で認証と承認](authentication-and-authorization-in-aspnet-web-api.md)します。
5. 前の手順が成功すると、コント ローラーは、保護されたリソースを返します。 それ以外の場合、クライアントは、401 (未承認) のエラーを受け取ります。

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>その他のリソース

- [ASP.NET Identity](../../../identity/index.md)
- [VS2013 RC の SPA テンプレートのセキュリティ機能の理解](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)します。 MSDN ブログでは、Sun Hongye を投稿します。
- [詳細に分析する Web API の個人アカウントのテンプレート – パート 2: ローカル アカウント](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/)します。 Dominick Baier によるブログ投稿です。
- [ホスト認証と Web API の owin 対応](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/)します。 適切な説明について`SuppressDefaultHostAuthentication`と`HostAuthenticationFilter`Brock Allen でします。
- [テンプレートの VS 2013 で ASP.NET Identity でのプロファイル情報のカスタマイズ](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)します。 Pranav Rastogi による MSDN ブログの投稿です。
- [ASP.NET Identity で UserManager クラスの要求の有効期間管理あたり](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx)します。 適切な説明を添えて Suhas Joshi での MSDN ブログの投稿、`UserManager`クラス。
