---
title: ASP.NET Core SignalR では、ストリーミングを使用して、
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: 6d5f707bd2a37e1999c6e87e3cfc369aa0301207
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708440"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="ada4e-102">ASP.NET Core SignalR では、ストリーミングを使用して、</span><span class="sxs-lookup"><span data-stu-id="ada4e-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="ada4e-103">によって[真紀 Brennan](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="ada4e-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="ada4e-104">ASP.NET Core SignalR は、サーバーのメソッドの戻り値のストリーミングをサポートします。</span><span class="sxs-lookup"><span data-stu-id="ada4e-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="ada4e-105">これは、データの断片化はどこ時間の経過と共にシナリオに便利です。</span><span class="sxs-lookup"><span data-stu-id="ada4e-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="ada4e-106">戻り値は、クライアントにストリーミングされるときに各フラグメントが送信されるクライアントになったとすぐに使用可能になるすべてのデータを待機するのではなく、使用可能な。</span><span class="sxs-lookup"><span data-stu-id="ada4e-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="ada4e-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="ada4e-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="ada4e-108">ハブを設定します。</span><span class="sxs-lookup"><span data-stu-id="ada4e-108">Set up the hub</span></span>

<span data-ttu-id="ada4e-109">ハブ メソッドはストリーミングのハブ メソッドを自動的になりますが返されるときに、`ChannelReader<T>`または`Task<ChannelReader<T>>`します。</span><span class="sxs-lookup"><span data-stu-id="ada4e-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="ada4e-110">クライアントへのデータのストリーミングの基礎を示すサンプルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="ada4e-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="ada4e-111">オブジェクトが書き込まれるたびに、`ChannelReader`そのオブジェクトがすぐに、クライアントに送信します。</span><span class="sxs-lookup"><span data-stu-id="ada4e-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="ada4e-112">最後に、`ChannelReader`ストリームが閉じていることをクライアントに指示するのには完了します。</span><span class="sxs-lookup"><span data-stu-id="ada4e-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="ada4e-113">書き込み、`ChannelReader`バック グラウンド スレッドと返された場合に、`ChannelReader`できるだけ早くします。</span><span class="sxs-lookup"><span data-stu-id="ada4e-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="ada4e-114">その他のハブ呼び出しまでブロックされます、`ChannelReader`が返されます。</span><span class="sxs-lookup"><span data-stu-id="ada4e-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?range=12-36)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=11-35)]

> [!NOTE]
> <span data-ttu-id="ada4e-115">ASP.NET Core 2.2 以降を受け入れることができるハブ メソッドのストリーミング、`CancellationToken`ストリームからクライアントをアンサブスク ライブするときにトリガーされるパラメーター。</span><span class="sxs-lookup"><span data-stu-id="ada4e-115">In ASP.NET Core 2.2 or later, streaming Hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="ada4e-116">このトークンを使用して、サーバーの操作を停止し、ストリームの終了前に、クライアントが切断した場合は、リソースを解放します。</span><span class="sxs-lookup"><span data-stu-id="ada4e-116">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="ada4e-117">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="ada4e-117">.NET client</span></span>

<span data-ttu-id="ada4e-118">`StreamAsChannelAsync`メソッド`HubConnection`ストリーミング メソッドを呼び出すために使用します。</span><span class="sxs-lookup"><span data-stu-id="ada4e-118">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="ada4e-119">ハブ メソッドの名前、およびハブ メソッドで定義されている引数を渡す`StreamAsChannelAsync`します。</span><span class="sxs-lookup"><span data-stu-id="ada4e-119">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="ada4e-120">ジェネリック パラメーター`StreamAsChannelAsync<T>`ストリーミング メソッドによって返されるオブジェクトの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="ada4e-120">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="ada4e-121">A`ChannelReader<T>`ストリームの呼び出しから返され、クライアントにストリームを表します。</span><span class="sxs-lookup"><span data-stu-id="ada4e-121">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="ada4e-122">データの読み取り、一般的なパターンは、経由でループする`WaitToReadAsync`を呼び出すと`TryRead`データが使用可能な場合。</span><span class="sxs-lookup"><span data-stu-id="ada4e-122">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="ada4e-123">サーバーによって、ストリームが閉じているかに渡されたキャンセル トークンと、ループが終了`StreamAsChannelAsync`は取り消されます。</span><span class="sxs-lookup"><span data-stu-id="ada4e-123">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to 
// the server, which will trigger the corresponding token in the Hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

```csharp
var channel = await hubConnection
    .StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="ada4e-124">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="ada4e-124">JavaScript client</span></span>

<span data-ttu-id="ada4e-125">JavaScript クライアントは、ハブにストリーミング メソッドを使用して呼び出す`connection.stream`します。</span><span class="sxs-lookup"><span data-stu-id="ada4e-125">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="ada4e-126">`stream`メソッドは 2 つの引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="ada4e-126">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="ada4e-127">ハブ メソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="ada4e-127">The name of the hub method.</span></span> <span data-ttu-id="ada4e-128">次の例ではハブのメソッド名は`Counter`します。</span><span class="sxs-lookup"><span data-stu-id="ada4e-128">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="ada4e-129">ハブ メソッドで定義されている引数。</span><span class="sxs-lookup"><span data-stu-id="ada4e-129">Arguments defined in the hub method.</span></span> <span data-ttu-id="ada4e-130">次の例では、引数は: ストリーム項目を受信して、ストリームの項目間の遅延の数をカウントします。</span><span class="sxs-lookup"><span data-stu-id="ada4e-130">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="ada4e-131">`connection.stream` 返します、`IStreamResult`が含まれています、`subscribe`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ada4e-131">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="ada4e-132">渡す、`IStreamSubscriber`に`subscribe`設定と、 `next`、 `error`、および`complete`からの通知を取得するコールバック、`stream`呼び出し。</span><span class="sxs-lookup"><span data-stu-id="ada4e-132">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="ada4e-133">クライアントからストリームを終了するには、呼び出し、`dispose`メソッドを`ISubscription`から返される、`subscribe`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ada4e-133">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ada4e-134">クライアントからストリームを終了するには、呼び出し、`dispose`メソッドを`ISubscription`から返される、`subscribe`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ada4e-134">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="ada4e-135">このメソッドは、通話、 `CancellationToken` (いずれかが提供される) 場合のハブ メソッドのパラメーターを中止します。</span><span class="sxs-lookup"><span data-stu-id="ada4e-135">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="ada4e-136">関連資料</span><span class="sxs-lookup"><span data-stu-id="ada4e-136">Related resources</span></span>

* [<span data-ttu-id="ada4e-137">ハブ</span><span class="sxs-lookup"><span data-stu-id="ada4e-137">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ada4e-138">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="ada4e-138">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="ada4e-139">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="ada4e-139">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="ada4e-140">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="ada4e-140">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
