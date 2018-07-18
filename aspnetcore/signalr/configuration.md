---
title: ASP.NET Core SignalR の構成
author: tdykstra
description: ASP.NET Core SignalR アプリケーションを構成する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: fac0226c939f4cf446c876b1c0b359d6c5b9dfd3
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095403"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="86cb6-103">ASP.NET Core SignalR の構成</span><span class="sxs-lookup"><span data-stu-id="86cb6-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="86cb6-104">JSON/MessagePack シリアル化オプション</span><span class="sxs-lookup"><span data-stu-id="86cb6-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="86cb6-105">ASP.NET Core SignalR は、メッセージをエンコードするための 2 つのプロトコルをサポートしています: [JSON](https://www.json.org/)と[MessagePack](https://msgpack.org/index.html)します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="86cb6-106">各プロトコルには、シリアル化の構成オプションがあります。</span><span class="sxs-lookup"><span data-stu-id="86cb6-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="86cb6-107">JSON のシリアル化を使用して、サーバーで構成できます、 [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)拡張メソッドは後に、追加する[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)で、`Startup.ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="86cb6-107">JSON serialization can be configured on the server using the [`AddJsonProtocol`](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="86cb6-108">`AddJsonProtocol`メソッドが受信するデリゲートを受け取り、`options`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="86cb6-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="86cb6-109">[ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)そのオブジェクトのプロパティは、JSON.NET`JsonSerializerSettings`引数のシリアル化の構成し、戻り値を使用できるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="86cb6-109">The [`PayloadSerializerSettings`](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="86cb6-110">参照してください、 [JSON.NET のドキュメント](https://www.newtonsoft.com/json/help/html/Introduction.htm)の詳細。</span><span class="sxs-lookup"><span data-stu-id="86cb6-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="86cb6-111">たとえば、「キャメル ケース」の既定名ではなく"PascalCase"プロパティの名前を使用するシリアライザーを構成する次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="86cb6-112">.NET クライアントで同じ`AddJsonHubProtocol`に拡張メソッドが存在する[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="86cb6-113">`Microsoft.Extensions.DependencyInjection`拡張メソッドを解決するのには名前空間をインポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="86cb6-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="86cb6-114">この時点で、JavaScript クライアント内で JSON のシリアル化を構成することはできません。</span><span class="sxs-lookup"><span data-stu-id="86cb6-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="86cb6-115">MessagePack シリアル化オプション</span><span class="sxs-lookup"><span data-stu-id="86cb6-115">MessagePack serialization options</span></span>

<span data-ttu-id="86cb6-116">デリゲートを提供することで MessagePack シリアル化を構成することができます、 [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼び出します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="86cb6-117">参照してください[SignalR で MessagePack](xref:signalr/messagepackhubprotocol)の詳細。</span><span class="sxs-lookup"><span data-stu-id="86cb6-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="86cb6-118">この時点で、JavaScript クライアント内で MessagePack シリアル化を構成することはできません。</span><span class="sxs-lookup"><span data-stu-id="86cb6-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="86cb6-119">サーバー オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-119">Configure server options</span></span>

<span data-ttu-id="86cb6-120">次の表では、SignalR ハブを構成するためのオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="86cb6-121">オプション</span><span class="sxs-lookup"><span data-stu-id="86cb6-121">Option</span></span> | <span data-ttu-id="86cb6-122">説明</span><span class="sxs-lookup"><span data-stu-id="86cb6-122">Description</span></span> |
| ------ | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="86cb6-123">クライアントがこの時間間隔内で最初のハンドシェイク メッセージを送信しない場合、接続は閉じられます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-123">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="86cb6-124">サーバーがこの間隔内でメッセージを送信していない場合、接続を開いたままに ping メッセージが自動的に送信されます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-124">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="86cb6-125">このハブでサポートされるプロトコル。</span><span class="sxs-lookup"><span data-stu-id="86cb6-125">Protocols supported by this hub.</span></span> <span data-ttu-id="86cb6-126">既定では、サーバーに登録されているプロトコルをすべて許可されますが、プロトコルは、個々 のハブに対する特定のプロトコルを無効にするには、この一覧から削除できます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-126">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | <span data-ttu-id="86cb6-127">場合`true`詳細のハブ メソッドで例外がスローされたときに、例外メッセージがクライアントに返されます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-127">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="86cb6-128">既定値は`false`、これらの例外メッセージは、機密情報を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-128">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="86cb6-129">オプションのデリゲートを提供することですべてのハブのオプションを構成でき、`AddSignalR`呼び出す`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-129">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="86cb6-130">1 つのハブのオプションで提供されるグローバル オプションを上書き`AddSignalR`を使用して構成できると[ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="86cb6-130">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [`AddHubOptions<T>`](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="86cb6-131">使用`HttpConnectionDispatcherOptions`トランスポートおよびメモリ バッファーの管理に関連する高度な設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-131">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="86cb6-132">これらのオプションを構成するデリゲートを渡すことによって[ `MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-132">These options are configured by passing a delegate to [`MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="86cb6-133">オプション</span><span class="sxs-lookup"><span data-stu-id="86cb6-133">Option</span></span> | <span data-ttu-id="86cb6-134">説明</span><span class="sxs-lookup"><span data-stu-id="86cb6-134">Description</span></span> |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="86cb6-135">クライアントから受信したバイトの最大数をサーバーのバッファー。</span><span class="sxs-lookup"><span data-stu-id="86cb6-135">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="86cb6-136">この値を増やすことによりより大きなメッセージを受信するサーバーが、メモリ消費量に悪影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-136">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="86cb6-137">既定値は、32 KB です。</span><span class="sxs-lookup"><span data-stu-id="86cb6-137">The default value is 32KB.</span></span> |
| `AuthorizationData` | <span data-ttu-id="86cb6-138">一連の[ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)オブジェクトは、クライアントがハブへの接続を承認されているかどうかを判断するために使用します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-138">A list of [`IAuthorizeData`](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> <span data-ttu-id="86cb6-139">既定では、この値が格納されますから、`Authorize`ハブ クラスに適用される属性。</span><span class="sxs-lookup"><span data-stu-id="86cb6-139">By default, this is populated with values from the `Authorize` attributes applied to the Hub class.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="86cb6-140">アプリによって送信されたバイト数の最大数をサーバーのバッファー。</span><span class="sxs-lookup"><span data-stu-id="86cb6-140">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="86cb6-141">この値を増やすことによりより大きなメッセージを送信するサーバーが、メモリ消費量に悪影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-141">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="86cb6-142">既定値は、32 KB です。</span><span class="sxs-lookup"><span data-stu-id="86cb6-142">The default value is 32KB.</span></span> |
| `Transports` | <span data-ttu-id="86cb6-143">ビットマスク`HttpTransportType`トランスポートを制限する値をクライアントが接続に使用できます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-143">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> <span data-ttu-id="86cb6-144">既定では、すべてのトランスポートが有効にします。</span><span class="sxs-lookup"><span data-stu-id="86cb6-144">All transports are enabled by default.</span></span> |
| `LongPolling` | <span data-ttu-id="86cb6-145">長いポーリングのトランスポートに固有の追加オプション。</span><span class="sxs-lookup"><span data-stu-id="86cb6-145">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="86cb6-146">Websocket トランスポートに固有の追加オプション。</span><span class="sxs-lookup"><span data-stu-id="86cb6-146">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="86cb6-147">ポーリングの長いトランスポートを使用して構成できるその他のオプションを持つ、`LongPolling`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="86cb6-147">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="86cb6-148">オプション</span><span class="sxs-lookup"><span data-stu-id="86cb6-148">Option</span></span> | <span data-ttu-id="86cb6-149">説明</span><span class="sxs-lookup"><span data-stu-id="86cb6-149">Description</span></span> |
| ------ | ----------- |
| `PollTimeout` | <span data-ttu-id="86cb6-150">1 つのポーリング要求を終了する前に、クライアントに送信するメッセージのサーバーが待機する最長時間。</span><span class="sxs-lookup"><span data-stu-id="86cb6-150">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="86cb6-151">新しいポーリング要求をより頻繁に発行するクライアントは、この値を小さきます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-151">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> <span data-ttu-id="86cb6-152">既定値は、90 秒です。</span><span class="sxs-lookup"><span data-stu-id="86cb6-152">The default value is 90 seconds.</span></span> |

<span data-ttu-id="86cb6-153">WebSocket トランスポートが追加のオプションを使用して構成できる、`WebSockets`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="86cb6-153">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="86cb6-154">オプション</span><span class="sxs-lookup"><span data-stu-id="86cb6-154">Option</span></span> | <span data-ttu-id="86cb6-155">説明</span><span class="sxs-lookup"><span data-stu-id="86cb6-155">Description</span></span> |
| ------ | ----------- |
| `CloseTimeout` | <span data-ttu-id="86cb6-156">サーバーが閉じ、クライアントがこの時間間隔内に閉じるが失敗した場合、接続は終了します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-156">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | <span data-ttu-id="86cb6-157">設定するために使用するデリゲート、`Sec-WebSocket-Protocol`ヘッダーをカスタム値。</span><span class="sxs-lookup"><span data-stu-id="86cb6-157">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="86cb6-158">デリゲートは、入力としてクライアントによって要求された値を受け取るし、目的の値を返します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-158">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="86cb6-159">クライアント オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-159">Configure client options</span></span>

<span data-ttu-id="86cb6-160">クライアント オプションを構成できる、 `HubConnectionBuilder` (.NET と JavaScript の両方のクライアントで使用できる)、型ではなく、`HubConnection`自体。</span><span class="sxs-lookup"><span data-stu-id="86cb6-160">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="86cb6-161">ログを構成します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-161">Configure logging</span></span>

<span data-ttu-id="86cb6-162">.NET を使用してクライアントでログ記録が構成されている、`ConfigureLogging`メソッド。</span><span class="sxs-lookup"><span data-stu-id="86cb6-162">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="86cb6-163">サーバー上にある、同じ方法でログ記録プロバイダーおよびフィルターを登録できます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-163">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="86cb6-164">参照してください、 [ASP.NET Core でのログ記録](xref:fundamentals/logging/index#how-to-add-providers)詳細についてはドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="86cb6-164">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="86cb6-165">ログ プロバイダーを登録するために必要なパッケージをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="86cb6-165">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="86cb6-166">参照してください、[組み込みのログ プロバイダー](xref:fundamentals/logging/index#built-in-logging-providers)完全な一覧については、ドキュメントのセクション。</span><span class="sxs-lookup"><span data-stu-id="86cb6-166">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="86cb6-167">たとえば、コンソールのログ記録を有効にするインストール、 `Microsoft.Extensions.Logging.Console` NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="86cb6-167">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="86cb6-168">呼び出す、`AddConsole`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="86cb6-168">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="86cb6-169">同様に、JavaScript クライアント内で`configureLogging`メソッドが存在します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-169">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="86cb6-170">提供、`LogLevel`を生成するログ メッセージの最小レベルを示す値。</span><span class="sxs-lookup"><span data-stu-id="86cb6-170">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="86cb6-171">ブラウザーのコンソール ウィンドウには、ログが書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-171">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="86cb6-172">完全ログ記録を無効にするには指定`signalR.LogLevel.None`で、`configureLogging`メソッド。</span><span class="sxs-lookup"><span data-stu-id="86cb6-172">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="86cb6-173">JavaScript クライアントに利用可能なログ レベルは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-173">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="86cb6-174">ログ レベルをこれらの値のいずれかに設定でメッセージのログ記録を有効になります**以上**レベル。</span><span class="sxs-lookup"><span data-stu-id="86cb6-174">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="86cb6-175">レベル</span><span class="sxs-lookup"><span data-stu-id="86cb6-175">Level</span></span> | <span data-ttu-id="86cb6-176">説明</span><span class="sxs-lookup"><span data-stu-id="86cb6-176">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="86cb6-177">メッセージは記録されません。</span><span class="sxs-lookup"><span data-stu-id="86cb6-177">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="86cb6-178">アプリ全体で障害を示すメッセージ。</span><span class="sxs-lookup"><span data-stu-id="86cb6-178">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="86cb6-179">現在の操作の失敗を示すメッセージ。</span><span class="sxs-lookup"><span data-stu-id="86cb6-179">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="86cb6-180">致命的でない問題を示すメッセージ。</span><span class="sxs-lookup"><span data-stu-id="86cb6-180">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="86cb6-181">情報メッセージ。</span><span class="sxs-lookup"><span data-stu-id="86cb6-181">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="86cb6-182">診断メッセージのデバッグに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-182">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="86cb6-183">非常に詳細な診断メッセージが特定の問題を診断するために設計されています。</span><span class="sxs-lookup"><span data-stu-id="86cb6-183">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="86cb6-184">許可されているトランスポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-184">Configure allowed transports</span></span>

<span data-ttu-id="86cb6-185">SignalR によって使用されるトランスポートで構成できる、`WithUrl`を呼び出す (`withUrl` JavaScript で)。</span><span class="sxs-lookup"><span data-stu-id="86cb6-185">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="86cb6-186">値のビットごとの OR`HttpTransportType`のみ指定されたトランスポートを使用するクライアントを制限するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-186">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="86cb6-187">既定では、すべてのトランスポートが有効にします。</span><span class="sxs-lookup"><span data-stu-id="86cb6-187">All transports are enabled by default.</span></span>

<span data-ttu-id="86cb6-188">たとえば、Server-Sent イベント転送を無効にすることを許可する WebSockets および長いポーリングの接続。</span><span class="sxs-lookup"><span data-stu-id="86cb6-188">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="86cb6-189">設定して、JavaScript クライアントでのトランスポートが構成されている、`transport`フィールドに指定されたオプションのオブジェクトに`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="86cb6-189">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="86cb6-190">ベアラー認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-190">Configure bearer authentication</span></span>

<span data-ttu-id="86cb6-191">SignalR 要求と共に認証データを提供するには、使用、`AccessTokenProvider`オプション (`accessTokenFactory` JavaScript で) 必要なアクセス トークンを返す関数を指定します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-191">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="86cb6-192">.NET クライアントでこのアクセス トークンとして渡される HTTP「ベアラー認証」トークン (を使用して、`Authorization`ヘッダーの型を持つ`Bearer`)。</span><span class="sxs-lookup"><span data-stu-id="86cb6-192">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="86cb6-193">アクセス トークンをベアラー トークンとして使用、JavaScript クライアントで**を除く**ブラウザー Api が (具体的には、Server-Sent イベントと Websocket の要求) のヘッダーを適用する機能を制限するような場合。</span><span class="sxs-lookup"><span data-stu-id="86cb6-193">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="86cb6-194">このような場合は、アクセス トークンはクエリ文字列値として指定`access_token`します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-194">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="86cb6-195">.NET クライアントで、`AccessTokenProvider`でオプションのデリゲートを使用してオプションを指定することができます`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="86cb6-195">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="86cb6-196">設定して、JavaScript クライアントでアクセス トークンが構成されている、`accessTokenFactory`フィールド内のオプション オブジェクトに`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="86cb6-196">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="86cb6-197">タイムアウトとキープ アライブ オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-197">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="86cb6-198">タイムアウトとキープ アライブ動作を構成するための追加のオプションがあります、`HubConnection`オブジェクト自体。</span><span class="sxs-lookup"><span data-stu-id="86cb6-198">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="86cb6-199">.NET オプション</span><span class="sxs-lookup"><span data-stu-id="86cb6-199">.NET Option</span></span> | <span data-ttu-id="86cb6-200">JavaScript のオプション</span><span class="sxs-lookup"><span data-stu-id="86cb6-200">JavaScript Option</span></span> | <span data-ttu-id="86cb6-201">説明</span><span class="sxs-lookup"><span data-stu-id="86cb6-201">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="86cb6-202">サーバーの利用状況のタイムアウト。</span><span class="sxs-lookup"><span data-stu-id="86cb6-202">Timeout for server activity.</span></span> <span data-ttu-id="86cb6-203">サーバーは、この間隔ですべてのメッセージを送信していない、クライアントと見なされ、サーバーが切断されているトリガー、`Closed`イベント (`onclose` JavaScript で)。</span><span class="sxs-lookup"><span data-stu-id="86cb6-203">If the server hasn't sent any message in this interval, the client considers the server disconnected and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="86cb6-204">構成できません。</span><span class="sxs-lookup"><span data-stu-id="86cb6-204">Not configurable</span></span> | <span data-ttu-id="86cb6-205">サーバーの初期ハンドシェイクのタイムアウト。</span><span class="sxs-lookup"><span data-stu-id="86cb6-205">Timeout for initial server handshake.</span></span> <span data-ttu-id="86cb6-206">サーバーは、この間隔でハンドシェイクの応答を送信しない場合、は、クライアントがキャンセル、ハンドシェイクとトリガー、`Closed`イベント (`onclose` JavaScript で)。</span><span class="sxs-lookup"><span data-stu-id="86cb6-206">If the server doesn't send a handshake response in this interval, the client cancels the handshake and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |

<span data-ttu-id="86cb6-207">として、.NET クライアントでタイムアウト値が指定された`TimeSpan`値。</span><span class="sxs-lookup"><span data-stu-id="86cb6-207">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="86cb6-208">JavaScript クライアントでタイムアウト値は数値として指定します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-208">In the JavaScript client, timeout values are specified as numbers.</span></span> <span data-ttu-id="86cb6-209">数値は、時刻の値 (ミリ秒単位) を表します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-209">The numbers represent time values in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="86cb6-210">追加のオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-210">Configure additional options</span></span>

<span data-ttu-id="86cb6-211">追加のオプションを構成することができます、 `WithUrl` (`withUrl` JavaScript で) メソッドを`HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="86cb6-211">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="86cb6-212">.NET オプション</span><span class="sxs-lookup"><span data-stu-id="86cb6-212">.NET Option</span></span> | <span data-ttu-id="86cb6-213">JavaScript のオプション</span><span class="sxs-lookup"><span data-stu-id="86cb6-213">JavaScript Option</span></span> | <span data-ttu-id="86cb6-214">説明</span><span class="sxs-lookup"><span data-stu-id="86cb6-214">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | <span data-ttu-id="86cb6-215">HTTP 要求でベアラー認証トークンとして提供されている文字列を返す関数。</span><span class="sxs-lookup"><span data-stu-id="86cb6-215">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | <span data-ttu-id="86cb6-216">これを設定`true`ネゴシエーションの手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="86cb6-216">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="86cb6-217">**Websocket トランスポートがトランスポートには、のみの有効な場合にのみサポート**します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-217">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="86cb6-218">Azure SignalR サービスを使用する場合は、この設定を有効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="86cb6-218">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="86cb6-219">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="86cb6-219">Not configurable \*</span></span> | <span data-ttu-id="86cb6-220">要求の認証に送信する TLS 証明書のコレクション。</span><span class="sxs-lookup"><span data-stu-id="86cb6-220">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="86cb6-221">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="86cb6-221">Not configurable \*</span></span> | <span data-ttu-id="86cb6-222">すべての HTTP 要求を送信する HTTP クッキーのコレクション。</span><span class="sxs-lookup"><span data-stu-id="86cb6-222">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="86cb6-223">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="86cb6-223">Not configurable \*</span></span> | <span data-ttu-id="86cb6-224">すべての HTTP 要求を送信する資格情報です。</span><span class="sxs-lookup"><span data-stu-id="86cb6-224">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="86cb6-225">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="86cb6-225">Not configurable \*</span></span> | <span data-ttu-id="86cb6-226">Websocket だけです。</span><span class="sxs-lookup"><span data-stu-id="86cb6-226">WebSockets only.</span></span> <span data-ttu-id="86cb6-227">最長時間、クライアントは、サーバーが閉じる要求を確認するための終了後に待機します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-227">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="86cb6-228">サーバーは、この時間内終了を確認は、クライアントが切断されます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-228">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="86cb6-229">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="86cb6-229">Not configurable \*</span></span> | <span data-ttu-id="86cb6-230">すべての HTTP 要求を送信する追加の HTTP ヘッダーのディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="86cb6-230">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="86cb6-231">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="86cb6-231">Not configurable \*</span></span> | <span data-ttu-id="86cb6-232">構成または置換するために使用するデリゲート、 `HttpMessageHandler` HTTP 要求を送信するために使用します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-232">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="86cb6-233">WebSocket 接続に使用されません。</span><span class="sxs-lookup"><span data-stu-id="86cb6-233">Not used for WebSocket connections.</span></span> <span data-ttu-id="86cb6-234">このデリゲートは、null 以外の値を返す必要があり、既定値をパラメーターとして受け取ります。</span><span class="sxs-lookup"><span data-stu-id="86cb6-234">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="86cb6-235">その既定値の設定を変更し、それを返すか返さまったく新しい`HttpMessageHandler`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="86cb6-235">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="86cb6-236">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="86cb6-236">Not configurable \*</span></span> | <span data-ttu-id="86cb6-237">HTTP 要求を送信するときに使用する HTTP プロキシ。</span><span class="sxs-lookup"><span data-stu-id="86cb6-237">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="86cb6-238">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="86cb6-238">Not configurable \*</span></span> | <span data-ttu-id="86cb6-239">HTTP と Websocket の要求の既定の資格情報を送信するこのブール値を設定します。</span><span class="sxs-lookup"><span data-stu-id="86cb6-239">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="86cb6-240">これにより、Windows 認証を使用できます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-240">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="86cb6-241">構成できない \*</span><span class="sxs-lookup"><span data-stu-id="86cb6-241">Not configurable \*</span></span> | <span data-ttu-id="86cb6-242">追加の WebSocket オプションを構成するために使用するデリゲート。</span><span class="sxs-lookup"><span data-stu-id="86cb6-242">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="86cb6-243">インスタンスを受け取る[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)オプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-243">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="86cb6-244">アスタリスク (\*) でマークされたオプションはブラウザー Api の制限により、JavaScript クライアントで構成できます。</span><span class="sxs-lookup"><span data-stu-id="86cb6-244">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="86cb6-245">指定されたオプションのデリゲートで、.NET クライアントでこれらのオプションを変更できます`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="86cb6-245">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="86cb6-246">指定された JavaScript オブジェクトで、JavaScript クライアントでこれらのオプションを指定できる`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="86cb6-246">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="86cb6-247">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="86cb6-247">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
