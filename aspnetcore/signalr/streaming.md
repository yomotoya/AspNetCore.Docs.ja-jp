---
title: ASP.NET Core SignalR では、ストリーミングを使用します。
author: rachelappel
description: ''
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/streaming
ms.openlocfilehash: e8b32d5f25ae3bf8555fd2375c0fbb99a6f8bd53
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358432"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>ASP.NET Core SignalR では、ストリーミングを使用します。

によって[真紀 Brennan](https://github.com/BrennanConroy)

ASP.NET Core SignalR には、サーバーのメソッドの戻り値のストリーミングがサポートしています。 これは、データの断片化はどこ時間の経過と共にシナリオに便利です。 戻り値がクライアントにストリーム配信されるときに各フラグメントは、クライアントに送信なります、すぐに使用可能になるすべてのデータの待機中ではなく、使用可能です。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="set-up-the-hub"></a>ハブを設定します。

ハブ メソッドはストリーミングのハブ メソッドを自動的に戻ったとき、`ChannelReader<T>`または`Task<ChannelReader<T>>`です。 クライアントへのデータをストリーミングの基礎を示すサンプルを次に示します。 オブジェクトを記述するときに、`ChannelReader`そのオブジェクトがクライアントに直ちに送信されます。 最後に、`ChannelReader`ストリームが閉じていることをクライアントに指示するのには完了します。

> [!NOTE]
> 書き込み、`ChannelReader`バック グラウンド スレッドと戻り値の`ChannelReader`できるだけ早くです。 その他のハブ呼び出しをするまでブロックされます、`ChannelReader`が返されます。

[!code-csharp[Streaming hub method](streaming/sample/hubs/streamhub.cs?range=10-34)]

## <a name="net-client"></a>.NET クライアント

`StreamAsChannelAsync`メソッド`HubConnection`ストリーミング メソッドの呼び出しに使用します。 ハブ メソッドの名前、およびハブ メソッドで定義されている引数を渡す`StreamAsChannelAsync`です。 ジェネリック パラメーターの`StreamAsChannelAsync<T>`ストリーミング メソッドによって返されるオブジェクトの種類を指定します。 A`ChannelReader<T>`ストリーム呼び出しから返され、クライアントにストリームを表します。 データの読み取り、一般的なパターンでは、ループする`WaitToReadAsync`を呼び出すと`TryRead`データを使用する場合。 ストリームが、サーバーによって閉じられてまたはにキャンセル トークンが渡されるときに、ループは終了`StreamAsChannelAsync`は取り消されます。

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

JavaScript クライアントを使用してハブのストリーミング メソッドを呼び出す`connection.stream`です。 `stream`メソッドは 2 つの引数を受け取ります。

* ハブ メソッドの名前。 ハブ メソッドの名前は、次の例では、`Counter`です。
* ハブ メソッドで定義されている引数。 次の例では引数が: ストリーム項目を受信してストリーム アイテム間の遅延の数をカウントします。

`connection.stream` 返します、`IStreamResult`が含まれている、`subscribe`メソッドです。 渡す、`IStreamSubscriber`に`subscribe`設定と、 `next`、 `error`、および`complete`コールバックから通知されるように、`stream`呼び出しです。

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

クライアントの呼び出し元のストリームを終了する、`dispose`メソッドを`ISubscription`から返された、`subscribe`メソッドです。

## <a name="related-resources"></a>関連資料

* [ハブ](xref:signalr/hubs)
* [.NET クライアント](xref:signalr/dotnet-client)
* [JavaScript クライアント](xref:signalr/javascript-client)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)