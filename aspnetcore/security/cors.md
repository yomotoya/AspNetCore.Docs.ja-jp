---
title: ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。
author: rick-anderson
description: 学習方法として許可または ASP.NET Core アプリでのクロス オリジン要求を拒否するための標準 CORS します。
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2018
uid: security/cors
ms.openlocfilehash: cfbf24edb1dae76f676d51738b0d57266688d53e
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045589"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。

作成者 [Mike Wasson](https://github.com/mikewasson)、 [Shayne Boyer](https://twitter.com/spboyer)、および [Tom Dykstra](https://github.com/tdykstra)

ブラウザーのセキュリティは、web ページが web ページを提供するものとは異なるドメインに要求を行うことを防ぎます。 この制限と呼ばれる、*同一オリジン ポリシー*します。 同一オリジン ポリシーは、悪意のあるサイトが別のサイトから機密データを読み取ることを防ぎます。 場合によっては、他のサイトでは、クロス オリジン要求を行うアプリに許可する可能性があります。

[クロス オリジン リソース共有](https://www.w3.org/TR/cors/)(CORS) は、サーバーに同一生成元ポリシーの制限を緩和させる W3C 標準の１つです。 CORS を使用することによって、不明なリクエストは拒否しながら、一部のクロス オリジン要求のみを明示的に許可できるようになります。 CORS は安全性と以前の手法よりも柔軟性など[JSONP](https://wikipedia.org/wiki/JSONP)します。 このトピックでは、ASP.NET Core アプリで CORS を有効にする方法を示します。

## <a name="same-origin"></a>同じ生成元

同じスキーム、ホスト、およびポートがある場合、2 つの Url が同じ配信元がある ([RFC 6454](https://tools.ietf.org/html/rfc6454))。

次の 2 つの URL は生成元が同じです。

* `https://example.com/foo.html`
* `https://example.com/bar.html`

これらの Url があるさまざまなオリジンより前の 2 つの Url:

* `https://example.net` &ndash; 別のドメイン
* `https://www.example.com/foo.html` &ndash; 別のサブドメイン
* `http://example.com/foo.html` &ndash; 別の配色
* `https://example.com:9000/foo.html` &ndash; 別のポート

> [!NOTE]
> Internet Explorer は、生成元を比較するときにポートを考慮しません。

## <a name="register-cors-services"></a>CORS のサービスを登録します。

::: moniker range=">= aspnetcore-2.1"

参照、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)へのパッケージ参照を追加したり、 [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)パッケージ。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

参照、 [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)へのパッケージ参照を追加したり、 [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)パッケージ。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

パッケージ参照を追加、 [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)パッケージ。

::: moniker-end

呼び出す<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>で`Startup.ConfigureServices`CORS サービス アプリのサービス コンテナーを追加します。

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a>CORS を有効にします。

CORS のサービスを登録すると、ASP.NET Core アプリで CORS を有効にするのに方法を次のいずれかを使用します。

* [CORS ミドルウェア](#enable-cors-with-cors-middleware)&ndash;ミドルウェアを使用してアプリをグローバルに適用する CORS ポリシー。
* [MVC で CORS](#enable-cors-in-mvc) &ndash;アクションごとまたはコント ローラーごとに適用する CORS ポリシー。 CORS ミドルウェアは使用されません。

### <a name="enable-cors-with-cors-middleware"></a>CORS ミドルウェアで CORS を有効にします。

CORS ミドルウェアは、アプリへのクロス オリジン要求を処理します。 要求処理パイプラインでは、CORS ミドルウェアを有効にするを呼び出して、<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>拡張メソッドで`Startup.Configure`します。

CORS ミドルウェアする必要がありますの前に、エンドポイントが定義されて、アプリでのクロス オリジン要求をサポートする (たとえば、呼び出しの前に`UseMvc`MVC と Razor ページのミドルウェアの)。

A*クロス オリジン ポリシー*を使用して、CORS ミドルウェアを追加するときに指定することができます、<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>クラス。 CORS ポリシーを定義するための 2 つの方法はあります。

* 呼び出す`UseCors`ラムダで。

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  ラムダは、<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> オブジェクトをとります。 [構成オプション](#cors-policy-options)など`WithOrigins`はこのトピックの後半で説明します。 上記の例では、ポリシーによりからのクロス オリジン要求`https://example.com`およびその他のオリジンはありません。

  末尾のスラッシュせず、URL を指定する必要があります (`/`)。 URL が終了した場合は`/`、比較を返します`false`ヘッダーは返されません。

  `CorsPolicyBuilder` メソッドの呼び出しをチェーンできるように、fluent API があります。

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* 1 つまたは複数の名前付き CORS ポリシーを定義し、実行時に名前で、ポリシーを選択します。 次の例では、という名前のユーザー定義の CORS ポリシー *AllowSpecificOrigin*します。 ポリシーを選択する名前を渡す`UseCors`:

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a>MVC で CORS を有効にします。

MVC を使用して、アクションごとまたはコント ローラーごとに特定の CORS ポリシーを適用することもできます。 MVC を使用して、CORS を有効にする、登録されている CORS サービスが使用されます。 CORS ミドルウェアは使用されません。

### <a name="per-action"></a>アクションごと

特定のアクションの CORS ポリシーを指定するには、追加、 [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)属性をアクションにします。 ポリシー名を指定します。

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a>コント ローラーごと

特定のコント ローラーの CORS ポリシーを指定するには、追加、 [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)属性をコント ローラー クラス。 ポリシー名を指定します。

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

優先順位は次のとおりです。

1. アクション
1. コントローラー

### <a name="disable-cors"></a>CORS を無効にします。

コント ローラーまたはアクションの CORS を無効にする、 [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)属性。

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a>CORS ポリシー オプション

このセクションでは、CORS ポリシーで設定できるさまざまなオプションについて説明します。

* [許可されるオリジンを設定します。](#set-the-allowed-origins)
* [許可される HTTP メソッドを設定します。](#set-the-allowed-http-methods)
* [許可されている要求ヘッダーを設定します。](#set-the-allowed-request-headers)
* [公開されている応答ヘッダーを設定します。](#set-the-exposed-response-headers)
* [クロス オリジン要求で資格情報](#credentials-in-cross-origin-requests)
* [プレフライトの有効期限を設定します。](#set-the-preflight-expiration-time)

いくつかのオプションの読み取りをすると役立つ場合があります、 [CORS ではどのように動作](#how-cors-works)最初のセクションします。

### <a name="set-the-allowed-origins"></a>許可されるオリジンを設定します。

1 つまたは複数の特定のオリジンを許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

すべてのオリジンを許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

任意のオリジンからの要求を許可する前に慎重に検討してください。 任意のオリジンからの要求を許可することを意味*任意の web サイト*アプリへのクロス オリジン要求を行うことができます。

この設定に影響[プレフライト要求とアクセス制御の許可-オリジン ヘッダー](#preflight-requests) (このトピックの後半で説明)。

### <a name="set-the-allowed-http-methods"></a>許可される HTTP メソッドを設定します。

すべての HTTP メソッドを許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

この設定に影響[プレフライト要求とアクセスの制御-許可する-メソッド ヘッダー](#preflight-requests) (このトピックの後半で説明)。

### <a name="set-the-allowed-request-headers"></a>許可されている要求ヘッダーを設定します。

CORS 要求で送信される特定のヘッダーを許可するという*要求ヘッダーを作成する*、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>し、許可されたヘッダーを指定します。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

許可するのには、すべての著者要求ヘッダー、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

この設定に影響[プレフライト要求とアクセス制御の要求ヘッダー ヘッダー](#preflight-requests) (このトピックの後半で説明)。

::: moniker range=">= aspnetcore-2.2"

指定された特定のヘッダーに一致する CORS ミドルウェア ポリシー`WithHeaders`ヘッダーが送信されるときにのみ可能なは`Access-Control-Request-Headers`に記載されているヘッダーと正確に一致`WithHeaders`します。

たとえば、次のように構成されているアプリを検討してください。

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS ミドルウェアは、次の要求ヘッダーでプレフライト要求を拒否`Content-Language`([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) の一覧にない`WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

アプリを返します、 *200 ok をクリック*応答が戻り、CORS ヘッダーを送信しません。 そのため、ブラウザーでは、クロス オリジン要求を試行しません。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

CORS ミドルウェアで 4 つのヘッダーを常に許可する、 `Access-Control-Request-Headers` CorsPolicy.Headers で構成されている値に関係なく送信します。 このヘッダーの一覧は次のとおりです。

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

たとえば、次のように構成されているアプリを検討してください。

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS ミドルウェアは、次の要求ヘッダーでプレフライト要求に正常に応答ため`Content-Language`は常にホワイト リストに登録します。

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>公開されている応答ヘッダーを設定します。

既定では、ブラウザーはすべてのアプリに応答ヘッダーで公開されません。 詳細については、次を参照してください。 [W3C のクロス オリジン リソース共有 (用語集): 単純な応答ヘッダー](https://www.w3.org/TR/cors/#simple-response-header)します。

既定で使用できる応答ヘッダーは次のとおりです。

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

CORS の仕様は、これらのヘッダーを呼び出す*単純な応答ヘッダー*します。 で他のヘッダーをアプリに使用できるように呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>クロス オリジン要求で資格情報

資格情報では、CORS 要求で特別な処理が必要です。 既定では、ブラウザーは、クロス オリジン要求に資格情報を送信しません。 Cookie および HTTP 認証方式は、資格情報が含まれます。 クロス オリジン要求に資格情報を送信するクライアントを設定する必要があります`XMLHttpRequest.withCredentials`に`true`します。

使用して`XMLHttpRequest`直接。

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

Jquery では。

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

さらに、サーバーは、資格情報を許可する必要があります。 クロス オリジンの資格情報を許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

HTTP 応答が含まれる、`Access-Control-Allow-Credentials`ヘッダーで、サーバーでクロス オリジン要求の資格情報は、ブラウザーに指示します。

ブラウザーが資格情報を送信しますが、有効な応答を含まない`Access-Control-Allow-Credentials`ヘッダー、ブラウザーは、アプリへの応答を公開しないし、クロス オリジン要求は失敗します。

クロス オリジンの資格情報を許可する際に注意します。 別のドメインに web サイトでは、ユーザーの知識がなくても、ユーザーの代わりに、アプリにサインイン済みのユーザーの資格情報を送信できます。

CORS の仕様もその設定を示すオリジンを`"*"`(すべてのオリジン) 有効でない場合、`Access-Control-Allow-Credentials`ヘッダーが存在します。

### <a name="preflight-requests"></a>プレフライト要求

一部の CORS 要求では、ブラウザーは、実際の要求を行う前に、追加の要求を送信します。 この要求と呼ばれる、*プレフライト要求*します。 次の条件に該当する場合、ブラウザーでプレフライト要求をスキップできます。

* 要求メソッドは、GET、HEAD、または POST です。
* アプリが要求ヘッダー以外に設定されていない`Accept`、 `Accept-Language`、 `Content-Language`、 `Content-Type`、または`Last-Event-ID`します。
* `Content-Type`ヘッダー場合、次の値のいずれか。
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

要求ヘッダーでルール セットのクライアント要求を呼び出すことによって、アプリを設定するヘッダーに適用の`setRequestHeader`上、`XMLHttpRequest`オブジェクト。 CORS の仕様は、これらのヘッダーを呼び出す*要求ヘッダーを作成する*します。 ヘッダーは、ブラウザー設定できるように、ルールは適用されません`User-Agent`、 `Host`、または`Content-Length`します。

プレフライト要求の例を次に示します。

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

事前要求は HTTP OPTIONS メソッドを使用します。 2 つの特殊なヘッダーが含まれています。

* `Access-Control-Request-Method`: 実際の要求に使用される HTTP メソッド。
* `Access-Control-Request-Headers`: アプリが、実際の要求で設定できる要求ヘッダーの一覧。 前述のように、ブラウザー設定などのヘッダーは含まれません`User-Agent`します。

CORS プレフライト要求を含めることができます、`Access-Control-Request-Headers`ヘッダーで、サーバーの実際の要求で送信されるヘッダーを示します。

特定のヘッダーを許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

許可するのには、すべての著者要求ヘッダー、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

ブラウザーはどのように設定でまったく一貫性のある`Access-Control-Request-Headers`します。 以外に何もヘッダーを設定する場合`"*"`(を使用して、または<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>)、以上含める必要がある`Accept`、 `Content-Type`、および`Origin`、さらにサポートするカスタム ヘッダー。

(サーバーが要求を許可することを想定) プレフライト要求に応答の例を次に示します。

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

応答が含まれています、`Access-Control-Allow-Methods`ヘッダーを許可されるメソッドを一覧表示して、必要に応じて、`Access-Control-Allow-Headers`ヘッダーで、許可されたヘッダーを一覧表示されます。 プレフライト要求が成功すると、ブラウザーは、実際の要求を送信します。

アプリが返したプレフライト要求が拒否された場合、 *200 OK*応答戻る、CORS ヘッダーを送信しないが。 そのため、ブラウザーでは、クロス オリジン要求を試行しません。

### <a name="set-the-preflight-expiration-time"></a>プレフライトの有効期限を設定します。

`Access-Control-Max-Age`ヘッダーは、プレフライト要求に応答をキャッシュできる期間を指定します。 このヘッダーを設定するには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a>CORS のしくみ

このセクションでは、HTTP メッセージのレベルでの CORS 要求での動作について説明します。 CORS ポリシーを正しく構成されているし、予期しない動作が発生したときにデバッグできるようにの CORS のしくみを理解しておく必要があります。

CORS の仕様には、クロス オリジン要求を有効にするいくつかの新しい HTTP ヘッダーが導入されています。 ブラウザーでは、CORS をサポートする場合は、クロス オリジン要求を自動的にこれらのヘッダーを設定します。 カスタム JavaScript コードは、CORS を有効にする必要はありません。

次は、クロス オリジン要求の例です。 `Origin`ヘッダーは要求を行っているサイトのドメインを提供します。

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

設定されているサーバーでは、要求を許可している場合、`Access-Control-Allow-Origin`応答のヘッダー。 このヘッダーの値と一致するか、`Origin`要求からヘッダーまたはワイルドカード値`"*"`、任意のオリジンを許可することを意味します。

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

応答に含まれていない場合、`Access-Control-Allow-Origin`ヘッダー、クロス オリジン要求は失敗します。 具体的には、ブラウザーには、要求が許可されていません。 サーバーに正常な応答が返される場合でも、ブラウザーは、応答を使用できるようにクライアント アプリ。

## <a name="additional-resources"></a>その他の技術情報

* [クロス オリジン リソース共有 (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
