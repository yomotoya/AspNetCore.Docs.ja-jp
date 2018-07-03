---
uid: signalr/overview/older-versions/signalr-performance
title: SignalR パフォーマンス (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: SignalR パフォーマンス
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: bb5bc29306ad94597239fa6d366be231ab1e86e1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362477"
---
<a name="signalr-performance-signalr-1x"></a><span data-ttu-id="4b69a-103">SignalR パフォーマンス (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="4b69a-103">SignalR Performance (SignalR 1.x)</span></span>
====================
<span data-ttu-id="4b69a-104">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="4b69a-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="4b69a-105">このトピックでは、ための設計、計測、および SignalR アプリケーションのパフォーマンスを向上する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>


<span data-ttu-id="4b69a-106">SignalR パフォーマンスとスケーリングに最近行ったプレゼンテーションは、次を参照してください。 [ASP.NET SignalR によるリアルタイムの Web スケール](https://channel9.msdn.com/Events/Build/2013/3-502)します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="4b69a-107">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="4b69a-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="4b69a-108">設計に関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="4b69a-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="4b69a-109">SignalR サーバーのパフォーマンスのチューニング</span><span class="sxs-lookup"><span data-stu-id="4b69a-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="4b69a-110">パフォーマンスの問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="4b69a-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="4b69a-111">SignalR パフォーマンス カウンターの使用</span><span class="sxs-lookup"><span data-stu-id="4b69a-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="4b69a-112">その他のパフォーマンス カウンターの使用</span><span class="sxs-lookup"><span data-stu-id="4b69a-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="4b69a-113">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="4b69a-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="4b69a-114">設計に関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="4b69a-114">Design considerations</span></span>

<span data-ttu-id="4b69a-115">このセクションでは、不要なネットワーク トラフィックを生成することによってパフォーマンスが低下しないことを確認するには SignalR アプリケーションのデザイン時に実装できるパターンについて説明します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="4b69a-116">メッセージの頻度を調整</span><span class="sxs-lookup"><span data-stu-id="4b69a-116">Throttling message frequency</span></span>

<span data-ttu-id="4b69a-117">を (リアルタイムのゲーム アプリケーションの場合) などの高頻度でメッセージを送信するアプリケーションであっても、ほとんどのアプリケーションは、1 秒間に複数のメッセージを送信する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="4b69a-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="4b69a-118">各クライアントを生成するトラフィックの量を減らすためには、メッセージ ループを実装するキューおよび送信メッセージありませんより固定レートよりも頻繁に (つまり、メッセージ数まで送信されます 1 秒ごとに、その時点でのメッセージがある場合terval 送信する)。</span><span class="sxs-lookup"><span data-stu-id="4b69a-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="4b69a-119">メッセージを (クライアントとサーバーの両方) から、一定のレートを調整するサンプル アプリケーションの場合、次を参照してください。 [SignalR による高頻度リアルタイム メッセージング](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="4b69a-120">メッセージのサイズを縮小します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-120">Reducing message size</span></span>

<span data-ttu-id="4b69a-121">SignalR メッセージのサイズを小さくには、シリアル化されたオブジェクトのサイズを小さきます。</span><span class="sxs-lookup"><span data-stu-id="4b69a-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="4b69a-122">サーバー コードでは、転送する必要がないプロパティを格納しているオブジェクトを送信する場合は妨げられるこれらのプロパティを使用してシリアル化される、`JsonIgnore`属性。</span><span class="sxs-lookup"><span data-stu-id="4b69a-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="4b69a-123">プロパティの名前がメッセージにも格納されています。使用してプロパティの名前を短縮できます、`JsonProperty`属性。</span><span class="sxs-lookup"><span data-stu-id="4b69a-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="4b69a-124">次のコード サンプルでは、プロパティ名を短縮する方法と、クライアントに送信されてからプロパティを除外する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="4b69a-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="4b69a-125">**.NET サーバー コードから、クライアントに送信されるデータを除外する JsonIgnore 属性とメッセージのサイズを小さく JsonProperty 属性を説明します。**</span><span class="sxs-lookup"><span data-stu-id="4b69a-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="4b69a-126">読みやすさを維持するために、クライアント コードで保守性、プロパティの省略名は、わかりやすいにマップが変更された名、メッセージが受信されるとします。</span><span class="sxs-lookup"><span data-stu-id="4b69a-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="4b69a-127">次のコード サンプルに示します (マッピング) のメッセージ コントラクトを定義することより長いものを使用する簡略名を再マップの方法の 1 つを使用して、`reMap`最適化されたメッセージ クラスに、コントラクトを適用する関数。</span><span class="sxs-lookup"><span data-stu-id="4b69a-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="4b69a-128">**クライアント側の JavaScript コードを再配置するには、人間が判読できる名前のプロパティ名を短縮します。**</span><span class="sxs-lookup"><span data-stu-id="4b69a-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="4b69a-129">名前は、同じメソッドを使用しても、サーバーに、クライアントからのメッセージで短縮できます。</span><span class="sxs-lookup"><span data-stu-id="4b69a-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="4b69a-130">(つまり、メッセージに使用されるメモリ量) のメモリ使用量の削減、メッセージのオブジェクトもパフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="4b69a-131">たとえば場合は、さまざまな、`int`は必要ありません、`short`または`byte`代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="4b69a-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="4b69a-132">メッセージがメッセージのサイズを減らし、サーバーのメモリにメッセージ バス内で格納されるためは、サーバー メモリの問題を解決でこともできます。</span><span class="sxs-lookup"><span data-stu-id="4b69a-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="4b69a-133">SignalR サーバーのパフォーマンスのチューニング</span><span class="sxs-lookup"><span data-stu-id="4b69a-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="4b69a-134">SignalR アプリケーションでパフォーマンスの向上のため、サーバーをチューニングするは、次の構成設定を使用できます。</span><span class="sxs-lookup"><span data-stu-id="4b69a-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="4b69a-135">ASP.NET アプリケーションのパフォーマンスを向上させる方法の概要については、次を参照してください。 [ASP.NET パフォーマンスの向上](https://msdn.microsoft.com/library/ff647787.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="4b69a-136">**SignalR の構成設定**</span><span class="sxs-lookup"><span data-stu-id="4b69a-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="4b69a-137">**DefaultMessageBufferSize**: 既定では、SignalR は接続ごとにハブあたりのメモリに 1000 メッセージを保持します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="4b69a-138">サイズの大きいメッセージを使用している場合は、この値を減らすことで緩和できるメモリの問題を作成この可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4b69a-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="4b69a-139">この設定を設定することができます、`Application_Start`または ASP.NET アプリケーションでのイベント ハンドラー、`Configuration`で自己ホスト型アプリケーションの OWIN startup クラスのメソッド。</span><span class="sxs-lookup"><span data-stu-id="4b69a-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="4b69a-140">次の例では、サーバーのメモリ使用量を削減するために、アプリケーションのメモリ使用量を減らすためにこの値を小さく方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="4b69a-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="4b69a-141">**既定のメッセージ バッファーのサイズを減らすため、Global.asax で .NET サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="4b69a-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="4b69a-142">**IIS の構成設定**</span><span class="sxs-lookup"><span data-stu-id="4b69a-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="4b69a-143">**アプリケーションごとの最大同時要求**: IIS の同時実行の数を増やして、要求で要求を提供するために使用可能なサーバー リソースが増加します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="4b69a-144">既定値は 5000 です。この設定を向上させるのに管理者特権でコマンド プロンプトで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="4b69a-145">**ASP.NET 構成の設定**</span><span class="sxs-lookup"><span data-stu-id="4b69a-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="4b69a-146">このセクションにはで設定できる構成設定が含まれています、`aspnet.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="4b69a-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="4b69a-147">このファイルはプラットフォームに応じて、2 つの場所のいずれかであります。</span><span class="sxs-lookup"><span data-stu-id="4b69a-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="4b69a-148">SignalR パフォーマンスを向上させることがあります ASP.NET の設定を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="4b69a-149">**CPU ごとの同時要求の最大**: この設定を増やすとパフォーマンスのボトルネックを軽減する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4b69a-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="4b69a-150">この設定を大きくには、次の構成設定を追加、`aspnet.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="4b69a-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="4b69a-151">**要求キューの制限**: 接続の合計数を超える場合、`maxConcurrentRequestsPerCPU`設定すると、ASP.NET は開始キューを使用して要求を調整します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="4b69a-152">キューのサイズを増やすを増やすことができます、`requestQueueLimit`設定します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="4b69a-153">これを行うには、次の構成設定を追加、`processModel`ノード`config/machine.config`(なく`aspnet.config`)。</span><span class="sxs-lookup"><span data-stu-id="4b69a-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="4b69a-154">パフォーマンスの問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="4b69a-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="4b69a-155">このセクションでは、アプリケーションのパフォーマンスのボトルネックを見つける方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="4b69a-156">WebSocket が使用されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="4b69a-157">SignalR は、さまざまなトランスポートを使用して、クライアントとサーバー間の通信、WebSocket は、パフォーマンスに大きなメリットが提供し、クライアントとサーバーがサポートしている場合に使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b69a-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="4b69a-158">調べるには、クライアントとサーバーが WebSocket の要件を満たしているかどうかは、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="4b69a-159">どのようなトランスポートは、アプリケーションで使用されているを確認するのには、ブラウザー開発者ツールを使用し、トランスポートが接続に使用されているログを調べてできます。</span><span class="sxs-lookup"><span data-stu-id="4b69a-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="4b69a-160">Internet Explorer と Chrome ブラウザーの開発ツールを使用する方法の詳細については、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="4b69a-161">SignalR パフォーマンス カウンターの使用</span><span class="sxs-lookup"><span data-stu-id="4b69a-161">Using SignalR performance counters</span></span>

<span data-ttu-id="4b69a-162">有効にして、SignalR パフォーマンス カウンターを使用する方法を説明にある、`Microsoft.AspNet.SignalR.Utils`パッケージ。</span><span class="sxs-lookup"><span data-stu-id="4b69a-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="4b69a-163">Signalr.exe をインストールします。</span><span class="sxs-lookup"><span data-stu-id="4b69a-163">Installing signalr.exe</span></span>

<span data-ttu-id="4b69a-164">SignalR.exe というユーティリティを使用してサーバーには、パフォーマンス カウンターを追加できます。</span><span class="sxs-lookup"><span data-stu-id="4b69a-164">Peformance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="4b69a-165">このユーティリティをインストールするには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="4b69a-166">Visual Studio アプリケーションで次のように選択します**ツール**、**ライブラリ パッケージ マネージャー**、 **Manage NuGet Packages for Solution しています。**</span><span class="sxs-lookup"><span data-stu-id="4b69a-166">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="4b69a-167">検索**signalr.utils**インストールを選択します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="4b69a-168">パッケージをインストールするライセンス条項に同意します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="4b69a-169">SignalR.exe にインストールされる`<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="4b69a-170">SignalR.exe でパフォーマンス カウンターをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4b69a-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="4b69a-171">SignalR パフォーマンス カウンターをインストールするには、次のパラメーターを使用して、管理者特権でコマンド プロンプトで SignalR.exe を実行します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="4b69a-172">SignalR パフォーマンス カウンターを削除するには、次のパラメーターを使用して、管理者特権でコマンド プロンプトで SignalR.exe を実行します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="4b69a-173">SignalR パフォーマンス カウンター</span><span class="sxs-lookup"><span data-stu-id="4b69a-173">SignalR Performance counters</span></span>

<span data-ttu-id="4b69a-174">ユーティリティ パッケージは、次のパフォーマンス カウンターをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4b69a-174">The utilites package installs the following performance counters.</span></span> <span data-ttu-id="4b69a-175">"Total"カウンターは、最後のアプリケーション プールまたはサーバーを再起動するために、イベントの数を計測します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="4b69a-176">**接続メトリック**</span><span class="sxs-lookup"><span data-stu-id="4b69a-176">**Connection metrics**</span></span>

<span data-ttu-id="4b69a-177">次のメトリックは、発生する接続の有効期間イベントを測定します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="4b69a-178">詳細については、次を参照してください。[接続の有効期間イベントの処理と理解](../guide-to-the-api/handling-connection-lifetime-events.md)します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="4b69a-179">**接続されている接続**</span><span class="sxs-lookup"><span data-stu-id="4b69a-179">**Connections Connected**</span></span>
- <span data-ttu-id="4b69a-180">**接続の再接続**</span><span class="sxs-lookup"><span data-stu-id="4b69a-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="4b69a-181">**接続外して**</span><span class="sxs-lookup"><span data-stu-id="4b69a-181">**Connections Disonnected**</span></span>
- <span data-ttu-id="4b69a-182">**現在の接続**</span><span class="sxs-lookup"><span data-stu-id="4b69a-182">**Connections Current**</span></span>

<span data-ttu-id="4b69a-183">**メッセージ メトリックス**</span><span class="sxs-lookup"><span data-stu-id="4b69a-183">**Message metrics**</span></span>

<span data-ttu-id="4b69a-184">次のメトリックは、SignalR によって生成されるメッセージ トラフィックを測定します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="4b69a-185">**接続メッセージの受信した合計**</span><span class="sxs-lookup"><span data-stu-id="4b69a-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="4b69a-186">**合計送信接続メッセージ**</span><span class="sxs-lookup"><span data-stu-id="4b69a-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="4b69a-187">**接続を受信メッセージ数/秒**</span><span class="sxs-lookup"><span data-stu-id="4b69a-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="4b69a-188">**接続が送信メッセージ/秒**</span><span class="sxs-lookup"><span data-stu-id="4b69a-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="4b69a-189">**メッセージ バスのメトリック**</span><span class="sxs-lookup"><span data-stu-id="4b69a-189">**Message bus metrics**</span></span>

<span data-ttu-id="4b69a-190">次のメトリックは、内部の SignalR メッセージ バス、すべての受信および送信の SignalR メッセージの配置のキューを通過するトラフィックを測定します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="4b69a-191">メッセージが**Published**が送信またはブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="4b69a-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="4b69a-192">A**サブスクライバー**メッセージ バス上のサブスクリプションは、このコンテキストでは、クライアントとサーバー自体の数と等しくなります。</span><span class="sxs-lookup"><span data-stu-id="4b69a-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="4b69a-193">**割り当てられたワーカー** ; のアクティブな接続にデータを送信するコンポーネントを**ビジー状態の Worker**がアクティブにメッセージを送信する 1 つです。</span><span class="sxs-lookup"><span data-stu-id="4b69a-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="4b69a-194">**メッセージ バス メッセージの受信の合計数**</span><span class="sxs-lookup"><span data-stu-id="4b69a-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="4b69a-195">**メッセージ バス メッセージの受信/秒**</span><span class="sxs-lookup"><span data-stu-id="4b69a-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="4b69a-196">**メッセージ バスのメッセージの合計を発行します。**</span><span class="sxs-lookup"><span data-stu-id="4b69a-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="4b69a-197">**メッセージ バス メッセージの 1 秒あたりの発行**</span><span class="sxs-lookup"><span data-stu-id="4b69a-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="4b69a-198">**メッセージ バスのサブスクライバーの現在**</span><span class="sxs-lookup"><span data-stu-id="4b69a-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="4b69a-199">**メッセージ バスのサブスクライバーの合計**</span><span class="sxs-lookup"><span data-stu-id="4b69a-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="4b69a-200">**メッセージ バスのサブスクライバー/秒**</span><span class="sxs-lookup"><span data-stu-id="4b69a-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="4b69a-201">**メッセージ バスには、ワーカーが割り当てられています。**</span><span class="sxs-lookup"><span data-stu-id="4b69a-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="4b69a-202">**メッセージ バスのビジー状態のワーカー**</span><span class="sxs-lookup"><span data-stu-id="4b69a-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="4b69a-203">**メッセージ バスのトピックの現在**</span><span class="sxs-lookup"><span data-stu-id="4b69a-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="4b69a-204">**誤差のメトリック**</span><span class="sxs-lookup"><span data-stu-id="4b69a-204">**Error metrics**</span></span>

<span data-ttu-id="4b69a-205">次のメトリックは、SignalR メッセージ トラフィックによって生成されたエラーを測定します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="4b69a-206">**ハブの解決**ハブまたはハブのメソッドは解決できない場合にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="4b69a-207">**ハブ呼び出し**エラーは、ハブ メソッドの呼び出し中にスローされる例外。</span><span class="sxs-lookup"><span data-stu-id="4b69a-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="4b69a-208">**トランスポート**エラーは、接続エラーの HTTP 要求または応答時にスローされます。</span><span class="sxs-lookup"><span data-stu-id="4b69a-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="4b69a-209">**エラー: すべての合計**</span><span class="sxs-lookup"><span data-stu-id="4b69a-209">**Errors: All Total**</span></span>
- <span data-ttu-id="4b69a-210">**エラー: 1 秒あたりのすべて**</span><span class="sxs-lookup"><span data-stu-id="4b69a-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="4b69a-211">**エラー: ハブの解像度の合計**</span><span class="sxs-lookup"><span data-stu-id="4b69a-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="4b69a-212">**エラー: ハブの解像度/秒**</span><span class="sxs-lookup"><span data-stu-id="4b69a-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="4b69a-213">**エラー: ハブ呼び出し合計**</span><span class="sxs-lookup"><span data-stu-id="4b69a-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="4b69a-214">**エラー: ハブ呼び出し/秒**</span><span class="sxs-lookup"><span data-stu-id="4b69a-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="4b69a-215">**エラー: トランスポートの合計**</span><span class="sxs-lookup"><span data-stu-id="4b69a-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="4b69a-216">**1 秒あたりのトランスポート エラー:**</span><span class="sxs-lookup"><span data-stu-id="4b69a-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="4b69a-217">**スケール アウトのメトリック**</span><span class="sxs-lookup"><span data-stu-id="4b69a-217">**Scaleout metrics**</span></span>

<span data-ttu-id="4b69a-218">次のメトリックは、トラフィックと、スケール アウト プロバイダーによって生成されたエラーを測定します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="4b69a-219">A **Stream**このコンテキストでは、スケール ユニットはスケール アウト プロバイダーで使用される。 これは SQL Server を使用する場合は、テーブル、Service Bus を使用する場合は、トピックとサブスクリプション Redis を使用する場合。</span><span class="sxs-lookup"><span data-stu-id="4b69a-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="4b69a-220">既定では、1 つのみのストリームを使用するが、これは、SQL サーバーと Service Bus の構成を増やすことができます。</span><span class="sxs-lookup"><span data-stu-id="4b69a-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="4b69a-221">A**バッファリング**ストリームが途中終了状態に入った 1 つ。 ストリームが不要になったエラーになるまでにすぐにバック プレーンに送信されるすべてのメッセージはエラー ストリームが途中終了状態のときは、にします。</span><span class="sxs-lookup"><span data-stu-id="4b69a-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="4b69a-222">**Send Queue Length**は送信されているが、まだ送信されるメッセージの数です。</span><span class="sxs-lookup"><span data-stu-id="4b69a-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="4b69a-223">**スケール アウト メッセージ バス メッセージの受信/秒**</span><span class="sxs-lookup"><span data-stu-id="4b69a-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="4b69a-224">**スケール アウトのストリームの合計**</span><span class="sxs-lookup"><span data-stu-id="4b69a-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="4b69a-225">**スケール アウトのストリームを開く**</span><span class="sxs-lookup"><span data-stu-id="4b69a-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="4b69a-226">**スケール アウト ストリームのバッファリング**</span><span class="sxs-lookup"><span data-stu-id="4b69a-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="4b69a-227">**スケール アウト エラーの合計**</span><span class="sxs-lookup"><span data-stu-id="4b69a-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="4b69a-228">**スケール アウト エラー/秒**</span><span class="sxs-lookup"><span data-stu-id="4b69a-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="4b69a-229">**スケール アウト送信キューの長さ**</span><span class="sxs-lookup"><span data-stu-id="4b69a-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="4b69a-230">これらのカウンターの測定の詳細については、次を参照してください。 [Azure Service Bus による SignalR スケール アウト](scaleout-with-windows-azure-service-bus.md)します。</span><span class="sxs-lookup"><span data-stu-id="4b69a-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="4b69a-231">その他のパフォーマンス カウンターの使用</span><span class="sxs-lookup"><span data-stu-id="4b69a-231">Using other performance counters</span></span>

<span data-ttu-id="4b69a-232">次のパフォーマンス カウンターも、アプリケーションのパフォーマンスの監視に役立つ場合があります。</span><span class="sxs-lookup"><span data-stu-id="4b69a-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="4b69a-233">**メモリ**</span><span class="sxs-lookup"><span data-stu-id="4b69a-233">**Memory**</span></span>

- <span data-ttu-id="4b69a-234">.NET CLR メモリとヒープ (w3wp) のすべてのバイト数</span><span class="sxs-lookup"><span data-stu-id="4b69a-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="4b69a-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="4b69a-235">**ASP.NET**</span></span>

- <span data-ttu-id="4b69a-236">ASP.NET\Requests Current</span><span class="sxs-lookup"><span data-stu-id="4b69a-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="4b69a-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="4b69a-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="4b69a-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="4b69a-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="4b69a-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="4b69a-239">**CPU**</span></span>

- <span data-ttu-id="4b69a-240">プロセッサ時間の Information\Processor</span><span class="sxs-lookup"><span data-stu-id="4b69a-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="4b69a-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="4b69a-241">**TCP/IP**</span></span>

- <span data-ttu-id="4b69a-242">TCPv6/接続の確立</span><span class="sxs-lookup"><span data-stu-id="4b69a-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="4b69a-243">TCPv4/接続の確立</span><span class="sxs-lookup"><span data-stu-id="4b69a-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="4b69a-244">**Web サービス**</span><span class="sxs-lookup"><span data-stu-id="4b69a-244">**Web Service**</span></span>

- <span data-ttu-id="4b69a-245">Web service \current Connections</span><span class="sxs-lookup"><span data-stu-id="4b69a-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="4b69a-246">Web Service\Maximum 接続</span><span class="sxs-lookup"><span data-stu-id="4b69a-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="4b69a-247">**スレッド化**</span><span class="sxs-lookup"><span data-stu-id="4b69a-247">**Threading**</span></span>

- <span data-ttu-id="4b69a-248">.NET CLR LocksAndThreads\#の現在の論理スレッド</span><span class="sxs-lookup"><span data-stu-id="4b69a-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="4b69a-249">.NET CLR LocksAnd スレッド\#物理スレッドの現在の</span><span class="sxs-lookup"><span data-stu-id="4b69a-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="4b69a-250">その他の参照情報</span><span class="sxs-lookup"><span data-stu-id="4b69a-250">Other Resources</span></span>

<span data-ttu-id="4b69a-251">ASP.NET パフォーマンスの監視とチューニングの詳細については、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4b69a-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="4b69a-252">[ASP.NET のパフォーマンスの概要](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="4b69a-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="4b69a-253">IIS 7.5、IIS 7.0、IIS 6.0 で ASP.NET のスレッドの使用状況</span><span class="sxs-lookup"><span data-stu-id="4b69a-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="4b69a-254">&lt;applicationPool&gt;要素 (Web 設定)</span><span class="sxs-lookup"><span data-stu-id="4b69a-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
