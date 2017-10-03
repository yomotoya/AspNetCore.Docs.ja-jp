---
title: "ASP.NET Core ミドルウェア"
author: rick-anderson
description: "ASP.NET Core ミドルウェアと要求パイプラインについて説明します。"
keywords: "ASP.NET Core、パイプライン、デリゲートのミドルウェア"
ms.author: riande
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: db9a86ab-46c2-40e0-baed-86e38c16af1f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: 730b4c281a766059b16ca1c36bbeb9611b979b72
ms.sourcegitcommit: 0f23400cae837e90927043aa0dfd6c31108a4e2c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2017
---
# <a name="aspnet-core-middleware-fundamentals"></a>ASP.NET Core ミドルウェアの基本事項

<a name=fundamentals-middleware></a>

によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[Steve Smith](https://ardalis.com/)

[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-middleware"></a>ミドルウェアは、新機能

ミドルウェアは、要求と応答を処理するアプリケーション パイプラインとして構成されたソフトウェアです。 各コンポーネントには:

* 要求をパイプライン内の次のコンポーネントに渡すかどうかを選択します。
* 前に、と、パイプラインの次のコンポーネントが呼び出された後、作業を実行できます。 

要求のデリゲートは、要求パイプラインの構築に使用されます。 要求のデリゲートでは、各 HTTP 要求を処理します。

デリゲートを使用して構成を要求[実行](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)、[マップ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)、および[使用](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions)拡張メソッド。 個々 の要求はデリゲートがインラインで指定された (インライン ミドルウェアと呼ばれます)、匿名メソッド、としてまたは再利用可能なクラスで定義されていることができます。 これらの再利用可能なクラスとインラインの匿名メソッドは*ミドルウェア*、または*ミドルウェア コンポーネント*です。 要求パイプライン内の各ミドルウェア コンポーネントは、パイプラインでは、次のコンポーネントを呼び出すか、該当する場合、チェーンをショート サーキットします。

[ミドルウェアを移行する HTTP モジュール](../migration/http-modules.md)ASP.NET Core での要求パイプラインと、以前のバージョンの違いについて説明し、他のミドルウェア サンプルを提供します。

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>IApplicationBuilder のミドルウェア パイプラインを作成します。

ASP.NET Core 要求パイプラインは、この図に示す (実行次のように黒の矢印のスレッド) 後に、他のいずれかと呼ばれる、要求のデリゲートのシーケンスで構成されます。

![要求の処理パターンが到着の場合、次の 3 つの middlewares とアプリケーションの応答を通じて処理要求を表示します。 各ミドルウェアは、そのロジックを実行し、要求は next() ステートメントで次のミドルウェアをオフに渡します。 3 番目のミドルウェアが、要求を処理した後は処理するため追加 next() ステートメントの後にさらに、クライアントへの応答としてアプリケーションを終了する前に前の 2 つ middlewares を介して返さ左手です。](middleware/_static/request-delegate-pipeline.png)

各デリゲートは、次のデリゲートの前後に、操作を実行できます。 デリゲートは、要求パイプラインをショート サーキットと呼ばれる次のデリゲートに要求を渡しませんすることもできます。 不要な処理を回避しているので、ショート サーキット望ましいがよくあります。 たとえば、静的ファイル ミドルウェアは、静的ファイルの要求を返すし、残りのパイプラインをショート サーキットします。 例外処理のデリゲートは、パイプラインの後の段階で発生する例外をキャッチできるように、パイプラインの早い段階で呼び出される必要があります。

考えられる最も簡単な ASP.NET Core アプリは、すべての要求を処理する 1 つの要求のデリゲートを設定します。 この場合に、実際の要求パイプラインが含まれていません。 代わりに、各 HTTP 要求に対する応答で単一の匿名関数が呼び出されます。

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

最初の[アプリ。実行](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)デリゲートは、パイプラインを終了します。

と共に複数の要求の代理人をチェーンする[アプリ。使用して](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions)です。 `next`パラメーターはパイプラインの次のデリゲートを表します。 (でパイプラインをショート サーキットできることに注意してください*いない*を呼び出す、*次*パラメーターです)。通常を実行できますアクション前に、と後、次のデリゲートでは、この例で示すように。

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> 呼び出す必要はありません`next.Invoke`応答がクライアントに送信された後です。 変更`HttpResponse`応答が開始された後に例外がスローされます。 たとえば、ヘッダー、ステータス コードなど、設定などの変化例外がスローされます。 呼び出した後、応答本文に書き込む`next`:
> - プロトコル違反が発生する可能性があります。 たとえば、書き込み、示されたより`content-length`です。
> - 本文の形式が破損する可能性があります。 たとえば、CSS ファイルを HTML のフッターを書き込んでいます。
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted)ヘッダーが送信されたかどうかや、本体に書き込まれたを示すために役立ちますヒントします。

## <a name="ordering"></a>並べ替え

ミドルウェア コンポーネントを追加する順序、`Configure`メソッドは、要求で呼び出された順序と応答の逆の順序を定義します。 この順序は、セキュリティ、パフォーマンス、および機能にとって重要です。

Configure メソッド (下図参照) は、次のミドルウェア コンポーネントを追加します。

1. 例外とエラー処理
2. 静的なファイル サーバー
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

、上記のコードで`UseExceptionHandler`は、最初のミドルウェア コンポーネントをパイプラインに追加: そのため、その後の呼び出しで発生する例外をキャッチします。

静的ファイル ミドルウェアがパイプラインの早い段階で呼び出されるは、要求を処理し、残りのコンポーネントを通過せずショート サーキットためです。 静的ファイル ミドルウェアを提供**ありません**承認チェックします。 下にあるものを含め、すべてのファイルが処理される*wwwroot*、一般に公開されます。 参照してください[静的ファイルで作業](xref:fundamentals/static-files)の静的なファイルをセキュリティで保護する方法です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


静的ファイル ミドルウェアによって要求が処理されない場合に Identity ミドルウェア渡されます (`app.UseAuthentication`)、認証を実行します。 Identity に認証されていない要求がショート サーキットされません。 Identity は、要求を認証、承認 (および却下) 実行されますが、MVC は、固有の Razor ページまたはコント ローラーとアクションを選択した後のみです。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

静的ファイル ミドルウェアによって要求が処理されない場合に Identity ミドルウェア渡されます (`app.UseIdentity`)、認証を実行します。 Identity に認証されていない要求がショート サーキットされません。 Identity は、要求を認証、承認 (および却下) 実行されますが、MVC は、特定のコント ローラーとアクションを選択した後のみです。

-----------

次の例では、応答の圧縮のミドルウェアの前に、静的ファイル ミドルウェアによって静的ファイルに対する要求を処理する、順序付けミドルウェアを示します。 このミドルウェアの順序では、静的なファイルは圧縮されません。 MVC 応答から[UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_)圧縮することができます。

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name=middleware-run-map-use></a>

### <a name="use-run-and-map"></a>使用して、実行、およびマップ

HTTP パイプラインを使用して、構成する`Use`、 `Run`、および`Map`です。 `Use`メソッドは、パイプラインをショート サーキットできます (呼び出されない場合は、`next`要求デリゲート)。 `Run`規則は、一部のミドルウェア コンポーネントが公開される`Run[Middleware]`パイプラインの最後に実行されるメソッド。

`Map*`拡張機能は、パイプラインを分岐に、規則として使用されます。 [マップ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)指定された要求パスの一致に基づいて、要求パイプラインを分岐します。 要求パスに指定されたパスで起動する場合、分岐が実行されます。

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

次の表は、要求および応答から`http://localhost:1234`前のコードを使用します。

| 要求 | 応答 |
| --- | --- |
| localhost:1234 | 非マップ デリゲートからこんにちはです。  |
| localhost:1234/map1 | マップのテスト 1 |
| localhost:1234/map2 | マップのテスト 2 |
| localhost:1234/map3 | 非マップ デリゲートからこんにちはです。  |

ときに`Map`はこれを使用すると、一致したパス セグメントはから削除されます`HttpRequest.Path`に付加して`HttpRequest.PathBase`要求ごとにします。

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions)指定の述語の結果に基づいて、要求パイプラインを分岐します。 型の任意の述語`Func<HttpContext, bool>`パイプラインの新しい分岐に要求をマップするために使用できます。 次の例では、述語はクエリ文字列変数の存在を検出する使用`branch`:

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

次の表は、要求および応答から`http://localhost:1234`前のコードを使用します。

| 要求 | 応答 |
| --- | --- |
| localhost:1234 | 非マップ デリゲートからこんにちはです。  |
| localhost:1234/? ブランチ マスターを = | 分岐を使用するマスターを =|

`Map`たとえば、入れ子をサポートします。

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

`Map`できますと一致しても複数のセグメントを一度に例。

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>組み込みのミドルウェア

ASP.NET Core は、次のミドルウェア コンポーネントに付属します。

| ミドルウェア | 説明 |
| ----- | ------- |
| [認証](xref:security/authentication/identity) | 認証のサポートを提供します。 |
| [CORS](xref:security/cors) | クロス オリジン リソース共有を構成します。 |
| [応答キャッシュ](xref:performance/caching/middleware) | 応答のキャッシュのサポートを提供します。 |
| [応答の圧縮](xref:performance/response-compression) | 応答の圧縮のサポートを提供します。 |
| [ルーティング](xref:fundamentals/routing) | 定義し、要求のルートを制約します。 |
| [セッション](xref:fundamentals/app-state) | ユーザー セッションを管理するためのサポートを提供します。 |
| [静的ファイル](xref:fundamentals/static-files) | 静的ファイルとディレクトリの参照を提供しているは、サポートを提供します。 |
| [URL リライト ミドルウェア](xref:fundamentals/url-rewriting) | Url の書き換えと、要求をリダイレクトするサポートを提供します。 |

<a name=middleware-writing-middleware></a>

## <a name="writing-middleware"></a>書き込みミドルウェア

ミドルウェアは一般に、クラスにカプセル化し、拡張メソッドを公開します。 次のミドルウェアは、クエリ文字列からの現在の要求のカルチャの設定を考慮してください。

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

注: 上記のサンプル コードは、ミドルウェア コンポーネントの作成を示すために使用されます。 参照してください[グローバリゼーションとローカリゼーション](xref:fundamentals/localization)ASP.NET Core の組み込みのローカリゼーションをサポートします。

たとえば、カルチャに渡すことによって、ミドルウェアをテストする`http://localhost:7997/?culture=no`です。

次のコードは、ミドルウェア デリゲートをクラスに移動します。

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

次の拡張メソッドを公開、ミドルウェアを介して[IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

次のコードを呼び出すからミドルウェア`Configure`:

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

ミドルウェアが従う必要があります、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)のコンス トラクターにその依存関係を公開することによりします。 1 回あたりのミドルウェアが構築*アプリケーションの有効期間*です。 参照してください*要求ごとの依存関係*場合は、次は、要求内のミドルウェアとサービスを共有する必要があります。

ミドルウェア コンポーネントには、コンス トラクターのパラメーターを使用して依存関係の挿入からその依存関係を解決できます。 [`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)追加のパラメーターを直接受け入れるもできます。

### <a name="per-request-dependencies"></a>要求ごとの依存関係

ミドルウェアがない要求ごとの、アプリの起動時に構築されたため*スコープ*ミドルウェア コンス トラクターによって使用される有効期間サービスは各要求での他の依存関係挿入の種類と共有されません。 共有する場合は、*スコープ*ミドルウェアとその他の種類のサービス、これらのサービスを追加、`Invoke`メソッドのシグネチャ。 `Invoke`メソッドは、依存関係の挿入によって設定される追加のパラメーターを受け取ることができます。 例:

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
