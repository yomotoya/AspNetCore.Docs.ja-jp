---
title: ASP.NET Core SignalR の構成
author: rachelappel
description: ASP.NET Core SignalR アプリケーションを構成する方法を説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961984"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="28645-103">ASP.NET Core SignalR の構成</span><span class="sxs-lookup"><span data-stu-id="28645-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="28645-104">JSON/MessagePack シリアル化オプション</span><span class="sxs-lookup"><span data-stu-id="28645-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="28645-105">ASP.NET Core SignalR は、メッセージをエンコードするための 2 つのプロトコルをサポートしています: [JSON](https://www.json.org/)と[MessagePack](https://msgpack.org/index.html)です。</span><span class="sxs-lookup"><span data-stu-id="28645-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="28645-106">各プロトコルには、シリアル化の構成オプションがあります。</span><span class="sxs-lookup"><span data-stu-id="28645-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="28645-107">JSON のシリアル化を使用して、サーバーで構成できます。、 [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)拡張メソッドは、後に追加する[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)で、`Startup.ConfigureServices`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="28645-107">JSON serialization can be configured on the server using the [`AddJsonProtocol`](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="28645-108">`AddJsonProtocol`メソッドは、受信するデリゲートを受け取る、`options`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="28645-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="28645-109">[ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)そのオブジェクトのプロパティには、JSON.NET`JsonSerializerSettings`と戻り値の引数のシリアル化の構成に使用できるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="28645-109">The [`PayloadSerializerSettings`](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="28645-110">参照してください、 [JSON.NET ドキュメント](https://www.newtonsoft.com/json/help/html/Introduction.htm)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="28645-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="28645-111">たとえば、名の代わりに、既定値「キャメル ケース」、"PascalCase"プロパティの名前を使用するシリアライザーを構成する次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="28645-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="28645-112">.NET クライアントで、同じ`AddJsonHubProtocol`の拡張メソッドが存在する[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)です。</span><span class="sxs-lookup"><span data-stu-id="28645-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="28645-113">`Microsoft.Extensions.DependencyInjection`拡張メソッドを解決するのには名前空間をインポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="28645-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> <span data-ttu-id="28645-114">この時点での JavaScript クライアントでの JSON のシリアル化を構成することはありません。</span><span class="sxs-lookup"><span data-stu-id="28645-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="28645-115">MessagePack シリアル化オプション</span><span class="sxs-lookup"><span data-stu-id="28645-115">MessagePack serialization options</span></span>

<span data-ttu-id="28645-116">デリゲートを提供することによって MessagePack シリアル化を構成することができます、 [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼び出します。</span><span class="sxs-lookup"><span data-stu-id="28645-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="28645-117">参照してください[SignalR で MessagePack](xref:signalr/messagepackhubprotocol)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="28645-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="28645-118">この時点での JavaScript クライアントで MessagePack シリアル化を構成することはありません。</span><span class="sxs-lookup"><span data-stu-id="28645-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="28645-119">サーバー オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="28645-119">Configure server options</span></span>

<span data-ttu-id="28645-120">次の表では、SignalR ハブの構成オプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="28645-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="28645-121">オプション</span><span class="sxs-lookup"><span data-stu-id="28645-121">Option</span></span> | <span data-ttu-id="28645-122">説明</span><span class="sxs-lookup"><span data-stu-id="28645-122">Description</span></span> |
| ------ | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="28645-123">クライアントがこの時間間隔内で最初のハンドシェイク メッセージを送信しない場合、接続は閉じられます。</span><span class="sxs-lookup"><span data-stu-id="28645-123">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="28645-124">サーバーは、この時間内でメッセージを送信していない場合、接続を開いたままに ping メッセージは自動的に送信します。</span><span class="sxs-lookup"><span data-stu-id="28645-124">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="28645-125">このハブでサポートされるプロトコル。</span><span class="sxs-lookup"><span data-stu-id="28645-125">Protocols supported by this hub.</span></span> <span data-ttu-id="28645-126">既定では、サーバーに登録されているすべてのプロトコルが許可されるが、プロトコルは、個々 のハブの特定のプロトコルを無効にするには、この一覧から削除できます。</span><span class="sxs-lookup"><span data-stu-id="28645-126">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | <span data-ttu-id="28645-127">場合`true`、詳細なハブ メソッドで例外がスローされたときに、例外メッセージがクライアントに返されます。</span><span class="sxs-lookup"><span data-stu-id="28645-127">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="28645-128">既定値は`false`ように、これらの例外メッセージは機密性の高い情報を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="28645-128">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="28645-129">オプション デリゲートを提供することですべてのハブのオプションを構成でき、`AddSignalR`で呼び出す`Startup.ConfigureServices`です。</span><span class="sxs-lookup"><span data-stu-id="28645-129">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

<span data-ttu-id="28645-130">1 つのハブのオプションで提供されるグローバル オプションを上書き`AddSignalR`を使用して構成するには[ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="28645-130">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [`AddHubOptions<T>`](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="28645-131">使用して`HttpConnectionDispatcherOptions`トランスポートとメモリ バッファーの管理に関連する高度な設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="28645-131">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="28645-132">これらのオプションを構成するデリゲートを渡すことによって[ `MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)です。</span><span class="sxs-lookup"><span data-stu-id="28645-132">These options are configured by passing a delegate to [`MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="28645-133">オプション</span><span class="sxs-lookup"><span data-stu-id="28645-133">Option</span></span> | <span data-ttu-id="28645-134">説明</span><span class="sxs-lookup"><span data-stu-id="28645-134">Description</span></span> |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="28645-135">クライアントから受信したバイトの最大数をサーバーのバッファー。</span><span class="sxs-lookup"><span data-stu-id="28645-135">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="28645-136">この値を増やすことによりより大きなメッセージを受信するサーバーが、メモリ消費量に悪影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="28645-136">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="28645-137">既定値は、32 KB です。</span><span class="sxs-lookup"><span data-stu-id="28645-137">The default value is 32KB.</span></span> |
| `AuthorizationData` | <span data-ttu-id="28645-138">一連の[ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)オブジェクトが、クライアントがハブへの接続を承認されているかどうかを判断するために使用します。</span><span class="sxs-lookup"><span data-stu-id="28645-138">A list of [`IAuthorizeData`](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> <span data-ttu-id="28645-139">既定では、この値が格納されますから、`Authorize`ハブ クラスに適用される属性です。</span><span class="sxs-lookup"><span data-stu-id="28645-139">By default, this is populated with values from the `Authorize` attributes applied to the Hub class.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="28645-140">アプリによって送信されたバイト数の最大数をサーバーのバッファー。</span><span class="sxs-lookup"><span data-stu-id="28645-140">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="28645-141">この値を増やすことによりより大きなメッセージを送信するサーバーが、メモリ消費量に悪影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="28645-141">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="28645-142">既定値は、32 KB です。</span><span class="sxs-lookup"><span data-stu-id="28645-142">The default value is 32KB.</span></span> |
| `Transports` | <span data-ttu-id="28645-143">ビットマスク`HttpTransportType`値をトランスポートを制限するクライアントを使用して接続します。</span><span class="sxs-lookup"><span data-stu-id="28645-143">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> <span data-ttu-id="28645-144">既定では、すべてのトランスポートが有効にします。</span><span class="sxs-lookup"><span data-stu-id="28645-144">All transports are enabled by default.</span></span> |
| `LongPolling` | <span data-ttu-id="28645-145">長いポーリング トランスポートに固有の追加オプション。</span><span class="sxs-lookup"><span data-stu-id="28645-145">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="28645-146">Websocket トランスポートに固有の追加オプション。</span><span class="sxs-lookup"><span data-stu-id="28645-146">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="28645-147">長いポーリングのトランスポートを使用して構成できる追加のオプションがあります、`LongPolling`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="28645-147">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="28645-148">オプション</span><span class="sxs-lookup"><span data-stu-id="28645-148">Option</span></span> | <span data-ttu-id="28645-149">説明</span><span class="sxs-lookup"><span data-stu-id="28645-149">Description</span></span> |
| ------ | ----------- |
| `PollTimeout` | <span data-ttu-id="28645-150">時間の最大量、サーバーは 1 つのポーリング要求を終了する前に、クライアントに送信するメッセージを待機します。</span><span class="sxs-lookup"><span data-stu-id="28645-150">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="28645-151">新しいポーリング要求をより頻繁に発行するクライアントは、この値を小さきます。</span><span class="sxs-lookup"><span data-stu-id="28645-151">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> <span data-ttu-id="28645-152">既定値は、90 秒です。</span><span class="sxs-lookup"><span data-stu-id="28645-152">The default value is 90 seconds.</span></span> |

<span data-ttu-id="28645-153">WebSocket トランスポートを使用して構成できる追加のオプションがあります、`WebSockets`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="28645-153">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="28645-154">オプション</span><span class="sxs-lookup"><span data-stu-id="28645-154">Option</span></span> | <span data-ttu-id="28645-155">説明</span><span class="sxs-lookup"><span data-stu-id="28645-155">Description</span></span> |
| ------ | ----------- |
| `CloseTimeout` | <span data-ttu-id="28645-156">サーバーが閉じ、クライアントがこの時間間隔内に閉じるが失敗した場合、接続は終了します。</span><span class="sxs-lookup"><span data-stu-id="28645-156">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | <span data-ttu-id="28645-157">設定するために使用されるデリゲート、`Sec-WebSocket-Protocol`ヘッダーをカスタム値。</span><span class="sxs-lookup"><span data-stu-id="28645-157">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="28645-158">デリゲートは、入力としてクライアントによって要求された値を受け取るし、目的の値を返します。</span><span class="sxs-lookup"><span data-stu-id="28645-158">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="28645-159">クライアントのオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="28645-159">Configure client options</span></span>

<span data-ttu-id="28645-160">クライアントのオプションを構成することができます、 `HubConnectionBuilder` (.NET と JavaScript の両方のクライアントで使用できる)、型ではなく、`HubConnection`自体です。</span><span class="sxs-lookup"><span data-stu-id="28645-160">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="28645-161">ログを構成します。</span><span class="sxs-lookup"><span data-stu-id="28645-161">Configure logging</span></span>

<span data-ttu-id="28645-162">使用して、.NET クライアントでログ記録が構成されている、`ConfigureLogging`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="28645-162">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="28645-163">サーバー上にある、同じ方法でログ記録プロバイダーおよびフィルターを登録できます。</span><span class="sxs-lookup"><span data-stu-id="28645-163">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="28645-164">参照してください、 [ASP.NET Core でのログ記録](xref:fundamentals/logging/index#how-to-add-providers)詳細についてはドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="28645-164">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="28645-165">ログ プロバイダーを登録するために必要なパッケージをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="28645-165">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="28645-166">参照してください、[組み込みのログ記録プロバイダー](xref:fundamentals/logging/index#built-in-logging-providers)一覧についてはドキュメントの「します。</span><span class="sxs-lookup"><span data-stu-id="28645-166">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="28645-167">たとえば、コンソールのログ記録を有効にするをインストール、 `Microsoft.Extensions.Logging.Console` NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="28645-167">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="28645-168">呼び出す、`AddConsole`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="28645-168">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="28645-169">JavaScript クライアントで、類似した`configureLogging`メソッドが存在します。</span><span class="sxs-lookup"><span data-stu-id="28645-169">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="28645-170">提供、`LogLevel`生成するためにログ メッセージの最小レベルを示す値。</span><span class="sxs-lookup"><span data-stu-id="28645-170">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="28645-171">ブラウザーのコンソール ウィンドウには、ログが書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="28645-171">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="28645-172">完全ログ記録を無効にするには指定`signalR.LogLevel.None`で、`configureLogging`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="28645-172">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="28645-173">ログ レベルは、JavaScript クライアントには次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="28645-173">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="28645-174">ログ レベルをこれらの値のいずれかに設定でメッセージのログ記録を有効になります**以上**レベル。</span><span class="sxs-lookup"><span data-stu-id="28645-174">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="28645-175">レベル</span><span class="sxs-lookup"><span data-stu-id="28645-175">Level</span></span> | <span data-ttu-id="28645-176">説明</span><span class="sxs-lookup"><span data-stu-id="28645-176">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="28645-177">メッセージは記録されません。</span><span class="sxs-lookup"><span data-stu-id="28645-177">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="28645-178">アプリ全体の失敗を示すメッセージ。</span><span class="sxs-lookup"><span data-stu-id="28645-178">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="28645-179">現在の操作の失敗を示すメッセージ。</span><span class="sxs-lookup"><span data-stu-id="28645-179">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="28645-180">致命的でない問題を示すメッセージ。</span><span class="sxs-lookup"><span data-stu-id="28645-180">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="28645-181">情報メッセージです。</span><span class="sxs-lookup"><span data-stu-id="28645-181">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="28645-182">診断メッセージのデバッグに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="28645-182">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="28645-183">非常に詳細な診断メッセージが特定の問題を診断するために設計されています。</span><span class="sxs-lookup"><span data-stu-id="28645-183">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="28645-184">許可されているトランスポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="28645-184">Configure allowed transports</span></span>

<span data-ttu-id="28645-185">SignalR によって使用されるトランスポートを構成することができます、`WithUrl`呼び出し (`withUrl` JavaScript で)。</span><span class="sxs-lookup"><span data-stu-id="28645-185">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="28645-186">値のビット演算 OR`HttpTransportType`だけ、指定されたトランスポートを使用するクライアントを制限するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="28645-186">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="28645-187">既定では、すべてのトランスポートが有効にします。</span><span class="sxs-lookup"><span data-stu-id="28645-187">All transports are enabled by default.</span></span>

<span data-ttu-id="28645-188">たとえば、Server-Sent イベント転送を無効にするが、接続を許可する Websocket と長いポーリングします。</span><span class="sxs-lookup"><span data-stu-id="28645-188">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="28645-189">設定して、JavaScript クライアントで、トランスポートが構成されている、`transport`オプション オブジェクトに提供されるフィールド`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="28645-189">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="28645-190">ベアラー認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="28645-190">Configure bearer authentication</span></span>

<span data-ttu-id="28645-191">SignalR 要求と共に認証データを提供するには、使用、`AccessTokenProvider`オプション (`accessTokenFactory` JavaScript で) を対象とするアクセス トークンを返す関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="28645-191">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="28645-192">.NET クライアントで、このアクセス トークンとして渡される HTTP「ベアラー認証」トークン (を使用して、`Authorization`の型を持つヘッダー `Bearer`)。</span><span class="sxs-lookup"><span data-stu-id="28645-192">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="28645-193">JavaScript クライアントで、アクセス トークンはベアラー トークンとして使用**を除く**まれにブラウザーの Api が (具体的には、Server-Sent イベントと Websocket 要求) 内のヘッダーを適用する機能を制限します。</span><span class="sxs-lookup"><span data-stu-id="28645-193">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="28645-194">このような場合は、アクセス トークンがクエリ文字列値として提供される`access_token`です。</span><span class="sxs-lookup"><span data-stu-id="28645-194">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="28645-195">.NET クライアントで、`AccessTokenProvider`でオプションのデリゲートを使用してオプションを指定することができます`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="28645-195">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="28645-196">JavaScript クライアントで、アクセス トークンが構成されている設定によって、`accessTokenFactory`フィールド内のオプション オブジェクトに`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="28645-196">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="28645-197">タイムアウトとキープ アライブ オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="28645-197">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="28645-198">タイムアウトとキープ アライブ動作の構成の追加オプションが 利用可能な`HubConnection`オブジェクト自体。</span><span class="sxs-lookup"><span data-stu-id="28645-198">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="28645-199">.NET オプション</span><span class="sxs-lookup"><span data-stu-id="28645-199">.NET Option</span></span> | <span data-ttu-id="28645-200">JavaScript のオプション</span><span class="sxs-lookup"><span data-stu-id="28645-200">JavaScript Option</span></span> | <span data-ttu-id="28645-201">説明</span><span class="sxs-lookup"><span data-stu-id="28645-201">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="28645-202">サーバーの利用状況のタイムアウト期間。</span><span class="sxs-lookup"><span data-stu-id="28645-202">Timeout for server activity.</span></span> <span data-ttu-id="28645-203">サーバーは、この間隔ですべてのメッセージを送信していない、クライアントと見なされ、サーバーが切断されているトリガー、`Closed`イベント (`onclose` JavaScript で)。</span><span class="sxs-lookup"><span data-stu-id="28645-203">If the server hasn't sent any message in this interval, the client considers the server disconnected and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="28645-204">構成できません。</span><span class="sxs-lookup"><span data-stu-id="28645-204">Not configurable</span></span> | <span data-ttu-id="28645-205">サーバーの初期ハンドシェイクのタイムアウト期間。</span><span class="sxs-lookup"><span data-stu-id="28645-205">Timeout for initial server handshake.</span></span> <span data-ttu-id="28645-206">クライアントが、ハンドシェイクおよびトリガーを取り消すサーバーがこの間隔のハンドシェイクの応答を送信しない場合、`Closed`イベント (`onclose` JavaScript で)。</span><span class="sxs-lookup"><span data-stu-id="28645-206">If the server doesn't send a handshake response in this interval, the client cancels the handshake and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |

<span data-ttu-id="28645-207">.NET クライアントで、タイムアウトは値として指定`TimeSpan`値。</span><span class="sxs-lookup"><span data-stu-id="28645-207">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="28645-208">JavaScript クライアントでは、タイムアウト値を数値として指定します。</span><span class="sxs-lookup"><span data-stu-id="28645-208">In the JavaScript client, timeout values are specified as numbers.</span></span> <span data-ttu-id="28645-209">数値は、時刻の値 (ミリ秒単位) を表します。</span><span class="sxs-lookup"><span data-stu-id="28645-209">The numbers represent time values in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="28645-210">追加のオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="28645-210">Configure additional options</span></span>

<span data-ttu-id="28645-211">追加のオプションを構成することができます、 `WithUrl` (`withUrl` JavaScript で) メソッドを`HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="28645-211">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="28645-212">.NET オプション</span><span class="sxs-lookup"><span data-stu-id="28645-212">.NET Option</span></span> | <span data-ttu-id="28645-213">JavaScript のオプション</span><span class="sxs-lookup"><span data-stu-id="28645-213">JavaScript Option</span></span> | <span data-ttu-id="28645-214">説明</span><span class="sxs-lookup"><span data-stu-id="28645-214">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | <span data-ttu-id="28645-215">HTTP 要求にベアラー認証トークンとして提供される文字列を返す関数。</span><span class="sxs-lookup"><span data-stu-id="28645-215">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | <span data-ttu-id="28645-216">これを設定して`true`ネゴシエーションの手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="28645-216">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="28645-217">**Websocket トランスポートが、唯一の有効なトランスポートの場合のみサポート**です。</span><span class="sxs-lookup"><span data-stu-id="28645-217">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="28645-218">Azure SignalR サービスを使用する場合は、この設定を有効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="28645-218">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="28645-219">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="28645-219">Not configurable \*</span></span> | <span data-ttu-id="28645-220">要求の認証に送信する TLS 証明書のコレクション。</span><span class="sxs-lookup"><span data-stu-id="28645-220">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="28645-221">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="28645-221">Not configurable \*</span></span> | <span data-ttu-id="28645-222">すべての HTTP 要求を送信する HTTP クッキーのコレクション。</span><span class="sxs-lookup"><span data-stu-id="28645-222">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="28645-223">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="28645-223">Not configurable \*</span></span> | <span data-ttu-id="28645-224">すべての HTTP 要求を送信する資格情報。</span><span class="sxs-lookup"><span data-stu-id="28645-224">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="28645-225">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="28645-225">Not configurable \*</span></span> | <span data-ttu-id="28645-226">Websocket でのみ使用します。</span><span class="sxs-lookup"><span data-stu-id="28645-226">WebSockets only.</span></span> <span data-ttu-id="28645-227">最大時間数、クライアントは、サーバーが閉じる要求を確認するための終了後に待機します。</span><span class="sxs-lookup"><span data-stu-id="28645-227">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="28645-228">サーバーは、この時間内、閉じるを認識しない、クライアントが切断されます。</span><span class="sxs-lookup"><span data-stu-id="28645-228">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="28645-229">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="28645-229">Not configurable \*</span></span> | <span data-ttu-id="28645-230">すべての HTTP 要求を送信する追加の HTTP ヘッダーのディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="28645-230">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="28645-231">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="28645-231">Not configurable \*</span></span> | <span data-ttu-id="28645-232">構成または置換するために使用するデリゲート、 `HttpMessageHandler` HTTP 要求を送信するために使用します。</span><span class="sxs-lookup"><span data-stu-id="28645-232">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="28645-233">WebSocket 接続に使用されません。</span><span class="sxs-lookup"><span data-stu-id="28645-233">Not used for WebSocket connections.</span></span> <span data-ttu-id="28645-234">このデリゲートは null 以外の値を返す必要があり、既定値をパラメーターとして受け取ります。</span><span class="sxs-lookup"><span data-stu-id="28645-234">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="28645-235">その既定値の設定を変更し、取得、またはまったく新しい返す`HttpMessageHandler`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="28645-235">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="28645-236">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="28645-236">Not configurable \*</span></span> | <span data-ttu-id="28645-237">HTTP 要求を送信するときに使用する HTTP プロキシ。</span><span class="sxs-lookup"><span data-stu-id="28645-237">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="28645-238">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="28645-238">Not configurable \*</span></span> | <span data-ttu-id="28645-239">HTTP や Websocket の要求の既定の資格情報を送信するこのブール値を設定します。</span><span class="sxs-lookup"><span data-stu-id="28645-239">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="28645-240">これにより、Windows 認証を使用できます。</span><span class="sxs-lookup"><span data-stu-id="28645-240">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="28645-241">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="28645-241">Not configurable \*</span></span> | <span data-ttu-id="28645-242">追加の WebSocket オプションを構成するために使用するデリゲート。</span><span class="sxs-lookup"><span data-stu-id="28645-242">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="28645-243">インスタンスを受け取り[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)オプションの構成を使用できます。</span><span class="sxs-lookup"><span data-stu-id="28645-243">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="28645-244">アスタリスク (\*) でマークされた」オプションは、ブラウザーの Api の制限のための JavaScript クライアントで構成できます。</span><span class="sxs-lookup"><span data-stu-id="28645-244">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="28645-245">.NET クライアントで、これらのオプションでは変更に提供されるオプション デリゲート`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="28645-245">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="28645-246">指定された JavaScript オブジェクトで、JavaScript クライアントで、これらのオプションを指定できます`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="28645-246">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="28645-247">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="28645-247">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
