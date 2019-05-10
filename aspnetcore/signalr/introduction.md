---
title: ASP.NET Core SignalR の概要
author: bradygaster
description: ASP.NET Core SignalR ライブラリがアプリへのリアルタイムの機能の追加を簡略化する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 673efafce60dfa46cb99f9537fda2bca42bf9822
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892249"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="9508a-103">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="9508a-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="9508a-104">SignalR とは何か</span><span class="sxs-lookup"><span data-stu-id="9508a-104">What is SignalR?</span></span>

<span data-ttu-id="9508a-105">ASP.NET Core SignalR は、アプリへのリアルタイム Web 機能の追加を簡素化するオープン ソース ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="9508a-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="9508a-106">リアルタイム Web 機能は、サーバー側コードからクライアントにコンテンツを即座にプッシュすることを可能にします。</span><span class="sxs-lookup"><span data-stu-id="9508a-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="9508a-107">SignalR に適した候補:</span><span class="sxs-lookup"><span data-stu-id="9508a-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="9508a-108">サーバーから頻度の高い更新を必要とするアプリ。</span><span class="sxs-lookup"><span data-stu-id="9508a-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="9508a-109">例えば、ゲーム、ソーシャル ネットワーク、投票、オークション、マップ、および GPS アプリです。
</span><span class="sxs-lookup"><span data-stu-id="9508a-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="9508a-110">ダッシュ ボードや監視アプリ。</span><span class="sxs-lookup"><span data-stu-id="9508a-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="9508a-111">会社のダッシュ ボード、即座に更新される販売情報、渡航に関する危険情報、が例に含まれます。</span><span class="sxs-lookup"><span data-stu-id="9508a-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="9508a-112">コラボレーション アプリ。</span><span class="sxs-lookup"><span data-stu-id="9508a-112">Collaborative apps.</span></span> <span data-ttu-id="9508a-113">ホワイト ボード アプリやチーム会議ソフトウェアがコラボレーション アプリの例です。</span><span class="sxs-lookup"><span data-stu-id="9508a-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="9508a-114">通知を必要とするアプリ。</span><span class="sxs-lookup"><span data-stu-id="9508a-114">Apps that require notifications.</span></span> <span data-ttu-id="9508a-115">ソーシャル ネットワーク、電子メール、チャット、ゲーム、渡航に関する危険情報、およびその他の多くのアプリが通知を使用します。</span><span class="sxs-lookup"><span data-stu-id="9508a-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="9508a-116">SignalR は、サーバー・クライアント間の[リモート プロシージャ コール (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)を作成するための API を提供します。</span><span class="sxs-lookup"><span data-stu-id="9508a-116">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="9508a-117">Rpc では、サーバー側の .NET Core コードからのクライアントで JavaScript 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="9508a-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="9508a-118">ASP.NET core SignalR の機能の一部を次に示します。</span><span class="sxs-lookup"><span data-stu-id="9508a-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="9508a-119">接続の管理を自動的に処理します。</span><span class="sxs-lookup"><span data-stu-id="9508a-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="9508a-120">同時に接続されているすべてのクライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="9508a-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="9508a-121">例えば、チャット ルーム。</span><span class="sxs-lookup"><span data-stu-id="9508a-121">For example, a chat room.</span></span>
* <span data-ttu-id="9508a-122">特定のクライアントまたはクライアントのグループにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="9508a-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="9508a-123">トラフィックの増加の対応にスケールします。</span><span class="sxs-lookup"><span data-stu-id="9508a-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="9508a-124">[GitHub の SignalR リポジトリ](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR)にソースがホストされています。</span><span class="sxs-lookup"><span data-stu-id="9508a-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span></span>

## <a name="transports"></a><span data-ttu-id="9508a-125">トランスポート</span><span class="sxs-lookup"><span data-stu-id="9508a-125">Transports</span></span>

<span data-ttu-id="9508a-126">SignalR には、リアルタイム通信を処理するためのいくつかの手法がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="9508a-126">SignalR supports several techniques for handling real-time communications:</span></span>

* [<span data-ttu-id="9508a-127">WebSocket</span><span class="sxs-lookup"><span data-stu-id="9508a-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="9508a-128">Server-Sent Events</span><span class="sxs-lookup"><span data-stu-id="9508a-128">Server-Sent Events</span></span>
* <span data-ttu-id="9508a-129">ロング ポーリング</span><span class="sxs-lookup"><span data-stu-id="9508a-129">Long Polling</span></span>

<span data-ttu-id="9508a-130">SignalR は、サーバーおよびクライアントの機能に応じて最適なトランスポート メソッドを自動的に選択します。</span><span class="sxs-lookup"><span data-stu-id="9508a-130">SignalR automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="9508a-131">ハブ</span><span class="sxs-lookup"><span data-stu-id="9508a-131">Hubs</span></span>

<span data-ttu-id="9508a-132">SignalR は、クライアントとサーバー間の通信に*ハブ*を使用します。</span><span class="sxs-lookup"><span data-stu-id="9508a-132">SignalR uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="9508a-133">ハブは、相互にメソッドを呼び出すことをクライアントとサーバーに許可する、高度なパイプラインです。</span><span class="sxs-lookup"><span data-stu-id="9508a-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> <span data-ttu-id="9508a-134">SignalR は、コンピューターの境界を越えてディスパッチを自動的に処理します。これにより、クライアントはサーバーのメソッドを呼び出すことが許可され、その逆も許可されます。</span><span class="sxs-lookup"><span data-stu-id="9508a-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="9508a-135">モデル バインディングを有効にする厳密に型指定されたパラメーターを、メソッドに渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="9508a-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="9508a-136">SignalR は、2 つの組み込みハブプロトコルを提供します。JSONに基づくテキスト プロトコルと、[MessagePack](https://msgpack.org/)に基づくバイナリ プロトコルです。</span><span class="sxs-lookup"><span data-stu-id="9508a-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="9508a-137">MessagePack は一般に、JSON と比較して小さいメッセージを作成します。</span><span class="sxs-lookup"><span data-stu-id="9508a-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="9508a-138">MessagePack プロトコル サポートを提供するため、古いブラウザーは[XHR レベル 2](https://caniuse.com/#feat=xhr2)をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="9508a-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="9508a-139">ハブは、クライアント側メソッドのパラメーターと名前を含むメッセージを送信することによって、クライアント側のコードを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="9508a-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="9508a-140">メソッドのパラメーターとして送信されたオブジェクトは、設定されたプロトコルを使用して逆シリアル化されます。</span><span class="sxs-lookup"><span data-stu-id="9508a-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="9508a-141">クライアントは、クライアント側コードのメソッド名との一致を試みます。</span><span class="sxs-lookup"><span data-stu-id="9508a-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="9508a-142">クライアントが一致を検出すると、逆シリアル化されたパラメーター データを渡してメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="9508a-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9508a-143">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="9508a-143">Additional resources</span></span>

* [<span data-ttu-id="9508a-144">ASP.NET Core の SignalR 概要</span><span class="sxs-lookup"><span data-stu-id="9508a-144">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="9508a-145">サポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="9508a-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="9508a-146">ハブ</span><span class="sxs-lookup"><span data-stu-id="9508a-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="9508a-147">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="9508a-147">JavaScript client</span></span>](xref:signalr/javascript-client)
