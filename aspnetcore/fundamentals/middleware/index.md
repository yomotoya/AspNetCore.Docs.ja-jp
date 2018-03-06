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
uid: fundamentals/middleware/index
ms.openlocfilehash: 5d236c79120d79195c1970cc87d164002b56d0f1
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-middleware"></a>ASP.NET Core のミドルウェア

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="what-is-middleware"></a>ミドルウェアとは

ミドルウェアは、要求と応答を処理するアプリケーション パイプラインとして構成されたソフトウェアです。 各コンポーネントで次の処理を実行します。

* 要求をパイプライン内の次のコンポーネントに渡すかどうかを選択します。
* パイプラインの次のコンポーネントが呼び出される前または呼び出された後に作業を実行できます。 

要求デリゲートは、要求パイプラインの構築に使用されます。 要求デリゲートは、各 HTTP 要求を処理します。

要求デリゲートは、[Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)、[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)、[Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) の各拡張メソッドを使って構成されます。 個々の要求デリゲートは、匿名メソッドとしてインラインで指定するか (インライン ミドルウェアと呼ばれます)、または再利用可能なクラスで定義することができます。 これらの再利用可能なクラスとインラインの匿名メソッドは、"*ミドルウェア*" または "*ミドルウェア コンポーネント*" です。 要求パイプライン内の各ミドルウェア コンポーネントは、パイプラインの次のコンポーネントを呼び出すか、妥当な場合はチェーンをショートさせます。

[HTTP モジュールのミドルウェアへの移行](xref:migration/http-modules)に関するページでは、ASP.NET Core と ASP.NET 4.x の要求パイプラインの違いについての説明と、他のミドルウェア サンプルが提供されています。

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>IApplicationBuilder を使用したミドルウェア パイプラインの作成

ASP.NET Core の要求パイプラインは、次の図で示すように (実行のスレッドは黒い矢印に沿って進みます)、順番に呼び出される要求デリゲートのシーケンスで構成されます。

![要求の到着、3 つのミドルウェアによる処理、アプリケーションからの応答の送信を示す要求処理パターン。 各ミドルウェアは、そのロジックを実行し、next() ステートメントで要求を次のミドルウェアに渡します。 3 番目のミドルウェアが要求を処理した後、要求は前の 2 つのミドルウェアを逆の順序で通過して、それぞれの next() ステートメントの後の追加処理が行われた後、クライアントへの応答としてアプリケーションを終了します。](index/_static/request-delegate-pipeline.png)

各デリゲートは、次のデリゲートの前と後に操作を実行できます。 また、デリゲートは要求を次のデリゲートに渡さないこともできます。これは、要求パイプラインのショートサーキットと呼ばれます。 不要な処理を回避するために、ショートさせることが望ましい場合がよくあります。 たとえば、静的ファイル ミドルウェアは、静的ファイルの要求を返して、残りのパイプラインをショートさせることができます。 例外処理デリゲートは、パイプラインの後のステージで発生する例外をキャッチできるように、パイプラインの早い段階で呼び出される必要があります。

考えられる最も簡単な ASP.NET Core アプリは、1 つの要求デリゲートを設定してすべての要求を処理するものです。 この場合、実際の要求パイプラインは含まれません。 代わりに、すべての HTTP 要求に対応して単一の匿名関数が呼び出されます。

[!code-csharp[](index/sample/Middleware/Startup.cs)]

最初の [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) デリゲートが、パイプラインを終了します。

複数の要求デリゲートを [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) と一緒にチェーンすることができます。 `next` パラメーターは、パイプラインの次のデリゲートを表します  (*next* パラメーターを "*呼び出さない*" ことで、パイプラインをショートさせることができることに注意してください)。次の例で示すように、通常は、次のデリゲートの前と後の両方でアクションを実行できます。

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> 応答がクライアントに送信された後に、`next.Invoke` を呼び出さないでください。 応答が開始した後で `HttpResponse` を変更すると、例外がスローされます。 たとえば、ヘッダーや状態コードの設定といった変化を行うと、例外がスローされます。 `next` を呼び出した後で応答本文に書き込むと、次のようになります。
> - プロトコル違反が発生する可能性があります。 たとえば、示されている `content-length` より多くを書き込んだ場合。
> - 本文の形式が破損する可能性があります。 たとえば、CSS ファイルに HTML フッターを書き込んだ場合。
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

上記のコードでは、`UseExceptionHandler` がパイプラインに追加される最初のミドルウェア コンポーネントです。そのため、後続の呼び出しで発生するすべての例外をキャッチします。

静的ファイル ミドルウェアはパイプラインの早い段階で呼び出されるので、要求を処理し、残りのコンポーネントを通過せずにショートさせることができます。 静的ファイル ミドルウェアでは、承認チェックは提供**されません**。 *wwwroot* の下にあるものも含め、このミドルウェアによって提供されるすべてのファイルは、一般に公開されます。 静的ファイルを保護する方法については、「[Working with static files](xref:fundamentals/static-files)」(静的ファイルの操作) を参照してください。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


要求が静的ファイル ミドルウェアによって処理されない場合、要求は認証を実行する ID ミドルウェア (`app.UseAuthentication`) に渡されます。 ID は、認証されない要求をショートさせません。 ID は要求を認証しますが、承認 (および却下) は、MVC が特定の Razor ページまたはコントローラーとアクションを選んだ後でのみ行われます。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

要求が静的ファイル ミドルウェアによって処理されない場合、要求は認証を実行する ID ミドルウェア (`app.UseIdentity`) に渡されます。 ID は、認証されない要求をショートさせません。 ID は要求を認証しますが、承認 (および却下) は、MVC が特定のコントローラーとアクションを選んだ後でのみ行われます。

-----------

次の例では、静的ファイルの要求が応答圧縮ミドルウェアの前に静的ファイル ミドルウェアによって処理される、ミドルウェアの順序を示します。 このミドルウェアの順序では、静的なファイルは圧縮されません。 [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) からの MVC 応答を圧縮することができます。

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

HTTP パイプラインを構成するには、`Use`、`Run`、`Map` を使います。 `Use` メソッドは、パイプラインをショートさせることができます (つまり `next` 要求デリゲートを呼び出さない場合)。 `Run` は規則であり、一部のミドルウェア コンポーネントは、パイプラインの最後に実行される `Run[Middleware]` メソッドを公開することがあります。

`Map*` 拡張メソッドは、パイプラインを分岐する規則として使われます。 [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) は、指定された要求パスの一致に基づいて、要求パイプラインを分岐します。 要求パスが指定されたパスで開始する場合、分岐が実行されます。

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

次の表は、前のコードを使用した `http://localhost:1234` からの要求および応答を示しています。

| 要求 | 応答 |
| --- | --- |
| localhost:1234 | Hello from non-Map delegate.  |
| localhost:1234/map1 | Map Test 1 |
| localhost:1234/map2 | Map Test 2 |
| localhost:1234/map3 | Hello from non-Map delegate.  |

`Map` を使用すると、一致したパス セグメントが `HttpRequest.Path` から削除され、要求ごとに `HttpRequest.PathBase` に付加されます。

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) は、指定された述語の結果に基づいて、要求パイプラインを分岐します。 `Func<HttpContext, bool>` という型の任意の述語を使って、要求をパイプラインの新しい分岐にマップできます。 次の例では、クエリ文字列変数 `branch` の存在を検出するために術後が使用されます。

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

次の表は、前のコードを使用した `http://localhost:1234` からの要求および応答を示しています。

| 要求 | 応答 |
| --- | --- |
| localhost:1234 | Hello from non-Map delegate.  |
| localhost:1234/?branch=master | Branch used = master|

`Map` は入れ子をサポートします。次にその例を示します。

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

`Map` では、一度に複数のセグメントを照合できます。次にその例を示します。

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>組み込みミドルウェア

ASP.NET Core には、次のミドルウェア コンポーネントおよびそれらを追加する順序の説明が付属しています。

| ミドルウェア | 説明 | 順番 |
| ---------- | ----------- | ----- |
| [認証](xref:security/authentication/identity) | 認証のサポートを提供します。 | `HttpContext.User` が必要な場所の前。 OAuth コールバックの終端。 |
| [CORS](xref:security/cors) | クロス オリジン リソース共有を構成します。 | CORS を使うコンポーネントの前。 |
| [診断](xref:fundamentals/error-handling) | 診断を構成します。 | エラーを生成するコンポーネントの前。 |
| [ForwardedHeaders/HttpOverrides](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | プロキシされたヘッダーを現在の要求に転送します。 | 更新されたフィールド (例: Scheme、Host、ClientIP、Method) を使うコンポーネントの前。 |
| [応答キャッシュ](xref:performance/caching/middleware) | 応答のキャッシュのサポートを提供します。 | キャッシュが必要なコンポーネントの前。 |
| [応答圧縮](xref:performance/response-compression) | 応答の圧縮のサポートを提供します。 | 圧縮が必要なコンポーネントの前。 |
| [RequestLocalization](xref:fundamentals/localization) | ローカライズのサポートを提供します。 | ローカリゼーションが重要なコンポーネントの前。 |
| [ルーティング](xref:fundamentals/routing) | 要求のルートを定義および制約します。 | 一致するルートの終端。 |
| [セッション](xref:fundamentals/app-state) | ユーザー セッションの管理のサポートを提供します。 | セッションが必要なコンポーネントの前。 |
| [静的ファイル](xref:fundamentals/static-files) | 静的ファイルとディレクトリ参照に対応するサポートを提供します。 | 要求がファイルと一致した場合の終端。 |
| [URL リライト](xref:fundamentals/url-rewriting) | URL の書き換えと要求のリダイレクトのサポートを提供します。 | URL を使うコンポーネントの前。 |
| [WebSocket](xref:fundamentals/websockets) | WebSocket プロトコルを有効にします。 | WebSocket 要求を受け入れる必要があるコンポーネントの前。 |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>ミドルウェアの作成

ミドルウェアは一般に、クラスにカプセル化され、拡張メソッドを使用して公開されます。 クエリ文字列から現在の要求のカルチャを設定する次のようなミドルウェアを考慮します。

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

注: 上のサンプル コードを使って、ミドルウェア コンポーネントの作成方法を示します。 ASP.NET Core に組み込まれているローカリゼーションのサポートについては、「[ASP.NET Core のグローバリゼーションおよびローカリゼーション](xref:fundamentals/localization)」をご覧ください。

カルチャを渡すことによって、ミドルウェアをテストできます (例: `http://localhost:7997/?culture=no`)。

次のコードは、ミドルウェアのデリゲートをクラスに移動します。

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

次の拡張メソッドは、[IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) を介してミドルウェアを公開します。

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

次のコードは、`Configure` からミドルウェアを呼び出します。

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

ミドルウェアは、コンストラクターで依存関係を公開することによって、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)に従う必要があります。 ミドルウェアは、"*アプリケーションの有効期間*" ごとに 1 回構築されます。 要求内でミドルウェアとサービスを共有する必要がある場合は、下記の「*要求ごとの依存関係*」をご覧ください。

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

## <a name="additional-resources"></a>その他の技術情報

* [ミドルウェアへの HTTP モジュールの移行](xref:migration/http-modules)
* [アプリケーションの起動](xref:fundamentals/startup)
* [要求機能](xref:fundamentals/request-features)
* [ファクトリ ベースのミドルウェアのアクティブ化](xref:fundamentals/middleware/extensibility)
* [サードパーティー コンテナーによるミドルウェアのアクティブ化](xref:fundamentals/middleware/extensibility-third-party-container)
