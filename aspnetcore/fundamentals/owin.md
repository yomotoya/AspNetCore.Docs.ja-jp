---
title: Open Web Interface for .NET (OWIN)
author: ardalis
description: "ASP.NET Core のサポートについて Open Web Interface の .NET (OWIN)、これにより web サーバーから切り離すことが可能に web アプリを検出します。"
keywords: "ASP.NET Core、.NET では、OWIN の開いている Web インターフェイス"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 70c4e6bc-a773-4039-96ec-6fe557c9369d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2ee970a1c9cd05ebee76b92c3e2c7c6c6cc6ef8
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a>開くには .NET (OWIN) 用 Web インターフェイスの概要

によって[Steve Smith](https://ardalis.com/)と[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core は、Open Web Interface for .NET (OWIN) をサポートします。 OWIN により、Web アプリを Web サーバーから切り離すことが可能になります。 要求と関連付けられている応答を処理するパイプラインで使用されるミドルウェアの標準的な方法を定義します。 ASP.NET Core アプリケーションとミドルウェアは、OWIN ベースのアプリケーション、サーバー、およびミドルウェアと相互運用できます。

OWIN は、2 つのフレームワークに一緒に使用するさまざまなオブジェクト モデルでは、分離のレイヤーを提供します。 `Microsoft.AspNetCore.Owin`パッケージは次の 2 つのアダプター実装を提供します。
- OWIN に ASP.NET Core 
- ASP.NET Core を OWIN

これにより、ホストの上、OWIN 互換サーバー/、または ASP.NET Core 上に実行するその他の OWIN 互換コンポーネントをホストする ASP.NET Core できます。

注: これらのアダプターを使用して、パフォーマンス コストが付属します。 ASP.NET Core コンポーネントのみを使用するアプリケーションでは、Owin パッケージまたはアダプターは使用しないでください。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a>ASP.NET パイプラインで実行中の OWIN ミドルウェア

ASP.NET Core の OWIN のサポートがの一部として展開されている、`Microsoft.AspNetCore.Owin`パッケージです。 OWIN のサポートは、このパッケージをインストールすることによって、プロジェクトにインポートできます。

OWIN ミドルウェアに準拠している、 [OWIN 仕様](http://owin.org/spec/spec/owin-1.0.0.html)、する必要があります、`Func<IDictionary<string, object>, Task>`インターフェイス、および特定のキーを設定する (など`owin.ResponseBody`)。 次の単純な OWIN ミドルウェアには、"Hello World"が表示されます。

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

サンプルの署名を返します、`Task`を受け入れると、 `IDictionary<string, object>` OWIN による要求どおりです。

次のコードを追加する方法を示しています、 `OwinHello` (前に示した) での ASP.NET パイプラインにミドルウェア、`UseOwin`拡張メソッド。

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

OWIN パイプライン内で実行するには、その他のアクションを構成することができます。

> [!NOTE]
> 応答ヘッダーは、応答ストリームへの最初の書き込みの前にのみ変更する必要があります。

> [!NOTE]
> 複数回呼び出す`UseOwin`をパフォーマンス上の理由からお勧めします。 OWIN コンポーネントは、一緒にグループ化する場合、最適動作します。

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a>OWIN ベースのサーバーで ASP.NET ホストの使用

OWIN ベースのサーバーには、ASP.NET アプリケーションをホストできます。 このような 1 つのサーバーが[Nowin](https://github.com/Bobris/Nowin)、.NET OWIN web サーバー。 この記事のサンプルでは、含めました Nowin を参照し、それを使用して作成されるプロジェクト、 `IServer` ASP.NET Core 自己ホスト型の対応します。

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer`インターフェイスを必要とする、`Features`プロパティおよび`Start`メソッドです。

`Start`構成して、サーバーは、ここでは、一連の IServerAddressesFeature から解析されたアドレスを設定する fluent API 呼び出しを担当します。 なお fluent 構成の`_builder`変数は、要求で処理することを指定します、`appFunc`メソッドで定義しました。 これは、`Func`は着信要求を処理する要求のたびに呼び出されます。

追加しても、`IWebHostBuilder`を追加して Nowin サーバーを構成するが簡単に拡張します。

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

こうすると、そのために必要なこのカスタムのサーバー拡張機能の呼び出しを使用した ASP.NET アプリケーションを実行する*Program.cs*:

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

詳細については、ASP.NET[サーバー](servers/index.md)です。

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>OWIN ベースのサーバーで ASP.NET Core を実行し、Websocket のサポートを使用

OWIN ベースのサーバーの機能の別の例で利用できる ASP.NET Core は、Websocket などの機能にアクセスします。 前の例で使用される .NET OWIN web サーバーには、ASP.NET Core アプリケーションによって利用されるように組み込まれており、Web ソケットのサポートがいます。 次の例では、Web ソケットをサポートし、Websocket 経由でサーバーに送信されるすべてのものでエコー バックする単純な web アプリを示します。

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

これは、[サンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)が構成されている同じ`NowinServer`唯一の違いはで、アプリケーションを構成する方法として、1 つ前の`Configure`メソッドです。 使用してテスト[単純な websocket クライアント](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en)アプリケーションを示します。

![Web ソケットのテスト用クライアント](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>OWIN 環境

使用する OWIN 環境を構築することができます、`HttpContext`です。

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>OWIN キー

OWIN によって異なります、 `IDictionary<string,object>` HTTP 要求/応答の交換全体の情報を通信するオブジェクト。 ASP.NET Core では、以下に示すキーを実装します。 参照してください、[プライマリ仕様、拡張機能](http://owin.org/#spec)、および[OWIN キー ガイドラインと一般的なキー](http://owin.org/spec/spec/CommonKeys.html)です。

### <a name="request-data-owin-v100"></a>要求データ (OWIN v1.0.0)

| キー               | 値 (型) | 説明 |
| ----------------- | ------------ | ----------- |
| owin です。RequestScheme | `String` |  |
| owin です。RequestMethod  | `String` | |    
| owin です。RequestPathBase  | `String` | |    
| owin です。RequestPath | `String` | |     
| owin です。RequestQueryString  | `String` | |    
| owin です。RequestProtocol  | `String` | |    
| owin です。RequestHeaders | `IDictionary<string,string[]>`  | |
| owin です。RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>要求データ (OWIN v1.1.0)

| キー               | 値 (型) | 説明 |
| ----------------- | ------------ | ----------- |
| owin です。RequestId | `String` | Optional |

### <a name="response-data-owin-v100"></a>応答データ (OWIN v1.0.0)

| キー               | 値 (型) | 説明 |
| ----------------- | ------------ | ----------- |
| owin です。ResponseStatusCode | `int` | Optional |
| owin です。ResponseReasonPhrase | `String` | Optional |
| owin です。ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin です。ResponseBody | `Stream`  | |


### <a name="other-data-owin-v100"></a>その他のデータ (OWIN v1.0.0)

| キー               | 値 (型) | 説明 |
| ----------------- | ------------ | ----------- |
| owin です。CallCancelled | `CancellationToken` |  |
| owin です。バージョン  | `String` | |   


### <a name="common-keys"></a>共通キー

| キー               | 値 (型) | 説明 |
| ----------------- | ------------ | ----------- |
| ssl です。ClientCertificate | `X509Certificate` |  |
| ssl です。LoadClientCertAsync  | `Func<Task>` | |    
| サーバー。RemoteIpAddress  | `String` | |    
| サーバー。リモート ポート | `String` | |     
| サーバー。LocalIpAddress  | `String` | |    
| サーバー。ローカル ポート  | `String` | |    
| サーバー。IsLocal  | `bool` | |    
| サーバー。OnSendingHeaders  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| キー               | 値 (型) | 説明 |
| ----------------- | ------------ | ----------- |
| sendfile です。SendAsync | 参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | 1 回の要求 |


### <a name="opaque-v030"></a>不透明な v0.3.0

| キー               | 値 (型) | 説明 |
| ----------------- | ------------ | ----------- |
| 不透明になります。バージョン | `String` |  |
| 不透明になります。アップグレード | `OpaqueUpgrade` | 参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| 不透明になります。ストリーム | `Stream` |  |
| 不透明になります。CallCancelled | `CancellationToken` |  |


### <a name="websocket-v030"></a>WebSocket v0.3.0

| キー               | 値 (型) | 説明 |
| ----------------- | ------------ | ----------- |
| websocket です。バージョン | `String` |  |
| websocket です。そのまま使用します。 | `WebSocketAccept` | 参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| websocket です。AcceptAlt |  | 非仕様 |
| websocket です。サブプロトコル | `String` | 参照してください[RFC6455 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2)手順 5.5 |
| websocket です。SendAsync | `WebSocketSendAsync` | 参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket です。ReceiveAsync | `WebSocketReceiveAsync` | 参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket です。CloseAsync | `WebSocketCloseAsync` | 参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket です。CallCancelled | `CancellationToken` |  |
| websocket です。ClientCloseStatus | `int` | Optional |
| websocket です。ClientCloseDescription | `String` | Optional |


## <a name="additional-resources"></a>その他のリソース

* [ミドルウェア](middleware.md)

* [サーバー](servers/index.md)
