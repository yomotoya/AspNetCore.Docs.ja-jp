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
<a name="introduction-to-signalr"></a><span data-ttu-id="a90b2-103">SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="a90b2-103">Introduction to SignalR</span></span>
====================

<span data-ttu-id="a90b2-104">参照してください[ASP.NET Core SignalR の概要](/aspnet/core/signalr/introduction)最新バージョンの Visual Studio を使用するこのチュートリアルの更新バージョン。</span><span class="sxs-lookup"><span data-stu-id="a90b2-104">See [Introduction to ASP.NET Core SignalR](/aspnet/core/signalr/introduction) for an updated version of this tutorial that uses the latest version of Visual Studio.</span></span> <span data-ttu-id="a90b2-105">新しいチュートリアルでは使用[ASP.NET Core](/aspnet/core/)、このチュートリアルで多くの機能強化を提供します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-105">The new tutorial uses [ASP.NET Core](/aspnet/core/), which provides many improvements over this tutorial.</span></span>

<span data-ttu-id="a90b2-106">提供者: [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="a90b2-106">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="a90b2-107">この記事では、SignalR は、いくつかのソリューションを作成するため、デザインされたについて説明します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-107">This article describes what SignalR is, and some of the solutions it was designed to create.</span></span> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="a90b2-108">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="a90b2-108">Questions and comments</span></span>
> 
> <span data-ttu-id="a90b2-109">このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。</span><span class="sxs-lookup"><span data-stu-id="a90b2-109">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="a90b2-110">チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr)にて投稿してください。</span><span class="sxs-lookup"><span data-stu-id="a90b2-110">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).</span></span>


## <a name="what-is-signalr"></a><span data-ttu-id="a90b2-111">SignalR とは何か</span><span class="sxs-lookup"><span data-stu-id="a90b2-111">What is SignalR?</span></span>

<span data-ttu-id="a90b2-112">ASP.NET SignalR は、リアルタイム Web 機能をアプリケーションに追加するプロセスを簡素化する ASP.NET 開発者向けのライブラリです。</span><span class="sxs-lookup"><span data-stu-id="a90b2-112">ASP.NET SignalR is a library for ASP.NET developers that simplifies the process of adding real-time web functionality to applications.</span></span> <span data-ttu-id="a90b2-113">リアルタイム Web 機能とは、サーバーがクライアントからの新しいデータ要求を待つのではなく、サーバー コードが接続クライアントに対して利用可能になったコンテンツを瞬時にプッシュできる機能です。</span><span class="sxs-lookup"><span data-stu-id="a90b2-113">Real-time web functionality is the ability to have server code push content to connected clients instantly as it becomes available, rather than having the server wait for a client to request new data.</span></span>

<span data-ttu-id="a90b2-114">SignalR は、ASP.NET アプリケーションに何らかの「リアルタイム」Web 機能を追加するために使用することができます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-114">SignalR can be used to add any sort of "real-time" web functionality to your ASP.NET application.</span></span> <span data-ttu-id="a90b2-115">よくチャットが例として使用されますが、もっと多くのことを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-115">While chat is often used as an example, you can do a whole lot more.</span></span> <span data-ttu-id="a90b2-116">ユーザーが新しいデータを表示するために Web ページを更新したり、ページが[ロングポーリング](http://en.wikipedia.org/wiki/Push_technology#Long_polling)を実装して新しいデータを取得したりする場合は、SignalR を使用することをおすすめします。</span><span class="sxs-lookup"><span data-stu-id="a90b2-116">Any time a user refreshes a web page to see new data, or the page implements [long polling](http://en.wikipedia.org/wiki/Push_technology#Long_polling) to retrieve new data, it is a candidate for using SignalR.</span></span> <span data-ttu-id="a90b2-117">サンプルとしては、ダッシュ ボード アプリケーションや監視アプリケーション、コラボレーション アプリケーション (ドキュメントの同時編集など)、ジョブの進行状況の更新プログラム、およびリアルタイム フォームが含まれます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-117">Examples include dashboards and monitoring applications, collaborative applications (such as simultaneous editing of documents), job progress updates, and real-time forms.</span></span>

<span data-ttu-id="a90b2-118">SignalR はリアルタイム ゲームなど、サーバーから高頻度で更新を必要とするまったく新しいタイプの Web アプリケーションを有効にできます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-118">SignalR also enables completely new types of web applications that require high frequency updates from the server, for example, real-time gaming.</span></span>

<span data-ttu-id="a90b2-119">SignalR は、サーバー側の .NET コードからクライアント ブラウザー (およびその他のクライアント プラットフォーム) に JavaScript 関数を呼び出す、サーバーからクライアントへのリモート プロシージャ コール (RPC) を作成するための単純な API を提供します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-119">SignalR provides a simple API for creating server-to-client remote procedure calls (RPC) that call JavaScript functions in client browsers (and other client platforms) from server-side .NET code.</span></span> <span data-ttu-id="a90b2-120">SignalR には、接続管理 (イベントの接続や切断など) やグループ接続のための API も含まれます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-120">SignalR also includes API for connection management (for instance, connect and disconnect events), and grouping connections.</span></span>

![SignalR によるメソッドの呼び出し](introduction-to-signalr/_static/image1.png)

<span data-ttu-id="a90b2-122">SignalR では、自動的に接続管理を処理し、チャット ルームのように、接続されているすべてのクライアントへのメッセージを同時に配信できます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-122">SignalR handles connection management automatically, and lets you broadcast messages to all connected clients simultaneously, like a chat room.</span></span> <span data-ttu-id="a90b2-123">特定のクライアントにメッセージを送信することもできます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-123">You can also send messages to specific clients.</span></span> <span data-ttu-id="a90b2-124">クライアントとサーバー間の接続は、通信ごとに再確立される従来の HTTP 接続とは異なり永続的です。</span><span class="sxs-lookup"><span data-stu-id="a90b2-124">The connection between the client and server is persistent, unlike a classic HTTP connection, which is re-established for each communication.</span></span>

<span data-ttu-id="a90b2-125">SignalR は「サーバー プッシュ」機能をサポートしています。この機能では、現在 Web において一般的な要求 - 応答モデルではなく、リモート プロシージャ コール (RPC) を使用して、サーバー コードによってブラウザー内のクライアント コードを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-125">SignalR supports "server push" functionality, in which server code can call out to client code in the browser using Remote Procedure Calls (RPC), rather than the request-response model common on the web today.</span></span>

<span data-ttu-id="a90b2-126">SignalR アプリケーションは、Service Bus、SQL Server または [Redis](http://redis.io) を使用して何千ものクライアントにスケール アウトできます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-126">SignalR applications can scale out to thousands of clients using Service Bus, SQL Server or [Redis](http://redis.io).</span></span>

<span data-ttu-id="a90b2-127">SignalR はオープンソースで、[GitHub](https://github.com/signalr) を通してアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-127">SignalR is open-source, accessible through [GitHub](https://github.com/signalr).</span></span>

## <a name="signalr-and-websocket"></a><span data-ttu-id="a90b2-128">SignalR と WebSocket</span><span class="sxs-lookup"><span data-stu-id="a90b2-128">SignalR and WebSocket</span></span>

<span data-ttu-id="a90b2-129">SignalR では、使用可能な場合は新しい WebSocket トランスポートを使用し、必要に応じて、古いトランスポートにフォールバックします。</span><span class="sxs-lookup"><span data-stu-id="a90b2-129">SignalR uses the new WebSocket transport where available and falls back to older transports where necessary.</span></span> <span data-ttu-id="a90b2-130">WebSocket を直接使用してアプリを作成することはできますが、SignalR を使用するメリットは、実装する必要のある他の機能の多くが既に作成されているという点です。</span><span class="sxs-lookup"><span data-stu-id="a90b2-130">While you could certainly write your app using WebSocket directly, using SignalR means that a lot of the extra functionality you would need to implement is already done for you.</span></span> <span data-ttu-id="a90b2-131">これにより、最も重要な事ととして、古いクライアントのために個別のコード パスを作成することなく、WebSocket を活用できるようにアプリをコーディングすることができます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-131">Most importantly, this means that you can code your app to take advantage of WebSocket without having to worry about creating a separate code path for older clients.</span></span> <span data-ttu-id="a90b2-132">SignalR は基本トランスポートの変更をサポートするために更新され、異なるバージョンの WebSocket でもアプリケーションにおいて一貫したインターフェイスが提供されるため、SignalR は WebSocket の更新に関する心配からユーザーを解放します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-132">SignalR also shields you from having to worry about updates to WebSocket, since SignalR is updated to support changes in the underlying transport, providing your application a consistent interface across versions of WebSocket.</span></span>

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a><span data-ttu-id="a90b2-133">トランスポートとフォールバック</span><span class="sxs-lookup"><span data-stu-id="a90b2-133">Transports and fallbacks</span></span>

<span data-ttu-id="a90b2-134">SignalR は、クライアントとサーバー間のリアルタイムの作業を行うために必要なトランスポートの一部を抽象化したものです。</span><span class="sxs-lookup"><span data-stu-id="a90b2-134">SignalR is an abstraction over some of the transports that are required to do real-time work between client and server.</span></span> <span data-ttu-id="a90b2-135">SignalR 接続は HTTP として起動し、WebSocket 接続が可能な場合は WebSocket 接続となります。</span><span class="sxs-lookup"><span data-stu-id="a90b2-135">A SignalR connection starts as HTTP, and is then promoted to a WebSocket connection if it is available.</span></span> <span data-ttu-id="a90b2-136">WebSocket は、サーバーのメモリを最も効率的に使用し、待機時間が最も短く、最も基本的な機能 (クライアントとサーバー間の全二重通信など) が備わっているため、SignalR の理想的なトランスポートですが、細かい要件があります。WebSocket では、Windows Server 2012 または Windows 8、および .NET Framework 4.5 を使用するサーバーが必要です。</span><span class="sxs-lookup"><span data-stu-id="a90b2-136">WebSocket is the ideal transport for SignalR, since it makes the most efficient use of server memory, has the lowest latency, and has the most underlying features (such as full duplex communication between client and server), but it also has the most stringent requirements: WebSocket requires the server to be using Windows Server 2012 or Windows 8, and .NET Framework 4.5.</span></span> <span data-ttu-id="a90b2-137">これらの要件が満たされない場合、SignalR は、接続するために他のトランスポートの使用を試みます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-137">If these requirements are not met, SignalR will attempt to use other transports to make its connections.</span></span>

### <a name="html-5-transports"></a><span data-ttu-id="a90b2-138">HTML 5 トランスポート</span><span class="sxs-lookup"><span data-stu-id="a90b2-138">HTML 5 transports</span></span>

<span data-ttu-id="a90b2-139">これらのトランスポートは [HTML 5](http://en.wikipedia.org/wiki/HTML5) のサポートに依存します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-139">These transports depend on support for [HTML 5](http://en.wikipedia.org/wiki/HTML5).</span></span> <span data-ttu-id="a90b2-140">クライアントのブラウザーが HTML 5 の標準をサポートしていない場合は、古いトランスポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-140">If the client browser does not support the HTML 5 standard, older transports will be used.</span></span>

- <span data-ttu-id="a90b2-141">**WebSocket** (サーバーとブラウザーの両方が Websocket をサポートすることを示す場合):</span><span class="sxs-lookup"><span data-stu-id="a90b2-141">**WebSocket** (if the both the server and browser indicate they can support Websocket).</span></span> <span data-ttu-id="a90b2-142">WebSocket は、クライアントとサーバー間の通信における真に永続的な双方向接続を確立するためのトランスポートです。</span><span class="sxs-lookup"><span data-stu-id="a90b2-142">WebSocket is the only transport that establishes a true persistent, two-way connection between client and server.</span></span> <span data-ttu-id="a90b2-143">ただし、WebSocket には細かい要件もあります。WebSocket は Microsoft Internet Explorer、Google Chrome、Mozilla Firefox の最新のバージョンでのみ完全にサポートされ、Opera や Safari などの他のブラウザーでは部分的にしか実装されません。</span><span class="sxs-lookup"><span data-stu-id="a90b2-143">However, WebSocket also has the most stringent requirements; it is fully supported only in the latest versions of Microsoft Internet Explorer, Google Chrome, and Mozilla Firefox, and only has a partial implementation in other browsers such as Opera and Safari.</span></span>
- <span data-ttu-id="a90b2-144">**Server Sent Events**: EventSource とも呼ばれます (ブラウザーがサーバー送信イベントをサポートする場合。基本的に Internet Explorer を除くすべてのブラウザーでサポートされます)。</span><span class="sxs-lookup"><span data-stu-id="a90b2-144">**Server Sent Events**, also known as EventSource (if the browser supports Server Sent Events, which is basically all browsers except Internet Explorer.)</span></span>

### <a name="comet-transports"></a><span data-ttu-id="a90b2-145">Comet トランスポート</span><span class="sxs-lookup"><span data-stu-id="a90b2-145">Comet transports</span></span>

<span data-ttu-id="a90b2-146">以下のトランスポートは、ブラウザーまたは他のクライアントが長期保持 HTTP リクエストを管理する [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) Web アプリケーションのモデルに基づいています。これは、クライアントからの特別な要求をすることなくサーバーからクライアントへデータをプッシュすることができます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-146">The following transports are based on the [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web application model, in which a browser or other client maintains a long-held HTTP request, which the server can use to push data to the client without the client specifically requesting it.</span></span>

- <span data-ttu-id="a90b2-147">**Forever Frame** (Internet Explorer の場合のみ):</span><span class="sxs-lookup"><span data-stu-id="a90b2-147">**Forever Frame** (for Internet Explorer only).</span></span> <span data-ttu-id="a90b2-148">Forever Frame は、サーバー上のエンドポイントへの要求が完了しない非表示の IFrame を作成します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-148">Forever Frame creates a hidden IFrame which makes a request to an endpoint on the server that does not complete.</span></span> <span data-ttu-id="a90b2-149">サーバーは、すぐに実行されるスクリプトを継続的にクライアントに送信し、サーバーからクライアントへの一方向のリアルタイム接続を提供します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-149">The server then continually sends script to the client which is immediately executed, providing a one-way realtime connection from server to client.</span></span> <span data-ttu-id="a90b2-150">クライアントからサーバーへの接続は、サーバーからクライアントとは異なる接続を使用し、標準の HTTP 要求のように、送信する必要のあるデータがあるたびに新しい接続が作成されます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-150">The connection from client to server uses a separate connection from the server to client connection, and like a standard HTTP request, a new connection is created for each piece of data that needs to be sent.</span></span>
- <span data-ttu-id="a90b2-151">**Ajax long polling**:</span><span class="sxs-lookup"><span data-stu-id="a90b2-151">**Ajax long polling**.</span></span> <span data-ttu-id="a90b2-152">Long Polling では永続的な接続は作成されませんが、サーバーが応答を完了するまで開いたままの要求でサーバーをポーリングします。接続が閉じられたら即座に新しい接続が要求されます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-152">Long polling does not create a persistent connection, but instead polls the server with a request that stays open until the server responds, at which point the connection closes, and a new connection is requested immediately.</span></span> <span data-ttu-id="a90b2-153">接続のリセット中に、いくつかの待機時間があります。</span><span class="sxs-lookup"><span data-stu-id="a90b2-153">This may introduce some latency while the connection resets.</span></span>

<span data-ttu-id="a90b2-154">どの構成でどのようなトランスポートがサポートされているかについての詳細は、[サポートされているプラットフォーム](supported-platforms.md)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a90b2-154">For more information on what transports are supported under which configurations, see [Supported Platforms](supported-platforms.md).</span></span>

### <a name="transport-selection-process"></a><span data-ttu-id="a90b2-155">トランスポート選択のプロセス</span><span class="sxs-lookup"><span data-stu-id="a90b2-155">Transport selection process</span></span>

<span data-ttu-id="a90b2-156">SignalR が使用するトランスポートを決定する手順を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-156">The following list shows the steps that SignalR uses to decide which transport to use.</span></span>

1. <span data-ttu-id="a90b2-157">ブラウザーが Internet Explorer 8 またはそれ以前の場合は、Long Polling が使用されます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-157">If the browser is Internet Explorer 8 or earlier, Long Polling is used.</span></span>
2. <span data-ttu-id="a90b2-158">JSONP が設定されている場合 (つまり、接続が開始されると `jsonp`パラメーターが `true`に設定される)、Long Polling が使用されます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-158">If JSONP is configured (that is, the `jsonp` parameter is set to `true` when the connection is started), Long Polling is used.</span></span>
3. <span data-ttu-id="a90b2-159">クロス ドメイン接続が確立されている場合 (つまり、SignalR エンドポイントが、ホスティング ページと同じドメインにない)、次の条件が満たされた場合に WebSocket が使用されます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-159">If a cross-domain connection is being made (that is, if the SignalR endpoint is not in the same domain as the hosting page), then WebSocket will be used if the following criteria are met:</span></span>

   - <span data-ttu-id="a90b2-160">クライアントが CORS (クロス オリジン リソース共有) をサポートしている。</span><span class="sxs-lookup"><span data-stu-id="a90b2-160">The client supports CORS (Cross-Origin Resource Sharing).</span></span> <span data-ttu-id="a90b2-161">CORS をサポートしているクライアントの詳細については、[caniuse.com での CORS](http://www.caniuse.com/CORS) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a90b2-161">For details on which clients support CORS, see [CORS at caniuse.com](http://www.caniuse.com/CORS).</span></span>
   - <span data-ttu-id="a90b2-162">クライアントが WebSocket をサポートしている。</span><span class="sxs-lookup"><span data-stu-id="a90b2-162">The client supports WebSocket</span></span>
   - <span data-ttu-id="a90b2-163">サーバーが WebSocket をサポートしている。</span><span class="sxs-lookup"><span data-stu-id="a90b2-163">The server supports WebSocket</span></span>

     <span data-ttu-id="a90b2-164">これらの条件のいずれかが満たされていない場合は、Long Polling が使用されます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-164">If any of these criteria are not met, Long Polling will be used.</span></span> <span data-ttu-id="a90b2-165">ドメイン間の接続の詳細については、「[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a90b2-165">For more information on cross-domain connections, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>
4. <span data-ttu-id="a90b2-166">JSONP が設定されておらず、ドメイン間接続でない場合は WebSocket が使用されます (クライアントとサーバーの両方がサポートしている場合)。</span><span class="sxs-lookup"><span data-stu-id="a90b2-166">If JSONP is not configured and the connection is not cross-domain, WebSocket will be used if both the client and server support it.</span></span>
5. <span data-ttu-id="a90b2-167">クライアントまたはサーバーのいずれかが WebSocket をサポートしていない場合は Server Sent Events が使用されます (利用可能に場合)。</span><span class="sxs-lookup"><span data-stu-id="a90b2-167">If either the client or server do not support WebSocket, Server Sent Events is used if it is available.</span></span>
6. <span data-ttu-id="a90b2-168">サーバー送信イベントが使用できない場合は、永久にフレームが試行されます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-168">If Server Sent Events is not available, Forever Frame is attempted.</span></span>
7. <span data-ttu-id="a90b2-169">Server Sent Events が使用できない場合は、Forever Frame が試されます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-169">If Forever Frame fails, Long Polling is used.</span></span>

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a><span data-ttu-id="a90b2-170">トランスポートの監視</span><span class="sxs-lookup"><span data-stu-id="a90b2-170">Monitoring transports</span></span>

<span data-ttu-id="a90b2-171">ハブでログを有効にし、ブラウザーでコンソール ウィンドウを開くことによって、アプリケーションで使用されているトランスポートを特定できます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-171">You can determine what transport your application is using by enabling logging on your hub, and opening the console window in your browser.</span></span>

<span data-ttu-id="a90b2-172">ブラウザーで、ハブのイベントのログ記録を有効にするには、クライアント アプリケーションに、次のコマンドを追加します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-172">To enable logging for your hub's events in a browser, add the following command to your client application:</span></span>

`$.connection.hub.logging = true;`

- <span data-ttu-id="a90b2-173">Internet Explorer で、F12 キーを押して開発者ツールを開き、[コンソール] タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a90b2-173">In Internet Explorer, open the developer tools by pressing F12, and click the Console tab.</span></span>

    ![Microsoft Internet Explorer でコンソール](introduction-to-signalr/_static/image2.png)
- <span data-ttu-id="a90b2-175">Chrome では、Ctrl + Shift + J キーを押して、コンソールを開きます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-175">In Chrome, open the console by pressing Ctrl+Shift+J.</span></span>

    ![Google Chrome でコンソール](introduction-to-signalr/_static/image3.png)

<span data-ttu-id="a90b2-177">コンソールが開いた状態で、ログが有効な場合は、SignalR で利用されているトランスポートを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-177">With the console open and logging enabled, you'll be able to see which transport is being used by SignalR.</span></span>

![Internet explorer が WebSocket トランスポートを示すコンソールします。](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a><span data-ttu-id="a90b2-179">トランスポートの指定</span><span class="sxs-lookup"><span data-stu-id="a90b2-179">Specifying a transport</span></span>

<span data-ttu-id="a90b2-180">トランスポートのネゴシエートには一定の時間を要し、クライアントとサーバーの一定量のリソースを使用します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-180">Negotiating a transport takes a certain amount of time and client/server resources.</span></span> <span data-ttu-id="a90b2-181">クライアントの機能が分かっている場合は、クライアント接続が開始された時点でトランスポートを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-181">If the client capabilities are known, then a transport can be specified when the client connection is started.</span></span> <span data-ttu-id="a90b2-182">次のコード スニペットでは、クラインが他のプロトコルをサポートしていないことが分かっている場合にも使用される、Ajax Long Polling のトランスポートを使用して接続を開始する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="a90b2-182">The following code snippet demonstrates starting a connection using the Ajax Long Polling transport, as would be used if it was known that the client did not support any other protocol:</span></span>

`connection.start({ transport: 'longPolling' });`

<span data-ttu-id="a90b2-183">クライアントが特定のトランスポートを順番に試行するようにする場合は、フォールバック順序を指定できます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-183">You can specify a fallback order if you want a client to try specific transports in order.</span></span> <span data-ttu-id="a90b2-184">次のコード スニペットは、WebSocket を試し、失敗した場合、Long Polling に直接移行する例を示します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-184">The following code snippet demonstrates trying WebSocket, and failing that, going directly to Long Polling.</span></span>

`connection.start({ transport: ['webSockets','longPolling'] });`

<span data-ttu-id="a90b2-185">トランスポートを指定する文字列定数の定義は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a90b2-185">The string constants for specifying transports are defined as follows:</span></span>

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a><span data-ttu-id="a90b2-186">接続とハブ</span><span class="sxs-lookup"><span data-stu-id="a90b2-186">Connections and Hubs</span></span>

<span data-ttu-id="a90b2-187">SignalR の API には、クライアントとサーバー間の通信に 2 つのモデルが含まれています: 永続的な接続とハブ。</span><span class="sxs-lookup"><span data-stu-id="a90b2-187">The SignalR API contains two models for communicating between clients and servers: Persistent Connections and Hubs.</span></span>

<span data-ttu-id="a90b2-188">接続は、単一受信者メッセージ、グループ化メッセージ、またはブロードキャスト メッセージを送信するための単純なエンドポイントを表します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-188">A Connection represents a simple endpoint for sending single-recipient, grouped, or broadcast messages.</span></span> <span data-ttu-id="a90b2-189">固定接続 API (PersistentConnection クラスによって .NET コードで表される) は、SignalR が公開している低レベルの通信プロトコルへの直接アクセスを開発者に提供します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-189">The Persistent Connection API (represented in .NET code by the PersistentConnection class) gives the developer direct access to the low-level communication protocol that SignalR exposes.</span></span> <span data-ttu-id="a90b2-190">接続の通信モデルは、Windows Communication Foundation などの接続に基づく API を使用したことのある開発者にとって使いやすいものです。</span><span class="sxs-lookup"><span data-stu-id="a90b2-190">Using the Connections communication model will be familiar to developers who have used connection-based APIs such as Windows Communication Foundation.</span></span>

<span data-ttu-id="a90b2-191">ハブは、クライアントとサーバーが相互に直接メソッドを呼び出すことができるようにする接続 API に基づいて構築されたより高度なパイプラインです。</span><span class="sxs-lookup"><span data-stu-id="a90b2-191">A Hub is a more high-level pipeline built upon the Connection API that allows your client and server to call methods on each other directly.</span></span> <span data-ttu-id="a90b2-192">SignalR は、マシンの境界でのディスパッチを魔法のように行い、クライアントがローカルのメソッドのように簡単にサーバーでメソッドを呼び出せるようにします (またはその逆)。</span><span class="sxs-lookup"><span data-stu-id="a90b2-192">SignalR handles the dispatching across machine boundaries as if by magic, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="a90b2-193">ハブ通信モデルは、.NET Remoting などのリモート呼び出し API を使用したことのある開発者にとって使いやすいものです。</span><span class="sxs-lookup"><span data-stu-id="a90b2-193">Using the Hubs communication model will be familiar to developers who have used remote invocation APIs such as .NET Remoting.</span></span> <span data-ttu-id="a90b2-194">また、ハブを使用すると、厳密に型指定されたパラメーターをメソッドに渡すことができ、モデルのバインディングが可能になります。</span><span class="sxs-lookup"><span data-stu-id="a90b2-194">Using a Hub also allows you to pass strongly typed parameters to methods, enabling model binding.</span></span>

### <a name="architecture-diagram"></a><span data-ttu-id="a90b2-195">アーキテクチャ図</span><span class="sxs-lookup"><span data-stu-id="a90b2-195">Architecture diagram</span></span>

<span data-ttu-id="a90b2-196">次の図は、ハブ、永続的な接続、およびトランスポートに使用される基になるテクノロジ間の関係を示します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-196">The following diagram shows the relationship between Hubs, Persistent Connections, and the underlying technologies used for transports.</span></span>

![Api、トランスポート、およびクライアントを示す、SignalR のアーキテクチャ図](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a><span data-ttu-id="a90b2-198">ハブのしくみ</span><span class="sxs-lookup"><span data-stu-id="a90b2-198">How Hubs work</span></span>

<span data-ttu-id="a90b2-199">サーバー側コードでは、クライアントでメソッドを呼び出すと、呼び出されるメソッドの名前とパラメーターを含むパケットがアクティブなトランスポート経由で送信されます (オブジェクトがメソッド パラメーターとして送信される場合、JSON を使用してシリアル化されます)。</span><span class="sxs-lookup"><span data-stu-id="a90b2-199">When server-side code calls a method on the client, a packet is sent across the active transport that contains the name and parameters of the method to be called (when an object is sent as a method parameter, it is serialized using JSON).</span></span> <span data-ttu-id="a90b2-200">次にクライアントは、メソッド名をクライアント側のコードで定義されているメソッドと一致させます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-200">The client then matches the method name to methods defined in client-side code.</span></span> <span data-ttu-id="a90b2-201">一致がある場合、逆シリアル化されたパラメーターのデータを使用してクライアント メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-201">If there is a match, the client method will be executed using the deserialized parameter data.</span></span>

<span data-ttu-id="a90b2-202">メソッドは、[Fiddler](http://fiddler2.com/) などのツールを使用して監視することができます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-202">The method call can be monitored using tools like [Fiddler.](http://fiddler2.com/)</span></span> <span data-ttu-id="a90b2-203">次のイメージは、Fiddler の [ログ] ウィンドウで、SignalR サーバーから Web ブラウザー クライアントに送信されたメソッドの呼び出しを示します。</span><span class="sxs-lookup"><span data-stu-id="a90b2-203">The following image shows a method call sent from a SignalR server to a web browser client in the Logs pane of Fiddler.</span></span> <span data-ttu-id="a90b2-204">メソッドの呼び出しは、`MoveShapeHub` というハブから送信され、呼び出されるメソッドは `updateShape`と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-204">The method call is being sent from a hub called `MoveShapeHub`, and the method being invoked is called `updateShape`.</span></span>

![SignalR のトラフィックを示す Fiddler ログの表示](introduction-to-signalr/_static/image6.png)

<span data-ttu-id="a90b2-206">この例では、ハブ名は`H`パラメーターで識別されます。メソッド名は、`M`パラメーターで識別され、メソッドに送信されるデータは`A`パラメーターで識別されます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-206">In this example, the hub name is identified with the `H` parameter; the method name is identified with the `M` parameter, and the data being sent to the method is identified with the `A` parameter.</span></span> <span data-ttu-id="a90b2-207">このメッセージを生成したアプリケーションは、[高頻度リアルタイム メッセージング](tutorial-high-frequency-realtime-with-signalr.md)チュートリアルで作成されます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-207">The application that generated this message is created in the [High-Frequency Realtime](tutorial-high-frequency-realtime-with-signalr.md) tutorial.</span></span>

### <a name="choosing-a-communication-model"></a><span data-ttu-id="a90b2-208">通信モデルの選択</span><span class="sxs-lookup"><span data-stu-id="a90b2-208">Choosing a communication model</span></span>

<span data-ttu-id="a90b2-209">ほとんどのアプリケーションでは、ハブ API を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a90b2-209">Most applications should use the Hubs API.</span></span> <span data-ttu-id="a90b2-210">接続 API は次の状況で使用できます。</span><span class="sxs-lookup"><span data-stu-id="a90b2-210">The Connections API could be used in the following circumstances:</span></span>

- <span data-ttu-id="a90b2-211">実際の送信メッセージの形式を指定する必要がある</span><span class="sxs-lookup"><span data-stu-id="a90b2-211">The format of the actual message sent needs to be specified.</span></span>
- <span data-ttu-id="a90b2-212">開発者が、リモート呼び出しモデルではなく、メッセージおよびディスパッチ モデルを利用する事を推奨している</span><span class="sxs-lookup"><span data-stu-id="a90b2-212">The developer prefers to work with a messaging and dispatching model rather than a remote invocation model.</span></span>
- <span data-ttu-id="a90b2-213">メッセージングモデルを使用する既存のアプリケーションが SignalR を使用するように移植されている</span><span class="sxs-lookup"><span data-stu-id="a90b2-213">An existing application that uses a messaging model is being ported to use SignalR.</span></span>
