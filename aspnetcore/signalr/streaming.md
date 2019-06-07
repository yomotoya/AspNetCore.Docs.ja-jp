---
title: ASP.NET Core SignalR では、ストリーミングを使用して、
author: bradygaster
description: クライアントとサーバー間でデータをストリーミングする方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/05/2019
uid: signalr/streaming
ms.openlocfilehash: a75156f398e113393ddb891d16eec3f09de80c09
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750195"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="7f446-103">ASP.NET Core SignalR では、ストリーミングを使用して、</span><span class="sxs-lookup"><span data-stu-id="7f446-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="7f446-104">によって[真紀 Brennan](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="7f446-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7f446-105">ASP.NET Core SignalR は、クライアント サーバーからとサーバーからクライアントへのストリーミングをサポートします。</span><span class="sxs-lookup"><span data-stu-id="7f446-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="7f446-106">これは、時間の経過と共にデータの断片化が到着した場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="7f446-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="7f446-107">ストリーミングする場合、各フラグメントが送信されるクライアントまたはサーバーになったとすぐに待機しているすべての使用可能になるデータではなく、使用可能です。</span><span class="sxs-lookup"><span data-stu-id="7f446-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7f446-108">ASP.NET Core SignalR は、サーバーのメソッドの戻り値のストリーミングをサポートします。</span><span class="sxs-lookup"><span data-stu-id="7f446-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="7f446-109">これは、時間の経過と共にデータの断片化が到着した場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="7f446-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="7f446-110">戻り値は、クライアントにストリーミングされるときに各フラグメントが送信されるクライアントになったとすぐに使用可能になるすべてのデータを待機するのではなく、使用可能な。</span><span class="sxs-lookup"><span data-stu-id="7f446-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="7f446-111">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="7f446-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="7f446-112">ストリーミング用のハブを設定します。</span><span class="sxs-lookup"><span data-stu-id="7f446-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7f446-113">ハブ メソッドはストリーミングのハブ メソッドを自動的になりますが返されるときに<xref:System.Collections.Generic.IAsyncEnumerable`1>、 <xref:System.Threading.Channels.ChannelReader%601>、 `Task<IAsyncEnumerable<T>>`、または`Task<ChannelReader<T>>`します。</span><span class="sxs-lookup"><span data-stu-id="7f446-113">A hub method automatically becomes a streaming hub method when it returns <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, or `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7f446-114">ハブ メソッドはストリーミングのハブ メソッドを自動的になりますが返されるときに、<xref:System.Threading.Channels.ChannelReader%601>または`Task<ChannelReader<T>>`します。</span><span class="sxs-lookup"><span data-stu-id="7f446-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="7f446-115">サーバーからクライアントのストリーミング</span><span class="sxs-lookup"><span data-stu-id="7f446-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7f446-116">ハブ メソッドのストリーミングを返せる`IAsyncEnumerable<T>`に加えて`ChannelReader<T>`します。</span><span class="sxs-lookup"><span data-stu-id="7f446-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="7f446-117">返す最も簡単な方法`IAsyncEnumerable<T>`は次の例に示すように、非同期反復子メソッドのハブ メソッドにすることです。</span><span class="sxs-lookup"><span data-stu-id="7f446-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="7f446-118">ハブ非同期反復子メソッドが受け入れることができます、`CancellationToken`ストリームからクライアントをアンサブスク ライブときにトリガーされるパラメーター。</span><span class="sxs-lookup"><span data-stu-id="7f446-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="7f446-119">非同期反復子メソッドが返されないなどのチャネルでの一般的な問題を回避、`ChannelReader`早期完了しなくても、メソッドを終了するか、<xref:System.Threading.Channels.ChannelWriter`1>します。</span><span class="sxs-lookup"><span data-stu-id="7f446-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="7f446-120">次の例では、ストリーミング チャネルを使用してクライアントにデータの基本を示します。</span><span class="sxs-lookup"><span data-stu-id="7f446-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="7f446-121">オブジェクトが書き込まれるたびに、<xref:System.Threading.Channels.ChannelWriter%601>オブジェクトがすぐに、クライアントに送信します。</span><span class="sxs-lookup"><span data-stu-id="7f446-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="7f446-122">最後に、`ChannelWriter`ストリームが閉じていることをクライアントに指示するのには完了します。</span><span class="sxs-lookup"><span data-stu-id="7f446-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="7f446-123">書き込み、`ChannelWriter<T>`バック グラウンド スレッドと返された場合に、`ChannelReader`できるだけ早くします。</span><span class="sxs-lookup"><span data-stu-id="7f446-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="7f446-124">その他のハブ呼び出しがまでブロックされます、`ChannelReader`が返されます。</span><span class="sxs-lookup"><span data-stu-id="7f446-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="7f446-125">内のロジックをラップする`try ... catch`します。</span><span class="sxs-lookup"><span data-stu-id="7f446-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="7f446-126">完了、`Channel`で、`catch`内外の`catch`メソッドの呼び出しが正しく完了したハブを確認します。</span><span class="sxs-lookup"><span data-stu-id="7f446-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Streaming hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/samples/2.2/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/samples/2.1/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7f446-127">ハブ メソッドのストリーミング サーバーからクライアントが受け入れることができます、`CancellationToken`ストリームからクライアントをアンサブスク ライブときにトリガーされるパラメーター。</span><span class="sxs-lookup"><span data-stu-id="7f446-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="7f446-128">このトークンを使用して、サーバーの操作を停止し、ストリームの終了前に、クライアントが切断した場合は、リソースを解放します。</span><span class="sxs-lookup"><span data-stu-id="7f446-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="7f446-129">クライアントとサーバーのストリーミング</span><span class="sxs-lookup"><span data-stu-id="7f446-129">Client-to-server streaming</span></span>

<span data-ttu-id="7f446-130">ハブ メソッドはクライアントからサーバーへのストリーミングのハブ メソッドを自動的になりますが、型の 1 つまたは複数のオブジェクトを受け入れる場合<xref:System.Threading.Channels.ChannelReader%601>または<xref:System.Collections.Generic.IAsyncEnumerable%601>します。</span><span class="sxs-lookup"><span data-stu-id="7f446-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more objects of type <xref:System.Threading.Channels.ChannelReader%601> or <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span></span> <span data-ttu-id="7f446-131">次の例では、クライアントから送信されたストリーミング データの読み取りの基礎を示します。</span><span class="sxs-lookup"><span data-stu-id="7f446-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="7f446-132">クライアントが書き込むたびに、<xref:System.Threading.Channels.ChannelWriter%601>に、データが書き込まれる、`ChannelReader`のハブ メソッドの読み取り元となるサーバー上。</span><span class="sxs-lookup"><span data-stu-id="7f446-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter%601>, the data is written into the `ChannelReader` on the server from which the hub method is reading.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

<span data-ttu-id="7f446-133"><xref:System.Collections.Generic.IAsyncEnumerable%601>メソッドのバージョンに依存します。</span><span class="sxs-lookup"><span data-stu-id="7f446-133">An <xref:System.Collections.Generic.IAsyncEnumerable%601> version of the method follows.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
public async Task UploadStream(IAsyncEnumerable<Stream> stream) 
{
    await foreach (var item in stream)
    {
        Console.WriteLine(item);
    }
}
```

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="7f446-134">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="7f446-134">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="7f446-135">サーバーからクライアントのストリーミング</span><span class="sxs-lookup"><span data-stu-id="7f446-135">Server-to-client streaming</span></span>


::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7f446-136">`StreamAsync`と`StreamAsChannelAsync`メソッド`HubConnection`サーバーからクライアントにストリーミング メソッドの呼び出しに使用されます。</span><span class="sxs-lookup"><span data-stu-id="7f446-136">The `StreamAsync` and `StreamAsChannelAsync` methods on `HubConnection` are used to invoke server-to-client streaming methods.</span></span> <span data-ttu-id="7f446-137">メソッド名のハブおよびハブ メソッドで定義されている引数を渡す`StreamAsync`または`StreamAsChannelAsync`します。</span><span class="sxs-lookup"><span data-stu-id="7f446-137">Pass the hub method name and arguments defined in the hub method to `StreamAsync` or `StreamAsChannelAsync`.</span></span> <span data-ttu-id="7f446-138">ジェネリック パラメーター`StreamAsync<T>`と`StreamAsChannelAsync<T>`ストリーミング メソッドによって返されるオブジェクトの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="7f446-138">The generic parameter on `StreamAsync<T>` and `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="7f446-139">型のオブジェクト`IAsyncEnumerable<T>`または`ChannelReader<T>`ストリーム呼び出しから返され、クライアントにストリームを表します。</span><span class="sxs-lookup"><span data-stu-id="7f446-139">An object of type `IAsyncEnumerable<T>` or `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

<span data-ttu-id="7f446-140">A`StreamAsync`を返す例`IAsyncEnumerable<int>`:</span><span class="sxs-lookup"><span data-stu-id="7f446-140">A `StreamAsync` example that returns `IAsyncEnumerable<int>`:</span></span>

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var stream = await hubConnection.StreamAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

await foreach (var count in stream)
{
    Console.WriteLine($"{count}");
}

Console.WriteLine("Streaming completed");
```

<span data-ttu-id="7f446-141">対応する`StreamAsChannelAsync`を返す例`ChannelReader<int>`:</span><span class="sxs-lookup"><span data-stu-id="7f446-141">A corresponding `StreamAsChannelAsync` example that returns `ChannelReader<int>`:</span></span>

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

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7f446-142">`StreamAsChannelAsync`メソッド`HubConnection`ストリーミング サーバーからクライアント メソッドを呼び出すために使用します。</span><span class="sxs-lookup"><span data-stu-id="7f446-142">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="7f446-143">メソッド名のハブおよびハブ メソッドで定義されている引数を渡す`StreamAsChannelAsync`します。</span><span class="sxs-lookup"><span data-stu-id="7f446-143">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="7f446-144">ジェネリック パラメーター`StreamAsChannelAsync<T>`ストリーミング メソッドによって返されるオブジェクトの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="7f446-144">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="7f446-145">A`ChannelReader<T>`ストリーム呼び出しから返され、クライアントにストリームを表します。</span><span class="sxs-lookup"><span data-stu-id="7f446-145">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

<span data-ttu-id="7f446-146">`StreamAsChannelAsync`メソッド`HubConnection`ストリーミング サーバーからクライアント メソッドを呼び出すために使用します。</span><span class="sxs-lookup"><span data-stu-id="7f446-146">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="7f446-147">メソッド名のハブおよびハブ メソッドで定義されている引数を渡す`StreamAsChannelAsync`します。</span><span class="sxs-lookup"><span data-stu-id="7f446-147">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="7f446-148">ジェネリック パラメーター`StreamAsChannelAsync<T>`ストリーミング メソッドによって返されるオブジェクトの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="7f446-148">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="7f446-149">A`ChannelReader<T>`ストリーム呼び出しから返され、クライアントにストリームを表します。</span><span class="sxs-lookup"><span data-stu-id="7f446-149">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="7f446-150">クライアントとサーバーのストリーミング</span><span class="sxs-lookup"><span data-stu-id="7f446-150">Client-to-server streaming</span></span>

<span data-ttu-id="7f446-151">.NET クライアントからサーバーへのクライアント ストリーミング ハブ メソッドを呼び出すための 2 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="7f446-151">There are two ways to invoke a client-to-server streaming hub method from the .NET client.</span></span> <span data-ttu-id="7f446-152">渡したりすることができます、`IAsyncEnumerable<T>`または`ChannelReader`への引数として`SendAsync`、 `InvokeAsync`、または`StreamAsChannelAsync`のハブ メソッドが呼び出されるとは異なりますが、します。</span><span class="sxs-lookup"><span data-stu-id="7f446-152">You can either pass in an `IAsyncEnumerable<T>` or a `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="7f446-153">データが書き込まれるたびに、`IAsyncEnumerable`または`ChannelWriter`オブジェクト、サーバー上のハブ メソッドは、クライアントからのデータの新しい項目を受信します。</span><span class="sxs-lookup"><span data-stu-id="7f446-153">Whenever data is written to the `IAsyncEnumerable` or `ChannelWriter` object, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="7f446-154">使用する場合、`IAsyncEnumerable`オブジェクト、ストリームが終了の項目を返すメソッドの後、ストリームを終了します。</span><span class="sxs-lookup"><span data-stu-id="7f446-154">If using an `IAsyncEnumerable` object, the stream ends after the method returning stream items exits.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
    //After the for loop has completed and the local function exits the stream completion will be sent.
}

await connection.SendAsync("UploadStream", clientStreamData());
```

<span data-ttu-id="7f446-155">使用する場合や、 `ChannelWriter`、チャネルを完了する`channel.Writer.Complete()`:</span><span class="sxs-lookup"><span data-stu-id="7f446-155">Or if you're using a `ChannelWriter`, you complete the channel with `channel.Writer.Complete()`:</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="7f446-156">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="7f446-156">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="7f446-157">サーバーからクライアントのストリーミング</span><span class="sxs-lookup"><span data-stu-id="7f446-157">Server-to-client streaming</span></span>

<span data-ttu-id="7f446-158">JavaScript クライアントでは、ハブでのサーバーからクライアントにストリーミング メソッドを呼び出す`connection.stream`します。</span><span class="sxs-lookup"><span data-stu-id="7f446-158">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="7f446-159">`stream`メソッドは 2 つの引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="7f446-159">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="7f446-160">ハブ メソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="7f446-160">The name of the hub method.</span></span> <span data-ttu-id="7f446-161">次の例ではハブのメソッド名は`Counter`します。</span><span class="sxs-lookup"><span data-stu-id="7f446-161">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="7f446-162">ハブ メソッドで定義されている引数。</span><span class="sxs-lookup"><span data-stu-id="7f446-162">Arguments defined in the hub method.</span></span> <span data-ttu-id="7f446-163">次の例では、引数はストリーム項目を受信して、ストリームの項目間の遅延の数をカウントします。</span><span class="sxs-lookup"><span data-stu-id="7f446-163">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="7f446-164">`connection.stream` 返します、`IStreamResult`が含まれています、`subscribe`メソッド。</span><span class="sxs-lookup"><span data-stu-id="7f446-164">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="7f446-165">渡す、`IStreamSubscriber`に`subscribe`設定と、 `next`、 `error`、および`complete`から通知を受信するコールバック、`stream`呼び出し。</span><span class="sxs-lookup"><span data-stu-id="7f446-165">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="7f446-166">クライアントからストリームを終了するには、呼び出し、`dispose`メソッドを`ISubscription`から返される、`subscribe`メソッド。</span><span class="sxs-lookup"><span data-stu-id="7f446-166">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="7f446-167">このメソッドを呼び出すことにより、`CancellationToken`いずれかを指定した場合、ハブ メソッドのパラメーター。</span><span class="sxs-lookup"><span data-stu-id="7f446-167">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="7f446-168">クライアントからストリームを終了するには、呼び出し、`dispose`メソッドを`ISubscription`から返される、`subscribe`メソッド。</span><span class="sxs-lookup"><span data-stu-id="7f446-168">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="7f446-169">クライアントとサーバーのストリーミング</span><span class="sxs-lookup"><span data-stu-id="7f446-169">Client-to-server streaming</span></span>

<span data-ttu-id="7f446-170">JavaScript クライアントでは、ハブのクライアントとサーバーのストリーミング メソッドを呼び出すを渡すことによって、`Subject`への引数として`send`、 `invoke`、または`stream`のハブ メソッドが呼び出されるとは異なりますが、します。</span><span class="sxs-lookup"><span data-stu-id="7f446-170">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="7f446-171">`Subject`は次のようなクラス、`Subject`します。</span><span class="sxs-lookup"><span data-stu-id="7f446-171">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="7f446-172">たとえば、RxJS に使える、[サブジェクト](https://rxjs-dev.firebaseapp.com/api/index/class/Subject)ライブラリからのクラス。</span><span class="sxs-lookup"><span data-stu-id="7f446-172">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="7f446-173">呼び出す`subject.next(item)`で項目が、項目をストリームに書き込みますおよびハブ メソッドは、サーバー上の項目を受信します。</span><span class="sxs-lookup"><span data-stu-id="7f446-173">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="7f446-174">ストリームを終了するには、呼び出す`subject.complete()`します。</span><span class="sxs-lookup"><span data-stu-id="7f446-174">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="7f446-175">Java クライアント</span><span class="sxs-lookup"><span data-stu-id="7f446-175">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="7f446-176">サーバーからクライアントのストリーミング</span><span class="sxs-lookup"><span data-stu-id="7f446-176">Server-to-client streaming</span></span>

<span data-ttu-id="7f446-177">SignalR の Java クライアントを使用して、`stream`ストリーミング メソッドを呼び出すメソッド。</span><span class="sxs-lookup"><span data-stu-id="7f446-177">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="7f446-178">`stream` 次の 3 つ以上の引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="7f446-178">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="7f446-179">ストリームの項目の予測される型。</span><span class="sxs-lookup"><span data-stu-id="7f446-179">The expected type of the stream items.</span></span>
* <span data-ttu-id="7f446-180">ハブ メソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="7f446-180">The name of the hub method.</span></span>
* <span data-ttu-id="7f446-181">ハブ メソッドで定義されている引数。</span><span class="sxs-lookup"><span data-stu-id="7f446-181">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="7f446-182">`stream`メソッド`HubConnection`ストリーム項目の種類の観測可能なオブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="7f446-182">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="7f446-183">監視可能な型の`subscribe`メソッドは、where `onNext`、`onError`と`onCompleted`ハンドラーが定義されています。</span><span class="sxs-lookup"><span data-stu-id="7f446-183">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="7f446-184">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="7f446-184">Additional resources</span></span>

* [<span data-ttu-id="7f446-185">ハブ</span><span class="sxs-lookup"><span data-stu-id="7f446-185">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="7f446-186">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="7f446-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="7f446-187">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="7f446-187">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="7f446-188">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="7f446-188">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
