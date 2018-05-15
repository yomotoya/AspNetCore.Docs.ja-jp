---
title: ASP.NET Core での Websocket のサポート
author: tdykstra
description: ASP.NET Core で Websocket の使用を開始する方法を説明します。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: e744ab5b81ff85f48edb012a86b55003cc74929c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="websockets-support-in-aspnet-core"></a>ASP.NET Core での Websocket のサポート

作成者: [Tom Dykstra](https://github.com/tdykstra) および [Andrew Stanton-Nurse](https://github.com/anurse)

この記事では、ASP.NET Core で Websocket の使用を開始する方法について説明します。 [WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) は、TCP 接続を使用した双方向の永続的通信チャネルを有効にするプロトコルです。 このプロトコルは、チャット、ダッシュボード、ゲーム アプリなど、高速かつリアルタイムのコミュニケーションを活用するアプリで使用されます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。 詳細については、「[次の手順](#next-steps)」のセクションを参照してください。

## <a name="prerequisites"></a>必須コンポーネント

* ASP.NET Core 1.1 以降
* ASP.NET Core をサポートする任意の OS:
  
  * Windows 7 / Windows Server 2008 以降
  * Linux
  * macOS
  
* アプリが IIS を含む Windows 上で実行されている場合:

  * Windows 8 / Windows Server 2012 以降
  * IIS 8 / IIS 8 Express
  * WebSockets は IIS で有効にされる必要があります (「[IIS/IIS Express のサポート](#iisiis-express-support)」セクションを参照してください。)
  
* アプリが [HTTP.sys](xref:fundamentals/servers/httpsys) で実行されている場合:

  * Windows 8 / Windows Server 2012 以降

* サポートされているブラウザーについては、https://caniuse.com/#feat=websockets を参照してください。

## <a name="when-to-use-websockets"></a>WebSockets を使用する場合

Websocket を使用して、ソケット接続を直接使用します。 たとえば、リアルタイムのゲームで最高のパフォーマンスが必要な場合に WebSockets を使用します。

リアルタイムの機能には、[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) の方がアプリ モデルとして充実していますが、これは ASP.NET 4.x でのみ実行できるものであり、ASP.NET Core では実行できません。 SignalR の ASP.NET Core バージョンは、ASP.NET Core 2.1 でリリースされる予定です。 「[ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288)」 (ASP.NET Core 2.1 の高度なプラン) を参照してください。

SignalR Core がリリースされるまで、WebSockets を使用することができます。 ただし、SignalR が提供する機能は、開発者が提供またはサポートする必要があります。 例:

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
* `ReceiveBufferSize`: データの受信に使用されるバッファーのサイズ。 これは、上級ユーザーが、データのサイズに応じたパフォーマンス調整のために変更する必要がある場合があります。

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>WebSocket の要求の受け入れ

以降の要求ライフサイクルのどこかで (たとえば、以降の `Configure` メソッドまたは MVC アクションで)、それが WebSocket 要求であるかを確認し、WebSocket 要求を受け入れます。

次の例は、以降の `Configure` メソッドから抜粋したものです。

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

WebSocket 要求はどの URL からも受け取る場合がありますが、このサンプル コードでは `/ws` の要求のみを受け取ります。

### <a name="send-and-receive-messages"></a>メッセージの送受信

`AcceptWebSocketAsync` メソッドは、TCP 接続を WebSocket 接続にアップグレードし、[WebSocket](/dotnet/core/api/system.net.websockets.websocket) オブジェクトを提供します。 メッセージの送受信に、`WebSocket` オブジェクトを使用します。

前に示した、WebSocket 要求を受け入れるコードが、`WebSocket` オブジェクトを `Echo` メソッドに渡します。 このコードは、メッセージを受信し、同じメッセージをすぐに送信します。 クライアントが接続を閉じるまで、メッセージがループで送受信されます。

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

このループを開始する前に、WebSocket 接続を受け入れた場合、ミドルウェア パイプラインは終了します。 ソケットを閉じると、パイプラインはアンワインドされます。 つまり、WebSocket を受け入れると、要求はパイプラインでの先への移動を中止します。 ループを終了し、ソケットを閉じた場合、要求はパイプラインのバックアップを続けます。

## <a name="iisiis-express-support"></a>IIS/IIS Express のサポート

IIS/IIS Express 8 以降を含む、Windows Server 2012 以降および Windows 8 以降では、WebSocket プロトコルをサポートします。

Windows Server 2012 以降で WebSocket プロトコルのサポートを有効にするには

1. **[管理]** メニューから**役割と機能の追加**ウィザードを使用するか、**サーバー マネージャー**にあるリンクを使用します。
1. **[役割ベースまたは機能ベースのインストール]** を選択します。 **[次へ]** を選択します。
1. 適切なサーバーを選択します (既定では、ローカル サーバーが選択されます)。 **[次へ]** を選択します。
1. **[役割]** ツリーで **[Web サーバー (IIS)]** を展開し、**[Web サーバー]**、**[アプリケーション開発]** の順に展開します。
1. **[WebSocket プロトコル]** を選択します。 **[次へ]** を選択します。
1. 追加機能が不要な場合は、**[次へ]** を選択します。
1. **[インストール]** を選択します。
1. インストールが完了したら、**[閉じる]** を選択してウィザードを終了します。

Windows 8 以降で WebSocket プロトコルのサポートを有効にするには

1. **[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。
1. 次のノード: **[インターネット インフォメーション サービス]** > **[World Wide Web サービス]** > **[アプリケーション開発機能]** を開きます。
1. **[WebSocket プロトコル]** 機能を選択します。 **[OK]** を選択します。

**node.js で socket.io を使用する場合に WebSocket を無効にする**

[Node.js](https://nodejs.org/) の [socket.io](https://socket.io/) で WebSocket サポートを使用する場合、`webSocket` 要素を使用する既定の WebSocket モジュールを *web.config* または *applicationHost.config* で無効にします。この手順が実行されない場合、IIS WebSocket モジュールは Node.js とアプリ以外の WebSocket コミュニケーションを処理しようとします。

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>次の手順

この記事に添えられている[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)は、エコー アプリです。 これには、WebSocket 接続を作成する Web ページがあり、サーバーが受け取るすべてのメッセージをクライアントに再送信します。 コマンド プロンプトからアプリを実行し (IIS Express を使用した Visual Studio からは実行するように設定されていません)、http://localhost:5000 に移動します。 Web ページの左上に、接続の状態が示されます。

![Web ページの初期状態](websockets/_static/start.png)

**[接続]** を選択し、表示されている URL に WebSocket 要求を送信します。 テスト メッセージを入力し、**[送信]** を選択します。 完了したら、**[Close Socket]\(ソケットを閉じる\)** を選択します。 **[Communication Log]\(コミュニケーション ログ\)** セクションに、発生した各オープン、送信、クローズのアクションが表示されます。

![Web ページの初期状態](websockets/_static/end.png)
