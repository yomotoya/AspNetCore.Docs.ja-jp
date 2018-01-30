---
title: "クロス オリジン要求 (CORS) を有効にします。"
author: rick-anderson
description: "このドキュメントでは、許可するか、または ASP.NET Core アプリケーションでのクロス オリジン要求を拒否するための基準として CORS が導入されています。"
manager: wpickett
ms.author: riande
ms.date: 05/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cors
ms.openlocfilehash: 1c0d87b61882f69dbf2aeb0a896d9294bd029374
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="enabling-cross-origin-requests-cors"></a>クロス オリジン要求 (CORS) を有効にします。

によって[Mike Wasson](https://github.com/mikewasson)、 [Shayne Boyer](https://twitter.com/spboyer)、および[Tom Dykstra](https://github.com/tdykstra)

ブラウザーのセキュリティは、web ページが別のドメインに AJAX 要求を行うことを防止します。 この制限が呼び出された、*同一生成元ポリシー*、悪意のあるサイトが別のサイトから機密データを読み取ることを防ぎます。 ただし、場合もあります可能性がある、web API へのクロス オリジン要求を行う他のサイトを使用できます。

[クロス オリジン リソース共有](http://www.w3.org/TR/cors/)(CORS) は、W3C 標準により、同じオリジンのポリシーを緩和するサーバーです。 CORS を使用して、サーバー明示的に許可できますいくつかのクロス オリジン要求中に、他のユーザーを拒否します。 CORS などがより安全なと以前の手法より柔軟な[JSONP](https://wikipedia.org/wiki/JSONP)です。 このトピックでは、ASP.NET Core アプリケーションで CORS を有効にする方法を示します。

## <a name="what-is-same-origin"></a>「同じ発生元」とは何ですか。

2 つの Url では、同じスキーム、ホスト、およびポートがある場合の原点が同じがあります。 ([RFC 6454](http://tools.ietf.org/html/rfc6454))

これら 2 つの Url は、原点が同じであります。

* `http://example.com/foo.html`

* `http://example.com/bar.html`

これらの Url は、2 つよりも前の別の原点をあります。

* `http://example.net`-別のドメイン

* `http://www.example.com/foo.html`-別のサブドメイン

* `https://example.com/foo.html`-別のスキーム

* `http://example.com:9000/foo.html`-別のポート

> [!NOTE]
> Internet Explorer は、オリジンを比較するときにポートを検討しません。

## <a name="setting-up-cors"></a>CORS の設定

設定するアプリケーションの CORS の追加、`Microsoft.AspNetCore.Cors`をプロジェクトにパッケージします。

Startup.cs の CORS サービスを追加します。

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>ミドルウェアで CORS を有効にします。

有効にする、アプリケーション全体の CORS では、要求パイプラインを使用して、CORS ミドルウェアを追加、`UseCors`拡張メソッド。 CORS ミドルウェアが (ex クロス オリジン要求をサポートするアプリで定義されたエンドポイントを付ける必要がありますに注意してください。 呼び出しの前に`UseMvc`)。

クロス オリジン ポリシーを指定するには、CORS ミドルウェアを使用して、追加するときに、`CorsPolicyBuilder`クラスです。 これには、2 つの方法があります。 1 つは、ラムダで UseCors を呼び出すには。

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**注:**末尾のスラッシュせず、URL を指定する必要があります (`/`)。 URL が終了した場合は`/`、比較が返されます`false`され、ヘッダーは返されません。

ラムダは、`CorsPolicyBuilder`オブジェクト。 リストができたら、[構成オプション](#cors-policy-options)このトピックで後述します。 この例では、ポリシーによりからのクロス オリジン要求`http://example.com`しない他のオリジンです。

CorsPolicyBuilder いる fluent API では、メソッド呼び出しを連結することができますので注意してください。

[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

2 番目の方法を 1 つまたは複数名前付き CORS ポリシーを定義し、名前によって実行時にポリシーを選択します。

[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

この例では、"AllowSpecificOrigin"をという名前の CORS ポリシーを追加します。 ポリシーを選択するには、名前を渡す`UseCors`です。

## <a name="enabling-cors-in-mvc"></a>MVC での CORS を有効にします。

MVC は、アクション、コント ローラーごとまたはグローバルにすべてのコント ローラーごとの特定の CORS を適用するのに代わりに使用できます。 MVC を使用して、CORS を有効にする場合、同じ CORS サービスを使用するが、CORS ミドルウェアではありません。

### <a name="per-action"></a>アクションごと

特定のアクションの CORS ポリシーの追加を指定する、`[EnableCors]`属性をアクションにします。 ポリシー名を指定します。

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>コント ローラーあたり

特定のコント ローラーの CORS ポリシーの追加を指定する、`[EnableCors]`属性をコント ローラー クラスにします。 ポリシー名を指定します。

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>グローバル

有効にする CORS グローバルにすべてのコント ローラーの追加、`CorsAuthorizationFilterFactory`グローバル フィルターのコレクションをフィルターします。

[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

優先順位: アクション、コント ローラーで、グローバルです。 アクション レベル ポリシー コント ローラー レベルのポリシーより優先し、コント ローラー レベルのポリシーのグローバル ポリシーよりも優先します。

### <a name="disable-cors"></a>CORS を無効にします。

コント ローラーまたはアクションの CORS を無効にする、`[DisableCors]`属性。

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>CORS ポリシーのオプション

このセクションでは、CORS ポリシーで設定できるさまざまなオプションについて説明します。

* [許可されるオリジンを設定します。](#set-the-allowed-origins)

* [許可される HTTP メソッドを設定します。](#set-the-allowed-http-methods)

* [許可されている要求ヘッダーを設定します。](#set-the-allowed-request-headers)

* [公開されている応答ヘッダーを設定します。](#set-the-exposed-response-headers)

* [クロス オリジン要求に資格情報](#credentials-in-cross-origin-requests)

* [プレフライト有効期限を設定します。](#set-the-preflight-expiration-time)

いくつかのオプションの読み取りに役立つ場合があります[方法 CORS 機能](#how-cors-works)最初。

### <a name="set-the-allowed-origins"></a>許可されるオリジンを設定します。

1 つまたは複数の特定のオリジンを許可します。

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]

すべてのオリジンを許可します。

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]

すべてのオリジンからの要求を許可する前に慎重に検討してください。 これは、事実上あらゆる web サイトが、API への AJAX 呼び出しを実行できることを意味します。

### <a name="set-the-allowed-http-methods"></a>許可される HTTP メソッドを設定します。

すべての HTTP メソッドを使用するには。

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]

これは、事前要求とアクセスの制御の許可する-メソッド ヘッダーに影響します。

### <a name="set-the-allowed-request-headers"></a>許可されている要求ヘッダーを設定します。

CORS プレフライト要求がアプリケーションによって設定される HTTP ヘッダーの一覧を表示する、アクセス コントロール-要求ヘッダー ヘッダーが含ま可能性があります (いわゆる"要求ヘッダーを author") です。

特定のヘッダー。 ホワイト リストを

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]

許可するには、すべての著者要求ヘッダー。

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]

ブラウザーがアクセス コントロール-要求ヘッダーを設定する方法の完全一致しません。 以外の何もするヘッダーを設定する場合は"*"、する必要がありますを含めるには、少なくとも「受け入れる」、「コンテンツの種類」と「発生元」、およびサポートする任意のカスタム ヘッダー。

### <a name="set-the-exposed-response-headers"></a>公開されている応答ヘッダーを設定します。

既定では、ブラウザーはすべてのアプリケーションに応答ヘッダーを公開しません。 (を参照してください[http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header))。既定で利用できる応答ヘッダーは次のとおりです。

* Cache-Control

* コンテンツの言語

* Content-Type

* 有効期限が切れる

* Last-Modified

* プラグマ

CORS の仕様を呼び出す*単純な応答ヘッダー*です。 その他のヘッダー使用可能にするアプリケーション。

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>クロス オリジン要求に資格情報

資格情報では、CORS 要求で特別な処理が必要です。 既定では、ブラウザーは、クロス オリジン要求に資格情報を送信しません。 Cookie と、HTTP 認証スキームの資格情報が含まれます。 クロス オリジン要求に資格情報を送信するには、クライアントは XMLHttpRequest.withCredentials を true に設定する必要があります。

XMLHttpRequest を直接使用するには。

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

JQuery: で

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

さらに、サーバーは、資格情報を許可する必要があります。 クロス オリジンの資格情報を使用するには。

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]

これで、HTTP 応答では、サーバーでクロス オリジン要求の資格情報は、ブラウザーに指示する、アクセス コントロール-を許可する-資格情報ヘッダーが含まれます。

ブラウザーが資格情報を送信、応答には有効なアクセス制御を許可する-資格情報ヘッダーが含まれていない場合は、ブラウザーは、アプリケーションへの応答を公開しないし、AJAX 要求は失敗します。

クロス オリジンの資格情報を許可する場合に注意します。 別のドメインで web サイトは、ユーザーの知らない間にユーザーの代理でアプリにログインしているユーザーの資格情報を送信できます。 CORS の仕様もその設定を規定する配信元"*"(すべてのオリジン) が有効ではない場合、`Access-Control-Allow-Credentials`ヘッダーが存在します。

### <a name="set-the-preflight-expiration-time"></a>プレフライト有効期限を設定します。

アクセス コントロール-Max-age ヘッダーでは、プレフライト要求に応答をキャッシュできる期間を指定します。 このヘッダーを設定します。

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>CORS のしくみ

このセクションでは、HTTP メッセージのレベルでの CORS 要求での動作について説明します。 予期しない動作が発生したときに CORS ポリシーを正しく構成できるようにする CORS のしくみと troubleshooted を理解しておく必要があります。

CORS の仕様には、クロス オリジン要求を有効にするいくつかの新しい HTTP ヘッダーが導入されています。 ブラウザーでは、CORS をサポートする場合は、クロス オリジン要求を自動的にこれらのヘッダーを設定します。 カスタムの JavaScript コードは、CORS を有効にするため必要はありません。

クロス オリジン要求の例を次に示します。 `Origin`ヘッダーは要求を行っているサイトのドメインを提供します。

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

サーバーは、要求を許可している場合は、応答でアクセス制御の許可する-オリジン ヘッダーを設定します。 このヘッダーの値は、要求の Origin ヘッダーと一致するか、ワイルドカード文字は、"*"、すべてのオリジンを許可されていることを意味します。

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

応答には、アクセス コントロール-を許可する-オリジン ヘッダーが含まれていない、AJAX 要求が失敗します。 具体的には、ブラウザーには、要求が許可されていません。 サーバーでは、正常な応答を返す、場合でも、ブラウザーしない応答を使用できるように、クライアント アプリケーション。

### <a name="preflight-requests"></a>プレフライト要求

いくつかの CORS 要求については、ブラウザーは、リソースの実際の要求を送信する前に「プレフライト要求を」と呼ばれる、追加の要求を送信します。 ブラウザーは、次の条件に該当する場合、プレフライト要求を省略できます。

* 要求メソッドが GET、HEAD、または POST、および

* アプリケーションが、要求のヘッダーを受け入れる、Accept-language、Content-language 以外に設定されていないコンテンツの種類、または最後のイベント ID、および

* Content-type ヘッダー (場合に設定) は、次のいずれか。

  * application/x-www-form-urlencoded

  * マルチパート フォーム データ

  * text/plain

アプリケーションで、XMLHttpRequest オブジェクトで setRequestHeader を呼び出すことによって設定されたヘッダーを要求ヘッダーについて規則が適用されます。 (CORS の仕様は、これら「作成者要求ヘッダー」を呼び出します)。ルールは、ユーザー エージェント、ホスト、またはコンテンツの長さなど、ブラウザーを設定できますヘッダーに適用されません。

プレフライト要求の例を次に示します。

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

事前要求は、HTTP OPTIONS メソッドを使用します。 2 つの特殊なヘッダーが含まれています。

* アクセス コントロール-要求メソッド: 実際の要求に使用される HTTP メソッド。

* アクセス コントロール-要求ヘッダー。 アプリケーションが、実際の要求で設定できる要求ヘッダーの一覧。 (ここでも、これが含まれていないブラウザーを設定するヘッダーには。)

次に、応答の例、サーバーで要求を許可すると仮定した場合を示します。

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

応答には、許可されているメソッドを一覧表示するアクセスの制御の許可する-メソッド ヘッダーおよび必要に応じて、アクセス コントロール-を許可する-ヘッダー ヘッダー、許可されているヘッダーの一覧が含まれています。 プレフライト要求が成功した場合、ブラウザーは、前述のとおり、実際の要求を送信します。
