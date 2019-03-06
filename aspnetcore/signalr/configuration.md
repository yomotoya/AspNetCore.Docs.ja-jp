---
title: ASP.NET Core SignalR の構成
author: bradygaster
description: ASP.NET Core SignalR アプリケーションを構成する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/07/2019
uid: signalr/configuration
ms.openlocfilehash: c5921db895a732c9663c9d962195a2c0635f5aa0
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400659"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="8146f-103">ASP.NET Core SignalR の構成</span><span class="sxs-lookup"><span data-stu-id="8146f-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="8146f-104">JSON/MessagePack シリアル化オプション</span><span class="sxs-lookup"><span data-stu-id="8146f-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="8146f-105">ASP.NET Core SignalR には、メッセージをエンコードするための 2 つのプロトコルがサポートされています。[JSON](https://www.json.org/)と[MessagePack](https://msgpack.org/index.html)します。</span><span class="sxs-lookup"><span data-stu-id="8146f-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="8146f-106">各プロトコルには、シリアル化の構成オプションがあります。</span><span class="sxs-lookup"><span data-stu-id="8146f-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="8146f-107">JSON のシリアル化を使用して、サーバーで構成できます、 [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)拡張メソッドは後に、追加する[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)で、`Startup.ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="8146f-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="8146f-108">`AddJsonProtocol`メソッドが受信するデリゲートを受け取り、`options`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="8146f-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="8146f-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)そのオブジェクトのプロパティは、JSON.NET`JsonSerializerSettings`引数のシリアル化の構成し、戻り値を使用できるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="8146f-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="8146f-110">参照してください、 [JSON.NET のドキュメント](https://www.newtonsoft.com/json/help/html/Introduction.htm)の詳細。</span><span class="sxs-lookup"><span data-stu-id="8146f-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="8146f-111">たとえば、「キャメル ケース」の既定名ではなく"PascalCase"プロパティの名前を使用するシリアライザーを構成する次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="8146f-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="8146f-112">.NET クライアントで同じ`AddJsonProtocol`に拡張メソッドが存在する[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)します。</span><span class="sxs-lookup"><span data-stu-id="8146f-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="8146f-113">`Microsoft.Extensions.DependencyInjection`拡張メソッドを解決するのには名前空間をインポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8146f-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
            new DefaultContractResolver();
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="8146f-114">この時点で、JavaScript クライアント内で JSON のシリアル化を構成することはできません。</span><span class="sxs-lookup"><span data-stu-id="8146f-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="8146f-115">MessagePack シリアル化オプション</span><span class="sxs-lookup"><span data-stu-id="8146f-115">MessagePack serialization options</span></span>

<span data-ttu-id="8146f-116">デリゲートを提供することで MessagePack シリアル化を構成することができます、 [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8146f-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="8146f-117">参照してください[SignalR で MessagePack](xref:signalr/messagepackhubprotocol)の詳細。</span><span class="sxs-lookup"><span data-stu-id="8146f-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="8146f-118">この時点で、JavaScript クライアント内で MessagePack シリアル化を構成することはできません。</span><span class="sxs-lookup"><span data-stu-id="8146f-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="8146f-119">サーバー オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="8146f-119">Configure server options</span></span>

<span data-ttu-id="8146f-120">次の表では、SignalR ハブを構成するためのオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="8146f-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="8146f-121">オプション</span><span class="sxs-lookup"><span data-stu-id="8146f-121">Option</span></span> | <span data-ttu-id="8146f-122">既定値</span><span class="sxs-lookup"><span data-stu-id="8146f-122">Default Value</span></span> | <span data-ttu-id="8146f-123">説明</span><span class="sxs-lookup"><span data-stu-id="8146f-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="8146f-124">30 秒</span><span class="sxs-lookup"><span data-stu-id="8146f-124">30 seconds</span></span> | <span data-ttu-id="8146f-125">サーバーは、クライアントを検討してください、この間隔で (キープ アライブを含む) メッセージを受信していない場合は切断されます。</span><span class="sxs-lookup"><span data-stu-id="8146f-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="8146f-126">実際にマーク済みであるため、これを実装する方法、切断されたクライアントの場合は、このタイムアウト間隔よりも長い時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="8146f-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="8146f-127">推奨値は二重、`KeepAliveInterval`値。</span><span class="sxs-lookup"><span data-stu-id="8146f-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="8146f-128">15 秒</span><span class="sxs-lookup"><span data-stu-id="8146f-128">15 seconds</span></span> | <span data-ttu-id="8146f-129">クライアントがこの時間間隔内で最初のハンドシェイク メッセージを送信しない場合、接続は閉じられます。</span><span class="sxs-lookup"><span data-stu-id="8146f-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="8146f-130">これは、重大なネットワーク待機時間が原因のハンドシェイクのタイムアウト エラーが発生している場合にのみ変更する高度な設定です。</span><span class="sxs-lookup"><span data-stu-id="8146f-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8146f-131">ハンドシェイク プロセスの詳細については、次を参照してください。、 [SignalR ハブ プロトコル仕様](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)します。</span><span class="sxs-lookup"><span data-stu-id="8146f-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="8146f-132">15 秒</span><span class="sxs-lookup"><span data-stu-id="8146f-132">15 seconds</span></span> | <span data-ttu-id="8146f-133">サーバーがこの間隔内でメッセージを送信していない場合、接続を開いたままに ping メッセージが自動的に送信されます。</span><span class="sxs-lookup"><span data-stu-id="8146f-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="8146f-134">変更するときに`KeepAliveInterval`、変更、 `ServerTimeout` / `serverTimeoutInMilliseconds`クライアントに設定します。</span><span class="sxs-lookup"><span data-stu-id="8146f-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="8146f-135">推奨される`ServerTimeout` / `serverTimeoutInMilliseconds`値が double、`KeepAliveInterval`値。</span><span class="sxs-lookup"><span data-stu-id="8146f-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="8146f-136">インストールされているすべてのプロトコル</span><span class="sxs-lookup"><span data-stu-id="8146f-136">All installed protocols</span></span> | <span data-ttu-id="8146f-137">このハブでサポートされるプロトコル。</span><span class="sxs-lookup"><span data-stu-id="8146f-137">Protocols supported by this hub.</span></span> <span data-ttu-id="8146f-138">既定では、サーバーに登録されているプロトコルをすべて許可されますが、プロトコルは、個々 のハブに対する特定のプロトコルを無効にするには、この一覧から削除できます。</span><span class="sxs-lookup"><span data-stu-id="8146f-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="8146f-139">場合`true`詳細のハブ メソッドで例外がスローされたときに、例外メッセージがクライアントに返されます。</span><span class="sxs-lookup"><span data-stu-id="8146f-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="8146f-140">既定値は`false`、これらの例外メッセージは、機密情報を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8146f-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="8146f-141">オプションのデリゲートを提供することですべてのハブのオプションを構成でき、`AddSignalR`呼び出す`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="8146f-141">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

<span data-ttu-id="8146f-142">1 つのハブのオプションで提供されるグローバル オプションを上書き`AddSignalR`を使用して構成できると[AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="8146f-142">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="8146f-143">高度な HTTP 構成オプション</span><span class="sxs-lookup"><span data-stu-id="8146f-143">Advanced HTTP configuration options</span></span>

<span data-ttu-id="8146f-144">使用`HttpConnectionDispatcherOptions`トランスポートおよびメモリ バッファーの管理に関連する高度な設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="8146f-144">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="8146f-145">これらのオプションを構成するデリゲートを渡すことによって[MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)で`Startup.Configure`します。</span><span class="sxs-lookup"><span data-stu-id="8146f-145">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSignalR((configure) => 
    {
        var desiredTransports = 
            HttpTransportType.WebSockets |
            HttpTransportType.LongPolling;

        configure.MapHub<MyHub>("/myhub", (options) => 
        {
            options.Transports = desiredTransports;
        });
    });
}
```

<span data-ttu-id="8146f-146">次の表では、ASP.NET Core SignalR の高度な HTTP オプションを構成するためのオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="8146f-146">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="8146f-147">オプション</span><span class="sxs-lookup"><span data-stu-id="8146f-147">Option</span></span> | <span data-ttu-id="8146f-148">既定値</span><span class="sxs-lookup"><span data-stu-id="8146f-148">Default Value</span></span> | <span data-ttu-id="8146f-149">説明</span><span class="sxs-lookup"><span data-stu-id="8146f-149">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="8146f-150">32 KB</span><span class="sxs-lookup"><span data-stu-id="8146f-150">32 KB</span></span> | <span data-ttu-id="8146f-151">クライアントから受信したバイトの最大数をサーバーのバッファー。</span><span class="sxs-lookup"><span data-stu-id="8146f-151">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="8146f-152">この値を増やすことによりより大きなメッセージを受信するサーバーが、メモリ消費量に悪影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="8146f-152">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="8146f-153">データが自動的に収集、`Authorize`ハブ クラスに適用される属性。</span><span class="sxs-lookup"><span data-stu-id="8146f-153">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="8146f-154">一連の[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)オブジェクトは、クライアントがハブへの接続を承認されているかどうかを判断するために使用します。</span><span class="sxs-lookup"><span data-stu-id="8146f-154">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="8146f-155">32 KB</span><span class="sxs-lookup"><span data-stu-id="8146f-155">32 KB</span></span> | <span data-ttu-id="8146f-156">アプリによって送信されたバイト数の最大数をサーバーのバッファー。</span><span class="sxs-lookup"><span data-stu-id="8146f-156">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="8146f-157">この値を増やすことによりより大きなメッセージを送信するサーバーが、メモリ消費量に悪影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="8146f-157">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="8146f-158">すべてのトランスポートが有効になります。</span><span class="sxs-lookup"><span data-stu-id="8146f-158">All Transports are enabled.</span></span> | <span data-ttu-id="8146f-159">ビットマスク`HttpTransportType`トランスポートを制限する値をクライアントが接続に使用できます。</span><span class="sxs-lookup"><span data-stu-id="8146f-159">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="8146f-160">以下を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8146f-160">See below.</span></span> | <span data-ttu-id="8146f-161">長いポーリングのトランスポートに固有の追加オプション。</span><span class="sxs-lookup"><span data-stu-id="8146f-161">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="8146f-162">以下を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8146f-162">See below.</span></span> | <span data-ttu-id="8146f-163">Websocket トランスポートに固有の追加オプション。</span><span class="sxs-lookup"><span data-stu-id="8146f-163">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="8146f-164">ポーリングの長いトランスポートを使用して構成できるその他のオプションを持つ、`LongPolling`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="8146f-164">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="8146f-165">オプション</span><span class="sxs-lookup"><span data-stu-id="8146f-165">Option</span></span> | <span data-ttu-id="8146f-166">既定値</span><span class="sxs-lookup"><span data-stu-id="8146f-166">Default Value</span></span> | <span data-ttu-id="8146f-167">説明</span><span class="sxs-lookup"><span data-stu-id="8146f-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="8146f-168">90 秒</span><span class="sxs-lookup"><span data-stu-id="8146f-168">90 seconds</span></span> | <span data-ttu-id="8146f-169">1 つのポーリング要求を終了する前に、クライアントに送信するメッセージのサーバーが待機する最長時間。</span><span class="sxs-lookup"><span data-stu-id="8146f-169">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="8146f-170">新しいポーリング要求をより頻繁に発行するクライアントは、この値を小さきます。</span><span class="sxs-lookup"><span data-stu-id="8146f-170">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="8146f-171">WebSocket トランスポートが追加のオプションを使用して構成できる、`WebSockets`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="8146f-171">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="8146f-172">オプション</span><span class="sxs-lookup"><span data-stu-id="8146f-172">Option</span></span> | <span data-ttu-id="8146f-173">既定値</span><span class="sxs-lookup"><span data-stu-id="8146f-173">Default Value</span></span> | <span data-ttu-id="8146f-174">説明</span><span class="sxs-lookup"><span data-stu-id="8146f-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="8146f-175">5 秒</span><span class="sxs-lookup"><span data-stu-id="8146f-175">5 seconds</span></span> | <span data-ttu-id="8146f-176">サーバーが閉じ、クライアントがこの時間間隔内に閉じるが失敗した場合、接続は終了します。</span><span class="sxs-lookup"><span data-stu-id="8146f-176">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="8146f-177">設定するために使用するデリゲート、`Sec-WebSocket-Protocol`ヘッダーをカスタム値。</span><span class="sxs-lookup"><span data-stu-id="8146f-177">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="8146f-178">デリゲートは、入力としてクライアントによって要求された値を受け取るし、目的の値を返します。</span><span class="sxs-lookup"><span data-stu-id="8146f-178">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="8146f-179">クライアント オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="8146f-179">Configure client options</span></span>

<span data-ttu-id="8146f-180">クライアント オプションを構成でき、`HubConnectionBuilder`型 (.NET、JavaScript クライアントで使用可能)。</span><span class="sxs-lookup"><span data-stu-id="8146f-180">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="8146f-181">Java クライアントで使用できますが、`HttpHubConnectionBuilder`サブクラスはでもビルダー構成オプションが含まれます、`HubConnection`自体。</span><span class="sxs-lookup"><span data-stu-id="8146f-181">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="8146f-182">ログを構成します。</span><span class="sxs-lookup"><span data-stu-id="8146f-182">Configure logging</span></span>

<span data-ttu-id="8146f-183">.NET を使用してクライアントでログ記録が構成されている、`ConfigureLogging`メソッド。</span><span class="sxs-lookup"><span data-stu-id="8146f-183">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="8146f-184">サーバー上にある、同じ方法でログ記録プロバイダーおよびフィルターを登録できます。</span><span class="sxs-lookup"><span data-stu-id="8146f-184">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="8146f-185">参照してください、 [ASP.NET Core でのログ記録](xref:fundamentals/logging/index)詳細についてはドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="8146f-185">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="8146f-186">ログ プロバイダーを登録するために必要なパッケージをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8146f-186">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="8146f-187">参照してください、[組み込みのログ プロバイダー](xref:fundamentals/logging/index#built-in-logging-providers)完全な一覧については、ドキュメントのセクション。</span><span class="sxs-lookup"><span data-stu-id="8146f-187">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="8146f-188">たとえば、コンソールのログ記録を有効にするインストール、 `Microsoft.Extensions.Logging.Console` NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="8146f-188">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="8146f-189">呼び出す、`AddConsole`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="8146f-189">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="8146f-190">同様に、JavaScript クライアント内で`configureLogging`メソッドが存在します。</span><span class="sxs-lookup"><span data-stu-id="8146f-190">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="8146f-191">提供、`LogLevel`を生成するログ メッセージの最小レベルを示す値。</span><span class="sxs-lookup"><span data-stu-id="8146f-191">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="8146f-192">ブラウザーのコンソール ウィンドウには、ログが書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="8146f-192">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="8146f-193">完全ログ記録を無効にするには指定`signalR.LogLevel.None`で、`configureLogging`メソッド。</span><span class="sxs-lookup"><span data-stu-id="8146f-193">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="8146f-194">ログ記録の詳細については、次を参照してください。、 [SignalR 診断のドキュメント](xref:signalr/diagnostics)します。</span><span class="sxs-lookup"><span data-stu-id="8146f-194">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="8146f-195">SignalR の Java クライアントを使用して、 [SLF4J](https://www.slf4j.org/)のログ記録ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="8146f-195">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="8146f-196">ライブラリのユーザーは、特定のログ出力の依存関係に導入することで独自の特定のログ記録の実装を選択できるようにする高度なログ記録 API になります。</span><span class="sxs-lookup"><span data-stu-id="8146f-196">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="8146f-197">次のコード スニペットは、使用する方法を示します`java.util.logging`SignalR Java クライアントを使用します。</span><span class="sxs-lookup"><span data-stu-id="8146f-197">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="8146f-198">依存関係でのログ記録を構成しない場合、SLF4J は、次の警告メッセージが既定の操作なしロガーを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="8146f-198">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="8146f-199">これは無視しても。</span><span class="sxs-lookup"><span data-stu-id="8146f-199">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="8146f-200">許可されているトランスポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="8146f-200">Configure allowed transports</span></span>

<span data-ttu-id="8146f-201">SignalR によって使用されるトランスポートで構成できる、`WithUrl`を呼び出す (`withUrl` JavaScript で)。</span><span class="sxs-lookup"><span data-stu-id="8146f-201">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="8146f-202">値のビットごとの OR`HttpTransportType`のみ指定されたトランスポートを使用するクライアントを制限するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="8146f-202">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="8146f-203">既定では、すべてのトランスポートが有効にします。</span><span class="sxs-lookup"><span data-stu-id="8146f-203">All transports are enabled by default.</span></span>

<span data-ttu-id="8146f-204">たとえば、Server-Sent イベント転送を無効にすることを許可する WebSockets および長いポーリングの接続。</span><span class="sxs-lookup"><span data-stu-id="8146f-204">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="8146f-205">設定して、JavaScript クライアントでのトランスポートが構成されている、`transport`フィールドに指定されたオプションのオブジェクトに`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="8146f-205">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="8146f-206">このバージョンの Java では、クライアント websocket は、のみ使用可能なトランスポートです。</span><span class="sxs-lookup"><span data-stu-id="8146f-206">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="8146f-207">Java クライアント トランスポートが選択されている、`withTransport`メソッドを`HttpHubConnectionBuilder`します。</span><span class="sxs-lookup"><span data-stu-id="8146f-207">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="8146f-208">Java クライアント Websocket トランスポートを使用して既定値です。</span><span class="sxs-lookup"><span data-stu-id="8146f-208">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```
> [!NOTE]
> <span data-ttu-id="8146f-209">SignalR の Java クライアントは、まだトランスポートにフォールバックをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="8146f-209">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="8146f-210">ベアラー認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="8146f-210">Configure bearer authentication</span></span>

<span data-ttu-id="8146f-211">SignalR 要求と共に認証データを提供するには、使用、`AccessTokenProvider`オプション (`accessTokenFactory` JavaScript で) 必要なアクセス トークンを返す関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="8146f-211">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="8146f-212">.NET クライアントでこのアクセス トークンとして渡される HTTP「ベアラー認証」トークン (を使用して、`Authorization`ヘッダーの型を持つ`Bearer`)。</span><span class="sxs-lookup"><span data-stu-id="8146f-212">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="8146f-213">アクセス トークンをベアラー トークンとして使用、JavaScript クライアントで**を除く**ブラウザー Api が (具体的には、Server-Sent イベントと Websocket の要求) のヘッダーを適用する機能を制限するような場合。</span><span class="sxs-lookup"><span data-stu-id="8146f-213">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="8146f-214">このような場合は、アクセス トークンはクエリ文字列値として指定`access_token`します。</span><span class="sxs-lookup"><span data-stu-id="8146f-214">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="8146f-215">.NET クライアントで、`AccessTokenProvider`でオプションのデリゲートを使用してオプションを指定することができます`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="8146f-215">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="8146f-216">設定して、JavaScript クライアントでアクセス トークンが構成されている、`accessTokenFactory`フィールド内のオプション オブジェクトに`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="8146f-216">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```


<span data-ttu-id="8146f-217">アクセス トークン ファクトリを提供することで、認証に使用するベアラー トークンを構成する、SignalR の Java クライアントで、 [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)します。</span><span class="sxs-lookup"><span data-stu-id="8146f-217">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="8146f-218">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)を提供する、 [RxJava](https://github.com/ReactiveX/RxJava) [単一<String>](http://reactivex.io/documentation/single.html)します。</span><span class="sxs-lookup"><span data-stu-id="8146f-218">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="8146f-219">呼び出して[Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)クライアントのアクセス トークンを生成するロジックを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="8146f-219">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="8146f-220">タイムアウトとキープ アライブ オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="8146f-220">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="8146f-221">タイムアウトとキープ アライブ動作を構成するための追加のオプションがあります、`HubConnection`オブジェクト自体。</span><span class="sxs-lookup"><span data-stu-id="8146f-221">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="8146f-222">.NET</span><span class="sxs-lookup"><span data-stu-id="8146f-222">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="8146f-223">オプション</span><span class="sxs-lookup"><span data-stu-id="8146f-223">Option</span></span> | <span data-ttu-id="8146f-224">既定値</span><span class="sxs-lookup"><span data-stu-id="8146f-224">Default value</span></span> | <span data-ttu-id="8146f-225">説明</span><span class="sxs-lookup"><span data-stu-id="8146f-225">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="8146f-226">30 秒 (30,000 ミリ秒)</span><span class="sxs-lookup"><span data-stu-id="8146f-226">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="8146f-227">サーバーの利用状況のタイムアウト。</span><span class="sxs-lookup"><span data-stu-id="8146f-227">Timeout for server activity.</span></span> <span data-ttu-id="8146f-228">サーバーは、この間隔でメッセージを送信していない、クライアントと見なされ、サーバーが切断されているトリガー、`Closed`イベント (`onclose` JavaScript で)。</span><span class="sxs-lookup"><span data-stu-id="8146f-228">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="8146f-229">この値は、サーバーから送信される ping メッセージに十分な大きさである必要があります**と**タイムアウト間隔内に、クライアントによって受信されます。</span><span class="sxs-lookup"><span data-stu-id="8146f-229">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="8146f-230">推奨値は数値、サーバーに少なくとも 2 倍の`KeepAliveInterval`の ping が到着するまでの値。</span><span class="sxs-lookup"><span data-stu-id="8146f-230">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="8146f-231">15 秒</span><span class="sxs-lookup"><span data-stu-id="8146f-231">15 seconds</span></span> | <span data-ttu-id="8146f-232">サーバーの初期ハンドシェイクのタイムアウト。</span><span class="sxs-lookup"><span data-stu-id="8146f-232">Timeout for initial server handshake.</span></span> <span data-ttu-id="8146f-233">サーバーは、この間隔でハンドシェイクの応答を送信しない場合、は、クライアントがキャンセル ハンドシェイクとトリガー、`Closed`イベント (`onclose` JavaScript で)。</span><span class="sxs-lookup"><span data-stu-id="8146f-233">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="8146f-234">これは、重大なネットワーク待機時間が原因のハンドシェイクのタイムアウト エラーが発生している場合にのみ変更する高度な設定です。</span><span class="sxs-lookup"><span data-stu-id="8146f-234">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8146f-235">ハンドシェイク プロセスの詳細については、次を参照してください。、 [SignalR ハブ プロトコル仕様](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)します。</span><span class="sxs-lookup"><span data-stu-id="8146f-235">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="8146f-236">として、.NET クライアントでタイムアウト値が指定された`TimeSpan`値。</span><span class="sxs-lookup"><span data-stu-id="8146f-236">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="8146f-237">JavaScript</span><span class="sxs-lookup"><span data-stu-id="8146f-237">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="8146f-238">オプション</span><span class="sxs-lookup"><span data-stu-id="8146f-238">Option</span></span> | <span data-ttu-id="8146f-239">既定値</span><span class="sxs-lookup"><span data-stu-id="8146f-239">Default value</span></span> | <span data-ttu-id="8146f-240">説明</span><span class="sxs-lookup"><span data-stu-id="8146f-240">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="8146f-241">30 秒 (30,000 ミリ秒)</span><span class="sxs-lookup"><span data-stu-id="8146f-241">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="8146f-242">サーバーの利用状況のタイムアウト。</span><span class="sxs-lookup"><span data-stu-id="8146f-242">Timeout for server activity.</span></span> <span data-ttu-id="8146f-243">サーバーは、この間隔でメッセージを送信していない、クライアントと見なされ、サーバーが切断されているトリガー、`onclose`イベント。</span><span class="sxs-lookup"><span data-stu-id="8146f-243">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="8146f-244">この値は、サーバーから送信される ping メッセージに十分な大きさである必要があります**と**タイムアウト間隔内に、クライアントによって受信されます。</span><span class="sxs-lookup"><span data-stu-id="8146f-244">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="8146f-245">推奨値は数値、サーバーに少なくとも 2 倍の`KeepAliveInterval`の ping が到着するまでの値。</span><span class="sxs-lookup"><span data-stu-id="8146f-245">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="8146f-246">Java</span><span class="sxs-lookup"><span data-stu-id="8146f-246">Java</span></span>](#tab/java)

| <span data-ttu-id="8146f-247">オプション</span><span class="sxs-lookup"><span data-stu-id="8146f-247">Option</span></span> | <span data-ttu-id="8146f-248">既定値</span><span class="sxs-lookup"><span data-stu-id="8146f-248">Default value</span></span> | <span data-ttu-id="8146f-249">説明</span><span class="sxs-lookup"><span data-stu-id="8146f-249">Description</span></span> |
| ----------- | ------------- | ----------- |
|<span data-ttu-id="8146f-250">`getServerTimeout` `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="8146f-250">`getServerTimeout` `setServerTimeout`</span></span> | <span data-ttu-id="8146f-251">30 秒 (30,000 ミリ秒)</span><span class="sxs-lookup"><span data-stu-id="8146f-251">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="8146f-252">サーバーの利用状況のタイムアウト。</span><span class="sxs-lookup"><span data-stu-id="8146f-252">Timeout for server activity.</span></span> <span data-ttu-id="8146f-253">サーバーは、この間隔でメッセージを送信していない、クライアントと見なされ、サーバーが切断されているトリガー、`onClose`イベント。</span><span class="sxs-lookup"><span data-stu-id="8146f-253">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="8146f-254">この値は、サーバーから送信される ping メッセージに十分な大きさである必要があります**と**タイムアウト間隔内に、クライアントによって受信されます。</span><span class="sxs-lookup"><span data-stu-id="8146f-254">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="8146f-255">推奨値は数値、サーバーに少なくとも 2 倍の`KeepAliveInterval`の ping が到着するまでの値。</span><span class="sxs-lookup"><span data-stu-id="8146f-255">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="8146f-256">15 秒</span><span class="sxs-lookup"><span data-stu-id="8146f-256">15 seconds</span></span> | <span data-ttu-id="8146f-257">サーバーの初期ハンドシェイクのタイムアウト。</span><span class="sxs-lookup"><span data-stu-id="8146f-257">Timeout for initial server handshake.</span></span> <span data-ttu-id="8146f-258">サーバーは、この間隔でハンドシェイクの応答を送信しない場合、は、クライアントがキャンセル ハンドシェイクとトリガー、`onClose`イベント。</span><span class="sxs-lookup"><span data-stu-id="8146f-258">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="8146f-259">これは、重大なネットワーク待機時間が原因のハンドシェイクのタイムアウト エラーが発生している場合にのみ変更する高度な設定です。</span><span class="sxs-lookup"><span data-stu-id="8146f-259">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8146f-260">ハンドシェイク プロセスの詳細については、次を参照してください。、 [SignalR ハブ プロトコル仕様](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)します。</span><span class="sxs-lookup"><span data-stu-id="8146f-260">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="8146f-261">追加のオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="8146f-261">Configure additional options</span></span>

<span data-ttu-id="8146f-262">追加のオプションを構成することができます、 `WithUrl` (`withUrl` JavaScript で) メソッドを`HubConnectionBuilder`やさまざまな構成 Api を上、 `HttpHubConnectionBuilder` Java クライアント。</span><span class="sxs-lookup"><span data-stu-id="8146f-262">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="8146f-263">.NET</span><span class="sxs-lookup"><span data-stu-id="8146f-263">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="8146f-264">.NET オプション</span><span class="sxs-lookup"><span data-stu-id="8146f-264">.NET Option</span></span> |  <span data-ttu-id="8146f-265">既定値</span><span class="sxs-lookup"><span data-stu-id="8146f-265">Default value</span></span> | <span data-ttu-id="8146f-266">説明</span><span class="sxs-lookup"><span data-stu-id="8146f-266">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="8146f-267">HTTP 要求でベアラー認証トークンとして提供されている文字列を返す関数。</span><span class="sxs-lookup"><span data-stu-id="8146f-267">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="8146f-268">これを設定`true`ネゴシエーションの手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="8146f-268">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="8146f-269">**Websocket トランスポートがトランスポートには、のみの有効な場合にのみサポート**します。</span><span class="sxs-lookup"><span data-stu-id="8146f-269">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="8146f-270">Azure SignalR サービスを使用する場合は、この設定を有効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8146f-270">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="8146f-271">Empty</span><span class="sxs-lookup"><span data-stu-id="8146f-271">Empty</span></span> | <span data-ttu-id="8146f-272">要求の認証に送信する TLS 証明書のコレクション。</span><span class="sxs-lookup"><span data-stu-id="8146f-272">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="8146f-273">Empty</span><span class="sxs-lookup"><span data-stu-id="8146f-273">Empty</span></span> | <span data-ttu-id="8146f-274">すべての HTTP 要求を送信する HTTP クッキーのコレクション。</span><span class="sxs-lookup"><span data-stu-id="8146f-274">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="8146f-275">Empty</span><span class="sxs-lookup"><span data-stu-id="8146f-275">Empty</span></span> | <span data-ttu-id="8146f-276">すべての HTTP 要求を送信する資格情報です。</span><span class="sxs-lookup"><span data-stu-id="8146f-276">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="8146f-277">5 秒</span><span class="sxs-lookup"><span data-stu-id="8146f-277">5 seconds</span></span> | <span data-ttu-id="8146f-278">Websocket だけです。</span><span class="sxs-lookup"><span data-stu-id="8146f-278">WebSockets only.</span></span> <span data-ttu-id="8146f-279">最長時間、クライアントは、サーバーが閉じる要求を確認するための終了後に待機します。</span><span class="sxs-lookup"><span data-stu-id="8146f-279">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="8146f-280">サーバーは、この時間内終了を確認は、クライアントが切断されます。</span><span class="sxs-lookup"><span data-stu-id="8146f-280">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="8146f-281">Empty</span><span class="sxs-lookup"><span data-stu-id="8146f-281">Empty</span></span> | <span data-ttu-id="8146f-282">すべての HTTP 要求を送信する追加の HTTP ヘッダーのマップです。</span><span class="sxs-lookup"><span data-stu-id="8146f-282">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="8146f-283">構成または置換するために使用するデリゲート、 `HttpMessageHandler` HTTP 要求を送信するために使用します。</span><span class="sxs-lookup"><span data-stu-id="8146f-283">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="8146f-284">WebSocket 接続に使用されません。</span><span class="sxs-lookup"><span data-stu-id="8146f-284">Not used for WebSocket connections.</span></span> <span data-ttu-id="8146f-285">このデリゲートは、null 以外の値を返す必要があり、既定値をパラメーターとして受け取ります。</span><span class="sxs-lookup"><span data-stu-id="8146f-285">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="8146f-286">その既定値の設定を変更し、それを返すかが返さ新しい`HttpMessageHandler`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="8146f-286">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="8146f-287">**ときは、ハンドラーを置き換える、指定したハンドラーから保持する設定をコピーすることを確認して、それ以外の場合、(Cookie やヘッダー) の構成オプションが新しいハンドラーを適用されません。**</span><span class="sxs-lookup"><span data-stu-id="8146f-287">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="8146f-288">HTTP 要求を送信するときに使用する HTTP プロキシ。</span><span class="sxs-lookup"><span data-stu-id="8146f-288">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="8146f-289">HTTP と Websocket の要求の既定の資格情報を送信するこのブール値を設定します。</span><span class="sxs-lookup"><span data-stu-id="8146f-289">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="8146f-290">これにより、Windows 認証を使用できます。</span><span class="sxs-lookup"><span data-stu-id="8146f-290">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="8146f-291">追加の WebSocket オプションを構成するために使用するデリゲート。</span><span class="sxs-lookup"><span data-stu-id="8146f-291">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="8146f-292">インスタンスを受け取る[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)オプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="8146f-292">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="8146f-293">JavaScript</span><span class="sxs-lookup"><span data-stu-id="8146f-293">JavaScript</span></span>](#tab/javascript)
| <span data-ttu-id="8146f-294">JavaScript のオプション</span><span class="sxs-lookup"><span data-stu-id="8146f-294">JavaScript Option</span></span> | <span data-ttu-id="8146f-295">既定値</span><span class="sxs-lookup"><span data-stu-id="8146f-295">Default Value</span></span> | <span data-ttu-id="8146f-296">説明</span><span class="sxs-lookup"><span data-stu-id="8146f-296">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="8146f-297">HTTP 要求でベアラー認証トークンとして提供されている文字列を返す関数。</span><span class="sxs-lookup"><span data-stu-id="8146f-297">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="8146f-298">これを設定`true`ネゴシエーションの手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="8146f-298">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="8146f-299">**Websocket トランスポートがトランスポートには、のみの有効な場合にのみサポート**します。</span><span class="sxs-lookup"><span data-stu-id="8146f-299">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="8146f-300">Azure SignalR サービスを使用する場合は、この設定を有効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8146f-300">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="8146f-301">Java</span><span class="sxs-lookup"><span data-stu-id="8146f-301">Java</span></span>](#tab/java)
| <span data-ttu-id="8146f-302">Java オプション</span><span class="sxs-lookup"><span data-stu-id="8146f-302">Java Option</span></span> | <span data-ttu-id="8146f-303">既定値</span><span class="sxs-lookup"><span data-stu-id="8146f-303">Default Value</span></span> | <span data-ttu-id="8146f-304">説明</span><span class="sxs-lookup"><span data-stu-id="8146f-304">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="8146f-305">HTTP 要求でベアラー認証トークンとして提供されている文字列を返す関数。</span><span class="sxs-lookup"><span data-stu-id="8146f-305">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="8146f-306">これを設定`true`ネゴシエーションの手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="8146f-306">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="8146f-307">**Websocket トランスポートがトランスポートには、のみの有効な場合にのみサポート**します。</span><span class="sxs-lookup"><span data-stu-id="8146f-307">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="8146f-308">Azure SignalR サービスを使用する場合は、この設定を有効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8146f-308">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="8146f-309">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="8146f-309">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="8146f-310">Empty</span><span class="sxs-lookup"><span data-stu-id="8146f-310">Empty</span></span> | <span data-ttu-id="8146f-311">すべての HTTP 要求を送信する追加の HTTP ヘッダーのマップです。</span><span class="sxs-lookup"><span data-stu-id="8146f-311">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="8146f-312">指定されたオプションのデリゲートで、.NET クライアントでこれらのオプションを変更できます`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="8146f-312">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="8146f-313">指定された JavaScript オブジェクトで、JavaScript クライアントでこれらのオプションを指定できる`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="8146f-313">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="8146f-314">Java クライアントでこれらのオプションで構成できますメソッドを使って、`HttpHubConnectionBuilder`から返される、 `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="8146f-314">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>


```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="8146f-315">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="8146f-315">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
