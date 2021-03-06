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
# <a name="use-streaming-in-aspnet-core-signalr"></a>ASP.NET Core SignalR では、ストリーミングを使用して、

によって[真紀 Brennan](https://github.com/BrennanConroy)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core SignalR は、クライアント サーバーからとサーバーからクライアントへのストリーミングをサポートします。 これは、時間の経過と共にデータの断片化が到着した場合に便利です。 ストリーミングする場合、各フラグメントが送信されるクライアントまたはサーバーになったとすぐに待機しているすべての使用可能になるデータではなく、使用可能です。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core SignalR は、サーバーのメソッドの戻り値のストリーミングをサポートします。 これは、時間の経過と共にデータの断片化が到着した場合に便利です。 戻り値は、クライアントにストリーミングされるときに各フラグメントが送信されるクライアントになったとすぐに使用可能になるすべてのデータを待機するのではなく、使用可能な。

::: moniker-end

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

## <a name="set-up-a-hub-for-streaming"></a>ストリーミング用のハブを設定します。

::: moniker range=">= aspnetcore-3.0"

ハブ メソッドはストリーミングのハブ メソッドを自動的になりますが返されるときに<xref:System.Collections.Generic.IAsyncEnumerable`1>、 <xref:System.Threading.Channels.ChannelReader%601>、 `Task<IAsyncEnumerable<T>>`、または`Task<ChannelReader<T>>`します。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ハブ メソッドはストリーミングのハブ メソッドを自動的になりますが返されるときに、<xref:System.Threading.Channels.ChannelReader%601>または`Task<ChannelReader<T>>`します。

::: moniker-end

### <a name="server-to-client-streaming"></a>サーバーからクライアントのストリーミング

::: moniker range=">= aspnetcore-3.0"

ハブ メソッドのストリーミングを返せる`IAsyncEnumerable<T>`に加えて`ChannelReader<T>`します。 返す最も簡単な方法`IAsyncEnumerable<T>`は次の例に示すように、非同期反復子メソッドのハブ メソッドにすることです。 ハブ非同期反復子メソッドが受け入れることができます、`CancellationToken`ストリームからクライアントをアンサブスク ライブときにトリガーされるパラメーター。 非同期反復子メソッドが返されないなどのチャネルでの一般的な問題を回避、`ChannelReader`早期完了しなくても、メソッドを終了するか、<xref:System.Threading.Channels.ChannelWriter`1>します。

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

次の例では、ストリーミング チャネルを使用してクライアントにデータの基本を示します。 オブジェクトが書き込まれるたびに、<xref:System.Threading.Channels.ChannelWriter%601>オブジェクトがすぐに、クライアントに送信します。 最後に、`ChannelWriter`ストリームが閉じていることをクライアントに指示するのには完了します。

> [!NOTE]
> 書き込み、`ChannelWriter<T>`バック グラウンド スレッドと返された場合に、`ChannelReader`できるだけ早くします。 その他のハブ呼び出しがまでブロックされます、`ChannelReader`が返されます。
>
> 内のロジックをラップする`try ... catch`します。 完了、`Channel`で、`catch`内外の`catch`メソッドの呼び出しが正しく完了したハブを確認します。

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

ハブ メソッドのストリーミング サーバーからクライアントが受け入れることができます、`CancellationToken`ストリームからクライアントをアンサブスク ライブときにトリガーされるパラメーター。 このトークンを使用して、サーバーの操作を停止し、ストリームの終了前に、クライアントが切断した場合は、リソースを解放します。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>クライアントとサーバーのストリーミング

ハブ メソッドはクライアントからサーバーへのストリーミングのハブ メソッドを自動的になりますが、型の 1 つまたは複数のオブジェクトを受け入れる場合<xref:System.Threading.Channels.ChannelReader%601>または<xref:System.Collections.Generic.IAsyncEnumerable%601>します。 次の例では、クライアントから送信されたストリーミング データの読み取りの基礎を示します。 クライアントが書き込むたびに、<xref:System.Threading.Channels.ChannelWriter%601>に、データが書き込まれる、`ChannelReader`のハブ メソッドの読み取り元となるサーバー上。

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

<xref:System.Collections.Generic.IAsyncEnumerable%601>メソッドのバージョンに依存します。

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

## <a name="net-client"></a>.NET クライアント

### <a name="server-to-client-streaming"></a>サーバーからクライアントのストリーミング


::: moniker range=">= aspnetcore-3.0"

`StreamAsync`と`StreamAsChannelAsync`メソッド`HubConnection`サーバーからクライアントにストリーミング メソッドの呼び出しに使用されます。 メソッド名のハブおよびハブ メソッドで定義されている引数を渡す`StreamAsync`または`StreamAsChannelAsync`します。 ジェネリック パラメーター`StreamAsync<T>`と`StreamAsChannelAsync<T>`ストリーミング メソッドによって返されるオブジェクトの種類を指定します。 型のオブジェクト`IAsyncEnumerable<T>`または`ChannelReader<T>`ストリーム呼び出しから返され、クライアントにストリームを表します。

A`StreamAsync`を返す例`IAsyncEnumerable<int>`:

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

対応する`StreamAsChannelAsync`を返す例`ChannelReader<int>`:

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

`StreamAsChannelAsync`メソッド`HubConnection`ストリーミング サーバーからクライアント メソッドを呼び出すために使用します。 メソッド名のハブおよびハブ メソッドで定義されている引数を渡す`StreamAsChannelAsync`します。 ジェネリック パラメーター`StreamAsChannelAsync<T>`ストリーミング メソッドによって返されるオブジェクトの種類を指定します。 A`ChannelReader<T>`ストリーム呼び出しから返され、クライアントにストリームを表します。

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

`StreamAsChannelAsync`メソッド`HubConnection`ストリーミング サーバーからクライアント メソッドを呼び出すために使用します。 メソッド名のハブおよびハブ メソッドで定義されている引数を渡す`StreamAsChannelAsync`します。 ジェネリック パラメーター`StreamAsChannelAsync<T>`ストリーミング メソッドによって返されるオブジェクトの種類を指定します。 A`ChannelReader<T>`ストリーム呼び出しから返され、クライアントにストリームを表します。

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

### <a name="client-to-server-streaming"></a>クライアントとサーバーのストリーミング

.NET クライアントからサーバーへのクライアント ストリーミング ハブ メソッドを呼び出すための 2 つの方法はあります。 渡したりすることができます、`IAsyncEnumerable<T>`または`ChannelReader`への引数として`SendAsync`、 `InvokeAsync`、または`StreamAsChannelAsync`のハブ メソッドが呼び出されるとは異なりますが、します。

データが書き込まれるたびに、`IAsyncEnumerable`または`ChannelWriter`オブジェクト、サーバー上のハブ メソッドは、クライアントからのデータの新しい項目を受信します。

使用する場合、`IAsyncEnumerable`オブジェクト、ストリームが終了の項目を返すメソッドの後、ストリームを終了します。

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

使用する場合や、 `ChannelWriter`、チャネルを完了する`channel.Writer.Complete()`:

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a>JavaScript クライアント

### <a name="server-to-client-streaming"></a>サーバーからクライアントのストリーミング

JavaScript クライアントでは、ハブでのサーバーからクライアントにストリーミング メソッドを呼び出す`connection.stream`します。 `stream`メソッドは 2 つの引数を受け取ります。

* ハブ メソッドの名前。 次の例ではハブのメソッド名は`Counter`します。
* ハブ メソッドで定義されている引数。 次の例では、引数はストリーム項目を受信して、ストリームの項目間の遅延の数をカウントします。

`connection.stream` 返します、`IStreamResult`が含まれています、`subscribe`メソッド。 渡す、`IStreamSubscriber`に`subscribe`設定と、 `next`、 `error`、および`complete`から通知を受信するコールバック、`stream`呼び出し。

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

クライアントからストリームを終了するには、呼び出し、`dispose`メソッドを`ISubscription`から返される、`subscribe`メソッド。 このメソッドを呼び出すことにより、`CancellationToken`いずれかを指定した場合、ハブ メソッドのパラメーター。

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

クライアントからストリームを終了するには、呼び出し、`dispose`メソッドを`ISubscription`から返される、`subscribe`メソッド。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>クライアントとサーバーのストリーミング

JavaScript クライアントでは、ハブのクライアントとサーバーのストリーミング メソッドを呼び出すを渡すことによって、`Subject`への引数として`send`、 `invoke`、または`stream`のハブ メソッドが呼び出されるとは異なりますが、します。 `Subject`は次のようなクラス、`Subject`します。 たとえば、RxJS に使える、[サブジェクト](https://rxjs-dev.firebaseapp.com/api/index/class/Subject)ライブラリからのクラス。

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

呼び出す`subject.next(item)`で項目が、項目をストリームに書き込みますおよびハブ メソッドは、サーバー上の項目を受信します。

ストリームを終了するには、呼び出す`subject.complete()`します。

## <a name="java-client"></a>Java クライアント

### <a name="server-to-client-streaming"></a>サーバーからクライアントのストリーミング

SignalR の Java クライアントを使用して、`stream`ストリーミング メソッドを呼び出すメソッド。 `stream` 次の 3 つ以上の引数を受け取ります。

* ストリームの項目の予測される型。
* ハブ メソッドの名前。
* ハブ メソッドで定義されている引数。

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

`stream`メソッド`HubConnection`ストリーム項目の種類の観測可能なオブジェクトを返します。 監視可能な型の`subscribe`メソッドは、where `onNext`、`onError`と`onCompleted`ハンドラーが定義されています。

::: moniker-end

## <a name="additional-resources"></a>その他の技術情報

* [ハブ](xref:signalr/hubs)
* [.NET クライアント](xref:signalr/dotnet-client)
* [JavaScript クライアント](xref:signalr/javascript-client)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)
