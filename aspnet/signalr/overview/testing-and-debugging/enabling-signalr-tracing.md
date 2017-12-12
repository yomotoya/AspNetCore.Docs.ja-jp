---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: "SignalR のトレースを有効化 |Microsoft ドキュメント"
author: tfitzmac
description: "このドキュメントでは、有効にし、SignalR のサーバーとクライアントのトレースを構成する方法について説明します。 トレースでは、イベントに関する診断情報を表示することができます。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 2f01ab5d66e44cd82634f1b3df1ca6c78b7fd9d5
ms.sourcegitcommit: c07fb5cb5df0a12f9fe6735fcbc90964608fa687
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2017
---
<a name="enabling-signalr-tracing"></a>SignalR のトレースを有効にします。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このドキュメントでは、有効にし、SignalR のサーバーとクライアントのトレースを構成する方法について説明します。 トレースでは、SignalR アプリケーションでのイベントに関する診断情報を表示することができます。
> 
> このトピックは、Patrick Fletcher によって最初書き込まれました。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET Framework 4.5
> - SignalR バージョン 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


トレースを有効にすると、SignalR アプリケーションは、イベントのログ エントリを作成します。 イベントは、クライアントとサーバーの両方からログに記録します。 サーバー ログの接続、スケール アウト プロバイダー、およびメッセージ バスのイベントをトレースしています。 クライアント ログの接続イベントをトレースしています。 SignalR 2.1 以降では、クライアント上のトレースは、ハブの呼び出しのメッセージの完全な内容を記録します。

## <a name="contents"></a>目次

- [サーバーのトレースを有効にします。](#server)

    - [サーバー イベントをテキスト ファイルにログ記録](#server_text)
    - [サーバー イベントのイベント ログにログ記録](#server_eventlog)
- [.NET クライアント (Windows デスクトップ アプリ) でのトレースを有効にします。](#net_client)

    - [コンソールにデスクトップ クライアント イベント ログの記録](#desktop_console)
    - [テキスト ファイルをデスクトップ クライアント イベント ログの記録](#desktop_text)
- [Windows Phone 8 クライアントでトレースを有効にします。](#phone)

    - [UI に Windows Phone クライアント イベント ログの記録](#phone_ui)
    - [デバッグ コンソールに Windows Phone クライアント イベント ログの記録](#phone_debug)
- [JavaScript クライアントでトレースを有効にします。](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>サーバーのトレースを有効にします。

アプリケーションの構成ファイル (App.config または Web.config によっては、プロジェクトの種類のいずれかです。) 内でサーバーのトレースを有効にします。ログに記録するイベントのカテゴリを指定します。 構成ファイルにも指定する、イベントをテキスト ファイル、Windows イベント ログまたはの実装を使用してカスタムのログに記録するかどうか[TraceListener](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener(v=vs.110).aspx)です。

Server イベント カテゴリには、メッセージの次のようながあります。

| ソース | メッセージ |
| --- | --- |
| SignalR.SqlMessageBus | SQL Message Bus スケール アウト プロバイダーの設定、データベースの操作、error、およびタイムアウト イベント |
| SignalR.ServiceBusMessageBus | Service bus スケール アウト プロバイダー トピック作成し、サブスクリプション、error、およびメッセージングのイベント |
| SignalR.RedisMessageBus | Redis スケール アウト プロバイダーの接続、切断、およびエラー イベント |
| SignalR.ScaleoutMessageBus | スケール アウト メッセージングのイベント |
| SignalR.Transports.WebSocketTransport | WebSocket トランスポートの接続、切断、メッセージング、およびエラー イベント |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents トランスポート接続、切断、メッセージング、およびエラー イベント |
| SignalR.Transports.ForeverFrameTransport | ForeverFrame トランスポートの接続、切断、メッセージング、およびエラー イベント |
| SignalR.Transports.LongPollingTransport | LongPolling トランスポートの接続、切断、メッセージング、およびエラー イベント |
| SignalR.Transports.TransportHeartBeat | トランスポートの接続、切断、keepalive イベント |
| SignalR.ReflectedHubDescriptorProvider | ハブ検出イベント |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>サーバー イベントをテキスト ファイルにログ記録

次のコードは、イベントの各カテゴリのトレースを有効にする方法を示しています。 このサンプルでは、テキスト ファイルにイベントを記録するアプリケーションを構成します。

**トレースを有効にするための XML サーバー コード**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

上記のコードで、`SignalRSwitch`エントリを指定します、 [TraceLevel](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelevel(v=vs.110).aspx)指定したログに送信されるイベントのために使用します。 設定されているこの場合、`Verbose`つまり、すべてのデバッグおよびトレースのメッセージ ログに記録されます。

次の出力からのエントリを示しています、`transports.log.txt`上記の構成ファイルを使用してアプリケーションのファイルです。 表示、新しい接続、削除された接続、およびトランスポート ハートビート イベント。

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>サーバー イベントのイベント ログにログ記録

テキスト ファイルではなく、イベント ログにイベントをログにエントリの値を変更、`sharedListeners`ノード。 次のコードでは、サーバー イベントをイベント ログに記録する方法を示します。

**イベント ログにイベントのログの XML サーバー コード**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

イベントは、アプリケーション ログに記録され、、次のように、イベント ビューアーを使用。

![SignalR のログを表示するイベント ビューアー](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> イベント ログを使用する場合は、設定、 **TraceLevel**に**エラー**管理可能なメッセージの数を保持します。

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>.NET クライアント (Windows デスクトップ アプリ) でのトレースを有効にします。

.NET クライアントをコンソールでは、テキスト ファイル、またはの実装を使用してカスタムのログにイベントがログ記録できます[TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx)です。

.NET クライアントでのログ記録を有効にする設定の接続の`TraceLevel`プロパティを[TraceLevels](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx)値、および`TraceWriter`プロパティを有効な[TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx)インスタンス。

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>コンソールにデスクトップ クライアント イベント ログの記録

次の c# コードは、.NET クライアント コンソールにイベントを記録する方法を示しています。

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>テキスト ファイルをデスクトップ クライアント イベント ログの記録

次の c# コードは、テキスト ファイルに、.NET クライアントでイベントを記録する方法を示しています。

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

次の出力からのエントリを示しています、`ClientLog.txt`上記の構成ファイルを使用してアプリケーションのファイルです。 表示、サーバーに接続するクライアントとクライアント メソッドを呼び出し、ハブと呼ばれる`addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Windows Phone 8 クライアントでトレースを有効にします。

SignalR アプリケーションを Windows Phone アプリのデスクトップ アプリケーションの場合と同じ .NET クライアントを使用して、 [Console.Out](https://msdn.microsoft.com/en-us/library/system.console.out(v=vs.110).aspx)でファイルを作成して[StreamWriter](https://msdn.microsoft.com/en-us/library/system.io.streamwriter(v=vs.110).aspx)は使用できません。 カスタム実装を作成する必要が代わりに、 [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx)トレースします。 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>UI に Windows Phone クライアント イベント ログの記録

[SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip)トレースに出力する Windows Phone のサンプルが含まれています、 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)カスタムを使用して[TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx)と呼ばれる実装`TextBlockWriter`です。 このクラスは含まれて、 **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples**プロジェクト。 インスタンスを作成するときに`TextBlockWriter`、現在の渡す[SynchronizationContext](https://msdn.microsoft.com/en-us/library/system.threading.synchronizationcontext(v=vs.110).aspx)、および[StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)が作成する場所、 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)トレースに使用するには出力:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

トレース出力は、新規に書き込まれます[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)で作成した、 [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)が渡されました。

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>デバッグ コンソールに Windows Phone クライアント イベント ログの記録

UI ではなく、デバッグ コンソールに出力を送信するには、実装を作成[TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx)デバッグ ウィンドウに書き込みの接続に割り当てる[では](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx)プロパティ。

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

トレース情報は、Visual Studio でのデバッグ ウィンドウに書き込まれます。

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>JavaScript クライアントでトレースを有効にします。

接続でクライアント側のログ記録を有効にするには設定、`logging`を呼び出す前に、接続オブジェクトのプロパティを`start`接続を確立する方法です。

**(生成されたプロキシ) を使用して、ブラウザーのコンソールへのトレースを有効にするためのクライアントの JavaScript コード**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**(、生成されたプロキシなし)、ブラウザーのコンソールへのトレースを有効にするためのクライアントの JavaScript コード**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

トレースを有効にすると、JavaScript クライアントは、ブラウザーのコンソールにイベントを記録します。 ブラウザーのコンソールにアクセスするを参照してください。[監視トランスポート](../getting-started/introduction-to-signalr.md#MonitoringTransports)です。

次のスクリーン ショットは、トレースを有効に SignalR JavaScript クライアントを示します。 ブラウザーのコンソールで、接続とハブ呼び出しイベントを示します。

![ブラウザーのコンソール内の SignalR トレース イベント](enabling-signalr-tracing/_static/image4.png)
