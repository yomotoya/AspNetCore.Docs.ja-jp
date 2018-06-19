---
title: ASP.NET Core SignalR の概要
author: rachelappel
description: ASP.NET Core SignalR ライブラリがリアルタイムの機能をアプリに追加する簡略化する方法について説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: f05b7cbf05372dc5d5cdadaf5a534d7a9d9bfecc
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
ms.locfileid: "33923356"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="48477-103">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="48477-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="48477-104">作成者: [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="48477-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="48477-105">SignalR とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="48477-105">What is SignalR?</span></span>

<span data-ttu-id="48477-106">ASP.NET Core SignalR は、アプリに追加のリアルタイム web 機能を簡略化するライブラリです。</span><span class="sxs-lookup"><span data-stu-id="48477-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="48477-107">リアルタイム web 機能は、クライアントへのプッシュ コンテンツ サーバー側コードを即座になります。</span><span class="sxs-lookup"><span data-stu-id="48477-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="48477-108">SignalR の適切な候補は:</span><span class="sxs-lookup"><span data-stu-id="48477-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="48477-109">高周波数の更新プログラムをサーバーを必要とするアプリです。</span><span class="sxs-lookup"><span data-stu-id="48477-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="48477-110">例は、ゲーム、ソーシャル ネットワーク、投票、オークション、マップ、および GPS アプリです。</span><span class="sxs-lookup"><span data-stu-id="48477-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="48477-111">ダッシュ ボードとアプリを監視します。</span><span class="sxs-lookup"><span data-stu-id="48477-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="48477-112">例では、自社のダッシュ ボード、販売のインスタントの更新プログラムを含めるか、アラートを移動します。</span><span class="sxs-lookup"><span data-stu-id="48477-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="48477-113">コラボレーション アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="48477-113">Collaborative apps.</span></span> <span data-ttu-id="48477-114">ホワイト ボード アプリとソフトウェアの会議チーム コラボレーション アプリケーションの例に示します。</span><span class="sxs-lookup"><span data-stu-id="48477-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="48477-115">通知を必要とするアプリです。</span><span class="sxs-lookup"><span data-stu-id="48477-115">Apps that require notifications.</span></span> <span data-ttu-id="48477-116">ソーシャル ネットワーク、電子メール、チャット、ゲーム、旅行のアラート、およびその他の多くのアプリは、通知を使用します。</span><span class="sxs-lookup"><span data-stu-id="48477-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="48477-117">SignalR では、サーバーからクライアントを作成するため、API が用意されています[リモート プロシージャ コール (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)です。</span><span class="sxs-lookup"><span data-stu-id="48477-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="48477-118">Rpc では、サーバー側の .NET Core コードから、クライアントでの JavaScript 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="48477-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="48477-119">ASP.NET Core の SignalR:</span><span class="sxs-lookup"><span data-stu-id="48477-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="48477-120">接続の管理を自動的に処理します。</span><span class="sxs-lookup"><span data-stu-id="48477-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="48477-121">同時に接続されているすべてのクライアントにメッセージのブロードキャストを有効にします。</span><span class="sxs-lookup"><span data-stu-id="48477-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="48477-122">チャット ルームなどです。</span><span class="sxs-lookup"><span data-stu-id="48477-122">For example, a chat room.</span></span>
* <span data-ttu-id="48477-123">特定のクライアントまたはクライアントのグループへのメッセージ送信を有効にします。</span><span class="sxs-lookup"><span data-stu-id="48477-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="48477-124">オープン ソースでは、 [GitHub](https://github.com/aspnet/signalr)です。</span><span class="sxs-lookup"><span data-stu-id="48477-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="48477-125">スケーラブルなです。</span><span class="sxs-lookup"><span data-stu-id="48477-125">Scalable.</span></span>

<span data-ttu-id="48477-126">クライアントとサーバー間の接続は HTTP 接続とは異なり、永続的です。</span><span class="sxs-lookup"><span data-stu-id="48477-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="48477-127">トランスポート</span><span class="sxs-lookup"><span data-stu-id="48477-127">Transports</span></span>

<span data-ttu-id="48477-128">リアルタイムの web アプリケーションを構築するための手法の数を SignalR 要約します。</span><span class="sxs-lookup"><span data-stu-id="48477-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="48477-129">[Websocket](https://tools.ietf.org/html/rfc7118)最適なトランスポートが、ものでは使用できないときに、Server-Sent イベントや長いポーリングのようなその他の手法を使用できます。</span><span class="sxs-lookup"><span data-stu-id="48477-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="48477-130">SignalR は自動的に検出して、サーバーとクライアントでサポートされる機能に基づいて、適切なトランスポートを初期化します。</span><span class="sxs-lookup"><span data-stu-id="48477-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="48477-131">ハブ</span><span class="sxs-lookup"><span data-stu-id="48477-131">Hubs</span></span>

<span data-ttu-id="48477-132">SignalR では、クライアントとサーバー間の通信にハブを使用します。</span><span class="sxs-lookup"><span data-stu-id="48477-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="48477-133">ハブは、クライアントとサーバーが互いにメソッドを呼び出すことができる高度なパイプラインが。</span><span class="sxs-lookup"><span data-stu-id="48477-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="48477-134">SignalR は、クライアントとして、ローカルのメソッドとして簡単にし、その逆の場合、サーバー上のメソッドを呼び出すをできるように、自動的に、コンピューターの境界を越えてディスパッチを処理します。</span><span class="sxs-lookup"><span data-stu-id="48477-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="48477-135">ハブは、モデル バインディングを有効にする方法を厳密に型指定されたパラメーターを渡すことです。</span><span class="sxs-lookup"><span data-stu-id="48477-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="48477-136">SignalR が 2 つの組み込みのハブ プロトコルを備えています。 テキスト プロトコルは、JSON とに基づいてバイナリ プロトコルに基づく[MessagePack](https://msgpack.org/)です。</span><span class="sxs-lookup"><span data-stu-id="48477-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="48477-137">MessagePack は通常 JSON を使用するよりも小さいメッセージを作成します。</span><span class="sxs-lookup"><span data-stu-id="48477-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="48477-138">古いバージョンのブラウザーをサポートする必要があります[XHR レベル 2](https://caniuse.com/#feat=xhr2) MessagePack プロトコルのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="48477-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="48477-139">ハブは、アクティブなトランスポートを使用してメッセージを送信することによって、クライアント側のコードを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="48477-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="48477-140">メッセージには、クライアント側のメソッドのパラメーターと名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="48477-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="48477-141">メソッド パラメーターとして送信されたオブジェクトが構成されているプロトコルを使用して逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="48477-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="48477-142">クライアントでは、クライアント側のコード内のメソッドに名前を照合します。</span><span class="sxs-lookup"><span data-stu-id="48477-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="48477-143">一致が見つかると、逆シリアル化されたパラメーター データを使用してクライアントのメソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="48477-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48477-144">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="48477-144">Additional resources</span></span>

* [<span data-ttu-id="48477-145">ASP.NET Core 用 SignalR を概要します。</span><span class="sxs-lookup"><span data-stu-id="48477-145">Get started with SignalR for ASP.NET Core</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="48477-146">サポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="48477-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="48477-147">ハブ</span><span class="sxs-lookup"><span data-stu-id="48477-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="48477-148">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="48477-148">JavaScript client</span></span>](xref:signalr/javascript-client)
