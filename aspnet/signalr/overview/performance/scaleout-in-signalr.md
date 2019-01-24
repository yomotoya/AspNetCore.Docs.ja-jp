---
uid: signalr/overview/performance/scaleout-in-signalr
title: SignalR のスケール アウト入門 |Microsoft Docs
author: bradygaster
description: このトピックの「Visual Studio 2013 .NET 4.5 SignalR 使用されるソフトウェアのバージョンは以前のバージョンについてはこのトピック以前バージョンをバージョン 2.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: 60ae0353745284796eb7e0ddb6397ecb48eceaf0
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836205"
---
<a name="introduction-to-scaleout-in-signalr"></a><span data-ttu-id="a0d91-103">SignalR のスケール アウト入門</span><span class="sxs-lookup"><span data-stu-id="a0d91-103">Introduction to Scaleout in SignalR</span></span>
====================
<span data-ttu-id="a0d91-104">によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="a0d91-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="a0d91-105">このトピックで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="a0d91-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="a0d91-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a0d91-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="a0d91-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a0d91-107">.NET 4.5</span></span>
> - <span data-ttu-id="a0d91-108">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="a0d91-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="a0d91-109">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="a0d91-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="a0d91-110">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="a0d91-111">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="a0d91-111">Questions and comments</span></span>
>
> <span data-ttu-id="a0d91-112">このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。</span><span class="sxs-lookup"><span data-stu-id="a0d91-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="a0d91-113">チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。</span><span class="sxs-lookup"><span data-stu-id="a0d91-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="a0d91-114">一般は、web アプリケーションのスケール設定の 2 つの方法があります:*スケール アップ*と*スケール アウト*します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-114">In general, there are two ways to scale a web application: *scale up* and *scale out*.</span></span>

- <span data-ttu-id="a0d91-115">スケール アップは、RAM、Cpu などの大規模なサーバー (またはより大きな VM) を使用することを意味します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-115">Scale up means using a larger server (or a larger VM) with more RAM, CPUs, etc.</span></span>
- <span data-ttu-id="a0d91-116">スケール アウト、負荷を処理するより多くのサーバーを追加することを意味します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-116">Scale out means adding more servers to handle the load.</span></span>

<span data-ttu-id="a0d91-117">スケール アップの問題は、迅速に、マシンのサイズの上限に達したことです。</span><span class="sxs-lookup"><span data-stu-id="a0d91-117">The problem with scaling up is that you quickly hit a limit on the size of the machine.</span></span> <span data-ttu-id="a0d91-118">さらに、スケール アウトする必要があります。ただし、スケール アウトするときにクライアントを別のサーバーにルーティングを取得できます。</span><span class="sxs-lookup"><span data-stu-id="a0d91-118">Beyond that, you need to scale out. However, when you scale out, clients can get routed to different servers.</span></span> <span data-ttu-id="a0d91-119">1 つのサーバーに接続されているクライアントは、別のサーバーから送信されたメッセージを受信しません。</span><span class="sxs-lookup"><span data-stu-id="a0d91-119">A client that is connected to one server will not receive messages sent from another server.</span></span>

![](scaleout-in-signalr/_static/image1.png)

<span data-ttu-id="a0d91-120">1 つのソリューションと呼ばれるコンポーネントを使用して、サーバー間でメッセージを転送するように、*バック プレーン*します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-120">One solution is to forward messages between servers, using a component called a *backplane*.</span></span> <span data-ttu-id="a0d91-121">有効になっているバック プレーン、各アプリケーション インスタンスが、バック プレーンにメッセージを送信し、バック プレーンの他のアプリケーション インスタンスに転送します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-121">With a backplane enabled, each application instance sends messages to the backplane, and the backplane forwards them to the other application instances.</span></span> <span data-ttu-id="a0d91-122">(電子機器、バック プレーンです並列コネクタのグループ。</span><span class="sxs-lookup"><span data-stu-id="a0d91-122">(In electronics, a backplane is a group of parallel connectors.</span></span> <span data-ttu-id="a0d91-123">たとえて、SignalR のバック プレーン接続する複数のサーバーです。)</span><span class="sxs-lookup"><span data-stu-id="a0d91-123">By analogy, a SignalR backplane connects multiple servers.)</span></span>

![](scaleout-in-signalr/_static/image2.png)

<span data-ttu-id="a0d91-124">現在、SignalR は、次の 3 つのバック プレーンを提供します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-124">SignalR currently provides three backplanes:</span></span>

- <span data-ttu-id="a0d91-125">**Azure Service Bus**します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-125">**Azure Service Bus**.</span></span> <span data-ttu-id="a0d91-126">Service Bus は、メッセージング インフラストラクチャを使用する疎結合方式でメッセージを送信するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="a0d91-126">Service Bus is a messaging infrastructure that allows components to send messages in a loosely coupled way.</span></span>
- <span data-ttu-id="a0d91-127">**Redis**します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-127">**Redis**.</span></span> <span data-ttu-id="a0d91-128">Redis はメモリ内のキー値ストアです。</span><span class="sxs-lookup"><span data-stu-id="a0d91-128">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="a0d91-129">Redis では、メッセージを送信するため、(「パブリッシュ/サブスクライブ」) の発行/サブスクライブ パターンをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="a0d91-129">Redis supports a publish/subscribe ("pub/sub") pattern for sending messages.</span></span>
- <span data-ttu-id="a0d91-130">**SQL Server**します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-130">**SQL Server**.</span></span> <span data-ttu-id="a0d91-131">SQL Server バック プレーンでは、SQL テーブルにメッセージを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="a0d91-131">The SQL Server backplane writes messages to SQL tables.</span></span> <span data-ttu-id="a0d91-132">バック プレーンでは、効率的なメッセージングを Service Broker を使用します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-132">The backplane uses Service Broker for efficient messaging.</span></span> <span data-ttu-id="a0d91-133">ただし、これは Service Broker が有効になっていない場合にも機能します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-133">However, it also works if Service Broker is not enabled.</span></span>

<span data-ttu-id="a0d91-134">Azure 上のアプリケーションをデプロイする場合は、Redis バック プレーンを使用して、使用することを検討してください[Azure Redis Cache](https://azure.microsoft.com/services/cache/)します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-134">If you deploy your application on Azure, consider using the Redis backplane using [Azure Redis Cache](https://azure.microsoft.com/services/cache/).</span></span> <span data-ttu-id="a0d91-135">サーバー ファームに展開する場合は、SQL Server または Redis のバック プレーンを検討してください。</span><span class="sxs-lookup"><span data-stu-id="a0d91-135">If you are deploying to your own server farm, consider the SQL Server or Redis backplanes.</span></span>

<span data-ttu-id="a0d91-136">次のトピックには、各バック プレーンのステップバイ ステップ チュートリアルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="a0d91-136">The following topics contain step-by-step tutorials for each backplane:</span></span>

- [<span data-ttu-id="a0d91-137">Azure Service Bus による SignalR スケールアウト</span><span class="sxs-lookup"><span data-stu-id="a0d91-137">SignalR Scaleout with Azure Service Bus</span></span>](scaleout-with-windows-azure-service-bus.md)
- [<span data-ttu-id="a0d91-138">Redis による SignalR スケールアウト</span><span class="sxs-lookup"><span data-stu-id="a0d91-138">SignalR Scaleout with Redis</span></span>](scaleout-with-redis.md)
- [<span data-ttu-id="a0d91-139">SQL Server による SignalR スケールアウト</span><span class="sxs-lookup"><span data-stu-id="a0d91-139">SignalR Scaleout with SQL Server</span></span>](scaleout-with-sql-server.md)

## <a name="implementation"></a><span data-ttu-id="a0d91-140">実装</span><span class="sxs-lookup"><span data-stu-id="a0d91-140">Implementation</span></span>

<span data-ttu-id="a0d91-141">Signalr では、すべてのメッセージはメッセージ バスを通じて送信されます。</span><span class="sxs-lookup"><span data-stu-id="a0d91-141">In SignalR, every message is sent through a message bus.</span></span> <span data-ttu-id="a0d91-142">メッセージ バス実装、 [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)インターフェイスで、パブリッシュ/サブスクライブ抽象化を提供します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-142">A message bus implements the [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, which provides a publish/subscribe abstraction.</span></span> <span data-ttu-id="a0d91-143">既定値を置き換えることで、バック プレーンの動作**IMessageBus**バス バック プレーンに対するに設計されています。</span><span class="sxs-lookup"><span data-stu-id="a0d91-143">The backplanes work by replacing the default **IMessageBus** with a bus designed for that backplane.</span></span> <span data-ttu-id="a0d91-144">Redis メッセージ バスは、たとえば、 [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)、Redis を使用して[パブリッシュ/サブスクライブ](http://redis.io/topics/pubsub)メッセージを送受信するためのメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="a0d91-144">For example, the message bus for Redis is [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), and it uses the Redis [pub/sub](http://redis.io/topics/pubsub) mechanism to send and receive messages.</span></span>

<span data-ttu-id="a0d91-145">各サーバー インスタンスは、バスを介してバック プレーンに接続します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-145">Each server instance connects to the backplane through the bus.</span></span> <span data-ttu-id="a0d91-146">メッセージが送信されると、バック プレーンに移動して、バック プレーンのすべてのサーバーに送信します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-146">When a message is sent, it goes to the backplane, and the backplane sends it to every server.</span></span> <span data-ttu-id="a0d91-147">サーバーは、バック プレーンからメッセージを取得、そのローカル キャッシュ内でメッセージが保存されます。</span><span class="sxs-lookup"><span data-stu-id="a0d91-147">When a server gets a message from the backplane, it puts the message in its local cache.</span></span> <span data-ttu-id="a0d91-148">サーバーはし、そのローカル キャッシュから、クライアントにメッセージを配信します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-148">The server then delivers messages to clients from its local cache.</span></span>

<span data-ttu-id="a0d91-149">各クライアント接続のため、カーソルを使用して、メッセージ ストリームを読み取り中に、クライアントの進行状況が追跡されます。</span><span class="sxs-lookup"><span data-stu-id="a0d91-149">For each client connection, the client's progress in reading the message stream is tracked using a cursor.</span></span> <span data-ttu-id="a0d91-150">(カーソルは、メッセージ ストリーム内の位置を表します)。クライアントが切断して再接続して、クライアントのカーソル値の後に到着したすべてのメッセージ バスを要求します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-150">(A cursor represents a position in the message stream.) If a client disconnects and then reconnects, it asks the bus for any messages that arrived after the client's cursor value.</span></span> <span data-ttu-id="a0d91-151">接続を使用する場合に、同じことが起こります[ポーリング時間の長い](../getting-started/introduction-to-signalr.md#transports)します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-151">The same thing happens when a connection uses [long polling](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="a0d91-152">長いポーリング要求が完了した後、クライアントは新しい接続を開き、カーソルより後に到着したメッセージを要求します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-152">After a long poll request completes, the client opens a new connection and asks for messages that arrived after the cursor.</span></span>

<span data-ttu-id="a0d91-153">カーソルのメカニズムの動作でクライアントが別のサーバーにルーティングされる場合でも再接続します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-153">The cursor mechanism works even if a client is routed to a different server on reconnect.</span></span> <span data-ttu-id="a0d91-154">バック プレーンは、すべてのサーバーを認識しており、クライアントが接続する先のサーバーかは関係ありません。</span><span class="sxs-lookup"><span data-stu-id="a0d91-154">The backplane is aware of all the servers, and it doesn't matter which server a client connects to.</span></span>

## <a name="limitations"></a><span data-ttu-id="a0d91-155">制限事項</span><span class="sxs-lookup"><span data-stu-id="a0d91-155">Limitations</span></span>

<span data-ttu-id="a0d91-156">バック プレーンを使用して、メッセージの最大スループットは、クライアントは、1 台のサーバー ノードに直接話すときよりも低い。</span><span class="sxs-lookup"><span data-stu-id="a0d91-156">Using a backplane, the maximum message throughput is lower than it is when clients talk directly to a single server node.</span></span> <span data-ttu-id="a0d91-157">バック プレーンがバック プレーンがボトルネックになることができますので、すべてのノードにすべてのメッセージを転送するためです。</span><span class="sxs-lookup"><span data-stu-id="a0d91-157">That's because the backplane forwards every message to every node, so the backplane can become a bottleneck.</span></span> <span data-ttu-id="a0d91-158">この制限は、問題であるかどうかは、アプリケーションによって異なります。</span><span class="sxs-lookup"><span data-stu-id="a0d91-158">Whether this limitation is a problem depends on the application.</span></span> <span data-ttu-id="a0d91-159">たとえば、SignalR の一般的なシナリオを示します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-159">For example, here are some typical SignalR scenarios:</span></span>

- <span data-ttu-id="a0d91-160">[サーバー ブロードキャスト](../getting-started/tutorial-server-broadcast-with-signalr.md)(株価情報など)。バック プレーンがこのシナリオに動作するは、サーバー メッセージが送信される速度を制御するためです。</span><span class="sxs-lookup"><span data-stu-id="a0d91-160">[Server broadcast](../getting-started/tutorial-server-broadcast-with-signalr.md) (e.g., stock ticker): Backplanes work well for this scenario, because the server controls the rate at which messages are sent.</span></span>
- <span data-ttu-id="a0d91-161">[クライアントで](../getting-started/tutorial-getting-started-with-signalr.md)(チャットなど)。このシナリオでバック プレーン場合があります、ボトルネックのメッセージの数に合わせてクライアントの数メッセージの数が増加した場合は、それに比例して増えるクライアントが参加します。</span><span class="sxs-lookup"><span data-stu-id="a0d91-161">[Client-to-client](../getting-started/tutorial-getting-started-with-signalr.md) (e.g., chat): In this scenario, the backplane might be a bottleneck if the number of messages scales with the number of clients; that is, if the rate of messages grows proportionally as more clients join.</span></span>
- <span data-ttu-id="a0d91-162">[高頻度リアルタイム メッセージング](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)(リアルタイムのゲームなど)。このシナリオでは、バック プレーンは推奨されません。</span><span class="sxs-lookup"><span data-stu-id="a0d91-162">[High-frequency realtime](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (e.g., real-time games): A backplane is not recommended for this scenario.</span></span>

## <a name="enabling-tracing-for-signalr-scaleout"></a><span data-ttu-id="a0d91-163">SignalR スケール アウトのトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="a0d91-163">Enabling Tracing For SignalR Scaleout</span></span>

<span data-ttu-id="a0d91-164">バック プレーンのトレースを有効にするには、web.config ファイルのルートの下に次のセクションを追加**構成**要素。</span><span class="sxs-lookup"><span data-stu-id="a0d91-164">To enable tracing for the backplanes, add the following sections to the web.config file, under the root **configuration** element:</span></span>

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
