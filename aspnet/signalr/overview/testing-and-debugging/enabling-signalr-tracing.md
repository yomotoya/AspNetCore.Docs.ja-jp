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
<a name="enabling-signalr-tracing"></a><span data-ttu-id="f8316-104">SignalR のトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f8316-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="f8316-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f8316-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f8316-106">このドキュメントでは、有効にし、SignalR のサーバーとクライアントのトレースを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f8316-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="f8316-107">トレースでは、SignalR アプリケーションでのイベントに関する診断情報を表示することができます。</span><span class="sxs-lookup"><span data-stu-id="f8316-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
> 
> <span data-ttu-id="f8316-108">このトピックは、Patrick Fletcher によって最初書き込まれました。</span><span class="sxs-lookup"><span data-stu-id="f8316-108">This topic was originally written by Patrick Fletcher.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f8316-109">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="f8316-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="f8316-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f8316-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="f8316-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="f8316-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="f8316-112">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="f8316-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="f8316-113">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="f8316-113">Questions and comments</span></span>
> 
> <span data-ttu-id="f8316-114">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="f8316-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f8316-115">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="f8316-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="f8316-116">トレースを有効にすると、SignalR アプリケーションは、イベントのログ エントリを作成します。</span><span class="sxs-lookup"><span data-stu-id="f8316-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="f8316-117">イベントは、クライアントとサーバーの両方からログに記録します。</span><span class="sxs-lookup"><span data-stu-id="f8316-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="f8316-118">サーバー ログの接続、スケール アウト プロバイダー、およびメッセージ バスのイベントをトレースしています。</span><span class="sxs-lookup"><span data-stu-id="f8316-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="f8316-119">クライアント ログの接続イベントをトレースしています。</span><span class="sxs-lookup"><span data-stu-id="f8316-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="f8316-120">SignalR 2.1 以降では、クライアント上のトレースは、ハブの呼び出しのメッセージの完全な内容を記録します。</span><span class="sxs-lookup"><span data-stu-id="f8316-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="f8316-121">目次</span><span class="sxs-lookup"><span data-stu-id="f8316-121">Contents</span></span>

- [<span data-ttu-id="f8316-122">サーバーのトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f8316-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="f8316-123">サーバー イベントをテキスト ファイルにログ記録</span><span class="sxs-lookup"><span data-stu-id="f8316-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="f8316-124">サーバー イベントのイベント ログにログ記録</span><span class="sxs-lookup"><span data-stu-id="f8316-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="f8316-125">.NET クライアント (Windows デスクトップ アプリ) でのトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f8316-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="f8316-126">コンソールにデスクトップ クライアント イベント ログの記録</span><span class="sxs-lookup"><span data-stu-id="f8316-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="f8316-127">テキスト ファイルをデスクトップ クライアント イベント ログの記録</span><span class="sxs-lookup"><span data-stu-id="f8316-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="f8316-128">Windows Phone 8 クライアントでトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f8316-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="f8316-129">UI に Windows Phone クライアント イベント ログの記録</span><span class="sxs-lookup"><span data-stu-id="f8316-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="f8316-130">デバッグ コンソールに Windows Phone クライアント イベント ログの記録</span><span class="sxs-lookup"><span data-stu-id="f8316-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="f8316-131">JavaScript クライアントでトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f8316-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="f8316-132">サーバーのトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f8316-132">Enabling tracing on the server</span></span>

<span data-ttu-id="f8316-133">アプリケーションの構成ファイル (App.config または Web.config によっては、プロジェクトの種類のいずれかです。) 内でサーバーのトレースを有効にします。ログに記録するイベントのカテゴリを指定します。</span><span class="sxs-lookup"><span data-stu-id="f8316-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="f8316-134">構成ファイルにも指定する、イベントをテキスト ファイル、Windows イベント ログまたはの実装を使用してカスタムのログに記録するかどうか[TraceListener](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener(v=vs.110).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="f8316-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="f8316-135">Server イベント カテゴリには、メッセージの次のようながあります。</span><span class="sxs-lookup"><span data-stu-id="f8316-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="f8316-136">ソース</span><span class="sxs-lookup"><span data-stu-id="f8316-136">Source</span></span> | <span data-ttu-id="f8316-137">メッセージ</span><span class="sxs-lookup"><span data-stu-id="f8316-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="f8316-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="f8316-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="f8316-139">SQL Message Bus スケール アウト プロバイダーの設定、データベースの操作、error、およびタイムアウト イベント</span><span class="sxs-lookup"><span data-stu-id="f8316-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="f8316-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="f8316-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="f8316-141">Service bus スケール アウト プロバイダー トピック作成し、サブスクリプション、error、およびメッセージングのイベント</span><span class="sxs-lookup"><span data-stu-id="f8316-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="f8316-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="f8316-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="f8316-143">Redis スケール アウト プロバイダーの接続、切断、およびエラー イベント</span><span class="sxs-lookup"><span data-stu-id="f8316-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="f8316-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="f8316-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="f8316-145">スケール アウト メッセージングのイベント</span><span class="sxs-lookup"><span data-stu-id="f8316-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="f8316-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="f8316-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="f8316-147">WebSocket トランスポートの接続、切断、メッセージング、およびエラー イベント</span><span class="sxs-lookup"><span data-stu-id="f8316-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="f8316-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="f8316-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="f8316-149">ServerSentEvents トランスポート接続、切断、メッセージング、およびエラー イベント</span><span class="sxs-lookup"><span data-stu-id="f8316-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="f8316-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="f8316-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="f8316-151">ForeverFrame トランスポートの接続、切断、メッセージング、およびエラー イベント</span><span class="sxs-lookup"><span data-stu-id="f8316-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="f8316-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="f8316-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="f8316-153">LongPolling トランスポートの接続、切断、メッセージング、およびエラー イベント</span><span class="sxs-lookup"><span data-stu-id="f8316-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="f8316-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="f8316-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="f8316-155">トランスポートの接続、切断、keepalive イベント</span><span class="sxs-lookup"><span data-stu-id="f8316-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="f8316-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="f8316-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="f8316-157">ハブ検出イベント</span><span class="sxs-lookup"><span data-stu-id="f8316-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="f8316-158">サーバー イベントをテキスト ファイルにログ記録</span><span class="sxs-lookup"><span data-stu-id="f8316-158">Logging server events to text files</span></span>

<span data-ttu-id="f8316-159">次のコードは、イベントの各カテゴリのトレースを有効にする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="f8316-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="f8316-160">このサンプルでは、テキスト ファイルにイベントを記録するアプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="f8316-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="f8316-161">**トレースを有効にするための XML サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="f8316-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="f8316-162">上記のコードで、`SignalRSwitch`エントリを指定します、 [TraceLevel](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelevel(v=vs.110).aspx)指定したログに送信されるイベントのために使用します。</span><span class="sxs-lookup"><span data-stu-id="f8316-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="f8316-163">設定されているこの場合、`Verbose`つまり、すべてのデバッグおよびトレースのメッセージ ログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="f8316-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="f8316-164">次の出力からのエントリを示しています、`transports.log.txt`上記の構成ファイルを使用してアプリケーションのファイルです。</span><span class="sxs-lookup"><span data-stu-id="f8316-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="f8316-165">表示、新しい接続、削除された接続、およびトランスポート ハートビート イベント。</span><span class="sxs-lookup"><span data-stu-id="f8316-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="f8316-166">サーバー イベントのイベント ログにログ記録</span><span class="sxs-lookup"><span data-stu-id="f8316-166">Logging server events to the event log</span></span>

<span data-ttu-id="f8316-167">テキスト ファイルではなく、イベント ログにイベントをログにエントリの値を変更、`sharedListeners`ノード。</span><span class="sxs-lookup"><span data-stu-id="f8316-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="f8316-168">次のコードでは、サーバー イベントをイベント ログに記録する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f8316-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="f8316-169">**イベント ログにイベントのログの XML サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="f8316-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="f8316-170">イベントは、アプリケーション ログに記録され、、次のように、イベント ビューアーを使用。</span><span class="sxs-lookup"><span data-stu-id="f8316-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![SignalR のログを表示するイベント ビューアー](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="f8316-172">イベント ログを使用する場合は、設定、 **TraceLevel**に**エラー**管理可能なメッセージの数を保持します。</span><span class="sxs-lookup"><span data-stu-id="f8316-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="f8316-173">.NET クライアント (Windows デスクトップ アプリ) でのトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f8316-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="f8316-174">.NET クライアントをコンソールでは、テキスト ファイル、またはの実装を使用してカスタムのログにイベントがログ記録できます[TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="f8316-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="f8316-175">.NET クライアントでのログ記録を有効にする設定の接続の`TraceLevel`プロパティを[TraceLevels](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx)値、および`TraceWriter`プロパティを有効な[TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx)インスタンス。</span><span class="sxs-lookup"><span data-stu-id="f8316-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="f8316-176">コンソールにデスクトップ クライアント イベント ログの記録</span><span class="sxs-lookup"><span data-stu-id="f8316-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="f8316-177">次の c# コードは、.NET クライアント コンソールにイベントを記録する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="f8316-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="f8316-178">テキスト ファイルをデスクトップ クライアント イベント ログの記録</span><span class="sxs-lookup"><span data-stu-id="f8316-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="f8316-179">次の c# コードは、テキスト ファイルに、.NET クライアントでイベントを記録する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="f8316-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="f8316-180">次の出力からのエントリを示しています、`ClientLog.txt`上記の構成ファイルを使用してアプリケーションのファイルです。</span><span class="sxs-lookup"><span data-stu-id="f8316-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="f8316-181">表示、サーバーに接続するクライアントとクライアント メソッドを呼び出し、ハブと呼ばれる`addMessage`:</span><span class="sxs-lookup"><span data-stu-id="f8316-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="f8316-182">Windows Phone 8 クライアントでトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f8316-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="f8316-183">SignalR アプリケーションを Windows Phone アプリのデスクトップ アプリケーションの場合と同じ .NET クライアントを使用して、 [Console.Out](https://msdn.microsoft.com/en-us/library/system.console.out(v=vs.110).aspx)でファイルを作成して[StreamWriter](https://msdn.microsoft.com/en-us/library/system.io.streamwriter(v=vs.110).aspx)は使用できません。</span><span class="sxs-lookup"><span data-stu-id="f8316-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/en-us/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/en-us/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="f8316-184">カスタム実装を作成する必要が代わりに、 [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx)トレースします。</span><span class="sxs-lookup"><span data-stu-id="f8316-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span> 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="f8316-185">UI に Windows Phone クライアント イベント ログの記録</span><span class="sxs-lookup"><span data-stu-id="f8316-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="f8316-186">[SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip)トレースに出力する Windows Phone のサンプルが含まれています、 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)カスタムを使用して[TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx)と呼ばれる実装`TextBlockWriter`です。</span><span class="sxs-lookup"><span data-stu-id="f8316-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="f8316-187">このクラスは含まれて、 **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="f8316-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="f8316-188">インスタンスを作成するときに`TextBlockWriter`、現在の渡す[SynchronizationContext](https://msdn.microsoft.com/en-us/library/system.threading.synchronizationcontext(v=vs.110).aspx)、および[StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)が作成する場所、 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)トレースに使用するには出力:</span><span class="sxs-lookup"><span data-stu-id="f8316-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/en-us/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="f8316-189">トレース出力は、新規に書き込まれます[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)で作成した、 [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)が渡されました。</span><span class="sxs-lookup"><span data-stu-id="f8316-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="f8316-190">デバッグ コンソールに Windows Phone クライアント イベント ログの記録</span><span class="sxs-lookup"><span data-stu-id="f8316-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="f8316-191">UI ではなく、デバッグ コンソールに出力を送信するには、実装を作成[TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx)デバッグ ウィンドウに書き込みの接続に割り当てる[では](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="f8316-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="f8316-192">トレース情報は、Visual Studio でのデバッグ ウィンドウに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="f8316-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="f8316-193">JavaScript クライアントでトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f8316-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="f8316-194">接続でクライアント側のログ記録を有効にするには設定、`logging`を呼び出す前に、接続オブジェクトのプロパティを`start`接続を確立する方法です。</span><span class="sxs-lookup"><span data-stu-id="f8316-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="f8316-195">**(生成されたプロキシ) を使用して、ブラウザーのコンソールへのトレースを有効にするためのクライアントの JavaScript コード**</span><span class="sxs-lookup"><span data-stu-id="f8316-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="f8316-196">**(、生成されたプロキシなし)、ブラウザーのコンソールへのトレースを有効にするためのクライアントの JavaScript コード**</span><span class="sxs-lookup"><span data-stu-id="f8316-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="f8316-197">トレースを有効にすると、JavaScript クライアントは、ブラウザーのコンソールにイベントを記録します。</span><span class="sxs-lookup"><span data-stu-id="f8316-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="f8316-198">ブラウザーのコンソールにアクセスするを参照してください。[監視トランスポート](../getting-started/introduction-to-signalr.md#MonitoringTransports)です。</span><span class="sxs-lookup"><span data-stu-id="f8316-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="f8316-199">次のスクリーン ショットは、トレースを有効に SignalR JavaScript クライアントを示します。</span><span class="sxs-lookup"><span data-stu-id="f8316-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="f8316-200">ブラウザーのコンソールで、接続とハブ呼び出しイベントを示します。</span><span class="sxs-lookup"><span data-stu-id="f8316-200">It shows connection and hub invocation events in the browser console:</span></span>

![ブラウザーのコンソール内の SignalR トレース イベント](enabling-signalr-tracing/_static/image4.png)
