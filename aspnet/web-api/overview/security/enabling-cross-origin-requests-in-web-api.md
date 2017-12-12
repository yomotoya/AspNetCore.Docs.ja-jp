---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: "ASP.NET Web API 2 でのクロス オリジン要求の有効化 |Microsoft ドキュメント"
author: MikeWasson
description: "ASP.NET Web API でクロス オリジン リソース共有 (CORS) をサポートする方法を示します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>ASP.NET Web API 2 でのクロス オリジン要求を有効にします。
====================
によって[Mike Wasson](https://github.com/MikeWasson)

> ブラウザーのセキュリティは、web ページが別のドメインに AJAX 要求を行うことを防止します。 この制限が呼び出された、*同一生成元ポリシー*、悪意のあるサイトが別のサイトから機密データを読み取ることを防ぎます。 ただし、場合もあります可能性がある、web API を呼び出す他のサイトを使用できます。
> 
> [クロス オリジン リソース共有](http://www.w3.org/TR/cors/)(CORS) は、W3C 標準により、同じオリジンのポリシーを緩和するサーバーです。 CORS を使用して、サーバー明示的に許可できますいくつかのクロス オリジン要求中に、他のユーザーを拒否します。 CORS などがより安全なと以前の手法より柔軟な[JSONP](http://en.wikipedia.org/wiki/JSONP)です。 このチュートリアルでは、Web API アプリケーションで CORS を有効にする方法を示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013 Update 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2.2


<a id="intro"></a>
## <a name="introduction"></a>はじめに

このチュートリアルでは、ASP.NET Web API で CORS のサポートについて説明します。 まず、– 1 つと呼ばれる"WebService"、Web API コント ローラーをホストして、その他の呼び出された"WebClient"、web サービスを呼び出すの 2 つの ASP.NET プロジェクトを作成します。 別のドメインでは、2 つのアプリケーションがホストされている、ため WebClient から web サービスへの AJAX 要求は、クロス オリジン要求です。

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>「同じ発生元」とは何ですか。

2 つの Url では、同じスキーム、ホスト、およびポートがある場合の原点が同じがあります。 ([RFC 6454](http://tools.ietf.org/html/rfc6454))

これら 2 つの Url は、原点が同じであります。

- `http://example.com/foo.html`
- `http://example.com/bar.html`

これらの Url は、2 つよりも前の別の原点をあります。

- `http://example.net`-別のドメイン
- `http://example.com:9000/foo.html`-別のポート
- `https://example.com/foo.html`-別のスキーム
- `http://www.example.com/foo.html`-別のサブドメイン

> [!NOTE]
> Internet Explorer では、元のドメインを比較するときに、ポートは考慮されません。


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>Web サービス プロジェクトを作成します。

> [!NOTE]
> このセクションでは、Web API プロジェクトを作成する方法を既に知っているものとします。 いない場合を参照してください。 [ASP.NET Web API の使用を開始する](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)です。


Visual Studio を起動し、新しい作成**ASP.NET Web アプリケーション**プロジェクト。 選択、**空**プロジェクト テンプレート。 「フォルダーの追加し、コアの参照」を選択、 **Web API**チェック ボックスをオンします。 必要に応じて、アプリを Azure にデプロイ Mircosoft「クラウドのホスト」オプションを選択します。 マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、[無料試用版の Azure アカウント](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)です。

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

という名前の Web API コント ローラーを追加`TestController`を次のコード。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

アプリケーションをローカルで実行したり、Azure にデプロイできます。 (このチュートリアルでは、スクリーン ショットを展開した Azure App Service Web Apps にします。)Web API が動作していることを確認するに移動`http://hostname/api/test/`ここで、 *hostname*アプリケーションの展開先のドメインです。 応答テキストが表示されます&quot;を取得します。 テスト メッセージ&quot;です。

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>WebClient プロジェクトを作成します。

ASP.NET Web アプリケーションの別のプロジェクトを作成し、選択、 **MVC**プロジェクト テンプレート。 必要に応じて、選択**認証の変更** > **認証なし**です。 このチュートリアルでは、認証が不要です。

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

ソリューション エクスプ ローラーで、Views/Home/Index.cshtml ファイルを開きます。 次のようにこのファイル内のコードに置き換えます。

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

*ServiceUrl*変数、web サービス アプリケーションの URI を使用します。 今すぐ WebClient アプリをローカルで実行または別の web サイトに発行します。

表示されている HTTP メソッドを使用して、web サービス アプリケーションに AJAX 要求を送信するボタンをクリックして"再試行"(GET、POST または PUT) ボックスの一覧です。 これにより、別のクロス オリジン要求をチェックします。 現在、web サービス アプリケーションは、CORS をサポートしていませんエラーが発生する場合は、ボタンをクリックするようにします。

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> ツールで、HTTP トラフィックを監視する場合と同様に[Fiddler](http://www.telerik.com/fiddler)ブラウザーは、GET 要求を送信し、要求が成功するしますが、AJAX 呼び出しがエラーを返しますが表示されます。 同一生成元ポリシーがからブラウザーを禁止していないことを理解することが重要*送信*要求します。 代わりに、アプリケーションが表示されるを防止、*応答*です。


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>CORS を有効にします。

今すぐ WebService アプリで CORS が有効にしてみましょう。 最初に、CORS の NuGet パッケージを追加します。 Visual Studio から、**ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

このコマンドは、最新のパッケージをインストールし、コア Web API ライブラリを含むすべての依存関係を更新します。 ユーザーを特定のバージョンを対象とするバージョン フラグ。 CORS パッケージには、Web API 2.0 以降が必要です。

アプリのファイルを開く\_Start/WebApiConfig.cs です。 次のコードを追加、 **WebApiConfig.Register**メソッドです。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

次に、追加、 **[EnableCors]**属性を`TestController`クラス。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

*オリジン*パラメーター、WebClient アプリケーションを配置した URI を使用します。 これにより、クロス オリジン要求から WebClient、まだ他のすべてのクロス ドメイン要求を許可中にします。 パラメーターを後で説明します**[EnableCors]**で詳しく説明します。

末尾にスラッシュを含めないでください、*オリジン*URL。

更新された web サービス アプリケーションを再展開します。 WebClient を更新する必要はありません。 WebClient から AJAX 要求が成功するようになりました。 GET、PUT、POST メソッドはすべて許可します。

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>CORS のしくみ

このセクションでは、HTTP メッセージのレベルでの CORS 要求での動作について説明します。 構成できるようにするために、CORS のしくみを理解することが重要、 **[EnableCors]**正しく、属性し、期待どおりに機能しない場合のトラブルシューティングを行います。

CORS の仕様には、クロス オリジン要求を有効にするいくつかの新しい HTTP ヘッダーが導入されています。 ブラウザーでは、CORS をサポートする場合、クロス オリジン要求を自動的にこれらのヘッダーを設定します。JavaScript コードで特別な何もする必要はありません。

クロス オリジン要求の例を次に示します。 "Origin"ヘッダーは、要求を行っているサイトのドメインを示します。

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

サーバーは、要求を許可している場合は、アクセス コントロール-を許可する-オリジン ヘッダーを設定します。 このヘッダーの値は、Origin ヘッダーと一致するか、ワイルドカード文字は、"\*"、すべてのオリジンを許可されていることを意味します。

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

応答に、アクセス コントロール-を許可する-オリジン ヘッダーが含まれていない場合、AJAX 要求は失敗します。 具体的には、ブラウザーには、要求が許可されていません。 サーバーでは、正常な応答を返す、場合でも、ブラウザーは行いません応答クライアント アプリケーションで使用できます。

**プレフライト要求**

いくつかの CORS 要求については、ブラウザーは、リソースの実際の要求を送信する前に「プレフライト要求を」と呼ばれる、追加の要求を送信します。

ブラウザーは、次の条件に該当する場合、プレフライト要求を省略できます。

- 要求メソッドが GET、HEAD、または POST、*と*
- アプリケーションは Accept、Accept-language、Content-language 以外の任意の要求ヘッダーを設定していないコンテンツの種類、または最後のイベント ID、*と*
- Content-type ヘッダー (場合に設定) は、次のいずれか。 

    - application/x-www-form-urlencoded
    - マルチパート フォーム データ
    - テキスト/プレーン

アプリケーションで呼び出すことによって設定されたヘッダーに要求ヘッダーについて、規則が適用されます**setRequestHeader**上、 **XMLHttpRequest**オブジェクト。 (CORS の仕様は、これら「作成者要求ヘッダー」を呼び出します)。このルールは、ヘッダーには適用されません、*ブラウザー*ユーザー エージェント、ホスト、またはコンテンツの長さなど、設定できます。

プレフライト要求の例を次に示します。

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

事前要求は、HTTP OPTIONS メソッドを使用します。 2 つの特殊なヘッダーが含まれています。

- アクセス コントロール-要求メソッド: 実際の要求に使用される HTTP メソッド。
- アクセス コントロール-要求ヘッダー。 要求ヘッダーの一覧を、*アプリケーション*実際の要求に設定します。 (ここでも、これは含まれません、ブラウザーを設定するヘッダー。)

次に、応答の例、サーバーで要求を許可すると仮定した場合を示します。

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

応答には、許可されているメソッドを一覧表示するアクセスの制御の許可する-メソッド ヘッダーおよび必要に応じて、アクセス コントロール-を許可する-ヘッダー ヘッダー、許可されているヘッダーの一覧が含まれています。 プレフライト要求が成功した場合、ブラウザーは、前述のとおり、実際の要求を送信します。

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>[EnableCors] の規則のスコープ

アプリケーションでは、アクション、コント ローラーごとまたはグローバルのすべての Web API コント ローラーあたり CORS を有効にできます。

**アクションごと**

1 つのアクションで CORS を有効にする設定、 **[EnableCors]**アクション メソッドの属性です。 次の例の CORS の有効、`GetItem`メソッドのみです。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**コント ローラーあたり**

設定した場合**[EnableCors]**コント ローラー クラスに、コント ローラーのすべてのアクションに適用します。 アクションの CORS を無効にする追加の**[DisableCors]**属性をアクションにします。 次の例では、CORS を有効を除くすべてのメソッドに対する`PutItem`です。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**グローバル**

アプリケーション内のすべての Web API コント ローラーで CORS を有効にするのには、渡す、 **EnableCorsAttribute**インスタンスを**EnableCors**メソッド。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

優先順位の順序は、1 つ以上のスコープ内で属性を設定する場合です。

1. 操作
2. コントローラー
3. Global

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>許可されるオリジンを設定します。

*オリジン*のパラメーター、 **[EnableCors]**属性は、どのオリジンはリソースへのアクセス許可を指定します。 値は、許可されるオリジンのコンマ区切りの一覧です。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

ワイルドカード文字を使用することもできます。"\*"すべてのオリジンから要求を許可します。

すべてのオリジンからの要求を許可する前に慎重に検討してください。 これは、事実上あらゆる web サイトが、web API への AJAX 呼び出しを実行できることを意味します。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>許可される HTTP メソッドを設定します。

*メソッド*のパラメーター、 **[EnableCors]**属性は、どの HTTP メソッドは、リソースへのアクセス許可を指定します。 すべてのメソッドを許可するワイルドカード値を使用"\*"です。 次の例では、GET および POST 要求だけを許可します。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>許可されている要求ヘッダーを設定します。

既に説明しましたプレフライト要求が、アクセス コントロール-要求ヘッダー ヘッダーが含まれて、アプリケーションによって設定される HTTP ヘッダーの一覧を表示する (いわゆる"要求ヘッダーを author") です。 *ヘッダー*のパラメーター、 **[EnableCors]**属性を指定する作成者要求ヘッダーが許可されます。 すべてのヘッダーは、次のように設定します。*ヘッダー*に"\*"です。 ホワイト リストの特定のヘッダーを次のように設定します。*ヘッダー*許可されたヘッダーのコンマ区切りの一覧にします。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

ただし、ブラウザーでは、アクセス コントロール-要求ヘッダーを設定する方法の完全一貫性がありません。 たとえば、Chrome 現在含まれています"origin"です。FireFox では、スクリプトでアプリケーションを設定する場合でも、"Accept"などの標準ヘッダーは含まれません中に。

設定した場合*ヘッダー*以外の値を"\*"、する必要がありますを含めるには、少なくとも「受け入れる」、「コンテンツの種類」と「発生元」、およびサポートする任意のカスタム ヘッダー。

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>許可されている応答ヘッダーを設定します。

既定では、ブラウザーは公開しませんすべてのアプリケーションに応答ヘッダー。 既定で利用できる応答ヘッダーは次のとおりです。

- キャッシュ制御
- コンテンツの言語
- コンテンツの種類
- 有効期限が切れる
- 最終更新
- プラグマ

CORS の仕様を呼び出す[単純な応答ヘッダー](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header)です。 その他のヘッダーをアプリケーションで使用できるようにする設定、 *exposedHeaders*のパラメーター **[EnableCors]**です。

次の例で、コント ローラーの`Get`メソッドが 'X カスタム ヘッダー' という名前のカスタム ヘッダーを設定します。 既定では、ブラウザーでクロス オリジン要求では、このヘッダーは公開されません。 ヘッダーを使用できるようにするに 'X カスタム ヘッダー' を含める*exposedHeaders*です。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>クロス オリジン要求に資格情報を渡す

資格情報では、CORS 要求で特別な処理が必要です。 既定では、ブラウザーは、クロス オリジン要求に資格情報を送信しません。 Cookie と、HTTP 認証スキームの資格情報が含まれます。 クロス オリジン要求に資格情報を送信するクライアントを設定する必要があります**XMLHttpRequest.withCredentials** true に設定します。

使用して**XMLHttpRequest**直接。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

JQuery: で

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

さらに、サーバーは、資格情報を許可する必要があります。 Web API のクロス オリジンの資格情報を許可する設定、 **SupportsCredentials**プロパティは true を**[EnableCors]**属性。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

このプロパティが true の場合、HTTP 応答ヘッダーには、アクセス コントロール-を許可する-資格情報が含まれます。 このヘッダーは、クロス オリジン要求の資格情報を許可することをブラウザーに指示します。

ブラウザーが資格情報を送信しますが、応答は有効なアクセス制御を許可する-資格情報ヘッダーを含まない、ブラウザーは、アプリケーションへの応答を公開しないと、AJAX 要求が失敗します。

設定するには十分に注意**SupportsCredentials**を true にため、別のドメインで web サイトはユーザーに気付かれることがなく、ユーザーの代理で Web API のログイン ユーザーの資格情報を送信することができます。 CORS の仕様もその設定を示す*オリジン*に&quot; \* &quot;が正しくない場合**SupportsCredentials**が true です。

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>カスタムの CORS ポリシー プロバイダー

**[EnableCors]**属性を実装して、 **ICorsPolicyProvider**インターフェイスです。 派生するクラスを作成して、独自の実装を指定できます**属性**を実装して**ICorsProlicyProvider**です。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

これで、任意の場所にする場合は、属性を適用する**[EnableCors]**です。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

たとえば、カスタム CORS ポリシー プロバイダーは、構成ファイルから設定を読み取る可能性があります。

属性を使用する代わりに、として登録することができます、 **ICorsPolicyProviderFactory**オブジェクトを作成する**ICorsPolicyProvider**オブジェクト。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

設定する、 **ICorsPolicyProviderFactory**を呼び出し、 **SetCorsPolicyProviderFactory**起動時に次のように、拡張メソッド。

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>ブラウザー サポート

Web API CORS パッケージは、サーバー側テクノロジです。 ユーザーのブラウザーは、CORS をサポートするためにも必要です。 幸いにも、すべての主要なブラウザーの現在のバージョンが含まれて[CORS のサポート](http://caniuse.com/cors)です。

Internet Explorer 8 および Internet Explorer 9 XMLHttpRequest ではなく、レガシ XDomainRequest オブジェクトを使用して、CORS の部分的なサポートに必要があります。 詳細については、次を参照してください。 [XDomainRequest の制限事項、制限事項と回避策](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx)です。
