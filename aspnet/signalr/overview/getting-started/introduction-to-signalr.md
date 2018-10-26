---
uid: signalr/overview/getting-started/introduction-to-signalr
title: SignalR の概要 |Microsoft Docs
author: pfletcher
description: この記事では、SignalR は、いくつかのソリューションを作成するため、デザインされたについて説明します。
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0b7e223b6b793d1860797157be6021ffb7f1bc12
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090294"
---
<a name="introduction-to-signalr"></a><span data-ttu-id="5952d-103">SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="5952d-103">Introduction to SignalR</span></span>
====================

<span data-ttu-id="5952d-104">参照してください[ASP.NET Core SignalR の概要](/aspnet/core/signalr/introduction)最新バージョンの Visual Studio を使用するこのチュートリアルの更新バージョン。</span><span class="sxs-lookup"><span data-stu-id="5952d-104">See [Introduction to ASP.NET Core SignalR](/aspnet/core/signalr/introduction) for an updated version of this tutorial that uses the latest version of Visual Studio.</span></span> <span data-ttu-id="5952d-105">新しいチュートリアルでは使用[ASP.NET Core](/aspnet/core/)、このチュートリアルで多くの機能強化を提供します。</span><span class="sxs-lookup"><span data-stu-id="5952d-105">The new tutorial uses [ASP.NET Core](/aspnet/core/), which provides many improvements over this tutorial.</span></span>

<span data-ttu-id="5952d-106">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5952d-106">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="5952d-107">この記事では、SignalR は、いくつかのソリューションを作成するため、デザインされたについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5952d-107">This article describes what SignalR is, and some of the solutions it was designed to create.</span></span> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="5952d-108">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="5952d-108">Questions and comments</span></span>
> 
> <span data-ttu-id="5952d-109">このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="5952d-109">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5952d-110">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr)します。</span><span class="sxs-lookup"><span data-stu-id="5952d-110">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).</span></span>


## <a name="what-is-signalr"></a><span data-ttu-id="5952d-111">SignalR とは何か</span><span class="sxs-lookup"><span data-stu-id="5952d-111">What is SignalR?</span></span>

<span data-ttu-id="5952d-112">ASP.NET SignalR は、リアルタイム web 機能をアプリケーションに追加するプロセスを簡素化する ASP.NET 開発者向けのライブラリです。</span><span class="sxs-lookup"><span data-stu-id="5952d-112">ASP.NET SignalR is a library for ASP.NET developers that simplifies the process of adding real-time web functionality to applications.</span></span> <span data-ttu-id="5952d-113">リアルタイム web 機能とは、サーバーの新しいデータを要求するクライアントの待機時間を移動するのではなく、サーバー コード プッシュが接続クライアントに対して瞬時にコンテンツが利用可能にする機能です。</span><span class="sxs-lookup"><span data-stu-id="5952d-113">Real-time web functionality is the ability to have server code push content to connected clients instantly as it becomes available, rather than having the server wait for a client to request new data.</span></span>

<span data-ttu-id="5952d-114">SignalR は ASP.NET アプリケーションに何らかの「リアルタイム」web 機能を追加するためにことができます。</span><span class="sxs-lookup"><span data-stu-id="5952d-114">SignalR can be used to add any sort of "real-time" web functionality to your ASP.NET application.</span></span> <span data-ttu-id="5952d-115">チャットの使用が例としては、多くの場合より多くを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="5952d-115">While chat is often used as an example, you can do a whole lot more.</span></span> <span data-ttu-id="5952d-116">いつでも、ユーザーは、新しいデータを表示する web ページを更新します。 または、ページを実装して[ポーリング時間の長い](http://en.wikipedia.org/wiki/Push_technology#Long_polling)の SignalR を使用して、候補では、新しいデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="5952d-116">Any time a user refreshes a web page to see new data, or the page implements [long polling](http://en.wikipedia.org/wiki/Push_technology#Long_polling) to retrieve new data, it is a candidate for using SignalR.</span></span> <span data-ttu-id="5952d-117">例としては、ダッシュ ボードとアプリケーションを監視するには、コラボレーション アプリケーション (など、同時ドキュメントの編集)、ジョブの進行状況の更新プログラム、およびリアルタイム フォーム。</span><span class="sxs-lookup"><span data-stu-id="5952d-117">Examples include dashboards and monitoring applications, collaborative applications (such as simultaneous editing of documents), job progress updates, and real-time forms.</span></span>

<span data-ttu-id="5952d-118">SignalR ができます。 完全に新しい種類のサーバーから頻度の高い更新プログラムを必要とする web アプリケーションのリアルタイムのゲームなど。</span><span class="sxs-lookup"><span data-stu-id="5952d-118">SignalR also enables completely new types of web applications that require high frequency updates from the server, for example, real-time gaming.</span></span>

<span data-ttu-id="5952d-119">SignalR は、サーバー側の .NET コードからのブラウザー (およびその他のクライアント プラットフォーム) クライアントで JavaScript 関数を呼び出すサーバーからクライアントへのリモート プロシージャ コール (RPC) を作成するための単純な API を提供します。</span><span class="sxs-lookup"><span data-stu-id="5952d-119">SignalR provides a simple API for creating server-to-client remote procedure calls (RPC) that call JavaScript functions in client browsers (and other client platforms) from server-side .NET code.</span></span> <span data-ttu-id="5952d-120">SignalR の接続管理の API も含まれます (接続し、切断イベントなど)、および接続をグループ化します。</span><span class="sxs-lookup"><span data-stu-id="5952d-120">SignalR also includes API for connection management (for instance, connect and disconnect events), and grouping connections.</span></span>

![SignalR によるメソッドの呼び出し](introduction-to-signalr/_static/image1.png)

<span data-ttu-id="5952d-122">SignalR では、自動的に、接続の管理を処理し、チャット ルームなど、接続されているすべてのクライアントへのブロードキャスト メッセージを同時に、できます。</span><span class="sxs-lookup"><span data-stu-id="5952d-122">SignalR handles connection management automatically, and lets you broadcast messages to all connected clients simultaneously, like a chat room.</span></span> <span data-ttu-id="5952d-123">特定のクライアントにメッセージを送信することもできます。</span><span class="sxs-lookup"><span data-stu-id="5952d-123">You can also send messages to specific clients.</span></span> <span data-ttu-id="5952d-124">クライアントとサーバー間の接続は、各通信の再確立される従来の HTTP 接続とは異なり、永続的です。</span><span class="sxs-lookup"><span data-stu-id="5952d-124">The connection between the client and server is persistent, unlike a classic HTTP connection, which is re-established for each communication.</span></span>

<span data-ttu-id="5952d-125">SignalR をサーバー コードを呼び出すクライアント コードの現在を使用して一般的な要求-応答のモデルではなく、リモート プロシージャ コール (RPC)、web ブラウザーで、「サーバー プッシュ」機能をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="5952d-125">SignalR supports "server push" functionality, in which server code can call out to client code in the browser using Remote Procedure Calls (RPC), rather than the request-response model common on the web today.</span></span>

<span data-ttu-id="5952d-126">SignalR アプリケーションを何千もの Service Bus、SQL Server を使用してクライアントをスケール アウトできますまたは[Redis](http://redis.io)します。</span><span class="sxs-lookup"><span data-stu-id="5952d-126">SignalR applications can scale out to thousands of clients using Service Bus, SQL Server or [Redis](http://redis.io).</span></span>

<span data-ttu-id="5952d-127">SignalR はオープン ソース、使用してアクセス[GitHub](https://github.com/signalr)します。</span><span class="sxs-lookup"><span data-stu-id="5952d-127">SignalR is open-source, accessible through [GitHub](https://github.com/signalr).</span></span>

## <a name="signalr-and-websocket"></a><span data-ttu-id="5952d-128">SignalR と WebSocket</span><span class="sxs-lookup"><span data-stu-id="5952d-128">SignalR and WebSocket</span></span>

<span data-ttu-id="5952d-129">SignalR では、使用可能な場合、新しい WebSocket トランスポートを使用し、必要に応じて、古いトランスポートにフォールバックします。</span><span class="sxs-lookup"><span data-stu-id="5952d-129">SignalR uses the new WebSocket transport where available and falls back to older transports where necessary.</span></span> <span data-ttu-id="5952d-130">確かに、WebSocket を直接使用して、SignalR を実装する必要がありますが、追加の機能の多くは既に実施することを意味を使用してアプリを作成できますが。</span><span class="sxs-lookup"><span data-stu-id="5952d-130">While you could certainly write your app using WebSocket directly, using SignalR means that a lot of the extra functionality you would need to implement is already done for you.</span></span> <span data-ttu-id="5952d-131">最も重要なは、WebSocket の古いクライアントの別のコード パスの作成について心配することがなく利用するアプリをコーディングすることがこれを意味します。</span><span class="sxs-lookup"><span data-stu-id="5952d-131">Most importantly, this means that you can code your app to take advantage of WebSocket without having to worry about creating a separate code path for older clients.</span></span> <span data-ttu-id="5952d-132">SignalR も明らかにならないようにする、アプリケーションが WebSocket のバージョン間での一貫性のあるインターフェイスを提供する、基になるトランスポートに変更をサポートするために SignalR が更新されるため、WebSocket への更新について心配する必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="5952d-132">SignalR also shields you from having to worry about updates to WebSocket, since SignalR is updated to support changes in the underlying transport, providing your application a consistent interface across versions of WebSocket.</span></span>

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a><span data-ttu-id="5952d-133">トランスポートとフォールバック</span><span class="sxs-lookup"><span data-stu-id="5952d-133">Transports and fallbacks</span></span>

<span data-ttu-id="5952d-134">SignalR では、クライアントとサーバー間のリアルタイムの作業を行うには、必要なトランスポートの中に、抽象化です。</span><span class="sxs-lookup"><span data-stu-id="5952d-134">SignalR is an abstraction over some of the transports that are required to do real-time work between client and server.</span></span> <span data-ttu-id="5952d-135">SignalR 接続は、HTTP、として起動し、入手可能になった場合、WebSocket 接続には、昇格します。</span><span class="sxs-lookup"><span data-stu-id="5952d-135">A SignalR connection starts as HTTP, and is then promoted to a WebSocket connection if it is available.</span></span> <span data-ttu-id="5952d-136">サーバーのメモリを最も効率的な使用によりが最も低い待機時間、および、(など、完全な双方向クライアントとサーバー間通信)、最も基本的な機能がいますが、最も厳格なので、WebSocket は、SignalR の理想的なトランスポート要件: WebSocket が Windows Server 2012 または Windows 8、および .NET Framework 4.5 を使用するサーバーが必要です。</span><span class="sxs-lookup"><span data-stu-id="5952d-136">WebSocket is the ideal transport for SignalR, since it makes the most efficient use of server memory, has the lowest latency, and has the most underlying features (such as full duplex communication between client and server), but it also has the most stringent requirements: WebSocket requires the server to be using Windows Server 2012 or Windows 8, and .NET Framework 4.5.</span></span> <span data-ttu-id="5952d-137">これらの要件が満たされず、SignalR は、接続するために他のトランスポートを使用しようとします。</span><span class="sxs-lookup"><span data-stu-id="5952d-137">If these requirements are not met, SignalR will attempt to use other transports to make its connections.</span></span>

### <a name="html-5-transports"></a><span data-ttu-id="5952d-138">HTML 5 を転送します。</span><span class="sxs-lookup"><span data-stu-id="5952d-138">HTML 5 transports</span></span>

<span data-ttu-id="5952d-139">これらのトランスポートのサポートによって異なります[HTML 5](http://en.wikipedia.org/wiki/HTML5)します。</span><span class="sxs-lookup"><span data-stu-id="5952d-139">These transports depend on support for [HTML 5](http://en.wikipedia.org/wiki/HTML5).</span></span> <span data-ttu-id="5952d-140">クライアントのブラウザーが HTML 5 の標準をサポートしていない場合は、古いトランスポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="5952d-140">If the client browser does not support the HTML 5 standard, older transports will be used.</span></span>

- <span data-ttu-id="5952d-141">**WebSocket** (場合、サーバーとブラウザーの両方は、Websocket をサポートすることができますを示します)。</span><span class="sxs-lookup"><span data-stu-id="5952d-141">**WebSocket** (if the both the server and browser indicate they can support Websocket).</span></span> <span data-ttu-id="5952d-142">WebSocket は、クライアントとサーバー間の場合は true。 永続的な双方向接続を確立するだけのトランスポートです。</span><span class="sxs-lookup"><span data-stu-id="5952d-142">WebSocket is the only transport that establishes a true persistent, two-way connection between client and server.</span></span> <span data-ttu-id="5952d-143">ただし、WebSocket でも最も厳しい要件があります。完全に Microsoft Internet Explorer、Google Chrome、Mozilla Firefox、最新のバージョンでのみサポートし、Opera、Safari などの他のブラウザーで部分の実装のみがあります。</span><span class="sxs-lookup"><span data-stu-id="5952d-143">However, WebSocket also has the most stringent requirements; it is fully supported only in the latest versions of Microsoft Internet Explorer, Google Chrome, and Mozilla Firefox, and only has a partial implementation in other browsers such as Opera and Safari.</span></span>
- <span data-ttu-id="5952d-144">**サーバー送信イベント**EventSource (ブラウザーがサポートする場合は Internet Explorer を除くすべてのブラウザーでは基本的にサーバー送信イベント、.) とも呼ばれます</span><span class="sxs-lookup"><span data-stu-id="5952d-144">**Server Sent Events**, also known as EventSource (if the browser supports Server Sent Events, which is basically all browsers except Internet Explorer.)</span></span>

### <a name="comet-transports"></a><span data-ttu-id="5952d-145">Comet トランスポート</span><span class="sxs-lookup"><span data-stu-id="5952d-145">Comet transports</span></span>

<span data-ttu-id="5952d-146">次のトランスポートがに基づいて、 [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web アプリケーションのモデル、ブラウザーまたは他のクライアントが長時間保持されている HTTP 要求データを具体的には、クライアントを使用せず、クライアントをプッシュするサーバーが使用できるを維持それを要求します。</span><span class="sxs-lookup"><span data-stu-id="5952d-146">The following transports are based on the [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web application model, in which a browser or other client maintains a long-held HTTP request, which the server can use to push data to the client without the client specifically requesting it.</span></span>

- <span data-ttu-id="5952d-147">**フレームの永久に**(用 Internet Explorer の場合のみ)。</span><span class="sxs-lookup"><span data-stu-id="5952d-147">**Forever Frame** (for Internet Explorer only).</span></span> <span data-ttu-id="5952d-148">永遠にフレームは、完了しませんサーバー上のエンドポイントへの要求を非表示の IFrame を作成します。</span><span class="sxs-lookup"><span data-stu-id="5952d-148">Forever Frame creates a hidden IFrame which makes a request to an endpoint on the server that does not complete.</span></span> <span data-ttu-id="5952d-149">サーバーは、サーバーからクライアントへの一方向のリアルタイム接続を提供することはすぐに実行するクライアントに、スクリプトを継続的に送信します。</span><span class="sxs-lookup"><span data-stu-id="5952d-149">The server then continually sends script to the client which is immediately executed, providing a one-way realtime connection from server to client.</span></span> <span data-ttu-id="5952d-150">クライアントからサーバーへの接続のクライアントの接続をサーバーから別の接続を使用して、送信する必要があるデータの各部分の標準の HTTP 要求のように、新しい接続が作成されます。</span><span class="sxs-lookup"><span data-stu-id="5952d-150">The connection from client to server uses a separate connection from the server to client connection, and like a standard HTTP request, a new connection is created for each piece of data that needs to be sent.</span></span>
- <span data-ttu-id="5952d-151">**長いポーリングの Ajax**します。</span><span class="sxs-lookup"><span data-stu-id="5952d-151">**Ajax long polling**.</span></span> <span data-ttu-id="5952d-152">ロング ポーリングは、永続的な接続は作成されませんが、代わりに、サーバーは、この時点で、接続が閉じ、新しい接続がすぐに要求されるまで開いたままになる要求でサーバーをポーリングします。</span><span class="sxs-lookup"><span data-stu-id="5952d-152">Long polling does not create a persistent connection, but instead polls the server with a request that stays open until the server responds, at which point the connection closes, and a new connection is requested immediately.</span></span> <span data-ttu-id="5952d-153">接続のリセット中に、いくつかの待機時間があります。</span><span class="sxs-lookup"><span data-stu-id="5952d-153">This may introduce some latency while the connection resets.</span></span>

<span data-ttu-id="5952d-154">どの構成でサポートされているどのようなトランスポートの詳細については、次を参照してください。[サポートされているプラットフォーム](supported-platforms.md)します。</span><span class="sxs-lookup"><span data-stu-id="5952d-154">For more information on what transports are supported under which configurations, see [Supported Platforms](supported-platforms.md).</span></span>

### <a name="transport-selection-process"></a><span data-ttu-id="5952d-155">トランスポートの選択のプロセス</span><span class="sxs-lookup"><span data-stu-id="5952d-155">Transport selection process</span></span>

<span data-ttu-id="5952d-156">SignalR を使用して使用するトランスポートを決定する手順を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="5952d-156">The following list shows the steps that SignalR uses to decide which transport to use.</span></span>

1. <span data-ttu-id="5952d-157">ブラウザーが Internet Explorer 8 またはそれ以前の場合は、長いポーリングが使用されます。</span><span class="sxs-lookup"><span data-stu-id="5952d-157">If the browser is Internet Explorer 8 or earlier, Long Polling is used.</span></span>
2. <span data-ttu-id="5952d-158">JSONP が構成されている場合 (つまり、`jsonp`パラメーターに設定されて`true`接続が開始された場合)、長いポーリングを使用します。</span><span class="sxs-lookup"><span data-stu-id="5952d-158">If JSONP is configured (that is, the `jsonp` parameter is set to `true` when the connection is started), Long Polling is used.</span></span>
3. <span data-ttu-id="5952d-159">(つまり、SignalR エンドポイントが、ホスティング ページと同じドメインにない) 場合、クロス ドメイン接続が確立されている場合、WebSocket が使用されます、次の条件が満たされた場合。</span><span class="sxs-lookup"><span data-stu-id="5952d-159">If a cross-domain connection is being made (that is, if the SignalR endpoint is not in the same domain as the hosting page), then WebSocket will be used if the following criteria are met:</span></span>

   - <span data-ttu-id="5952d-160">クライアントは、CORS (クロス オリジン リソース共有) をサポートします。</span><span class="sxs-lookup"><span data-stu-id="5952d-160">The client supports CORS (Cross-Origin Resource Sharing).</span></span> <span data-ttu-id="5952d-161">クライアントで CORS がサポートの詳細は、次を参照してください。 [caniuse.com で CORS](http://www.caniuse.com/CORS)します。</span><span class="sxs-lookup"><span data-stu-id="5952d-161">For details on which clients support CORS, see [CORS at caniuse.com](http://www.caniuse.com/CORS).</span></span>
   - <span data-ttu-id="5952d-162">クライアントは、WebSocket をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="5952d-162">The client supports WebSocket</span></span>
   - <span data-ttu-id="5952d-163">サーバーは、WebSocket をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="5952d-163">The server supports WebSocket</span></span>

     <span data-ttu-id="5952d-164">これらの条件のいずれかが満たされていない場合は、長いポーリングが使用されます。</span><span class="sxs-lookup"><span data-stu-id="5952d-164">If any of these criteria are not met, Long Polling will be used.</span></span> <span data-ttu-id="5952d-165">ドメイン間の接続の詳細については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)します。</span><span class="sxs-lookup"><span data-stu-id="5952d-165">For more information on cross-domain connections, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>
4. <span data-ttu-id="5952d-166">JSONP が構成されていない接続は、クロス ドメインではない場合は、WebSocket は、クライアントとサーバーの両方がサポートしている場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="5952d-166">If JSONP is not configured and the connection is not cross-domain, WebSocket will be used if both the client and server support it.</span></span>
5. <span data-ttu-id="5952d-167">クライアントまたはサーバーのいずれかが WebSocket をサポートしていない場合、サーバー送信イベントは、入手可能になった場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="5952d-167">If either the client or server do not support WebSocket, Server Sent Events is used if it is available.</span></span>
6. <span data-ttu-id="5952d-168">サーバー送信イベントが使用できない場合は、永久にフレームが試行されます。</span><span class="sxs-lookup"><span data-stu-id="5952d-168">If Server Sent Events is not available, Forever Frame is attempted.</span></span>
7. <span data-ttu-id="5952d-169">フレームを永久に失敗すると、長いポーリングが使用されます。</span><span class="sxs-lookup"><span data-stu-id="5952d-169">If Forever Frame fails, Long Polling is used.</span></span>

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a><span data-ttu-id="5952d-170">トランスポートの監視</span><span class="sxs-lookup"><span data-stu-id="5952d-170">Monitoring transports</span></span>

<span data-ttu-id="5952d-171">アプリケーションは、ハブにログ記録と、コンソール ウィンドウを開き、お使いのブラウザーで有効にして使用してトランスポートを指定できます。</span><span class="sxs-lookup"><span data-stu-id="5952d-171">You can determine what transport your application is using by enabling logging on your hub, and opening the console window in your browser.</span></span>

<span data-ttu-id="5952d-172">ブラウザーで、ハブのイベントのログ記録を有効にするには、クライアント アプリケーションに、次のコマンドを追加します。</span><span class="sxs-lookup"><span data-stu-id="5952d-172">To enable logging for your hub's events in a browser, add the following command to your client application:</span></span>

`$.connection.hub.logging = true;`

- <span data-ttu-id="5952d-173">Internet Explorer で、F12 キーを押して開発者ツールを開くし、[コンソール] タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5952d-173">In Internet Explorer, open the developer tools by pressing F12, and click the Console tab.</span></span>

    ![Microsoft Internet Explorer でコンソール](introduction-to-signalr/_static/image2.png)
- <span data-ttu-id="5952d-175">Chrome では、Ctrl + Shift + J キーを押して、コンソールを開きます。</span><span class="sxs-lookup"><span data-stu-id="5952d-175">In Chrome, open the console by pressing Ctrl+Shift+J.</span></span>

    ![Google Chrome でコンソール](introduction-to-signalr/_static/image3.png)

<span data-ttu-id="5952d-177">コンソールを開くとログが有効では、トランスポートは、SignalR によって使用されているを参照してください。 できるします。</span><span class="sxs-lookup"><span data-stu-id="5952d-177">With the console open and logging enabled, you'll be able to see which transport is being used by SignalR.</span></span>

![Internet explorer が WebSocket トランスポートを示すコンソールします。](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a><span data-ttu-id="5952d-179">トランスポートを指定します。</span><span class="sxs-lookup"><span data-stu-id="5952d-179">Specifying a transport</span></span>

<span data-ttu-id="5952d-180">一定の時間とクライアント/サーバーをリソースには、トランスポートをネゴシエートします。</span><span class="sxs-lookup"><span data-stu-id="5952d-180">Negotiating a transport takes a certain amount of time and client/server resources.</span></span> <span data-ttu-id="5952d-181">クライアントの機能がわかっている場合、トランスポートを指定できます、クライアント接続が開始されると。</span><span class="sxs-lookup"><span data-stu-id="5952d-181">If the client capabilities are known, then a transport can be specified when the client connection is started.</span></span> <span data-ttu-id="5952d-182">次のコード スニペットでは、クライアントがその他のプロトコルをサポートしていないことをわかっているが場合に使用されるように、Ajax の長いポーリングのトランスポートを使用して接続の開始を示しています。</span><span class="sxs-lookup"><span data-stu-id="5952d-182">The following code snippet demonstrates starting a connection using the Ajax Long Polling transport, as would be used if it was known that the client did not support any other protocol:</span></span>

`connection.start({ transport: 'longPolling' });`

<span data-ttu-id="5952d-183">場合は、クライアントの順序で特定のトランスポートにフォールバックの順序を指定できます。</span><span class="sxs-lookup"><span data-stu-id="5952d-183">You can specify a fallback order if you want a client to try specific transports in order.</span></span> <span data-ttu-id="5952d-184">次のコード スニペットは、しようとしての WebSocket とそれがない場合、長いポーリングを直接を示します。</span><span class="sxs-lookup"><span data-stu-id="5952d-184">The following code snippet demonstrates trying WebSocket, and failing that, going directly to Long Polling.</span></span>

`connection.start({ transport: ['webSockets','longPolling'] });`

<span data-ttu-id="5952d-185">トランスポートを指定する文字列定数の定義は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5952d-185">The string constants for specifying transports are defined as follows:</span></span>

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a><span data-ttu-id="5952d-186">接続とハブ</span><span class="sxs-lookup"><span data-stu-id="5952d-186">Connections and Hubs</span></span>

<span data-ttu-id="5952d-187">SignalR の API には、クライアントとサーバー間の通信に 2 つのモデルが含まれています: 永続的な接続とハブ。</span><span class="sxs-lookup"><span data-stu-id="5952d-187">The SignalR API contains two models for communicating between clients and servers: Persistent Connections and Hubs.</span></span>

<span data-ttu-id="5952d-188">接続では、単一受信者、グループ化、またはブロードキャスト メッセージの送信の単純なエンドポイントを表します。</span><span class="sxs-lookup"><span data-stu-id="5952d-188">A Connection represents a simple endpoint for sending single-recipient, grouped, or broadcast messages.</span></span> <span data-ttu-id="5952d-189">永続的な接続 API が (PersistentConnection クラスでは、.NET コードで表す) は、開発者は、SignalR を公開する低レベルの通信プロトコルへのアクセスを直接示しています。</span><span class="sxs-lookup"><span data-stu-id="5952d-189">The Persistent Connection API (represented in .NET code by the PersistentConnection class) gives the developer direct access to the low-level communication protocol that SignalR exposes.</span></span> <span data-ttu-id="5952d-190">接続の通信モデルを使用して Windows Communication Foundation など、接続ベースの Api を使用していた開発者にとって身近になります。</span><span class="sxs-lookup"><span data-stu-id="5952d-190">Using the Connections communication model will be familiar to developers who have used connection-based APIs such as Windows Communication Foundation.</span></span>

<span data-ttu-id="5952d-191">ハブは、クライアントとサーバーの相互に直接メソッドを呼び出すことができる接続 API に基づいて構築されたより高度なパイプラインです。</span><span class="sxs-lookup"><span data-stu-id="5952d-191">A Hub is a more high-level pipeline built upon the Connection API that allows your client and server to call methods on each other directly.</span></span> <span data-ttu-id="5952d-192">SignalR は、クライアントとして、ローカルのメソッドとして簡単にし、その逆の場合、サーバーでメソッドを呼び出すをできるように、不思議な力とした場合と同じコンピューターの境界を越えてディスパッチを処理します。</span><span class="sxs-lookup"><span data-stu-id="5952d-192">SignalR handles the dispatching across machine boundaries as if by magic, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="5952d-193">ハブの通信モデルを使用してリモート呼び出し .NET リモート処理などの Api を使用していた開発者にとって身近になります。</span><span class="sxs-lookup"><span data-stu-id="5952d-193">Using the Hubs communication model will be familiar to developers who have used remote invocation APIs such as .NET Remoting.</span></span> <span data-ttu-id="5952d-194">ハブを使用してではモデル バインドを有効にすると、メソッドに厳密に型指定されたパラメーターを渡すこともできます。</span><span class="sxs-lookup"><span data-stu-id="5952d-194">Using a Hub also allows you to pass strongly typed parameters to methods, enabling model binding.</span></span>

### <a name="architecture-diagram"></a><span data-ttu-id="5952d-195">アーキテクチャ図</span><span class="sxs-lookup"><span data-stu-id="5952d-195">Architecture diagram</span></span>

<span data-ttu-id="5952d-196">次の図は、ハブ、永続的な接続、およびトランスポートに使用される基になるテクノロジ間の関係を示します。</span><span class="sxs-lookup"><span data-stu-id="5952d-196">The following diagram shows the relationship between Hubs, Persistent Connections, and the underlying technologies used for transports.</span></span>

![Api、トランスポート、およびクライアントを示す、SignalR のアーキテクチャ図](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a><span data-ttu-id="5952d-198">ハブのしくみ</span><span class="sxs-lookup"><span data-stu-id="5952d-198">How Hubs work</span></span>

<span data-ttu-id="5952d-199">パケットが呼び出されるメソッドのパラメーターと名前を含むアクティブなトランスポート経由で送信されるときに、サーバー側コードでは、クライアントでメソッドを呼び出し、(オブジェクトがメソッド パラメーターとして送信されると、シリアル化される JSON を使用して)。</span><span class="sxs-lookup"><span data-stu-id="5952d-199">When server-side code calls a method on the client, a packet is sent across the active transport that contains the name and parameters of the method to be called (when an object is sent as a method parameter, it is serialized using JSON).</span></span> <span data-ttu-id="5952d-200">クライアントには、メソッド名をクライアント側のコードで定義されているメソッドを次と一致します。</span><span class="sxs-lookup"><span data-stu-id="5952d-200">The client then matches the method name to methods defined in client-side code.</span></span> <span data-ttu-id="5952d-201">一致がある場合、逆シリアル化されたパラメーターのデータを使用してクライアント メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="5952d-201">If there is a match, the client method will be executed using the deserialized parameter data.</span></span>

<span data-ttu-id="5952d-202">などのツールを使用して、メソッドの呼び出しを監視する[Fiddler します。](http://fiddler2.com/)</span><span class="sxs-lookup"><span data-stu-id="5952d-202">The method call can be monitored using tools like [Fiddler.](http://fiddler2.com/)</span></span> <span data-ttu-id="5952d-203">次の図は、Fiddler の [ログ] ウィンドウで、web ブラウザー クライアントに SignalR サーバーから送信されたメソッドの呼び出しを示します。</span><span class="sxs-lookup"><span data-stu-id="5952d-203">The following image shows a method call sent from a SignalR server to a web browser client in the Logs pane of Fiddler.</span></span> <span data-ttu-id="5952d-204">メソッドの呼び出しがという名前でハブから送信されて`MoveShapeHub`、呼び出されるメソッドが呼び出されて`updateShape`します。</span><span class="sxs-lookup"><span data-stu-id="5952d-204">The method call is being sent from a hub called `MoveShapeHub`, and the method being invoked is called `updateShape`.</span></span>

![SignalR のトラフィックを示す Fiddler ログの表示](introduction-to-signalr/_static/image6.png)

<span data-ttu-id="5952d-206">この例で、ハブの名前が付いて、`H`パラメーターは、メソッド名が付いて、`M`パラメーター、およびメソッドに送信されるデータがで識別される、`A`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="5952d-206">In this example, the hub name is identified with the `H` parameter; the method name is identified with the `M` parameter, and the data being sent to the method is identified with the `A` parameter.</span></span> <span data-ttu-id="5952d-207">このメッセージを生成したアプリケーションは、[高頻度リアルタイム メッセージング](tutorial-high-frequency-realtime-with-signalr.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="5952d-207">The application that generated this message is created in the [High-Frequency Realtime](tutorial-high-frequency-realtime-with-signalr.md) tutorial.</span></span>

### <a name="choosing-a-communication-model"></a><span data-ttu-id="5952d-208">通信モデルを選択します。</span><span class="sxs-lookup"><span data-stu-id="5952d-208">Choosing a communication model</span></span>

<span data-ttu-id="5952d-209">ほとんどのアプリケーションでは、ハブ API を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5952d-209">Most applications should use the Hubs API.</span></span> <span data-ttu-id="5952d-210">次の状況では、接続 API を使用できます。</span><span class="sxs-lookup"><span data-stu-id="5952d-210">The Connections API could be used in the following circumstances:</span></span>

- <span data-ttu-id="5952d-211">指定する実際のメッセージ送信のニーズの形式です。</span><span class="sxs-lookup"><span data-stu-id="5952d-211">The format of the actual message sent needs to be specified.</span></span>
- <span data-ttu-id="5952d-212">開発者は、リモート呼び出しモデルではなく、メッセージおよびディスパッチ モデルを操作しています。</span><span class="sxs-lookup"><span data-stu-id="5952d-212">The developer prefers to work with a messaging and dispatching model rather than a remote invocation model.</span></span>
- <span data-ttu-id="5952d-213">SignalR を使用してメッセージング モデルを使用する既存のアプリケーションに移植されるは。</span><span class="sxs-lookup"><span data-stu-id="5952d-213">An existing application that uses a messaging model is being ported to use SignalR.</span></span>
