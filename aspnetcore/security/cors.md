---
title: ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。
author: rick-anderson
description: 学習方法として許可または ASP.NET Core アプリでのクロス オリジン要求を拒否するための標準 CORS します。
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2018
ms.locfileid: "41828128"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。

作成者 [Mike Wasson](https://github.com/mikewasson)、 [Shayne Boyer](https://twitter.com/spboyer)、および [Tom Dykstra](https://github.com/tdykstra)

ブラウザーのセキュリティは、Web ページが別のドメインに AJAX 要求を行うことを防止します。 この制限は*同一生成元ポリシー*と呼ばれ、悪意のあるサイトが別のサイトから機密データを読み取れないようにします。 しかし、他のサイトがあなたの Web API にクロスオリジン要求を行えるようにする必要がある場合もあります。

[クロス オリジン リソース共有](http://www.w3.org/TR/cors/)(CORS) は、サーバーに同一生成元ポリシーの制限を緩和させる W3C 標準の１つです。 CORS を使用することによって、不明なリクエストは拒否しながら、一部のクロス オリジン要求のみを明示的に許可できるようになります。 CORS は [JSONP](https://wikipedia.org/wiki/JSONP) のようなかつての技術より安全でフレキシブルなものです。 このトピックでは、ASP.NET Core アプリケーションで CORS を有効にする方法を説明します。

## <a name="what-is-same-origin"></a>「同一生成元」とは

2 つの URL のスキーム、ホスト、ポートが同じである場合、その URL は同一生成元となります。 ([RFC 6454](http://tools.ietf.org/html/rfc6454))

次の 2 つの URL は生成元が同じです。

* `http://example.com/foo.html`

* `http://example.com/bar.html`

次の URL は、上の URL とは生成元が異なります。

* `http://example.net` - 異なるドメイン 

* `http://www.example.com/foo.html` - 異なるサブドメイン

* `https://example.com/foo.html` - 異なるスキーム

* `http://example.com:9000/foo.html` - 異なるポート

> [!NOTE]
> Internet Explorer は、生成元を比較するときにポートを考慮しません。

## <a name="enable-cors"></a>CORS を有効にします。

::: moniker range="<= aspnetcore-1.1"

アプリケーションに CORS を設定するために `Microsoft.AspNetCore.Cors` パッケージをプロジェクトに追加します。

::: moniker-end

呼び出す[AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors)で`Startup.ConfigureServices`:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>ミドルウェアによる CORS の有効化

CORS を有効にするには、要求パイプラインを使用して、CORS ミドルウェアを追加、`UseCors`拡張メソッド。 CORS ミドルウェアする必要がありますの前に、エンドポイントが定義されて、アプリでのクロス オリジン要求をサポートする (たとえばへの呼び出しの前に`UseMvc`)。

使用して、CORS ミドルウェアを追加するときに、クロス オリジン ポリシーを指定することができます、 [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors)クラス。 これには、2 つの方法があります。 最初に呼び出す`UseCors`ラムダで。

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**注:** URL は末尾にスラッシュを付けずに指定される必要があります (`/`)。 URL が `/`で終了する場合、比較時に`false`が返され、ヘッダーが返されません。

ラムダは、`CorsPolicyBuilder` オブジェクトをとります。 [構成オプション](#cors-policy-options)のリストはこのトピックで後述します。 この例では、ポリシーは `http://example.com` からのクロス オリジン要求を許可し、他の生成元からの要求は許可しません。

CorsPolicyBuilder では、メソッドの呼び出しをチェーンするように fluent API があります。

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

2 つ目の方法は、名前が付いた CORS ポリシーを 1 つまたは複数定義し、実行時に名前によってポリシーを選択することです。

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

この例では、"AllowSpecificOrigin" という名前の CORS ポリシーを追加します。  このポリシーを選択するには、`UseCors` にこの名前を渡します。

## <a name="enabling-cors-in-mvc"></a>MVC で CORS を有効にします。

MVC を使用して、アクション、コント ローラーごと、またはすべてのコント ローラーをグローバルにごとの特定の CORS を適用することもできます。 MVC を使用して CORS を有効にする場合、同じ CORS サービスを使用するが、CORS ミドルウェアはありません。

### <a name="per-action"></a>アクションごと

指定する特定のアクションの CORS ポリシーの追加、`[EnableCors]`属性をアクションにします。 ポリシー名を指定します。

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>コント ローラーごと

指定する CORS ポリシーを特定のコント ローラーの追加、`[EnableCors]`属性をコント ローラー クラス。 ポリシー名を指定します。

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>グローバルに

できます CORS を有効にグローバルにすべてのコント ローラーを追加することで、`CorsAuthorizationFilterFactory`グローバル フィルターのコレクションをフィルターします。

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

優先順位は、: アクション、コント ローラー、グローバルです。 アクション レベルのポリシーがコント ローラー レベルのポリシーよりも優先され、コント ローラー レベルのポリシーがグローバル ポリシーより優先します。

### <a name="disable-cors"></a>CORS を無効にします。

コント ローラーまたはアクションの CORS を無効にする、`[DisableCors]`属性。

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>CORS ポリシー オプション

このセクションでは、CORS ポリシーで設定できるさまざまなオプションについて説明します。

* [許可されるオリジンを設定します。](#set-the-allowed-origins)

* [許可される HTTP メソッドを設定します。](#set-the-allowed-http-methods)

* [許可されている要求ヘッダーを設定します。](#set-the-allowed-request-headers)

* [公開されている応答ヘッダーを設定します。](#set-the-exposed-response-headers)

* [クロス オリジン要求で資格情報](#credentials-in-cross-origin-requests)

* [プレフライトの有効期限を設定します。](#set-the-preflight-expiration-time)

いくつかのオプションの読み取りをすると役立つ場合があります[CORS ではどのように動作](#how-cors-works)最初。

### <a name="set-the-allowed-origins"></a>許可されるオリジンを設定します。

1 つまたは複数の特定のオリジンを許可します。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

すべてのオリジンを許可します。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

任意のオリジンからの要求を許可する前に慎重に検討してください。 あらゆる web サイトが、API への AJAX 呼び出しを実行できることを意味します。

### <a name="set-the-allowed-http-methods"></a>許可される HTTP メソッドを設定します。

すべての HTTP メソッドを許可します。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

これは、事前要求とアクセスの制御-許可する-メソッド ヘッダーに影響します。

### <a name="set-the-allowed-request-headers"></a>許可されている要求ヘッダーを設定します。

CORS プレフライト要求に、アプリケーションで設定される HTTP ヘッダーを一覧表示、アクセス制御の要求ヘッダー ヘッダーが含まれます (いわゆる"author 要求ヘッダー")。

特定のヘッダーをホワイト リストに登録します。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

すべてを許可するには、要求ヘッダーを作成します。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

ブラウザーのアクセス制御の要求ヘッダーを設定する方法で完全に一貫していません。 以外に何もヘッダーを設定する場合は、"*"を含める必要がある、少なくとも"accept"、「コンテンツの種類」と"origin"、およびサポートするカスタム ヘッダー。

### <a name="set-the-exposed-response-headers"></a>公開されている応答ヘッダーを設定します。

既定では、ブラウザーはすべてのアプリケーションに応答ヘッダーで公開されません。 (を参照してください[ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header))。既定で使用できる応答ヘッダーは次のとおりです。

* キャッシュ制御

* コンテンツの言語

* Content-Type

* 有効期限が切れます

* Last-Modified

* プラグマ

CORS の仕様を呼び出す*単純な応答ヘッダー*します。 その他のヘッダーをアプリケーションに使用可能には。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>クロス オリジン要求で資格情報

資格情報では、CORS 要求で特別な処理が必要です。 既定では、ブラウザーは、クロス オリジン要求と共に資格情報を送信しません。 資格情報には、cookie として HTTP 認証方式がなどがあります。 クロス オリジン要求に資格情報を送信するには、クライアントは XMLHttpRequest.withCredentials を true に設定する必要があります。

XMLHttpRequest を直接使用するには。

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

Jquery では。

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

さらに、サーバーは、資格情報を許可する必要があります。 クロス オリジンの資格情報を許可します。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

これで、HTTP 応答には、アクセス コントロール-許可する-資格情報のヘッダー。 クロス オリジン要求の資格情報で、サーバーは、ブラウザーに指示が含まれます。

ブラウザーが資格情報を送信、応答には有効なアクセス制御を許可する-資格情報のヘッダーが含まれていない場合は、ブラウザーは、アプリケーションへの応答を公開しないし、AJAX 要求は失敗します。

クロス オリジンの資格情報を許可する際に注意します。 別のドメインに web サイトでは、ユーザーの知識がなくても、ユーザーの代わりに、アプリにログインしているユーザーの資格情報を送信できます。 CORS の仕様もその設定を示すオリジンを`"*"`(すべてのオリジン) 有効でない場合、`Access-Control-Allow-Credentials`ヘッダーが存在します。

### <a name="set-the-preflight-expiration-time"></a>プレフライトの有効期限を設定します。

アクセス制御、Max-age ヘッダーでは、プレフライト要求に応答をキャッシュできる期間を指定します。 このヘッダーを設定します。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>CORS のしくみ

このセクションでは、HTTP メッセージのレベルでの CORS 要求での動作について説明します。 CORS ポリシーを正しく構成されているし、予期しない動作が発生したときにデバッグできるようにの CORS のしくみを理解しておく必要があります。

CORS の仕様には、クロス オリジン要求を有効にするいくつかの新しい HTTP ヘッダーが導入されています。 ブラウザーでは、CORS をサポートする場合は、クロス オリジン要求を自動的にこれらのヘッダーを設定します。 カスタム JavaScript コードは、CORS を有効にする必要はありません。

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

サーバーは、要求を許可している場合、応答のアクセス制御の許可-オリジン ヘッダーを設定します。 このヘッダーの値は、要求から配信元のヘッダーと一致するか、ワイルドカード値は、"*"、任意のオリジンを許可することを意味します。

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

応答には、アクセス制御の許可-オリジン ヘッダーが含まれていない、AJAX 要求は失敗します。 具体的には、ブラウザーには、要求が許可されていません。 サーバーに正常な応答が返される場合でも、ブラウザーは応答を使用できるように、クライアント アプリケーション。

### <a name="preflight-requests"></a>プレフライト要求

一部の CORS 要求では、ブラウザーは、リソースの実際の要求を送信する前に「プレフライト要求を」と呼ばれる追加の要求を送信します。 次の条件に該当する場合、ブラウザーでプレフライト要求をスキップできます。

* 要求メソッドが GET、HEAD、または POST、および

* アプリケーションが承諾、Accept-language、Content-language 以外のすべての要求ヘッダーに設定されていないコンテンツの種類、または最後のイベント ID、および

* Content-type ヘッダー (場合設定) は、次の 1 つです。

  * application/x-www-form-urlencoded

  * マルチパート/フォーム データ

  * テキスト/プレーン

要求ヘッダーについて、ルールは、XMLHttpRequest オブジェクトに対して setRequestHeader を呼び出すことで、アプリケーションを設定するヘッダーに適用されます。 (CORS の仕様は、これら「作成者要求ヘッダー」を呼び出します)。ユーザー エージェント、ホスト、またはコンテンツの長さなど、ブラウザーから設定できるヘッダーには、ルールは適用されません。

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

事前要求は HTTP OPTIONS メソッドを使用します。 2 つの特殊なヘッダーが含まれています。

* アクセス制御の要求メソッド: 実際の要求に使用される HTTP メソッド。

* アクセス制御の要求ヘッダー: アプリケーションが、実際の要求で設定できる要求ヘッダーの一覧。 (ここでも、これは含まれません、ブラウザーを設定するヘッダー。)

サーバーが要求を許可すると仮定すると、例応答を次に示します。

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

応答には、許可されているメソッドを一覧表示するアクセスの制御-許可する-メソッド ヘッダーと、必要に応じて、アクセスの制御-許可する-ヘッダー ヘッダー、許可されたヘッダーの一覧を表示するが含まれます。 プレフライト要求が成功すると、ブラウザーは、前述のように、実際の要求を送信します。
