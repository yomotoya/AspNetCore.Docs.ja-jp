---
uid: signalr/overview/performance/signalr-performance
title: SignalR パフォーマンス |Microsoft Docs
author: bradygaster
description: SignalR パフォーマンス
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 3326c2e600854fc7a4435d96c45b04a6188d3937
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836513"
---
<a name="signalr-performance"></a><span data-ttu-id="75fd4-103">SignalR パフォーマンス</span><span class="sxs-lookup"><span data-stu-id="75fd4-103">SignalR Performance</span></span>
====================
<span data-ttu-id="75fd4-104">提供者: [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="75fd4-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="75fd4-105">このトピックでは、ための設計、計測、および SignalR アプリケーションのパフォーマンスを向上する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="75fd4-106">このトピックで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="75fd4-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="75fd4-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="75fd4-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="75fd4-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="75fd4-108">.NET 4.5</span></span>
> - <span data-ttu-id="75fd4-109">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="75fd4-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="75fd4-110">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="75fd4-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="75fd4-111">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="75fd4-112">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="75fd4-112">Questions and comments</span></span>
>
> <span data-ttu-id="75fd4-113">このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。</span><span class="sxs-lookup"><span data-stu-id="75fd4-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="75fd4-114">チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。</span><span class="sxs-lookup"><span data-stu-id="75fd4-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="75fd4-115">SignalR パフォーマンスとスケーリングに最近行ったプレゼンテーションは、次を参照してください。 [ASP.NET SignalR によるリアルタイムの Web スケール](https://channel9.msdn.com/Events/Build/2013/3-502)します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="75fd4-116">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="75fd4-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="75fd4-117">設計に関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="75fd4-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="75fd4-118">SignalR サーバーのパフォーマンスのチューニング</span><span class="sxs-lookup"><span data-stu-id="75fd4-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="75fd4-119">パフォーマンスの問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="75fd4-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="75fd4-120">SignalR パフォーマンス カウンターの使用</span><span class="sxs-lookup"><span data-stu-id="75fd4-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="75fd4-121">その他のパフォーマンス カウンターの使用</span><span class="sxs-lookup"><span data-stu-id="75fd4-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="75fd4-122">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="75fd4-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="75fd4-123">設計に関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="75fd4-123">Design considerations</span></span>

<span data-ttu-id="75fd4-124">このセクションでは、不要なネットワーク トラフィックを生成することによってパフォーマンスが低下しないことを確認するには SignalR アプリケーションのデザイン時に実装できるパターンについて説明します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="75fd4-125">メッセージの頻度を調整</span><span class="sxs-lookup"><span data-stu-id="75fd4-125">Throttling message frequency</span></span>

<span data-ttu-id="75fd4-126">を (リアルタイムのゲーム アプリケーションの場合) などの高頻度でメッセージを送信するアプリケーションであっても、ほとんどのアプリケーションは、1 秒間に複数のメッセージを送信する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="75fd4-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="75fd4-127">各クライアントを生成するトラフィックの量を減らすためには、メッセージ ループを実装するキューおよび送信メッセージありませんより固定レートよりも頻繁に (つまり、メッセージ数まで送信されます 1 秒ごとに、その時点でのメッセージがある場合terval 送信する)。</span><span class="sxs-lookup"><span data-stu-id="75fd4-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="75fd4-128">メッセージを (クライアントとサーバーの両方) から、一定のレートを調整するサンプル アプリケーションの場合、次を参照してください。 [SignalR による高頻度リアルタイム メッセージング](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="75fd4-129">メッセージのサイズを縮小します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-129">Reducing message size</span></span>

<span data-ttu-id="75fd4-130">SignalR メッセージのサイズを小さくには、シリアル化されたオブジェクトのサイズを小さきます。</span><span class="sxs-lookup"><span data-stu-id="75fd4-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="75fd4-131">サーバー コードでは、転送する必要がないプロパティを格納しているオブジェクトを送信する場合は妨げられるこれらのプロパティを使用してシリアル化される、`JsonIgnore`属性。</span><span class="sxs-lookup"><span data-stu-id="75fd4-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="75fd4-132">プロパティの名前がメッセージにも格納されています。使用してプロパティの名前を短縮できます、`JsonProperty`属性。</span><span class="sxs-lookup"><span data-stu-id="75fd4-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="75fd4-133">次のコード サンプルでは、プロパティ名を短縮する方法と、クライアントに送信されてからプロパティを除外する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="75fd4-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="75fd4-134">**.NET サーバー コードから、クライアントに送信されるデータを除外する JsonIgnore 属性とメッセージのサイズを小さく JsonProperty 属性を説明します。**</span><span class="sxs-lookup"><span data-stu-id="75fd4-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="75fd4-135">読みやすさを維持するために、クライアント コードで保守性、プロパティの省略名は、わかりやすいにマップが変更された名、メッセージが受信されるとします。</span><span class="sxs-lookup"><span data-stu-id="75fd4-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="75fd4-136">次のコード サンプルに示します (マッピング) のメッセージ コントラクトを定義することより長いものを使用する簡略名を再マップの方法の 1 つを使用して、`reMap`最適化されたメッセージ クラスに、コントラクトを適用する関数。</span><span class="sxs-lookup"><span data-stu-id="75fd4-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="75fd4-137">**クライアント側の JavaScript コードを再配置するには、人間が判読できる名前のプロパティ名を短縮します。**</span><span class="sxs-lookup"><span data-stu-id="75fd4-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="75fd4-138">名前は、同じメソッドを使用しても、サーバーに、クライアントからのメッセージで短縮できます。</span><span class="sxs-lookup"><span data-stu-id="75fd4-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="75fd4-139">(つまり、メッセージに使用されるメモリ量) のメモリ使用量の削減、メッセージのオブジェクトもパフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="75fd4-140">たとえば場合は、さまざまな、`int`は必要ありません、`short`または`byte`代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="75fd4-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="75fd4-141">メッセージがメッセージのサイズを減らし、サーバーのメモリにメッセージ バス内で格納されるためは、サーバー メモリの問題を解決でこともできます。</span><span class="sxs-lookup"><span data-stu-id="75fd4-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="75fd4-142">SignalR サーバーのパフォーマンスのチューニング</span><span class="sxs-lookup"><span data-stu-id="75fd4-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="75fd4-143">SignalR アプリケーションでパフォーマンスの向上のため、サーバーをチューニングするは、次の構成設定を使用できます。</span><span class="sxs-lookup"><span data-stu-id="75fd4-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="75fd4-144">ASP.NET アプリケーションのパフォーマンスを向上させる方法の概要については、次を参照してください。 [ASP.NET パフォーマンスの向上](https://msdn.microsoft.com/library/ff647787.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="75fd4-145">**SignalR の構成設定**</span><span class="sxs-lookup"><span data-stu-id="75fd4-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="75fd4-146">**DefaultMessageBufferSize**:既定では、SignalR は 1000 メッセージごとの接続のハブあたりのメモリに保持されます。</span><span class="sxs-lookup"><span data-stu-id="75fd4-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="75fd4-147">サイズの大きいメッセージを使用している場合は、この値を減らすことで緩和できるメモリの問題を作成この可能性があります。</span><span class="sxs-lookup"><span data-stu-id="75fd4-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="75fd4-148">この設定を設定することができます、`Application_Start`または ASP.NET アプリケーションでのイベント ハンドラー、`Configuration`で自己ホスト型アプリケーションの OWIN startup クラスのメソッド。</span><span class="sxs-lookup"><span data-stu-id="75fd4-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="75fd4-149">次の例では、サーバーのメモリ使用量を削減するために、アプリケーションのメモリ使用量を減らすためにこの値を小さく方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="75fd4-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="75fd4-150">**既定のメッセージ バッファーのサイズを減らすため、Startup.cs で .NET サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="75fd4-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="75fd4-151">**IIS の構成設定**</span><span class="sxs-lookup"><span data-stu-id="75fd4-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="75fd4-152">**アプリケーションごとの最大同時要求**:IIS の同時実行の数を増やす要求の要求の処理の使用可能なサーバー リソースが増加します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="75fd4-153">既定値は 5000 です。この設定を向上させるのに管理者特権でコマンド プロンプトで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="75fd4-154">**ApplicationPool QueueLength**:これは、Http.sys がアプリケーション プールのキューを要求の最大数です。</span><span class="sxs-lookup"><span data-stu-id="75fd4-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="75fd4-155">キューには、新しい要求は 503「サービスを利用できません」の応答を受信します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="75fd4-156">既定値は 1000 です。</span><span class="sxs-lookup"><span data-stu-id="75fd4-156">The default value is 1000.</span></span>

    <span data-ttu-id="75fd4-157">アプリケーションをホストしているアプリケーション プールでワーカー プロセスのキューの長さを短くと、メモリ リソースを節約します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="75fd4-158">詳細については、次を参照してください。[管理、調整、およびアプリケーション プールの構成](https://technet.microsoft.com/library/cc745955.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="75fd4-159">**ASP.NET 構成の設定**</span><span class="sxs-lookup"><span data-stu-id="75fd4-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="75fd4-160">このセクションにはで設定できる構成設定が含まれています、`aspnet.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="75fd4-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="75fd4-161">このファイルはプラットフォームに応じて、2 つの場所のいずれかであります。</span><span class="sxs-lookup"><span data-stu-id="75fd4-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="75fd4-162">SignalR パフォーマンスを向上させることがあります ASP.NET の設定を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="75fd4-163">**CPU ごとの同時要求の最大**:この設定を増やすとパフォーマンスのボトルネックを軽減することがあります。</span><span class="sxs-lookup"><span data-stu-id="75fd4-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="75fd4-164">この設定を大きくには、次の構成設定を追加、`aspnet.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="75fd4-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="75fd4-165">**要求キューの制限**:接続の合計数を超える場合、`maxConcurrentRequestsPerCPU`設定すると、ASP.NET は開始キューを使用して要求を調整します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="75fd4-166">キューのサイズを増やすを増やすことができます、`requestQueueLimit`設定します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="75fd4-167">これを行うには、次の構成設定を追加、`processModel`ノード`config/machine.config`(なく`aspnet.config`)。</span><span class="sxs-lookup"><span data-stu-id="75fd4-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="75fd4-168">パフォーマンスの問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="75fd4-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="75fd4-169">このセクションでは、アプリケーションのパフォーマンスのボトルネックを見つける方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="75fd4-170">WebSocket が使用されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="75fd4-171">SignalR は、さまざまなトランスポートを使用して、クライアントとサーバー間の通信、WebSocket は、パフォーマンスに大きなメリットが提供し、クライアントとサーバーがサポートしている場合に使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="75fd4-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="75fd4-172">調べるには、クライアントとサーバーが WebSocket の要件を満たしているかどうかは、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="75fd4-173">どのようなトランスポートは、アプリケーションで使用されているを確認するのには、ブラウザー開発者ツールを使用し、トランスポートが接続に使用されているログを調べてできます。</span><span class="sxs-lookup"><span data-stu-id="75fd4-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="75fd4-174">Internet Explorer と Chrome ブラウザーの開発ツールを使用する方法の詳細については、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="75fd4-175">SignalR パフォーマンス カウンターの使用</span><span class="sxs-lookup"><span data-stu-id="75fd4-175">Using SignalR performance counters</span></span>

<span data-ttu-id="75fd4-176">有効にして、SignalR パフォーマンス カウンターを使用する方法を説明にある、`Microsoft.AspNet.SignalR.Utils`パッケージ。</span><span class="sxs-lookup"><span data-stu-id="75fd4-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="75fd4-177">Signalr.exe をインストールします。</span><span class="sxs-lookup"><span data-stu-id="75fd4-177">Installing signalr.exe</span></span>

<span data-ttu-id="75fd4-178">SignalR.exe というユーティリティを使用してサーバーには、パフォーマンス カウンターを追加できます。</span><span class="sxs-lookup"><span data-stu-id="75fd4-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="75fd4-179">このユーティリティをインストールするには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="75fd4-180">Visual Studio で、次のように選択します**ツール** > **NuGet パッケージ マネージャー** > **ソリューションの NuGet パッケージの管理。**</span><span class="sxs-lookup"><span data-stu-id="75fd4-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="75fd4-181">検索**signalr.utils**インストールを選択します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="75fd4-182">パッケージをインストールするライセンス条項に同意します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="75fd4-183">SignalR.exe にインストールされる`<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="75fd4-184">SignalR.exe でパフォーマンス カウンターをインストールします。</span><span class="sxs-lookup"><span data-stu-id="75fd4-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="75fd4-185">SignalR パフォーマンス カウンターをインストールするには、次のパラメーターを使用して、管理者特権でコマンド プロンプトで SignalR.exe を実行します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="75fd4-186">SignalR パフォーマンス カウンターを削除するには、次のパラメーターを使用して、管理者特権でコマンド プロンプトで SignalR.exe を実行します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="75fd4-187">SignalR パフォーマンス カウンター</span><span class="sxs-lookup"><span data-stu-id="75fd4-187">SignalR Performance counters</span></span>

<span data-ttu-id="75fd4-188">ユーティリティ パッケージは、次のパフォーマンス カウンターをインストールします。</span><span class="sxs-lookup"><span data-stu-id="75fd4-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="75fd4-189">"Total"カウンターは、最後のアプリケーション プールまたはサーバーを再起動するために、イベントの数を計測します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="75fd4-190">**接続メトリック**</span><span class="sxs-lookup"><span data-stu-id="75fd4-190">**Connection metrics**</span></span>

<span data-ttu-id="75fd4-191">次のメトリックは、発生する接続の有効期間イベントを測定します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="75fd4-192">詳細については、次を参照してください。[接続の有効期間イベントの処理と理解](../guide-to-the-api/handling-connection-lifetime-events.md)します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="75fd4-193">**接続されている接続**</span><span class="sxs-lookup"><span data-stu-id="75fd4-193">**Connections Connected**</span></span>
- <span data-ttu-id="75fd4-194">**接続の再接続**</span><span class="sxs-lookup"><span data-stu-id="75fd4-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="75fd4-195">**接続の切断**</span><span class="sxs-lookup"><span data-stu-id="75fd4-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="75fd4-196">**現在の接続**</span><span class="sxs-lookup"><span data-stu-id="75fd4-196">**Connections Current**</span></span>

<span data-ttu-id="75fd4-197">**メッセージ メトリックス**</span><span class="sxs-lookup"><span data-stu-id="75fd4-197">**Message metrics**</span></span>

<span data-ttu-id="75fd4-198">次のメトリックは、SignalR によって生成されるメッセージ トラフィックを測定します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="75fd4-199">**接続メッセージの受信した合計**</span><span class="sxs-lookup"><span data-stu-id="75fd4-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="75fd4-200">**合計送信接続メッセージ**</span><span class="sxs-lookup"><span data-stu-id="75fd4-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="75fd4-201">**接続を受信メッセージ数/秒**</span><span class="sxs-lookup"><span data-stu-id="75fd4-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="75fd4-202">**接続が送信メッセージ/秒**</span><span class="sxs-lookup"><span data-stu-id="75fd4-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="75fd4-203">**メッセージ バスのメトリック**</span><span class="sxs-lookup"><span data-stu-id="75fd4-203">**Message bus metrics**</span></span>

<span data-ttu-id="75fd4-204">次のメトリックは、内部の SignalR メッセージ バス、すべての受信および送信の SignalR メッセージの配置のキューを通過するトラフィックを測定します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="75fd4-205">メッセージが**Published**が送信またはブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="75fd4-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="75fd4-206">A**サブスクライバー**メッセージ バス上のサブスクリプションは、このコンテキストでは、クライアントとサーバー自体の数と等しくなります。</span><span class="sxs-lookup"><span data-stu-id="75fd4-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="75fd4-207">**割り当てられたワーカー** ; のアクティブな接続にデータを送信するコンポーネントを**ビジー状態の Worker**がアクティブにメッセージを送信する 1 つです。</span><span class="sxs-lookup"><span data-stu-id="75fd4-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="75fd4-208">**メッセージ バス メッセージの受信の合計数**</span><span class="sxs-lookup"><span data-stu-id="75fd4-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="75fd4-209">**メッセージ バス メッセージの受信/秒**</span><span class="sxs-lookup"><span data-stu-id="75fd4-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="75fd4-210">**メッセージ バスのメッセージの合計を発行します。**</span><span class="sxs-lookup"><span data-stu-id="75fd4-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="75fd4-211">**メッセージ バス メッセージの 1 秒あたりの発行**</span><span class="sxs-lookup"><span data-stu-id="75fd4-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="75fd4-212">**メッセージ バスのサブスクライバーの現在**</span><span class="sxs-lookup"><span data-stu-id="75fd4-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="75fd4-213">**メッセージ バスのサブスクライバーの合計**</span><span class="sxs-lookup"><span data-stu-id="75fd4-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="75fd4-214">**メッセージ バスのサブスクライバー/秒**</span><span class="sxs-lookup"><span data-stu-id="75fd4-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="75fd4-215">**メッセージ バスには、ワーカーが割り当てられています。**</span><span class="sxs-lookup"><span data-stu-id="75fd4-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="75fd4-216">**メッセージ バスのビジー状態のワーカー**</span><span class="sxs-lookup"><span data-stu-id="75fd4-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="75fd4-217">**メッセージ バスのトピックの現在**</span><span class="sxs-lookup"><span data-stu-id="75fd4-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="75fd4-218">**誤差のメトリック**</span><span class="sxs-lookup"><span data-stu-id="75fd4-218">**Error metrics**</span></span>

<span data-ttu-id="75fd4-219">次のメトリックは、SignalR メッセージ トラフィックによって生成されたエラーを測定します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="75fd4-220">**ハブの解決**ハブまたはハブのメソッドは解決できない場合にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="75fd4-221">**ハブ呼び出し**エラーは、ハブ メソッドの呼び出し中にスローされる例外。</span><span class="sxs-lookup"><span data-stu-id="75fd4-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="75fd4-222">**トランスポート**エラーは、接続エラーの HTTP 要求または応答時にスローされます。</span><span class="sxs-lookup"><span data-stu-id="75fd4-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="75fd4-223">**エラー:すべての合計**</span><span class="sxs-lookup"><span data-stu-id="75fd4-223">**Errors: All Total**</span></span>
- <span data-ttu-id="75fd4-224">**エラー:1 秒あたりのすべて**</span><span class="sxs-lookup"><span data-stu-id="75fd4-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="75fd4-225">**エラー:ハブの解像度の合計**</span><span class="sxs-lookup"><span data-stu-id="75fd4-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="75fd4-226">**エラー:ハブの解像度/秒**</span><span class="sxs-lookup"><span data-stu-id="75fd4-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="75fd4-227">**エラー:ハブ呼び出し合計**</span><span class="sxs-lookup"><span data-stu-id="75fd4-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="75fd4-228">**エラー:ハブ呼び出し数/秒**</span><span class="sxs-lookup"><span data-stu-id="75fd4-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="75fd4-229">**エラー:トランスポートの合計**</span><span class="sxs-lookup"><span data-stu-id="75fd4-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="75fd4-230">**エラー:1 秒あたりのトランスポート**</span><span class="sxs-lookup"><span data-stu-id="75fd4-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="75fd4-231">**スケール アウトのメトリック**</span><span class="sxs-lookup"><span data-stu-id="75fd4-231">**Scaleout metrics**</span></span>

<span data-ttu-id="75fd4-232">次のメトリックは、トラフィックと、スケール アウト プロバイダーによって生成されたエラーを測定します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="75fd4-233">A **Stream**このコンテキストでは、スケール ユニットはスケール アウト プロバイダーで使用される。 これは SQL Server を使用する場合は、テーブル、Service Bus を使用する場合は、トピックとサブスクリプション Redis を使用する場合。</span><span class="sxs-lookup"><span data-stu-id="75fd4-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="75fd4-234">各ストリームによって順序付けられた読み取りおよび書き込み操作です。1 つのストリームは、潜在的なスケール ボトルネックは、そのボトルネックを減らすためにストリームの数を増やすことができます。</span><span class="sxs-lookup"><span data-stu-id="75fd4-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="75fd4-235">複数のストリームを使用している場合 SignalR は自動的に配布されている順序で特定の接続から送信されたメッセージを保証する方法でこれらのストリーム間でメッセージの (数シャード) を使用します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="75fd4-236">[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)設定では、SignalR で保持されているスケール アウト送信キューの長さを制御します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="75fd4-237">値に設定する、構成済みのメッセージング バック プレーンに一度に 1 つずつ送信する送信キューのすべてのメッセージを配置は 0 より大きい。</span><span class="sxs-lookup"><span data-stu-id="75fd4-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="75fd4-238">キューのサイズが構成されている長さを超えた場合では送信する後続の呼び出しがすぐに失敗、 [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx)までキュー内のメッセージの数が少ない設定よりももう一度です。</span><span class="sxs-lookup"><span data-stu-id="75fd4-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="75fd4-239">実装されているバック プレーン一般に、独自のキューまたはフロー制御インプレース、キューが既定で無効にします。</span><span class="sxs-lookup"><span data-stu-id="75fd4-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="75fd4-240">場合は、SQL Server は任意の時点で起こっているの送信メッセージの数の制限接続プールを効果的に。</span><span class="sxs-lookup"><span data-stu-id="75fd4-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="75fd4-241">SQL Server と Redis の既定では、1 つのみのストリームが使用される、Service bus の 5 つのストリームが使用される、キューが無効になっているおよびが SQL サーバーと Service Bus の構成でこれらの設定を変更できます。</span><span class="sxs-lookup"><span data-stu-id="75fd4-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="75fd4-242">**SQL Server のバック プレーンのテーブルの数とキューの長さを構成するための .NET サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="75fd4-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="75fd4-243">**バック プレーンの Service Bus のトピックの数とキューの長さを構成するための .NET サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="75fd4-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="75fd4-244">A**バッファリング**ストリームが途中終了状態に入った 1 つ。 ストリームが不要になったエラーになるまでにすぐにバック プレーンに送信されるすべてのメッセージはエラー ストリームが途中終了状態のときは、にします。</span><span class="sxs-lookup"><span data-stu-id="75fd4-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="75fd4-245">**Send Queue Length**は送信されているが、まだ送信されるメッセージの数です。</span><span class="sxs-lookup"><span data-stu-id="75fd4-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="75fd4-246">**スケール アウト メッセージ バス メッセージの受信/秒**</span><span class="sxs-lookup"><span data-stu-id="75fd4-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="75fd4-247">**スケール アウトのストリームの合計**</span><span class="sxs-lookup"><span data-stu-id="75fd4-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="75fd4-248">**スケール アウトのストリームを開く**</span><span class="sxs-lookup"><span data-stu-id="75fd4-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="75fd4-249">**スケール アウト ストリームのバッファリング**</span><span class="sxs-lookup"><span data-stu-id="75fd4-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="75fd4-250">**スケール アウト エラーの合計**</span><span class="sxs-lookup"><span data-stu-id="75fd4-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="75fd4-251">**スケール アウト エラー/秒**</span><span class="sxs-lookup"><span data-stu-id="75fd4-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="75fd4-252">**スケール アウト送信キューの長さ**</span><span class="sxs-lookup"><span data-stu-id="75fd4-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="75fd4-253">これらのカウンターの測定の詳細については、次を参照してください。 [Azure Service Bus による SignalR スケール アウト](scaleout-with-windows-azure-service-bus.md)します。</span><span class="sxs-lookup"><span data-stu-id="75fd4-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="75fd4-254">その他のパフォーマンス カウンターの使用</span><span class="sxs-lookup"><span data-stu-id="75fd4-254">Using other performance counters</span></span>

<span data-ttu-id="75fd4-255">次のパフォーマンス カウンターも、アプリケーションのパフォーマンスの監視に役立つ場合があります。</span><span class="sxs-lookup"><span data-stu-id="75fd4-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="75fd4-256">**メモリ**</span><span class="sxs-lookup"><span data-stu-id="75fd4-256">**Memory**</span></span>

- <span data-ttu-id="75fd4-257">.NET CLR Memory\\ヒープ (w3wp) をすべて # bytes</span><span class="sxs-lookup"><span data-stu-id="75fd4-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="75fd4-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="75fd4-258">**ASP.NET**</span></span>

- <span data-ttu-id="75fd4-259">ASP.NET\Requests Current</span><span class="sxs-lookup"><span data-stu-id="75fd4-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="75fd4-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="75fd4-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="75fd4-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="75fd4-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="75fd4-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="75fd4-262">**CPU**</span></span>

- <span data-ttu-id="75fd4-263">プロセッサ時間の Information\Processor</span><span class="sxs-lookup"><span data-stu-id="75fd4-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="75fd4-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="75fd4-264">**TCP/IP**</span></span>

- <span data-ttu-id="75fd4-265">TCPv6/接続の確立</span><span class="sxs-lookup"><span data-stu-id="75fd4-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="75fd4-266">TCPv4/接続の確立</span><span class="sxs-lookup"><span data-stu-id="75fd4-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="75fd4-267">**Web サービス**</span><span class="sxs-lookup"><span data-stu-id="75fd4-267">**Web Service**</span></span>

- <span data-ttu-id="75fd4-268">Web service \current Connections</span><span class="sxs-lookup"><span data-stu-id="75fd4-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="75fd4-269">Web Service\Maximum 接続</span><span class="sxs-lookup"><span data-stu-id="75fd4-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="75fd4-270">**スレッド化**</span><span class="sxs-lookup"><span data-stu-id="75fd4-270">**Threading**</span></span>

- <span data-ttu-id="75fd4-271">.NET CLR をロックおよびスレッド\\現在の論理スレッド数</span><span class="sxs-lookup"><span data-stu-id="75fd4-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="75fd4-272">.NET CLR をロックおよびスレッド\\物理的な現在のスレッドの数</span><span class="sxs-lookup"><span data-stu-id="75fd4-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="75fd4-273">その他の参照情報</span><span class="sxs-lookup"><span data-stu-id="75fd4-273">Other Resources</span></span>

<span data-ttu-id="75fd4-274">ASP.NET パフォーマンスの監視とチューニングの詳細については、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="75fd4-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="75fd4-275">[ASP.NET のパフォーマンスの概要](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="75fd4-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="75fd4-276">IIS 7.5、IIS 7.0、IIS 6.0 で ASP.NET のスレッドの使用状況</span><span class="sxs-lookup"><span data-stu-id="75fd4-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="75fd4-277">&lt;applicationPool&gt;要素 (Web 設定)</span><span class="sxs-lookup"><span data-stu-id="75fd4-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
