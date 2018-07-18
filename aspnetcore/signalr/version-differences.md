---
title: SignalR と ASP.NET Core SignalR の違い
author: tdykstra
description: SignalR と ASP.NET Core SignalR の違い
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: 6ed7e2e1ecadef08d71c4d7a7c3469738d07bcda
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095009"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="dac82-103">SignalR と ASP.NET Core SignalR の違い</span><span class="sxs-lookup"><span data-stu-id="dac82-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="dac82-104">ASP.NET Core SignalR は、クライアントまたは ASP.NET SignalR のサーバーと互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="dac82-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="dac82-105">この記事では、削除されたか、ASP.NET Core SignalR で変更する機能を説明します。</span><span class="sxs-lookup"><span data-stu-id="dac82-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="dac82-106">機能の相違点</span><span class="sxs-lookup"><span data-stu-id="dac82-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="dac82-107">自動再接続</span><span class="sxs-lookup"><span data-stu-id="dac82-107">Automatic reconnects</span></span>

<span data-ttu-id="dac82-108">自動再接続がサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="dac82-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="dac82-109">以前は、SignalR は、接続が切断された場合、サーバーに再接続しようとします。</span><span class="sxs-lookup"><span data-stu-id="dac82-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="dac82-110">ここでは、クライアントが切断された場合に再接続する場合は、ユーザーは新しい接続を開始に明示的にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="dac82-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="dac82-111">プロトコルのサポート</span><span class="sxs-lookup"><span data-stu-id="dac82-111">Protocol support</span></span>

<span data-ttu-id="dac82-112">ASP.NET Core SignalR は、JSON、ほかに基づく新しいバイナリ プロトコルをサポート[MessagePack](xref:signalr/messagepackhubprotocol)します。</span><span class="sxs-lookup"><span data-stu-id="dac82-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="dac82-113">さらに、カスタム プロトコルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="dac82-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="dac82-114">サーバー上の相違点</span><span class="sxs-lookup"><span data-stu-id="dac82-114">Differences on the server</span></span>

<span data-ttu-id="dac82-115">SignalR のサーバー側のライブラリに含まれる、`Microsoft.AspNetCore.App`パッケージの一部である、 **ASP.NET Core Web アプリケーション**Razor と MVC の両方のプロジェクトのテンプレート。</span><span class="sxs-lookup"><span data-stu-id="dac82-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="dac82-116">呼び出すことによって構成する必要がありますので、SignalR は ASP.NET Core のミドルウェア、`AddSignalR`で`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="dac82-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="dac82-117">ルーティングを構成するには、ハブ内にルートをマップ、`UseSignalR`でメソッドを呼び出す、`Startup.Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="dac82-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="dac82-118">スティッキー セッションが必要になりました</span><span class="sxs-lookup"><span data-stu-id="dac82-118">Sticky sessions now required</span></span>

<span data-ttu-id="dac82-119">により、どのようにスケール アウトは、SignalR の以前のバージョンで作業した、クライアントは再接続し、ファーム内の任意のサーバーにメッセージを送信でした。</span><span class="sxs-lookup"><span data-stu-id="dac82-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="dac82-120">再接続をサポートしていないほか、スケール アウト モデルの変更によりこれがサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="dac82-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="dac82-121">次に、クライアントがサーバーに接続すると、接続の間のと同じサーバーの対話が必要があります。</span><span class="sxs-lookup"><span data-stu-id="dac82-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="dac82-122">接続ごとに 1 つのハブ</span><span class="sxs-lookup"><span data-stu-id="dac82-122">Single hub per connection</span></span>

<span data-ttu-id="dac82-123">ASP.NET Core signalr では、接続モデルを簡略化されました。</span><span class="sxs-lookup"><span data-stu-id="dac82-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="dac82-124">複数のハブへのアクセスを共有するために使用されている 1 つの接続ではなく、1 つのハブに直接接続が行われます。</span><span class="sxs-lookup"><span data-stu-id="dac82-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="dac82-125">ストリーム</span><span class="sxs-lookup"><span data-stu-id="dac82-125">Streaming</span></span>

<span data-ttu-id="dac82-126">SignalR ようになりました[ストリーミング データ](xref:signalr/streaming)クライアントにハブから。</span><span class="sxs-lookup"><span data-stu-id="dac82-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="dac82-127">状態</span><span class="sxs-lookup"><span data-stu-id="dac82-127">State</span></span>

<span data-ttu-id="dac82-128">進行状況メッセージのサポートと、クライアントと (HubState と呼ばれる多くの場合)、ハブ間で任意の状態を渡す機能が削除されました。</span><span class="sxs-lookup"><span data-stu-id="dac82-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="dac82-129">現時点では、ハブ プロキシの対応はありません。</span><span class="sxs-lookup"><span data-stu-id="dac82-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="dac82-130">クライアント上の相違点</span><span class="sxs-lookup"><span data-stu-id="dac82-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="dac82-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="dac82-131">TypeScript</span></span>

<span data-ttu-id="dac82-132">ASP.NET Core のバージョンの SignalR が記述された[TypeScript](https://www.typescriptlang.org/)します。</span><span class="sxs-lookup"><span data-stu-id="dac82-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="dac82-133">使用する場合は、JavaScript または TypeScript で記述することができます、 [JavaScript クライアント](xref:signalr/javascript-client)します。</span><span class="sxs-lookup"><span data-stu-id="dac82-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="dac82-134">ホストされている JavaScript クライアント[npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="dac82-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="dac82-135">以前のバージョンで、JavaScript クライアントは、Visual Studio で NuGet パッケージから取得されました。</span><span class="sxs-lookup"><span data-stu-id="dac82-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="dac82-136">Core のバージョン、 [ @aspnet/signalr npm パッケージ](https://www.npmjs.com/package/@aspnet/signalr)JavaScript ライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="dac82-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="dac82-137">このパッケージに含まれていない、 **ASP.NET Core Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="dac82-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="dac82-138">Npm を入手してインストールを使用して、 `@aspnet/signalr` npm パッケージ。</span><span class="sxs-lookup"><span data-stu-id="dac82-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="dac82-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="dac82-139">jQuery</span></span>

<span data-ttu-id="dac82-140">JQuery に依存関係は削除されたが、プロジェクトは、jQuery を引き続き使用できます。</span><span class="sxs-lookup"><span data-stu-id="dac82-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="dac82-141">JavaScript クライアント メソッドの構文</span><span class="sxs-lookup"><span data-stu-id="dac82-141">JavaScript client method syntax</span></span>

<span data-ttu-id="dac82-142">JavaScript 構文は、SignalR の以前のバージョンから変更されました。</span><span class="sxs-lookup"><span data-stu-id="dac82-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="dac82-143">使用してではなく、`$connection`オブジェクトを使用して接続を作成、 `HubConnectionBuilder` API。</span><span class="sxs-lookup"><span data-stu-id="dac82-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="dac82-144">使用`connection.on`ハブで呼び出すことができるクライアント メソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="dac82-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="dac82-145">クライアントのメソッドを作成した後、ハブの接続を開始します。</span><span class="sxs-lookup"><span data-stu-id="dac82-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="dac82-146">チェーンを`catch`ログまたはエラーを処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="dac82-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="dac82-147">ハブ プロキシ</span><span class="sxs-lookup"><span data-stu-id="dac82-147">Hub proxies</span></span>

<span data-ttu-id="dac82-148">ハブ プロキシが自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="dac82-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="dac82-149">代わりに、メソッド名に渡される、`invoke`を文字列としての API。</span><span class="sxs-lookup"><span data-stu-id="dac82-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="dac82-150">.NET およびその他のクライアント</span><span class="sxs-lookup"><span data-stu-id="dac82-150">.NET and other clients</span></span>

<span data-ttu-id="dac82-151">`Microsoft.AspNetCore.SignalR.Client` NuGet パッケージには、ASP.NET Core SignalR の .NET クライアント ライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="dac82-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="dac82-152">使用して、`HubConnectionBuilder`作成し、ハブへの接続のインスタンスを構築します。</span><span class="sxs-lookup"><span data-stu-id="dac82-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="dac82-153">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="dac82-153">Additional resources</span></span>

* [<span data-ttu-id="dac82-154">ハブ</span><span class="sxs-lookup"><span data-stu-id="dac82-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="dac82-155">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="dac82-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="dac82-156">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="dac82-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="dac82-157">サポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="dac82-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
