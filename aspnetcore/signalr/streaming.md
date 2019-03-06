---
title: ASP.NET Core SignalR では、ストリーミングを使用して、
author: bradygaster
description: サーバーのハブ メソッドから値のストリームを返すし、.NET、JavaScript クライアントを使用してストリームを使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: fb7183f7189d62c181f69ffdb170e3da25612919
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345588"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="34035-103">ASP.NET Core SignalR では、ストリーミングを使用して、</span><span class="sxs-lookup"><span data-stu-id="34035-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="34035-104">によって[真紀 Brennan](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="34035-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="34035-105">ASP.NET Core SignalR は、サーバーのメソッドの戻り値のストリーミングをサポートします。</span><span class="sxs-lookup"><span data-stu-id="34035-105">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="34035-106">これは、データの断片化はどこ時間の経過と共にシナリオに便利です。</span><span class="sxs-lookup"><span data-stu-id="34035-106">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="34035-107">戻り値は、クライアントにストリーミングされるときに各フラグメントが送信されるクライアントになったとすぐに使用可能になるすべてのデータを待機するのではなく、使用可能な。</span><span class="sxs-lookup"><span data-stu-id="34035-107">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="34035-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="34035-108">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="34035-109">ハブを設定します。</span><span class="sxs-lookup"><span data-stu-id="34035-109">Set up the hub</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="34035-110">ハブ メソッドはストリーミングのハブ メソッドを自動的になりますが返されるときに、 `ChannelReader<T>`、 `IAsyncEnumerable<T>`、 `Task<ChannelReader<T>>`、または`Task<IAsyncEnumerable<T>>`します。</span><span class="sxs-lookup"><span data-stu-id="34035-110">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, or `Task<IAsyncEnumerable<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="34035-111">ハブ メソッドはストリーミングのハブ メソッドを自動的になりますが返されるときに、`ChannelReader<T>`または`Task<ChannelReader<T>>`します。</span><span class="sxs-lookup"><span data-stu-id="34035-111">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="34035-112">ASP.NET Core 3.0 以降を返すことができますハブ メソッドのストリーミング`IAsyncEnumerable<T>`に加えて`ChannelReader<T>`します。</span><span class="sxs-lookup"><span data-stu-id="34035-112">In ASP.NET Core 3.0 or later, streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="34035-113">返す最も簡単な方法`IAsyncEnumerable<T>`は次の例に示すように、非同期反復子メソッドのハブ メソッドにすることです。</span><span class="sxs-lookup"><span data-stu-id="34035-113">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="34035-114">ハブ非同期反復子メソッドが受け入れることができます、`CancellationToken`ストリームからクライアントをアンサブスク ライブするときにトリガーされるパラメーター。</span><span class="sxs-lookup"><span data-stu-id="34035-114">Hub async iterator methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="34035-115">非同期反復子メソッドが返されないなどのチャネルの一般的な問題を容易に回避、`ChannelReader`早期完了しなくても、メソッドを終了するか、`ChannelWriter`します。</span><span class="sxs-lookup"><span data-stu-id="34035-115">Async iterator methods easily avoid problems common with Channels such as not returning the `ChannelReader` early enough or exiting the method without completing the `ChannelWriter`.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/sample/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="34035-116">次の例では、ストリーミング チャネルを使用してクライアントにデータの基本を示します。</span><span class="sxs-lookup"><span data-stu-id="34035-116">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="34035-117">オブジェクトが書き込まれるたびに、`ChannelWriter`そのオブジェクトがすぐに、クライアントに送信します。</span><span class="sxs-lookup"><span data-stu-id="34035-117">Whenever an object is written to the `ChannelWriter` that object is immediately sent to the client.</span></span> <span data-ttu-id="34035-118">最後に、`ChannelWriter`ストリームが閉じていることをクライアントに指示するのには完了します。</span><span class="sxs-lookup"><span data-stu-id="34035-118">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> * <span data-ttu-id="34035-119">書き込み、`ChannelWriter`バック グラウンド スレッドと返された場合に、`ChannelReader`できるだけ早くします。</span><span class="sxs-lookup"><span data-stu-id="34035-119">Write to the `ChannelWriter` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="34035-120">その他のハブ呼び出しまでブロックされます、`ChannelReader`が返されます。</span><span class="sxs-lookup"><span data-stu-id="34035-120">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>
> * <span data-ttu-id="34035-121">ロジックをラップ、`try ... catch`を完了して、`Channel`メソッドの呼び出しが正常に完了した、catch、および外部ハブを確認する catch します。</span><span class="sxs-lookup"><span data-stu-id="34035-121">Wrap your logic in a `try ... catch` and complete the `Channel` in the catch and outside the catch to make sure the hub method invocation is completed properly.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

<span data-ttu-id="34035-122">ASP.NET Core 2.2 以降を受け入れることができるハブ メソッドのストリーミング、`CancellationToken`ストリームからクライアントをアンサブスク ライブするときにトリガーされるパラメーター。</span><span class="sxs-lookup"><span data-stu-id="34035-122">In ASP.NET Core 2.2 or later, streaming hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="34035-123">このトークンを使用して、サーバーの操作を停止し、ストリームの終了前に、クライアントが切断した場合は、リソースを解放します。</span><span class="sxs-lookup"><span data-stu-id="34035-123">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="34035-124">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="34035-124">.NET client</span></span>

<span data-ttu-id="34035-125">`StreamAsChannelAsync`メソッド`HubConnection`ストリーミング メソッドを呼び出すために使用します。</span><span class="sxs-lookup"><span data-stu-id="34035-125">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="34035-126">ハブ メソッドの名前、およびハブ メソッドで定義されている引数を渡す`StreamAsChannelAsync`します。</span><span class="sxs-lookup"><span data-stu-id="34035-126">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="34035-127">ジェネリック パラメーター`StreamAsChannelAsync<T>`ストリーミング メソッドによって返されるオブジェクトの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="34035-127">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="34035-128">A`ChannelReader<T>`ストリームの呼び出しから返され、クライアントにストリームを表します。</span><span class="sxs-lookup"><span data-stu-id="34035-128">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="34035-129">データの読み取り、一般的なパターンは、経由でループする`WaitToReadAsync`を呼び出すと`TryRead`データが使用可能な場合。</span><span class="sxs-lookup"><span data-stu-id="34035-129">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="34035-130">サーバーによって、ストリームが閉じているかに渡されたキャンセル トークンと、ループが終了`StreamAsChannelAsync`は取り消されます。</span><span class="sxs-lookup"><span data-stu-id="34035-130">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
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

## <a name="javascript-client"></a><span data-ttu-id="34035-131">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="34035-131">JavaScript client</span></span>

<span data-ttu-id="34035-132">JavaScript クライアントは、ハブにストリーミング メソッドを使用して呼び出す`connection.stream`します。</span><span class="sxs-lookup"><span data-stu-id="34035-132">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="34035-133">`stream`メソッドは 2 つの引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="34035-133">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="34035-134">ハブ メソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="34035-134">The name of the hub method.</span></span> <span data-ttu-id="34035-135">次の例ではハブのメソッド名は`Counter`します。</span><span class="sxs-lookup"><span data-stu-id="34035-135">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="34035-136">ハブ メソッドで定義されている引数。</span><span class="sxs-lookup"><span data-stu-id="34035-136">Arguments defined in the hub method.</span></span> <span data-ttu-id="34035-137">次の例では、引数は: ストリーム項目を受信して、ストリームの項目間の遅延の数をカウントします。</span><span class="sxs-lookup"><span data-stu-id="34035-137">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="34035-138">`connection.stream` 返します、`IStreamResult`が含まれています、`subscribe`メソッド。</span><span class="sxs-lookup"><span data-stu-id="34035-138">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="34035-139">渡す、`IStreamSubscriber`に`subscribe`設定と、 `next`、 `error`、および`complete`からの通知を取得するコールバック、`stream`呼び出し。</span><span class="sxs-lookup"><span data-stu-id="34035-139">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="34035-140">クライアントからストリームを終了するには、呼び出し、`dispose`メソッドを`ISubscription`から返される、`subscribe`メソッド。</span><span class="sxs-lookup"><span data-stu-id="34035-140">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="34035-141">クライアントからストリームを終了するには、呼び出し、`dispose`メソッドを`ISubscription`から返される、`subscribe`メソッド。</span><span class="sxs-lookup"><span data-stu-id="34035-141">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="34035-142">このメソッドは、通話、 `CancellationToken` (いずれかが提供される) 場合のハブ メソッドのパラメーターを中止します。</span><span class="sxs-lookup"><span data-stu-id="34035-142">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"
## <a name="java-client"></a><span data-ttu-id="34035-143">Java クライアント</span><span class="sxs-lookup"><span data-stu-id="34035-143">Java client</span></span>
<span data-ttu-id="34035-144">SignalR の Java クライアントを使用して、`stream`ストリーミング メソッドを呼び出すメソッド。</span><span class="sxs-lookup"><span data-stu-id="34035-144">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="34035-145">次の 3 つ以上の引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="34035-145">It accepts three or more arguments:</span></span>

* <span data-ttu-id="34035-146">ストリームの項目の予期される型</span><span class="sxs-lookup"><span data-stu-id="34035-146">The expected type of the stream items</span></span> 
* <span data-ttu-id="34035-147">ハブ メソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="34035-147">The name of the hub method.</span></span>
* <span data-ttu-id="34035-148">ハブ メソッドで定義されている引数。</span><span class="sxs-lookup"><span data-stu-id="34035-148">Arguments defined in the hub method.</span></span> 

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```
<span data-ttu-id="34035-149">`stream`メソッド`HubConnection`ストリーム項目の種類の観測可能なオブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="34035-149">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="34035-150">監視可能な型の`subscribe`メソッドでは、定義、 `onNext`、`onError`と`onCompleted`ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="34035-150">The Observable type's `subscribe` method is where you define your `onNext`,  `onError` and  `onCompleted` handlers.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="34035-151">関連資料</span><span class="sxs-lookup"><span data-stu-id="34035-151">Related resources</span></span>

* [<span data-ttu-id="34035-152">ハブ</span><span class="sxs-lookup"><span data-stu-id="34035-152">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="34035-153">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="34035-153">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="34035-154">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="34035-154">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="34035-155">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="34035-155">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
