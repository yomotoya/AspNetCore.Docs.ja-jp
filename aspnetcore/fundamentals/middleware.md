---
title: "ASP.NET Core のミドルウェア"
author: rick-anderson
description: "ASP.NET Core のミドルウェアと要求パイプラインについて説明します。"
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware
ms.openlocfilehash: 697a3a8e475dda0d48a11dbfad8fbb0603aa1bf8
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a>ASP.NET Core のミドルウェアの基礎

<a name="fundamentals-middleware"></a>

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Steve Smith](https://ardalis.com/)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="what-is-middleware"></a>ミドルウェアとは

ミドルウェアは、要求と応答を処理するアプリケーション パイプラインとして構成されたソフトウェアです。 各コンポーネントで次の処理を実行します。

* 要求をパイプライン内の次のコンポーネントに渡すかどうかを選択します。
* パイプラインの次のコンポーネントが呼び出される前または呼び出された後に作業を実行できます。 

要求デリゲートは、要求パイプラインの構築に使用されます。 要求デリゲートは、各 HTTP 要求を処理します。

要求デリゲートは、[Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)、[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)、[Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) の各拡張メソッドを使って構成されます。 個々の要求デリゲートは、匿名メソッドとしてインラインで指定するか (インライン ミドルウェアと呼ばれます)、または再利用可能なクラスで定義することができます。 これらの再利用可能なクラスとインラインの匿名メソッドは、"*ミドルウェア*" または "*ミドルウェア コンポーネント*" です。 要求パイプライン内の各ミドルウェア コンポーネントは、パイプラインの次のコンポーネントを呼び出すか、妥当な場合はチェーンをショートサーキットします。

[HTTP モジュールのミドルウェアへの移行](../migration/http-modules.md)に関する記事では、ASP.NET Core および以前のバージョンの要求パイプラインの違いについての説明と、他のミドルウェア サンプルが提供されています。

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>IApplicationBuilder を使用したミドルウェア パイプラインの作成

ASP.NET Core の要求パイプラインは、次の図で示すように (実行のスレッドは黒い矢印に従います)、順番に呼び出される要求デリゲートのシーケンスで構成されます。

![要求の到着、3 つのミドルウェアによる処理、アプリケーションからの応答の送信を示す要求処理パターン。 各ミドルウェアは、そのロジックを実行し、next() ステートメントで要求を次のミドルウェアに渡します。 3 番目のミドルウェアが要求を処理した後、要求は前の 2 つのミドルウェアを通過して、それぞれの next() ステートメントの後にある追加処理を順番に行われた後、クライアントへの応答としてアプリケーションから出て行きます。](middleware/_static/request-delegate-pipeline.png)

各デリゲートは、次のデリゲートの前と後に操作を実行できます。 また、デリゲートは要求を次のデリゲートに渡さないこともできます。これは、要求パイプラインのショートサーキットと呼ばれます。 不要な処理を回避するために、ショートサーキットが望ましい場合がよくあります。 たとえば、静的ファイル ミドルウェアは、静的ファイルの要求を返して、残りのパイプラインをショートサーキットすることができます。 例外処理デリゲートは、パイプラインの後のステージで発生する例外をキャッチできるように、パイプラインの早い段階で呼び出される必要があります。

考えられる最も簡単な ASP.NET Core アプリは、1 つの要求デリゲートを設定してすべての要求を処理するものです。 この場合、実際の要求パイプラインは含まれません。 代わりに、すべての HTTP 要求に対応して単一の匿名関数が呼び出されます。

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

最初の [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) デリゲートは、パイプラインを終了します。

[app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) を使用して、複数の要求デリゲートをチェーン化することができます。 `next` パラメーターは、パイプラインの次のデリゲートを表します  (*next* パラメーターを "*呼び出さない*" ことで、パイプラインをショートサーキットできることに注意してください)。次の例で示すように、通常は、次のデリゲートの前と後の両方でアクションを実行できます。

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> 応答がクライアントに送信された後では、`next.Invoke` を呼び出さないでください。 応答が開始した後で `HttpResponse` を変更すると、例外がスローされます。 たとえば、ヘッダーや状態コードの設定といった変化を行うと、例外がスローされます。 `next` を呼び出した後で応答本文に書き込むと、次のようになります。
> - プロトコル違反が発生する可能性があります。 たとえば、示されている `content-length` より多くを書き込んだ場合。
> - 本文の形式が破損する可能性があります。 たとえば、CSS ファイルに HTML のフッターを書き込んだ場合。
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) は、ヘッダーが送信されたかどうかや本文が書き込まれたかどうかを示すために役立つヒントです。

## <a name="ordering"></a>並べ替え

`Configure` メソッドでミドルウェア コンポーネントを追加する順序は、要求でそれらが呼び出される順序および応答での逆の順序を定義します。 この順序は、セキュリティ、パフォーマンス、および機能にとって重要です。

Configure メソッド (下図参照) は、次のミドルウェア コンポーネントを追加します。

1. 例外/エラー処理
2. 静的ファイル サーバー
3. 認証
4. MVC

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

上記のコードでは、`UseExceptionHandler` が、パイプラインに追加される最初のミドルウェア コンポーネントなので、その後の呼び出しで発生する例外をキャッチします。

静的ファイル ミドルウェアは、パイプラインの早い段階で呼び出されるので、要求を処理し、残りのコンポーネントを通過せずにショート サーキットします。 静的ファイル ミドルウェアは、承認チェックを提供**しません**。 *wwwroot* の下にあるファイルを含め、処理されるすべてファイルが一般に公開されます。 静的ファイルを保護する方法については、「[Working with static files](xref:fundamentals/static-files)」(静的ファイルの操作) を参照してください。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


要求が静的ファイル ミドルウェアによって処理されない場合、要求が Identity ミドルウェア (`app.UseAuthentication`) に渡され、そこで認証が実行されます。 Identity は、認証されていない要求をショート サーキットしません。 Identity は要求を承認しますが、承認 (および却下) は、MVC が固有の Razor ページまたはコント ローラーとアクションを選択した後にのみ発生します。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

要求が静的ファイル ミドルウェアによって処理されない場合、要求が Identity ミドルウェア (`app.UseIdentity`) に渡され、そこで認証が実行されます。 Identity は、認証されていない要求をショート サーキットしません。 Identity は要求を承認しますが、承認 (および却下) は、MVC が固有のコント ローラーとアクションを選択した後にのみ発生します。

-----------

次の例では、応答の圧縮のミドルウェアの前に、静的ファイル ミドルウェアによって静的ファイルの要求を処理する、ミドルウェアの順序付けを示します。 このミドルウェアの順序では、静的なファイルは圧縮されません。 [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) からの MVC 応答を圧縮することができます。

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a>Use、Run、および Map

HTTP パイプラインは、`Use`、`Run`、および `Map` を使用して構成します。 `Use` メソッドは、パイプラインをショートサーキットすることができます (つまり `next` 要求デリゲートを呼び出さない場合)。 `Run` は規則であり、一部のミドルウェア コンポーネントは、パイプラインの最後に実行される `Run[Middleware]` メソッドを公開することがあります。

`Map*` 拡張機能は、パイプラインを分岐する規則として使用されます。 [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) は、指定された要求パスの一致に基づいて要求パイプラインを分岐します。 要求パスが、指定されたパスで始まる場合、分岐が実行されます。

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

次の表は、前のコードを使用した `http://localhost:1234` からの要求および応答を示しています。

| 要求 | 応答 |
| --- | --- |
| localhost:1234 | Hello from non-Map delegate.  |
| localhost:1234/map1 | Map Test 1 |
| localhost:1234/map2 | Map Test 2 |
| localhost:1234/map3 | Hello from non-Map delegate.  |

`Map` を使用すると、一致したパス セグメントが `HttpRequest.Path` から削除され、要求ごとに `HttpRequest.PathBase` に付加されます。

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) は、指定された述語の結果に基づいて、要求パイプラインを分岐します。 `Func<HttpContext, bool>` という型の任意の述語を使って、要求をパイプラインの新しい分岐にマップできます。 次の例では、クエリ文字列変数 `branch` の存在を検出するために術後が使用されます。

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

次の表は、前のコードを使用した `http://localhost:1234` からの要求および応答を示しています。

| 要求 | 応答 |
| --- | --- |
| localhost:1234 | Hello from non-Map delegate.  |
| localhost:1234/?branch=master | Branch used = master|

たとえば次のように、`Map` は入れ子をサポートします。

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

たとえば次のように、`Map` は複数のセグメントと一度に一致させることもできます。

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>組み込みミドルウェア

ASP.NET Core には、次のミドルウェア コンポーネントおよびそれらを追加する順序の説明が付属しています。

| ミドルウェア | 説明 | 順番 |
| ---------- | ----------- | ----- |
| [認証](xref:security/authentication/identity) | 認証のサポートを提供します。 | `HttpContext.User` が必要な場所の前。 OAuth コールバックの終端です。 |
| [CORS](xref:security/cors) | クロス オリジン リソース共有を構成します。 | CORS を使うコンポーネントの前。 |
| [診断](xref:fundamentals/error-handling) | 診断を構成します。 | エラーを生成するコンポーネントの前。 |
| [ForwardedHeaders/HttpOverrides](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | プロキシされたヘッダーを現在の要求に転送します。 | 更新されたフィールド (例: Scheme、Host、ClientIP、Method) を使うコンポーネントの前。 |
| [応答キャッシュ](xref:performance/caching/middleware) | 応答のキャッシュのサポートを提供します。 | キャッシュが必要なコンポーネントの前。 |
| [応答圧縮](xref:performance/response-compression) | 応答の圧縮のサポートを提供します。 | 圧縮が必要なコンポーネントの前。 |
| [RequestLocalization](xref:fundamentals/localization) | ローカライズのサポートを提供します。 | ローカリゼーションが重要なコンポーネントの前。 |
| [ルーティング](xref:fundamentals/routing) | 要求のルートを定義および制約します。 | 一致するルートの最後。 |
| [セッション](xref:fundamentals/app-state) | ユーザー セッションの管理のサポートを提供します。 | セッションが必要なコンポーネントの前。 |
| [静的ファイル](xref:fundamentals/static-files) | 静的ファイルとディレクトリ参照に対応するサポートを提供します。 | 要求がファイルと一致した場合の最後。 |
| [URL リライト](xref:fundamentals/url-rewriting) | URL の書き換えと要求のリダイレクトのサポートを提供します。 | URL を使うコンポーネントの前。 |
| [WebSocket](xref:fundamentals/websockets) | WebSocket プロトコルを有効にします。 | WebSocket 要求を受け入れる必要があるコンポーネントの前。 |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>ミドルウェアの作成

ミドルウェアは一般に、クラスにカプセル化され、拡張メソッドを使用して公開されます。 クエリ文字列からの現在の要求のカルチャを設定する次のミドルウェアについて考えてください。

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

注: 上記のサンプル コードは、ミドルウェア コンポーネントの作成を示すために使用されます。 ASP.NET Core の組み込みのローカリゼーション サポートについては、「[グローバリゼーションとローカリゼーション](xref:fundamentals/localization)」を参照してください。

たとえば `http://localhost:7997/?culture=no` のように、カルチャで渡すことによってミドルウェアをテストすることができます。

次のコードは、ミドルウェア デリゲートをクラスに移動します。

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

次の拡張メソッドは、[IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) を介してミドルウェアを公開します。

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

次のコードは、`Configure` からミドルウェアを呼び出します。

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

ミドルウェアは、コンストラクターで依存関係を公開することによって、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)に従う必要があります。 ミドルウェアは、"*アプリケーションの有効期間*" ごとに 1 回構築されます。 要求内でミドルウェアとサービスを共有する必要がある場合は、後の「*要求ごとの依存関係*」をご覧ください。

ミドルウェア コンポーネントは、コンストラクター パラメーターにより、依存関係の挿入から依存関係を解決できます。 [`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) は、追加パラメーターを直接受け入れることもできます。

### <a name="per-request-dependencies"></a>要求ごとの依存関係

ミドルウェアは要求ごとではなくアプリの起動時に構築されるため、ミドルウェアのコンストラクターによって使われる "*スコープ*" 有効期間のサービスは、各要求の間に、依存関係によって挿入される他の種類と共有されません。 ミドルウェアとその他の種類の間で "*スコープ*" サービスを共有する必要がある場合は、これらのサービスを `Invoke` メソッドのシグネチャに追加します。 `Invoke` メソッドは、依存関係の挿入によって設定される追加のパラメーターを受け取ることができます。 例:

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a>リソース

* [このドキュメントで使用するサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [ミドルウェアへの HTTP モジュールの移行](../migration/http-modules.md)
* [アプリケーションの起動](startup.md)
* [要求機能](request-features.md)
