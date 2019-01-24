---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: SignalR のスケール アウト入門 1.x |Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 04/29/2013
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: 78e53c38ec760334cecee0431d52d993a657b908
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837287"
---
<a name="introduction-to-scaleout-in-signalr-1x"></a><span data-ttu-id="e2a38-102">SignalR のスケール アウト入門 1.x</span><span class="sxs-lookup"><span data-stu-id="e2a38-102">Introduction to Scaleout in SignalR 1.x</span></span>
====================
<span data-ttu-id="e2a38-103">によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="e2a38-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="e2a38-104">一般は、web アプリケーションのスケール設定の 2 つの方法があります:*スケール アップ*と*スケール アウト*します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-104">In general, there are two ways to scale a web application: *scale up* and *scale out*.</span></span>

- <span data-ttu-id="e2a38-105">スケール アップは、RAM、Cpu などの大規模なサーバー (またはより大きな VM) を使用することを意味します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-105">Scale up means using a larger server (or a larger VM) with more RAM, CPUs, etc.</span></span>
- <span data-ttu-id="e2a38-106">スケール アウト、負荷を処理するより多くのサーバーを追加することを意味します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-106">Scale out means adding more servers to handle the load.</span></span>

<span data-ttu-id="e2a38-107">スケール アップの問題は、迅速に、マシンのサイズの上限に達したことです。</span><span class="sxs-lookup"><span data-stu-id="e2a38-107">The problem with scaling up is that you quickly hit a limit on the size of the machine.</span></span> <span data-ttu-id="e2a38-108">さらに、スケール アウトする必要があります。ただし、スケール アウトするときにクライアントを別のサーバーにルーティングを取得できます。</span><span class="sxs-lookup"><span data-stu-id="e2a38-108">Beyond that, you need to scale out. However, when you scale out, clients can get routed to different servers.</span></span> <span data-ttu-id="e2a38-109">1 つのサーバーに接続されているクライアントは、別のサーバーから送信されたメッセージを受信しません。</span><span class="sxs-lookup"><span data-stu-id="e2a38-109">A client that is connected to one server will not receive messages sent from another server.</span></span>

![](scaleout-in-signalr/_static/image1.png)

<span data-ttu-id="e2a38-110">1 つのソリューションと呼ばれるコンポーネントを使用して、サーバー間でメッセージを転送するように、*バック プレーン*します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-110">One solution is to forward messages between servers, using a component called a *backplane*.</span></span> <span data-ttu-id="e2a38-111">有効になっているバック プレーン、各アプリケーション インスタンスが、バック プレーンにメッセージを送信し、バック プレーンの他のアプリケーション インスタンスに転送します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-111">With a backplane enabled, each application instance sends messages to the backplane, and the backplane forwards them to the other application instances.</span></span> <span data-ttu-id="e2a38-112">(電子機器、バック プレーンです並列コネクタのグループ。</span><span class="sxs-lookup"><span data-stu-id="e2a38-112">(In electronics, a backplane is a group of parallel connectors.</span></span> <span data-ttu-id="e2a38-113">たとえて、SignalR のバック プレーン接続する複数のサーバーです。)</span><span class="sxs-lookup"><span data-stu-id="e2a38-113">By analogy, a SignalR backplane connects multiple servers.)</span></span>

![](scaleout-in-signalr/_static/image2.png)

<span data-ttu-id="e2a38-114">現在、SignalR は、次の 3 つのバック プレーンを提供します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-114">SignalR currently provides three backplanes:</span></span>

- <span data-ttu-id="e2a38-115">**Azure Service Bus**します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-115">**Azure Service Bus**.</span></span> <span data-ttu-id="e2a38-116">Service Bus は、メッセージング インフラストラクチャを使用する疎結合方式でメッセージを送信するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="e2a38-116">Service Bus is a messaging infrastructure that allows components to send messages in a loosely coupled way.</span></span>
- <span data-ttu-id="e2a38-117">**Redis**します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-117">**Redis**.</span></span> <span data-ttu-id="e2a38-118">Redis はメモリ内のキー値ストアです。</span><span class="sxs-lookup"><span data-stu-id="e2a38-118">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="e2a38-119">Redis では、メッセージを送信するため、(「パブリッシュ/サブスクライブ」) の発行/サブスクライブ パターンをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="e2a38-119">Redis supports a publish/subscribe ("pub/sub") pattern for sending messages.</span></span>
- <span data-ttu-id="e2a38-120">**SQL Server**します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-120">**SQL Server**.</span></span> <span data-ttu-id="e2a38-121">SQL Server バック プレーンでは、SQL テーブルにメッセージを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="e2a38-121">The SQL Server backplane writes messages to SQL tables.</span></span> <span data-ttu-id="e2a38-122">バック プレーンでは、効率的なメッセージングを Service Broker を使用します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-122">The backplane uses Service Broker for efficient messaging.</span></span> <span data-ttu-id="e2a38-123">ただし、これは Service Broker が有効になっていない場合にも機能します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-123">However, it also works if Service Broker is not enabled.</span></span>

<span data-ttu-id="e2a38-124">Azure 上のアプリケーションをデプロイする場合は、Azure Service Bus のバック プレーンの使用を検討します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-124">If you deploy your application on Azure, consider using the Azure Service Bus backplane.</span></span> <span data-ttu-id="e2a38-125">サーバー ファームに展開する場合は、SQL Server または Redis のバック プレーンを検討してください。</span><span class="sxs-lookup"><span data-stu-id="e2a38-125">If you are deploying to your own server farm, consider the SQL Server or Redis backplanes.</span></span>

<span data-ttu-id="e2a38-126">次のトピックには、各バック プレーンのステップバイ ステップ チュートリアルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e2a38-126">The following topics contain step-by-step tutorials for each backplane:</span></span>

- [<span data-ttu-id="e2a38-127">Azure Service Bus による SignalR スケールアウト</span><span class="sxs-lookup"><span data-stu-id="e2a38-127">SignalR Scaleout with Azure Service Bus</span></span>](scaleout-with-windows-azure-service-bus.md)
- [<span data-ttu-id="e2a38-128">Redis による SignalR スケールアウト</span><span class="sxs-lookup"><span data-stu-id="e2a38-128">SignalR Scaleout with Redis</span></span>](scaleout-with-redis.md)
- [<span data-ttu-id="e2a38-129">SQL Server による SignalR スケールアウト</span><span class="sxs-lookup"><span data-stu-id="e2a38-129">SignalR Scaleout with SQL Server</span></span>](scaleout-with-sql-server.md)

## <a name="implementation"></a><span data-ttu-id="e2a38-130">実装</span><span class="sxs-lookup"><span data-stu-id="e2a38-130">Implementation</span></span>

<span data-ttu-id="e2a38-131">Signalr では、すべてのメッセージはメッセージ バスを通じて送信されます。</span><span class="sxs-lookup"><span data-stu-id="e2a38-131">In SignalR, every message is sent through a message bus.</span></span> <span data-ttu-id="e2a38-132">メッセージ バス実装、 [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)インターフェイスで、パブリッシュ/サブスクライブ抽象化を提供します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-132">A message bus implements the [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, which provides a publish/subscribe abstraction.</span></span> <span data-ttu-id="e2a38-133">既定値を置き換えることで、バック プレーンの動作**IMessageBus**バス バック プレーンに対するに設計されています。</span><span class="sxs-lookup"><span data-stu-id="e2a38-133">The backplanes work by replacing the default **IMessageBus** with a bus designed for that backplane.</span></span> <span data-ttu-id="e2a38-134">Redis メッセージ バスは、たとえば、 [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)、Redis を使用して[パブリッシュ/サブスクライブ](http://redis.io/topics/pubsub)メッセージを送受信するためのメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="e2a38-134">For example, the message bus for Redis is [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), and it uses the Redis [pub/sub](http://redis.io/topics/pubsub) mechanism to send and receive messages.</span></span>

<span data-ttu-id="e2a38-135">各サーバー インスタンスは、バスを介してバック プレーンに接続します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-135">Each server instance connects to the backplane through the bus.</span></span> <span data-ttu-id="e2a38-136">メッセージが送信されると、バック プレーンに移動して、バック プレーンのすべてのサーバーに送信します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-136">When a message is sent, it goes to the backplane, and the backplane sends it to every server.</span></span> <span data-ttu-id="e2a38-137">サーバーは、バック プレーンからメッセージを取得、そのローカル キャッシュ内でメッセージが保存されます。</span><span class="sxs-lookup"><span data-stu-id="e2a38-137">When a server gets a message from the backplane, it puts the message in its local cache.</span></span> <span data-ttu-id="e2a38-138">サーバーはし、そのローカル キャッシュから、クライアントにメッセージを配信します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-138">The server then delivers messages to clients from its local cache.</span></span>

<span data-ttu-id="e2a38-139">各クライアント接続のため、カーソルを使用して、メッセージ ストリームを読み取り中に、クライアントの進行状況が追跡されます。</span><span class="sxs-lookup"><span data-stu-id="e2a38-139">For each client connection, the client's progress in reading the message stream is tracked using a cursor.</span></span> <span data-ttu-id="e2a38-140">(カーソルは、メッセージ ストリーム内の位置を表します)。クライアントが切断して再接続して、クライアントのカーソル値の後に到着したすべてのメッセージ バスを要求します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-140">(A cursor represents a position in the message stream.) If a client disconnects and then reconnects, it asks the bus for any messages that arrived after the client's cursor value.</span></span> <span data-ttu-id="e2a38-141">接続を使用する場合に、同じことが起こります[ポーリング時間の長い](../getting-started/introduction-to-signalr.md#transports)します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-141">The same thing happens when a connection uses [long polling](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="e2a38-142">長いポーリング要求が完了した後、クライアントは新しい接続を開き、カーソルより後に到着したメッセージを要求します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-142">After a long poll request completes, the client opens a new connection and asks for messages that arrived after the cursor.</span></span>

<span data-ttu-id="e2a38-143">カーソルのメカニズムの動作でクライアントが別のサーバーにルーティングされる場合でも再接続します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-143">The cursor mechanism works even if a client is routed to a different server on reconnect.</span></span> <span data-ttu-id="e2a38-144">バック プレーンは、すべてのサーバーを認識しており、クライアントが接続する先のサーバーかは関係ありません。</span><span class="sxs-lookup"><span data-stu-id="e2a38-144">The backplane is aware of all the servers, and it doesn't matter which server a client connects to.</span></span>

## <a name="limitations"></a><span data-ttu-id="e2a38-145">制限事項</span><span class="sxs-lookup"><span data-stu-id="e2a38-145">Limitations</span></span>

<span data-ttu-id="e2a38-146">バック プレーンを使用して、メッセージの最大スループットは、クライアントは、1 台のサーバー ノードに直接話すときよりも低い。</span><span class="sxs-lookup"><span data-stu-id="e2a38-146">Using a backplane, the maximum message throughput is lower than it is when clients talk directly to a single server node.</span></span> <span data-ttu-id="e2a38-147">バック プレーンがバック プレーンがボトルネックになることができますので、すべてのノードにすべてのメッセージを転送するためです。</span><span class="sxs-lookup"><span data-stu-id="e2a38-147">That's because the backplane forwards every message to every node, so the backplane can become a bottleneck.</span></span> <span data-ttu-id="e2a38-148">この制限は、問題であるかどうかは、アプリケーションによって異なります。</span><span class="sxs-lookup"><span data-stu-id="e2a38-148">Whether this limitation is a problem depends on the application.</span></span> <span data-ttu-id="e2a38-149">たとえば、SignalR の一般的なシナリオを示します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-149">For example, here are some typical SignalR scenarios:</span></span>

- <span data-ttu-id="e2a38-150">[サーバー ブロードキャスト](tutorial-server-broadcast-with-aspnet-signalr.md)(株価情報など)。バック プレーンがこのシナリオに動作するは、サーバー メッセージが送信される速度を制御するためです。</span><span class="sxs-lookup"><span data-stu-id="e2a38-150">[Server broadcast](tutorial-server-broadcast-with-aspnet-signalr.md) (e.g., stock ticker): Backplanes work well for this scenario, because the server controls the rate at which messages are sent.</span></span>
- <span data-ttu-id="e2a38-151">[クライアントで](tutorial-getting-started-with-signalr.md)(チャットなど)。このシナリオでバック プレーン場合があります、ボトルネックのメッセージの数に合わせてクライアントの数メッセージの数が増加した場合は、それに比例して増えるクライアントが参加します。</span><span class="sxs-lookup"><span data-stu-id="e2a38-151">[Client-to-client](tutorial-getting-started-with-signalr.md) (e.g., chat): In this scenario, the backplane might be a bottleneck if the number of messages scales with the number of clients; that is, if the rate of messages grows proportionally as more clients join.</span></span>
- <span data-ttu-id="e2a38-152">[高頻度リアルタイム メッセージング](tutorial-high-frequency-realtime-with-signalr.md)(リアルタイムのゲームなど)。このシナリオでは、バック プレーンは推奨されません。</span><span class="sxs-lookup"><span data-stu-id="e2a38-152">[High-frequency realtime](tutorial-high-frequency-realtime-with-signalr.md) (e.g., real-time games): A backplane is not recommended for this scenario.</span></span>

## <a name="enabling-tracing-for-signalr-scaleout"></a><span data-ttu-id="e2a38-153">SignalR スケール アウトのトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="e2a38-153">Enabling Tracing For SignalR Scaleout</span></span>

<span data-ttu-id="e2a38-154">バック プレーンのトレースを有効にするには、web.config ファイルのルートの下に次のセクションを追加**構成**要素。</span><span class="sxs-lookup"><span data-stu-id="e2a38-154">To enable tracing for the backplanes, add the following sections to the web.config file, under the root **configuration** element:</span></span>

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
