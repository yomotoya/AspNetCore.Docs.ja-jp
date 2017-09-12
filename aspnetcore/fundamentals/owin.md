---
title: "For .NET (OWIN) の Web インターフェイスを開く"
author: ardalis
description: "開くには .NET (OWIN) 用 Web インターフェイスの概要です。"
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
ms.openlocfilehash: 796d075d4d0c6b888be4fc2225fc482acdbad498
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="25ce7-104">開くには .NET (OWIN) 用 Web インターフェイスの概要</span><span class="sxs-lookup"><span data-stu-id="25ce7-104">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="25ce7-105">によって[Steve Smith](https://ardalis.com/)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="25ce7-105">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="25ce7-106">ASP.NET Core では、.NET (OWIN) の Open Web Interface をサポートします。</span><span class="sxs-lookup"><span data-stu-id="25ce7-106">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="25ce7-107">OWIN は、web サーバーから切り離すことが可能に web アプリを使用できます。</span><span class="sxs-lookup"><span data-stu-id="25ce7-107">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="25ce7-108">要求と関連付けられている応答を処理するパイプラインで使用されるミドルウェアの標準的な方法を定義します。</span><span class="sxs-lookup"><span data-stu-id="25ce7-108">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="25ce7-109">ASP.NET Core アプリケーションとミドルウェアは、OWIN ベースのアプリケーション、サーバー、およびミドルウェアと相互運用できます。</span><span class="sxs-lookup"><span data-stu-id="25ce7-109">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="25ce7-110">OWIN は、2 つのフレームワークに一緒に使用するさまざまなオブジェクト モデルでは、分離のレイヤーを提供します。</span><span class="sxs-lookup"><span data-stu-id="25ce7-110">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="25ce7-111">`Microsoft.AspNetCore.Owin`パッケージは次の 2 つのアダプター実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="25ce7-111">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="25ce7-112">OWIN に ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25ce7-112">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="25ce7-113">ASP.NET Core を OWIN</span><span class="sxs-lookup"><span data-stu-id="25ce7-113">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="25ce7-114">これにより、ホストの上、OWIN 互換サーバー/、または ASP.NET Core 上に実行するその他の OWIN 互換コンポーネントをホストする ASP.NET Core できます。</span><span class="sxs-lookup"><span data-stu-id="25ce7-114">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="25ce7-115">注: これらのアダプターを使用して、パフォーマンス コストが付属します。</span><span class="sxs-lookup"><span data-stu-id="25ce7-115">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="25ce7-116">ASP.NET Core コンポーネントのみを使用するアプリケーションでは、Owin パッケージまたはアダプターは使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="25ce7-116">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

[<span data-ttu-id="25ce7-117">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="25ce7-117">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="25ce7-118">ASP.NET パイプラインで実行中の OWIN ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="25ce7-118">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="25ce7-119">ASP.NET Core の OWIN のサポートがの一部として展開されている、`Microsoft.AspNetCore.Owin`パッケージです。</span><span class="sxs-lookup"><span data-stu-id="25ce7-119">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="25ce7-120">OWIN のサポートは、このパッケージをインストールすることによって、プロジェクトにインポートできます。</span><span class="sxs-lookup"><span data-stu-id="25ce7-120">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="25ce7-121">OWIN ミドルウェアに準拠している、 [OWIN 仕様](http://owin.org/spec/spec/owin-1.0.0.html)、する必要があります、`Func<IDictionary<string, object>, Task>`インターフェイス、および特定のキーを設定する (など`owin.ResponseBody`)。</span><span class="sxs-lookup"><span data-stu-id="25ce7-121">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="25ce7-122">次の単純な OWIN ミドルウェアには、"Hello World"が表示されます。</span><span class="sxs-lookup"><span data-stu-id="25ce7-122">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="25ce7-123">サンプルの署名を返します、`Task`を受け入れると、 `IDictionary<string, object>` OWIN による要求どおりです。</span><span class="sxs-lookup"><span data-stu-id="25ce7-123">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="25ce7-124">次のコードを追加する方法を示しています、 `OwinHello` (前に示した) での ASP.NET パイプラインにミドルウェア、`UseOwin`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="25ce7-124">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/OwinSample/Startup.cs"} -->

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}


```

<span data-ttu-id="25ce7-125">OWIN パイプライン内で実行するには、その他のアクションを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="25ce7-125">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="25ce7-126">応答ヘッダーは、応答ストリームへの最初の書き込みの前にのみ変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="25ce7-126">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="25ce7-127">複数回呼び出す`UseOwin`をパフォーマンス上の理由からお勧めします。</span><span class="sxs-lookup"><span data-stu-id="25ce7-127">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="25ce7-128">OWIN コンポーネントは、一緒にグループ化する場合、最適動作します。</span><span class="sxs-lookup"><span data-stu-id="25ce7-128">OWIN components will operate best if grouped together.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

<a name=hosting-on-owin></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="25ce7-129">OWIN ベースのサーバーで ASP.NET ホストの使用</span><span class="sxs-lookup"><span data-stu-id="25ce7-129">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="25ce7-130">OWIN ベースのサーバーには、ASP.NET アプリケーションをホストできます。</span><span class="sxs-lookup"><span data-stu-id="25ce7-130">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="25ce7-131">このような 1 つのサーバーが[Nowin](https://github.com/Bobris/Nowin)、.NET OWIN web サーバー。</span><span class="sxs-lookup"><span data-stu-id="25ce7-131">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="25ce7-132">この記事のサンプルでは、含めました Nowin を参照し、それを使用して作成されるプロジェクト、 `IServer` ASP.NET Core 自己ホスト型の対応します。</span><span class="sxs-lookup"><span data-stu-id="25ce7-132">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

<span data-ttu-id="25ce7-133">[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]</span><span class="sxs-lookup"><span data-stu-id="25ce7-133">[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]</span></span>

<span data-ttu-id="25ce7-134">`IServer`インターフェイスを必要とする、`Features`プロパティおよび`Start`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="25ce7-134">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="25ce7-135">`Start`構成して、サーバーは、ここでは、一連の IServerAddressesFeature から解析されたアドレスを設定する fluent API 呼び出しを担当します。</span><span class="sxs-lookup"><span data-stu-id="25ce7-135">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="25ce7-136">なお fluent 構成の`_builder`変数は、要求で処理することを指定します、`appFunc`メソッドで定義しました。</span><span class="sxs-lookup"><span data-stu-id="25ce7-136">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="25ce7-137">これは、`Func`は着信要求を処理する要求のたびに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="25ce7-137">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="25ce7-138">追加しても、`IWebHostBuilder`を追加して Nowin サーバーを構成するが簡単に拡張します。</span><span class="sxs-lookup"><span data-stu-id="25ce7-138">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [11], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinSample/NowinWebHostBuilderExtensions.cs"} -->

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

<span data-ttu-id="25ce7-139">こうすると、そのために必要なこのカスタムのサーバー拡張機能の呼び出しを使用した ASP.NET アプリケーションを実行する*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="25ce7-139">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [15], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinSample/Program.cs"} -->

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

<span data-ttu-id="25ce7-140">詳細については、ASP.NET[サーバー](servers/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="25ce7-140">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="25ce7-141">OWIN ベースのサーバーで ASP.NET Core を実行し、Websocket のサポートを使用</span><span class="sxs-lookup"><span data-stu-id="25ce7-141">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="25ce7-142">OWIN ベースのサーバーの機能の別の例で利用できる ASP.NET Core は、Websocket などの機能にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="25ce7-142">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="25ce7-143">前の例で使用される .NET OWIN web サーバーには、ASP.NET Core アプリケーションによって利用されるように組み込まれており、Web ソケットのサポートがいます。</span><span class="sxs-lookup"><span data-stu-id="25ce7-143">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="25ce7-144">次の例では、Web ソケットをサポートし、Websocket 経由でサーバーに送信されるすべてのものでエコー バックする単純な web アプリを示します。</span><span class="sxs-lookup"><span data-stu-id="25ce7-144">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [7, 9, 10], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": true, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinWebSockets/Startup.cs"} -->

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

<span data-ttu-id="25ce7-145">これは、[サンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)が構成されている同じ`NowinServer`唯一の違いはで、アプリケーションを構成する方法として、1 つ前の`Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="25ce7-145">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="25ce7-146">使用してテスト[単純な websocket クライアント](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en)アプリケーションを示します。</span><span class="sxs-lookup"><span data-stu-id="25ce7-146">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Web ソケットのテスト用クライアント](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="25ce7-148">OWIN 環境</span><span class="sxs-lookup"><span data-stu-id="25ce7-148">OWIN environment</span></span>

<span data-ttu-id="25ce7-149">使用する OWIN 環境を構築することができます、`HttpContext`です。</span><span class="sxs-lookup"><span data-stu-id="25ce7-149">You can construct a OWIN environment using the `HttpContext`.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="25ce7-150">OWIN キー</span><span class="sxs-lookup"><span data-stu-id="25ce7-150">OWIN keys</span></span>

<span data-ttu-id="25ce7-151">OWIN によって異なります、 `IDictionary<string,object>` HTTP 要求/応答の交換全体の情報を通信するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="25ce7-151">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="25ce7-152">ASP.NET Core では、以下に示すキーを実装します。</span><span class="sxs-lookup"><span data-stu-id="25ce7-152">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="25ce7-153">参照してください、[プライマリ仕様、拡張機能](http://owin.org/#spec)、および[OWIN キー ガイドラインと一般的なキー](http://owin.org/spec/spec/CommonKeys.html)です。</span><span class="sxs-lookup"><span data-stu-id="25ce7-153">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="25ce7-154">要求データ (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="25ce7-154">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="25ce7-155">キー</span><span class="sxs-lookup"><span data-stu-id="25ce7-155">Key</span></span>               | <span data-ttu-id="25ce7-156">値 (型)</span><span class="sxs-lookup"><span data-stu-id="25ce7-156">Value (type)</span></span> | <span data-ttu-id="25ce7-157">説明</span><span class="sxs-lookup"><span data-stu-id="25ce7-157">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="25ce7-158">owin です。RequestScheme</span><span class="sxs-lookup"><span data-stu-id="25ce7-158">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="25ce7-159">owin です。RequestMethod</span><span class="sxs-lookup"><span data-stu-id="25ce7-159">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="25ce7-160">owin です。RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="25ce7-160">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="25ce7-161">owin です。RequestPath</span><span class="sxs-lookup"><span data-stu-id="25ce7-161">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="25ce7-162">owin です。RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="25ce7-162">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="25ce7-163">owin です。RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="25ce7-163">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="25ce7-164">owin です。RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="25ce7-164">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="25ce7-165">owin です。RequestBody</span><span class="sxs-lookup"><span data-stu-id="25ce7-165">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="25ce7-166">要求データ (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="25ce7-166">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="25ce7-167">キー</span><span class="sxs-lookup"><span data-stu-id="25ce7-167">Key</span></span>               | <span data-ttu-id="25ce7-168">値 (型)</span><span class="sxs-lookup"><span data-stu-id="25ce7-168">Value (type)</span></span> | <span data-ttu-id="25ce7-169">説明</span><span class="sxs-lookup"><span data-stu-id="25ce7-169">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="25ce7-170">owin です。RequestId</span><span class="sxs-lookup"><span data-stu-id="25ce7-170">owin.RequestId</span></span> | `String` | <span data-ttu-id="25ce7-171">Optional</span><span class="sxs-lookup"><span data-stu-id="25ce7-171">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="25ce7-172">応答データ (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="25ce7-172">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="25ce7-173">キー</span><span class="sxs-lookup"><span data-stu-id="25ce7-173">Key</span></span>               | <span data-ttu-id="25ce7-174">値 (型)</span><span class="sxs-lookup"><span data-stu-id="25ce7-174">Value (type)</span></span> | <span data-ttu-id="25ce7-175">説明</span><span class="sxs-lookup"><span data-stu-id="25ce7-175">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="25ce7-176">owin です。ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="25ce7-176">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="25ce7-177">Optional</span><span class="sxs-lookup"><span data-stu-id="25ce7-177">Optional</span></span> |
| <span data-ttu-id="25ce7-178">owin です。ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="25ce7-178">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="25ce7-179">Optional</span><span class="sxs-lookup"><span data-stu-id="25ce7-179">Optional</span></span> |
| <span data-ttu-id="25ce7-180">owin です。ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="25ce7-180">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="25ce7-181">owin です。ResponseBody</span><span class="sxs-lookup"><span data-stu-id="25ce7-181">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="25ce7-182">その他のデータ (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="25ce7-182">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="25ce7-183">キー</span><span class="sxs-lookup"><span data-stu-id="25ce7-183">Key</span></span>               | <span data-ttu-id="25ce7-184">値 (型)</span><span class="sxs-lookup"><span data-stu-id="25ce7-184">Value (type)</span></span> | <span data-ttu-id="25ce7-185">説明</span><span class="sxs-lookup"><span data-stu-id="25ce7-185">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="25ce7-186">owin です。CallCancelled</span><span class="sxs-lookup"><span data-stu-id="25ce7-186">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="25ce7-187">owin です。バージョン</span><span class="sxs-lookup"><span data-stu-id="25ce7-187">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="25ce7-188">共通キー</span><span class="sxs-lookup"><span data-stu-id="25ce7-188">Common Keys</span></span>

| <span data-ttu-id="25ce7-189">キー</span><span class="sxs-lookup"><span data-stu-id="25ce7-189">Key</span></span>               | <span data-ttu-id="25ce7-190">値 (型)</span><span class="sxs-lookup"><span data-stu-id="25ce7-190">Value (type)</span></span> | <span data-ttu-id="25ce7-191">説明</span><span class="sxs-lookup"><span data-stu-id="25ce7-191">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="25ce7-192">ssl です。ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="25ce7-192">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="25ce7-193">ssl です。LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="25ce7-193">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="25ce7-194">サーバー。RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="25ce7-194">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="25ce7-195">サーバー。リモート ポート</span><span class="sxs-lookup"><span data-stu-id="25ce7-195">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="25ce7-196">サーバー。LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="25ce7-196">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="25ce7-197">サーバー。ローカル ポート</span><span class="sxs-lookup"><span data-stu-id="25ce7-197">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="25ce7-198">サーバー。IsLocal</span><span class="sxs-lookup"><span data-stu-id="25ce7-198">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="25ce7-199">サーバー。OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="25ce7-199">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="25ce7-200">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="25ce7-200">SendFiles v0.3.0</span></span>

| <span data-ttu-id="25ce7-201">キー</span><span class="sxs-lookup"><span data-stu-id="25ce7-201">Key</span></span>               | <span data-ttu-id="25ce7-202">値 (型)</span><span class="sxs-lookup"><span data-stu-id="25ce7-202">Value (type)</span></span> | <span data-ttu-id="25ce7-203">説明</span><span class="sxs-lookup"><span data-stu-id="25ce7-203">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="25ce7-204">sendfile です。SendAsync</span><span class="sxs-lookup"><span data-stu-id="25ce7-204">sendfile.SendAsync</span></span> | <span data-ttu-id="25ce7-205">参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="25ce7-205">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="25ce7-206">1 回の要求</span><span class="sxs-lookup"><span data-stu-id="25ce7-206">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="25ce7-207">不透明な v0.3.0</span><span class="sxs-lookup"><span data-stu-id="25ce7-207">Opaque v0.3.0</span></span>

| <span data-ttu-id="25ce7-208">キー</span><span class="sxs-lookup"><span data-stu-id="25ce7-208">Key</span></span>               | <span data-ttu-id="25ce7-209">値 (型)</span><span class="sxs-lookup"><span data-stu-id="25ce7-209">Value (type)</span></span> | <span data-ttu-id="25ce7-210">説明</span><span class="sxs-lookup"><span data-stu-id="25ce7-210">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="25ce7-211">不透明になります。バージョン</span><span class="sxs-lookup"><span data-stu-id="25ce7-211">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="25ce7-212">不透明になります。アップグレード</span><span class="sxs-lookup"><span data-stu-id="25ce7-212">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="25ce7-213">参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="25ce7-213">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="25ce7-214">不透明になります。ストリーム</span><span class="sxs-lookup"><span data-stu-id="25ce7-214">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="25ce7-215">不透明になります。CallCancelled</span><span class="sxs-lookup"><span data-stu-id="25ce7-215">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="25ce7-216">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="25ce7-216">WebSocket v0.3.0</span></span>

| <span data-ttu-id="25ce7-217">キー</span><span class="sxs-lookup"><span data-stu-id="25ce7-217">Key</span></span>               | <span data-ttu-id="25ce7-218">値 (型)</span><span class="sxs-lookup"><span data-stu-id="25ce7-218">Value (type)</span></span> | <span data-ttu-id="25ce7-219">説明</span><span class="sxs-lookup"><span data-stu-id="25ce7-219">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="25ce7-220">websocket です。バージョン</span><span class="sxs-lookup"><span data-stu-id="25ce7-220">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="25ce7-221">websocket です。そのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="25ce7-221">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="25ce7-222">参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="25ce7-222">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="25ce7-223">websocket です。AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="25ce7-223">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="25ce7-224">非仕様</span><span class="sxs-lookup"><span data-stu-id="25ce7-224">Non-spec</span></span> |
| <span data-ttu-id="25ce7-225">websocket です。サブプロトコル</span><span class="sxs-lookup"><span data-stu-id="25ce7-225">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="25ce7-226">参照してください[RFC6455 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2)手順 5.5</span><span class="sxs-lookup"><span data-stu-id="25ce7-226">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="25ce7-227">websocket です。SendAsync</span><span class="sxs-lookup"><span data-stu-id="25ce7-227">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="25ce7-228">参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="25ce7-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="25ce7-229">websocket です。ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="25ce7-229">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="25ce7-230">参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="25ce7-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="25ce7-231">websocket です。CloseAsync</span><span class="sxs-lookup"><span data-stu-id="25ce7-231">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="25ce7-232">参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="25ce7-232">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="25ce7-233">websocket です。CallCancelled</span><span class="sxs-lookup"><span data-stu-id="25ce7-233">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="25ce7-234">websocket です。ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="25ce7-234">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="25ce7-235">Optional</span><span class="sxs-lookup"><span data-stu-id="25ce7-235">Optional</span></span> |
| <span data-ttu-id="25ce7-236">websocket です。ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="25ce7-236">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="25ce7-237">Optional</span><span class="sxs-lookup"><span data-stu-id="25ce7-237">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="25ce7-238">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="25ce7-238">Additional Resources</span></span>

* [<span data-ttu-id="25ce7-239">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="25ce7-239">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="25ce7-240">サーバー</span><span class="sxs-lookup"><span data-stu-id="25ce7-240">Servers</span></span>](servers/index.md)
