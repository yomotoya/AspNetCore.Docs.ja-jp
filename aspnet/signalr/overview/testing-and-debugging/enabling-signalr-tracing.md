---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: SignalR トレースを有効にする |Microsoft Docs
author: Rick-Anderson
description: このドキュメントでは、有効にして、SignalR のサーバーとクライアントのトレースを構成する方法について説明します。 トレースでは、イベントに関する診断情報を表示することができます.
ms.author: riande
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 733163699f83cbf68d6d5a27e86d208c3922272c
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287664"
---
<a name="enabling-signalr-tracing"></a>SignalR トレースを有効にします。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> このドキュメントでは、有効にして、SignalR のサーバーとクライアントのトレースを構成する方法について説明します。 トレースでは、SignalR アプリケーションでのイベントに関する診断情報を表示することができます。
>
> このトピックでは、Patrick Fletcher によって作成当初されました。
>
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET Framework 4.5
> - SignalR 2 のバージョン
>
>
>
> ## <a name="questions-and-comments"></a>意見やご質問
>
> このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。 チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。


トレースが有効にすると、SignalR アプリケーションは、イベントのログ エントリを作成します。 クライアントとサーバーの両方からイベントを記録することができます。 サーバー ログの接続、スケール アウトのプロバイダー、およびメッセージ バスのイベントをトレースしています。 クライアント ログの接続イベントをトレースしています。 SignalR 2.1 以降では、クライアントのトレースはハブ呼び出しメッセージの完全な内容を記録します。

## <a name="contents"></a>目次

- [サーバーのトレースを有効にします。](#server)

    - [サーバー イベントをテキスト ファイルにログ記録](#server_text)
    - [サーバー イベントのイベント ログにログ記録](#server_eventlog)
- [.NET クライアント (Windows デスクトップ アプリ) でのトレースを有効にします。](#net_client)

    - [デスクトップ クライアントのイベントをコンソールにログ記録](#desktop_console)
    - [デスクトップ クライアントのイベントをテキスト ファイルにログ記録](#desktop_text)
- [Windows Phone 8 クライアントでトレースを有効にします。](#phone)

    - [UI を Windows Phone クライアント イベントのログ記録](#phone_ui)
    - [デバッグ コンソールに Windows Phone クライアント イベントのログ記録](#phone_debug)
- [JavaScript クライアント内でトレースを有効にします。](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>サーバーのトレースを有効にします。

アプリケーションの構成ファイル (App.config または Web.config プロジェクトの種類に応じてのいずれかです。) 内でサーバーのトレースを有効にします。ログに記録するイベントのカテゴリを指定するとします。 構成ファイルで指定することも、イベントをテキスト ファイルや、Windows イベント ログの実装を使用して、カスタム ログに記録するかどうか[TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx)します。

サーバーのイベント カテゴリには、次のようなメッセージがあります。

| ソース | メッセージ |
| --- | --- |
| SignalR.SqlMessageBus | SQL Message Bus スケール アウトのプロバイダーのセットアップ、データベースの操作、エラー、およびタイムアウト イベント |
| SignalR.ServiceBusMessageBus | Service bus スケール アウト プロバイダーのトピックで作成し、サブスクリプション、エラー、およびメッセージングのイベント |
| SignalR.RedisMessageBus | Redis スケール アウト プロバイダーの接続、切断、およびエラー イベント |
| SignalR.ScaleoutMessageBus | スケール アウト メッセージングのイベント |
| SignalR.Transports.WebSocketTransport | WebSocket トランスポートの接続、切断、メッセージング、およびエラー イベント |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents トランスポートの接続、切断、メッセージング、およびエラー イベント |
| SignalR.Transports.ForeverFrameTransport | ForeverFrame トランスポートの接続、切断、メッセージング、およびエラー イベント |
| SignalR.Transports.LongPollingTransport | LongPolling トランスポートの接続、切断、メッセージング、およびエラー イベント |
| SignalR.Transports.TransportHeartBeat | トランスポートの接続、切断、キープア ライブ イベント |
| SignalR.ReflectedHubDescriptorProvider | ハブの検出イベント |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>サーバー イベントをテキスト ファイルにログ記録

次のコードでは、各カテゴリのイベントのトレースを有効にする方法を示します。 このサンプルでは、テキスト ファイルにイベントを記録するアプリケーションを構成します。

**トレースを有効にするための XML サーバー コード**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

上記のコードで、`SignalRSwitch`エントリを指定します、 [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx)指定されたログに送信されるイベントのために使用します。 この場合設定されて`Verbose`つまりすべてデバッグ出力およびメッセージをトレース ログに記録されます。

次の出力からのエントリを示しています、`transports.log.txt`上記の構成ファイルを使用してアプリケーションのファイル。 表示、新しい接続、削除された接続では、およびトランスポート ハートビート イベント。

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>サーバー イベントのイベント ログにログ記録

テキスト ファイルではなく、イベント ログにイベントを記録するには、内のエントリの値を変更、`sharedListeners`ノード。 次のコードでは、サーバー イベントをイベント ログに記録する方法を示します。

**イベント ログにイベントのログの XML サーバー コード**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

イベントは、アプリケーション ログに記録され、、次に示すように、イベント ビューアーを利用。

![SignalR のログを表示するイベント ビューアー](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> イベント ログを使用する場合は、設定、 **TraceLevel**に**エラー**管理可能なメッセージの数を保持します。

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>.NET クライアント (Windows デスクトップ アプリ) でのトレースを有効にします。

.NET クライアントはコンソールにテキスト ファイル、またはの実装を使用して、カスタム ログにイベントを記録できます[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)します。

.NET クライアントでのログ記録を有効にする設定、接続の`TraceLevel`プロパティを[TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx)値、および`TraceWriter`プロパティを有効な[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)インスタンス。

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>デスクトップ クライアントのイベントをコンソールにログ記録

次の c# コードでは、コンソールに .NET クライアントでイベントを記録する方法を示します。

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>デスクトップ クライアントのイベントをテキスト ファイルにログ記録

次の c# コードでは、テキスト ファイルに .NET クライアントでイベントを記録する方法を示します。

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

次の出力からのエントリを示しています、`ClientLog.txt`上記の構成ファイルを使用してアプリケーションのファイル。 表示、サーバーに接続するクライアントとクライアント メソッドを呼び出し、ハブと呼ばれる`addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Windows Phone 8 クライアントでトレースを有効にします。

Windows Phone アプリ用の SignalR アプリケーションとデスクトップ アプリは、同じ .NET クライアントを使用してが[Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx)ファイルへの書き込みと[StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx)は使用できません。 代わりに、カスタムの実装を作成する必要があります[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)トレースします。

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>UI を Windows Phone クライアント イベントのログ記録

[SignalR コードベース](https://github.com/SignalR/SignalR/archive/master.zip)トレースに出力する Windows Phone のサンプルが含まれています、 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)カスタムを使用して[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)という実装`TextBlockWriter`します。 このクラスが記載されて、 **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples**プロジェクト。 インスタンスを作成するときに`TextBlockWriter`、現在の渡す[SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)と[StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)は作成、 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)トレースに使用するには出力:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

新しいトレース出力を記述し、 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)で作成した、 [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)で渡されます。

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>デバッグ コンソールに Windows Phone クライアント イベントのログ記録

実装を作成、UI ではなく、デバッグ コンソールに出力を送信する[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)をデバッグ ウィンドウに書き込みの接続に割り当てる[TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx)プロパティ。

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

トレース情報は、Visual Studio でのデバッグ ウィンドウに書き込まれます。

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>JavaScript クライアント内でトレースを有効にします。

接続でクライアント側のログ記録を有効にするには設定、`logging`を呼び出す前に、接続オブジェクトのプロパティ、`start`メソッドは、接続を確立します。

**(生成されたプロキシ) を使用して、ブラウザーのコンソールへのトレースを有効にするためのクライアントの JavaScript コード**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**(なし、生成されたプロキシ)、ブラウザーのコンソールへのトレースを有効にするためのクライアントの JavaScript コード**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

トレースが有効にすると、JavaScript クライアントは、ブラウザーのコンソールにイベントを記録します。 ブラウザーのコンソールにアクセスするを参照してください。[監視トランスポート](../getting-started/introduction-to-signalr.md#MonitoringTransports)します。

次のスクリーン ショットは、トレースを有効に SignalR JavaScript クライアントを示します。 ブラウザーのコンソールで、接続とハブ呼び出しイベントを示します。

![ブラウザーのコンソールで SignalR トレース イベント](enabling-signalr-tracing/_static/image4.png)
