---
uid: signalr/overview/performance/signalr-performance
title: "SignalR パフォーマンス |Microsoft ドキュメント"
author: pfletcher
description: "SignalR パフォーマンス"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 4468ee8031afccca847db67bd4b5b263f0a2c5ac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="signalr-performance"></a><span data-ttu-id="5191e-103">SignalR パフォーマンス</span><span class="sxs-lookup"><span data-stu-id="5191e-103">SignalR Performance</span></span>
====================
<span data-ttu-id="5191e-104">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5191e-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="5191e-105">このトピックでは、用にデザイン、メジャー、および SignalR アプリケーションのパフォーマンスを向上させる方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="5191e-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5191e-106">このトピックで使用されているソフトウェア バージョン</span><span class="sxs-lookup"><span data-stu-id="5191e-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="5191e-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5191e-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="5191e-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5191e-108">.NET 4.5</span></span>
> - <span data-ttu-id="5191e-109">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="5191e-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5191e-110">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="5191e-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="5191e-111">SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="5191e-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="5191e-112">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="5191e-112">Questions and comments</span></span>
> 
> <span data-ttu-id="5191e-113">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="5191e-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5191e-114">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="5191e-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="5191e-115">SignalR パフォーマンスとスケーリングの最近のプレゼンテーションを参照してください。 [ASP.NET SignalR でリアルタイム Web をスケーリング](https://channel9.msdn.com/Events/Build/2013/3-502)です。</span><span class="sxs-lookup"><span data-stu-id="5191e-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="5191e-116">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="5191e-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="5191e-117">設計に関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="5191e-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="5191e-118">パフォーマンスのため、SignalR のサーバーのチューニング</span><span class="sxs-lookup"><span data-stu-id="5191e-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="5191e-119">パフォーマンスの問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="5191e-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="5191e-120">SignalR パフォーマンス カウンターを使用します。</span><span class="sxs-lookup"><span data-stu-id="5191e-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="5191e-121">他のパフォーマンス カウンターを使用します。</span><span class="sxs-lookup"><span data-stu-id="5191e-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="5191e-122">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="5191e-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="5191e-123">設計に関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="5191e-123">Design considerations</span></span>

<span data-ttu-id="5191e-124">このセクションでは、パフォーマンスの不必要なネットワーク トラフィックを生成することによって障害されていないことを確認する SignalR アプリケーションのデザイン時に実装できるパターンについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5191e-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="5191e-125">メッセージの頻度を調整</span><span class="sxs-lookup"><span data-stu-id="5191e-125">Throttling message frequency</span></span>

<span data-ttu-id="5191e-126">(リアルタイムのゲーム アプリケーション) などの高頻度でメッセージを送信して、アプリケーションであっても、1 秒間に複数のメッセージを送信するほとんどのアプリケーションが不要です。</span><span class="sxs-lookup"><span data-stu-id="5191e-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="5191e-127">各クライアントを生成するトラフィックの量を減らすためには、メッセージ ループを実装できますキューおよび送信により頻繁に固定の率よりもメッセージないこと (つまり、最大メッセージ数は送信毎秒では、その期間中にメッセージがある場合terval 送信する)。</span><span class="sxs-lookup"><span data-stu-id="5191e-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="5191e-128">サンプル アプリケーション (クライアントとサーバーの両方) から特定のレートを調整してメッセージを次を参照してください。 [SignalR と頻度が高いリアルタイム](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)です。</span><span class="sxs-lookup"><span data-stu-id="5191e-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="5191e-129">メッセージのサイズを縮小します。</span><span class="sxs-lookup"><span data-stu-id="5191e-129">Reducing message size</span></span>

<span data-ttu-id="5191e-130">SignalR メッセージのサイズを小さくには、シリアル化されたオブジェクトのサイズを小さきます。</span><span class="sxs-lookup"><span data-stu-id="5191e-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="5191e-131">サーバー コードを送信する必要のないプロパティを格納するオブジェクトを送信する場合は妨げそれらのプロパティを使用してシリアル化される、`JsonIgnore`属性。</span><span class="sxs-lookup"><span data-stu-id="5191e-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="5191e-132">プロパティの名前がメッセージにも格納されています。使用して、プロパティの名前を簡略化、`JsonProperty`属性。</span><span class="sxs-lookup"><span data-stu-id="5191e-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="5191e-133">次のコード サンプルでは、プロパティ名を短縮する方法と、クライアントに送信されないプロパティを除外する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="5191e-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="5191e-134">**クライアントに送信されるデータを除外する JsonIgnore 属性とメッセージ サイズを縮小する JsonProperty 属性を示す .NET サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="5191e-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="5191e-135">読みやすさを保持するために、クライアント コードで保守性、省略形のプロパティ名は、わかりやすいに再割り当て名、メッセージの受信後に/です。</span><span class="sxs-lookup"><span data-stu-id="5191e-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="5191e-136">次のコード サンプルでは、メッセージ コントラクト (マッピング) を定義することで、長いパスワードを簡略化された名前を再マップの方法の 1 つを使用して、`reMap`最適化されたメッセージ クラスに、コントラクトを適用する関数。</span><span class="sxs-lookup"><span data-stu-id="5191e-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="5191e-137">**クライアント側の JavaScript コードを再配置を人間が判読できる名前にプロパティ名を短縮します。**</span><span class="sxs-lookup"><span data-stu-id="5191e-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="5191e-138">名前が同じメソッドを使用して同様に、サーバーにクライアントからのメッセージで短縮されることができます。</span><span class="sxs-lookup"><span data-stu-id="5191e-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="5191e-139">メモリ使用量 (つまり、メッセージで使用するメモリ量) の削減、メッセージのオブジェクトもパフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="5191e-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="5191e-140">たとえば場合のすべての範囲、`int`は必要ありません、`short`または`byte`代わりに使用されることができます。</span><span class="sxs-lookup"><span data-stu-id="5191e-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="5191e-141">メッセージのサイズを小さくする、サーバーのメモリ内メッセージ バス内でメッセージが格納されているためには、サーバー メモリの問題を解決でこともできます。</span><span class="sxs-lookup"><span data-stu-id="5191e-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="5191e-142">パフォーマンスのため、SignalR のサーバーのチューニング</span><span class="sxs-lookup"><span data-stu-id="5191e-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="5191e-143">次の構成設定は、SignalR アプリケーションでパフォーマンスを向上させるため、サーバーのチューニングに使用できます。</span><span class="sxs-lookup"><span data-stu-id="5191e-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="5191e-144">ASP.NET アプリケーションのパフォーマンスを向上させる方法の概要については、次を参照してください。 [ASP.NET のパフォーマンスを向上させる](https://msdn.microsoft.com/library/ff647787.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="5191e-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="5191e-145">**SignalR の構成設定**</span><span class="sxs-lookup"><span data-stu-id="5191e-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="5191e-146">**DefaultMessageBufferSize**: 既定では、SignalR には接続ごとのハブあたりのメモリ内の 1000 メッセージが保持されます。</span><span class="sxs-lookup"><span data-stu-id="5191e-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="5191e-147">サイズの大きいメッセージを使用している場合は、メモリの問題はこの値を減らすことによって軽減されることができますを作成この可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5191e-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="5191e-148">この設定で設定することができます、`Application_Start`や ASP.NET アプリケーションでイベント ハンドラー、`Configuration`自己ホスト型アプリケーションでの OWIN スタートアップ クラスのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="5191e-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="5191e-149">次の例では、サーバーのメモリ使用量を削減するために、アプリケーションのメモリ使用量を減らすためにこの値を小さく方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5191e-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="5191e-150">**既定のメッセージ バッファーのサイズを減らすため、Startup.cs の .NET サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="5191e-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="5191e-151">**IIS の構成設定**</span><span class="sxs-lookup"><span data-stu-id="5191e-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="5191e-152">**アプリケーションごとの最大同時要求**: IIS の同時実行の数を増やすと、要求で要求処理の使用可能なサーバー リソースは増加します。</span><span class="sxs-lookup"><span data-stu-id="5191e-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="5191e-153">既定値は 5000 です。この設定を増やすには、するには、管理者特権のコマンド プロンプトで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="5191e-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="5191e-154">**ApplicationPool QueueLength**: これは、Http.sys がアプリケーション プールのキューを要求の最大数。</span><span class="sxs-lookup"><span data-stu-id="5191e-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="5191e-155">キューがいっぱいになった場合、新しい要求は 503「サービスは使用できません」応答を受信します。</span><span class="sxs-lookup"><span data-stu-id="5191e-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="5191e-156">既定値は 1000 です。</span><span class="sxs-lookup"><span data-stu-id="5191e-156">The default value is 1000.</span></span>

    <span data-ttu-id="5191e-157">アプリケーションをホストするアプリケーション プールでワーカー プロセスのキューの長さを短くと、メモリ リソースを節約します。</span><span class="sxs-lookup"><span data-stu-id="5191e-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="5191e-158">詳細については、次を参照してください。[管理、調整、およびアプリケーション プールを構成する](https://technet.microsoft.com/library/cc745955.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="5191e-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="5191e-159">**ASP.NET 構成の設定**</span><span class="sxs-lookup"><span data-stu-id="5191e-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="5191e-160">このセクションにはで設定できる構成設定が含まれています、`aspnet.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="5191e-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="5191e-161">このファイルにはプラットフォームによっては、2 つの場所のいずれかであります。</span><span class="sxs-lookup"><span data-stu-id="5191e-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="5191e-162">SignalR パフォーマンスを向上させることがあります ASP.NET の設定を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="5191e-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="5191e-163">**CPU ごとの最大の同時要求**: この設定を増やすとパフォーマンスのボトルネックを軽減する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5191e-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="5191e-164">この設定を増やすには、するには、次の構成設定を追加、`aspnet.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="5191e-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="5191e-165">**要求キューの制限**: 接続の合計数を超えた場合、`maxConcurrentRequestsPerCPU`設定すると、ASP.NET は開始キューを使用して要求を調整します。</span><span class="sxs-lookup"><span data-stu-id="5191e-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="5191e-166">キューのサイズを増やすに増やすことができます、`requestQueueLimit`設定します。</span><span class="sxs-lookup"><span data-stu-id="5191e-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="5191e-167">これを行うには、次の構成設定を追加、`processModel`内のノード`config/machine.config`(なく`aspnet.config`)。</span><span class="sxs-lookup"><span data-stu-id="5191e-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="5191e-168">パフォーマンスの問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="5191e-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="5191e-169">このセクションでは、アプリケーションのパフォーマンスのボトルネックを検索する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="5191e-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="5191e-170">WebSocket が使用されていることを確認しています</span><span class="sxs-lookup"><span data-stu-id="5191e-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="5191e-171">SignalR は、さまざまなトランスポートを使用して、クライアントとサーバー間の通信、WebSocket 大幅なパフォーマンスの利点を提供し、クライアントとサーバーがサポートしている場合に使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5191e-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="5191e-172">調べるには、クライアントとサーバーが、WebSocket の要件を満たすかどうかは、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)です。</span><span class="sxs-lookup"><span data-stu-id="5191e-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="5191e-173">調べるにはどのようなトランスポートは、アプリケーションで使用されて、ブラウザーの開発者ツールを使用し、どのようなトランスポートが接続に使用されているログを調べてできます。</span><span class="sxs-lookup"><span data-stu-id="5191e-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="5191e-174">Internet Explorer と Chrome ブラウザーの開発ツールの使用方法の詳細については、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)です。</span><span class="sxs-lookup"><span data-stu-id="5191e-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="5191e-175">SignalR パフォーマンス カウンターを使用します。</span><span class="sxs-lookup"><span data-stu-id="5191e-175">Using SignalR performance counters</span></span>

<span data-ttu-id="5191e-176">このセクションを有効にし、SignalR パフォーマンス カウンターを使用する方法について説明で見つかった、`Microsoft.AspNet.SignalR.Utils`パッケージです。</span><span class="sxs-lookup"><span data-stu-id="5191e-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="5191e-177">Signalr.exe をインストールします。</span><span class="sxs-lookup"><span data-stu-id="5191e-177">Installing signalr.exe</span></span>

<span data-ttu-id="5191e-178">パフォーマンス カウンターは、という SignalR.exe ユーティリティを使用してサーバーに追加できます。</span><span class="sxs-lookup"><span data-stu-id="5191e-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="5191e-179">このユーティリティをインストールするには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="5191e-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="5191e-180">Visual Studio アプリケーションで次のように選択します**ツール**、**ライブラリ パッケージ マネージャー**、 **Manage NuGet Packages for Solution しています.。**</span><span class="sxs-lookup"><span data-stu-id="5191e-180">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="5191e-181">検索**signalr.utils**インストール を選択します。</span><span class="sxs-lookup"><span data-stu-id="5191e-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="5191e-182">パッケージをインストールするライセンス契約に同意します。</span><span class="sxs-lookup"><span data-stu-id="5191e-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="5191e-183">インストールされます SignalR.exe`<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`です。</span><span class="sxs-lookup"><span data-stu-id="5191e-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="5191e-184">SignalR.exe でパフォーマンス カウンターをインストールします。</span><span class="sxs-lookup"><span data-stu-id="5191e-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="5191e-185">SignalR パフォーマンス カウンターをインストールするには、次のパラメーターを使用して特権のコマンド プロンプトで SignalR.exe を実行します。</span><span class="sxs-lookup"><span data-stu-id="5191e-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="5191e-186">SignalR パフォーマンス カウンターを削除するには、次のパラメーターを使用して特権のコマンド プロンプトで SignalR.exe を実行します。</span><span class="sxs-lookup"><span data-stu-id="5191e-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="5191e-187">SignalR パフォーマンス カウンター</span><span class="sxs-lookup"><span data-stu-id="5191e-187">SignalR Performance counters</span></span>

<span data-ttu-id="5191e-188">ユーティリティ パッケージでは、次のパフォーマンス カウンターをインストールします。</span><span class="sxs-lookup"><span data-stu-id="5191e-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="5191e-189">"Total"カウンターは、最後のアプリケーション プールまたはサーバーが再起動した後に、イベントの数を計測します。</span><span class="sxs-lookup"><span data-stu-id="5191e-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="5191e-190">**接続のメトリック**</span><span class="sxs-lookup"><span data-stu-id="5191e-190">**Connection metrics**</span></span>

<span data-ttu-id="5191e-191">次のメトリックは、接続の有効期間発生するイベントを測定します。</span><span class="sxs-lookup"><span data-stu-id="5191e-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="5191e-192">詳細については、次を参照してください。[接続の有効期間イベントの処理と理解](../guide-to-the-api/handling-connection-lifetime-events.md)です。</span><span class="sxs-lookup"><span data-stu-id="5191e-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="5191e-193">**接続されている接続**</span><span class="sxs-lookup"><span data-stu-id="5191e-193">**Connections Connected**</span></span>
- <span data-ttu-id="5191e-194">**接続の再接続**</span><span class="sxs-lookup"><span data-stu-id="5191e-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="5191e-195">**接続が切断されました**</span><span class="sxs-lookup"><span data-stu-id="5191e-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="5191e-196">**現在の接続**</span><span class="sxs-lookup"><span data-stu-id="5191e-196">**Connections Current**</span></span>

<span data-ttu-id="5191e-197">**メッセージ メトリックス**</span><span class="sxs-lookup"><span data-stu-id="5191e-197">**Message metrics**</span></span>

<span data-ttu-id="5191e-198">次のメトリックは、SignalR によって生成されるメッセージ トラフィックを測定します。</span><span class="sxs-lookup"><span data-stu-id="5191e-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="5191e-199">**接続のメッセージの受信合計**</span><span class="sxs-lookup"><span data-stu-id="5191e-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="5191e-200">**合計送信接続メッセージ**</span><span class="sxs-lookup"><span data-stu-id="5191e-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="5191e-201">**接続を受信メッセージ数/秒**</span><span class="sxs-lookup"><span data-stu-id="5191e-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="5191e-202">**接続が送信メッセージ/秒**</span><span class="sxs-lookup"><span data-stu-id="5191e-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="5191e-203">**メッセージ バスのメトリック**</span><span class="sxs-lookup"><span data-stu-id="5191e-203">**Message bus metrics**</span></span>

<span data-ttu-id="5191e-204">次のメトリックは、内部の SignalR メッセージ バスは、すべての着信および発信の SignalR メッセージが置かれているキューを通過するトラフィックを測定します。</span><span class="sxs-lookup"><span data-stu-id="5191e-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="5191e-205">メッセージが**Published**が送信またはブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="5191e-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="5191e-206">A**サブスクライバー**このコンテキストでメッセージ バス上のサブスクリプションはクライアントとサーバー自体の数と等しくなります。</span><span class="sxs-lookup"><span data-stu-id="5191e-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="5191e-207">**割り当てられたワーカー** ; のアクティブな接続にデータを送信するコンポーネント、**ビジー状態のワーカー**は、アクティブにメッセージを送信しています。</span><span class="sxs-lookup"><span data-stu-id="5191e-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="5191e-208">**メッセージ バス メッセージを受信した合計**</span><span class="sxs-lookup"><span data-stu-id="5191e-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="5191e-209">**メッセージ バスのメッセージの受信/秒**</span><span class="sxs-lookup"><span data-stu-id="5191e-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="5191e-210">**メッセージ バスのメッセージの合計を発行します。**</span><span class="sxs-lookup"><span data-stu-id="5191e-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="5191e-211">**メッセージ バスのメッセージの発行/秒**</span><span class="sxs-lookup"><span data-stu-id="5191e-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="5191e-212">**メッセージ バスのサブスクライバーの現在**</span><span class="sxs-lookup"><span data-stu-id="5191e-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="5191e-213">**メッセージ バスのサブスクライバーの合計**</span><span class="sxs-lookup"><span data-stu-id="5191e-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="5191e-214">**メッセージ バスのサブスクライバー/秒**</span><span class="sxs-lookup"><span data-stu-id="5191e-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="5191e-215">**メッセージ バスには、ワーカーが割り当てられています。**</span><span class="sxs-lookup"><span data-stu-id="5191e-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="5191e-216">**メッセージ バスのビジー状態のワーカー**</span><span class="sxs-lookup"><span data-stu-id="5191e-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="5191e-217">**メッセージ バスのトピックの現在**</span><span class="sxs-lookup"><span data-stu-id="5191e-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="5191e-218">**誤差のメトリック**</span><span class="sxs-lookup"><span data-stu-id="5191e-218">**Error metrics**</span></span>

<span data-ttu-id="5191e-219">次のメトリックは、SignalR メッセージ トラフィックによって生成されたエラーを測定します。</span><span class="sxs-lookup"><span data-stu-id="5191e-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="5191e-220">**ハブの解決**ハブまたはハブのメソッドは解決できない場合にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5191e-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="5191e-221">**ハブ呼び出し**エラーは、ハブ メソッドの呼び出し中にスローされる例外。</span><span class="sxs-lookup"><span data-stu-id="5191e-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="5191e-222">**トランスポート**エラーは、接続エラーの HTTP 要求または応答の中にスローされます。</span><span class="sxs-lookup"><span data-stu-id="5191e-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="5191e-223">**エラー: すべての合計**</span><span class="sxs-lookup"><span data-stu-id="5191e-223">**Errors: All Total**</span></span>
- <span data-ttu-id="5191e-224">**エラー: 1 秒あたりのすべて**</span><span class="sxs-lookup"><span data-stu-id="5191e-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="5191e-225">**エラー: ハブ解像度の合計**</span><span class="sxs-lookup"><span data-stu-id="5191e-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="5191e-226">**エラー: ハブ解像度/秒**</span><span class="sxs-lookup"><span data-stu-id="5191e-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="5191e-227">**エラー: ハブ呼び出しの合計**</span><span class="sxs-lookup"><span data-stu-id="5191e-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="5191e-228">**エラー: ハブ呼び出し/秒**</span><span class="sxs-lookup"><span data-stu-id="5191e-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="5191e-229">**エラー: トランスポート合計**</span><span class="sxs-lookup"><span data-stu-id="5191e-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="5191e-230">**1 秒あたりのトランスポートのエラー:**</span><span class="sxs-lookup"><span data-stu-id="5191e-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="5191e-231">**スケール アウト メトリック**</span><span class="sxs-lookup"><span data-stu-id="5191e-231">**Scaleout metrics**</span></span>

<span data-ttu-id="5191e-232">次のメトリックは、トラフィックおよびスケール アウト プロバイダーによって生成されたエラーを測定します。</span><span class="sxs-lookup"><span data-stu-id="5191e-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="5191e-233">A**ストリーム**このコンテキストでは、スケール アウト プロバイダーによって、スケール ユニットを使用。 これは SQL Server が使用される場合は、テーブル、Service Bus を使用する場合のトピックとサブスクリプション Redis を使用する場合。</span><span class="sxs-lookup"><span data-stu-id="5191e-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="5191e-234">各ストリームにより、順序付けられた読み取りおよび書き込み操作です。1 つのストリームは、潜在的なスケール ボトルネックは、そのボトルネックを低減するストリームの数を増やすことができます。</span><span class="sxs-lookup"><span data-stu-id="5191e-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="5191e-235">複数のストリームを使用している場合 SignalR は自動的に配布されている順序で特定の接続から送信されたメッセージを保証する方法でこれらのストリーム間 (シャード) メッセージを使用します。</span><span class="sxs-lookup"><span data-stu-id="5191e-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="5191e-236">[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)設定では、SignalR で保持されているスケール アウト送信キューの長さを指定します。</span><span class="sxs-lookup"><span data-stu-id="5191e-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="5191e-237">値に設定する、構成済みのメッセージング バック プレーンに一度に 1 つずつ送信する送信キューのすべてのメッセージを配置が 0 より大きい。</span><span class="sxs-lookup"><span data-stu-id="5191e-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="5191e-238">キューのサイズが構成されている長さを超えた場合にを送信する後続の呼び出しはすぐに失敗、 [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx)キュー内のメッセージの数が、設定よりも小さくするまでもう一度です。</span><span class="sxs-lookup"><span data-stu-id="5191e-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="5191e-239">実装済みのバック プレーン一般に、独自のキューまたはフロー制御では、キューが既定では無効です。</span><span class="sxs-lookup"><span data-stu-id="5191e-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="5191e-240">SQL Server の場合は任意の時点で起こっているの送信メッセージの数の制限接続プールを効果的にします。</span><span class="sxs-lookup"><span data-stu-id="5191e-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="5191e-241">既定では、1 つだけのストリームが SQL Server および Redis の使用は、Service Bus の 5 つのストリームを使用およびキューが無効になっているがこれらの設定は、SQL Server と Service Bus の構成で変更できます。</span><span class="sxs-lookup"><span data-stu-id="5191e-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="5191e-242">**SQL Server のバック プレーンのテーブル数とキューの長さを構成するために .NET コードをサーバー**</span><span class="sxs-lookup"><span data-stu-id="5191e-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="5191e-243">**Service Bus のバック プレーンのトピック数とキューの長さを構成するために .NET コードをサーバー**</span><span class="sxs-lookup"><span data-stu-id="5191e-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="5191e-244">A**バッファリング**ストリームが途中終了状態になったものです。 ストリームが不要になったエラーが発生するまでにすぐに、ストリームが途中終了状態、バック プレーンに送信されるすべてのメッセージは失敗します。</span><span class="sxs-lookup"><span data-stu-id="5191e-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="5191e-245">**Send Queue Length**送信されているが、まだ送信されるメッセージの数です。</span><span class="sxs-lookup"><span data-stu-id="5191e-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="5191e-246">**スケール アウト メッセージ バスのメッセージの受信/秒**</span><span class="sxs-lookup"><span data-stu-id="5191e-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="5191e-247">**スケール アウト ストリームの合計**</span><span class="sxs-lookup"><span data-stu-id="5191e-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="5191e-248">**スケール アウトのストリームを開く**</span><span class="sxs-lookup"><span data-stu-id="5191e-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="5191e-249">**スケール アウト ストリームのバッファリング**</span><span class="sxs-lookup"><span data-stu-id="5191e-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="5191e-250">**スケール アウト エラーの合計**</span><span class="sxs-lookup"><span data-stu-id="5191e-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="5191e-251">**スケール アウト エラー/秒**</span><span class="sxs-lookup"><span data-stu-id="5191e-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="5191e-252">**スケール アウト送信キューの長さ**</span><span class="sxs-lookup"><span data-stu-id="5191e-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="5191e-253">これらのカウンターの測定の詳細については、次を参照してください。 [Azure Service Bus での SignalR スケール アウト](scaleout-with-windows-azure-service-bus.md)です。</span><span class="sxs-lookup"><span data-stu-id="5191e-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="5191e-254">他のパフォーマンス カウンターを使用します。</span><span class="sxs-lookup"><span data-stu-id="5191e-254">Using other performance counters</span></span>

<span data-ttu-id="5191e-255">次のパフォーマンス カウンターは、アプリケーションのパフォーマンスの監視に役立つ場合もあります。</span><span class="sxs-lookup"><span data-stu-id="5191e-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="5191e-256">**メモリ**</span><span class="sxs-lookup"><span data-stu-id="5191e-256">**Memory**</span></span>

- <span data-ttu-id="5191e-257">.NET CLR Memory\\ヒープ (w3wp) をすべて # バイト</span><span class="sxs-lookup"><span data-stu-id="5191e-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="5191e-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="5191e-258">**ASP.NET**</span></span>

- <span data-ttu-id="5191e-259">ASP.NET\Requests Current</span><span class="sxs-lookup"><span data-stu-id="5191e-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="5191e-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="5191e-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="5191e-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="5191e-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="5191e-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="5191e-262">**CPU**</span></span>

- <span data-ttu-id="5191e-263">プロセッサ時間の Information\Processor</span><span class="sxs-lookup"><span data-stu-id="5191e-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="5191e-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="5191e-264">**TCP/IP**</span></span>

- <span data-ttu-id="5191e-265">TCPv6/接続の確立</span><span class="sxs-lookup"><span data-stu-id="5191e-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="5191e-266">TCPv4/接続の確立</span><span class="sxs-lookup"><span data-stu-id="5191e-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="5191e-267">**Web サービス**</span><span class="sxs-lookup"><span data-stu-id="5191e-267">**Web Service**</span></span>

- <span data-ttu-id="5191e-268">Web service \current Connections</span><span class="sxs-lookup"><span data-stu-id="5191e-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="5191e-269">Web Service\Maximum 接続</span><span class="sxs-lookup"><span data-stu-id="5191e-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="5191e-270">**スレッド化**</span><span class="sxs-lookup"><span data-stu-id="5191e-270">**Threading**</span></span>

- <span data-ttu-id="5191e-271">.NET CLR ロックおよびスレッド\\論理の現在のスレッドの数</span><span class="sxs-lookup"><span data-stu-id="5191e-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="5191e-272">.NET CLR ロックおよびスレッド\\現在の物理スレッドの数</span><span class="sxs-lookup"><span data-stu-id="5191e-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="5191e-273">その他の参照情報</span><span class="sxs-lookup"><span data-stu-id="5191e-273">Other Resources</span></span>

<span data-ttu-id="5191e-274">ASP.NET のパフォーマンスの監視とチューニングの詳細については、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="5191e-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="5191e-275">[ASP.NET のパフォーマンスの概要](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="5191e-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="5191e-276">IIS 7.5、IIS 7.0 および IIS 6.0 に ASP.NET スレッドの使用状況</span><span class="sxs-lookup"><span data-stu-id="5191e-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="5191e-277">&lt;applicationPool&gt;要素 (Web 設定)</span><span class="sxs-lookup"><span data-stu-id="5191e-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
