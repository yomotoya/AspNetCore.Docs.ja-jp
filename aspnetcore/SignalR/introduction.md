---
title: ASP.NET Core SignalR の概要
author: rachelappel
description: ASP.NET Core SignalR ライブラリがリアルタイム web 機能を追加するアプリを簡略化する方法について説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: fa9b10201b5dc0e67bcd6d1321a3737e2025fda4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="43f55-103">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="43f55-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="43f55-104">作成者: [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="43f55-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>


[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-signalr"></a><span data-ttu-id="43f55-105">SignalR とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="43f55-105">What is SignalR?</span></span>

<span data-ttu-id="43f55-106">ASP.NET Core SignalR は、アプリに追加のリアルタイム web 機能を簡略化するライブラリです。</span><span class="sxs-lookup"><span data-stu-id="43f55-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="43f55-107">リアルタイム web 機能は、クライアントへのプッシュ コンテンツ サーバー側コードを即座になります。</span><span class="sxs-lookup"><span data-stu-id="43f55-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="43f55-108">SignalR の適切な候補は:</span><span class="sxs-lookup"><span data-stu-id="43f55-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="43f55-109">高周波数の更新プログラムをサーバーを必要とするアプリです。</span><span class="sxs-lookup"><span data-stu-id="43f55-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="43f55-110">例は、ゲーム、ソーシャル ネットワーク、投票、オークション、マップ、および GPS アプリです。</span><span class="sxs-lookup"><span data-stu-id="43f55-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="43f55-111">ダッシュ ボードとアプリを監視します。</span><span class="sxs-lookup"><span data-stu-id="43f55-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="43f55-112">例では、自社のダッシュ ボード、販売のインスタントの更新プログラムを含めるか、アラートを移動します。</span><span class="sxs-lookup"><span data-stu-id="43f55-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="43f55-113">コラボレーション アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="43f55-113">Collaborative apps.</span></span> <span data-ttu-id="43f55-114">ホワイト ボード アプリとソフトウェアの会議チーム コラボレーション アプリケーションの例に示します。</span><span class="sxs-lookup"><span data-stu-id="43f55-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="43f55-115">通知を必要とするアプリです。</span><span class="sxs-lookup"><span data-stu-id="43f55-115">Apps that require notifications.</span></span> <span data-ttu-id="43f55-116">ソーシャル ネットワーク、電子メール、チャット、ゲーム、旅行のアラート、およびその他の多くのアプリは、通知を使用します。</span><span class="sxs-lookup"><span data-stu-id="43f55-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="43f55-117">SignalR では、サーバーからクライアントを作成するため、API が用意されています[リモート プロシージャ コール (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)です。</span><span class="sxs-lookup"><span data-stu-id="43f55-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="43f55-118">Rpc では、サーバー側の .NET Core コードから、クライアントでの JavaScript 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="43f55-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="43f55-119">ASP.NET Core の SignalR:</span><span class="sxs-lookup"><span data-stu-id="43f55-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="43f55-120">接続の管理を自動的に処理します。</span><span class="sxs-lookup"><span data-stu-id="43f55-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="43f55-121">同時に接続されているすべてのクライアントにメッセージのブロードキャストを有効にします。</span><span class="sxs-lookup"><span data-stu-id="43f55-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="43f55-122">チャット ルームなどです。</span><span class="sxs-lookup"><span data-stu-id="43f55-122">For example, a chat room.</span></span>
* <span data-ttu-id="43f55-123">特定のクライアントまたはクライアントのグループへのメッセージ送信を有効にします。</span><span class="sxs-lookup"><span data-stu-id="43f55-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="43f55-124">オープン ソースでは、 [GitHub](https://github.com/aspnet/signalr)です。</span><span class="sxs-lookup"><span data-stu-id="43f55-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="43f55-125">スケーラブルなです。</span><span class="sxs-lookup"><span data-stu-id="43f55-125">Scalable.</span></span>

<span data-ttu-id="43f55-126">クライアントとサーバー間の接続は HTTP 接続とは異なり、永続的です。</span><span class="sxs-lookup"><span data-stu-id="43f55-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="43f55-127">トランスポート</span><span class="sxs-lookup"><span data-stu-id="43f55-127">Transports</span></span>

<span data-ttu-id="43f55-128">リアルタイムの web アプリケーションを構築するための手法の数を SignalR 要約します。</span><span class="sxs-lookup"><span data-stu-id="43f55-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="43f55-129">[Websocket](https://tools.ietf.org/html/rfc7118)最適なトランスポートが、ものでは使用できないときに、Server-Sent イベントや長いポーリングのようなその他の手法を使用できます。</span><span class="sxs-lookup"><span data-stu-id="43f55-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="43f55-130">SignalR は自動的に検出して、サーバーとクライアントでサポートされる機能に基づいて、適切なトランスポートを初期化します。</span><span class="sxs-lookup"><span data-stu-id="43f55-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs-and-endpoints"></a><span data-ttu-id="43f55-131">ハブとエンドポイント</span><span class="sxs-lookup"><span data-stu-id="43f55-131">Hubs and Endpoints</span></span>

<span data-ttu-id="43f55-132">SignalR では、ハブとエンドポイントを使用して、クライアントとサーバー間の通信をします。</span><span class="sxs-lookup"><span data-stu-id="43f55-132">SignalR uses Hubs and Endpoints to communicate between clients and servers.</span></span> <span data-ttu-id="43f55-133">ハブ API では、ほとんどのシナリオについて説明します。</span><span class="sxs-lookup"><span data-stu-id="43f55-133">The Hubs API covers the most scenarios.</span></span>

<span data-ttu-id="43f55-134">ハブは、クライアントとサーバーが互いにメソッドを呼び出すことができるエンドポイント API に基づいて構築されている高度なパイプラインが。</span><span class="sxs-lookup"><span data-stu-id="43f55-134">A hub is a high-level pipeline built upon the Endpoint API that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="43f55-135">SignalR は、クライアントとして、ローカルのメソッドとして簡単にし、その逆の場合、サーバー上のメソッドを呼び出すをできるように、自動的に、コンピューターの境界を越えてディスパッチを処理します。</span><span class="sxs-lookup"><span data-stu-id="43f55-135">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="43f55-136">ハブは、モデル バインディングを有効にする方法を厳密に型指定されたパラメーターを渡すことです。</span><span class="sxs-lookup"><span data-stu-id="43f55-136">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="43f55-137">SignalR が 2 つの組み込みのハブ プロトコルを備えています。 テキスト プロトコルは、JSON とに基づいてバイナリ プロトコルに基づく[MessagePack](https://msgpack.org/)です。</span><span class="sxs-lookup"><span data-stu-id="43f55-137">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="43f55-138">MessagePack は通常 JSON を使用するよりも小さいメッセージを作成します。</span><span class="sxs-lookup"><span data-stu-id="43f55-138">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="43f55-139">古いバージョンのブラウザーをサポートする必要があります[XHR レベル 2](https://caniuse.com/#feat=xhr2) MessagePack プロトコルのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="43f55-139">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="43f55-140">ハブは、アクティブなトランスポートを使用してメッセージを送信することによって、クライアント側のコードを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="43f55-140">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="43f55-141">メッセージには、クライアント側のメソッドのパラメーターと名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="43f55-141">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="43f55-142">メソッド パラメーターとして送信されたオブジェクトが構成されているプロトコルを使用して逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="43f55-142">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="43f55-143">クライアントでは、クライアント側のコード内のメソッドに名前を照合します。</span><span class="sxs-lookup"><span data-stu-id="43f55-143">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="43f55-144">一致が見つかると、逆シリアル化されたパラメーター データを使用してクライアントのメソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="43f55-144">When a match happens, the client method runs using the deserialized parameter data.</span></span>

<span data-ttu-id="43f55-145">エンドポイントは、クライアントから読み取りし、書き込みを有効にすると、生ソケットのような API を提供します。</span><span class="sxs-lookup"><span data-stu-id="43f55-145">Endpoints provide a raw socket-like API, enabling them to read and write from the client.</span></span> <span data-ttu-id="43f55-146">グループ化、ブロードキャスト、およびその他の機能を処理する開発者です。</span><span class="sxs-lookup"><span data-stu-id="43f55-146">It's up to the developer to handle grouping, broadcasting, and other functions.</span></span> <span data-ttu-id="43f55-147">ハブ API は、エンドポイントのレイヤーの上に作成されています。</span><span class="sxs-lookup"><span data-stu-id="43f55-147">The Hubs API is built on top of the Endpoints layer.</span></span>

<span data-ttu-id="43f55-148">次の図は、ハブ、エンドポイント、およびクライアント間のリレーションシップを示しています。</span><span class="sxs-lookup"><span data-stu-id="43f55-148">The following diagram shows the relationship between hubs, endpoints, and clients.</span></span>

![SignalR のマップ](introduction/_static/signalr-core-architecture.png)

## <a name="related-resources"></a><span data-ttu-id="43f55-150">関連資料</span><span class="sxs-lookup"><span data-stu-id="43f55-150">Related resources</span></span>

[<span data-ttu-id="43f55-151">ASP.NET Core 用 SignalR を概要します。</span><span class="sxs-lookup"><span data-stu-id="43f55-151">Get started with SignalR for ASP.NET Core</span></span>](xref:signalr/get-started)
