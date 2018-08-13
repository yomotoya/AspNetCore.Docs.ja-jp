---
title: ASP.NET Core SignalR の概要
author: tdykstra
description: ASP.NET Core SignalR ライブラリがアプリへのリアルタイムの機能の追加を簡略化する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 2fff24609caf7592bad763a077288990a29617aa
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342550"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="75796-103">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="75796-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="75796-104">SignalR とは何か</span><span class="sxs-lookup"><span data-stu-id="75796-104">What is SignalR?</span></span>

<span data-ttu-id="75796-105">ASP.NET Core SignalR は、アプリへのリアルタイム Web 機能の追加を簡素化するオープン ソース ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="75796-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="75796-106">リアルタイム Web 機能は、サーバー側コードからクライアントにコンテンツを即座にプッシュすることを可能にします。</span><span class="sxs-lookup"><span data-stu-id="75796-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="75796-107">SignalR に適した候補:</span><span class="sxs-lookup"><span data-stu-id="75796-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="75796-108">サーバーから頻度の高い更新プログラムを必要とするアプリです。</span><span class="sxs-lookup"><span data-stu-id="75796-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="75796-109">例は、ゲーム、ソーシャル ネットワーク、投票、オークション、マップ、および GPS アプリです。</span><span class="sxs-lookup"><span data-stu-id="75796-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="75796-110">ダッシュ ボードとアプリを監視します。</span><span class="sxs-lookup"><span data-stu-id="75796-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="75796-111">例では、自社のダッシュ ボード、販売のインスタントの更新プログラムを含めるか、移動アラートします。</span><span class="sxs-lookup"><span data-stu-id="75796-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="75796-112">コラボレーション アプリケーション。</span><span class="sxs-lookup"><span data-stu-id="75796-112">Collaborative apps.</span></span> <span data-ttu-id="75796-113">ホワイト ボード アプリとチームのソフトウェアの会議、コラボレーション アプリケーションの例を示します。</span><span class="sxs-lookup"><span data-stu-id="75796-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="75796-114">通知を必要とするアプリです。</span><span class="sxs-lookup"><span data-stu-id="75796-114">Apps that require notifications.</span></span> <span data-ttu-id="75796-115">ソーシャル ネットワーク、電子メール、チャット、ゲーム、移動のアラート、およびその他の多くのアプリ通知を使用します。</span><span class="sxs-lookup"><span data-stu-id="75796-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="75796-116">SignalR がサーバーからクライアントを作成するための API を提供[リモート プロシージャ コール (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)します。</span><span class="sxs-lookup"><span data-stu-id="75796-116">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="75796-117">Rpc では、サーバー側の .NET Core コードからのクライアントで JavaScript 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="75796-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="75796-118">ASP.NET core SignalR の機能の一部を次に示します。</span><span class="sxs-lookup"><span data-stu-id="75796-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="75796-119">接続の管理を自動的に処理します。</span><span class="sxs-lookup"><span data-stu-id="75796-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="75796-120">同時に接続されているすべてのクライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="75796-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="75796-121">たとえば、チャット ルーム。</span><span class="sxs-lookup"><span data-stu-id="75796-121">For example, a chat room.</span></span>
* <span data-ttu-id="75796-122">特定のクライアントまたはクライアントのグループにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="75796-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="75796-123">トラフィックの増加の対応にスケールします。</span><span class="sxs-lookup"><span data-stu-id="75796-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="75796-124">[GitHub の SignalR リポジトリ](https://github.com/aspnet/signalr)にソースがホストされています。</span><span class="sxs-lookup"><span data-stu-id="75796-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/aspnet/signalr).</span></span>

## <a name="transports"></a><span data-ttu-id="75796-125">トランスポート</span><span class="sxs-lookup"><span data-stu-id="75796-125">Transports</span></span>

<span data-ttu-id="75796-126">SignalR には、リアルタイム通信を処理するためのいくつかの手法がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="75796-126">SignalR supports several techniques for handling real-time communications:</span></span>

* [<span data-ttu-id="75796-127">WebSocket</span><span class="sxs-lookup"><span data-stu-id="75796-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="75796-128">Server-Sent Events</span><span class="sxs-lookup"><span data-stu-id="75796-128">Server-Sent Events</span></span>
* <span data-ttu-id="75796-129">ロング ポーリング</span><span class="sxs-lookup"><span data-stu-id="75796-129">Long Polling</span></span>

<span data-ttu-id="75796-130">SignalR は、サーバーおよびクライアントの機能に応じて最適なトランスポート メソッドを自動的に選択します。</span><span class="sxs-lookup"><span data-stu-id="75796-130">SignalR automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="75796-131">ハブ</span><span class="sxs-lookup"><span data-stu-id="75796-131">Hubs</span></span>

<span data-ttu-id="75796-132">SignalR は、クライアントとサーバー間の通信に*ハブ*を使用します。</span><span class="sxs-lookup"><span data-stu-id="75796-132">SignalR uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="75796-133">ハブは、相互にメソッドを呼び出すには、クライアントとサーバーを許可する高度なパイプラインです。</span><span class="sxs-lookup"><span data-stu-id="75796-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> <span data-ttu-id="75796-134">SignalR は、クライアント、サーバーで、その逆のメソッドを呼び出すをできるように、自動的に、コンピューターの境界を越えてディスパッチを処理します。</span><span class="sxs-lookup"><span data-stu-id="75796-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="75796-135">厳密に型指定されたパラメーターは、モデル バインディングを有効、メソッドに渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="75796-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="75796-136">SignalR は、2 つのプロトコルの組み込みのハブを提供します。 テキスト プロトコルでは、JSON とに基づいてバイナリ プロトコルに基づいて[MessagePack](https://msgpack.org/)します。</span><span class="sxs-lookup"><span data-stu-id="75796-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="75796-137">MessagePack は一般に、JSON と比較して小さいメッセージを作成します。</span><span class="sxs-lookup"><span data-stu-id="75796-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="75796-138">古いブラウザーをサポートする必要があります[XHR レベル 2](https://caniuse.com/#feat=xhr2) MessagePack プロトコル サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="75796-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="75796-139">ハブは、クライアント側メソッドのパラメーターと名前を含むメッセージを送信することによってクライアント側のコードを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="75796-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="75796-140">メソッドのパラメーターとして送信されたオブジェクトが構成されているプロトコルを使用して逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="75796-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="75796-141">クライアントは、クライアント側コードのメソッド名と一致を試みます。</span><span class="sxs-lookup"><span data-stu-id="75796-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="75796-142">クライアントには、一致が検出されると、メソッドを呼び出すし、逆シリアル化されたパラメーター データを渡します。</span><span class="sxs-lookup"><span data-stu-id="75796-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75796-143">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="75796-143">Additional resources</span></span>

* [<span data-ttu-id="75796-144">ASP.NET Core の SignalR 概要</span><span class="sxs-lookup"><span data-stu-id="75796-144">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="75796-145">サポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="75796-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="75796-146">ハブ</span><span class="sxs-lookup"><span data-stu-id="75796-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="75796-147">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="75796-147">JavaScript client</span></span>](xref:signalr/javascript-client)
