---
uid: signalr/overview/getting-started/introduction-to-signalr
title: SignalR の概要 |Microsoft Docs
author: pfletcher
description: この記事では、SignalR は、いくつかのソリューションを作成するため、デザインされたについて説明します。
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 4c34f99674a8213966c44aa434a0e00690b30f44
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820189"
---
<a name="introduction-to-signalr"></a><span data-ttu-id="07e17-103">SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="07e17-103">Introduction to SignalR</span></span>
====================
<span data-ttu-id="07e17-104">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="07e17-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="07e17-105">この記事では、SignalR は、いくつかのソリューションを作成するため、デザインされたについて説明します。</span><span class="sxs-lookup"><span data-stu-id="07e17-105">This article describes what SignalR is, and some of the solutions it was designed to create.</span></span> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="07e17-106">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="07e17-106">Questions and comments</span></span>
> 
> <span data-ttu-id="07e17-107">このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="07e17-107">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="07e17-108">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr)します。</span><span class="sxs-lookup"><span data-stu-id="07e17-108">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).</span></span>


## <a name="what-is-signalr"></a><span data-ttu-id="07e17-109">SignalR は何ですか。</span><span class="sxs-lookup"><span data-stu-id="07e17-109">What is SignalR?</span></span>

<span data-ttu-id="07e17-110">ASP.NET SignalR は、リアルタイム web 機能をアプリケーションに追加するプロセスを簡素化する ASP.NET 開発者向けのライブラリです。</span><span class="sxs-lookup"><span data-stu-id="07e17-110">ASP.NET SignalR is a library for ASP.NET developers that simplifies the process of adding real-time web functionality to applications.</span></span> <span data-ttu-id="07e17-111">リアルタイム web 機能とは、サーバーの新しいデータを要求するクライアントの待機時間を移動するのではなく、サーバー コード プッシュが接続クライアントに対して瞬時にコンテンツが利用可能にする機能です。</span><span class="sxs-lookup"><span data-stu-id="07e17-111">Real-time web functionality is the ability to have server code push content to connected clients instantly as it becomes available, rather than having the server wait for a client to request new data.</span></span>

<span data-ttu-id="07e17-112">SignalR は ASP.NET アプリケーションに何らかの「リアルタイム」web 機能を追加するためにことができます。</span><span class="sxs-lookup"><span data-stu-id="07e17-112">SignalR can be used to add any sort of "real-time" web functionality to your ASP.NET application.</span></span> <span data-ttu-id="07e17-113">チャットの使用が例としては、多くの場合より多くを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="07e17-113">While chat is often used as an example, you can do a whole lot more.</span></span> <span data-ttu-id="07e17-114">いつでも、ユーザーは、新しいデータを表示する web ページを更新します。 または、ページを実装して[ポーリング時間の長い](http://en.wikipedia.org/wiki/Push_technology#Long_polling)の SignalR を使用して、候補では、新しいデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="07e17-114">Any time a user refreshes a web page to see new data, or the page implements [long polling](http://en.wikipedia.org/wiki/Push_technology#Long_polling) to retrieve new data, it is a candidate for using SignalR.</span></span> <span data-ttu-id="07e17-115">例としては、ダッシュ ボードとアプリケーションを監視するには、コラボレーション アプリケーション (など、同時ドキュメントの編集)、ジョブの進行状況の更新プログラム、およびリアルタイム フォーム。</span><span class="sxs-lookup"><span data-stu-id="07e17-115">Examples include dashboards and monitoring applications, collaborative applications (such as simultaneous editing of documents), job progress updates, and real-time forms.</span></span>

<span data-ttu-id="07e17-116">SignalR ができます。 完全に新しい種類のサーバーから頻度の高い更新プログラムを必要とする web アプリケーションのリアルタイムのゲームなど。</span><span class="sxs-lookup"><span data-stu-id="07e17-116">SignalR also enables completely new types of web applications that require high frequency updates from the server, for example, real-time gaming.</span></span>

<span data-ttu-id="07e17-117">SignalR は、サーバー側の .NET コードからのブラウザー (およびその他のクライアント プラットフォーム) クライアントで JavaScript 関数を呼び出すサーバーからクライアントへのリモート プロシージャ コール (RPC) を作成するための単純な API を提供します。</span><span class="sxs-lookup"><span data-stu-id="07e17-117">SignalR provides a simple API for creating server-to-client remote procedure calls (RPC) that call JavaScript functions in client browsers (and other client platforms) from server-side .NET code.</span></span> <span data-ttu-id="07e17-118">SignalR の接続管理の API も含まれます (接続し、切断イベントなど)、および接続をグループ化します。</span><span class="sxs-lookup"><span data-stu-id="07e17-118">SignalR also includes API for connection management (for instance, connect and disconnect events), and grouping connections.</span></span>

![SignalR によるメソッドの呼び出し](introduction-to-signalr/_static/image1.png)

<span data-ttu-id="07e17-120">SignalR では、自動的に、接続の管理を処理し、チャット ルームなど、接続されているすべてのクライアントへのブロードキャスト メッセージを同時に、できます。</span><span class="sxs-lookup"><span data-stu-id="07e17-120">SignalR handles connection management automatically, and lets you broadcast messages to all connected clients simultaneously, like a chat room.</span></span> <span data-ttu-id="07e17-121">特定のクライアントにメッセージを送信することもできます。</span><span class="sxs-lookup"><span data-stu-id="07e17-121">You can also send messages to specific clients.</span></span> <span data-ttu-id="07e17-122">クライアントとサーバー間の接続は、各通信の再確立される従来の HTTP 接続とは異なり、永続的です。</span><span class="sxs-lookup"><span data-stu-id="07e17-122">The connection between the client and server is persistent, unlike a classic HTTP connection, which is re-established for each communication.</span></span>

<span data-ttu-id="07e17-123">SignalR をサーバー コードを呼び出すクライアント コードの現在を使用して一般的な要求-応答のモデルではなく、リモート プロシージャ コール (RPC)、web ブラウザーで、「サーバー プッシュ」機能をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="07e17-123">SignalR supports "server push" functionality, in which server code can call out to client code in the browser using Remote Procedure Calls (RPC), rather than the request-response model common on the web today.</span></span>

<span data-ttu-id="07e17-124">SignalR アプリケーションを何千もの Service Bus、SQL Server を使用してクライアントをスケール アウトできますまたは[Redis](http://redis.io)します。</span><span class="sxs-lookup"><span data-stu-id="07e17-124">SignalR applications can scale out to thousands of clients using Service Bus, SQL Server or [Redis](http://redis.io).</span></span>

<span data-ttu-id="07e17-125">SignalR はオープン ソース、使用してアクセス[GitHub](https://github.com/signalr)します。</span><span class="sxs-lookup"><span data-stu-id="07e17-125">SignalR is open-source, accessible through [GitHub](https://github.com/signalr).</span></span>

## <a name="signalr-and-websocket"></a><span data-ttu-id="07e17-126">SignalR と WebSocket</span><span class="sxs-lookup"><span data-stu-id="07e17-126">SignalR and WebSocket</span></span>

<span data-ttu-id="07e17-127">SignalR では、使用可能な場合、新しい WebSocket トランスポートを使用し、必要に応じて、古いトランスポートにフォールバックします。</span><span class="sxs-lookup"><span data-stu-id="07e17-127">SignalR uses the new WebSocket transport where available, and falls back to older transports where necessary.</span></span> <span data-ttu-id="07e17-128">確かに、アプリケーションを作成できますが WebSocket を直接使用して、SignalR を使用して実装することになる追加機能の多くは既にことを意味が実行されていること。</span><span class="sxs-lookup"><span data-stu-id="07e17-128">While you could certainly write your application using WebSocket directly, using SignalR means that a lot of the extra functionality you would need to implement will already have been done for you.</span></span> <span data-ttu-id="07e17-129">最も重要なは、WebSocket の古いクライアントの別のコード パスの作成について心配することがなく利用するアプリケーションをコーディングすることがこれを意味します。</span><span class="sxs-lookup"><span data-stu-id="07e17-129">Most importantly, this means that you can code your application to take advantage of WebSocket without having to worry about creating a separate code path for older clients.</span></span> <span data-ttu-id="07e17-130">SignalR では、SignalR は引き続き、基になるトランスポートに変更をサポートするために更新する WebSocket のバージョン間で、アプリケーションの一貫したインターフェイスを提供するために、WebSocket への更新について心配することからしても保護します。</span><span class="sxs-lookup"><span data-stu-id="07e17-130">SignalR also shields you from having to worry about updates to WebSocket, since SignalR will continue to be updated to support changes in the underlying transport, providing your application a consistent interface across versions of WebSocket.</span></span>

<span data-ttu-id="07e17-131">単独で WebSocket を使用してソリューションを作成することが確かに、SignalR はすべての他のトランスポートと WebSocket の実装に更新プログラム、アプリケーションへのフォールバックなどを自分で記述する必要があります機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="07e17-131">While you could certainly create a solution using WebSocket alone, SignalR provides all of the functionality you would need to write yourself, such as fallback to other transports and revising your application for updates to WebSocket implementations.</span></span>

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a><span data-ttu-id="07e17-132">トランスポートとフォールバック</span><span class="sxs-lookup"><span data-stu-id="07e17-132">Transports and fallbacks</span></span>

<span data-ttu-id="07e17-133">SignalR では、クライアントとサーバー間のリアルタイムの作業を行うには、必要なトランスポートの中に、抽象化です。</span><span class="sxs-lookup"><span data-stu-id="07e17-133">SignalR is an abstraction over some of the transports that are required to do real-time work between client and server.</span></span> <span data-ttu-id="07e17-134">SignalR 接続は、HTTP、として起動し、入手可能になった場合、WebSocket 接続には、昇格します。</span><span class="sxs-lookup"><span data-stu-id="07e17-134">A SignalR connection starts as HTTP, and is then promoted to a WebSocket connection if it is available.</span></span> <span data-ttu-id="07e17-135">サーバーのメモリを最も効率的な使用によりが最も低い待機時間、および、(など、完全な双方向クライアントとサーバー間通信)、最も基本的な機能がいますが、最も厳格なので、WebSocket は、SignalR の理想的なトランスポート要件: WebSocket が Windows Server 2012 または Windows 8、および .NET Framework 4.5 を使用するサーバーが必要です。</span><span class="sxs-lookup"><span data-stu-id="07e17-135">WebSocket is the ideal transport for SignalR, since it makes the most efficient use of server memory, has the lowest latency, and has the most underlying features (such as full duplex communication between client and server), but it also has the most stringent requirements: WebSocket requires the server to be using Windows Server 2012 or Windows 8, and .NET Framework 4.5.</span></span> <span data-ttu-id="07e17-136">これらの要件が満たされず、SignalR は、接続するために他のトランスポートを使用しようとします。</span><span class="sxs-lookup"><span data-stu-id="07e17-136">If these requirements are not met, SignalR will attempt to use other transports to make its connections.</span></span>

### <a name="html-5-transports"></a><span data-ttu-id="07e17-137">HTML 5 を転送します。</span><span class="sxs-lookup"><span data-stu-id="07e17-137">HTML 5 transports</span></span>

<span data-ttu-id="07e17-138">これらのトランスポートのサポートによって異なります[HTML 5](http://en.wikipedia.org/wiki/HTML5)します。</span><span class="sxs-lookup"><span data-stu-id="07e17-138">These transports depend on support for [HTML 5](http://en.wikipedia.org/wiki/HTML5).</span></span> <span data-ttu-id="07e17-139">クライアントのブラウザーが HTML 5 の標準をサポートしていない場合は、古いトランスポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="07e17-139">If the client browser does not support the HTML 5 standard, older transports will be used.</span></span>

- <span data-ttu-id="07e17-140">**WebSocket** (場合、サーバーとブラウザーの両方は、Websocket をサポートすることができますを示します)。</span><span class="sxs-lookup"><span data-stu-id="07e17-140">**WebSocket** (if the both the server and browser indicate they can support Websocket).</span></span> <span data-ttu-id="07e17-141">WebSocket は、クライアントとサーバー間の場合は true。 永続的な双方向接続を確立するだけのトランスポートです。</span><span class="sxs-lookup"><span data-stu-id="07e17-141">WebSocket is the only transport that establishes a true persistent, two-way connection between client and server.</span></span> <span data-ttu-id="07e17-142">ただし、WebSocket でも最も厳しい要件があります。完全に Microsoft Internet Explorer、Google Chrome、Mozilla Firefox、最新のバージョンでのみサポートし、Opera、Safari などの他のブラウザーで部分の実装のみがあります。</span><span class="sxs-lookup"><span data-stu-id="07e17-142">However, WebSocket also has the most stringent requirements; it is fully supported only in the latest versions of Microsoft Internet Explorer, Google Chrome, and Mozilla Firefox, and only has a partial implementation in other browsers such as Opera and Safari.</span></span>
- <span data-ttu-id="07e17-143">**サーバー送信イベント**EventSource (ブラウザーがサポートする場合は Internet Explorer を除くすべてのブラウザーでは基本的にサーバー送信イベント、.) とも呼ばれます</span><span class="sxs-lookup"><span data-stu-id="07e17-143">**Server Sent Events**, also known as EventSource (if the browser supports Server Sent Events, which is basically all browsers except Internet Explorer.)</span></span>

### <a name="comet-transports"></a><span data-ttu-id="07e17-144">Comet トランスポート</span><span class="sxs-lookup"><span data-stu-id="07e17-144">Comet transports</span></span>

<span data-ttu-id="07e17-145">次のトランスポートがに基づいて、 [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web アプリケーションのモデル、ブラウザーまたは他のクライアントが長時間保持されている HTTP 要求データを具体的には、クライアントを使用せず、クライアントをプッシュするサーバーが使用できるを維持それを要求します。</span><span class="sxs-lookup"><span data-stu-id="07e17-145">The following transports are based on the [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web application model, in which a browser or other client maintains a long-held HTTP request, which the server can use to push data to the client without the client specifically requesting it.</span></span>

- <span data-ttu-id="07e17-146">**フレームの永久に**(用 Internet Explorer の場合のみ)。</span><span class="sxs-lookup"><span data-stu-id="07e17-146">**Forever Frame** (for Internet Explorer only).</span></span> <span data-ttu-id="07e17-147">永遠にフレームは、完了しませんサーバー上のエンドポイントへの要求を非表示の IFrame を作成します。</span><span class="sxs-lookup"><span data-stu-id="07e17-147">Forever Frame creates a hidden IFrame which makes a request to an endpoint on the server that does not complete.</span></span> <span data-ttu-id="07e17-148">サーバーは、サーバーからクライアントへの一方向のリアルタイム接続を提供することはすぐに実行するクライアントに、スクリプトを継続的に送信します。</span><span class="sxs-lookup"><span data-stu-id="07e17-148">The server then continually sends script to the client which is immediately executed, providing a one-way realtime connection from server to client.</span></span> <span data-ttu-id="07e17-149">クライアントからサーバーへの接続のクライアントの接続をサーバーから別の接続を使用して、送信する必要があるデータの各部分の標準の HTTP 要求のように、新しい接続が作成されます。</span><span class="sxs-lookup"><span data-stu-id="07e17-149">The connection from client to server uses a separate connection from the server to client connection, and like a standard HTTP request, a new connection is created for each piece of data that needs to be sent.</span></span>
- <span data-ttu-id="07e17-150">**長いポーリングの Ajax**します。</span><span class="sxs-lookup"><span data-stu-id="07e17-150">**Ajax long polling**.</span></span> <span data-ttu-id="07e17-151">ロング ポーリングは、永続的な接続は作成されませんが、代わりに、サーバーは、この時点で、接続が閉じ、新しい接続がすぐに要求されるまで開いたままになる要求でサーバーをポーリングします。</span><span class="sxs-lookup"><span data-stu-id="07e17-151">Long polling does not create a persistent connection, but instead polls the server with a request that stays open until the server responds, at which point the connection closes, and a new connection is requested immediately.</span></span> <span data-ttu-id="07e17-152">接続のリセット中に、いくつかの待機時間があります。</span><span class="sxs-lookup"><span data-stu-id="07e17-152">This may introduce some latency while the connection resets.</span></span>

<span data-ttu-id="07e17-153">どの構成でサポートされているどのようなトランスポートの詳細については、次を参照してください。[サポートされているプラットフォーム](supported-platforms.md)します。</span><span class="sxs-lookup"><span data-stu-id="07e17-153">For more information on what transports are supported under which configurations, see [Supported Platforms](supported-platforms.md).</span></span>

### <a name="transport-selection-process"></a><span data-ttu-id="07e17-154">トランスポートの選択のプロセス</span><span class="sxs-lookup"><span data-stu-id="07e17-154">Transport selection process</span></span>

<span data-ttu-id="07e17-155">SignalR を使用して使用するトランスポートを決定する手順を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="07e17-155">The following list shows the steps that SignalR uses to decide which transport to use.</span></span>

1. <span data-ttu-id="07e17-156">ブラウザーが Internet Explorer 8 またはそれ以前の場合は、長いポーリングが使用されます。</span><span class="sxs-lookup"><span data-stu-id="07e17-156">If the browser is Internet Explorer 8 or earlier, Long Polling is used.</span></span>
2. <span data-ttu-id="07e17-157">JSONP が構成されている場合 (つまり、`jsonp`パラメーターに設定されて`true`接続が開始された場合)、長いポーリングを使用します。</span><span class="sxs-lookup"><span data-stu-id="07e17-157">If JSONP is configured (that is, the `jsonp` parameter is set to `true` when the connection is started), Long Polling is used.</span></span>
3. <span data-ttu-id="07e17-158">(つまり、SignalR エンドポイントが、ホスティング ページと同じドメインにない) 場合、クロス ドメイン接続が確立されている場合、WebSocket が使用されます、次の条件が満たされた場合。</span><span class="sxs-lookup"><span data-stu-id="07e17-158">If a cross-domain connection is being made (that is, if the SignalR endpoint is not in the same domain as the hosting page), then WebSocket will be used if the following criteria are met:</span></span>

   - <span data-ttu-id="07e17-159">クライアントは、CORS (クロス オリジン リソース共有) をサポートします。</span><span class="sxs-lookup"><span data-stu-id="07e17-159">The client supports CORS (Cross-Origin Resource Sharing).</span></span> <span data-ttu-id="07e17-160">クライアントで CORS がサポートの詳細は、次を参照してください。 [caniuse.com で CORS](http://www.caniuse.com/CORS)します。</span><span class="sxs-lookup"><span data-stu-id="07e17-160">For details on which clients support CORS, see [CORS at caniuse.com](http://www.caniuse.com/CORS).</span></span>
   - <span data-ttu-id="07e17-161">クライアントは、WebSocket をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="07e17-161">The client supports WebSocket</span></span>
   - <span data-ttu-id="07e17-162">サーバーは、WebSocket をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="07e17-162">The server supports WebSocket</span></span>

     <span data-ttu-id="07e17-163">これらの条件のいずれかが満たされていない場合は、長いポーリングが使用されます。</span><span class="sxs-lookup"><span data-stu-id="07e17-163">If any of these criteria are not met, Long Polling will be used.</span></span> <span data-ttu-id="07e17-164">ドメイン間の接続の詳細については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)します。</span><span class="sxs-lookup"><span data-stu-id="07e17-164">For more information on cross-domain connections, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>
4. <span data-ttu-id="07e17-165">JSONP が構成されていない接続は、クロス ドメインではない場合は、WebSocket は、クライアントとサーバーの両方がサポートしている場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="07e17-165">If JSONP is not configured and the connection is not cross-domain, WebSocket will be used if both the client and server support it.</span></span>
5. <span data-ttu-id="07e17-166">クライアントまたはサーバーのいずれかが WebSocket をサポートしていない場合、サーバー送信イベントは、入手可能になった場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="07e17-166">If either the client or server do not support WebSocket, Server Sent Events is used if it is available.</span></span>
6. <span data-ttu-id="07e17-167">サーバー送信イベントが使用できない場合は、永久にフレームが試行されます。</span><span class="sxs-lookup"><span data-stu-id="07e17-167">If Server Sent Events is not available, Forever Frame is attempted.</span></span>
7. <span data-ttu-id="07e17-168">フレームを永久に失敗すると、長いポーリングが使用されます。</span><span class="sxs-lookup"><span data-stu-id="07e17-168">If Forever Frame fails, Long Polling is used.</span></span>

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a><span data-ttu-id="07e17-169">トランスポートの監視</span><span class="sxs-lookup"><span data-stu-id="07e17-169">Monitoring transports</span></span>

<span data-ttu-id="07e17-170">アプリケーションは、ハブにログ記録と、コンソール ウィンドウを開き、お使いのブラウザーで有効にして使用してトランスポートを指定できます。</span><span class="sxs-lookup"><span data-stu-id="07e17-170">You can determine what transport your application is using by enabling logging on your hub, and opening the console window in your browser.</span></span>

<span data-ttu-id="07e17-171">ブラウザーで、ハブのイベントのログ記録を有効にするには、クライアント アプリケーションに、次のコマンドを追加します。</span><span class="sxs-lookup"><span data-stu-id="07e17-171">To enable logging for your hub's events in a browser, add the following command to your client application:</span></span>

`$.connection.hub.logging = true;`

- <span data-ttu-id="07e17-172">Internet Explorer で、F12 キーを押して開発者ツールを開くし、[コンソール] タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="07e17-172">In Internet Explorer, open the developer tools by pressing F12, and click the Console tab.</span></span>

    ![Microsoft Internet Explorer でコンソール](introduction-to-signalr/_static/image2.png)
- <span data-ttu-id="07e17-174">Chrome では、Ctrl + Shift + J キーを押して、コンソールを開きます。</span><span class="sxs-lookup"><span data-stu-id="07e17-174">In Chrome, open the console by pressing Ctrl+Shift+J.</span></span>

    ![Google Chrome でコンソール](introduction-to-signalr/_static/image3.png)

<span data-ttu-id="07e17-176">コンソールを開くとログが有効では、トランスポートは、SignalR によって使用されているを参照してください。 できるします。</span><span class="sxs-lookup"><span data-stu-id="07e17-176">With the console open and logging enabled, you'll be able to see which transport is being used by SignalR.</span></span>

![Internet explorer が WebSocket トランスポートを示すコンソールします。](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a><span data-ttu-id="07e17-178">トランスポートを指定します。</span><span class="sxs-lookup"><span data-stu-id="07e17-178">Specifying a transport</span></span>

<span data-ttu-id="07e17-179">一定の時間とクライアント/サーバーをリソースには、トランスポートをネゴシエートします。</span><span class="sxs-lookup"><span data-stu-id="07e17-179">Negotiating a transport takes a certain amount of time and client/server resources.</span></span> <span data-ttu-id="07e17-180">クライアントの機能がわかっている場合、トランスポートを指定できます、クライアント接続が開始されると。</span><span class="sxs-lookup"><span data-stu-id="07e17-180">If the client capabilities are known, then a transport can be specified when the client connection is started.</span></span> <span data-ttu-id="07e17-181">次のコード スニペットでは、クライアントがその他のプロトコルをサポートしていないことをわかっているが場合に使用されるように、Ajax の長いポーリングのトランスポートを使用して接続の開始を示しています。</span><span class="sxs-lookup"><span data-stu-id="07e17-181">The following code snippet demonstrates starting a connection using the Ajax Long Polling transport, as would be used if it was known that the client did not support any other protocol:</span></span>

`connection.start({ transport: 'longPolling' });`

<span data-ttu-id="07e17-182">場合は、クライアントの順序で特定のトランスポートにフォールバックの順序を指定できます。</span><span class="sxs-lookup"><span data-stu-id="07e17-182">You can specify a fallback order if you want a client to try specific transports in order.</span></span> <span data-ttu-id="07e17-183">次のコード スニペットは、しようとしての WebSocket とそれがない場合、長いポーリングを直接を示します。</span><span class="sxs-lookup"><span data-stu-id="07e17-183">The following code snippet demonstrates trying WebSocket, and failing that, going directly to Long Polling.</span></span>

`connection.start({ transport: ['webSockets','longPolling'] });`

<span data-ttu-id="07e17-184">トランスポートを指定する文字列定数の定義は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="07e17-184">The string constants for specifying transports are defined as follows:</span></span>

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a><span data-ttu-id="07e17-185">接続とハブ</span><span class="sxs-lookup"><span data-stu-id="07e17-185">Connections and Hubs</span></span>

<span data-ttu-id="07e17-186">SignalR の API には、クライアントとサーバー間の通信に 2 つのモデルが含まれています: 永続的な接続とハブ。</span><span class="sxs-lookup"><span data-stu-id="07e17-186">The SignalR API contains two models for communicating between clients and servers: Persistent Connections and Hubs.</span></span>

<span data-ttu-id="07e17-187">接続では、単一受信者、グループ化、またはブロードキャスト メッセージの送信の単純なエンドポイントを表します。</span><span class="sxs-lookup"><span data-stu-id="07e17-187">A Connection represents a simple endpoint for sending single-recipient, grouped, or broadcast messages.</span></span> <span data-ttu-id="07e17-188">永続的な接続 API が (PersistentConnection クラスでは、.NET コードで表す) は、開発者は、SignalR を公開する低レベルの通信プロトコルへのアクセスを直接示しています。</span><span class="sxs-lookup"><span data-stu-id="07e17-188">The Persistent Connection API (represented in .NET code by the PersistentConnection class) gives the developer direct access to the low-level communication protocol that SignalR exposes.</span></span> <span data-ttu-id="07e17-189">接続の通信モデルを使用して Windows Communication Foundation など、接続ベースの Api を使用していた開発者にとって身近になります。</span><span class="sxs-lookup"><span data-stu-id="07e17-189">Using the Connections communication model will be familiar to developers who have used connection-based APIs such as Windows Communication Foundation.</span></span>

<span data-ttu-id="07e17-190">ハブは、クライアントとサーバーの相互に直接メソッドを呼び出すことができる接続 API に基づいて構築されたより高度なパイプラインです。</span><span class="sxs-lookup"><span data-stu-id="07e17-190">A Hub is a more high-level pipeline built upon the Connection API that allows your client and server to call methods on each other directly.</span></span> <span data-ttu-id="07e17-191">SignalR は、クライアントとして、ローカルのメソッドとして簡単にし、その逆の場合、サーバーでメソッドを呼び出すをできるように、不思議な力とした場合と同じコンピューターの境界を越えてディスパッチを処理します。</span><span class="sxs-lookup"><span data-stu-id="07e17-191">SignalR handles the dispatching across machine boundaries as if by magic, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="07e17-192">ハブの通信モデルを使用してリモート呼び出し .NET リモート処理などの Api を使用していた開発者にとって身近になります。</span><span class="sxs-lookup"><span data-stu-id="07e17-192">Using the Hubs communication model will be familiar to developers who have used remote invocation APIs such as .NET Remoting.</span></span> <span data-ttu-id="07e17-193">ハブを使用してではモデル バインドを有効にすると、メソッドに厳密に型指定されたパラメーターを渡すこともできます。</span><span class="sxs-lookup"><span data-stu-id="07e17-193">Using a Hub also allows you to pass strongly typed parameters to methods, enabling model binding.</span></span>

### <a name="architecture-diagram"></a><span data-ttu-id="07e17-194">アーキテクチャ図</span><span class="sxs-lookup"><span data-stu-id="07e17-194">Architecture diagram</span></span>

<span data-ttu-id="07e17-195">次の図は、ハブ、永続的な接続、およびトランスポートに使用される基になるテクノロジ間の関係を示します。</span><span class="sxs-lookup"><span data-stu-id="07e17-195">The following diagram shows the relationship between Hubs, Persistent Connections, and the underlying technologies used for transports.</span></span>

![Api、トランスポート、およびクライアントを示す、SignalR のアーキテクチャ図](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a><span data-ttu-id="07e17-197">ハブのしくみ</span><span class="sxs-lookup"><span data-stu-id="07e17-197">How Hubs work</span></span>

<span data-ttu-id="07e17-198">パケットが呼び出されるメソッドのパラメーターと名前を含むアクティブなトランスポート経由で送信されるときに、サーバー側コードでは、クライアントでメソッドを呼び出し、(オブジェクトがメソッド パラメーターとして送信されると、シリアル化される JSON を使用して)。</span><span class="sxs-lookup"><span data-stu-id="07e17-198">When server-side code calls a method on the client, a packet is sent across the active transport that contains the name and parameters of the method to be called (when an object is sent as a method parameter, it is serialized using JSON).</span></span> <span data-ttu-id="07e17-199">クライアントには、メソッド名をクライアント側のコードで定義されているメソッドを次と一致します。</span><span class="sxs-lookup"><span data-stu-id="07e17-199">The client then matches the method name to methods defined in client-side code.</span></span> <span data-ttu-id="07e17-200">一致がある場合、逆シリアル化されたパラメーターのデータを使用してクライアント メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="07e17-200">If there is a match, the client method will be executed using the deserialized parameter data.</span></span>

<span data-ttu-id="07e17-201">などのツールを使用して、メソッドの呼び出しを監視する[Fiddler します。](http://fiddler2.com/)</span><span class="sxs-lookup"><span data-stu-id="07e17-201">The method call can be monitored using tools like [Fiddler.](http://fiddler2.com/)</span></span> <span data-ttu-id="07e17-202">次の図は、Fiddler の [ログ] ウィンドウで、web ブラウザー クライアントに SignalR サーバーから送信されたメソッドの呼び出しを示します。</span><span class="sxs-lookup"><span data-stu-id="07e17-202">The following image shows a method call sent from a SignalR server to a web browser client in the Logs pane of Fiddler.</span></span> <span data-ttu-id="07e17-203">メソッドの呼び出しがという名前でハブから送信されて`MoveShapeHub`、呼び出されるメソッドが呼び出されて`updateShape`します。</span><span class="sxs-lookup"><span data-stu-id="07e17-203">The method call is being sent from a hub called `MoveShapeHub`, and the method being invoked is called `updateShape`.</span></span>

![SignalR のトラフィックを示す Fiddler ログの表示](introduction-to-signalr/_static/image6.png)

<span data-ttu-id="07e17-205">この例で、ハブの名前が付いて、`H`パラメーターは、メソッド名が付いて、`M`パラメーター、およびメソッドに送信されるデータがで識別される、`A`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="07e17-205">In this example, the hub name is identified with the `H` parameter; the method name is identified with the `M` parameter, and the data being sent to the method is identified with the `A` parameter.</span></span> <span data-ttu-id="07e17-206">このメッセージを生成したアプリケーションは、[高頻度リアルタイム メッセージング](tutorial-high-frequency-realtime-with-signalr.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="07e17-206">The application that generated this message is created in the [High-Frequency Realtime](tutorial-high-frequency-realtime-with-signalr.md) tutorial.</span></span>

### <a name="choosing-a-communication-model"></a><span data-ttu-id="07e17-207">通信モデルを選択します。</span><span class="sxs-lookup"><span data-stu-id="07e17-207">Choosing a communication model</span></span>

<span data-ttu-id="07e17-208">ほとんどのアプリケーションでは、ハブ API を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="07e17-208">Most applications should use the Hubs API.</span></span> <span data-ttu-id="07e17-209">次の状況では、接続 API を使用できます。</span><span class="sxs-lookup"><span data-stu-id="07e17-209">The Connections API could be used in the following circumstances:</span></span>

- <span data-ttu-id="07e17-210">指定する実際のメッセージ送信のニーズの形式です。</span><span class="sxs-lookup"><span data-stu-id="07e17-210">The format of the actual message sent needs to be specified.</span></span>
- <span data-ttu-id="07e17-211">開発者は、リモート呼び出しモデルではなく、メッセージおよびディスパッチ モデルを操作しています。</span><span class="sxs-lookup"><span data-stu-id="07e17-211">The developer prefers to work with a messaging and dispatching model rather than a remote invocation model.</span></span>
- <span data-ttu-id="07e17-212">SignalR を使用してメッセージング モデルを使用する既存のアプリケーションに移植されるは。</span><span class="sxs-lookup"><span data-stu-id="07e17-212">An existing application that uses a messaging model is being ported to use SignalR.</span></span>
