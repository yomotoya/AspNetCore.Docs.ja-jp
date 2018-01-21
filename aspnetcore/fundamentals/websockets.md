---
title: "ASP.NET Core での Websocket のサポート"
author: tdykstra
description: "ASP.NET Core で Websocket を開始する方法を説明します。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 46c1f42b925a43df470d7491a1e259ab51ea5f50
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>Websocket の ASP.NET Core の概要

によって[Tom Dykstra](https://github.com/tdykstra)と[Andrew スタントン-看護師](https://github.com/anurse)

この記事では、ASP.NET Core で Websocket を開始する方法について説明します。 [WebSocket](https://wikipedia.org/wiki/WebSocket) は TCP 接続を使用した双方向の永続的通信チャネルを有効にするプロトコルです。 チャット、株価情報、ゲームなどのアプリケーションの使用は、web アプリケーションでのリアルタイムの機能を使用する任意の場所。

[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))。 参照してください、[次の手順](#next-steps)詳細についてはします。


## <a name="prerequisites"></a>必須コンポーネント

* ASP.NET Core 1.1 (は実行されません 1.0)
* ASP.NET Core 上で実行される OS:
  
  * Windows 7/Windows Server 2008 以降
  * Linux
  * macOS

* **例外**: 場合は、IIS での windows アプリの実行中または WebListener で使用する必要があります。

  * Windows 8/Windows Server 2012 以降
  * IIS 8 IIS 8 Express/
  * IIS で WebSocket を有効にする必要があります。

* サポートされるブラウザー http://caniuse.com/#feat=websockets を参照してください。

## <a name="when-to-use-it"></a>使用する状況

ソケット接続で直接作業する必要がある場合は、Websocket を使用します。 たとえば、リアルタイムのゲームを最高のパフォーマンスを必要があります。

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr)充実したは、リアルタイムの機能はアプリケーション モデルは、ASP.NET、ASP.NET Core いない上でのみ実行します。 SignalR の中核となるバージョンがあるが開発です。を実行する、進行状況を参照してください、 [SignalR Core の GitHub リポジトリ](https://github.com/aspnet/SignalR)です。

SignalR Core を待機しない場合は、できる Websocket を直接使用するようになりました。 ただし、SignalR 提供するなどの機能を開発する必要があります。

* 広い範囲の自動フォールバックを代替トランスポート メソッドを使用して、ブラウザーのバージョンをサポートします。
* 接続のドロップと自動再接続します。
* サーバー上またはその逆のメソッドを呼び出すクライアントをサポートします。
* 複数のサーバーに合わせたスケーリングのサポート。

## <a name="how-to-use-it"></a>使用方法

* インストール、 [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/)パッケージです。
* ミドルウェアを構成します。
* WebSocket 要求を受け入れます。
* メッセージを送受信します。

### <a name="configure-the-middleware"></a>ミドルウェアを構成します。

内で Websocket ミドルウェアを追加、`Configure`のメソッド、`Startup`クラスです。

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

次の設定を構成できます。

* `KeepAliveInterval`-方法プロキシ接続を維持しておくため、クライアントに"ping"フレームを送信する頻度。
* `ReceiveBufferSize`-データの受信に使用するバッファーのサイズ。 上級ユーザーだけは、そのデータのサイズに基づくパフォーマンス チューニングのため、これを変更する必要があります。

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>WebSocket 要求を受け入れる

要求のライフ サイクルの後 (後半、`Configure`メソッドまたは例については、MVC アクション内で) かどうかは WebSocket 要求を確認し、WebSocket 要求を受け入れます。

この例は後で、`Configure`メソッドです。

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

WebSocket 要求は、任意の URL でものがありますが、このサンプル コードの要求のみを受け付けます`/ws`です。

### <a name="send-and-receive-messages"></a>メッセージの送受信を行う

`AcceptWebSocketAsync`メソッドは WebSocket 接続への TCP 接続をアップグレードし、使用する、 [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket)オブジェクト。 メッセージを送受信するには、WebSocket オブジェクトを使用します。

WebSocket 要求を受け入れる前に示したコード渡し、`WebSocket`オブジェクトを`Echo`メソッドです。 ここでの、`Echo`メソッドです。 コードでは、メッセージを受信し、すぐに、同じメッセージを返信します。 クライアントが接続を閉じるまでこれを行うループ内に保持します。 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

このループを開始する前に、WebSocket を承諾するとミドルウェア パイプラインを終了します。  ソケットを閉じると、パイプラインをアンワインドします。 つまりと同様、WebSocket を承諾すると、パイプラインに進むと、要求が停止は、たとえば MVC アクションをヒットしたときに。  ただし、このループを終了し、ソケットを閉じると場合、に、パイプラインをバックアップする要求が行われます。

## <a name="next-steps"></a>次の手順

[サンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)に付属しているこの記事では、単純なエコー アプリケーションです。 WebSocket 接続を作成する web ページがあり、サーバーを再クライアントに受信したメッセージ。 (これがいない設定を IIS Express で Visual Studio を実行する)、コマンド プロンプトから実行し、http://localhost:5000 に移動します。 Web ページは、左上にある接続の状態を示しています。

![Web ページの初期状態](websockets/_static/start.png)

選択**接続**表示される URL に WebSocket 要求を送信します。  テスト メッセージを入力し、選択**送信**です。 終了したら、選択**閉じるソケット**です。 **通信ログ**セクションは、各 open、send をレポートでありは閉じたときの操作が行われます。

![Web ページの初期状態](websockets/_static/end.png)
