---
uid: web-api/overview/security/individual-accounts-in-web-api
title: "個々 のアカウントと ASP.NET Web API 2.2 でローカルのログインを使用して Web API をセキュリティで保護された |Microsoft ドキュメント"
author: MikeWasson
description: "このトピックでは、メンバーシップ データベースに対する認証に OAuth2 を使用して web API をセキュリティで保護する方法を示します。 ソフトウェアのバージョンが、チュートリアルの Visual Studio 201 で使用しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e2056e769edf972cba830b31cf37f6418148ca73
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>個々 のアカウントと ASP.NET Web API 2.2 でローカルのログインを使用して Web API をセキュリティで保護します。
====================
によって[Mike Wasson](https://github.com/MikeWasson)

[サンプル アプリをダウンロードします。](https://github.com/MikeWasson/LocalAccountsApp)

> このトピックでは、メンバーシップ データベースに対する認証に OAuth2 を使用して web API をセキュリティで保護する方法を示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Web API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


Visual Studio 2013 で、Web API プロジェクト テンプレートを使用して認証用の 3 つのオプション。

- **個々 のアカウントです。** アプリでは、メンバーシップ データベースを使用します。
- **組織のアカウントです。** ユーザーは、その Azure Active Directory、Office 365、または内部設置型 Active Directory の資格情報でサインインします。
- **Windows 認証です。** このオプションは、イントラネット アプリケーションのためのものでは、し、Windows 認証の IIS モジュールを使用します。

これらのオプションの詳細については、次を参照してください。 [Visual Studio 2013 で ASP.NET Web プロジェクトの作成](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth)です。

個々 のアカウントは、ユーザーがログインするための 2 つの方法を指定します。

- **ローカル ログイン**です。 ユーザー登録、サイトでユーザー名とパスワードを入力します。 アプリでは、メンバーシップ データベースのパスワード ハッシュを保存します。 ユーザーがログインすると、ASP.NET Identity システムは、パスワードを確認します。
- **ソーシャル ログイン**です。 ユーザーは、Facebook、Microsoft、Google などの外部サービスを使用してサインインします。 アプリでは、メンバーシップ データベース内のユーザー エントリを作成できますが、すべての資格情報は格納されません。 ユーザーは、外部のサービスにサインインして認証します。

この記事では、ローカル ログイン シナリオ、します。 ローカルおよびソーシャルの両方のログインには、Web API は、要求の認証に OAuth2 を使用します。 ただし、資格情報のフローは、ローカルおよびソーシャル ログインに異なります。

この記事で、ユーザーがログインし、web API への認証済みの AJAX 呼び出しを送信できる簡単なアプリを紹介します。 サンプル コードをダウンロードする[ここ](https://github.com/MikeWasson/LocalAccountsApp)です。 リリース ノートでは、Visual Studio での最初からサンプルを作成する方法について説明します。

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

サンプル アプリは、データ バインディング、および jQuery AJAX 要求を送信するための Knockout.js を使用します。 すればを重視して AJAX 呼び出しをこのアーティクルの Knockout.js を知る必要はありません。

その過程では、説明します。

- どのようなアプリケーションのクライアント側で実行します。
- サーバーで処理します。
- 中央の HTTP トラフィック。

最初に、いくつかの OAuth2 用語を定義する必要があります。

- *リソース*です。 一部のデータを保護することができます。
- *リソース サーバー*です。 リソースをホストするサーバー。
- *リソース所有者*です。 リソースにアクセスする権限を与えることのできるエンティティです。 (通常はユーザーです。)
- *クライアント*: リソースへのアクセスを必要があるアプリ。 この記事では、クライアントは、web ブラウザーがします。
- *アクセス トークン*です。 リソースへのアクセスを付与するトークンです。
- *ベアラー トークン*です。 特定の種類のすべてのユーザー トークンを使用できることプロパティを使用して、アクセス トークン。 つまり、クライアントは、暗号化キーまたはその他のシークレット ベアラー トークンを使用する必要はありません。 そのため、ベアラー トークンは、HTTPS 経由でのみ使用する必要があり、比較的短い形式の有効期限の日時を持つ必要があります。
- *承認サーバー*です。 アクセス トークンを提供するサーバー。

アプリケーションは、承認サーバーとリソース サーバーの両方として機能できます。 Web API プロジェクト テンプレートでは、このパターンに従います。

## <a name="local-login-credential-flow"></a>ローカルのログイン資格情報のフロー

ローカル ログインは、Web API を使用して、[リソース所有者のパスワード フロー](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) OAuth2 で定義されています。

1. ユーザーは、クライアント名とパスワードを入力します。
2. クライアントは、承認サーバーにこれらの資格情報を送信します。
3. 承認サーバーは、資格情報を認証し、アクセス トークンを返します。
4. クライアントには保護されたリソースにアクセスするには、HTTP 要求の承認ヘッダーにアクセス トークンが含まれています。

![](individual-accounts-in-web-api/_static/image3.png)

選択すると**個々 のアカウント**Web API プロジェクト テンプレートでプロジェクトには、ユーザーの資格情報を検証し、トークンを発行する承認サーバーが含まれます。 次の図は、Web API のコンポーネントの観点から、同じ資格情報フローを示しています。

![](individual-accounts-in-web-api/_static/image4.png)

このシナリオでは、Web API コント ローラーは、リソース サーバーとして機能します。 認証フィルターがアクセス トークンを検証し、 **[Authorize]**属性を使用してリソースを保護します。 コント ローラーまたはアクションが持っている場合、 **[Authorize]**属性がすべての要求にそのコント ローラーまたはアクションを認証する必要があります。 それ以外の場合、承認が拒否され、Web API には、401 (未承認) のエラーが返されます。

承認サーバーと両方呼び出せる認証フィルター、 [OWIN ミドルウェア](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)OAuth2 の詳細を処理するコンポーネントです。 このチュートリアルで後ほど詳しくデザインを説明します。

## <a name="sending-an-unauthorized-request"></a>承認されていない要求を送信します。

最初に、アプリケーションを実行し、をクリックして、 **API を呼び出す**ボタンをクリックします。 要求が完了したらのエラー メッセージが表示されます、**結果**ボックス。 要求がないため、アクセス トークン要求が承認されていないためにです。

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

**API を呼び出す**ボタンに AJAX 要求を送信 ~/api/値、Web API コント ローラーのアクションを呼び出す。 AJAX 要求を送信する JavaScript コードのセクションを次に示します。 サンプル アプリですべてのアプリの JavaScript コードは Scripts\app.js ファイルにあります。

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

ユーザーがログインするまではないベアラー トークンと要求の承認ヘッダーしたがってがありません。 これは、ため、401 エラーを返す要求します。

HTTP 要求を次に示します。 (を使用して[Fiddler](http://www.telerik.com/fiddler) HTTP トラフィックをキャプチャします)。

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

HTTP 応答:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

ベアラーに設定するという課題に Www 認証ヘッダーが応答が含まれていることを確認します。 サーバーにベアラー トークンが必要ですを示すです。

## <a name="register-a-user"></a>ユーザーを登録します。

**登録**セクションで、アプリの電子メールとパスワードを入力し、をクリックして、**登録**ボタンをクリックします。

このサンプルでは、有効な電子メール アドレスを使用する必要はありませんが、実際のアプリは、アドレスを確認します。 (を参照してください[ログイン、電子メールの確認とパスワードのリセットをセキュリティで保護された ASP.NET MVC 5 web アプリケーションを作成](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md))。パスワードの大文字、小文字アルファベット、番号、および英数字以外の文字と"Password1!"のようなものを使用します。 アプリをシンプルにするには、クライアント側の検証を残しましたため、パスワードの形式に問題がある場合は、400 (Bad Request) のエラーが表示されます。

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

**登録**ボタン ~/api/Account/Register に POST 要求を送信する/です。 要求本文は、ユーザー名とパスワードを保持する JSON オブジェクトです。 要求を送信する JavaScript コードを次に示します。

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

HTTP 要求。

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

HTTP 応答:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

この要求を処理、`AccountController`クラスです。 内部的には、 `AccountController` ASP.NET の Id を使用して、メンバーシップ データベースを管理します。

Visual Studio からアプリをローカルで実行する場合、ユーザー アカウントが AspNetUsers テーブルで LocalDB に格納されます。 テーブルを表示する、Visual Studio で、をクリックして、**ビュー**メニューの **サーバー エクスプ ローラー**の順に展開**データ接続**です。

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>アクセス トークンを取得します。

これまでは実行していない、OAuth が、今すぐお見せ、OAuth 承認サーバーの動作、アクセス トークンを要求時にします。 **ログで**サンプル アプリの領域が電子メール アドレスとパスワードを入力し、クリックして**ログで**です。

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

**ログで**ボタンは、トークン エンドポイントに要求を送信します。 要求の本文には、次の形式で url でエンコードされたデータが含まれています。

- grant\_type: "password"
- ユーザー名:&lt;ユーザーの電子メール&gt;
- password: &lt;password&gt;

AJAX 要求を送信する JavaScript コードを次に示します。

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

要求が成功すると、承認サーバーは、応答本文にアクセス トークンを返します。 API に要求を送信するときに後で使用する、セッションの記憶域にトークンを格納することを確認します。 (Cookie ベースの認証など) の認証の一部のフォームとは異なりブラウザーは自動的にアクセス トークンに含めません後続の要求。 アプリケーションする必要があります明示的に行います。 制限されるため、良いことをある[CSRF 脆弱性](preventing-cross-site-request-forgery-csrf-attacks.md)です。

HTTP 要求。

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

要求にユーザーの資格情報が含まれることを確認できます。 *必要があります*HTTPS を使用して、トランスポート層セキュリティを提供します。

HTTP 応答:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

読みやすさは、JSON をインデントし、これはかなり長いはアクセス トークンを切り捨てられます。

`access_token`、 `token_type`、および`expires_in`プロパティは、OAuth2 仕様によって定義されます。その他のプロパティ (`userName`、 `.issued`、および`.expires`) は情報提供を目的用だけです。 これらのプロパティを追加するコードを検索することができます、 `TokenEndpoint` /Providers/ApplicationOAuthProvider.cs ファイル内のメソッドです。

## <a name="send-an-authenticated-request"></a>認証された要求を送信します。

ベアラー トークンである、API に対して認証された要求を行うおことができます。 これは、要求で Authorization ヘッダーを設定します。 クリックして、 **API を呼び出す**ボタンをもう一度このを参照してください。

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

HTTP 要求。

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

HTTP 応答:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>ログアウトします。

ブラウザーでは、資格情報またはアクセス トークンをキャッシュしません、ためには、セッションの記憶域から削除して、トークンを「忘れた」だけログ記録です。

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>個々 のアカウントのプロジェクト テンプレートを理解します。

選択すると**個々 のアカウント**ASP.NET Web アプリケーション プロジェクト テンプレートで、プロジェクトが含まれます。

- OAuth2 承認サーバーの場合は。
- ユーザー アカウントを管理するための Web API エンドポイント
- ユーザー アカウントを格納するための EF モデルです。

次に、これらの機能を実装するアプリケーションのメイン クラスを示します。

- `AccountController`。 ユーザー アカウントを管理するためには、Web API エンドポイントを提供します。 `Register`アクションは、このチュートリアルで使用した 1 つだけです。 クラスの他のメソッドは、パスワードのリセット、ソーシャル ログイン、およびその他の機能をサポートします。
- `ApplicationUser`、/Models/IdentityModels.cs で定義されています。 このクラスは、メンバーシップ データベース内のユーザー アカウントの EF モデルです。
- `ApplicationUserManager`、/App で定義されている\_このクラスから派生 Start/IdentityConfig.cs [UserManager](https://msdn.microsoft.com/library/dn613290.aspx)パスワード、およびなどを確認する、新しいユーザーを作成するなどのユーザー アカウントの操作を実行し、自動的に保持し、データベースに変更します。
- `ApplicationOAuthProvider`。 このオブジェクトは、OWIN ミドルウェアに接続し、ミドルウェアによって生成されるイベントを処理します。 派生して[OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx)です。

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>承認サーバーを構成します。

StartupAuth.cs では、次のコードは、OAuth2 承認サーバーを構成します。

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

`TokenEndpointPath`プロパティは、承認サーバーのエンドポイントへの URL パス。 URL はそのアプリを使用して、ベアラー トークンを取得します。

`Provider`プロパティは、OWIN ミドルウェアに接続し、ミドルウェアによって生成されるイベントを処理するプロバイダーを指定します。

アプリがトークンを取得するときに、基本的な流れを次に示します。

1. アプリが要求を送信するアクセス トークンを取得するには、~/トークンです。
2. OAuth ミドルウェア呼び出し`GrantResourceOwnerCredentials`プロバイダー。
3. プロバイダーの呼び出し、`ApplicationUserManager`を資格情報を検証し、要求 id を作成します。
4. 成功すると、プロバイダーは、トークンの生成に使用される認証チケットを作成します。

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

OAuth ミドルウェアは、ユーザー アカウントの設定を認識しません。 プロバイダーは、ミドルウェアと ASP.NET Identity の間で通信します。 承認サーバーの実装の詳細については、次を参照してください。 [OWIN OAuth 2.0 承認サーバー](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md)です。

### <a name="configuring-web-api-to-use-bearer-tokens"></a>ベアラー トークンを使用する Web API を構成します。

`WebApiConfig.Register`メソッド、次のコードは、Web API パイプラインの認証を設定します。

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

**HostAuthenticationFilter**クラス ベアラー トークンを使用して認証を有効にします。

**SuppressDefaultHostAuthentication**メソッドが Web API、Web API パイプラインは、IIS または OWIN ミドルウェアのいずれかに到達する前に発生する任意の認証を無視するように指示します。 こうすれば、のみベアラー トークンを使用して認証を Web API を限定できます。

> [!NOTE]
> 具体的には、アプリの MVC 部分は、資格情報を cookie に格納フォーム認証を使用する可能性があります。 Cookie ベースの認証には、CSRF 攻撃を防ぐため、偽造防止トークンの使用が必要です。 偽造防止トークンをクライアントに送信する web API の便利な方法がないために、web Api では、問題があります。 (この問題の詳細については、次を参照してください[CSRF は Web API の攻撃の防止](preventing-cross-site-request-forgery-csrf-attacks.md)。)。呼び出す**SuppressDefaultHostAuthentication**で Web API が cookie に格納されている資格情報から CSRF 攻撃に対して脆弱なことはありません。


クライアントは、保護されたリソースを要求するときに次に、Web API パイプラインでの動作を示します。

1. **HostAuthentication**フィルターは、トークンを検証する OAuth ミドルウェアを呼び出します。
2. ミドルウェアは、トークンを要求の id に変換します。
3. この時点では、要求が*認証*ではなく*承認*です。
4. 承認フィルターは、要求の id を検査します。 クレームは、そのリソースのユーザーを承認する要求が承認されています。 既定では、 **[Authorize]**属性は、認証されているすべての要求が承認されます。 ただし、ロールまたはその他の要求を承認できます。 詳細については、次を参照してください。 [Web API で認証と承認](authentication-and-authorization-in-aspnet-web-api.md)です。
5. 前の手順が成功した場合は、コント ローラーは、保護されたリソースを返します。 それ以外の場合、クライアントは、401 (未承認) のエラーを受け取ります。

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>その他のリソース

- [ASP.NET Identity](../../../identity/index.md)
- [VS2013 RC の SPA テンプレートのセキュリティ機能の理解](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)です。 MSDN のブログでは、Sun Hongye で投稿します。
- [アカウント、Web API の個々 の分解をテンプレート – パート 2: ローカル アカウント](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/)です。 ブログ投稿 Dominick Baier でします。
- [ホストの認証および Web API OWIN の](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/)します。 説明については、`SuppressDefaultHostAuthentication`と`HostAuthenticationFilter`Brock Allen でします。
- [VS 2013 テンプレートの ASP.NET Identity のプロファイル情報をカスタマイズする](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)です。 Pranav Rastogi が MSDN ブログの投稿。
- [UserManager クラスは、ASP.NET Identity の要求の有効期間管理あたり](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx)します。 適切な説明 Suhas Joshi で MSDN ブログの投稿、`UserManager`クラスです。
