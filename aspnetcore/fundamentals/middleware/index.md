---
title: ASP.NET Core のミドルウェア
author: rick-anderson
description: ASP.NET Core のミドルウェアと要求パイプラインについて説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: c55dbd5a9ac31f55daf1cb3146fb18b91b016919
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341590"
---
# <a name="aspnet-core-middleware"></a>ASP.NET Core のミドルウェア

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/)

ミドルウェアとは、要求と応答を処理するために、アプリのパイプラインに組み込まれたソフトウェアです。 各コンポーネントで次の処理を実行します。

* 要求をパイプライン内の次のコンポーネントに渡すかどうかを選択します。
* パイプラインの次のコンポーネントの前または後に作業を実行できます。

要求デリゲートは、要求パイプラインの構築に使用されます。 要求デリゲートは、各 HTTP 要求を処理します。

要求デリゲートは、<xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>、<xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> の各拡張メソッドを使って構成されます。 個々の要求デリゲートは、匿名メソッドとしてインラインで指定するか (インライン ミドルウェアと呼ばれます)、または再利用可能なクラスで定義することができます。 これらの再利用可能なクラスとインラインの匿名メソッドは、"*ミドルウェア*" です。"*ミドルウェア コンポーネント*" とも呼ばれます。 要求パイプライン内の各ミドルウェア コンポーネントは、パイプラインの次のコンポーネントを呼び出すか、パイプラインをショートさせます。

「<xref:migration/http-modules>」では、ASP.NET Core と ASP.NET 4.x の要求パイプラインの違いについての説明と、他のミドルウェア サンプルが提供されています。

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a>IApplicationBuilder を使用したミドルウェア パイプラインの作成

ASP.NET Core 要求パイプラインは、順番に呼び出される一連の要求デリゲートで構成されています。 次の図は、その概念を示しています。 実行のスレッドは黒い矢印をたどります。

![要求の到着、3 つのミドルウェアによる処理、アプリからの応答の送信を示す要求処理パターン。 各ミドルウェアは、そのロジックを実行し、next() ステートメントで要求を次のミドルウェアに渡します。 3 番目のミドルウェアが要求を処理した後、要求は前の 2 つのミドルウェアを逆の順序で通過して、それぞれの next() ステートメントの後の追加処理が行われた後、クライアントへの応答としてアプリを終了します。](index/_static/request-delegate-pipeline.png)

各デリゲートは、次のデリゲートの前と後に操作を実行できます。 また、デリゲートは要求を次のデリゲートに渡さないこともできます。これは、*要求パイプラインのショートサーキット*と呼ばれます。 不要な処理を回避するために、ショートさせることが望ましい場合がよくあります。 たとえば、静的ファイル ミドルウェアは、静的ファイルの要求を返して、残りのパイプラインをショートさせることができます。 例外処理デリゲートは、パイプラインの後のステージで発生する例外をキャッチできるように、パイプラインの早い段階で呼び出されます。

考えられる最も簡単な ASP.NET Core アプリは、1 つの要求デリゲートを設定してすべての要求を処理するものです。 この場合、実際の要求パイプラインは含まれません。 代わりに、すべての HTTP 要求に対応して単一の匿名関数が呼び出されます。

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

最初の <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> デリゲートが、パイプラインを終了します。

複数の要求デリゲートを <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> と一緒にチェーンします。 `next` パラメーターは、パイプラインの次のデリゲートを表します  *next* パラメーターを "*呼び出さない*" ことで、パイプラインをショートさせることができます。 次の例で示すように、通常は、次のデリゲートの前と後の両方でアクションを実行できます。

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> 応答がクライアントに送信された後に、`next.Invoke` を呼び出さないでください。 応答が開始した後で <xref:Microsoft.AspNetCore.Http.HttpResponse> を変更すると、例外がスローされます。 たとえば、ヘッダーや状態コードの設定などの変更があると、例外がスローされます。 `next` を呼び出した後で応答本文に書き込むと、次のようになります。
>
> * プロトコル違反が発生する可能性があります。 たとえば、示されている `Content-Length` より多くを書き込んだ場合。
> * 本文の形式が破損する可能性があります。 たとえば、CSS ファイルに HTML フッターを書き込んだ場合。
>
> <xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> は、ヘッダーが送信されたかどうかや本文が書き込まれたかどうかを示すために役立つヒントです。

## <a name="order"></a>順番

`Startup.Configure` メソッドでミドルウェア コンポーネントを追加する順序は、要求でミドルウェア コンポーネントが呼び出される順序および応答での逆の順序を定義します。 この順序は、セキュリティ、パフォーマンス、および機能にとって重要です。

次の `Startup.Configure` メソッドにより、共通アプリ シナリオのためのミドルウェア コンポーネントが追加されます。

::: moniker range=">= aspnetcore-2.0"

1. 例外/エラー処理
1. HTTP Strict Transport Security プロトコル
1. HTTPS リダイレクト
1. 静的ファイル サーバー
1. Cookie ポリシーの適用
1. 認証
1. セッション
1. MVC

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    // Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // If the app uses session state, call Session Middleware after Cookie 
    // Policy Middleware and before MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. 例外/エラー処理
1. 静的ファイル
1. 認証
1. セッション
1. MVC

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // If the app uses session state, call UseSession before 
    // MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

前のコード例では、各ミドルウェアの拡張メソッドが <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 名前空間を通じて <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> で公開されています。

パイプラインに追加された最初のミドルウェア コンポーネントは <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> です。 そのため、例外ハンドラー ミドルウェアでは、以降の呼び出しで発生するすべての例外がキャッチされます。

静的ファイル ミドルウェアはパイプラインの早い段階で呼び出されるので、要求を処理し、残りのコンポーネントを通過せずにショートさせることができます。 静的ファイル ミドルウェアでは、承認チェックは提供**されません**。 *wwwroot* の下にあるものも含め、このミドルウェアによって提供されるすべてのファイルは、一般に公開されます。 静的ファイルを保護する方法については、「<xref:fundamentals/static-files>」を参照してください。

::: moniker range=">= aspnetcore-2.0"

要求が静的ファイル ミドルウェアによって処理されない場合、要求は認証を実行する認証ミドルウェア (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) に渡されます。 認証は、認証されない要求をショートさせません。 認証ミドルウェアは要求を認証しますが、承認 (および却下) は、MVC が特定の Razor ページまたは MVC コントローラーとアクションを選んだ後でのみ行われます。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

要求が静的ファイル ミドルウェアによって処理されない場合、要求は認証を実行する ID ミドルウェア (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>) に渡されます。 ID は、認証されない要求をショートさせません。 ID は要求を認証しますが、承認 (および却下) は、MVC が特定のコントローラーとアクションを選んだ後でのみ行われます。

::: moniker-end

次の例は、静的ファイルの要求が応答圧縮ミドルウェアの前に静的ファイル ミドルウェアによって処理される、ミドルウェアの順序を示します。 静的ファイルは、このミドルウェアの順序では圧縮されません。 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> からの MVC 応答を圧縮することができます。

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a>Use、Run、および Map

HTTP パイプラインを構成するには、`Use`、`Run`、`Map` を使います。 `Use` メソッドは、パイプラインをショートさせることができます (つまり `next` 要求デリゲートを呼び出さない場合)。 `Run` は規則であり、一部のミドルウェア コンポーネントは、パイプラインの最後に実行される `Run[Middleware]` メソッドを公開することがあります。

<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 拡張メソッドは、パイプラインを分岐する規則として使われます。 `Map*` は、指定された要求パスの一致に基づいて、要求パイプラインを分岐します。 要求パスが指定されたパスで開始する場合、分岐が実行されます。

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

次の表は、前のコードを使用した `http://localhost:1234` からの要求および応答を示しています。

| 要求             | 応答                     |
| ------------------- | ---------------------------- |
| localhost:1234      | Hello from non-Map delegate. |
| localhost:1234/map1 | Map Test 1                   |
| localhost:1234/map2 | Map Test 2                   |
| localhost:1234/map3 | Hello from non-Map delegate. |

`Map` を使用すると、一致したパス セグメントが `HttpRequest.Path` から削除され、要求ごとに `HttpRequest.PathBase` に付加されます。

[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) は、指定された述語の結果に基づいて、要求パイプラインを分岐します。 `Func<HttpContext, bool>` という型の任意の述語を使って、要求をパイプラインの新しい分岐にマップできます。 次の例では、クエリ文字列変数 `branch` の存在を検出するために術後が使用されます。

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

次の表は、前のコードを使用した `http://localhost:1234` からの要求および応答を示しています。

| 要求                       | 応答                     |
| ----------------------------- | ---------------------------- |
| localhost:1234                | Hello from non-Map delegate. |
| localhost:1234/?branch=master | Branch used = master         |

`Map` は入れ子をサポートします。次にその例を示します。

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

`Map` では、次のように一度に複数のセグメントを照合できます。

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a>組み込みミドルウェア

ASP.NET Core には、次のミドルウェア コンポーネントが付属しています。 「*順番*」列には、要求パイプライン内のミドルウェアの配置と、ミドルウェアが要求を中断し、他のミドルウェアが要求を処理できなくなる条件に関する注意が記載されています。

| ミドルウェア | 説明 | 順番 |
| ---------- | ----------- | ----- |
| [認証](xref:security/authentication/identity) | 認証のサポートを提供します。 | `HttpContext.User` が必要な場所の前。 OAuth コールバックの終端。 |
| [Cookie のポリシー](xref:security/gdpr) | 個人情報の保存に関してユーザーからの同意を追跡し、`secure` や `SameSite` など、Cookie フィールドの最小要件を適用します。 | Cookie を発行するミドルウェアの前。 次に例を示します。認証、セッション、MVC (TempData) |
| [CORS](xref:security/cors) | クロス オリジン リソース共有を構成します。 | CORS を使うコンポーネントの前。 |
| [診断](xref:fundamentals/error-handling) | 診断を構成します。 | エラーを生成するコンポーネントの前。 |
| [転送されるヘッダー](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | プロキシされたヘッダーを現在の要求に転送します。 | 更新されたフィールドを使用するコンポーネントの前。 例: スキーム、ホスト、クライアント IP、メソッド。 |
| [正常性チェック](xref:host-and-deploy/health-checks) | ASP.NET Core アプリとその依存関係の正常性を、データベースの可用性などで確認します。 | 要求が正常性チェックのエンドポイントと一致した場合の終端。 |
| [HTTP メソッドのオーバーライド](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | メソッドをオーバーライドする受信 POST 要求を許可します。 | 更新されたメソッドを使うコンポーネントの前。 |
| [HTTPS リダイレクト](xref:security/enforcing-ssl#require-https) | すべての HTTP 要求を HTTPS (ASP.NET Core 2.1 以降) にリダイレクトします。 | URL を使うコンポーネントの前。 |
| [HTTP Strict Transport Security (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | 特殊な応答ヘッダーを追加するセキュリティ拡張機能のミドルウェア (ASP.NET Core 2.1 以降)。 | 応答が送信される前と要求を変更するコンポーネントの後。 次に例を示します。転送されるヘッダー、URL リライト。 |
| [MVC](xref:mvc/overview) | MVC/Razor Pages (ASP.NET Core 2.0 以降) で要求を処理します。 | 要求がルートと一致した場合の終端。 |
| [OWIN](xref:fundamentals/owin) | OWIN ベースのアプリ、サーバー、およびミドルウェアと相互運用します。 | OWIN ミドルウェアが要求を完全に処理した場合の終端。 |
| [応答キャッシュ](xref:performance/caching/middleware) | 応答のキャッシュのサポートを提供します。 | キャッシュが必要なコンポーネントの前。 |
| [応答圧縮](xref:performance/response-compression) | 応答の圧縮のサポートを提供します。 | 圧縮が必要なコンポーネントの前。 |
| [要求のローカライズ](xref:fundamentals/localization) | ローカライズのサポートを提供します。 | ローカリゼーションが重要なコンポーネントの前。 |
| [ルーティング](xref:fundamentals/routing) | 要求のルートを定義および制約します。 | 一致するルートの終端。 |
| [セッション](xref:fundamentals/app-state) | ユーザー セッションの管理のサポートを提供します。 | セッションが必要なコンポーネントの前。 |
| [静的ファイル](xref:fundamentals/static-files) | 静的ファイルとディレクトリ参照に対応するサポートを提供します。 | 要求がファイルと一致した場合の終端。 |
| [URL リライト](xref:fundamentals/url-rewriting) | URL の書き換えと要求のリダイレクトのサポートを提供します。 | URL を使うコンポーネントの前。 |
| [WebSocket](xref:fundamentals/websockets) | WebSocket プロトコルを有効にします。 | WebSocket 要求を受け入れる必要があるコンポーネントの前。 |

## <a name="write-middleware"></a>ミドルウェアの作成

ミドルウェアは一般に、クラスにカプセル化され、拡張メソッドを使用して公開されます。 クエリ文字列から現在の要求のカルチャを設定する次のようなミドルウェアを考慮します。

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

上のサンプル コードを使って、ミドルウェア コンポーネントの作成方法を示します。 ASP.NET Core に組み込まれているローカライズのサポートについては、「<xref:fundamentals/localization>」を参照してください。

カルチャを渡すことによって、ミドルウェアをテストできます (例: `http://localhost:7997/?culture=no`)。

次のコードは、ミドルウェアのデリゲートをクラスに移動します。

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

ミドルウェア `Task` メソッドの名前は `Invoke` である必要があります。 ASP.NET Core 2.0 以降では、名前は `Invoke` でも `InvokeAsync` でも構いません。

::: moniker-end

次の拡張メソッドは、<xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> を介してミドルウェアを公開します。

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

次のコードは、`Startup.Configure` からミドルウェアを呼び出します。

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

ミドルウェアは、コンストラクターで依存関係を公開することによって、[明示的な依存関係の原則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)に従う必要があります。 ミドルウェアは、"*アプリケーションの有効期間*" ごとに 1 回構築されます。 要求内でミドルウェアとサービスを共有する必要がある場合は、「[要求ごとの依存関係](#per-request-dependencies)」を参照してください。

ミドルウェア コンポーネントは、コンストラクター パラメーターにより、[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) から依存関係を解決できます。 [UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) は、追加パラメーターを直接受け入れることもできます。

### <a name="per-request-dependencies"></a>要求ごとの依存関係

ミドルウェアは要求ごとではなくアプリの起動時に構築されるため、ミドルウェアのコンストラクターによって使われる "*スコープ*" 有効期間のサービスは、各要求の間に、依存関係によって挿入される他の種類と共有されません。 ミドルウェアとその他の種類の間で "*スコープ*" サービスを共有する必要がある場合は、これらのサービスを `Invoke` メソッドのシグネチャに追加します。 `Invoke` メソッドは、DI によって設定される追加のパラメーターを受け取ることができます。

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a>その他の技術情報

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
