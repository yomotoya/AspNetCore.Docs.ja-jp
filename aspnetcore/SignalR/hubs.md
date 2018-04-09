---
title: ASP.NET Core SignalR ハブを使用します。
author: rachelappel
description: ASP.NET Core SignalR ハブを使用する方法を説明します。
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 73397ba6951f2752653575920bdf739439874366
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>ASP.NET Core の SignalR でハブを使用します。

によって[Rachel Appel](https://twitter.com/rachelappel)と[Kevin Griffin](https://twitter.com/1kevgriff)

## <a name="what-is-a-signalr-hub"></a>SignalR ハブとは

SignalR ハブ API を使用すると、サーバーから接続しているクライアントのメソッドを呼び出すことができます。 サーバー コードでは、クライアントによって呼び出されるメソッドを定義します。 クライアント コードでは、サーバーから呼び出されるメソッドを定義します。 バック グラウンドでリアルタイムのクライアントとサーバーおよびサーバーからクライアントへの通信を可能にするすべての SignalR によって行われます。

## <a name="configure-signalr-hubs"></a>SignalR ハブを構成します。

SignalR のミドルウェアが一部のサービスでは、呼び出すことによって構成を必要と`services.AddSignalR`です。

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

SignalR の機能を ASP.NET Core アプリケーションを追加する場合は、SignalR のルートをセットアップを呼び出して`app.UseSignalR`で、`Startup.Configure`メソッドです。

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a>作成し、ハブの使用

継承するクラスを宣言することで、ハブの作成`Hub`、しをパブリック メソッドを追加します。 クライアントとして定義されているメソッドを呼び出すことができます`public`です。

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

戻り値の型と複合型アレイなど、任意の c# のメソッドの場合と、パラメーターを指定することができます。 SignalR では、複雑なオブジェクトおよびパラメーターと戻り値の配列の逆シリアル化とシリアル化を処理します。

## <a name="the-clients-object"></a>クライアント オブジェクト

各インスタンス、`Hub`クラスという名前のプロパティには`Clients`サーバーとクライアント間の通信は、次のメンバーを格納しています。

| プロパティ | 説明 |
| ------ | ----------- |
| `All` | 接続されているすべてのクライアントのメソッドを呼び出す |
| `Caller` | ハブ メソッドを呼び出したクライアントのメソッドを呼び出す |
| `Others` | メソッドを呼び出したクライアントを除くすべての接続されているクライアントのメソッドを呼び出す |

さらに、`Hub`クラスには、次のメソッドが含まれています。

| メソッド | 説明 |
| ------ | ----------- |
| `AllExcept` | 指定された接続を除くすべての接続されているクライアントのメソッドを呼び出す |
| `Client` | 特定の接続されているクライアントのメソッドを呼び出す |
| `Clients` | 特定の接続されているクライアントのメソッドを呼び出す |
| `Group` | 指定したグループ内のすべての接続にメッセージを送信します。  |
| `GroupExcept` | 指定した接続を除く、指定したグループ内のすべての接続にメッセージを送信します。 |
| `Groups` | 接続の複数のグループにメッセージを送信します。  |
| `OthersInGroup` | ハブ メソッドを呼び出したクライアントを除く、接続のグループにメッセージを送信します。  |
| `User` | 特定のユーザーに関連付けられているすべての接続にメッセージを送信します。 |
| `Users` | 指定されたユーザーに関連付けられているすべての接続にメッセージを送信します。 |

各プロパティまたは上記のテーブル内のメソッドを持つオブジェクトを返します、`SendAsync`メソッドです。 `SendAsync`メソッドを呼び出すクライアント メソッドのパラメーターと名前を指定することができます。

## <a name="send-messages-to-clients"></a>クライアントにメッセージを送信します。

特定のクライアントへの呼び出しをするためには、プロパティを使用して、`Clients`オブジェクト。 次の例では、次に、`SendMessageToCaller`ハブ メソッドを呼び出した接続にメッセージを送信するメソッドに示します。 `SendMessageToGroups`メソッドに格納されているグループにメッセージを送信する、`List`という`groups`です。

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>接続のイベントの処理

SignalR ハブ API は、提供、`OnConnectedAsync`と`OnDisconnectedAsync`仮想メソッドを管理し、接続を追跡します。 上書き、`OnConnectedAsync`クライアントがグループに追加するなど、ハブに接続するときにアクションを実行する仮想メソッドです。

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a>エラーを処理します。

ハブ メソッドでスローされた例外は、メソッドを呼び出したクライアントに送信されます。 JavaScript クライアントで、`invoke`メソッドを返します、 [JavaScript の約束](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)です。 Promise を使用して、接続されているクライアントがハンドラーでエラーを受信すると`catch`、呼び出され、JavaScript に渡されることが`Error`オブジェクト。

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a>関連資料

[ASP.NET Core SignalR の概要](xref:signalr/introduction)