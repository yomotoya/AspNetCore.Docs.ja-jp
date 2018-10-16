---
title: ASP.NET Core SignalR では、ストリーミングを使用して、
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 3ae9b83d60019eaa3196f35645bf9b4b03f6d8c6
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325641"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>ASP.NET Core SignalR では、ストリーミングを使用して、

によって[真紀 Brennan](https://github.com/BrennanConroy)

ASP.NET Core SignalR は、サーバーのメソッドの戻り値のストリーミングをサポートします。 これは、データの断片化はどこ時間の経過と共にシナリオに便利です。 戻り値は、クライアントにストリーミングされるときに各フラグメントが送信されるクライアントになったとすぐに使用可能になるすべてのデータを待機するのではなく、使用可能な。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="set-up-the-hub"></a>ハブを設定します。

ハブ メソッドはストリーミングのハブ メソッドを自動的になりますが返されるときに、`ChannelReader<T>`または`Task<ChannelReader<T>>`します。 クライアントへのデータのストリーミングの基礎を示すサンプルを次に示します。 オブジェクトが書き込まれるたびに、`ChannelReader`そのオブジェクトがすぐに、クライアントに送信します。 最後に、`ChannelReader`ストリームが閉じていることをクライアントに指示するのには完了します。

> [!NOTE]
> 書き込み、`ChannelReader`バック グラウンド スレッドと返された場合に、`ChannelReader`できるだけ早くします。 その他のハブ呼び出しまでブロックされます、`ChannelReader`が返されます。

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a>.NET クライアント

`StreamAsChannelAsync`メソッド`HubConnection`ストリーミング メソッドを呼び出すために使用します。 ハブ メソッドの名前、およびハブ メソッドで定義されている引数を渡す`StreamAsChannelAsync`します。 ジェネリック パラメーター`StreamAsChannelAsync<T>`ストリーミング メソッドによって返されるオブジェクトの種類を指定します。 A`ChannelReader<T>`ストリームの呼び出しから返され、クライアントにストリームを表します。 データの読み取り、一般的なパターンは、経由でループする`WaitToReadAsync`を呼び出すと`TryRead`データが使用可能な場合。 サーバーによって、ストリームが閉じているかに渡されたキャンセル トークンと、ループが終了`StreamAsChannelAsync`は取り消されます。

```csharp
var channel = await hubConnection.StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

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

## <a name="javascript-client"></a>JavaScript クライアント

JavaScript クライアントは、ハブにストリーミング メソッドを使用して呼び出す`connection.stream`します。 `stream`メソッドは 2 つの引数を受け取ります。

* ハブ メソッドの名前。 次の例ではハブのメソッド名は`Counter`します。
* ハブ メソッドで定義されている引数。 次の例では、引数は: ストリーム項目を受信して、ストリームの項目間の遅延の数をカウントします。

`connection.stream` 返します、`IStreamResult`が含まれています、`subscribe`メソッド。 渡す、`IStreamSubscriber`に`subscribe`設定と、 `next`、 `error`、および`complete`からの通知を取得するコールバック、`stream`呼び出し。

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

クライアントからストリームを終了するには、呼び出し、`dispose`メソッドを`ISubscription`から返される、`subscribe`メソッド。

## <a name="related-resources"></a>関連資料

* [ハブ](xref:signalr/hubs)
* [.NET クライアント](xref:signalr/dotnet-client)
* [JavaScript クライアント](xref:signalr/javascript-client)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)
