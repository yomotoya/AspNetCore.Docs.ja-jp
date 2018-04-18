---
uid: signalr/overview/older-versions/signalr-performance
title: SignalR パフォーマンス (SignalR 1.x) |Microsoft ドキュメント
author: pfletcher
description: SignalR パフォーマンス
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: bad742af28d6c36bb1b66207c2ba09d140332449
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="signalr-performance-signalr-1x"></a><span data-ttu-id="aa657-103">SignalR パフォーマンス (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="aa657-103">SignalR Performance (SignalR 1.x)</span></span>
====================
<span data-ttu-id="aa657-104">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="aa657-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="aa657-105">このトピックでは、用にデザイン、メジャー、および SignalR アプリケーションのパフォーマンスを向上させる方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="aa657-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>


<span data-ttu-id="aa657-106">SignalR パフォーマンスとスケーリングの最近のプレゼンテーションを参照してください。 [ASP.NET SignalR でリアルタイム Web をスケーリング](https://channel9.msdn.com/Events/Build/2013/3-502)です。</span><span class="sxs-lookup"><span data-stu-id="aa657-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="aa657-107">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="aa657-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="aa657-108">設計に関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="aa657-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="aa657-109">パフォーマンスのため、SignalR のサーバーのチューニング</span><span class="sxs-lookup"><span data-stu-id="aa657-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="aa657-110">パフォーマンスの問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="aa657-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="aa657-111">SignalR パフォーマンス カウンターを使用します。</span><span class="sxs-lookup"><span data-stu-id="aa657-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="aa657-112">他のパフォーマンス カウンターを使用します。</span><span class="sxs-lookup"><span data-stu-id="aa657-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="aa657-113">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="aa657-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="aa657-114">設計に関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="aa657-114">Design considerations</span></span>

<span data-ttu-id="aa657-115">このセクションでは、パフォーマンスの不必要なネットワーク トラフィックを生成することによって障害されていないことを確認する SignalR アプリケーションのデザイン時に実装できるパターンについて説明します。</span><span class="sxs-lookup"><span data-stu-id="aa657-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="aa657-116">メッセージの頻度を調整</span><span class="sxs-lookup"><span data-stu-id="aa657-116">Throttling message frequency</span></span>

<span data-ttu-id="aa657-117">(リアルタイムのゲーム アプリケーション) などの高頻度でメッセージを送信して、アプリケーションであっても、1 秒間に複数のメッセージを送信するほとんどのアプリケーションが不要です。</span><span class="sxs-lookup"><span data-stu-id="aa657-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="aa657-118">各クライアントを生成するトラフィックの量を減らすためには、メッセージ ループを実装できますキューおよび送信により頻繁に固定の率よりもメッセージないこと (つまり、最大メッセージ数は送信毎秒では、その期間中にメッセージがある場合terval 送信する)。</span><span class="sxs-lookup"><span data-stu-id="aa657-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="aa657-119">サンプル アプリケーション (クライアントとサーバーの両方) から特定のレートを調整してメッセージを次を参照してください。 [SignalR と頻度が高いリアルタイム](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)です。</span><span class="sxs-lookup"><span data-stu-id="aa657-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="aa657-120">メッセージのサイズを縮小します。</span><span class="sxs-lookup"><span data-stu-id="aa657-120">Reducing message size</span></span>

<span data-ttu-id="aa657-121">SignalR メッセージのサイズを小さくには、シリアル化されたオブジェクトのサイズを小さきます。</span><span class="sxs-lookup"><span data-stu-id="aa657-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="aa657-122">サーバー コードを送信する必要のないプロパティを格納するオブジェクトを送信する場合は妨げそれらのプロパティを使用してシリアル化される、`JsonIgnore`属性。</span><span class="sxs-lookup"><span data-stu-id="aa657-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="aa657-123">プロパティの名前がメッセージにも格納されています。使用して、プロパティの名前を簡略化、`JsonProperty`属性。</span><span class="sxs-lookup"><span data-stu-id="aa657-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="aa657-124">次のコード サンプルでは、プロパティ名を短縮する方法と、クライアントに送信されないプロパティを除外する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="aa657-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="aa657-125">**クライアントに送信されるデータを除外する JsonIgnore 属性とメッセージ サイズを縮小する JsonProperty 属性を示す .NET サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="aa657-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="aa657-126">読みやすさを保持するために、クライアント コードで保守性、省略形のプロパティ名は、わかりやすいに再割り当て名、メッセージの受信後に/です。</span><span class="sxs-lookup"><span data-stu-id="aa657-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="aa657-127">次のコード サンプルでは、メッセージ コントラクト (マッピング) を定義することで、長いパスワードを簡略化された名前を再マップの方法の 1 つを使用して、`reMap`最適化されたメッセージ クラスに、コントラクトを適用する関数。</span><span class="sxs-lookup"><span data-stu-id="aa657-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="aa657-128">**クライアント側の JavaScript コードを再配置を人間が判読できる名前にプロパティ名を短縮します。**</span><span class="sxs-lookup"><span data-stu-id="aa657-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="aa657-129">名前が同じメソッドを使用して同様に、サーバーにクライアントからのメッセージで短縮されることができます。</span><span class="sxs-lookup"><span data-stu-id="aa657-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="aa657-130">メモリ使用量 (つまり、メッセージで使用するメモリ量) の削減、メッセージのオブジェクトもパフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="aa657-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="aa657-131">たとえば場合のすべての範囲、`int`は必要ありません、`short`または`byte`代わりに使用されることができます。</span><span class="sxs-lookup"><span data-stu-id="aa657-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="aa657-132">メッセージのサイズを小さくする、サーバーのメモリ内メッセージ バス内でメッセージが格納されているためには、サーバー メモリの問題を解決でこともできます。</span><span class="sxs-lookup"><span data-stu-id="aa657-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="aa657-133">パフォーマンスのため、SignalR のサーバーのチューニング</span><span class="sxs-lookup"><span data-stu-id="aa657-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="aa657-134">次の構成設定は、SignalR アプリケーションでパフォーマンスを向上させるため、サーバーのチューニングに使用できます。</span><span class="sxs-lookup"><span data-stu-id="aa657-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="aa657-135">ASP.NET アプリケーションのパフォーマンスを向上させる方法の概要については、次を参照してください。 [ASP.NET のパフォーマンスを向上させる](https://msdn.microsoft.com/library/ff647787.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="aa657-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="aa657-136">**SignalR の構成設定**</span><span class="sxs-lookup"><span data-stu-id="aa657-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="aa657-137">**DefaultMessageBufferSize**: 既定では、SignalR には接続ごとのハブあたりのメモリ内の 1000 メッセージが保持されます。</span><span class="sxs-lookup"><span data-stu-id="aa657-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="aa657-138">サイズの大きいメッセージを使用している場合は、メモリの問題はこの値を減らすことによって軽減されることができますを作成この可能性があります。</span><span class="sxs-lookup"><span data-stu-id="aa657-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="aa657-139">この設定で設定することができます、`Application_Start`や ASP.NET アプリケーションでイベント ハンドラー、`Configuration`自己ホスト型アプリケーションでの OWIN スタートアップ クラスのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="aa657-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="aa657-140">次の例では、サーバーのメモリ使用量を削減するために、アプリケーションのメモリ使用量を減らすためにこの値を小さく方法を示します。</span><span class="sxs-lookup"><span data-stu-id="aa657-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="aa657-141">**Global.asax で既定のメッセージ バッファーのサイズを減らすための .NET サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="aa657-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="aa657-142">**IIS の構成設定**</span><span class="sxs-lookup"><span data-stu-id="aa657-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="aa657-143">**アプリケーションごとの最大同時要求**: IIS の同時実行の数を増やすと、要求で要求処理の使用可能なサーバー リソースは増加します。</span><span class="sxs-lookup"><span data-stu-id="aa657-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="aa657-144">既定値は 5000 です。この設定を増やすには、するには、管理者特権のコマンド プロンプトで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="aa657-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="aa657-145">**ASP.NET 構成の設定**</span><span class="sxs-lookup"><span data-stu-id="aa657-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="aa657-146">このセクションにはで設定できる構成設定が含まれています、`aspnet.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="aa657-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="aa657-147">このファイルにはプラットフォームによっては、2 つの場所のいずれかであります。</span><span class="sxs-lookup"><span data-stu-id="aa657-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="aa657-148">SignalR パフォーマンスを向上させることがあります ASP.NET の設定を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="aa657-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="aa657-149">**CPU ごとの最大の同時要求**: この設定を増やすとパフォーマンスのボトルネックを軽減する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="aa657-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="aa657-150">この設定を増やすには、するには、次の構成設定を追加、`aspnet.config`ファイル。</span><span class="sxs-lookup"><span data-stu-id="aa657-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="aa657-151">**要求キューの制限**: 接続の合計数を超えた場合、`maxConcurrentRequestsPerCPU`設定すると、ASP.NET は開始キューを使用して要求を調整します。</span><span class="sxs-lookup"><span data-stu-id="aa657-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="aa657-152">キューのサイズを増やすに増やすことができます、`requestQueueLimit`設定します。</span><span class="sxs-lookup"><span data-stu-id="aa657-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="aa657-153">これを行うには、次の構成設定を追加、`processModel`内のノード`config/machine.config`(なく`aspnet.config`)。</span><span class="sxs-lookup"><span data-stu-id="aa657-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="aa657-154">パフォーマンスの問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="aa657-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="aa657-155">このセクションでは、アプリケーションのパフォーマンスのボトルネックを検索する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="aa657-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="aa657-156">WebSocket が使用されていることを確認しています</span><span class="sxs-lookup"><span data-stu-id="aa657-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="aa657-157">SignalR は、さまざまなトランスポートを使用して、クライアントとサーバー間の通信、WebSocket 大幅なパフォーマンスの利点を提供し、クライアントとサーバーがサポートしている場合に使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa657-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="aa657-158">調べるには、クライアントとサーバーが、WebSocket の要件を満たすかどうかは、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)です。</span><span class="sxs-lookup"><span data-stu-id="aa657-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="aa657-159">調べるにはどのようなトランスポートは、アプリケーションで使用されて、ブラウザーの開発者ツールを使用し、どのようなトランスポートが接続に使用されているログを調べてできます。</span><span class="sxs-lookup"><span data-stu-id="aa657-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="aa657-160">Internet Explorer と Chrome ブラウザーの開発ツールの使用方法の詳細については、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)です。</span><span class="sxs-lookup"><span data-stu-id="aa657-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="aa657-161">SignalR パフォーマンス カウンターを使用します。</span><span class="sxs-lookup"><span data-stu-id="aa657-161">Using SignalR performance counters</span></span>

<span data-ttu-id="aa657-162">このセクションを有効にし、SignalR パフォーマンス カウンターを使用する方法について説明で見つかった、`Microsoft.AspNet.SignalR.Utils`パッケージです。</span><span class="sxs-lookup"><span data-stu-id="aa657-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="aa657-163">Signalr.exe をインストールします。</span><span class="sxs-lookup"><span data-stu-id="aa657-163">Installing signalr.exe</span></span>

<span data-ttu-id="aa657-164">パフォーマンス カウンターは、という SignalR.exe ユーティリティを使用してサーバーに追加できます。</span><span class="sxs-lookup"><span data-stu-id="aa657-164">Peformance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="aa657-165">このユーティリティをインストールするには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="aa657-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="aa657-166">Visual Studio アプリケーションで次のように選択します**ツール**、**ライブラリ パッケージ マネージャー**、 **Manage NuGet Packages for Solution しています...**</span><span class="sxs-lookup"><span data-stu-id="aa657-166">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="aa657-167">検索**signalr.utils**インストール を選択します。</span><span class="sxs-lookup"><span data-stu-id="aa657-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="aa657-168">パッケージをインストールするライセンス契約に同意します。</span><span class="sxs-lookup"><span data-stu-id="aa657-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="aa657-169">インストールされます SignalR.exe`<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`です。</span><span class="sxs-lookup"><span data-stu-id="aa657-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="aa657-170">SignalR.exe でパフォーマンス カウンターをインストールします。</span><span class="sxs-lookup"><span data-stu-id="aa657-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="aa657-171">SignalR パフォーマンス カウンターをインストールするには、次のパラメーターを使用して特権のコマンド プロンプトで SignalR.exe を実行します。</span><span class="sxs-lookup"><span data-stu-id="aa657-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="aa657-172">SignalR パフォーマンス カウンターを削除するには、次のパラメーターを使用して特権のコマンド プロンプトで SignalR.exe を実行します。</span><span class="sxs-lookup"><span data-stu-id="aa657-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="aa657-173">SignalR パフォーマンス カウンター</span><span class="sxs-lookup"><span data-stu-id="aa657-173">SignalR Performance counters</span></span>

<span data-ttu-id="aa657-174">ユーティリティ パッケージでは、次のパフォーマンス カウンターをインストールします。</span><span class="sxs-lookup"><span data-stu-id="aa657-174">The utilites package installs the following performance counters.</span></span> <span data-ttu-id="aa657-175">"Total"カウンターは、最後のアプリケーション プールまたはサーバーが再起動した後に、イベントの数を計測します。</span><span class="sxs-lookup"><span data-stu-id="aa657-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="aa657-176">**接続のメトリック**</span><span class="sxs-lookup"><span data-stu-id="aa657-176">**Connection metrics**</span></span>

<span data-ttu-id="aa657-177">次のメトリックは、接続の有効期間発生するイベントを測定します。</span><span class="sxs-lookup"><span data-stu-id="aa657-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="aa657-178">詳細については、次を参照してください。[接続の有効期間イベントの処理と理解](../guide-to-the-api/handling-connection-lifetime-events.md)です。</span><span class="sxs-lookup"><span data-stu-id="aa657-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="aa657-179">**接続されている接続**</span><span class="sxs-lookup"><span data-stu-id="aa657-179">**Connections Connected**</span></span>
- <span data-ttu-id="aa657-180">**接続の再接続**</span><span class="sxs-lookup"><span data-stu-id="aa657-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="aa657-181">**接続外して**</span><span class="sxs-lookup"><span data-stu-id="aa657-181">**Connections Disonnected**</span></span>
- <span data-ttu-id="aa657-182">**現在の接続**</span><span class="sxs-lookup"><span data-stu-id="aa657-182">**Connections Current**</span></span>

<span data-ttu-id="aa657-183">**メッセージ メトリックス**</span><span class="sxs-lookup"><span data-stu-id="aa657-183">**Message metrics**</span></span>

<span data-ttu-id="aa657-184">次のメトリックは、SignalR によって生成されるメッセージ トラフィックを測定します。</span><span class="sxs-lookup"><span data-stu-id="aa657-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="aa657-185">**接続のメッセージの受信合計**</span><span class="sxs-lookup"><span data-stu-id="aa657-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="aa657-186">**合計送信接続メッセージ**</span><span class="sxs-lookup"><span data-stu-id="aa657-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="aa657-187">**接続を受信メッセージ数/秒**</span><span class="sxs-lookup"><span data-stu-id="aa657-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="aa657-188">**接続が送信メッセージ/秒**</span><span class="sxs-lookup"><span data-stu-id="aa657-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="aa657-189">**メッセージ バスのメトリック**</span><span class="sxs-lookup"><span data-stu-id="aa657-189">**Message bus metrics**</span></span>

<span data-ttu-id="aa657-190">次のメトリックは、内部の SignalR メッセージ バスは、すべての着信および発信の SignalR メッセージが置かれているキューを通過するトラフィックを測定します。</span><span class="sxs-lookup"><span data-stu-id="aa657-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="aa657-191">メッセージが**Published**が送信またはブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="aa657-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="aa657-192">A**サブスクライバー**このコンテキストでメッセージ バス上のサブスクリプションはクライアントとサーバー自体の数と等しくなります。</span><span class="sxs-lookup"><span data-stu-id="aa657-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="aa657-193">**割り当てられたワーカー** ; のアクティブな接続にデータを送信するコンポーネント、**ビジー状態のワーカー**は、アクティブにメッセージを送信しています。</span><span class="sxs-lookup"><span data-stu-id="aa657-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="aa657-194">**メッセージ バス メッセージを受信した合計**</span><span class="sxs-lookup"><span data-stu-id="aa657-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="aa657-195">**メッセージ バスのメッセージの受信/秒**</span><span class="sxs-lookup"><span data-stu-id="aa657-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="aa657-196">**メッセージ バスのメッセージの合計を発行します。**</span><span class="sxs-lookup"><span data-stu-id="aa657-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="aa657-197">**メッセージ バスのメッセージの発行/秒**</span><span class="sxs-lookup"><span data-stu-id="aa657-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="aa657-198">**メッセージ バスのサブスクライバーの現在**</span><span class="sxs-lookup"><span data-stu-id="aa657-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="aa657-199">**メッセージ バスのサブスクライバーの合計**</span><span class="sxs-lookup"><span data-stu-id="aa657-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="aa657-200">**メッセージ バスのサブスクライバー/秒**</span><span class="sxs-lookup"><span data-stu-id="aa657-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="aa657-201">**メッセージ バスには、ワーカーが割り当てられています。**</span><span class="sxs-lookup"><span data-stu-id="aa657-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="aa657-202">**メッセージ バスのビジー状態のワーカー**</span><span class="sxs-lookup"><span data-stu-id="aa657-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="aa657-203">**メッセージ バスのトピックの現在**</span><span class="sxs-lookup"><span data-stu-id="aa657-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="aa657-204">**誤差のメトリック**</span><span class="sxs-lookup"><span data-stu-id="aa657-204">**Error metrics**</span></span>

<span data-ttu-id="aa657-205">次のメトリックは、SignalR メッセージ トラフィックによって生成されたエラーを測定します。</span><span class="sxs-lookup"><span data-stu-id="aa657-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="aa657-206">**ハブの解決**ハブまたはハブのメソッドは解決できない場合にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="aa657-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="aa657-207">**ハブ呼び出し**エラーは、ハブ メソッドの呼び出し中にスローされる例外。</span><span class="sxs-lookup"><span data-stu-id="aa657-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="aa657-208">**トランスポート**エラーは、接続エラーの HTTP 要求または応答の中にスローされます。</span><span class="sxs-lookup"><span data-stu-id="aa657-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="aa657-209">**エラー: すべての合計**</span><span class="sxs-lookup"><span data-stu-id="aa657-209">**Errors: All Total**</span></span>
- <span data-ttu-id="aa657-210">**エラー: 1 秒あたりのすべて**</span><span class="sxs-lookup"><span data-stu-id="aa657-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="aa657-211">**エラー: ハブ解像度の合計**</span><span class="sxs-lookup"><span data-stu-id="aa657-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="aa657-212">**エラー: ハブ解像度/秒**</span><span class="sxs-lookup"><span data-stu-id="aa657-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="aa657-213">**エラー: ハブ呼び出しの合計**</span><span class="sxs-lookup"><span data-stu-id="aa657-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="aa657-214">**エラー: ハブ呼び出し/秒**</span><span class="sxs-lookup"><span data-stu-id="aa657-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="aa657-215">**エラー: トランスポート合計**</span><span class="sxs-lookup"><span data-stu-id="aa657-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="aa657-216">**1 秒あたりのトランスポートのエラー:**</span><span class="sxs-lookup"><span data-stu-id="aa657-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="aa657-217">**スケール アウト メトリック**</span><span class="sxs-lookup"><span data-stu-id="aa657-217">**Scaleout metrics**</span></span>

<span data-ttu-id="aa657-218">次のメトリックは、トラフィックおよびスケール アウト プロバイダーによって生成されたエラーを測定します。</span><span class="sxs-lookup"><span data-stu-id="aa657-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="aa657-219">A**ストリーム**このコンテキストでは、スケール アウト プロバイダーによって、スケール ユニットを使用。 これは SQL Server が使用される場合は、テーブル、Service Bus を使用する場合のトピックとサブスクリプション Redis を使用する場合。</span><span class="sxs-lookup"><span data-stu-id="aa657-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="aa657-220">既定では、1 つだけのストリームを使用するが、これは、SQL Server と Service Bus の構成を増やすことができます。</span><span class="sxs-lookup"><span data-stu-id="aa657-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="aa657-221">A**バッファリング**ストリームが途中終了状態になったものです。 ストリームが不要になったエラーが発生するまでにすぐに、ストリームが途中終了状態、バック プレーンに送信されるすべてのメッセージは失敗します。</span><span class="sxs-lookup"><span data-stu-id="aa657-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="aa657-222">**Send Queue Length**送信されているが、まだ送信されるメッセージの数です。</span><span class="sxs-lookup"><span data-stu-id="aa657-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="aa657-223">**スケール アウト メッセージ バスのメッセージの受信/秒**</span><span class="sxs-lookup"><span data-stu-id="aa657-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="aa657-224">**スケール アウト ストリームの合計**</span><span class="sxs-lookup"><span data-stu-id="aa657-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="aa657-225">**スケール アウトのストリームを開く**</span><span class="sxs-lookup"><span data-stu-id="aa657-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="aa657-226">**スケール アウト ストリームのバッファリング**</span><span class="sxs-lookup"><span data-stu-id="aa657-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="aa657-227">**スケール アウト エラーの合計**</span><span class="sxs-lookup"><span data-stu-id="aa657-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="aa657-228">**スケール アウト エラー/秒**</span><span class="sxs-lookup"><span data-stu-id="aa657-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="aa657-229">**スケール アウト送信キューの長さ**</span><span class="sxs-lookup"><span data-stu-id="aa657-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="aa657-230">これらのカウンターの測定の詳細については、次を参照してください。 [Azure Service Bus での SignalR スケール アウト](scaleout-with-windows-azure-service-bus.md)です。</span><span class="sxs-lookup"><span data-stu-id="aa657-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="aa657-231">他のパフォーマンス カウンターを使用します。</span><span class="sxs-lookup"><span data-stu-id="aa657-231">Using other performance counters</span></span>

<span data-ttu-id="aa657-232">次のパフォーマンス カウンターは、アプリケーションのパフォーマンスの監視に役立つ場合もあります。</span><span class="sxs-lookup"><span data-stu-id="aa657-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="aa657-233">**メモリ**</span><span class="sxs-lookup"><span data-stu-id="aa657-233">**Memory**</span></span>

- <span data-ttu-id="aa657-234">.NET CLR メモリ # ヒープ (w3wp) をすべてのバイト数</span><span class="sxs-lookup"><span data-stu-id="aa657-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="aa657-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="aa657-235">**ASP.NET**</span></span>

- <span data-ttu-id="aa657-236">ASP.NET\Requests Current</span><span class="sxs-lookup"><span data-stu-id="aa657-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="aa657-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="aa657-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="aa657-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="aa657-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="aa657-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="aa657-239">**CPU**</span></span>

- <span data-ttu-id="aa657-240">プロセッサ時間の Information\Processor</span><span class="sxs-lookup"><span data-stu-id="aa657-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="aa657-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="aa657-241">**TCP/IP**</span></span>

- <span data-ttu-id="aa657-242">TCPv6/接続の確立</span><span class="sxs-lookup"><span data-stu-id="aa657-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="aa657-243">TCPv4/接続の確立</span><span class="sxs-lookup"><span data-stu-id="aa657-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="aa657-244">**Web サービス**</span><span class="sxs-lookup"><span data-stu-id="aa657-244">**Web Service**</span></span>

- <span data-ttu-id="aa657-245">Web service \current Connections</span><span class="sxs-lookup"><span data-stu-id="aa657-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="aa657-246">Web Service\Maximum 接続</span><span class="sxs-lookup"><span data-stu-id="aa657-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="aa657-247">**スレッド化**</span><span class="sxs-lookup"><span data-stu-id="aa657-247">**Threading**</span></span>

- <span data-ttu-id="aa657-248">.NET CLR LocksAndThreads\#論理スレッドの現在の</span><span class="sxs-lookup"><span data-stu-id="aa657-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="aa657-249">.NET CLR LocksAnd スレッド\#物理スレッドの現在の</span><span class="sxs-lookup"><span data-stu-id="aa657-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="aa657-250">その他の参照情報</span><span class="sxs-lookup"><span data-stu-id="aa657-250">Other Resources</span></span>

<span data-ttu-id="aa657-251">ASP.NET のパフォーマンスの監視とチューニングの詳細については、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="aa657-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="aa657-252">[ASP.NET のパフォーマンスの概要](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="aa657-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="aa657-253">IIS 7.5、IIS 7.0 および IIS 6.0 に ASP.NET スレッドの使用状況</span><span class="sxs-lookup"><span data-stu-id="aa657-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="aa657-254">&lt;applicationPool&gt;要素 (Web 設定)</span><span class="sxs-lookup"><span data-stu-id="aa657-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
