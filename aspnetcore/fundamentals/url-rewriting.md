---
title: "URL の ASP.NET Core のミドルウェアの書き換え"
author: guardrex
description: "URL 書き換えおよび ASP.NET Core アプリケーションの URL 書き換えミドルウェアにリダイレクトすることについて説明します。"
keywords: "ASP.NET Core、URL 書き換え、URL 書き換え URL をリダイレクトする、URL リダイレクト、ミドルウェア apache_mod"
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: dde0b5673c9885db2fecbb24b384752e5ddf70eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>URL の ASP.NET Core のミドルウェアの書き換え

によって[Luke Latham](https://github.com/guardrex)と[Mikael メンギストゥ](https://github.com/mikaelm12)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

URL の書き換えは 1 つまたは複数の事前定義された規則に基づいて、Url 要求を変更することです。 URL 書き換え、場所とアドレスが緊密にリンクされていないように、リソースの場所とその住所間の抽象化を作成します。 URL 書き換えが役に立ちますいくつかのシナリオはあります。
* 移動またはそれらのリソースの安定したロケーターを維持しながら、サーバー リソースを一時的または永続的に交換
* 要求処理でアプリを 1 つの区分にわたってまたは別のアプリ間での分割
* 削除、追加、または受信した要求の URL セグメントを再編成
* 検索エンジン最適化 (SEO) のパブリック Url を最適化します。
* 次のリンクを検索できるが、コンテンツの予測を手助けするフレンドリなパブリック Url の使用を許可します。
* エンドポイントをセキュリティで保護するセキュリティ保護されていない要求をリダイレクトします。
* イメージ hotlinking の防止

いくつかの方法で URL を変更する regex、Apache mod_rewrite モジュール ルール、IIS の書き換え規則など、カスタムの規則のロジックを使用するための規則を定義できます。 このドキュメントでは、ASP.NET Core アプリケーションで URL 書き換えミドルウェアを使用する方法について記載された URL の書き換えが導入されています。

> [!NOTE]
> URL の書き換えと、アプリのパフォーマンスが低下することができます。 可能であれば、規則の複雑さと数を制限する必要があります。

## <a name="url-redirect-and-url-rewrite"></a>URL リダイレクトと URL の書き換え
表現の間に違い*URL リダイレクト*と*URL 書き換え*に微妙なように見えますが最初のクライアントにリソースを提供するための重要な意味を含んでいます。 ASP.NET Core URL 書き換えミドルウェアは、両方のニーズを満たせるです。

A *URL リダイレクト*クライアント側の操作は、クライアントが別のアドレスにあるリソースにアクセスするよう指示します。 サーバーへのラウンド トリップが必要ですされ、クライアントが、リソースに対する新しい要求を行うときに、ブラウザーのアドレス バーに、クライアントに返され、リダイレクト URL が表示されます。 場合`/resource`は*リダイレクト*に`/different-resource`、クライアントからの要求`/resource`、サーバーは、クライアントがリソースをリソースを取得する必要があります、応答と`/different-resource`リダイレクトがであることを示すステータス コードで一時的または永続的です。 クライアントは、リダイレクト URL にあるリソースに対する新しい要求を実行します。

![WebAPI サービス エンドポイントは、サーバー上のバージョン 2 (v2) をバージョン 1 (v1) から一時的に変更されましたが。 クライアントは、バージョン 1 パス/v1/api のサービスに要求がします。 サーバー応答を返信 302 (検出)、サービスの新しい、一時的なパスとバージョン 2/v2/api にします。 クライアントは、2 番目の要求をリダイレクト URL のサービスには。 サーバーは、200 (OK) 状態コードを返します。](url-rewriting/_static/url_redirect.png)

要求を別の URL にリダイレクトする場合は、リダイレクトが永続的または一時的であるかどうかを示します。 301 (完全な移動) 状態コードには、リソースのすべての要求が新しい URL を使用する必要がありますをクライアントに指示する、新しい永続的な URL は、リソースは、するは使用されます。 *301 ステータス コードを受信すると、クライアントは応答をキャッシュ可能性があります。* 302 (検出) ステータス コードを使用、リダイレクトが一時的または一般にサブジェクトが変更するべきではありません、クライアントの格納し、リダイレクト URL を後で再になるようにします。 詳細については、次を参照してください。 [RFC 2616: 状態コード定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)です。

A *URL 書き換え*はサーバー側の操作を別のリソースのアドレスからのリソースを提供します。 URL の書き換えと、サーバーへのラウンド トリップが必要としません。 書き換えられた URL では、クライアントに返されません。 と、ブラウザーのアドレス バーに表示されません。 ときに`/resource`は*書き直す*に`/different-resource`、クライアントからの要求`/resource`とサーバー*内部的に*にあるリソースをフェッチ`/different-resource`です。 クライアントは、書き換えられた URL にあるリソースを取得できる場合がありますがその要求を行う応答を受信して、ときに、書き換えられた URL にリソースが存在するクライアントが通知されません。

![WebAPI サービス エンドポイントが、サーバー上のバージョン 2 (v2) をバージョン 1 (v1) から変更されました。 クライアントは、バージョン 1 パス/v1/api のサービスに要求がします。 要求 URL は、バージョン 2 パス/v2/api でサービスへのアクセスに再度書き込まれます。 サービスは、200 (OK) ステータス コードでクライアントに応答します。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>URL 書き換えのサンプル アプリ
URL 書き換えミドルウェアの機能を調べることができます、 [URL 書き換えのサンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)です。 アプリが書き換えを適用し、リダイレクト ルールし、書き換えられたかリダイレクトされる URL を表示します。

## <a name="when-to-use-url-rewriting-middleware"></a>URL 書き換えミドルウェアを使用する場合
使用できない場合は、URL 書き換えミドルウェアを使用して、 [URL Rewrite モジュール](https://www.iis.net/downloads/microsoft/url-rewrite)Windows Server 上の IIS で、 [Apache mod_rewrite モジュール](https://httpd.apache.org/docs/2.4/rewrite/)Apache サーバーで、 [NginxでURLの書き換え](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)でホストされているアプリまたは[HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (旧称[WebListener](xref:fundamentals/servers/weblistener))。 テクノロジを使用して、サーバー ベース URL 書き換え IIS、Apache、または Nginx の主な理由ではこれらのモジュールの全機能をサポートしていないミドルウェア ミドルウェアのパフォーマンス可能性とも一致しません、モジュールの。 ただし、ASP.NET Core プロジェクトなどを操作しないサーバー モジュールの一部の機能がある、`IsFile`と`IsDirectory`IIS Rewrite モジュールの制約。 これらのシナリオでは、代わりに、ミドルウェアを使用します。

## <a name="package"></a>Package
ミドルウェアをプロジェクトに含めるへの参照を追加、 [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/)パッケージです。 この機能は、ASP.NET Core 1.1 を対象とするアプリの使用可能な以降です。

## <a name="extension-and-options"></a>拡張機能とオプション
URL 書き換えを確立しのインスタンスを作成することでルールをリダイレクト、`RewriteOptions`クラスでは、ルールの各拡張メソッド。 ようにした場合に処理された順序で複数のルールを連結します。 `RewriteOptions`と要求パイプラインに追加されるように、URL 書き換えミドルウェアに渡される`app.UseRewriter(options);`です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a>URL リダイレクト
使用して`AddRedirect`要求をリダイレクトします。 最初のパラメーターには、着信 URL のパスに一致する正規表現が含まれています。 2 番目のパラメーターは、置換文字列です。 3 番目のパラメーターでは、存在する場合は、ステータス コードを指定します。 ステータス コードを指定しない場合、既定値 302 (検出)、リソースが一時的に移動または置き換えることを示すです。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

ブラウザーで有効になっている開発者ツールで、パスのサンプル アプリに要求を行う`/redirect-rule/1234/5678`です。 正規表現の要求パスに一致する`redirect-rule/(.*)`、パスが置き換え`/redirected/1234/5678`です。 リダイレクト URL は 302 (検出) のステータス コードでクライアントに送信されます。 ブラウザーでは、新しい要求をブラウザーのアドレス バーに表示されるリダイレクト URL でです。 2 番目の要求は、アプリから 200 (OK) 応答を受信し、リダイレクト URL のサンプル アプリの規則が一致しないため、応答の本体はリダイレクト URL が表示できます。 URL がサーバーとのやり取りが行われた*リダイレクト*です。

> [!WARNING]
> リダイレクト ルールを確立するときに注意します。 リダイレクト後など、アプリへの各要求では、リダイレクト ルールが評価されます。 簡単に誤って無限のリダイレクトのループを作成します。

元の要求:`/redirect-rule/1234/5678`

![ブラウザー ウィンドウで、要求および応答を追跡開発者ツール](url-rewriting/_static/add_redirect.png)

かっこ内に含まれる式の一部と呼ばれる、*キャプチャ グループ*です。 ドット (`.`) 式の意味*任意の文字を一致*です。 アスタリスク (`*`) を示す*直前の文字に 0 回以上一致*です。 URL の最後の 2 つのパス セグメントではそのため、 `1234/5678`、キャプチャ グループによってキャプチャされた`(.*)`です。 任意の値の後に、要求 URL で指定する`redirect-rule/`はこの単一のキャプチャ グループによってキャプチャされます。

置換文字列にキャプチャされたグループが挿入され、ドル記号の文字列 (`$`) に続けてのキャプチャのシーケンス番号。 最初のキャプチャ グループ値を取得`$1`、2 番目は`$2`の正規表現内のキャプチャ グループのシーケンスで続行されます。 1 つだけのキャプチャ グループにあるリダイレクト ルール regex サンプル アプリでは置換文字列に 1 つだけの挿入されたグループがあるため`$1`です。 ルールが適用されるときに、URL は次のようになります。`/redirected/1234/5678`です。

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a>セキュリティで保護されたエンドポイントへの URL リダイレクト
使用して`AddRedirectToHttps`HTTP 要求を同じホストと HTTPS を使用して、パスにリダイレクト (`https://`)。 ステータス コードが指定されていない場合、ミドルウェアを 302 (検出) に既定値です。 ポートが指定されていない場合は、ミドルウェア既定`null`、つまり、プロトコルに変更`https://`クライアント ポート 443 上のリソースにアクセスするとします。 この例では、301 (完全な移動) に、状態コードを設定し、5001 にポートを変更する方法を示します。
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
使用して`AddRedirectToHttpsPermanent`同じホストのパスとセキュリティで保護された HTTPS プロトコルを使用するセキュリティ保護されていない要求をリダイレクトする (`https://`ポート 443)。 ミドルウェアは、ステータス コードを 301 (完全な移動) に設定します。

サンプル アプリが使用する方法を示す可能`AddRedirectToHttps`または`AddRedirectToHttpsPermanent`です。 拡張メソッドを追加、`RewriteOptions`です。 任意の URL にアプリをセキュリティ保護されていない要求を行います。 自己署名証明書が信頼できることを警告するブラウザーのセキュリティを消去します。

元の要求を使用して`AddRedirectToHttps(301, 5001)`:`/secure`

![ブラウザー ウィンドウで、要求および応答を追跡開発者ツール](url-rewriting/_static/add_redirect_to_https.png)

元の要求を使用して`AddRedirectToHttpsPermanent`:`/secure`

![ブラウザー ウィンドウで、要求および応答を追跡開発者ツール](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>URL 書き換え
使用して`AddRewrite`Url の書き換えの規則を作成します。 最初のパラメーターには、着信 URL パスに一致する正規表現が含まれています。 2 番目のパラメーターは、置換文字列です。 3 番目のパラメーターでは、`skipRemainingRules: {true|false}`ミドルウェアに示す現在のルールが適用されている場合は、追加の書き換えルールをスキップするかどうかを指定します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

元の要求:`/rewrite-rule/1234/5678`

![ブラウザー ウィンドウで、要求と応答を追跡開発者ツール](url-rewriting/_static/add_rewrite.png)

最初、正規表現で注目するは、キャレット (`^`) 式の先頭にします。 これは、URL パスの先頭で一致が開始されることを意味します。

リダイレクト ルールでは、先ほどの例で`redirect-rule/(.*)`、正規表現の先頭のキャレットはありません。 したがって、任意の文字の前可能性があります`redirect-rule/`に成功した一致のパスにします。

| パス                               | 一致したもの |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | はい   |
| `/my-cool-redirect-rule/1234/5678` | はい   |
| `/anotherredirect-rule/1234/5678`  | はい   |

書き換えルール`^rewrite-rule/(\d+)/(\d+)`、のみで開始される場合、パスと一致する`rewrite-rule/`です。 書き換えルールの下と上のリダイレクト ルール間の一致の違いに注意してください。

| パス                              | 一致したもの |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | はい   |
| `/my-cool-rewrite-rule/1234/5678` | いいえ    |
| `/anotherrewrite-rule/1234/5678`  | いいえ    |

次の`^rewrite-rule/`部分式の 2 つのキャプチャ グループが`(\d+)/(\d+)`です。 `\d`ことを示します*digit (数) を一致*です。 プラス記号 (`+`) ことを意味*直前の文字の 1 つ以上の一致*です。 そのため、URL では、スラッシュを他の数値の後に続く数値を含める必要があります。 これらのキャプチャ グループが挿入され、書き換えられた URL として`$1`と`$2`です。 書き換えルールの置換文字列は、クエリ文字列に、キャプチャされたグループを配置します。 要求されたパスの`/rewrite-rule/1234/5678`にあるリソースを取得する書き直す`/rewritten?var1=1234&var2=5678`です。 クエリ文字列が元の要求に存在する場合、URL は再作成時に保持します。

リソースを取得するためにサーバーとのやり取りはありません。 リソースが存在する場合は、フェッチされ、200 (OK) ステータス コードをクライアントに返されることがされます。 クライアントがリダイレクトされていないために、ブラウザーのアドレス バーの URL は変更されません。 クライアントに関しては、限りことはありません、URL 書き換え操作が発生しました。

> [!NOTE]
> 使用して`skipRemainingRules: true`可能な限り、コストのかかるプロセスは、アプリの応答時間を短縮の規則に一致するためです。 最も高速のアプリの応答。
> * 最もよく一致するルールを最も頻繁に一致したルールから、書き換えルールを並べ替えます。
> * 一致が発生し、その他のルールの処理は必要ありません、残りのルールの処理をスキップします。

### <a name="apache-modrewrite"></a>Apache mod_rewrite
Apache mod_rewrite ルールを適用`AddApacheModRewrite`です。 アプリの規則ファイルが展開されていることを確認します。 詳細と mod_rewrite 規則の例については、次を参照してください。 [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

A`StreamReader`からルールの読み取りに使用される、 *ApacheModRewrite.txt*ルール ファイルです。

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

最初のパラメーターには、 `IFileProvider`、経由で提供されています[依存性の注入](dependency-injection.md)です。 `IHostingEnvironment`を提供する挿入、`ContentRootFileProvider`です。 2 番目のパラメーターは、ファイルへのパスの規則、これは*ApacheModRewrite.txt*サンプル アプリでします。

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

サンプル アプリからの要求をリダイレクトする`/apache-mod-rules-redirect/(.\*)`に`/redirected?id=$1`です。 応答状態コードは、302 (検出) です。

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

元の要求:`/apache-mod-rules-redirect/1234`

![ブラウザー ウィンドウで、要求および応答を追跡開発者ツール](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>サポートされているサーバー変数
ミドルウェアは、次の Apache mod_rewrite サーバー変数をサポートします。
* CONN_REMOTE_ADDR
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_FORWARDED
* HTTP_HOST
* ボタン
* HTTP_USER_AGENT
* HTTPS
* IPV6
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_METHOD
* REQUEST_SCHEME
* REQUEST_URI
* SCRIPT_FILENAME
* SERVER_ADDR
* これらの用語
* SERVER_PROTOCOL
* 時間
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>IIS URL Rewrite モジュール ルール
IIS URL Rewrite モジュールに適用される規則を使用する`AddIISUrlRewrite`です。 アプリの規則ファイルが展開されていることを確認します。 使用するミドルウェアを直接しない、 *web.config*ファイルの Windows Server IIS で実行されているときにします。 外部で IIS にこれらの規則を保存するか、 *web.config* IIS Rewrite モジュールと競合しないようにします。 詳細と IIS URL Rewrite モジュールの規則の例については、次を参照してください。 [Url Rewrite モジュール 2.0 の使用](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)と[URL 書き換えモジュール構成の参照](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

A`StreamReader`からルールの読み取りに使用される、 *IISUrlRewrite.xml*ルール ファイルです。

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

最初のパラメーターには、 `IFileProvider`、2 番目のパラメーターはこれは、XML ルール ファイルのパスを*IISUrlRewrite.xml*サンプル アプリでします。

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

サンプル アプリからの要求をリライト`/iis-rules-rewrite/(.*)`に`/rewritten?id=$1`です。 応答は、200 (OK) ステータス コードでクライアントに送信されます。

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

元の要求:`/iis-rules-rewrite/1234`

![ブラウザー ウィンドウで、要求と応答を追跡開発者ツール](url-rewriting/_static/add_iis_url_rewrite.png)

、望ましくない方法でアプリに影響するように構成されたサーバー レベル ルールにアクティブな IIS Rewrite モジュールがある場合は、アプリの IIS Rewrite モジュールを無効にできます。 詳細については、次を参照してください。[を無効にする IIS モジュール](xref:hosting/iis-modules#disabling-iis-modules)です。

#### <a name="unsupported-features"></a>サポートされていない機能

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

リリース ミドルウェアと ASP.NET Core 2.x IIS URL Rewrite モジュール機能をサポートしていません。
* 送信の規則
* カスタム サーバー変数
* ワイルドカード
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

リリース ミドルウェアと ASP.NET Core 1.x IIS URL Rewrite モジュール機能をサポートしていません。
* グローバル ルール
* 送信の規則
* マップを書き直してください。
* CustomResponse アクション
* カスタム サーバー変数
* ワイルドカード
* アクション: CustomResponse
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>サポートされているサーバー変数
ミドルウェアは、次の URL Rewrite モジュールを IIS サーバー変数をサポートします。
* 多く
* CONTENT_TYPE
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_HOST
* ボタン
* HTTP_URL
* HTTP_USER_AGENT
* HTTPS
* LOCAL_ADDR
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_URI

> [!NOTE]
> 取得することも、`IFileProvider`を介して、`PhysicalFileProvider`です。 このアプローチは可能性があります規則ファイルを書き換えの場所をより柔軟に提供します。 書き換え規則ファイルを指定したパスにあるサーバーにデプロイされていることを確認してください。
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>メソッド ベースのルール
使用して`Add(Action<RewriteContext> applyRule)`メソッドで、独自の規則ロジックを実装します。 `RewriteContext`公開、`HttpContext`メソッドで使用するためです。 `context.Result`パイプラインを追加する方法を決定する処理が行われます。

| コンテキスト。結果                       | 操作                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (既定値) | ルールの適用を続行します。                                         |
| `RuleResult.EndResponse`             | 規則の適用を停止し、応答を送信                       |
| `RuleResult.SkipRemainingRules`      | ルールの適用を停止し、次のミドルウェアにコンテキストを送信 |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

サンプル アプリで終了するパスの要求をリダイレクトするメソッドに示します*.xml*です。 要求を行う場合`/file.xml`にリダイレクトされます`/xmlfiles/file.xml`です。 ステータス コードが 301 (完全な移動) に設定されます。 リダイレクトは、応答のステータス コード明示的に設定する必要があります。それ以外の場合は 200 (OK) のステータス コードが返され、リダイレクトはクライアントで発生しません。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

元の要求:`/file.xml`

![ブラウザー ウィンドウで、要求および file.xml の応答を追跡開発者ツール](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>IRule ベースのルール
使用して`Add(IRule)`から派生したクラスに、独自の規則ロジックを実装する`IRule`です。 使用して、`IRule`メソッド ベースのルールのアプローチを使用する場合より高い柔軟性が実現します。 派生クラスでのパラメーターに渡すことができます、コンス トラクターを含めることがあります、`ApplyRule`メソッドです。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

用のサンプル アプリケーションでは、パラメーターの値、`extension`と`newPath`をいくつかの条件を満たすためにチェックされます。 `extension` 、値を格納する必要があります値を指定する必要があります*.png*、 *.jpg*、または*.gif*です。 場合、`newPath`が有効でない、`ArgumentException`がスローされます。 要求を行う場合*image.png*にリダイレクトされます`/png-images/image.png`です。 要求を行う場合*image.jpg*にリダイレクトされます`/jpg-images/image.jpg`です。 ステータス コードが 301 (完全な移動) に設定され、`context.Result`規則の処理を停止し、応答の送信に設定されています。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

元の要求:`/image.png`

![ブラウザー ウィンドウで、要求および image.png の応答を追跡開発者ツール](url-rewriting/_static/add_redirect_png_requests.png)

元の要求:`/image.jpg`

![ブラウザー ウィンドウで、要求および image.jpg の応答を追跡開発者ツール](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>正規表現の例

| Goal | Regex 文字列 (& a)<br>一致の例 | 置換後の文字列 (& a)<br>出力例 |
| ---- | :-----------------------------: | :------------------------------------: |
| クエリ文字列にパスを書き直してください。 | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| 末尾のスラッシュを除去します。 | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| 末尾のスラッシュを適用します。 | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| 特定の要求を再作成を回避します。 | `(.*[^(\.axd)])$`<br>うん：`/resource.htm`<br>違います：`/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| URL セグメントを再配置します。 | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| URL セグメントを置き換えます | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>その他の技術情報
* [アプリケーションの起動](startup.md)
* [ミドルウェア](middleware.md)
* [.NET の正規表現](/dotnet/articles/standard/base-types/regular-expressions)
* [正規表現言語 - クイック リファレンス](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [(IIS) の Url 書き換えモジュール 2.0 を使用してください。](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [URL 書き換えモジュール構成のリファレンス](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [IIS URL 書き換えモジュール フォーラム](https://forums.iis.net/1152.aspx)
* [単純な URL 構造を保持します。](https://support.google.com/webmasters/answer/76329?hl=en)
* [10 URL 書き換えヒントし、テクニック](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [スラッシュまたは円記号が](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
