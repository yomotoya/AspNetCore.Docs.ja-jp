---
uid: signalr/overview/performance/scaleout-in-signalr
title: "SignalR でスケール アウトの概要 |Microsoft ドキュメント"
author: MikeWasson
description: "ここ Visual Studio 2013 .NET 4.5 SignalR で使用するソフトウェアのバージョンはの以前のバージョンについてはこのトピックの以前のバージョンをバージョン 2."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: f1d15250682305f6d0512b72bd2e40cb4a8a18e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-scaleout-in-signalr"></a><span data-ttu-id="77cb8-103">SignalR でスケール アウトの概要</span><span class="sxs-lookup"><span data-stu-id="77cb8-103">Introduction to Scaleout in SignalR</span></span>
====================
<span data-ttu-id="77cb8-104">によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="77cb8-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="77cb8-105">このトピックで使用されているソフトウェア バージョン</span><span class="sxs-lookup"><span data-stu-id="77cb8-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="77cb8-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="77cb8-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="77cb8-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="77cb8-107">.NET 4.5</span></span>
> - <span data-ttu-id="77cb8-108">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="77cb8-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="77cb8-109">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="77cb8-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="77cb8-110">SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="77cb8-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="77cb8-111">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="77cb8-111">Questions and comments</span></span>
> 
> <span data-ttu-id="77cb8-112">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="77cb8-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="77cb8-113">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="77cb8-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="77cb8-114">一般は、web アプリケーションのスケールの 2 つの方法があります:*スケール アップ*と*スケール アウト*です。</span><span class="sxs-lookup"><span data-stu-id="77cb8-114">In general, there are two ways to scale a web application: *scale up* and *scale out*.</span></span>

- <span data-ttu-id="77cb8-115">スケール アップ RAM や Cpu などのより大きなサーバー (または大規模な VM) を使用することを意味します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-115">Scale up means using a larger server (or a larger VM) with more RAM, CPUs, etc.</span></span>
- <span data-ttu-id="77cb8-116">スケール アウトへの負荷がより多くのサーバーを追加します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-116">Scale out means adding more servers to handle the load.</span></span>

<span data-ttu-id="77cb8-117">スケール アップに関する問題は、マシンのサイズの制限を迅速にヒットすることです。</span><span class="sxs-lookup"><span data-stu-id="77cb8-117">The problem with scaling up is that you quickly hit a limit on the size of the machine.</span></span> <span data-ttu-id="77cb8-118">さらに、スケール アウトする必要があります。ただし、スケール アウトするときにクライアントを別のサーバーに送られますことができます。</span><span class="sxs-lookup"><span data-stu-id="77cb8-118">Beyond that, you need to scale out. However, when you scale out, clients can get routed to different servers.</span></span> <span data-ttu-id="77cb8-119">1 つのサーバーに接続されているクライアントは、別のサーバーから送信されたメッセージを受信しません。</span><span class="sxs-lookup"><span data-stu-id="77cb8-119">A client that is connected to one server will not receive messages sent from another server.</span></span>

![](scaleout-in-signalr/_static/image1.png)

<span data-ttu-id="77cb8-120">1 つのソリューションと呼ばれるコンポーネントを使用して、サーバー間でメッセージを転送する、*バック プレーン*です。</span><span class="sxs-lookup"><span data-stu-id="77cb8-120">One solution is to forward messages between servers, using a component called a *backplane*.</span></span> <span data-ttu-id="77cb8-121">有効になっているバック プレーンと各アプリケーション インスタンスが、バック プレーンにメッセージを送信バック プレーンに転送し、その他のアプリケーション インスタンス。</span><span class="sxs-lookup"><span data-stu-id="77cb8-121">With a backplane enabled, each application instance sends messages to the backplane, and the backplane forwards them to the other application instances.</span></span> <span data-ttu-id="77cb8-122">(エレクトロニクス バック プレーンは並列コネクタのグループです。</span><span class="sxs-lookup"><span data-stu-id="77cb8-122">(In electronics, a backplane is a group of parallel connectors.</span></span> <span data-ttu-id="77cb8-123">たとえて、SignalR のバック プレーン接続する複数のサーバーです。)</span><span class="sxs-lookup"><span data-stu-id="77cb8-123">By analogy, a SignalR backplane connects multiple servers.)</span></span>

![](scaleout-in-signalr/_static/image2.png)

<span data-ttu-id="77cb8-124">SignalR には、次の 3 つのバック プレーン現在提供しています。</span><span class="sxs-lookup"><span data-stu-id="77cb8-124">SignalR currently provides three backplanes:</span></span>

- <span data-ttu-id="77cb8-125">**Azure Service Bus**です。</span><span class="sxs-lookup"><span data-stu-id="77cb8-125">**Azure Service Bus**.</span></span> <span data-ttu-id="77cb8-126">Service Bus は、メッセージング インフラストラクチャにより、疎結合の方法でメッセージを送信するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="77cb8-126">Service Bus is a messaging infrastructure that allows components to send messages in a loosely coupled way.</span></span>
- <span data-ttu-id="77cb8-127">**Redis**です。</span><span class="sxs-lookup"><span data-stu-id="77cb8-127">**Redis**.</span></span> <span data-ttu-id="77cb8-128">Redis は、メモリ内キー値ストアです。</span><span class="sxs-lookup"><span data-stu-id="77cb8-128">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="77cb8-129">Redis は、メッセージを送信する (「パブリッシュ/サブスクライブ」) の公開/定期受信パターンをサポートします。</span><span class="sxs-lookup"><span data-stu-id="77cb8-129">Redis supports a publish/subscribe ("pub/sub") pattern for sending messages.</span></span>
- <span data-ttu-id="77cb8-130">**SQL Server**です。</span><span class="sxs-lookup"><span data-stu-id="77cb8-130">**SQL Server**.</span></span> <span data-ttu-id="77cb8-131">SQL Server のバック プレーンでは、SQL テーブルにメッセージを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="77cb8-131">The SQL Server backplane writes messages to SQL tables.</span></span> <span data-ttu-id="77cb8-132">バック プレーンでは、効率的なメッセージの Service Broker を使用します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-132">The backplane uses Service Broker for efficient messaging.</span></span> <span data-ttu-id="77cb8-133">ただし、これは Service Broker が有効になっていない場合にでも動作します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-133">However, it also works if Service Broker is not enabled.</span></span>

<span data-ttu-id="77cb8-134">Azure でアプリケーションを配置する場合は、Redis バック プレーンを使用して、使用を検討して[Azure Redis Cache](https://azure.microsoft.com/services/cache/)です。</span><span class="sxs-lookup"><span data-stu-id="77cb8-134">If you deploy your application on Azure, consider using the Redis backplane using [Azure Redis Cache](https://azure.microsoft.com/services/cache/).</span></span> <span data-ttu-id="77cb8-135">サーバー ファームに配置する場合は、SQL Server または Redis のバック プレーンに検討してください。</span><span class="sxs-lookup"><span data-stu-id="77cb8-135">If you are deploying to your own server farm, consider the SQL Server or Redis backplanes.</span></span>

<span data-ttu-id="77cb8-136">次のトピックには、各バック プレーンのステップバイ ステップ チュートリアルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="77cb8-136">The following topics contain step-by-step tutorials for each backplane:</span></span>

- [<span data-ttu-id="77cb8-137">Azure Service Bus による SignalR スケールアウト</span><span class="sxs-lookup"><span data-stu-id="77cb8-137">SignalR Scaleout with Azure Service Bus</span></span>](scaleout-with-windows-azure-service-bus.md)
- [<span data-ttu-id="77cb8-138">Redis による SignalR スケールアウト</span><span class="sxs-lookup"><span data-stu-id="77cb8-138">SignalR Scaleout with Redis</span></span>](scaleout-with-redis.md)
- [<span data-ttu-id="77cb8-139">SQL Server による SignalR スケールアウト</span><span class="sxs-lookup"><span data-stu-id="77cb8-139">SignalR Scaleout with SQL Server</span></span>](scaleout-with-sql-server.md)

## <a name="implementation"></a><span data-ttu-id="77cb8-140">実装</span><span class="sxs-lookup"><span data-stu-id="77cb8-140">Implementation</span></span>

<span data-ttu-id="77cb8-141">、SignalR メッセージ バスを介してすべてのメッセージが送信されます。</span><span class="sxs-lookup"><span data-stu-id="77cb8-141">In SignalR, every message is sent through a message bus.</span></span> <span data-ttu-id="77cb8-142">メッセージ バスを実装して、 [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)インターフェイスで、パブリッシュ/サブスクライブ抽象化を提供します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-142">A message bus implements the [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, which provides a publish/subscribe abstraction.</span></span> <span data-ttu-id="77cb8-143">既定値に置き換えることで、バック プレーンの作業**IMessageBus**そのバック プレーン用に設計されたバスとします。</span><span class="sxs-lookup"><span data-stu-id="77cb8-143">The backplanes work by replacing the default **IMessageBus** with a bus designed for that backplane.</span></span> <span data-ttu-id="77cb8-144">Redis メッセージ バスは、たとえば、 [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)、し、Redis を使用して[パブリッシュ/サブスクライブ](http://redis.io/topics/pubsub)メッセージを送信するためのメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="77cb8-144">For example, the message bus for Redis is [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), and it uses the Redis [pub/sub](http://redis.io/topics/pubsub) mechanism to send and receive messages.</span></span>

<span data-ttu-id="77cb8-145">各サーバー インスタンスは、バスを介してバック プレーンに接続します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-145">Each server instance connects to the backplane through the bus.</span></span> <span data-ttu-id="77cb8-146">メッセージが送信されると、バック プレーンに移動して、バック プレーンのすべてのサーバーに送信します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-146">When a message is sent, it goes to the backplane, and the backplane sends it to every server.</span></span> <span data-ttu-id="77cb8-147">サーバーでは、バック プレーンからメッセージが取得されるメッセージをローカル キャッシュに保存されます。</span><span class="sxs-lookup"><span data-stu-id="77cb8-147">When a server gets a message from the backplane, it puts the message in its local cache.</span></span> <span data-ttu-id="77cb8-148">サーバーはのローカル キャッシュから、クライアントにメッセージを配信します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-148">The server then delivers messages to clients from its local cache.</span></span>

<span data-ttu-id="77cb8-149">各クライアント接続のため、カーソルを使用して、メッセージ ストリームを読み取る際に、クライアントの進行状況が追跡されます。</span><span class="sxs-lookup"><span data-stu-id="77cb8-149">For each client connection, the client's progress in reading the message stream is tracked using a cursor.</span></span> <span data-ttu-id="77cb8-150">(カーソルは、メッセージ ストリーム内の位置を表します)。クライアントは、接続が切断され、再接続する場合、クライアントのカーソルの値の後に到着したメッセージのバスを要求します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-150">(A cursor represents a position in the message stream.) If a client disconnects and then reconnects, it asks the bus for any messages that arrived after the client's cursor value.</span></span> <span data-ttu-id="77cb8-151">接続を使用する場合に、同じことが起こります[ポーリング時間の長い](../getting-started/introduction-to-signalr.md#transports)です。</span><span class="sxs-lookup"><span data-stu-id="77cb8-151">The same thing happens when a connection uses [long polling](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="77cb8-152">長いポーリング要求が完了した後、クライアントは新しい接続が開かし、カーソルより後に受信したメッセージを要求します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-152">After a long poll request completes, the client opens a new connection and asks for messages that arrived after the cursor.</span></span>

<span data-ttu-id="77cb8-153">カーソルのメカニズムのしくみに、クライアントが別のサーバーにルーティングされる場合でも再接続します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-153">The cursor mechanism works even if a client is routed to a different server on reconnect.</span></span> <span data-ttu-id="77cb8-154">バック プレーンは、すべてのサーバーを認識しており、クライアントに接続するサーバーでもかまいません。</span><span class="sxs-lookup"><span data-stu-id="77cb8-154">The backplane is aware of all the servers, and it doesn't matter which server a client connects to.</span></span>

## <a name="limitations"></a><span data-ttu-id="77cb8-155">制限事項</span><span class="sxs-lookup"><span data-stu-id="77cb8-155">Limitations</span></span>

<span data-ttu-id="77cb8-156">バック プレーン使用すると、メッセージの最大スループットは、クライアントと 1 台のサーバー ノードに直接通信できる場合はよりも低いです。</span><span class="sxs-lookup"><span data-stu-id="77cb8-156">Using a backplane, the maximum message throughput is lower than it is when clients talk directly to a single server node.</span></span> <span data-ttu-id="77cb8-157">バック プレーンがバック プレーンがボトルネックになるために、すべてのノードにすべてのメッセージを転送するためです。</span><span class="sxs-lookup"><span data-stu-id="77cb8-157">That's because the backplane forwards every message to every node, so the backplane can become a bottleneck.</span></span> <span data-ttu-id="77cb8-158">この制限に問題があるかどうかは、アプリケーションによって異なります。</span><span class="sxs-lookup"><span data-stu-id="77cb8-158">Whether this limitation is a problem depends on the application.</span></span> <span data-ttu-id="77cb8-159">たとえば、SignalR の一般的なシナリオを次に示します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-159">For example, here are some typical SignalR scenarios:</span></span>

- <span data-ttu-id="77cb8-160">[サーバーのブロードキャスト](../getting-started/tutorial-server-broadcast-with-signalr.md)(たとえば、株価表示器): サーバーにメッセージが送信される速度を制御するために、バック プレーンがこのシナリオの適切に動作します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-160">[Server broadcast](../getting-started/tutorial-server-broadcast-with-signalr.md) (e.g., stock ticker): Backplanes work well for this scenario, because the server controls the rate at which messages are sent.</span></span>
- <span data-ttu-id="77cb8-161">[クライアントからクライアントへの](../getting-started/tutorial-getting-started-with-signalr.md)(チャットなど)。 このシナリオでバック プレーン可能性がありますするボトルネックになっている場合は、クライアントの数によるメッセージの数が拡大縮小。 つまり、メッセージの数が増えた場合比例して増えるにつれてクライアントが参加します。</span><span class="sxs-lookup"><span data-stu-id="77cb8-161">[Client-to-client](../getting-started/tutorial-getting-started-with-signalr.md) (e.g., chat): In this scenario, the backplane might be a bottleneck if the number of messages scales with the number of clients; that is, if the rate of messages grows proportionally as more clients join.</span></span>
- <span data-ttu-id="77cb8-162">[高周波数のリアルタイム](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)(リアルタイムのゲームなど)。 このシナリオのバック プレーンは推奨されません。</span><span class="sxs-lookup"><span data-stu-id="77cb8-162">[High-frequency realtime](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (e.g., real-time games): A backplane is not recommended for this scenario.</span></span>

## <a name="enabling-tracing-for-signalr-scaleout"></a><span data-ttu-id="77cb8-163">SignalR スケール アウトのトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="77cb8-163">Enabling Tracing For SignalR Scaleout</span></span>

<span data-ttu-id="77cb8-164">バック プレーンのトレースを有効にするには、web.config ファイルのルートの下に次のセクションを追加**構成**要素。</span><span class="sxs-lookup"><span data-stu-id="77cb8-164">To enable tracing for the backplanes, add the following sections to the web.config file, under the root **configuration** element:</span></span>

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
