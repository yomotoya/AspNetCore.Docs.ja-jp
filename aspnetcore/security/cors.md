---
title: ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。
author: rick-anderson
description: 学習方法として許可または ASP.NET Core アプリでのクロス オリジン要求を拒否するための標準 CORS します。
ms.author: riande
ms.custom: mvc
ms.date: 02/08/2019
uid: security/cors
ms.openlocfilehash: bc3a0883043a4d6fa33c1ff76fcb7be457b6b840
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899347"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

この記事では、ASP.NET Core アプリで CORS を有効にする方法を示します。

ブラウザーのセキュリティは、web ページが web ページを提供するものとは異なるドメインに要求を行うことを防ぎます。 この制限と呼ばれる、*同一オリジン ポリシー*します。 同一オリジン ポリシーは、悪意のあるサイトが別のサイトから機密データを読み取ることを防ぎます。 場合によっては、他のサイトでは、クロス オリジン要求を行うアプリに許可する可能性があります。 詳細については、次を参照してください。、 [Mozilla CORS 記事](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)します。

[クロス オリジン リソース共有](https://www.w3.org/TR/cors/)(CORS)。

* W3C による同一オリジン ポリシーの緩和するようにサーバーを利用できる標準。
* **いない**CORS、セキュリティ機能は、セキュリティを緩和します。 CORS を許可することで、API をより安全なすることはできません。 詳細については、次を参照してください。 [CORS ではどのように動作](#how-cors)します。
* 他のユーザーを拒否しながら、いくつかのクロス オリジン要求を明示的に許可するサーバーを使用できます。
* 安全性と以前の手法よりも柔軟性はなど、 [JSONP](/dotnet/framework/wcf/samples/jsonp)します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

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

Internet Explorer は、生成元を比較するときにポートを考慮しません。

## <a name="cors-with-named-policy-and-middleware"></a>名前付きポリシーとミドルウェアで CORS

CORS ミドルウェアは、クロス オリジン要求を処理します。 次のコードでは、指定された配信元とアプリ全体に対して CORS を有効にします。

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

上のコードでは以下の操作が行われます。

* "_MyAllowSpecificOrigins"に、ポリシー名を設定します。 ポリシーの名前は任意です。
* 呼び出し、<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>拡張メソッドは、コアを使用します。
* 呼び出し<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*>で、[ラムダ式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)します。 ラムダは、<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> オブジェクトをとります。 [構成オプション](#cors-policy-options)など`WithOrigins`はこの記事の後半で説明します。

<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>メソッドの呼び出しは、アプリのサービス コンテナーにサービスの CORS を追加します。

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

詳細については、次を参照してください。 [CORS ポリシー オプション](#cpo)このドキュメントで。

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>メソッドは次のコードに示すように、メソッドを連結できます。

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

次の強調表示されたコードを使用してすべてのアプリのエンドポイントに CORS ポリシーを適用する[CORS ミドルウェア](#enable-cors-with-cors-middleware):

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet3&highlight=12)]

参照してください[Razor ページ、コントローラ、およびアクション メソッドで CORS を有効にする](#ecors)アクション/ページ/コント ローラー レベルでの CORS ポリシーを適用します。

メモ:

* `UseCors` 前に呼び出す必要があります`UseMvc`します。
* URL にする必要があります**いない**末尾のスラッシュを含める (`/`)。 URL が終了した場合は`/`、比較を返します`false`ヘッダーは返されません。

参照してください[テスト CORS](#test)については、上記のコードをテストします。

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a>属性を持つ CORS を有効にします。

[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)属性は、CORS をグローバルに適用する代替を提供します。 `[EnableCors]`属性は、すべてのエンドポイントではなく、選択した最後の点で CORS を使用します。

使用`[EnableCors]`既定のポリシーを指定して`[EnableCors("{Policy String}")]`ポリシーを指定します。

`[EnableCors]`に属性を適用できます。

* Razor ページ `PageModel`
* コントローラー
* コント ローラー アクション メソッド

コント ローラー/ページ-モデル/アクションとするさまざまなポリシーを適用することができます、`[EnableCors]`属性。 ときに、`[EnableCors]`属性をコント ローラー/ページ-モデル/アクション メソッドに適用されます、ミドルウェアで CORS が有効になっていると、両方のポリシーが適用されます。 ポリシーを組み合わせることをお勧めします。 使用して、`[EnableCors]`属性または両方で同じアプリでは、ミドルウェア。

次のコードでは、各メソッドに別のポリシーが適用されます。

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

次のコードは、CORS の既定のポリシーとという名前のポリシーを作成します`"AnotherPolicy"`:。

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>CORS を無効にします。

[ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)属性は、コント ローラー/ページ-モデル/アクションに対して CORS を無効になります。

<a name="cpo"></a>

## <a name="cors-policy-options"></a>CORS ポリシー オプション

このセクションでは、CORS ポリシーで設定できるさまざまなオプションについて説明します。

* [許可されるオリジンを設定します。](#set-the-allowed-origins)
* [許可される HTTP メソッドを設定します。](#set-the-allowed-http-methods)
* [許可されている要求ヘッダーを設定します。](#set-the-allowed-request-headers)
* [公開されている応答ヘッダーを設定します。](#set-the-exposed-response-headers)
* [クロス オリジン要求で資格情報](#credentials-in-cross-origin-requests)
* [プレフライトの有効期限を設定します。](#set-the-preflight-expiration-time)

 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> 呼び出される`Startup.ConfigureServices`します。 いくつかのオプションの読み取りをすると役立つ場合があります、 [CORS ではどのように動作](#how-cors)最初のセクションします。

## <a name="set-the-allowed-origins"></a>許可されるオリジンを設定します。

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; 任意のスキームですべてのオリジンからの CORS 要求を許可 (`http`または`https`)。 `AllowAnyOrigin` 安全でないため、*任意の web サイト*をアプリにクロスオリジン要求を行うことができます。

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > 指定する`AllowAnyOrigin`と`AllowCredentials`構成は安全でないと、クロスサイト リクエスト フォージェリで発生することができます。 CORS サービスは、アプリが両方の方法で構成されている場合、CORS の無効な応答を返します。

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > 指定する`AllowAnyOrigin`と`AllowCredentials`構成は安全でないと、クロスサイト リクエスト フォージェリで発生することができます。 セキュリティで保護されたアプリでは、クライアントを承認する必要があります自体と、サーバー リソースにアクセスする場合のオリジンの正確なリストを指定します。

  ::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**. 
-->

  `AllowAnyOrigin` プリフライト要求の影響と`Access-Control-Allow-Origin`ヘッダー。 詳細については、次を参照してください。、[プレフライト要求](#preflight-requests)セクション。

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; セット、<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>配信元が許可されている場合に評価するときに構成されているワイルドカード ドメインに一致するオリジンを許可する関数として、ポリシーのプロパティ。

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>許可される HTTP メソッドを設定します。

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* 任意の HTTP メソッドを使用できます。
* プリフライト要求の影響と`Access-Control-Allow-Methods`ヘッダー。 詳細については、次を参照してください。、[プレフライト要求](#preflight-requests)セクション。

### <a name="set-the-allowed-request-headers"></a>許可されている要求ヘッダーを設定します。

CORS 要求で送信される特定のヘッダーを許可するという*要求ヘッダーを作成する*、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>し、許可されたヘッダーを指定します。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

許可するのには、すべての著者要求ヘッダー、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

この設定に影響を与えますプレフライト要求と`Access-Control-Request-Headers`ヘッダー。 詳細については、次を参照してください。、[プレフライト要求](#preflight-requests)セクション。

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

既定では、ブラウザーはすべてのアプリに応答ヘッダーで公開されません。 詳細については、次を参照してください。 [W3C のクロス オリジン リソース共有 (用語集)。単純な応答ヘッダー](https://www.w3.org/TR/cors/#simple-response-header)します。

既定で使用できる応答ヘッダーは次のとおりです。

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

CORS の仕様は、これらのヘッダーを呼び出す*単純な応答ヘッダー*します。 で他のヘッダーをアプリに使用できるように呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>クロス オリジン要求で資格情報

資格情報では、CORS 要求で特別な処理が必要です。 既定では、ブラウザーは、クロス オリジン要求に資格情報を送信しません。 Cookie および HTTP 認証方式は、資格情報が含まれます。 クロス オリジン要求に資格情報を送信するクライアントを設定する必要があります`XMLHttpRequest.withCredentials`に`true`します。

使用して`XMLHttpRequest`直接。

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

JQuery を使用します。

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

使用して、 [API フェッチ](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

サーバーは、資格情報を許可する必要があります。 クロス オリジンの資格情報を許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

HTTP 応答が含まれる、`Access-Control-Allow-Credentials`ヘッダーで、サーバーでクロス オリジン要求の資格情報は、ブラウザーに指示します。

ブラウザーが資格情報を送信しますが、有効な応答を含まない`Access-Control-Allow-Credentials`ヘッダー、ブラウザーは、アプリへの応答を公開しないし、クロス オリジン要求は失敗します。

クロス オリジンの資格情報がセキュリティ上のリスクです。 別のドメインに web サイトでは、ユーザーの知識がなくても、ユーザーの代わりに、アプリにサインイン済みのユーザーの資格情報を送信できます。 <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

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

* `Access-Control-Request-Method`:実際の要求に使用される HTTP メソッド。
* `Access-Control-Request-Headers`:実際の要求にアプリを設定する要求ヘッダーの一覧。 前述のように、ブラウザー設定などのヘッダーは含まれません`User-Agent`します。

CORS プレフライト要求を含めることができます、`Access-Control-Request-Headers`ヘッダーで、サーバーの実際の要求で送信されるヘッダーを示します。

特定のヘッダーを許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

許可するのには、すべての著者要求ヘッダー、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

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

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>CORS のしくみ

このセクションでの動作について説明します、 [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) HTTP メッセージのレベルで要求します。

* CORS は**いない**セキュリティ機能。 CORS は、サーバーによる同一オリジン ポリシーの緩和にできる W3C 標準です。
  * たとえば、悪意のあるアクターを使用[ようにクロス サイト スクリプティング (XSS)](xref:security/cross-site-scripting)サイトに対して、CORS が有効になっているサイトに情報の盗用のサイト間で要求を実行します。
* CORS を許可することで、API をより安全なすることはできません。
  * クライアント (ブラウザー) に CORS を適用します。 サーバーが要求を実行し、応答を返します、それがクライアントでエラーとブロックの応答を返します。 たとえば、次のツールのいずれかのサーバーの応答が表示されます。
     * [Fiddler](https://www.telerik.com/fiddler)
     * [Postman](https://www.getpostman.com/)
     * [.NET HttpClient](/dotnet/csharp/tutorials/console-webapiclient)
     * アドレス バーに URL を入力して、web ブラウザー。
* クロス オリジンを実行するには、サーバーがブラウザーを許可する方法は[XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)または[フェッチ API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)それ以外の場合、禁止されるよう要求します。
  * (CORS) なしのブラウザーでは、クロス オリジン要求を実行できません。 CORS をする前に[JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)この制限を回避するために使用されました。 JSONP は、XHR を使用していないを使用して、`<script>`応答を受信するタグ。 スクリプトが読み込まれたクロス オリジンが許可されます。

[CORS の仕様]()のクロス オリジン要求を有効にするいくつかの新しい HTTP ヘッダーが導入されました。 ブラウザーでは、CORS をサポートする場合は、クロス オリジン要求を自動的にこれらのヘッダーを設定します。 カスタム JavaScript コードは、CORS を有効にする必要はありません。

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

<a name="test"></a>

## <a name="test-cors"></a>CORS をテストします。

CORS をテストします。

1. [API プロジェクトを作成](xref:tutorials/first-web-api)です。 または、[サンプルをダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors)します。
1. このドキュメントで、アプローチの 1 つを使用して、CORS を有効にします。 例:

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]
  
  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` ようなサンプル アプリをテストするためにのみ使用する必要があります、[サンプル コードをダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors)します。

1. (Razor ページまたは MVC) web アプリ プロジェクトを作成します。 このサンプルでは、Razor ページを使用します。 API プロジェクトと同じソリューションでは、web アプリを作成することができます。
1. 次の強調表示されたコードを追加、 *Index.cshtml*ファイル。

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. 上記のコードで置き換えます`url: 'https://<web app>.azurewebsites.net/api/values/1',`でデプロイされたアプリの URL。
1. API プロジェクトをデプロイします。 たとえば、[を Azure にデプロイ](xref:host-and-deploy/azure-apps/index)します。
1. デスクトップから、Razor ページまたは MVC アプリを実行し、をクリックして、**テスト**ボタンをクリックします。 F12 ツールを使用して、エラー メッセージを確認します。
1. Localhost 原点からの削除`WithOrigins`と、アプリをデプロイします。 または、別のポートでクライアント アプリを実行します。 たとえば、Visual Studio から実行します。
1. クライアント アプリをテストします。 CORS の障害には、エラーが返されますが、エラー メッセージは JavaScript を使用できません。 F12 ツールで、[コンソール] タブを使用して、エラーを参照してください。 ブラウザーによっては、次のような (F12 ツールのコンソール) で、エラーが発生します。

  * Microsoft Edge の使用。

    **SEC7120: [CORS]、配信元 'https://localhost:44375'が見つかりませんでした'https://localhost:44375でクロス オリジン リソースへのアクセス制御の許可-オリジン応答ヘッダー' で'https://webapi.azurewebsites.net/api/values/1'。**

  * Chrome を使用します。

    **XMLHttpRequest へのアクセス 'https://webapi.azurewebsites.net/api/values/1'から配信元'https://localhost:44375' CORS ポリシーによってブロックされています。'へのアクセス制御の許可-オリジン' ヘッダーが要求されたリソースに存在しません。**

## <a name="additional-resources"></a>その他の技術情報

* [クロス オリジン リソース共有 (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
