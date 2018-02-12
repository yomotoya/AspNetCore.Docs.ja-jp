---
uid: signalr/overview/getting-started/introduction-to-signalr
title: "SignalR の概要 |Microsoft ドキュメント"
author: pfletcher
description: "この記事は、SignalR は、作成するため、デザインされたソリューションの一部について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 64ac4e3575f69c164ba053943984ef25f906d7f4
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2018
---
<a name="introduction-to-signalr"></a><span data-ttu-id="67ad1-103">SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="67ad1-103">Introduction to SignalR</span></span>
====================
<span data-ttu-id="67ad1-104">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="67ad1-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="67ad1-105">この記事は、SignalR は、作成するため、デザインされたソリューションの一部について説明します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-105">This article describes what SignalR is, and some of the solutions it was designed to create.</span></span> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="67ad1-106">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="67ad1-106">Questions and comments</span></span>
> 
> <span data-ttu-id="67ad1-107">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="67ad1-107">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="67ad1-108">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="67ad1-108">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="what-is-signalr"></a><span data-ttu-id="67ad1-109">SignalR とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="67ad1-109">What is SignalR?</span></span>

<span data-ttu-id="67ad1-110">ASP.NET SignalR は、ASP.NET 開発者向けのアプリケーションにリアルタイム web 機能を追加するプロセスを簡略化するライブラリです。</span><span class="sxs-lookup"><span data-stu-id="67ad1-110">ASP.NET SignalR is a library for ASP.NET developers that simplifies the process of adding real-time web functionality to applications.</span></span> <span data-ttu-id="67ad1-111">リアルタイム web 機能とは、サーバーによって新しいデータを要求するクライアントを待機するのではなく、サーバー コード プッシュのように、使用可能になったら、接続クライアントに対して瞬時にコンテンツを保持する機能です。</span><span class="sxs-lookup"><span data-stu-id="67ad1-111">Real-time web functionality is the ability to have server code push content to connected clients instantly as it becomes available, rather than having the server wait for a client to request new data.</span></span>

<span data-ttu-id="67ad1-112">SignalR は ASP.NET アプリケーションに何らかの「リアルタイム」web 機能を追加するためにことができます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-112">SignalR can be used to add any sort of "real-time" web functionality to your ASP.NET application.</span></span> <span data-ttu-id="67ad1-113">チャットは例として使用される多くの場合中よりもずっとを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-113">While chat is often used as an example, you can do a whole lot more.</span></span> <span data-ttu-id="67ad1-114">いつでも、ユーザーに、新しいデータを表示する web ページを更新するか、ページを実装[ポーリング時間の長い](http://en.wikipedia.org/wiki/Push_technology#Long_polling)新しいデータを取得するが、候補 SignalR を使用するためです。</span><span class="sxs-lookup"><span data-stu-id="67ad1-114">Any time a user refreshes a web page to see new data, or the page implements [long polling](http://en.wikipedia.org/wiki/Push_technology#Long_polling) to retrieve new data, it is a candidate for using SignalR.</span></span> <span data-ttu-id="67ad1-115">例としては、ダッシュ ボードとアプリケーションの監視、共同作業などのアプリケーション (同時ドキュメントの編集)、ジョブの進行状況の更新プログラム、およびリアルタイム フォーム。</span><span class="sxs-lookup"><span data-stu-id="67ad1-115">Examples include dashboards and monitoring applications, collaborative applications (such as simultaneous editing of documents), job progress updates, and real-time forms.</span></span>

<span data-ttu-id="67ad1-116">SignalR では、完全に新しい種類の web アプリケーションをサーバーからの頻度の高い更新プログラムを必要とすることもできますリアルタイム ゲームなどです。</span><span class="sxs-lookup"><span data-stu-id="67ad1-116">SignalR also enables completely new types of web applications that require high frequency updates from the server, for example, real-time gaming.</span></span>

<span data-ttu-id="67ad1-117">SignalR には、サーバー側の .NET コードからブラウザー (およびその他のクライアント プラットフォーム) クライアントでの JavaScript 関数を呼び出してサーバーからクライアントへのリモート プロシージャ コール (RPC) を作成するための単純な API が用意されています。</span><span class="sxs-lookup"><span data-stu-id="67ad1-117">SignalR provides a simple API for creating server-to-client remote procedure calls (RPC) that call JavaScript functions in client browsers (and other client platforms) from server-side .NET code.</span></span> <span data-ttu-id="67ad1-118">SignalR には、接続の管理の API にはもが含まれています (接続し、切断イベントなど) との接続をグループ化します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-118">SignalR also includes API for connection management (for instance, connect and disconnect events), and grouping connections.</span></span>

![SignalR でメソッドを呼び出す](introduction-to-signalr/_static/image1.png)

<span data-ttu-id="67ad1-120">SignalR 接続の管理を自動的に処理し、ブロードキャスト メッセージをすべて接続されているクライアントに同時に、チャット ルームのようにできます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-120">SignalR handles connection management automatically, and lets you broadcast messages to all connected clients simultaneously, like a chat room.</span></span> <span data-ttu-id="67ad1-121">特定のクライアントにメッセージを送信することもできます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-121">You can also send messages to specific clients.</span></span> <span data-ttu-id="67ad1-122">クライアントとサーバー間の接続は、各通信の再確立される従来の HTTP 接続とは異なり、永続的です。</span><span class="sxs-lookup"><span data-stu-id="67ad1-122">The connection between the client and server is persistent, unlike a classic HTTP connection, which is re-established for each communication.</span></span>

<span data-ttu-id="67ad1-123">SignalR をサーバー コードを呼び出すクライアント コードを使用して一般的な要求/応答モデルではなく、リモート プロシージャ コール (RPC)、web で現在のブラウザーで、「サーバー プッシュ」機能がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="67ad1-123">SignalR supports "server push" functionality, in which server code can call out to client code in the browser using Remote Procedure Calls (RPC), rather than the request-response model common on the web today.</span></span>

<span data-ttu-id="67ad1-124">SignalR アプリケーションを Service Bus、SQL Server を使用してクライアントの数千にスケール アウトできますまたは[Redis](http://redis.io)です。</span><span class="sxs-lookup"><span data-stu-id="67ad1-124">SignalR applications can scale out to thousands of clients using Service Bus, SQL Server or [Redis](http://redis.io).</span></span>

<span data-ttu-id="67ad1-125">SignalR はオープン ソース、経由でアクセスできる[GitHub](https://github.com/signalr)です。</span><span class="sxs-lookup"><span data-stu-id="67ad1-125">SignalR is open-source, accessible through [GitHub](https://github.com/signalr).</span></span>

## <a name="signalr-and-websocket"></a><span data-ttu-id="67ad1-126">SignalR と WebSocket</span><span class="sxs-lookup"><span data-stu-id="67ad1-126">SignalR and WebSocket</span></span>

<span data-ttu-id="67ad1-127">SignalR では、使用可能な場合、新しい WebSocket トランスポートを使用し、必要に応じて、古いトランスポートにフォールバックします。</span><span class="sxs-lookup"><span data-stu-id="67ad1-127">SignalR uses the new WebSocket transport where available, and falls back to older transports where necessary.</span></span> <span data-ttu-id="67ad1-128">WebSocket を直接使用して、多数の追加の機能を実装する必要がありますが、既に SignalR 手段を使用して、アプリケーションを確実に記述することもできますが完了するのです。</span><span class="sxs-lookup"><span data-stu-id="67ad1-128">While you could certainly write your application using WebSocket directly, using SignalR means that a lot of the extra functionality you would need to implement will already have been done for you.</span></span> <span data-ttu-id="67ad1-129">最も重要なは、つまり、活用するために WebSocket 古いクライアントの個別のコード パスの作成について心配することがなく、アプリケーションをコーディングすることができます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-129">Most importantly, this means that you can code your application to take advantage of WebSocket without having to worry about creating a separate code path for older clients.</span></span> <span data-ttu-id="67ad1-130">SignalR も明らかにならないようにする SignalR は引き続き、基になるトランスポートでの変更をサポートするために更新する、アプリケーションが WebSocket の異なるバージョン間での一貫性のあるインターフェイスを提供するために、WebSocket への更新について心配する必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="67ad1-130">SignalR also shields you from having to worry about updates to WebSocket, since SignalR will continue to be updated to support changes in the underlying transport, providing your application a consistent interface across versions of WebSocket.</span></span>

<span data-ttu-id="67ad1-131">単独で WebSocket を使用してソリューションを作成することが確かに、SignalR はすべての他のトランスポートと WebSocket 実装への更新プログラム、アプリケーションへのフォールバックなどを自分で作成する必要があります機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-131">While you could certainly create a solution using WebSocket alone, SignalR provides all of the functionality you would need to write yourself, such as fallback to other transports and revising your application for updates to WebSocket implementations.</span></span>

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a><span data-ttu-id="67ad1-132">トランスポートとフォールバック</span><span class="sxs-lookup"><span data-stu-id="67ad1-132">Transports and fallbacks</span></span>

<span data-ttu-id="67ad1-133">SignalR は、クライアントとサーバー間のリアルタイムの作業を行うために必要なトランスポートの中に、抽象です。</span><span class="sxs-lookup"><span data-stu-id="67ad1-133">SignalR is an abstraction over some of the transports that are required to do real-time work between client and server.</span></span> <span data-ttu-id="67ad1-134">SignalR 接続では、HTTP、として起動し、使用可能になる場合、WebSocket 接続には、昇格します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-134">A SignalR connection starts as HTTP, and is then promoted to a WebSocket connection if it is available.</span></span> <span data-ttu-id="67ad1-135">WebSocket は、SignalR の理想的なトランスポートがサーバーのメモリの最も効率的に使用する、最短の待機時間があり、基になる最もなどの機能 (全二重通信クライアントとサーバー間)、いますが、最も厳しい要件: WebSocket が Windows Server 2012 または Windows 8、および .NET Framework 4.5 を使用するサーバーが必要です。</span><span class="sxs-lookup"><span data-stu-id="67ad1-135">WebSocket is the ideal transport for SignalR, since it makes the most efficient use of server memory, has the lowest latency, and has the most underlying features (such as full duplex communication between client and server), but it also has the most stringent requirements: WebSocket requires the server to be using Windows Server 2012 or Windows 8, and .NET Framework 4.5.</span></span> <span data-ttu-id="67ad1-136">これらの要件が満たされず、SignalR はその接続を確立する他のトランスポートを使用しようとします。</span><span class="sxs-lookup"><span data-stu-id="67ad1-136">If these requirements are not met, SignalR will attempt to use other transports to make its connections.</span></span>

### <a name="html-5-transports"></a><span data-ttu-id="67ad1-137">HTML 5 トランスポートします。</span><span class="sxs-lookup"><span data-stu-id="67ad1-137">HTML 5 transports</span></span>

<span data-ttu-id="67ad1-138">これらのトランスポートのサポートによって異なります[HTML 5](http://en.wikipedia.org/wiki/HTML5)です。</span><span class="sxs-lookup"><span data-stu-id="67ad1-138">These transports depend on support for [HTML 5](http://en.wikipedia.org/wiki/HTML5).</span></span> <span data-ttu-id="67ad1-139">クライアントのブラウザーが HTML 5 の標準をサポートしていない場合は、古いトランスポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-139">If the client browser does not support the HTML 5 standard, older transports will be used.</span></span>

- <span data-ttu-id="67ad1-140">**WebSocket** (場合、サーバーとブラウザーの両方は、Websocket をサポートすることを示します)。</span><span class="sxs-lookup"><span data-stu-id="67ad1-140">**WebSocket** (if the both the server and browser indicate they can support Websocket).</span></span> <span data-ttu-id="67ad1-141">WebSocket は、クライアントとサーバー間の場合は true。 永続的で双方向接続を確立するだけのトランスポートです。</span><span class="sxs-lookup"><span data-stu-id="67ad1-141">WebSocket is the only transport that establishes a true persistent, two-way connection between client and server.</span></span> <span data-ttu-id="67ad1-142">ただし、WebSocket も最も厳格な要件です。Microsoft Internet Explorer、Google Chrome、Mozilla Firefox、最新のバージョンでのみ完全にサポートし、のみ実装の一部は、元の Opera、Safari などその他のブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="67ad1-142">However, WebSocket also has the most stringent requirements; it is fully supported only in the latest versions of Microsoft Internet Explorer, Google Chrome, and Mozilla Firefox, and only has a partial implementation in other browsers such as Opera and Safari.</span></span>
- <span data-ttu-id="67ad1-143">**サーバー送信イベント**EventSource (ブラウザーがサポートする場合は Internet Explorer を除くすべてのブラウザーでは基本的にサーバー送信イベント、します) とも呼ばれる、。</span><span class="sxs-lookup"><span data-stu-id="67ad1-143">**Server Sent Events**, also known as EventSource (if the browser supports Server Sent Events, which is basically all browsers except Internet Explorer.)</span></span>

### <a name="comet-transports"></a><span data-ttu-id="67ad1-144">Comet トランスポート</span><span class="sxs-lookup"><span data-stu-id="67ad1-144">Comet transports</span></span>

<span data-ttu-id="67ad1-145">次のトランスポートがに基づいて、 [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web アプリケーションのモデル、ブラウザーまたは他のクライアントが、長時間保持されている HTTP 要求、具体的には、クライアントを使用せず、クライアントにデータをプッシュするサーバーを使用できるを維持それを要求します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-145">The following transports are based on the [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web application model, in which a browser or other client maintains a long-held HTTP request, which the server can use to push data to the client without the client specifically requesting it.</span></span>

- <span data-ttu-id="67ad1-146">**Forever Frame** (の Internet Explorer の場合のみ)。</span><span class="sxs-lookup"><span data-stu-id="67ad1-146">**Forever Frame** (for Internet Explorer only).</span></span> <span data-ttu-id="67ad1-147">永久にフレームでは、これは、要求が完了しない、サーバー上のエンドポイントを非表示の IFrame を作成します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-147">Forever Frame creates a hidden IFrame which makes a request to an endpoint on the server that does not complete.</span></span> <span data-ttu-id="67ad1-148">サーバーは、クライアントはすぐに実行するサーバーからクライアントへの一方向のリアルタイム接続を提供することをスクリプトを継続的に送信します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-148">The server then continually sends script to the client which is immediately executed, providing a one-way realtime connection from server to client.</span></span> <span data-ttu-id="67ad1-149">クライアントからサーバーへの接続がサーバーからクライアントへの個別の接続を使用し、個々 のデータを送信する必要があるため、標準の HTTP 要求と同様に、新しい接続を作成します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-149">The connection from client to server uses a separate connection from the server to client connection, and like a standard HTTP request, a new connection is created for each piece of data that needs to be sent.</span></span>
- <span data-ttu-id="67ad1-150">**Ajax ロング ポーリング**です。</span><span class="sxs-lookup"><span data-stu-id="67ad1-150">**Ajax long polling**.</span></span> <span data-ttu-id="67ad1-151">ロング ポーリングは、永続的な接続は作成されませんが、代わりに、サーバーは、この時点では、接続が閉じ、および新しい接続がすぐに要求されるまで開いたままのある要求を使用してサーバーをポーリングします。</span><span class="sxs-lookup"><span data-stu-id="67ad1-151">Long polling does not create a persistent connection, but instead polls the server with a request that stays open until the server responds, at which point the connection closes, and a new connection is requested immediately.</span></span> <span data-ttu-id="67ad1-152">接続のリセット中に、ある程度の遅延があります。</span><span class="sxs-lookup"><span data-stu-id="67ad1-152">This may introduce some latency while the connection resets.</span></span>

<span data-ttu-id="67ad1-153">どの構成でサポートされているどのようなトランスポートの詳細については、次を参照してください。[サポートされているプラットフォーム](supported-platforms.md)です。</span><span class="sxs-lookup"><span data-stu-id="67ad1-153">For more information on what transports are supported under which configurations, see [Supported Platforms](supported-platforms.md).</span></span>

### <a name="transport-selection-process"></a><span data-ttu-id="67ad1-154">トランスポート選択のプロセス</span><span class="sxs-lookup"><span data-stu-id="67ad1-154">Transport selection process</span></span>

<span data-ttu-id="67ad1-155">次の一覧は、SignalR を使用して使用するトランスポートを決定する手順を示します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-155">The following list shows the steps that SignalR uses to decide which transport to use.</span></span>

1. <span data-ttu-id="67ad1-156">ブラウザーが Internet Explorer 8 またはそれ以前の場合は、長いポーリングが使用されます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-156">If the browser is Internet Explorer 8 or earlier, Long Polling is used.</span></span>
2. <span data-ttu-id="67ad1-157">JSONP が構成されている場合 (つまり、`jsonp`にパラメーターが設定されている`true`接続が開始されたときに)、長いポーリングを使用します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-157">If JSONP is configured (that is, the `jsonp` parameter is set to `true` when the connection is started), Long Polling is used.</span></span>
3. <span data-ttu-id="67ad1-158">(つまり、SignalR エンドポイントがホストするページと同じドメインではない) 場合、クロス ドメイン接続が確立されている場合、WebSocket が適用されます、次の条件を満たしている場合。</span><span class="sxs-lookup"><span data-stu-id="67ad1-158">If a cross-domain connection is being made (that is, if the SignalR endpoint is not in the same domain as the hosting page), then WebSocket will be used if the following criteria are met:</span></span>

    - <span data-ttu-id="67ad1-159">クライアントは、CORS (クロス オリジン リソース共有) をサポートします。</span><span class="sxs-lookup"><span data-stu-id="67ad1-159">The client supports CORS (Cross-Origin Resource Sharing).</span></span> <span data-ttu-id="67ad1-160">詳細については CORS をサポートしているクライアントを次を参照してください。 [caniuse.com で CORS](http://www.caniuse.com/CORS)です。</span><span class="sxs-lookup"><span data-stu-id="67ad1-160">For details on which clients support CORS, see [CORS at caniuse.com](http://www.caniuse.com/CORS).</span></span>
    - <span data-ttu-id="67ad1-161">クライアントは、WebSocket をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="67ad1-161">The client supports WebSocket</span></span>
    - <span data-ttu-id="67ad1-162">このサーバーは、WebSocket をサポートします。</span><span class="sxs-lookup"><span data-stu-id="67ad1-162">The server supports WebSocket</span></span>

    <span data-ttu-id="67ad1-163">これらの条件が満たされない場合、長いポーリングが使用されます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-163">If any of these criteria are not met, Long Polling will be used.</span></span> <span data-ttu-id="67ad1-164">ドメイン間の接続の詳細については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)です。</span><span class="sxs-lookup"><span data-stu-id="67ad1-164">For more information on cross-domain connections, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>
4. <span data-ttu-id="67ad1-165">JSONP が構成されていない接続は、クロス ドメインではない場合は、WebSocket は、クライアントとサーバーの両方がサポートしている場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-165">If JSONP is not configured and the connection is not cross-domain, WebSocket will be used if both the client and server support it.</span></span>
5. <span data-ttu-id="67ad1-166">クライアントまたはサーバーのいずれかが WebSocket をサポートしていない場合は、使用可能になる場合にサーバー送信イベントが使用されます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-166">If either the client or server do not support WebSocket, Server Sent Events is used if it is available.</span></span>
6. <span data-ttu-id="67ad1-167">サーバー送信イベントが使用できない場合は、永久にフレームが試行されます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-167">If Server Sent Events is not available, Forever Frame is attempted.</span></span>
7. <span data-ttu-id="67ad1-168">無限のフレームが失敗すると、長いポーリングが使用されます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-168">If Forever Frame fails, Long Polling is used.</span></span>

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a><span data-ttu-id="67ad1-169">トランスポートの監視</span><span class="sxs-lookup"><span data-stu-id="67ad1-169">Monitoring transports</span></span>

<span data-ttu-id="67ad1-170">ハブで、ログに記録して、ブラウザーで、コンソール ウィンドウを開くようにすることで、アプリケーションが使用するトランスポートを指定できます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-170">You can determine what transport your application is using by enabling logging on your hub, and opening the console window in your browser.</span></span>

<span data-ttu-id="67ad1-171">ブラウザーで、ハブのイベントのログ記録を有効にするには、クライアント アプリケーションに、次のコマンドを追加します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-171">To enable logging for your hub's events in a browser, add the following command to your client application:</span></span>

`$.connection.hub.logging = true;`

- <span data-ttu-id="67ad1-172">Internet Explorer で、F12 キーを押して開発者ツールを開くし、コンソール タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="67ad1-172">In Internet Explorer, open the developer tools by pressing F12, and click the Console tab.</span></span>

    ![Microsoft Internet Explorer でコンソール](introduction-to-signalr/_static/image2.png)
- <span data-ttu-id="67ad1-174">Chrome では、ctrl キーと shift キーを押しながら J キーを押して、コンソールを開きます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-174">In Chrome, open the console by pressing Ctrl+Shift+J.</span></span>

    ![Google Chrome でコンソール](introduction-to-signalr/_static/image3.png)

<span data-ttu-id="67ad1-176">コンソールを開いて、ログを有効に、どのトランスポートは、SignalR によって使用されているを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-176">With the console open and logging enabled, you'll be able to see which transport is being used by SignalR.</span></span>

![Internet explorer が WebSocket トランスポートを示すコンソールします。](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a><span data-ttu-id="67ad1-178">トランスポートを指定します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-178">Specifying a transport</span></span>

<span data-ttu-id="67ad1-179">一定の時間とクライアント/サーバーをリソースには、トランスポートをネゴシエートします。</span><span class="sxs-lookup"><span data-stu-id="67ad1-179">Negotiating a transport takes a certain amount of time and client/server resources.</span></span> <span data-ttu-id="67ad1-180">クライアントの機能がわかっている場合、トランスポートを指定できます、クライアント接続が開始されたときに。</span><span class="sxs-lookup"><span data-stu-id="67ad1-180">If the client capabilities are known, then a transport can be specified when the client connection is started.</span></span> <span data-ttu-id="67ad1-181">次のコード スニペットでは、クライアントがその他のプロトコルをサポートしていないことことがわかっている場合に使用されるよう、Ajax の長いポーリング トランスポートを使用して、接続の開始を示しています。</span><span class="sxs-lookup"><span data-stu-id="67ad1-181">The following code snippet demonstrates starting a connection using the Ajax Long Polling transport, as would be used if it was known that the client did not support any other protocol:</span></span>

`connection.start({ transport: 'longPolling' });`

<span data-ttu-id="67ad1-182">クライアントの順序で特定のトランスポートの再試行をする場合は、フォールバックの順序を指定できます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-182">You can specify a fallback order if you want a client to try specific transports in order.</span></span> <span data-ttu-id="67ad1-183">次のコード スニペットでは、しようとしての WebSocket とそれがない場合、長いポーリングに直接移動を示しています。</span><span class="sxs-lookup"><span data-stu-id="67ad1-183">The following code snippet demonstrates trying WebSocket, and failing that, going directly to Long Polling.</span></span>

`connection.start({ transport: ['webSockets','longPolling'] });`

<span data-ttu-id="67ad1-184">トランスポートを指定する文字列定数の定義は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="67ad1-184">The string constants for specifying transports are defined as follows:</span></span>

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a><span data-ttu-id="67ad1-185">接続とハブ</span><span class="sxs-lookup"><span data-stu-id="67ad1-185">Connections and Hubs</span></span>

<span data-ttu-id="67ad1-186">SignalR の API には、クライアントとサーバー間で通信するための 2 つのモデルが含まれています: 永続的な接続とハブ。</span><span class="sxs-lookup"><span data-stu-id="67ad1-186">The SignalR API contains two models for communicating between clients and servers: Persistent Connections and Hubs.</span></span>

<span data-ttu-id="67ad1-187">接続では、受信者の 1 つ、グループ化、またはブロードキャスト メッセージを送信するための単純なエンドポイントを表します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-187">A Connection represents a simple endpoint for sending single-recipient, grouped, or broadcast messages.</span></span> <span data-ttu-id="67ad1-188">開発者は、SignalR を公開する低レベルの通信プロトコルへのアクセスを直接 (PersistentConnection クラスでは、.NET コードで表現) 永続的な接続 API によりします。</span><span class="sxs-lookup"><span data-stu-id="67ad1-188">The Persistent Connection API (represented in .NET code by the PersistentConnection class) gives the developer direct access to the low-level communication protocol that SignalR exposes.</span></span> <span data-ttu-id="67ad1-189">接続通信モデルを使用してに使い慣れた Windows Communication Foundation などの接続ベースの Api を使用している開発者になります。</span><span class="sxs-lookup"><span data-stu-id="67ad1-189">Using the Connections communication model will be familiar to developers who have used connection-based APIs such as Windows Communication Foundation.</span></span>

<span data-ttu-id="67ad1-190">ハブは、相互に直接メソッドを呼び出すには、クライアントとサーバーをできるようにする接続 API に基づいて構築されておりより高度なパイプラインです。</span><span class="sxs-lookup"><span data-stu-id="67ad1-190">A Hub is a more high-level pipeline built upon the Connection API that allows your client and server to call methods on each other directly.</span></span> <span data-ttu-id="67ad1-191">SignalR ディスパッチを処理する、コンピューターの境界を越えてマジック、した場合と同じサーバーで、ローカルのメソッドとして簡単にし、その逆としてメソッドを呼び出すには、クライアントできるようにします。</span><span class="sxs-lookup"><span data-stu-id="67ad1-191">SignalR handles the dispatching across machine boundaries as if by magic, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="67ad1-192">ハブ通信モデルを使用してにリモート呼び出し .NET リモート処理などの Api を使用している開発者にとって使い慣れたなります。</span><span class="sxs-lookup"><span data-stu-id="67ad1-192">Using the Hubs communication model will be familiar to developers who have used remote invocation APIs such as .NET Remoting.</span></span> <span data-ttu-id="67ad1-193">厳密に型指定されたパラメーターをメソッドに渡す、モデル バインディングを有効にするハブを使用できます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-193">Using a Hub also allows you to pass strongly typed parameters to methods, enabling model binding.</span></span>

### <a name="architecture-diagram"></a><span data-ttu-id="67ad1-194">アーキテクチャ図</span><span class="sxs-lookup"><span data-stu-id="67ad1-194">Architecture diagram</span></span>

<span data-ttu-id="67ad1-195">次の図は、ハブ、永続的な接続、および基になるテクノロジは、トランスポートの関係を示しています。</span><span class="sxs-lookup"><span data-stu-id="67ad1-195">The following diagram shows the relationship between Hubs, Persistent Connections, and the underlying technologies used for transports.</span></span>

![Api、トランスポート、およびクライアントを示す SignalR アーキテクチャ図](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a><span data-ttu-id="67ad1-197">ハブのしくみ</span><span class="sxs-lookup"><span data-stu-id="67ad1-197">How Hubs work</span></span>

<span data-ttu-id="67ad1-198">パケットが呼び出されるメソッドのパラメーターと名前を含むアクティブなトランスポート経由で送信されるときに、サーバー側コードでは、クライアントのメソッドを呼び出す、(オブジェクトが、メソッド パラメーターとして送信されると、シリアル化される JSON を使用して)。</span><span class="sxs-lookup"><span data-stu-id="67ad1-198">When server-side code calls a method on the client, a packet is sent across the active transport that contains the name and parameters of the method to be called (when an object is sent as a method parameter, it is serialized using JSON).</span></span> <span data-ttu-id="67ad1-199">クライアントは、クライアント側のコードで定義されたメソッドにメソッド名を一致します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-199">The client then matches the method name to methods defined in client-side code.</span></span> <span data-ttu-id="67ad1-200">一致があるの逆シリアル化されたパラメーターのデータを使用して、クライアントのメソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-200">If there is a match, the client method will be executed using the deserialized parameter data.</span></span>

<span data-ttu-id="67ad1-201">などのツールを使用して、メソッドの呼び出しを監視する[Fiddler です。](http://fiddler2.com/)</span><span class="sxs-lookup"><span data-stu-id="67ad1-201">The method call can be monitored using tools like [Fiddler.](http://fiddler2.com/)</span></span> <span data-ttu-id="67ad1-202">次の図は、SignalR のサーバーから Fiddler の [ログ] ウィンドウで web ブラウザー クライアントに送信されるメソッドの呼び出しを示しています。</span><span class="sxs-lookup"><span data-stu-id="67ad1-202">The following image shows a method call sent from a SignalR server to a web browser client in the Logs pane of Fiddler.</span></span> <span data-ttu-id="67ad1-203">メソッドの呼び出しと呼ばれるハブから送信されて`MoveShapeHub`、呼び出されるメソッドが呼び出されて`updateShape`です。</span><span class="sxs-lookup"><span data-stu-id="67ad1-203">The method call is being sent from a hub called `MoveShapeHub`, and the method being invoked is called `updateShape`.</span></span>

![SignalR のトラフィックを示す Fiddler ログの表示](introduction-to-signalr/_static/image6.png)

<span data-ttu-id="67ad1-205">この例ではハブ名が付いて、`H`パラメーター以外のメソッド名が付いて、`M`パラメーター、およびメソッドに送信されるデータが付いた、`A`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="67ad1-205">In this example, the hub name is identified with the `H` parameter; the method name is identified with the `M` parameter, and the data being sent to the method is identified with the `A` parameter.</span></span> <span data-ttu-id="67ad1-206">このメッセージを生成したアプリケーションを作成、[頻度が高いリアルタイム](tutorial-high-frequency-realtime-with-signalr.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="67ad1-206">The application that generated this message is created in the [High-Frequency Realtime](tutorial-high-frequency-realtime-with-signalr.md) tutorial.</span></span>

### <a name="choosing-a-communication-model"></a><span data-ttu-id="67ad1-207">通信モデルを選択します。</span><span class="sxs-lookup"><span data-stu-id="67ad1-207">Choosing a communication model</span></span>

<span data-ttu-id="67ad1-208">ほとんどのアプリケーションでは、ハブ API を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="67ad1-208">Most applications should use the Hubs API.</span></span> <span data-ttu-id="67ad1-209">次のような状況では、接続 API を使用できます。</span><span class="sxs-lookup"><span data-stu-id="67ad1-209">The Connections API could be used in the following circumstances:</span></span>

- <span data-ttu-id="67ad1-210">実際のメッセージを送信する必要がありますを指定する形式。</span><span class="sxs-lookup"><span data-stu-id="67ad1-210">The format of the actual message sent needs to be specified.</span></span>
- <span data-ttu-id="67ad1-211">開発者は、リモート起動のモデルではなく、メッセージおよびディスパッチのモデルを使用しています。</span><span class="sxs-lookup"><span data-stu-id="67ad1-211">The developer prefers to work with a messaging and dispatching model rather than a remote invocation model.</span></span>
- <span data-ttu-id="67ad1-212">メッセージング モデルを使用する既存のアプリケーションは、SignalR を使用するように移植したされているがします。</span><span class="sxs-lookup"><span data-stu-id="67ad1-212">An existing application that uses a messaging model is being ported to use SignalR.</span></span>
