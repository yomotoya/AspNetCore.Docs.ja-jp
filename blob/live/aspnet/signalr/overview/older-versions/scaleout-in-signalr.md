---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: "SignalR でスケール アウトの概要 1.x |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/29/2013
ms.topic: article
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: e6230d4d65adb8c9a064545ad761898ca53562bf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-scaleout-in-signalr-1x"></a><span data-ttu-id="56b53-102">SignalR でスケール アウトの概要 1.x</span><span class="sxs-lookup"><span data-stu-id="56b53-102">Introduction to Scaleout in SignalR 1.x</span></span>
====================
<span data-ttu-id="56b53-103">によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="56b53-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="56b53-104">一般は、web アプリケーションのスケールの 2 つの方法があります:*スケール アップ*と*スケール アウト*です。</span><span class="sxs-lookup"><span data-stu-id="56b53-104">In general, there are two ways to scale a web application: *scale up* and *scale out*.</span></span>

- <span data-ttu-id="56b53-105">スケール アップ RAM や Cpu などのより大きなサーバー (または大規模な VM) を使用することを意味します。</span><span class="sxs-lookup"><span data-stu-id="56b53-105">Scale up means using a larger server (or a larger VM) with more RAM, CPUs, etc.</span></span>
- <span data-ttu-id="56b53-106">スケール アウトへの負荷がより多くのサーバーを追加します。</span><span class="sxs-lookup"><span data-stu-id="56b53-106">Scale out means adding more servers to handle the load.</span></span>

<span data-ttu-id="56b53-107">スケール アップに関する問題は、マシンのサイズの制限を迅速にヒットすることです。</span><span class="sxs-lookup"><span data-stu-id="56b53-107">The problem with scaling up is that you quickly hit a limit on the size of the machine.</span></span> <span data-ttu-id="56b53-108">さらに、スケール アウトする必要があります。ただし、スケール アウトするときにクライアントを別のサーバーに送られますことができます。</span><span class="sxs-lookup"><span data-stu-id="56b53-108">Beyond that, you need to scale out. However, when you scale out, clients can get routed to different servers.</span></span> <span data-ttu-id="56b53-109">1 つのサーバーに接続されているクライアントは、別のサーバーから送信されたメッセージを受信しません。</span><span class="sxs-lookup"><span data-stu-id="56b53-109">A client that is connected to one server will not receive messages sent from another server.</span></span>

![](scaleout-in-signalr/_static/image1.png)

<span data-ttu-id="56b53-110">1 つのソリューションと呼ばれるコンポーネントを使用して、サーバー間でメッセージを転送する、*バック プレーン*です。</span><span class="sxs-lookup"><span data-stu-id="56b53-110">One solution is to forward messages between servers, using a component called a *backplane*.</span></span> <span data-ttu-id="56b53-111">有効になっているバック プレーンと各アプリケーション インスタンスが、バック プレーンにメッセージを送信バック プレーンに転送し、その他のアプリケーション インスタンス。</span><span class="sxs-lookup"><span data-stu-id="56b53-111">With a backplane enabled, each application instance sends messages to the backplane, and the backplane forwards them to the other application instances.</span></span> <span data-ttu-id="56b53-112">(エレクトロニクス バック プレーンは並列コネクタのグループです。</span><span class="sxs-lookup"><span data-stu-id="56b53-112">(In electronics, a backplane is a group of parallel connectors.</span></span> <span data-ttu-id="56b53-113">たとえて、SignalR のバック プレーン接続する複数のサーバーです。)</span><span class="sxs-lookup"><span data-stu-id="56b53-113">By analogy, a SignalR backplane connects multiple servers.)</span></span>

![](scaleout-in-signalr/_static/image2.png)

<span data-ttu-id="56b53-114">SignalR には、次の 3 つのバック プレーン現在提供しています。</span><span class="sxs-lookup"><span data-stu-id="56b53-114">SignalR currently provides three backplanes:</span></span>

- <span data-ttu-id="56b53-115">**Azure Service Bus**です。</span><span class="sxs-lookup"><span data-stu-id="56b53-115">**Azure Service Bus**.</span></span> <span data-ttu-id="56b53-116">Service Bus は、メッセージング インフラストラクチャにより、疎結合の方法でメッセージを送信するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="56b53-116">Service Bus is a messaging infrastructure that allows components to send messages in a loosely coupled way.</span></span>
- <span data-ttu-id="56b53-117">**Redis**です。</span><span class="sxs-lookup"><span data-stu-id="56b53-117">**Redis**.</span></span> <span data-ttu-id="56b53-118">Redis は、メモリ内キー値ストアです。</span><span class="sxs-lookup"><span data-stu-id="56b53-118">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="56b53-119">Redis は、メッセージを送信する (「パブリッシュ/サブスクライブ」) の公開/定期受信パターンをサポートします。</span><span class="sxs-lookup"><span data-stu-id="56b53-119">Redis supports a publish/subscribe ("pub/sub") pattern for sending messages.</span></span>
- <span data-ttu-id="56b53-120">**SQL Server**です。</span><span class="sxs-lookup"><span data-stu-id="56b53-120">**SQL Server**.</span></span> <span data-ttu-id="56b53-121">SQL Server のバック プレーンでは、SQL テーブルにメッセージを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="56b53-121">The SQL Server backplane writes messages to SQL tables.</span></span> <span data-ttu-id="56b53-122">バック プレーンでは、効率的なメッセージの Service Broker を使用します。</span><span class="sxs-lookup"><span data-stu-id="56b53-122">The backplane uses Service Broker for efficient messaging.</span></span> <span data-ttu-id="56b53-123">ただし、これは Service Broker が有効になっていない場合にでも動作します。</span><span class="sxs-lookup"><span data-stu-id="56b53-123">However, it also works if Service Broker is not enabled.</span></span>

<span data-ttu-id="56b53-124">Azure でアプリケーションを配置する場合は、Azure Service Bus バック プレーンの使用を検討します。</span><span class="sxs-lookup"><span data-stu-id="56b53-124">If you deploy your application on Azure, consider using the Azure Service Bus backplane.</span></span> <span data-ttu-id="56b53-125">サーバー ファームに配置する場合は、SQL Server または Redis のバック プレーンに検討してください。</span><span class="sxs-lookup"><span data-stu-id="56b53-125">If you are deploying to your own server farm, consider the SQL Server or Redis backplanes.</span></span>

<span data-ttu-id="56b53-126">次のトピックには、各バック プレーンのステップバイ ステップ チュートリアルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="56b53-126">The following topics contain step-by-step tutorials for each backplane:</span></span>

- [<span data-ttu-id="56b53-127">Azure Service Bus での SignalR スケール アウト</span><span class="sxs-lookup"><span data-stu-id="56b53-127">SignalR Scaleout with Azure Service Bus</span></span>](scaleout-with-windows-azure-service-bus.md)
- [<span data-ttu-id="56b53-128">Redis で SignalR スケール アウト</span><span class="sxs-lookup"><span data-stu-id="56b53-128">SignalR Scaleout with Redis</span></span>](scaleout-with-redis.md)
- [<span data-ttu-id="56b53-129">SQL Server での SignalR スケール アウト</span><span class="sxs-lookup"><span data-stu-id="56b53-129">SignalR Scaleout with SQL Server</span></span>](scaleout-with-sql-server.md)

## <a name="implementation"></a><span data-ttu-id="56b53-130">実装</span><span class="sxs-lookup"><span data-stu-id="56b53-130">Implementation</span></span>

<span data-ttu-id="56b53-131">、SignalR メッセージ バスを介してすべてのメッセージが送信されます。</span><span class="sxs-lookup"><span data-stu-id="56b53-131">In SignalR, every message is sent through a message bus.</span></span> <span data-ttu-id="56b53-132">メッセージ バスを実装して、 [IMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)インターフェイスで、パブリッシュ/サブスクライブ抽象化を提供します。</span><span class="sxs-lookup"><span data-stu-id="56b53-132">A message bus implements the [IMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, which provides a publish/subscribe abstraction.</span></span> <span data-ttu-id="56b53-133">既定値に置き換えることで、バック プレーンの作業**IMessageBus**そのバック プレーン用に設計されたバスとします。</span><span class="sxs-lookup"><span data-stu-id="56b53-133">The backplanes work by replacing the default **IMessageBus** with a bus designed for that backplane.</span></span> <span data-ttu-id="56b53-134">Redis メッセージ バスは、たとえば、 [RedisMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)、し、Redis を使用して[パブリッシュ/サブスクライブ](http://redis.io/topics/pubsub)メッセージを送信するためのメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="56b53-134">For example, the message bus for Redis is [RedisMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), and it uses the Redis [pub/sub](http://redis.io/topics/pubsub) mechanism to send and receive messages.</span></span>

<span data-ttu-id="56b53-135">各サーバー インスタンスは、バスを介してバック プレーンに接続します。</span><span class="sxs-lookup"><span data-stu-id="56b53-135">Each server instance connects to the backplane through the bus.</span></span> <span data-ttu-id="56b53-136">メッセージが送信されると、バック プレーンに移動して、バック プレーンのすべてのサーバーに送信します。</span><span class="sxs-lookup"><span data-stu-id="56b53-136">When a message is sent, it goes to the backplane, and the backplane sends it to every server.</span></span> <span data-ttu-id="56b53-137">サーバーでは、バック プレーンからメッセージが取得されるメッセージをローカル キャッシュに保存されます。</span><span class="sxs-lookup"><span data-stu-id="56b53-137">When a server gets a message from the backplane, it puts the message in its local cache.</span></span> <span data-ttu-id="56b53-138">サーバーはのローカル キャッシュから、クライアントにメッセージを配信します。</span><span class="sxs-lookup"><span data-stu-id="56b53-138">The server then delivers messages to clients from its local cache.</span></span>

<span data-ttu-id="56b53-139">各クライアント接続のため、カーソルを使用して、メッセージ ストリームを読み取る際に、クライアントの進行状況が追跡されます。</span><span class="sxs-lookup"><span data-stu-id="56b53-139">For each client connection, the client's progress in reading the message stream is tracked using a cursor.</span></span> <span data-ttu-id="56b53-140">(カーソルは、メッセージ ストリーム内の位置を表します)。クライアントは、接続が切断され、再接続する場合、クライアントのカーソルの値の後に到着したメッセージのバスを要求します。</span><span class="sxs-lookup"><span data-stu-id="56b53-140">(A cursor represents a position in the message stream.) If a client disconnects and then reconnects, it asks the bus for any messages that arrived after the client's cursor value.</span></span> <span data-ttu-id="56b53-141">接続を使用する場合に、同じことが起こります[ポーリング時間の長い](../getting-started/introduction-to-signalr.md#transports)です。</span><span class="sxs-lookup"><span data-stu-id="56b53-141">The same thing happens when a connection uses [long polling](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="56b53-142">長いポーリング要求が完了した後、クライアントは新しい接続が開かし、カーソルより後に受信したメッセージを要求します。</span><span class="sxs-lookup"><span data-stu-id="56b53-142">After a long poll request completes, the client opens a new connection and asks for messages that arrived after the cursor.</span></span>

<span data-ttu-id="56b53-143">カーソルのメカニズムのしくみに、クライアントが別のサーバーにルーティングされる場合でも再接続します。</span><span class="sxs-lookup"><span data-stu-id="56b53-143">The cursor mechanism works even if a client is routed to a different server on reconnect.</span></span> <span data-ttu-id="56b53-144">バック プレーンは、すべてのサーバーを認識しており、クライアントに接続するサーバーでもかまいません。</span><span class="sxs-lookup"><span data-stu-id="56b53-144">The backplane is aware of all the servers, and it doesn't matter which server a client connects to.</span></span>

## <a name="limitations"></a><span data-ttu-id="56b53-145">制限事項</span><span class="sxs-lookup"><span data-stu-id="56b53-145">Limitations</span></span>

<span data-ttu-id="56b53-146">バック プレーン使用すると、メッセージの最大スループットは、クライアントと 1 台のサーバー ノードに直接通信できる場合はよりも低いです。</span><span class="sxs-lookup"><span data-stu-id="56b53-146">Using a backplane, the maximum message throughput is lower than it is when clients talk directly to a single server node.</span></span> <span data-ttu-id="56b53-147">バック プレーンがバック プレーンがボトルネックになるために、すべてのノードにすべてのメッセージを転送するためです。</span><span class="sxs-lookup"><span data-stu-id="56b53-147">That's because the backplane forwards every message to every node, so the backplane can become a bottleneck.</span></span> <span data-ttu-id="56b53-148">この制限に問題があるかどうかは、アプリケーションによって異なります。</span><span class="sxs-lookup"><span data-stu-id="56b53-148">Whether this limitation is a problem depends on the application.</span></span> <span data-ttu-id="56b53-149">たとえば、SignalR の一般的なシナリオを次に示します。</span><span class="sxs-lookup"><span data-stu-id="56b53-149">For example, here are some typical SignalR scenarios:</span></span>

- <span data-ttu-id="56b53-150">[サーバーのブロードキャスト](tutorial-server-broadcast-with-aspnet-signalr.md)(たとえば、株価表示器): サーバーにメッセージが送信される速度を制御するために、バック プレーンがこのシナリオの適切に動作します。</span><span class="sxs-lookup"><span data-stu-id="56b53-150">[Server broadcast](tutorial-server-broadcast-with-aspnet-signalr.md) (e.g., stock ticker): Backplanes work well for this scenario, because the server controls the rate at which messages are sent.</span></span>
- <span data-ttu-id="56b53-151">[クライアントからクライアントへの](tutorial-getting-started-with-signalr.md)(チャットなど)。 このシナリオでバック プレーン可能性がありますするボトルネックになっている場合は、クライアントの数によるメッセージの数が拡大縮小。 つまり、メッセージの数が増えた場合比例して増えるにつれてクライアントが参加します。</span><span class="sxs-lookup"><span data-stu-id="56b53-151">[Client-to-client](tutorial-getting-started-with-signalr.md) (e.g., chat): In this scenario, the backplane might be a bottleneck if the number of messages scales with the number of clients; that is, if the rate of messages grows proportionally as more clients join.</span></span>
- <span data-ttu-id="56b53-152">[高周波数のリアルタイム](tutorial-high-frequency-realtime-with-signalr.md)(リアルタイムのゲームなど)。 このシナリオのバック プレーンは推奨されません。</span><span class="sxs-lookup"><span data-stu-id="56b53-152">[High-frequency realtime](tutorial-high-frequency-realtime-with-signalr.md) (e.g., real-time games): A backplane is not recommended for this scenario.</span></span>

## <a name="enabling-tracing-for-signalr-scaleout"></a><span data-ttu-id="56b53-153">SignalR スケール アウトのトレースを有効にします。</span><span class="sxs-lookup"><span data-stu-id="56b53-153">Enabling Tracing For SignalR Scaleout</span></span>

<span data-ttu-id="56b53-154">バック プレーンのトレースを有効にするには、web.config ファイルのルートの下に次のセクションを追加**構成**要素。</span><span class="sxs-lookup"><span data-stu-id="56b53-154">To enable tracing for the backplanes, add the following sections to the web.config file, under the root **configuration** element:</span></span>

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
