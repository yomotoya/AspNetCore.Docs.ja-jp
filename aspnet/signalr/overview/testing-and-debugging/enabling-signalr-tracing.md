---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: SignalR トレースを有効にする |Microsoft Docs
author: bradygaster
description: このドキュメントでは、有効にして、SignalR のサーバーとクライアントのトレースを構成する方法について説明します。 トレースでは、イベントに関する診断情報を表示することができます.
ms.author: bradyg
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: c6515e3653798ef50e2d2dcb7354ffed407a190e
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836468"
---
<a name="enabling-signalr-tracing"></a><span data-ttu-id="84c0d-104">SignalR トレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="84c0d-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="84c0d-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="84c0d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="84c0d-106">このドキュメントでは、有効にして、SignalR のサーバーとクライアントのトレースを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="84c0d-107">トレースでは、SignalR アプリケーションでのイベントに関する診断情報を表示することができます。</span><span class="sxs-lookup"><span data-stu-id="84c0d-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
>
> <span data-ttu-id="84c0d-108">このトピックでは、Patrick Fletcher によって作成当初されました。</span><span class="sxs-lookup"><span data-stu-id="84c0d-108">This topic was originally written by Patrick Fletcher.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="84c0d-109">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="84c0d-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="84c0d-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="84c0d-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="84c0d-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="84c0d-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="84c0d-112">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="84c0d-112">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="84c0d-113">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="84c0d-113">Questions and comments</span></span>
>
> <span data-ttu-id="84c0d-114">このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。</span><span class="sxs-lookup"><span data-stu-id="84c0d-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="84c0d-115">チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。</span><span class="sxs-lookup"><span data-stu-id="84c0d-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="84c0d-116">トレースが有効にすると、SignalR アプリケーションは、イベントのログ エントリを作成します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="84c0d-117">クライアントとサーバーの両方からイベントを記録することができます。</span><span class="sxs-lookup"><span data-stu-id="84c0d-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="84c0d-118">サーバー ログの接続、スケール アウトのプロバイダー、およびメッセージ バスのイベントをトレースしています。</span><span class="sxs-lookup"><span data-stu-id="84c0d-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="84c0d-119">クライアント ログの接続イベントをトレースしています。</span><span class="sxs-lookup"><span data-stu-id="84c0d-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="84c0d-120">SignalR 2.1 以降では、クライアントのトレースはハブ呼び出しメッセージの完全な内容を記録します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="84c0d-121">目次</span><span class="sxs-lookup"><span data-stu-id="84c0d-121">Contents</span></span>

- [<span data-ttu-id="84c0d-122">サーバーのトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="84c0d-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="84c0d-123">サーバー イベントをテキスト ファイルにログ記録</span><span class="sxs-lookup"><span data-stu-id="84c0d-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="84c0d-124">サーバー イベントのイベント ログにログ記録</span><span class="sxs-lookup"><span data-stu-id="84c0d-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="84c0d-125">.NET クライアント (Windows デスクトップ アプリ) でのトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="84c0d-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="84c0d-126">デスクトップ クライアントのイベントをコンソールにログ記録</span><span class="sxs-lookup"><span data-stu-id="84c0d-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="84c0d-127">デスクトップ クライアントのイベントをテキスト ファイルにログ記録</span><span class="sxs-lookup"><span data-stu-id="84c0d-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="84c0d-128">Windows Phone 8 クライアントでトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="84c0d-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="84c0d-129">UI を Windows Phone クライアント イベントのログ記録</span><span class="sxs-lookup"><span data-stu-id="84c0d-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="84c0d-130">デバッグ コンソールに Windows Phone クライアント イベントのログ記録</span><span class="sxs-lookup"><span data-stu-id="84c0d-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="84c0d-131">JavaScript クライアント内でトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="84c0d-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="84c0d-132">サーバーのトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="84c0d-132">Enabling tracing on the server</span></span>

<span data-ttu-id="84c0d-133">アプリケーションの構成ファイル (App.config または Web.config プロジェクトの種類に応じてのいずれかです。) 内でサーバーのトレースを有効にします。ログに記録するイベントのカテゴリを指定するとします。</span><span class="sxs-lookup"><span data-stu-id="84c0d-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="84c0d-134">構成ファイルで指定することも、イベントをテキスト ファイルや、Windows イベント ログの実装を使用して、カスタム ログに記録するかどうか[TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="84c0d-135">サーバーのイベント カテゴリには、次のようなメッセージがあります。</span><span class="sxs-lookup"><span data-stu-id="84c0d-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="84c0d-136">ソース</span><span class="sxs-lookup"><span data-stu-id="84c0d-136">Source</span></span> | <span data-ttu-id="84c0d-137">メッセージ</span><span class="sxs-lookup"><span data-stu-id="84c0d-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="84c0d-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="84c0d-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="84c0d-139">SQL Message Bus スケール アウトのプロバイダーのセットアップ、データベースの操作、エラー、およびタイムアウト イベント</span><span class="sxs-lookup"><span data-stu-id="84c0d-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="84c0d-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="84c0d-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="84c0d-141">Service bus スケール アウト プロバイダーのトピックで作成し、サブスクリプション、エラー、およびメッセージングのイベント</span><span class="sxs-lookup"><span data-stu-id="84c0d-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="84c0d-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="84c0d-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="84c0d-143">Redis スケール アウト プロバイダーの接続、切断、およびエラー イベント</span><span class="sxs-lookup"><span data-stu-id="84c0d-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="84c0d-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="84c0d-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="84c0d-145">スケール アウト メッセージングのイベント</span><span class="sxs-lookup"><span data-stu-id="84c0d-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="84c0d-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="84c0d-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="84c0d-147">WebSocket トランスポートの接続、切断、メッセージング、およびエラー イベント</span><span class="sxs-lookup"><span data-stu-id="84c0d-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="84c0d-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="84c0d-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="84c0d-149">ServerSentEvents トランスポートの接続、切断、メッセージング、およびエラー イベント</span><span class="sxs-lookup"><span data-stu-id="84c0d-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="84c0d-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="84c0d-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="84c0d-151">ForeverFrame トランスポートの接続、切断、メッセージング、およびエラー イベント</span><span class="sxs-lookup"><span data-stu-id="84c0d-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="84c0d-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="84c0d-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="84c0d-153">LongPolling トランスポートの接続、切断、メッセージング、およびエラー イベント</span><span class="sxs-lookup"><span data-stu-id="84c0d-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="84c0d-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="84c0d-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="84c0d-155">トランスポートの接続、切断、キープア ライブ イベント</span><span class="sxs-lookup"><span data-stu-id="84c0d-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="84c0d-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="84c0d-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="84c0d-157">ハブの検出イベント</span><span class="sxs-lookup"><span data-stu-id="84c0d-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="84c0d-158">サーバー イベントをテキスト ファイルにログ記録</span><span class="sxs-lookup"><span data-stu-id="84c0d-158">Logging server events to text files</span></span>

<span data-ttu-id="84c0d-159">次のコードでは、各カテゴリのイベントのトレースを有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="84c0d-160">このサンプルでは、テキスト ファイルにイベントを記録するアプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="84c0d-161">**トレースを有効にするための XML サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="84c0d-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="84c0d-162">上記のコードで、`SignalRSwitch`エントリを指定します、 [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx)指定されたログに送信されるイベントのために使用します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="84c0d-163">この場合設定されて`Verbose`つまりすべてデバッグ出力およびメッセージをトレース ログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="84c0d-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="84c0d-164">次の出力からのエントリを示しています、`transports.log.txt`上記の構成ファイルを使用してアプリケーションのファイル。</span><span class="sxs-lookup"><span data-stu-id="84c0d-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="84c0d-165">表示、新しい接続、削除された接続では、およびトランスポート ハートビート イベント。</span><span class="sxs-lookup"><span data-stu-id="84c0d-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="84c0d-166">サーバー イベントのイベント ログにログ記録</span><span class="sxs-lookup"><span data-stu-id="84c0d-166">Logging server events to the event log</span></span>

<span data-ttu-id="84c0d-167">テキスト ファイルではなく、イベント ログにイベントを記録するには、内のエントリの値を変更、`sharedListeners`ノード。</span><span class="sxs-lookup"><span data-stu-id="84c0d-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="84c0d-168">次のコードでは、サーバー イベントをイベント ログに記録する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="84c0d-169">**イベント ログにイベントのログの XML サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="84c0d-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="84c0d-170">イベントは、アプリケーション ログに記録され、、次に示すように、イベント ビューアーを利用。</span><span class="sxs-lookup"><span data-stu-id="84c0d-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![SignalR のログを表示するイベント ビューアー](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="84c0d-172">イベント ログを使用する場合は、設定、 **TraceLevel**に**エラー**管理可能なメッセージの数を保持します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="84c0d-173">.NET クライアント (Windows デスクトップ アプリ) でのトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="84c0d-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="84c0d-174">.NET クライアントはコンソールにテキスト ファイル、またはの実装を使用して、カスタム ログにイベントを記録できます[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="84c0d-175">.NET クライアントでのログ記録を有効にする設定、接続の`TraceLevel`プロパティを[TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx)値、および`TraceWriter`プロパティを有効な[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)インスタンス。</span><span class="sxs-lookup"><span data-stu-id="84c0d-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="84c0d-176">デスクトップ クライアントのイベントをコンソールにログ記録</span><span class="sxs-lookup"><span data-stu-id="84c0d-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="84c0d-177">次の c# コードでは、コンソールに .NET クライアントでイベントを記録する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="84c0d-178">デスクトップ クライアントのイベントをテキスト ファイルにログ記録</span><span class="sxs-lookup"><span data-stu-id="84c0d-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="84c0d-179">次の c# コードでは、テキスト ファイルに .NET クライアントでイベントを記録する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="84c0d-180">次の出力からのエントリを示しています、`ClientLog.txt`上記の構成ファイルを使用してアプリケーションのファイル。</span><span class="sxs-lookup"><span data-stu-id="84c0d-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="84c0d-181">表示、サーバーに接続するクライアントとクライアント メソッドを呼び出し、ハブと呼ばれる`addMessage`:</span><span class="sxs-lookup"><span data-stu-id="84c0d-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="84c0d-182">Windows Phone 8 クライアントでトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="84c0d-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="84c0d-183">Windows Phone アプリ用の SignalR アプリケーションとデスクトップ アプリは、同じ .NET クライアントを使用してが[Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx)ファイルへの書き込みと[StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx)は使用できません。</span><span class="sxs-lookup"><span data-stu-id="84c0d-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="84c0d-184">代わりに、カスタムの実装を作成する必要があります[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)トレースします。</span><span class="sxs-lookup"><span data-stu-id="84c0d-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span>

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="84c0d-185">UI を Windows Phone クライアント イベントのログ記録</span><span class="sxs-lookup"><span data-stu-id="84c0d-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="84c0d-186">[SignalR コードベース](https://github.com/SignalR/SignalR/archive/master.zip)トレースに出力する Windows Phone のサンプルが含まれています、 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)カスタムを使用して[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)という実装`TextBlockWriter`します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="84c0d-187">このクラスが記載されて、 **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="84c0d-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="84c0d-188">インスタンスを作成するときに`TextBlockWriter`、現在の渡す[SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)と[StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)は作成、 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)トレースに使用するには出力:</span><span class="sxs-lookup"><span data-stu-id="84c0d-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="84c0d-189">新しいトレース出力を記述し、 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)で作成した、 [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)で渡されます。</span><span class="sxs-lookup"><span data-stu-id="84c0d-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="84c0d-190">デバッグ コンソールに Windows Phone クライアント イベントのログ記録</span><span class="sxs-lookup"><span data-stu-id="84c0d-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="84c0d-191">実装を作成、UI ではなく、デバッグ コンソールに出力を送信する[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)をデバッグ ウィンドウに書き込みの接続に割り当てる[TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="84c0d-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="84c0d-192">トレース情報は、Visual Studio でのデバッグ ウィンドウに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="84c0d-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="84c0d-193">JavaScript クライアント内でトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="84c0d-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="84c0d-194">接続でクライアント側のログ記録を有効にするには設定、`logging`を呼び出す前に、接続オブジェクトのプロパティ、`start`メソッドは、接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="84c0d-195">**(生成されたプロキシ) を使用して、ブラウザーのコンソールへのトレースを有効にするためのクライアントの JavaScript コード**</span><span class="sxs-lookup"><span data-stu-id="84c0d-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="84c0d-196">**(なし、生成されたプロキシ)、ブラウザーのコンソールへのトレースを有効にするためのクライアントの JavaScript コード**</span><span class="sxs-lookup"><span data-stu-id="84c0d-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="84c0d-197">トレースが有効にすると、JavaScript クライアントは、ブラウザーのコンソールにイベントを記録します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="84c0d-198">ブラウザーのコンソールにアクセスするを参照してください。[監視トランスポート](../getting-started/introduction-to-signalr.md#MonitoringTransports)します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="84c0d-199">次のスクリーン ショットは、トレースを有効に SignalR JavaScript クライアントを示します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="84c0d-200">ブラウザーのコンソールで、接続とハブ呼び出しイベントを示します。</span><span class="sxs-lookup"><span data-stu-id="84c0d-200">It shows connection and hub invocation events in the browser console:</span></span>

![ブラウザーのコンソールで SignalR トレース イベント](enabling-signalr-tracing/_static/image4.png)
