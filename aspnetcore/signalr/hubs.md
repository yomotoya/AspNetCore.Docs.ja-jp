---
title: ASP.NET Core SignalR のハブを使用します。
author: tdykstra
description: ASP.NET Core SignalR のハブを使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: be39666373e2b099054bb71f4a7fcf17aeb9a01c
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095282"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>ASP.NET core SignalR のハブの使用

によって[Rachel Appel](https://twitter.com/rachelappel)と[Kevin Griffin](https://twitter.com/1kevgriff)

[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>SignalR ハブとは

SignalR ハブの API を使用すると、サーバーから接続しているクライアントのメソッドを呼び出すことができます。 サーバー コードでは、クライアントによって呼び出されるメソッドを定義します。 クライアント コードでは、サーバーから呼び出されるメソッドを定義します。 SignalR は、バック グラウンドでクライアントとサーバーとサーバーからクライアントのリアルタイム通信を可能にするすべての処理されます。

## <a name="configure-signalr-hubs"></a>SignalR ハブを構成します。

SignalR のミドルウェアがサービスによっては、呼び出すことによって構成されている必要があります`services.AddSignalR`します。

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

SignalR の機能を ASP.NET Core アプリを追加する場合は、SignalR のルートをセットアップを呼び出して`app.UseSignalR`で、`Startup.Configure`メソッド。

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>作成し、ハブの使用

継承するクラスを宣言することで、ハブの作成`Hub`、しをパブリック メソッドを追加します。 クライアントとして定義されているメソッドを呼び出すことができます`public`します。

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

戻り値の型と複合型と配列を含む c# メソッドの場合と、パラメーターを指定できます。 SignalR では、複雑なオブジェクトと、パラメーターと戻り値の配列の逆シリアル化とシリアル化を処理します。

## <a name="the-clients-object"></a>クライアント オブジェクト

各インスタンス、`Hub`クラスという名前のプロパティには`Clients`サーバーとクライアント間の通信は、次のメンバーを格納しています。

| プロパティ | 説明 |
| ------ | ----------- |
| `All` | 接続されているすべてのクライアントのメソッドを呼び出す |
| `Caller` | ハブ メソッドを呼び出したクライアントのメソッドを呼び出す |
| `Others` | メソッドを呼び出したクライアントを除くすべての接続されているクライアントのメソッドを呼び出す |


さらに、`Hub.Clients`次のメソッドが含まれています。

| メソッド | 説明 |
| ------ | ----------- |
| `AllExcept` | 指定された接続を除くすべての接続されているクライアントのメソッドを呼び出す |
| `Client` | 特定の接続されているクライアントのメソッドを呼び出す |
| `Clients` | 接続されているクライアントが特定のメソッドを呼び出す |
| `Group` | 指定したグループのすべての接続にメソッドを呼び出す  |
| `GroupExcept` | 指定された接続を除く、指定されたグループのすべての接続にメソッドを呼び出す |
| `Groups` | 接続の複数のグループにメソッドを呼び出します  |
| `OthersInGroup` | ハブ メソッドを呼び出したクライアントを除く、接続のグループにメソッドを呼び出します  |
| `User` | 特定のユーザーに関連付けられているすべての接続にメソッドを呼び出します |
| `Users` | 指定されたユーザーに関連付けられているすべての接続にメソッドを呼び出します |

プロパティまたはメソッドでは、上記のテーブルごとにオブジェクトを返します、`SendAsync`メソッド。 `SendAsync`メソッドを呼び出すクライアント メソッドのパラメーターと名前を指定することができます。

## <a name="send-messages-to-clients"></a>クライアントにメッセージを送信します。

特定のクライアントへの呼び出しをするためには、プロパティを使用して、`Clients`オブジェクト。 次の例では、`SendMessageToCaller`ハブ メソッドを呼び出した接続にメッセージを送信するメソッドに示します。 `SendMessageToGroups`メソッドに格納されているグループにメッセージを送信する、`List`という`groups`します。

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>接続のイベントの処理

SignalR ハブ API は、提供、`OnConnectedAsync`と`OnDisconnectedAsync`接続を管理するための仮想メソッド。 上書き、`OnConnectedAsync`クライアントがグループに追加するなど、ハブに接続するときにアクションを実行する仮想メソッド。

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>エラー処理

ハブ メソッドでスローされた例外は、メソッドを呼び出したクライアントに送信されます。 JavaScript クライアントで、`invoke`メソッドを返します。 を[JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)します。 クライアントが、ハンドラーでエラーを受信すると promise を使用して、接続されている`catch`、呼び出されるは、JavaScript として渡される`Error`オブジェクト。

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>関連資料

* [ASP.NET Core SignalR の概要](xref:signalr/introduction)
* [JavaScript クライアント](xref:signalr/javascript-client)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)
