---
title: "ASP.NET Core の URL リライト ミドルウェア"
author: guardrex
description: "ASP.NET Core アプリケーションの URL リライト ミドルウェアを使用した URL の書き換えとリダイレクトについて説明します。"
manager: wpickett
ms.author: riande
ms.date: 08/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/url-rewriting
ms.openlocfilehash: ca1f2f366bcf12cd3df83c3cdefa460cb9a68e2a
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2018
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>ASP.NET Core の URL リライト ミドルウェア

作成者: [Luke Latham](https://github.com/guardrex) および [Mikael Mengistu](https://github.com/mikaelm12)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

URL の書き換えは、1 つまたは複数の事前定義された規則に基づいて URL 要求を変更する操作です。 URL 書き換えは、リソースの場所とそれらのアドレスの間の抽象化を作成し、場所とアドレスが緊密にリンクされていないようにします。 URL 書き換えが役に立ついくつかのシナリオがあります。
* サーバー リソースの安定したロケーターを維持しながらそれらのリソースを一時的または永続的に移動するか置き換える
* 要求処理を異なる複数のアプリまたは 1 つのアプリの複数の区分にわたって分割する
* 受信した要求の URL セグメントを削除、追加、または再編成する
* 検索エンジン最適化 (SEO) のためにパブリック URL を最適化する
* リンクをたどることで見つかるコンテンツをユーザーが予測しやすくするフレンドリなパブリック URL の使用を許可する
* セキュリティで保護されていない要求をセキュリティで保護されたエンドポイントにリダイレクトする
* イメージのホットリンクを防止する

正規表現、Apache mod_rewrite モジュール ルール、IIS Rewrite Module ルール、カスタム ルール ロジックの使用など、いくつかの方法で URL を変更するルールを定義することができます。 このドキュメントでは、ASP.NET Core アプリの URL リライト ミドルウェアを使用して URL を書き換える手順について説明します。

> [!NOTE]
> URL の書き換えによってアプリのパフォーマンスが低下することがあります。 可能であれば、ルールの複雑さと数を制限する必要があります。

## <a name="url-redirect-and-url-rewrite"></a>URL リダイレクトと URL 書き換え
*URL リダイレクト*と *URL 書き換え*の表現の違いは、最初はわずかに見えるかもしれませんが、クライアントへのリソースの提供に関しては重要な意味を持っています。 ASP.NET Core の URL リライト ミドルウェアは、両方のニーズを満たすことができます。

*URL リダイレクト*は、クライアントに別のアドレスにあるリソースにアクセスするよう指示するクライアント側の操作です。 これによりサーバーへのラウンドトリップが必要になります。 クライアントが、リソースの新しい要求を実行するときに、クライアントに返されたリダイレクト URL がブラウザーのアドレス バーに表示されます。 

`/resource` が `/different-resource` に*リダイレクト*される場合、クライアントは `/resource` を要求します。 サーバーは、リダイレクトが一時的または永続的のどちらかを示すステータスコードと共に応答し、`/different-resource` にあるリソースをクライアントが取得する必要があることを示します。 クライアントは、リダイレクト URL にあるリソースに対する新しい要求を実行します。

![WebAPI サービス エンドポイントは、サーバー上でバージョン 1 (v1) からバージョン 2 (v2) に一時的に変更されました。 クライアントは、バージョン 1 パス /v1/api のサービスに対して要求を実行します。 サーバーは、バージョン 2/v2/api のサービスの新しい一時的なパスを使用して 302 (検出) 応答を返します。 クライアントは、リダイレクト URL のサービスに対して 2 回目の要求を実行します。 サーバーは、200 (OK) 状態コードで応答します。](url-rewriting/_static/url_redirect.png)

要求を別の URL にリダイレクトする場合は、リダイレクトが永続的または一時的のどちらかを示します。 301 (永続的な移動) 状態コードは、リソースに新しい永続的な URL があり、リソースに対する将来のすべての要求で新しい URL を使用する必要があることをクライアントに指示したい場合に使用します。 *301 状態コードを受信すると、クライアントは応答をキャッシュに入れる場合があります。* 302 (検出) 状態コードは、リダイレクトが一時的または一般的に変更される場合に使用され、クライアントがリダイレクト URL を保存して再利用しないようにします。 詳細については、「[RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)」(RFC 2616: ステータスコードの定義) を参照してください。

*URL 書き換え*は、別のリソースのアドレスからのリソースを提供するサーバー側の操作です。 URL の書き換えには、サーバーへのラウンドトリップは必要ありません。 書き換えられた URL は、クライアントに返されず、ブラウザーのアドレス バーに表示されません。 `/resource` が `/different-resource` に*書き換えられた*場合、クライアントは `/resource` を要求し、サーバーは `/different-resource` にあるリソースを*内部的に*取得します。 クライアントは、書き換えられた URL にあるリソースを取得できる場合がありますが、クライアントは、その要求を実行して応答を受信したときに、書き換えられた URL にリソースが存在することを通知されません。

![WebAPI サービス エンドポイントは、サーバー上でバージョン 1 (v1) からバージョン 2 (v2) に変更されました。 クライアントは、バージョン 1 パス /v1/api のサービスに対して要求を実行します。 要求 URL は、バージョン 2 パス/v2/api でサービスにアクセスするように再度書き込まれます。 サービスは、200 (OK) 状態コードでクライアントに応答します。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>URL リライト サンプル アプリ
[URL リライト サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)を使用して、URL リライト ミドルウェアの機能を調べることができます。 アプリが書き換えとリダイレクトのルールを適用し、書き換えおよびリダイレクトが実行された URL を表示します。

## <a name="when-to-use-url-rewriting-middleware"></a>URL リライト ミドルウェアを使用する状況
URL リライト ミドルウェアは、Windows サーバー上の IIS の [URL リライト モジュール](https://www.iis.net/downloads/microsoft/url-rewrite)と、Apache サーバー上の [Apache mod_rewrite モジュール](https://httpd.apache.org/docs/2.4/rewrite/)、[Nginx 上の URL 書き換え](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)を使用できない場合、またはアプリが [HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (旧称 [WebListener](xref:fundamentals/servers/weblistener)) 上でホストされている場合に使用します。 IIS、Apache、Nginx でサーバー ベースの URL 書き換テクノロジを使用する主な理由は、ミドルウェアがこれらのモジュールの完全な機能をサポートせず、ミドルウェアのパフォーマンスがモジュールのパフォーマンスと一致しないことです。 ただし、IIS リライト モジュールの `IsFile` 制約や `IsDirectory` 制約など、ASP.NET Core プロジェクトと連携しないサーバー モジュールのいくつかの機能があります。 これらのシナリオでは、代わりにミドルウェアを使用します。

## <a name="package"></a>Package
ミドルウェアをプロジェクトに含めるには、[ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/)パッケージへの参照を追加します。 この機能は、ASP.NET Core 1.1.0 以上をターゲットとするアプリで使用できます。

## <a name="extension-and-options"></a>拡張機能とオプション
各ルールの拡張メソッドを使用して、`RewriteOptions` クラスのインスタンスを作成することによって、URL 書き換えとリダイレクトを確立します。 処理したい順序で複数のルールを連結します。 `RewriteOptions` は、`app.UseRewriter(options);` を使用して要求パイプラインに追加されたときに、URL リライト ミドルウェアに渡されます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a>URL リダイレクト
要求をリダイレクトするには、`AddRedirect` を使用します。 最初のパラメーターには、着信 URL のパスに一致する正規表現が含まれています。 2 番目のパラメーターは、置換文字列です。 3 番目のパラメーター (存在する場合) は、状態コードを指定します。 状態コードを指定しない場合、既定値である 302 (検出) に設定されます。これは、リソースが一時的に移動されるか置き換えられることを示します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

開発者ツールが有効になっているブラウザーで、パス `/redirect-rule/1234/5678` を指定してサンプル アプリに対する要求を実行します。 正規表現が `redirect-rule/(.*)` の要求のパスに一致し、パスが `/redirected/1234/5678` に置き換えられます。 リダイレクト URL が、302 (検出) の状態コードでクライアントに返されます。 ブラウザーは、ブラウザーのアドレス バーに表示されるリダイレクト URL に新しい要求を実行します サンプル アプリにはリダイレクト URL に一致するルールがないので、2 番目の要求は、アプリから 200 (OK) 応答を受信し、応答の本文にリダイレクト URL が表示されます。 URL が*リダイレクト*されるときにサーバーへのラウンドトリップが実行されます。

> [!WARNING]
> リダイレクト ルールを確立するときには注意してください。 リダイレクト ルールは、リダイレクト後を含めて、アプリへの各要求で評価されます。 無限リダイレクトのループを誤って作成することがよくあります。

元の要求: `/redirect-rule/1234/5678`

![要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_redirect.png)

かっこ内に含まれる式の一部は、*キャプチャ グループ*と呼ばれます。 式のドット (`.`) は、*どの文字とも一致する*ことを意味します。 アスタリスク (`*`) は、*直前の文字に 0 回以上一致する*ことを示します。 そのため、URL の最後の 2 つのパス セグメント `1234/5678` は、キャプチャ グループ `(.*)` によってキャプチャされます。 要求 URL で `redirect-rule/` の後に指定した任意の値が、この単一のキャプチャ グループによってキャプチャされます。

置換文字列では、キャプチャされたグループがドル記号 (`$`) を使用して文字列に挿入され、その後にキャプチャのシーケンス番号が続きます。 最初のキャプチャ グループ値は、`$1` で取得され、2 番目は `$2` で取得され、これらは正規表現内のキャプチャ グループの順序で続行されます。 サンプル アプリでは、リダイレクト ルールの正規表現内に 1 つのキャプチャ グループだけがあるので、置換文字列に 1 つの挿入されたグループ (`$1`) だけがあります。 ルールが適用されるときに、URL `/redirected/1234/5678` になります。

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a>セキュリティで保護されたエンドポイントへの URL リダイレクト
`AddRedirectToHttps` を使用して HTTP 要求を、HTTPS (`https://`) を使用する同じホストとパスにリダイレクトします。 状態コードが指定されていない場合、ミドルウェアは既定で 302 (検出) に設定します。 ポートが指定されていない場合は、ミドルウェアは既定で `null` に設定します。つまり、プロトコルを `https://` に変更し、クライアントはポート 443 上でリソースにアクセスします。 この例では、状態コードを 301 (完全な移動) に設定し、ポートを 5001 に変更する方法を示します。
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
`AddRedirectToHttpsPermanent` を使用してセキュリティで保護されていない要求を、セキュリティで保護された HTTPS プロトコル (ポート 443 上の `https://`) を使用する同じホストとパスにリダイレクトします。 ミドルウェアは状態コードを 301 (完全な移動) に設定します。

サンプル アプリは、`AddRedirectToHttps` または `AddRedirectToHttpsPermanent` の使用方法の例を示すことができます。 拡張メソッドを `RewriteOptions` に追加します。 任意の URL でセキュリティ保護されていない要求をアプリに対して行います。 自己署名証明書が信頼できないことを示すブラウザーのセキュリティ警告を消去します。

`AddRedirectToHttps(301, 5001)`: `/secure` を使用する元の要求

![要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_redirect_to_https.png)

`AddRedirectToHttpsPermanent`: `/secure` を使用する元の要求

![要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>URL 書き換え
URL 書き換えのルールを作成するには、`AddRewrite` を使用します。 最初のパラメーターには、着信 URL のパスに一致する正規表現が含まれています。 2 番目のパラメーターは、置換文字列です。 3 番目のパラメーター `skipRemainingRules: {true|false}` は、現在のルールが適用される場合に追加の書き換えルールをスキップするかどうかをミドルウェアに示します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

元の要求: `/rewrite-rule/1234/5678`

![要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_rewrite.png)

正規表現で最初に注目するのは、式の先頭にあるキャレット (`^`) です。 これは、URL パスの先頭からマッチングが開始されることを意味します。

リダイレクト ルール `redirect-rule/(.*)` を使用する先ほどの例では、正規表現の先頭のキャレットはありません。したがって、パス内で `redirect-rule/` の前にある任意の文字が一致します。

| パス                               | 一致したもの |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | [はい]   |
| `/my-cool-redirect-rule/1234/5678` | [はい]   |
| `/anotherredirect-rule/1234/5678`  | [はい]   |

書き換えルール `^rewrite-rule/(\d+)/(\d+)` は、`rewrite-rule/` で始まる場合のみパスと一致します。 下の書き換えルールの上のリダイレクト ルール間の一致の違いに注意してください。

| パス                              | 一致したもの |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | [はい]   |
| `/my-cool-rewrite-rule/1234/5678` | ×    |
| `/anotherrewrite-rule/1234/5678`  | ×    |

式の `^rewrite-rule/` に続く部分に、2 つのキャプチャ グループ `(\d+)/(\d+)` があります。 `\d` は、*1 桁 (数字) に一致する*ことを示します。 プラス記号 (`+`) は、*直前の 1 つ以上の文字に一致する*ことを意味します。 そのため、URL は、数字とその後に続くスラッシュ、さらにその後に続く別の数字を含んでいる必要があります。 これらのキャプチャ グループが `$1` および `$2` として書き換えられた URL に挿入されます。 書き換えルールの置換文字列は、クエリ文字列にキャプチャされたグループを配置します。 `/rewrite-rule/1234/5678` の要求されたパスは、`/rewritten?var1=1234&var2=5678` でリソースを取得するように書き換えられます。 クエリ文字列が元の要求に存在する場合、URL が書き換えられるときに保持されます。

リソースを取得するためのサーバーへのラウンドトリップはありません。 リソースが存在する場合は、取得され、200 (OK) 状態コードと共にクライアントに返されます。 クライアントはリダイレクトされていないため、ブラウザーのアドレス バーの URL は変更されません。 クライアントに関しては、URL 書き換え操作は発生しません。

> [!NOTE]
> 式内の照合ルールは、高コストな処理であり、アプリケーションの応答時間を短縮するので、可能な限り `skipRemainingRules: true` を使用します。 最も高速なアプリの応答を実現するには以下のようにします。
> * 一致する頻度が最も多いルールから一致頻度が最も少ないルールへの順番で書き換えルールを並べ替えます。
> * 一致が発生し、追加の処理が必要ないときに残りのルールの処理をスキップします。

### <a name="apache-modrewrite"></a>Apache mod_rewrite
`AddApacheModRewrite` を使用して Apache mod_rewrite ルールを適用します。 ルール ファイルがアプリと共に展開されていることを確認します。 mod_rewrite ルールの情報と例については、「[Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)」を参照してください。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*ApacheModRewrite.txt* ルール ファイルからルールを読み取るには `StreamReader` を使用します。

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

最初のパラメーターは、[依存関係の挿入](dependency-injection.md)によって提供される `IFileProvider` を取得します。 `IHostingEnvironment` が挿入され、`ContentRootFileProvider` を提供します。 2 番目のパラメーターは、ルール ファイルへのパスであり、これはサンプル アプリでは *ApacheModRewrite.txt* です。

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

サンプル アプリは、要求を `/apache-mod-rules-redirect/(.\*)` から `/redirected?id=$1` にリダイレクトします。 応答の状態コードは 302 (検出) です。

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

元の要求: `/apache-mod-rules-redirect/1234`

![要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>サポートされるサーバー変数
ミドルウェアは、次の Apache mod_rewrite サーバー変数をサポートします。
* CONN_REMOTE_ADDR
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_FORWARDED
* HTTP_HOST
* HTTP_REFERER
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
* SERVER_PORT
* SERVER_PROTOCOL
* TIME
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>IIS URL リライト モジュールのルール
IIS URL リライト モジュールに適用されるルールを使用するには、`AddIISUrlRewrite` を使用します。 ルール ファイルがアプリと共に展開されていることを確認します。 Windows Server IIS で実行されているときにミドルウェアで *web.config* ファイルを使用するように指定しないでください。 IIS を使用する場合、IIS リライト モジュールとの競合を避けるために、これらのルールを *web.config* の外部に保存する必要があります。 IIS URL リライト モジュールのルールの詳細と例については、「[Using Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)」(URL リライト モジュール 2.0 の使用 ) と「[URL Rewrite Module Configuration Reference](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)」(URL リライト モジュール構成リファレンス) を参照してください。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*IISUrlRewrite.xml* ルール ファイルからルールを読み取るには `StreamReader` を使用します。

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

最初のパラメーターは `IFileProvider` を取得します。2 番目のパラメーターは XML ルール ファイルのパスであり、このサンプル アプリでは *IISUrlRewrite.xml* です。

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

サンプル アプリは、要求を `/iis-rules-rewrite/(.*)` から `/rewritten?id=$1` に書き換えます。 応答は、200 (OK) 状態コードでクライアントに送信されます。

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

元の要求: `/iis-rules-rewrite/1234`

![要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_iis_url_rewrite.png)

望ましくない方法でアプリに影響するように構成されたサーバー レベル ルールを使用するアクティブな IIS リライト モジュールがある場合は、アプリに対して IIS リライト モジュールを無効にできます。 詳細については、「[Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules)」 (IIS モジュールの無効化) を参照してください。

#### <a name="unsupported-features"></a>サポートされていない機能

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.x でリリースされたミドルウェアは、次の IIS URL リライト モジュールの機能をサポートしていません。
* 送信ルール
* カスタム サーバー変数
* ワイルドカード
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET Core 1.x でリリースされたミドルウェアは、次の IIS URL リライト モジュールの機能をサポートしていません。
* グローバル ルール
* 送信ルール
* マップの書き直し
* CustomResponse アクション
* カスタム サーバー変数
* ワイルドカード
* Action:CustomResponse
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>サポートされるサーバー変数
ミドルウェアは、次の IIS URL リライト モジュール サーバー変数をサポートします。
* CONTENT_LENGTH
* CONTENT_TYPE
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_HOST
* HTTP_REFERER
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
> `PhysicalFileProvider` を介して `IFileProvider` を取得することもできます。 このアプローチは、書き換えルール ファイルの場所に関する大きな柔軟性を提供する場合があります。 書き換えルール ファイルを指定したパスでサーバーに導入されていることを確認してください。
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>メソッド ベースのルール
メソッド内で独自のルール ロジックを実装するには、`Add(Action<RewriteContext> applyRule)` を使用します。 `RewriteContext` は、メソッドで使用するための `HttpContext` を公開します。 `context.Result` は、追加のパイプライン処理の実行方法を決定します。

| context.Result                       | アクション                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (既定値) | ルールの適用を続行する                                         |
| `RuleResult.EndResponse`             | ルールの適用を停止し、応答を送信する                       |
| `RuleResult.SkipRemainingRules`      | ルールの適用を停止し、次のミドルウェアにコンテキストを送信する |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

サンプル アプリは、*.xml* で終了するパスの要求をリダイレクトするメソッドの例を示します。 `/file.xml` の要求を行う場合、`/xmlfiles/file.xml` にリダイレクトされます。 状態コードが 301 (完全な移動) に設定されます。 リダイレクトの場合、応答の状態コードを明示的に設定する必要があります。そのようにしないと、200 (OK) の状態コードが返され、クライアントでリダイレクトは発生しません。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

元の要求: `/file.xml`

![file.xml の要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>IRule ベースのルール
`IRule` から派生するクラス内で独自のルール ロジックを実装するには、`Add(IRule)` を使用します。 `IRule` を使用すると、メソッド ベースのルールのアプローチを使用する場合により高い柔軟性が実現します。 派生クラスにコンストラクターを含め、`ApplyRule` メソッドのパラメーターで渡すことができます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

サンプル アプリの `extension` と `newPath` のパラメーターの値は、いくつかの条件を満たすかチェックされます。 `extension` は、値を含んでいる必要があり、値は *.png*、*.jpg*、または *.gif* でなければなりません。 `newPath` が有効ではない場合、`ArgumentException` がスローされます。 *image.png* の要求を行う場合、`/png-images/image.png` にリダイレクトされます。 *image.jpg* の要求を行う場合、`/jpg-images/image.jpg` にリダイレクトされます。 状態コードが 301 (完全な移動) に設定され、`context.Result` がルールの処理を停止し、応答を送信するように設定されます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

元の要求: `/image.png`

![image.png の要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_redirect_png_requests.png)

元の要求: `/image.jpg`

![image.jpg の要求および応答を追跡する開発者ツールを備えたブラウザー ウィンドウ](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>正規表現の例

| Goal | 正規表現文字列と<br>一致の例 | 置換文字列と<br>出力の例 |
| ---- | :-----------------------------: | :------------------------------------: |
| クエリ文字列内のパスを書き換える | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| 末尾のスラッシュを除去する | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| 末尾のスラッシュを強制する | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| 特定の要求を書き換えを回避する | `^(.*)(?<!\.axd)$` または `^(?!.*\.axd$)(.*)$`<br>はい: `/resource.htm`<br>いいえ: `/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| URL セグメントを再配置する | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| URL セグメントを置き換える | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>その他の技術情報
* [アプリケーションの起動](startup.md)
* [ミドルウェア](xref:fundamentals/middleware/index)
* [.NET の正規表現](/dotnet/articles/standard/base-types/regular-expressions)
* [正規表現言語 - クイック リファレンス](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [URL リライト モジュール 2.0 (IIS 用) の使用](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [URL リライト モジュール構成リファレンス](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [IIS URL リライト モジュール フォーラム](https://forums.iis.net/1152.aspx)
* [単純な URL 構造を保持する](https://support.google.com/webmasters/answer/76329?hl=en)
* [URL 書き換えの 10 のヒントとテクニック](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [スラッシュを使用すべきかどうか](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
