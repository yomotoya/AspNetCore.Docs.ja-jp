---
title: ASP.NET Core SignalR の運用ホスティングとスケーリング
author: bradygaster
description: パフォーマンスとスケーリングの ASP.NET Core SignalR を使用するアプリで問題を回避する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/scale
ms.openlocfilehash: 4ac4509acc89d0091a3757c7cfbc9981614f29ad
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836923"
---
# <a name="aspnet-core-signalr-hosting-and-scaling"></a><span data-ttu-id="28de7-103">ASP.NET Core SignalR ホスティングとスケーリング</span><span class="sxs-lookup"><span data-stu-id="28de7-103">ASP.NET Core SignalR hosting and scaling</span></span>

<span data-ttu-id="28de7-104">によって[Andrew Stanton-nurse](https://twitter.com/anurse)、 [Brady Gaster](https://twitter.com/bradygaster)、および[Tom Dykstra](https://github.com/tdykstra)、</span><span class="sxs-lookup"><span data-stu-id="28de7-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="28de7-105">この記事では、ホスティングとスケーリングの ASP.NET Core SignalR を使用するトラフィック量の多いアプリの考慮事項について説明します。</span><span class="sxs-lookup"><span data-stu-id="28de7-105">This article explains hosting and scaling considerations for high-traffic apps that use ASP.NET Core SignalR.</span></span>

## <a name="tcp-connection-resources"></a><span data-ttu-id="28de7-106">TCP 接続のリソース</span><span class="sxs-lookup"><span data-stu-id="28de7-106">TCP connection resources</span></span>

<span data-ttu-id="28de7-107">Web サーバーがサポートできる同時 TCP 接続の数は制限されます。</span><span class="sxs-lookup"><span data-stu-id="28de7-107">The number of concurrent TCP connections that a web server can support is limited.</span></span> <span data-ttu-id="28de7-108">標準の HTTP クライアントを使用して、*短期*接続します。</span><span class="sxs-lookup"><span data-stu-id="28de7-108">Standard HTTP clients use *ephemeral* connections.</span></span> <span data-ttu-id="28de7-109">クライアントは、アイドル状態し、後で再度開くときに、これらの接続を終了できます。</span><span class="sxs-lookup"><span data-stu-id="28de7-109">These connections can be closed when the client goes idle and reopened later.</span></span> <span data-ttu-id="28de7-110">SignalR 接続は、その一方で、*永続的な*します。</span><span class="sxs-lookup"><span data-stu-id="28de7-110">On the other hand, a SignalR connection is *persistent*.</span></span> <span data-ttu-id="28de7-111">SignalR 接続は、クライアントはアイドル状態時も開いたままです。</span><span class="sxs-lookup"><span data-stu-id="28de7-111">SignalR connections stay open even when the client goes idle.</span></span> <span data-ttu-id="28de7-112">多くのクライアント トラフィックの多いアプリをこれらの永続的な接続サーバー接続の最大数をヒットする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="28de7-112">In a high-traffic app that serves many clients, these persistent connections can cause servers to hit their maximum number of connections.</span></span>

<span data-ttu-id="28de7-113">永続的な接続では、各接続を追跡するために、追加のメモリも消費します。</span><span class="sxs-lookup"><span data-stu-id="28de7-113">Persistent connections also consume some additional memory, to track each connection.</span></span>

<span data-ttu-id="28de7-114">SignalR 接続に関連するリソースを多用する同じサーバーでホストされている他の web アプリに影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="28de7-114">The heavy use of connection-related resources by SignalR can affect other web apps that are hosted on the same server.</span></span> <span data-ttu-id="28de7-115">SignalR を開くし、最後の利用可能な TCP 接続を保持している、ときに、同じサーバー上の他の web アプリが使用できるようにする接続がもありません。</span><span class="sxs-lookup"><span data-stu-id="28de7-115">When SignalR opens and holds the last available TCP connections, other web apps on the same server also have no more connections available to them.</span></span>

<span data-ttu-id="28de7-116">サーバーは、接続から実行する場合は、ランダムなソケット エラーが表示されますおよび接続リセット エラー。</span><span class="sxs-lookup"><span data-stu-id="28de7-116">If a server runs out of connections, you'll see random socket errors and connection reset errors.</span></span> <span data-ttu-id="28de7-117">例:</span><span class="sxs-lookup"><span data-stu-id="28de7-117">For example:</span></span>

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

<span data-ttu-id="28de7-118">他の web apps でのエラーの原因から SignalR リソース使用率を維持するには、他の web アプリとは異なるサーバーで SignalR を実行します。</span><span class="sxs-lookup"><span data-stu-id="28de7-118">To keep SignalR resource usage from causing errors in other web apps, run SignalR on different servers than your other web apps.</span></span>

<span data-ttu-id="28de7-119">SignalR のリソース使用量が SignalR アプリでエラーが発生するを防ぐ、スケール アウト処理するために、サーバーには接続の数を制限します。</span><span class="sxs-lookup"><span data-stu-id="28de7-119">To keep SignalR resource usage from causing errors in a SignalR app, scale out to limit the number of connections a server has to handle.</span></span>

## <a name="scale-out"></a><span data-ttu-id="28de7-120">スケール アウト</span><span class="sxs-lookup"><span data-stu-id="28de7-120">Scale out</span></span>

<span data-ttu-id="28de7-121">SignalR を使用するアプリは、サーバー ファームの問題を作成しますが、そのすべての接続を追跡する必要があります。</span><span class="sxs-lookup"><span data-stu-id="28de7-121">An app that uses SignalR needs to keep track of all its connections, which creates problems for a server farm.</span></span> <span data-ttu-id="28de7-122">サーバーを追加し、他のサーバーについて把握していない新しい接続を取得します。</span><span class="sxs-lookup"><span data-stu-id="28de7-122">Add a server, and it gets new connections that the other servers don't know about.</span></span> <span data-ttu-id="28de7-123">たとえば、次の図の各サーバーで SignalR では、他のサーバー上の接続の認識しません。</span><span class="sxs-lookup"><span data-stu-id="28de7-123">For example, SignalR on each server in the following diagram is unaware of the connections on the other servers.</span></span> <span data-ttu-id="28de7-124">SignalR のサーバーのいずれかでは、すべてのクライアントにメッセージを送信する場合、そのサーバーに接続しているクライアントにのみ、メッセージが移動します。</span><span class="sxs-lookup"><span data-stu-id="28de7-124">When SignalR on one of the servers wants to send a message to all clients, the message only goes to the clients connected to that server.</span></span>

![SignalR のバック プレーンのないスケーリング](scale/_static/scale-no-backplane.png)

<span data-ttu-id="28de7-126">この問題を解決するためのオプションは、 [Azure SignalR サービス](#azure-signalr-service)と[バック プレーンの Redis](#redis-backplane)します。</span><span class="sxs-lookup"><span data-stu-id="28de7-126">The options for solving this problem are the [Azure SignalR Service](#azure-signalr-service) and [Redis backplane](#redis-backplane).</span></span>

## <a name="azure-signalr-service"></a><span data-ttu-id="28de7-127">Azure SignalR サービス</span><span class="sxs-lookup"><span data-stu-id="28de7-127">Azure SignalR Service</span></span>

<span data-ttu-id="28de7-128">Azure SignalR サービスには、バック プレーンではなく、プロキシです。</span><span class="sxs-lookup"><span data-stu-id="28de7-128">The Azure SignalR Service is a proxy rather than a backplane.</span></span> <span data-ttu-id="28de7-129">クライアントが、サーバーへの接続を開始するたびに、クライアントは、サービスへの接続にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="28de7-129">Each time a client initiates a connection to the server, the client is redirected to connect to the service.</span></span> <span data-ttu-id="28de7-130">そのプロセスは、次の図に示します。</span><span class="sxs-lookup"><span data-stu-id="28de7-130">That process is illustrated in the following diagram:</span></span>

![Azure SignalR サービスへの接続を確立します。](scale/_static/azure-signalr-service-one-connection.png)

<span data-ttu-id="28de7-132">結果は、サービスを管理するすべてのクライアント接続では、各サーバーは、次の図に示すように小さな一定数のサービスへの接続のみを必要があります。</span><span class="sxs-lookup"><span data-stu-id="28de7-132">The result is that the service manages all of the client connections, while each server needs only a small constant number of connections to the service, as shown in the following diagram:</span></span>

![クライアントは、サービス、サービスに接続されているサーバーに接続](scale/_static/azure-signalr-service-multiple-connections.png)

<span data-ttu-id="28de7-134">この方法でスケール アウトするには、Redis バック プレーンの代わりとして十いくつかの利点があります。</span><span class="sxs-lookup"><span data-stu-id="28de7-134">This approach to scale-out has several advantages over the Redis backplane alternative:</span></span>

* <span data-ttu-id="28de7-135">スティッキー セッションとも呼ばれます[クライアント アフィニティ](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)、接続するときに、クライアントは Azure SignalR サービスにリダイレクトされますすぐにために必須ではありません。</span><span class="sxs-lookup"><span data-stu-id="28de7-135">Sticky sessions, also known as [client affinity](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), is not required, because clients are immediately redirected to the Azure SignalR Service when they connect.</span></span>
* <span data-ttu-id="28de7-136">アプリがスケール アウト SignalR は、任意の数の接続を処理するために、Azure SignalR サービスが自動的にスケーリング中に送信されたメッセージの数に基づきます。</span><span class="sxs-lookup"><span data-stu-id="28de7-136">A SignalR app can scale out based on the number of messages sent, while the Azure SignalR Service automatically scales to handle any number of connections.</span></span> <span data-ttu-id="28de7-137">たとえば、何千ものクライアントがありますが、SignalR のアプリを接続自体の処理に複数のサーバーへのスケール アウトする必要はありません、1 秒あたりのいくつかのメッセージが送信専用の場合。</span><span class="sxs-lookup"><span data-stu-id="28de7-137">For example, there could be thousands of clients, but if only a few messages per second are sent, the SignalR app won't need to scale out to multiple servers just to handle the connections themselves.</span></span>
* <span data-ttu-id="28de7-138">SignalR アプリケーションでは、SignalR せず web アプリよりもはるかに多くの接続リソースを使用しません。</span><span class="sxs-lookup"><span data-stu-id="28de7-138">A SignalR app won't use significantly more connection resources than a web app without SignalR.</span></span>

<span data-ttu-id="28de7-139">これらの理由から、App Service、Vm、およびコンテナーを含む、Azure でホストされているすべての ASP.NET Core SignalR アプリ Azure SignalR サービスはお勧めします。</span><span class="sxs-lookup"><span data-stu-id="28de7-139">For these reasons, we recommend the Azure SignalR Service for all ASP.NET Core SignalR apps hosted on Azure, including App Service, VMs, and containers.</span></span>

<span data-ttu-id="28de7-140">詳細については、、 [Azure SignalR サービスのドキュメント](/azure/azure-signalr/signalr-overview)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="28de7-140">For more information see the [Azure SignalR Service documentation](/azure/azure-signalr/signalr-overview).</span></span>

## <a name="redis-backplane"></a><span data-ttu-id="28de7-141">Redis のバック プレーン</span><span class="sxs-lookup"><span data-stu-id="28de7-141">Redis backplane</span></span>

<span data-ttu-id="28de7-142">[Redis](https://redis.io/)はパブリッシュ/サブスクライブ モデルでのメッセージング システムをサポートするメモリ内キー値ストアです。</span><span class="sxs-lookup"><span data-stu-id="28de7-142">[Redis](https://redis.io/) is an in-memory key-value store that supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="28de7-143">SignalR で Redis バック プレーンでは、パブリッシュ/サブスクライブ機能を使用して、他のサーバーにメッセージを転送します。</span><span class="sxs-lookup"><span data-stu-id="28de7-143">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span> <span data-ttu-id="28de7-144">クライアントが接続した場合、接続情報は、バック プレーンに渡されます。</span><span class="sxs-lookup"><span data-stu-id="28de7-144">When a client makes a connection, the connection information is passed to the backplane.</span></span> <span data-ttu-id="28de7-145">サーバーは、すべてのクライアントにメッセージを送信する場合、バック プレーンに送信します。</span><span class="sxs-lookup"><span data-stu-id="28de7-145">When a server wants to send a message to all clients, it sends to the backplane.</span></span> <span data-ttu-id="28de7-146">接続されているすべてのクライアントとのバック プレーンを知っているサーバー上にいます。</span><span class="sxs-lookup"><span data-stu-id="28de7-146">The backplane knows all connected clients and which servers they're on.</span></span> <span data-ttu-id="28de7-147">それぞれのサーバーを経由してすべてのクライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="28de7-147">It sends the message to all clients via their respective servers.</span></span> <span data-ttu-id="28de7-148">このプロセスは、次の図に示します。</span><span class="sxs-lookup"><span data-stu-id="28de7-148">This process is illustrated in the following diagram:</span></span>

![Redis のバック プレーン、1 つのサーバーからすべてのクライアントに送信されるメッセージ](scale/_static/redis-backplane.png)

<span data-ttu-id="28de7-150">Redis のバック プレーンは、独自のインフラストラクチャでホストされているアプリの推奨されるスケール アウト アプローチです。</span><span class="sxs-lookup"><span data-stu-id="28de7-150">The Redis backplane is the recommended scale-out approach for apps hosted on your own infrastructure.</span></span> <span data-ttu-id="28de7-151">Azure SignalR サービスでは、運用環境で使用、データ センターと Azure データ センター間の接続の待機時間により、オンプレミスのアプリを使用して実用的な選択肢はありません。</span><span class="sxs-lookup"><span data-stu-id="28de7-151">Azure SignalR Service isn't a practical option for production use with on-premises apps due to connection latency between your data center and an Azure data center.</span></span>

<span data-ttu-id="28de7-152">前に説明した Azure SignalR サービスの利点は、Redis のバック プレーンの短所は。</span><span class="sxs-lookup"><span data-stu-id="28de7-152">The Azure SignalR Service advantages noted earlier are disadvantages for the Redis backplane:</span></span>

* <span data-ttu-id="28de7-153">スティッキー セッションとも呼ばれます[クライアント アフィニティ](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)が必要です。</span><span class="sxs-lookup"><span data-stu-id="28de7-153">Sticky sessions, also known as [client affinity](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), is required.</span></span> <span data-ttu-id="28de7-154">サーバー上の接続が開始されると、接続にはそのサーバーに留まりますが。</span><span class="sxs-lookup"><span data-stu-id="28de7-154">Once a connection is initiated on a server, the connection has to stay on that server.</span></span>
* <span data-ttu-id="28de7-155">SignalR のアプリのスケールする必要があります、いくつかのメッセージが送信される場合でもに、クライアントの数に基づいてアウト。</span><span class="sxs-lookup"><span data-stu-id="28de7-155">A SignalR app must scale out based on number of clients even if few messages are being sent.</span></span>
* <span data-ttu-id="28de7-156">SignalR アプリケーションでは、SignalR せず web アプリよりもはるかに多くの接続リソースを使用します。</span><span class="sxs-lookup"><span data-stu-id="28de7-156">A SignalR app uses significantly more connection resources than a web app without SignalR.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28de7-157">次の手順</span><span class="sxs-lookup"><span data-stu-id="28de7-157">Next steps</span></span>

<span data-ttu-id="28de7-158">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="28de7-158">For more information, see the following resources:</span></span>

* [<span data-ttu-id="28de7-159">Azure SignalR サービスのドキュメント</span><span class="sxs-lookup"><span data-stu-id="28de7-159">Azure SignalR Service documentation</span></span>](/azure/azure-signalr/signalr-overview)
* [<span data-ttu-id="28de7-160">Redis のバック プレーンを設定します。</span><span class="sxs-lookup"><span data-stu-id="28de7-160">Set up a Redis backplane</span></span>](xref:signalr/redis-backplane)
