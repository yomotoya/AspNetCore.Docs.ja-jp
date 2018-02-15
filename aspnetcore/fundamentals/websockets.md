---
title: "ASP.NET Core での Websocket のサポート"
author: tdykstra
description: "ASP.NET Core で Websocket の使用を開始する方法を説明します。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>ASP.NET Core での WebSockets の概要

作成者: [Tom Dykstra](https://github.com/tdykstra) および [Andrew Stanton-Nurse](https://github.com/anurse)

この記事では、ASP.NET Core で Websocket の使用を開始する方法について説明します。 [WebSocket](https://wikipedia.org/wiki/WebSocket) は TCP 接続を使用した双方向の永続的通信チャネルを有効にするプロトコルです。 これは、チャット、株価情報、ゲームなどのアプリケーション、また Web アプリケーションでリアルタイム機能が必要とされる場所で使用されます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。 詳細については、「[次の手順](#next-steps)」のセクションを参照してください。


## <a name="prerequisites"></a>必須コンポーネント

* ASP.NET Core 1.1 (1.0 では動作しません)
* ASP.NET Core を実行できる次の任意の OS:
  
  * Windows 7 / Windows Server 2008 以降
  * Linux
  * macOS

* **例外**: アプリが IIS または WebListener を含む Windows 上で実行される場合、次を使用する必要があります。

  * Windows 8 / Windows Server 2012 以降
  * IIS 8 / IIS 8 Express
  * IIS で WebSocket を有効にする必要があります

* サポートされているブラウザーについては、http://caniuse.com/#feat=websockets を参照してください。

## <a name="when-to-use-it"></a>使用する状況

Websocket は、ソケット接続を直接使用する必要がある場合に使用します。 たとえば、リアルタイムのゲームで最高のパフォーマンスが必要な場合などです。

リアルタイムの機能には、[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) の方がアプリケーション モデルとしてはより充実していますが、これは ASP.NET でのみ実行できるものであり、ASP.NET Core では実行できません。 SignalR の Core バージョンは現在開発中です。進捗を確認するには、「[GitHub repository for SignalR Core](https://github.com/aspnet/SignalR)」 (SignalR Core の GitHub リポジトリ) を参照してください。

SignalR Core を待つことができない場合は、そのまま Websocket を使用できます。 ただし、次などの SignalR が提供する機能を開発する必要がある場合があります。

* 他の転送方法への自動フォールバックが使用された、より多くのブラウザ バージョンのサポート。
* 切断された場合の自動接続。
* サーバー上のクライアント、またはこの逆の呼び出しメソッドのサポート。
* 複数のサーバーへのスケーリングのサポート。

## <a name="how-to-use-it"></a>使用方法

* [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) パッケージをインストールします。
* ミドルウェアを構成します。
* WebSocket の要求を受け入れます。
* メッセージを送受信します。

### <a name="configure-the-middleware"></a>ミドルウェアの構成

`Startup` クラスの `Configure` メソッドに、Websocket ミドルウェアを追加します。

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

次の設定を構成できます。

* `KeepAliveInterval`: プロキシの接続の維持を保証する、クライアントに "ping" フレームを送信する頻度。
* `ReceiveBufferSize`: データの受信に使用されるバッファーのサイズ。 これは、上級ユーザーのみが、データのサイズに応じたパフォーマンス調整のために変更する必要があります。

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>WebSocket の要求の受け入れ

以降の要求ライフサイクルのどこかで (たとえば、以降の `Configure` メソッドまたは MVC アクションで)、それが WebSocket 要求であるかを確認し、WebSocket 要求を受け入れます。

この例は、以降の `Configure` メソッドから抜粋したものです。

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

WebSocket 要求はどの URL からも受け取る場合がありますが、このサンプル コードでは `/ws` の要求のみを受け取ります。

### <a name="send-and-receive-messages"></a>メッセージの送受信

`AcceptWebSocketAsync` メソッドは、TCP 接続を WebSocket 接続にアップグレードし、[WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) オブジェクトを渡します。 メッセージの送受信に、WebSocket オブジェクトを使用します。

前に示した、WebSocket 要求を受け入れるコードが、`WebSocket` オブジェクトを `Echo` メソッドに渡します (ここでは、`Echo` メソッドです)。 このコードは、メッセージを受信し、同じメッセージをすぐに送信します。 クライアントが接続を閉じるまで、これを継続しループ内に存在し続けます。 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

このループを開始する前に、WebSocket を受け入れた場合、ミドルウェア パイプラインは終了します。  ソケットを閉じると、パイプラインはアンワインドされます。 つまり、WebSocket 受け入れると、たとえば、MVC アクションにヒットしたときのように、要求がパイプラインで前進しなくなります。  ただし、このループを終了し、ソケットを閉じると、要求はパイプラインを遡って進みます。

## <a name="next-steps"></a>次の手順

この記事に付属している[サンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)は、単純なエコー アプリケーションです。 これには、WebSocket 接続を作成する Web ページがあり、サーバーが受け取るすべてのメッセージを単純にクライアントに再送信します。 これをコマンド プロンプトから実行し (IIS Express を使用した Visual Studio からは実行するように設定されていません)、http://localhost:5000 に移動します。 Web ページの左上に、接続の状態が示されます。

![Web ページの初期状態](websockets/_static/start.png)

**[接続]** を選択し、表示されている URL に WebSocket 要求を送信します。  テスト メッセージを入力し、**[送信]** を選択します。 完了したら、**[Close Socket]\(ソケットを閉じる\)** を選択します。 **[Communication Log]\(コミュニケーション ログ\)** セクションに、発生した各オープン、送信、クローズのアクションが表示されます。

![Web ページの初期状態](websockets/_static/end.png)
