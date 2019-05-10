---
title: ASP.NET Core SignalR の構成
author: bradygaster
description: ASP.NET Core SignalR アプリケーションを構成する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/15/2019
uid: signalr/configuration
ms.openlocfilehash: 703357fd52805e01515e5bac3b1a364ce7fe00f0
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087650"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="59656-103">ASP.NET Core SignalR の構成</span><span class="sxs-lookup"><span data-stu-id="59656-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="59656-104">JSON/MessagePack シリアル化オプション</span><span class="sxs-lookup"><span data-stu-id="59656-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="59656-105">ASP.NET Core SignalR には、メッセージをエンコードするための 2 つのプロトコルがサポートされています。[JSON](https://www.json.org/)と[MessagePack](https://msgpack.org/index.html)します。</span><span class="sxs-lookup"><span data-stu-id="59656-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="59656-106">各プロトコルには、シリアル化の構成オプションがあります。</span><span class="sxs-lookup"><span data-stu-id="59656-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="59656-107">JSON のシリアル化を使用して、サーバーで構成できます、 [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)拡張メソッドは後に、追加する[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)で、`Startup.ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="59656-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="59656-108">`AddJsonProtocol`メソッドが受信するデリゲートを受け取り、`options`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="59656-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="59656-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)そのオブジェクトのプロパティは、JSON.NET`JsonSerializerSettings`引数のシリアル化の構成し、戻り値を使用できるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="59656-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="59656-110">参照してください、 [JSON.NET のドキュメント](https://www.newtonsoft.com/json/help/html/Introduction.htm)の詳細。</span><span class="sxs-lookup"><span data-stu-id="59656-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="59656-111">たとえば、「キャメル ケース」の既定名ではなく"PascalCase"プロパティの名前を使用するシリアライザーを構成する次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="59656-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="59656-112">.NET クライアントで同じ`AddJsonProtocol`に拡張メソッドが存在する[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)します。</span><span class="sxs-lookup"><span data-stu-id="59656-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="59656-113">`Microsoft.Extensions.DependencyInjection`拡張メソッドを解決するのには名前空間をインポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="59656-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="59656-114">この時点で、JavaScript クライアント内で JSON のシリアル化を構成することはできません。</span><span class="sxs-lookup"><span data-stu-id="59656-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="59656-115">MessagePack シリアル化オプション</span><span class="sxs-lookup"><span data-stu-id="59656-115">MessagePack serialization options</span></span>

<span data-ttu-id="59656-116">デリゲートを提供することで MessagePack シリアル化を構成することができます、 [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼び出します。</span><span class="sxs-lookup"><span data-stu-id="59656-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="59656-117">参照してください[SignalR で MessagePack](xref:signalr/messagepackhubprotocol)の詳細。</span><span class="sxs-lookup"><span data-stu-id="59656-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="59656-118">この時点で、JavaScript クライアント内で MessagePack シリアル化を構成することはできません。</span><span class="sxs-lookup"><span data-stu-id="59656-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="59656-119">サーバー オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="59656-119">Configure server options</span></span>

<span data-ttu-id="59656-120">次の表では、SignalR ハブを構成するためのオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="59656-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="59656-121">オプション</span><span class="sxs-lookup"><span data-stu-id="59656-121">Option</span></span> | <span data-ttu-id="59656-122">既定値</span><span class="sxs-lookup"><span data-stu-id="59656-122">Default Value</span></span> | <span data-ttu-id="59656-123">説明</span><span class="sxs-lookup"><span data-stu-id="59656-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="59656-124">30 秒</span><span class="sxs-lookup"><span data-stu-id="59656-124">30 seconds</span></span> | <span data-ttu-id="59656-125">サーバーは、クライアントを検討してください、この間隔で (キープ アライブを含む) メッセージを受信していない場合は切断されます。</span><span class="sxs-lookup"><span data-stu-id="59656-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="59656-126">実際にマーク済みであるため、これを実装する方法、切断されたクライアントの場合は、このタイムアウト間隔よりも長い時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="59656-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="59656-127">推奨値は二重、`KeepAliveInterval`値。</span><span class="sxs-lookup"><span data-stu-id="59656-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="59656-128">15 秒</span><span class="sxs-lookup"><span data-stu-id="59656-128">15 seconds</span></span> | <span data-ttu-id="59656-129">クライアントがこの時間間隔内で最初のハンドシェイク メッセージを送信しない場合、接続は閉じられます。</span><span class="sxs-lookup"><span data-stu-id="59656-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="59656-130">これは、重大なネットワーク待機時間が原因のハンドシェイクのタイムアウト エラーが発生している場合にのみ変更する高度な設定です。</span><span class="sxs-lookup"><span data-stu-id="59656-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="59656-131">ハンドシェイク プロセスの詳細については、次を参照してください。、 [SignalR ハブ プロトコル仕様](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)します。</span><span class="sxs-lookup"><span data-stu-id="59656-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="59656-132">15 秒</span><span class="sxs-lookup"><span data-stu-id="59656-132">15 seconds</span></span> | <span data-ttu-id="59656-133">サーバーがこの間隔内でメッセージを送信していない場合、接続を開いたままに ping メッセージが自動的に送信されます。</span><span class="sxs-lookup"><span data-stu-id="59656-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="59656-134">変更するときに`KeepAliveInterval`、変更、 `ServerTimeout` / `serverTimeoutInMilliseconds`クライアントに設定します。</span><span class="sxs-lookup"><span data-stu-id="59656-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="59656-135">推奨される`ServerTimeout` / `serverTimeoutInMilliseconds`値が double、`KeepAliveInterval`値。</span><span class="sxs-lookup"><span data-stu-id="59656-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="59656-136">インストールされているすべてのプロトコル</span><span class="sxs-lookup"><span data-stu-id="59656-136">All installed protocols</span></span> | <span data-ttu-id="59656-137">このハブでサポートされるプロトコル。</span><span class="sxs-lookup"><span data-stu-id="59656-137">Protocols supported by this hub.</span></span> <span data-ttu-id="59656-138">既定では、サーバーに登録されているプロトコルをすべて許可されますが、プロトコルは、個々 のハブに対する特定のプロトコルを無効にするには、この一覧から削除できます。</span><span class="sxs-lookup"><span data-stu-id="59656-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="59656-139">場合`true`詳細のハブ メソッドで例外がスローされたときに、例外メッセージがクライアントに返されます。</span><span class="sxs-lookup"><span data-stu-id="59656-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="59656-140">既定値は`false`、これらの例外メッセージは、機密情報を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="59656-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="59656-141">オプションのデリゲートを提供することですべてのハブのオプションを構成でき、`AddSignalR`呼び出す`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="59656-141">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="59656-142">1 つのハブのオプションで提供されるグローバル オプションを上書き`AddSignalR`を使用して構成できると<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="59656-142">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="59656-143">高度な HTTP 構成オプション</span><span class="sxs-lookup"><span data-stu-id="59656-143">Advanced HTTP configuration options</span></span>

<span data-ttu-id="59656-144">使用`HttpConnectionDispatcherOptions`トランスポートおよびメモリ バッファーの管理に関連する高度な設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="59656-144">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="59656-145">これらのオプションを構成するデリゲートを渡すことによって[MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)で`Startup.Configure`します。</span><span class="sxs-lookup"><span data-stu-id="59656-145">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="59656-146">次の表では、ASP.NET Core SignalR の高度な HTTP オプションを構成するためのオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="59656-146">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="59656-147">オプション</span><span class="sxs-lookup"><span data-stu-id="59656-147">Option</span></span> | <span data-ttu-id="59656-148">既定値</span><span class="sxs-lookup"><span data-stu-id="59656-148">Default Value</span></span> | <span data-ttu-id="59656-149">説明</span><span class="sxs-lookup"><span data-stu-id="59656-149">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="59656-150">32 KB</span><span class="sxs-lookup"><span data-stu-id="59656-150">32 KB</span></span> | <span data-ttu-id="59656-151">クライアントから受信したバイトの最大数をサーバーのバッファー。</span><span class="sxs-lookup"><span data-stu-id="59656-151">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="59656-152">この値を増やすことによりより大きなメッセージを受信するサーバーが、メモリ消費量に悪影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="59656-152">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="59656-153">データが自動的に収集、`Authorize`ハブ クラスに適用される属性。</span><span class="sxs-lookup"><span data-stu-id="59656-153">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="59656-154">一連の[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)オブジェクトは、クライアントがハブへの接続を承認されているかどうかを判断するために使用します。</span><span class="sxs-lookup"><span data-stu-id="59656-154">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="59656-155">32 KB</span><span class="sxs-lookup"><span data-stu-id="59656-155">32 KB</span></span> | <span data-ttu-id="59656-156">アプリによって送信されたバイト数の最大数をサーバーのバッファー。</span><span class="sxs-lookup"><span data-stu-id="59656-156">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="59656-157">この値を増やすことによりより大きなメッセージを送信するサーバーが、メモリ消費量に悪影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="59656-157">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="59656-158">すべてのトランスポートが有効になります。</span><span class="sxs-lookup"><span data-stu-id="59656-158">All Transports are enabled.</span></span> | <span data-ttu-id="59656-159">ビットマスク`HttpTransportType`トランスポートを制限する値をクライアントが接続に使用できます。</span><span class="sxs-lookup"><span data-stu-id="59656-159">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="59656-160">以下を参照してください。</span><span class="sxs-lookup"><span data-stu-id="59656-160">See below.</span></span> | <span data-ttu-id="59656-161">長いポーリングのトランスポートに固有の追加オプション。</span><span class="sxs-lookup"><span data-stu-id="59656-161">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="59656-162">以下を参照してください。</span><span class="sxs-lookup"><span data-stu-id="59656-162">See below.</span></span> | <span data-ttu-id="59656-163">Websocket トランスポートに固有の追加オプション。</span><span class="sxs-lookup"><span data-stu-id="59656-163">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="59656-164">ポーリングの長いトランスポートを使用して構成できるその他のオプションを持つ、`LongPolling`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="59656-164">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="59656-165">オプション</span><span class="sxs-lookup"><span data-stu-id="59656-165">Option</span></span> | <span data-ttu-id="59656-166">既定値</span><span class="sxs-lookup"><span data-stu-id="59656-166">Default Value</span></span> | <span data-ttu-id="59656-167">説明</span><span class="sxs-lookup"><span data-stu-id="59656-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="59656-168">90 秒</span><span class="sxs-lookup"><span data-stu-id="59656-168">90 seconds</span></span> | <span data-ttu-id="59656-169">1 つのポーリング要求を終了する前に、クライアントに送信するメッセージのサーバーが待機する最長時間。</span><span class="sxs-lookup"><span data-stu-id="59656-169">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="59656-170">新しいポーリング要求をより頻繁に発行するクライアントは、この値を小さきます。</span><span class="sxs-lookup"><span data-stu-id="59656-170">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="59656-171">WebSocket トランスポートが追加のオプションを使用して構成できる、`WebSockets`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="59656-171">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="59656-172">オプション</span><span class="sxs-lookup"><span data-stu-id="59656-172">Option</span></span> | <span data-ttu-id="59656-173">既定値</span><span class="sxs-lookup"><span data-stu-id="59656-173">Default Value</span></span> | <span data-ttu-id="59656-174">説明</span><span class="sxs-lookup"><span data-stu-id="59656-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="59656-175">5 秒</span><span class="sxs-lookup"><span data-stu-id="59656-175">5 seconds</span></span> | <span data-ttu-id="59656-176">サーバーが閉じ、クライアントがこの時間間隔内に閉じるが失敗した場合、接続は終了します。</span><span class="sxs-lookup"><span data-stu-id="59656-176">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="59656-177">設定するために使用するデリゲート、`Sec-WebSocket-Protocol`ヘッダーをカスタム値。</span><span class="sxs-lookup"><span data-stu-id="59656-177">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="59656-178">デリゲートは、入力としてクライアントによって要求された値を受け取るし、目的の値を返します。</span><span class="sxs-lookup"><span data-stu-id="59656-178">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="59656-179">クライアント オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="59656-179">Configure client options</span></span>

<span data-ttu-id="59656-180">クライアント オプションを構成でき、`HubConnectionBuilder`型 (.NET、JavaScript クライアントで使用可能)。</span><span class="sxs-lookup"><span data-stu-id="59656-180">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="59656-181">Java クライアントで使用できますが、`HttpHubConnectionBuilder`サブクラスはでもビルダー構成オプションが含まれます、`HubConnection`自体。</span><span class="sxs-lookup"><span data-stu-id="59656-181">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="59656-182">ログを構成します。</span><span class="sxs-lookup"><span data-stu-id="59656-182">Configure logging</span></span>

<span data-ttu-id="59656-183">.NET を使用してクライアントでログ記録が構成されている、`ConfigureLogging`メソッド。</span><span class="sxs-lookup"><span data-stu-id="59656-183">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="59656-184">サーバー上にある、同じ方法でログ記録プロバイダーおよびフィルターを登録できます。</span><span class="sxs-lookup"><span data-stu-id="59656-184">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="59656-185">参照してください、 [ASP.NET Core でのログ記録](xref:fundamentals/logging/index)詳細についてはドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="59656-185">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="59656-186">ログ プロバイダーを登録するために必要なパッケージをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="59656-186">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="59656-187">参照してください、[組み込みのログ プロバイダー](xref:fundamentals/logging/index#built-in-logging-providers)完全な一覧については、ドキュメントのセクション。</span><span class="sxs-lookup"><span data-stu-id="59656-187">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="59656-188">たとえば、コンソールのログ記録を有効にするインストール、 `Microsoft.Extensions.Logging.Console` NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="59656-188">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="59656-189">呼び出す、`AddConsole`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="59656-189">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="59656-190">同様に、JavaScript クライアント内で`configureLogging`メソッドが存在します。</span><span class="sxs-lookup"><span data-stu-id="59656-190">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="59656-191">提供、`LogLevel`を生成するログ メッセージの最小レベルを示す値。</span><span class="sxs-lookup"><span data-stu-id="59656-191">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="59656-192">ブラウザーのコンソール ウィンドウには、ログが書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="59656-192">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="59656-193">代わりに、`LogLevel`値を提供することも、`string`ログのレベル名を表す値。</span><span class="sxs-lookup"><span data-stu-id="59656-193">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="59656-194">アクセスの必要がない SignalR 環境でのログ記録を構成するときに便利ですが、`LogLevel`定数。</span><span class="sxs-lookup"><span data-stu-id="59656-194">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="59656-195">次の表では、使用可能なログ レベルを示します。</span><span class="sxs-lookup"><span data-stu-id="59656-195">The following table lists the available log levels.</span></span> <span data-ttu-id="59656-196">指定した値`configureLogging`設定、**最小**ログ レベルをログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="59656-196">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="59656-197">このレベルで記録されたメッセージ**レベルがその後に、表に示すまたは**、ログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="59656-197">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="59656-198">string</span><span class="sxs-lookup"><span data-stu-id="59656-198">string</span></span> | <span data-ttu-id="59656-199">LogLevel</span><span class="sxs-lookup"><span data-stu-id="59656-199">LogLevel</span></span> |
| - | - |
| `"trace"` | `LogLevel.Trace` |
| `"debug"` | `LogLevel.Debug` |
| <span data-ttu-id="59656-200">`"info"` **または** `"information"`</span><span class="sxs-lookup"><span data-stu-id="59656-200">`"info"` **or** `"information"`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="59656-201">`"warn"` **または** `"warning"`</span><span class="sxs-lookup"><span data-stu-id="59656-201">`"warn"` **or** `"warning"`</span></span> | `LogLevel.Warning` |
| `"error"` | `LogLevel.Error` |
| `"critical"` | `LogLevel.Critical` |
| `"none"` | `LogLevel.None` |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="59656-202">完全ログ記録を無効にするには指定`signalR.LogLevel.None`で、`configureLogging`メソッド。</span><span class="sxs-lookup"><span data-stu-id="59656-202">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="59656-203">ログ記録の詳細については、次を参照してください。、 [SignalR 診断のドキュメント](xref:signalr/diagnostics)します。</span><span class="sxs-lookup"><span data-stu-id="59656-203">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="59656-204">SignalR の Java クライアントを使用して、 [SLF4J](https://www.slf4j.org/)のログ記録ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="59656-204">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="59656-205">ライブラリのユーザーは、特定のログ出力の依存関係に導入することで独自の特定のログ記録の実装を選択できるようにする高度なログ記録 API になります。</span><span class="sxs-lookup"><span data-stu-id="59656-205">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="59656-206">次のコード スニペットは、使用する方法を示します`java.util.logging`SignalR Java クライアントを使用します。</span><span class="sxs-lookup"><span data-stu-id="59656-206">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="59656-207">依存関係でのログ記録を構成しない場合、SLF4J は、次の警告メッセージが既定の操作なしロガーを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="59656-207">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="59656-208">これは無視しても。</span><span class="sxs-lookup"><span data-stu-id="59656-208">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="59656-209">許可されているトランスポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="59656-209">Configure allowed transports</span></span>

<span data-ttu-id="59656-210">SignalR によって使用されるトランスポートで構成できる、`WithUrl`を呼び出す (`withUrl` JavaScript で)。</span><span class="sxs-lookup"><span data-stu-id="59656-210">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="59656-211">値のビットごとの OR`HttpTransportType`のみ指定されたトランスポートを使用するクライアントを制限するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="59656-211">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="59656-212">既定では、すべてのトランスポートが有効にします。</span><span class="sxs-lookup"><span data-stu-id="59656-212">All transports are enabled by default.</span></span>

<span data-ttu-id="59656-213">たとえば、Server-Sent イベント転送を無効にすることを許可する WebSockets および長いポーリングの接続。</span><span class="sxs-lookup"><span data-stu-id="59656-213">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="59656-214">設定して、JavaScript クライアントでのトランスポートが構成されている、`transport`フィールドに指定されたオプションのオブジェクトに`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="59656-214">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="59656-215">このバージョンの Java では、クライアント websocket は、のみ使用可能なトランスポートです。</span><span class="sxs-lookup"><span data-stu-id="59656-215">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="59656-216">Java クライアント トランスポートが選択されている、`withTransport`メソッドを`HttpHubConnectionBuilder`します。</span><span class="sxs-lookup"><span data-stu-id="59656-216">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="59656-217">Java クライアント Websocket トランスポートを使用して既定値です。</span><span class="sxs-lookup"><span data-stu-id="59656-217">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="59656-218">SignalR の Java クライアントは、まだトランスポートにフォールバックをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="59656-218">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="59656-219">ベアラー認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="59656-219">Configure bearer authentication</span></span>

<span data-ttu-id="59656-220">SignalR 要求と共に認証データを提供するには、使用、`AccessTokenProvider`オプション (`accessTokenFactory` JavaScript で) 必要なアクセス トークンを返す関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="59656-220">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="59656-221">.NET クライアントでこのアクセス トークンとして渡される HTTP「ベアラー認証」トークン (を使用して、`Authorization`ヘッダーの型を持つ`Bearer`)。</span><span class="sxs-lookup"><span data-stu-id="59656-221">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="59656-222">アクセス トークンをベアラー トークンとして使用、JavaScript クライアントで**を除く**ブラウザー Api が (具体的には、Server-Sent イベントと Websocket の要求) のヘッダーを適用する機能を制限するような場合。</span><span class="sxs-lookup"><span data-stu-id="59656-222">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="59656-223">このような場合は、アクセス トークンはクエリ文字列値として指定`access_token`します。</span><span class="sxs-lookup"><span data-stu-id="59656-223">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="59656-224">.NET クライアントで、`AccessTokenProvider`でオプションのデリゲートを使用してオプションを指定することができます`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="59656-224">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="59656-225">設定して、JavaScript クライアントでアクセス トークンが構成されている、`accessTokenFactory`フィールド内のオプション オブジェクトに`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="59656-225">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="59656-226">アクセス トークン ファクトリを提供することで、認証に使用するベアラー トークンを構成する、SignalR の Java クライアントで、 [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)します。</span><span class="sxs-lookup"><span data-stu-id="59656-226">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="59656-227">使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)を提供する、 [RxJava](https://github.com/ReactiveX/RxJava) [単一\<文字列 >](http://reactivex.io/documentation/single.html)します。</span><span class="sxs-lookup"><span data-stu-id="59656-227">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="59656-228">呼び出して[Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)クライアントのアクセス トークンを生成するロジックを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="59656-228">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="59656-229">タイムアウトとキープ アライブ オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="59656-229">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="59656-230">タイムアウトとキープ アライブ動作を構成するための追加のオプションがあります、`HubConnection`オブジェクト自体。</span><span class="sxs-lookup"><span data-stu-id="59656-230">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="59656-231">.NET</span><span class="sxs-lookup"><span data-stu-id="59656-231">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="59656-232">オプション</span><span class="sxs-lookup"><span data-stu-id="59656-232">Option</span></span> | <span data-ttu-id="59656-233">既定値</span><span class="sxs-lookup"><span data-stu-id="59656-233">Default value</span></span> | <span data-ttu-id="59656-234">説明</span><span class="sxs-lookup"><span data-stu-id="59656-234">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="59656-235">30 秒 (30,000 ミリ秒)</span><span class="sxs-lookup"><span data-stu-id="59656-235">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="59656-236">サーバーの利用状況のタイムアウト。</span><span class="sxs-lookup"><span data-stu-id="59656-236">Timeout for server activity.</span></span> <span data-ttu-id="59656-237">サーバーは、この間隔でメッセージを送信していない、クライアントと見なされ、サーバーが切断されているトリガー、`Closed`イベント (`onclose` JavaScript で)。</span><span class="sxs-lookup"><span data-stu-id="59656-237">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="59656-238">この値は、サーバーから送信される ping メッセージに十分な大きさである必要があります**と**タイムアウト間隔内に、クライアントによって受信されます。</span><span class="sxs-lookup"><span data-stu-id="59656-238">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="59656-239">推奨値は数値、サーバーに少なくとも 2 倍の`KeepAliveInterval`の ping が到着するまでの値。</span><span class="sxs-lookup"><span data-stu-id="59656-239">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="59656-240">15 秒</span><span class="sxs-lookup"><span data-stu-id="59656-240">15 seconds</span></span> | <span data-ttu-id="59656-241">サーバーの初期ハンドシェイクのタイムアウト。</span><span class="sxs-lookup"><span data-stu-id="59656-241">Timeout for initial server handshake.</span></span> <span data-ttu-id="59656-242">サーバーは、この間隔でハンドシェイクの応答を送信しない場合、は、クライアントがキャンセル ハンドシェイクとトリガー、`Closed`イベント (`onclose` JavaScript で)。</span><span class="sxs-lookup"><span data-stu-id="59656-242">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="59656-243">これは、重大なネットワーク待機時間が原因のハンドシェイクのタイムアウト エラーが発生している場合にのみ変更する高度な設定です。</span><span class="sxs-lookup"><span data-stu-id="59656-243">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="59656-244">ハンドシェイク プロセスの詳細については、次を参照してください。、 [SignalR ハブ プロトコル仕様](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)します。</span><span class="sxs-lookup"><span data-stu-id="59656-244">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="59656-245">として、.NET クライアントでタイムアウト値が指定された`TimeSpan`値。</span><span class="sxs-lookup"><span data-stu-id="59656-245">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="59656-246">JavaScript</span><span class="sxs-lookup"><span data-stu-id="59656-246">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="59656-247">オプション</span><span class="sxs-lookup"><span data-stu-id="59656-247">Option</span></span> | <span data-ttu-id="59656-248">既定値</span><span class="sxs-lookup"><span data-stu-id="59656-248">Default value</span></span> | <span data-ttu-id="59656-249">説明</span><span class="sxs-lookup"><span data-stu-id="59656-249">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="59656-250">30 秒 (30,000 ミリ秒)</span><span class="sxs-lookup"><span data-stu-id="59656-250">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="59656-251">サーバーの利用状況のタイムアウト。</span><span class="sxs-lookup"><span data-stu-id="59656-251">Timeout for server activity.</span></span> <span data-ttu-id="59656-252">サーバーは、この間隔でメッセージを送信していない、クライアントと見なされ、サーバーが切断されているトリガー、`onclose`イベント。</span><span class="sxs-lookup"><span data-stu-id="59656-252">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="59656-253">この値は、サーバーから送信される ping メッセージに十分な大きさである必要があります**と**タイムアウト間隔内に、クライアントによって受信されます。</span><span class="sxs-lookup"><span data-stu-id="59656-253">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="59656-254">推奨値は数値、サーバーに少なくとも 2 倍の`KeepAliveInterval`の ping が到着するまでの値。</span><span class="sxs-lookup"><span data-stu-id="59656-254">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="59656-255">Java</span><span class="sxs-lookup"><span data-stu-id="59656-255">Java</span></span>](#tab/java)

| <span data-ttu-id="59656-256">オプション</span><span class="sxs-lookup"><span data-stu-id="59656-256">Option</span></span> | <span data-ttu-id="59656-257">既定値</span><span class="sxs-lookup"><span data-stu-id="59656-257">Default value</span></span> | <span data-ttu-id="59656-258">説明</span><span class="sxs-lookup"><span data-stu-id="59656-258">Description</span></span> |
| ----------- | ------------- | ----------- |
|<span data-ttu-id="59656-259">`getServerTimeout` `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="59656-259">`getServerTimeout` `setServerTimeout`</span></span> | <span data-ttu-id="59656-260">30 秒 (30,000 ミリ秒)</span><span class="sxs-lookup"><span data-stu-id="59656-260">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="59656-261">サーバーの利用状況のタイムアウト。</span><span class="sxs-lookup"><span data-stu-id="59656-261">Timeout for server activity.</span></span> <span data-ttu-id="59656-262">サーバーは、この間隔でメッセージを送信していない、クライアントと見なされ、サーバーが切断されているトリガー、`onClose`イベント。</span><span class="sxs-lookup"><span data-stu-id="59656-262">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="59656-263">この値は、サーバーから送信される ping メッセージに十分な大きさである必要があります**と**タイムアウト間隔内に、クライアントによって受信されます。</span><span class="sxs-lookup"><span data-stu-id="59656-263">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="59656-264">推奨値は数値、サーバーに少なくとも 2 倍の`KeepAliveInterval`の ping が到着するまでの値。</span><span class="sxs-lookup"><span data-stu-id="59656-264">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="59656-265">15 秒</span><span class="sxs-lookup"><span data-stu-id="59656-265">15 seconds</span></span> | <span data-ttu-id="59656-266">サーバーの初期ハンドシェイクのタイムアウト。</span><span class="sxs-lookup"><span data-stu-id="59656-266">Timeout for initial server handshake.</span></span> <span data-ttu-id="59656-267">サーバーは、この間隔でハンドシェイクの応答を送信しない場合、は、クライアントがキャンセル ハンドシェイクとトリガー、`onClose`イベント。</span><span class="sxs-lookup"><span data-stu-id="59656-267">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="59656-268">これは、重大なネットワーク待機時間が原因のハンドシェイクのタイムアウト エラーが発生している場合にのみ変更する高度な設定です。</span><span class="sxs-lookup"><span data-stu-id="59656-268">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="59656-269">ハンドシェイク プロセスの詳細については、次を参照してください。、 [SignalR ハブ プロトコル仕様](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)します。</span><span class="sxs-lookup"><span data-stu-id="59656-269">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="59656-270">追加のオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="59656-270">Configure additional options</span></span>

<span data-ttu-id="59656-271">追加のオプションを構成することができます、 `WithUrl` (`withUrl` JavaScript で) メソッドを`HubConnectionBuilder`やさまざまな構成 Api を上、 `HttpHubConnectionBuilder` Java クライアント。</span><span class="sxs-lookup"><span data-stu-id="59656-271">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="59656-272">.NET</span><span class="sxs-lookup"><span data-stu-id="59656-272">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="59656-273">.NET オプション</span><span class="sxs-lookup"><span data-stu-id="59656-273">.NET Option</span></span> |  <span data-ttu-id="59656-274">既定値</span><span class="sxs-lookup"><span data-stu-id="59656-274">Default value</span></span> | <span data-ttu-id="59656-275">説明</span><span class="sxs-lookup"><span data-stu-id="59656-275">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="59656-276">HTTP 要求でベアラー認証トークンとして提供されている文字列を返す関数。</span><span class="sxs-lookup"><span data-stu-id="59656-276">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="59656-277">これを設定`true`ネゴシエーションの手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="59656-277">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="59656-278">**Websocket トランスポートがトランスポートには、のみの有効な場合にのみサポート**します。</span><span class="sxs-lookup"><span data-stu-id="59656-278">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="59656-279">Azure SignalR サービスを使用する場合は、この設定を有効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="59656-279">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="59656-280">Empty</span><span class="sxs-lookup"><span data-stu-id="59656-280">Empty</span></span> | <span data-ttu-id="59656-281">要求の認証に送信する TLS 証明書のコレクション。</span><span class="sxs-lookup"><span data-stu-id="59656-281">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="59656-282">Empty</span><span class="sxs-lookup"><span data-stu-id="59656-282">Empty</span></span> | <span data-ttu-id="59656-283">すべての HTTP 要求を送信する HTTP クッキーのコレクション。</span><span class="sxs-lookup"><span data-stu-id="59656-283">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="59656-284">Empty</span><span class="sxs-lookup"><span data-stu-id="59656-284">Empty</span></span> | <span data-ttu-id="59656-285">すべての HTTP 要求を送信する資格情報です。</span><span class="sxs-lookup"><span data-stu-id="59656-285">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="59656-286">5 秒</span><span class="sxs-lookup"><span data-stu-id="59656-286">5 seconds</span></span> | <span data-ttu-id="59656-287">Websocket だけです。</span><span class="sxs-lookup"><span data-stu-id="59656-287">WebSockets only.</span></span> <span data-ttu-id="59656-288">最長時間、クライアントは、サーバーが閉じる要求を確認するための終了後に待機します。</span><span class="sxs-lookup"><span data-stu-id="59656-288">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="59656-289">サーバーは、この時間内終了を確認は、クライアントが切断されます。</span><span class="sxs-lookup"><span data-stu-id="59656-289">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="59656-290">Empty</span><span class="sxs-lookup"><span data-stu-id="59656-290">Empty</span></span> | <span data-ttu-id="59656-291">すべての HTTP 要求を送信する追加の HTTP ヘッダーのマップです。</span><span class="sxs-lookup"><span data-stu-id="59656-291">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="59656-292">構成または置換するために使用するデリゲート、 `HttpMessageHandler` HTTP 要求を送信するために使用します。</span><span class="sxs-lookup"><span data-stu-id="59656-292">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="59656-293">WebSocket 接続に使用されません。</span><span class="sxs-lookup"><span data-stu-id="59656-293">Not used for WebSocket connections.</span></span> <span data-ttu-id="59656-294">このデリゲートは、null 以外の値を返す必要があり、既定値をパラメーターとして受け取ります。</span><span class="sxs-lookup"><span data-stu-id="59656-294">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="59656-295">その既定値の設定を変更し、それを返すかが返さ新しい`HttpMessageHandler`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="59656-295">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="59656-296">**ときは、ハンドラーを置き換える、指定したハンドラーから保持する設定をコピーすることを確認して、それ以外の場合、(Cookie やヘッダー) の構成オプションが新しいハンドラーを適用されません。**</span><span class="sxs-lookup"><span data-stu-id="59656-296">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="59656-297">HTTP 要求を送信するときに使用する HTTP プロキシ。</span><span class="sxs-lookup"><span data-stu-id="59656-297">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="59656-298">HTTP と Websocket の要求の既定の資格情報を送信するこのブール値を設定します。</span><span class="sxs-lookup"><span data-stu-id="59656-298">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="59656-299">これにより、Windows 認証を使用できます。</span><span class="sxs-lookup"><span data-stu-id="59656-299">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="59656-300">追加の WebSocket オプションを構成するために使用するデリゲート。</span><span class="sxs-lookup"><span data-stu-id="59656-300">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="59656-301">インスタンスを受け取る[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)オプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="59656-301">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="59656-302">JavaScript</span><span class="sxs-lookup"><span data-stu-id="59656-302">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="59656-303">JavaScript のオプション</span><span class="sxs-lookup"><span data-stu-id="59656-303">JavaScript Option</span></span> | <span data-ttu-id="59656-304">既定値</span><span class="sxs-lookup"><span data-stu-id="59656-304">Default Value</span></span> | <span data-ttu-id="59656-305">説明</span><span class="sxs-lookup"><span data-stu-id="59656-305">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="59656-306">HTTP 要求でベアラー認証トークンとして提供されている文字列を返す関数。</span><span class="sxs-lookup"><span data-stu-id="59656-306">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="59656-307">これを設定`true`ネゴシエーションの手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="59656-307">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="59656-308">**Websocket トランスポートがトランスポートには、のみの有効な場合にのみサポート**します。</span><span class="sxs-lookup"><span data-stu-id="59656-308">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="59656-309">Azure SignalR サービスを使用する場合は、この設定を有効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="59656-309">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="59656-310">Java</span><span class="sxs-lookup"><span data-stu-id="59656-310">Java</span></span>](#tab/java)

| <span data-ttu-id="59656-311">Java オプション</span><span class="sxs-lookup"><span data-stu-id="59656-311">Java Option</span></span> | <span data-ttu-id="59656-312">既定値</span><span class="sxs-lookup"><span data-stu-id="59656-312">Default Value</span></span> | <span data-ttu-id="59656-313">説明</span><span class="sxs-lookup"><span data-stu-id="59656-313">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="59656-314">HTTP 要求でベアラー認証トークンとして提供されている文字列を返す関数。</span><span class="sxs-lookup"><span data-stu-id="59656-314">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="59656-315">これを設定`true`ネゴシエーションの手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="59656-315">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="59656-316">**Websocket トランスポートがトランスポートには、のみの有効な場合にのみサポート**します。</span><span class="sxs-lookup"><span data-stu-id="59656-316">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="59656-317">Azure SignalR サービスを使用する場合は、この設定を有効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="59656-317">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="59656-318">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="59656-318">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="59656-319">Empty</span><span class="sxs-lookup"><span data-stu-id="59656-319">Empty</span></span> | <span data-ttu-id="59656-320">すべての HTTP 要求を送信する追加の HTTP ヘッダーのマップです。</span><span class="sxs-lookup"><span data-stu-id="59656-320">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="59656-321">指定されたオプションのデリゲートで、.NET クライアントでこれらのオプションを変更できます`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="59656-321">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="59656-322">指定された JavaScript オブジェクトで、JavaScript クライアントでこれらのオプションを指定できる`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="59656-322">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="59656-323">Java クライアントでこれらのオプションで構成できますメソッドを使って、`HttpHubConnectionBuilder`から返される、 `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="59656-323">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="59656-324">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="59656-324">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
