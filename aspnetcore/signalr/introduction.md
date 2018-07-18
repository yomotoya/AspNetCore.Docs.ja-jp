---
title: ASP.NET Core SignalR の概要
author: tdykstra
description: ASP.NET Core SignalR ライブラリがアプリへのリアルタイムの機能の追加を簡略化する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095390"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="7bba5-103">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="7bba5-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="7bba5-104">作成者: [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="7bba5-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="7bba5-105">SignalR は何ですか。</span><span class="sxs-lookup"><span data-stu-id="7bba5-105">What is SignalR?</span></span>

<span data-ttu-id="7bba5-106">ASP.NET Core SignalR は、リアルタイム web 機能を追加してアプリを簡単にするライブラリです。</span><span class="sxs-lookup"><span data-stu-id="7bba5-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="7bba5-107">リアルタイム web 機能は、プッシュのコンテンツをクライアントにサーバー側コードをすぐになります。</span><span class="sxs-lookup"><span data-stu-id="7bba5-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="7bba5-108">SignalR に適した候補:</span><span class="sxs-lookup"><span data-stu-id="7bba5-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="7bba5-109">サーバーから頻度の高い更新プログラムを必要とするアプリです。</span><span class="sxs-lookup"><span data-stu-id="7bba5-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="7bba5-110">例は、ゲーム、ソーシャル ネットワーク、投票、オークション、マップ、および GPS アプリです。</span><span class="sxs-lookup"><span data-stu-id="7bba5-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="7bba5-111">ダッシュ ボードとアプリを監視します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="7bba5-112">例では、自社のダッシュ ボード、販売のインスタントの更新プログラムを含めるか、移動アラートします。</span><span class="sxs-lookup"><span data-stu-id="7bba5-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="7bba5-113">コラボレーション アプリケーション。</span><span class="sxs-lookup"><span data-stu-id="7bba5-113">Collaborative apps.</span></span> <span data-ttu-id="7bba5-114">ホワイト ボード アプリとチームのソフトウェアの会議、コラボレーション アプリケーションの例を示します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="7bba5-115">通知を必要とするアプリです。</span><span class="sxs-lookup"><span data-stu-id="7bba5-115">Apps that require notifications.</span></span> <span data-ttu-id="7bba5-116">ソーシャル ネットワーク、電子メール、チャット、ゲーム、移動のアラート、およびその他の多くのアプリ通知を使用します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="7bba5-117">SignalR がサーバーからクライアントを作成するための API を提供[リモート プロシージャ コール (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="7bba5-118">Rpc では、サーバー側の .NET Core コードからのクライアントで JavaScript 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="7bba5-119">ASP.NET core SignalR:</span><span class="sxs-lookup"><span data-stu-id="7bba5-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="7bba5-120">接続の管理を自動的に処理します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="7bba5-121">同時に接続されているすべてのクライアントにメッセージのブロードキャストを有効にします。</span><span class="sxs-lookup"><span data-stu-id="7bba5-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="7bba5-122">たとえば、チャット ルーム。</span><span class="sxs-lookup"><span data-stu-id="7bba5-122">For example, a chat room.</span></span>
* <span data-ttu-id="7bba5-123">特定のクライアントまたはクライアントのグループにメッセージを送信できるようにします。</span><span class="sxs-lookup"><span data-stu-id="7bba5-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="7bba5-124">オープン ソース化では、 [GitHub](https://github.com/aspnet/signalr)します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="7bba5-125">高い拡張性。</span><span class="sxs-lookup"><span data-stu-id="7bba5-125">Scalable.</span></span>

<span data-ttu-id="7bba5-126">クライアントとサーバー間の接続は、HTTP 接続とは異なり、永続的です。</span><span class="sxs-lookup"><span data-stu-id="7bba5-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="7bba5-127">トランスポート</span><span class="sxs-lookup"><span data-stu-id="7bba5-127">Transports</span></span>

<span data-ttu-id="7bba5-128">多くのリアルタイムの web アプリケーションを構築するための手法、SignalR の要約。</span><span class="sxs-lookup"><span data-stu-id="7bba5-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="7bba5-129">[Websocket](https://tools.ietf.org/html/rfc7118)で最適なトランスポートが、それらが使用できない場合、Server-Sent イベント、長いポーリングなどの他の手法を使用できます。</span><span class="sxs-lookup"><span data-stu-id="7bba5-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="7bba5-130">SignalR は自動的に検出し、サーバーとクライアントでサポートされる機能に基づいて、適切なトランスポートを初期化します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="7bba5-131">ハブ</span><span class="sxs-lookup"><span data-stu-id="7bba5-131">Hubs</span></span>

<span data-ttu-id="7bba5-132">SignalR では、クライアントとサーバー間の通信にハブを使用します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="7bba5-133">ハブは、他のメソッドを呼び出すには、クライアントとサーバーを許可する高度なパイプラインです。</span><span class="sxs-lookup"><span data-stu-id="7bba5-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="7bba5-134">SignalR は、クライアントとして、ローカルのメソッドとして簡単にし、その逆の場合、サーバーでメソッドを呼び出すをできるように、自動的に、コンピューターの境界を越えてディスパッチを処理します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="7bba5-135">ハブは、モデル バインディングを有効、メソッドに厳密に型指定されたパラメーターを渡します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="7bba5-136">SignalR は、2 つのプロトコルの組み込みのハブを提供します。 テキスト プロトコルでは、JSON とに基づいてバイナリ プロトコルに基づいて[MessagePack](https://msgpack.org/)します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="7bba5-137">MessagePack は一般に、JSON を使用するよりも小さいメッセージを作成します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="7bba5-138">古いブラウザーをサポートする必要があります[XHR レベル 2](https://caniuse.com/#feat=xhr2) MessagePack プロトコル サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="7bba5-139">ハブは、アクティブなトランスポートを使用してメッセージを送信することによって、クライアント側のコードを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="7bba5-140">メッセージには、クライアント側メソッドのパラメーターと名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7bba5-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="7bba5-141">メソッドのパラメーターとして送信されたオブジェクトが構成されているプロトコルを使用して逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="7bba5-142">クライアントは、クライアント側コードのメソッド名と一致を試みます。</span><span class="sxs-lookup"><span data-stu-id="7bba5-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="7bba5-143">一致が見つかると、クライアントのメソッドは、逆シリアル化されたパラメーターのデータを使用してを実行します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7bba5-144">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="7bba5-144">Additional resources</span></span>

* [<span data-ttu-id="7bba5-145">ASP.NET core SignalR を概要します。</span><span class="sxs-lookup"><span data-stu-id="7bba5-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="7bba5-146">サポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="7bba5-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="7bba5-147">ハブ</span><span class="sxs-lookup"><span data-stu-id="7bba5-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="7bba5-148">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="7bba5-148">JavaScript client</span></span>](xref:signalr/javascript-client)
