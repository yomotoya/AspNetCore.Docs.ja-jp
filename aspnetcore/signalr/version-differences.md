---
title: SignalR と ASP.NET Core SignalR の相違点
author: rachelappel
description: SignalR と ASP.NET Core SignalR の相違点
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090065"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="51596-103">SignalR と ASP.NET Core SignalR の相違点</span><span class="sxs-lookup"><span data-stu-id="51596-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="51596-104">ASP.NET Core SignalR は、クライアントまたは ASP.NET SignalR のサーバーと互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="51596-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="51596-105">この記事では、削除または ASP.NET Core SignalR で変更された機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="51596-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="51596-106">機能の相違点</span><span class="sxs-lookup"><span data-stu-id="51596-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="51596-107">自動再接続</span><span class="sxs-lookup"><span data-stu-id="51596-107">Automatic reconnects</span></span>

<span data-ttu-id="51596-108">自動再接続がサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="51596-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="51596-109">以前は、SignalR では、接続が削除された場合、サーバーに再接続しようとしました。</span><span class="sxs-lookup"><span data-stu-id="51596-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="51596-110">ここでは、クライアントが切断された場合に再接続する場合は、ユーザーは新しい接続を開始に明示的にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="51596-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="51596-111">プロトコルのサポート</span><span class="sxs-lookup"><span data-stu-id="51596-111">Protocol support</span></span>

<span data-ttu-id="51596-112">ASP.NET Core SignalR に基づく新しいバイナリ プロトコルと同様に、JSON をサポートしている[MessagePack](xref:signalr/messagepackhubprotocol)です。</span><span class="sxs-lookup"><span data-stu-id="51596-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="51596-113">さらに、カスタム プロトコルを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="51596-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="51596-114">サーバー上の相違点</span><span class="sxs-lookup"><span data-stu-id="51596-114">Differences on the server</span></span>

<span data-ttu-id="51596-115">SignalR のサーバー側のライブラリに含まれる、`Microsoft.AspNetCore.App`パッケージの一部である、 **ASP.NET Core Web アプリケーション**Razor と MVC プロジェクトの両方のテンプレートです。</span><span class="sxs-lookup"><span data-stu-id="51596-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="51596-116">SignalR には、ASP.NET Core ミドルウェアがあるため呼び出すことによって構成する必要がありますに`AddSignalR`で`Startup.ConfigureServices`です。</span><span class="sxs-lookup"><span data-stu-id="51596-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="51596-117">ルーティングを構成するには、ハブ内にルートをマップ、`UseSignalR`メソッドの呼び出し、`Startup.Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="51596-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="51596-118">スティッキー セッションが必要になりました</span><span class="sxs-lookup"><span data-stu-id="51596-118">Sticky sessions now required</span></span>

<span data-ttu-id="51596-119">どのように対応するスケール アウト SignalR の以前のバージョンで、作業のためクライアントでした再接続し、ファーム内の任意のサーバーにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="51596-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="51596-120">変更されたのため、スケール アウト モデル、およびに再接続をサポートしていない、これはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="51596-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="51596-121">ここで、クライアントがサーバーに接続したら、接続の間の同じサーバーと対話する必要があります。</span><span class="sxs-lookup"><span data-stu-id="51596-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="51596-122">接続ごとに 1 つのハブ</span><span class="sxs-lookup"><span data-stu-id="51596-122">Single hub per connection</span></span>

<span data-ttu-id="51596-123">ASP.NET Core SignalR で接続モデルを簡素化されています。</span><span class="sxs-lookup"><span data-stu-id="51596-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="51596-124">複数のハブへのアクセスを共有するために使用されている 1 つの接続ではなく、1 つのハブに直接接続が行われます。</span><span class="sxs-lookup"><span data-stu-id="51596-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="51596-125">ストリーム</span><span class="sxs-lookup"><span data-stu-id="51596-125">Streaming</span></span>

<span data-ttu-id="51596-126">SignalR なりました[ストリーミング データ](xref:signalr/streaming)クライアント ハブからです。</span><span class="sxs-lookup"><span data-stu-id="51596-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="51596-127">状態</span><span class="sxs-lookup"><span data-stu-id="51596-127">State</span></span>

<span data-ttu-id="51596-128">進行状況メッセージのサポートに加えて、クライアントと、ハブ (HubState と呼びます) 間で任意の状態を渡す機能が削除されましたがします。</span><span class="sxs-lookup"><span data-stu-id="51596-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="51596-129">現時点では、相当するハブ プロキシはありません。</span><span class="sxs-lookup"><span data-stu-id="51596-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="51596-130">クライアント上の相違点</span><span class="sxs-lookup"><span data-stu-id="51596-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="51596-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="51596-131">TypeScript</span></span>

<span data-ttu-id="51596-132">SignalR の ASP.NET Core バージョンがで記述された[TypeScript](https://www.typescriptlang.org/)です。</span><span class="sxs-lookup"><span data-stu-id="51596-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="51596-133">使用する場合は、JavaScript または TypeScript で記述することができます、 [JavaScript クライアント](xref:signalr/javascript-client)です。</span><span class="sxs-lookup"><span data-stu-id="51596-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="51596-134">JavaScript クライアントがホストされている[npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="51596-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="51596-135">以前のバージョンでの JavaScript クライアントは、Visual Studio での NuGet パッケージから取得されました。</span><span class="sxs-lookup"><span data-stu-id="51596-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="51596-136">コアのバージョンの[ @aspnet/signalr npm パッケージ](https://www.npmjs.com/package/@aspnet/signalr)JavaScript ライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="51596-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="51596-137">このパッケージに含まれていない、 **ASP.NET Core Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="51596-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="51596-138">Npm を使用して、入手してインストール、 `@aspnet/signalr` npm パッケージです。</span><span class="sxs-lookup"><span data-stu-id="51596-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="51596-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="51596-139">jQuery</span></span>

<span data-ttu-id="51596-140">JQuery への依存関係が削除されました、ただし、プロジェクトは、jQuery を引き続き使用できます。</span><span class="sxs-lookup"><span data-stu-id="51596-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="51596-141">JavaScript クライアント メソッドの構文</span><span class="sxs-lookup"><span data-stu-id="51596-141">JavaScript client method syntax</span></span>

<span data-ttu-id="51596-142">JavaScript 構文は、SignalR の以前のバージョンから変更されました。</span><span class="sxs-lookup"><span data-stu-id="51596-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="51596-143">使用するのではなく、`$connection`オブジェクトを使用して接続を作成、 `HubConnectionBuilder` API です。</span><span class="sxs-lookup"><span data-stu-id="51596-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="51596-144">使用して`connection.on`ハブで呼び出すことができるクライアント メソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="51596-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="51596-145">クライアントのメソッドを作成すると、ハブ接続を開始します。</span><span class="sxs-lookup"><span data-stu-id="51596-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="51596-146">チェーン、`catch`ログまたはエラーを処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="51596-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="51596-147">ハブ プロキシ</span><span class="sxs-lookup"><span data-stu-id="51596-147">Hub proxies</span></span>

<span data-ttu-id="51596-148">ハブ プロキシが自動的に行われなく生成されます。</span><span class="sxs-lookup"><span data-stu-id="51596-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="51596-149">代わりに、メソッド名に渡される、 `invoke` API には文字列です。</span><span class="sxs-lookup"><span data-stu-id="51596-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="51596-150">.NET およびその他のクライアント</span><span class="sxs-lookup"><span data-stu-id="51596-150">.NET and other clients</span></span>

<span data-ttu-id="51596-151">`Microsoft.AspNetCore.SignalR.Client` NuGet パッケージには、ASP.NET Core SignalR 用の .NET クライアント ライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="51596-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="51596-152">使用して、`HubConnectionBuilder`を作成し、ハブへの接続のインスタンスを構築します。</span><span class="sxs-lookup"><span data-stu-id="51596-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="51596-153">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="51596-153">Additional resources</span></span>

* [<span data-ttu-id="51596-154">ハブ</span><span class="sxs-lookup"><span data-stu-id="51596-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="51596-155">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="51596-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="51596-156">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="51596-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="51596-157">サポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="51596-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
