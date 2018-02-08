---
title: "ASP.NET Core での Websocket のサポート"
author: tdykstra
description: "ASP.NET Core で Websocket の使用を開始する方法を説明します。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="a64c1-103">ASP.NET Core での WebSockets の概要</span><span class="sxs-lookup"><span data-stu-id="a64c1-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="a64c1-104">作成者: [Tom Dykstra](https://github.com/tdykstra) および [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="a64c1-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="a64c1-105">この記事では、ASP.NET Core で Websocket の使用を開始する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="a64c1-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) は TCP 接続を使用した双方向の永続的通信チャネルを有効にするプロトコルです。</span><span class="sxs-lookup"><span data-stu-id="a64c1-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="a64c1-107">これは、チャット、株価情報、ゲームなどのアプリケーション、また Web アプリケーションでリアルタイム機能が必要とされる場所で使用されます。</span><span class="sxs-lookup"><span data-stu-id="a64c1-107">It's used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="a64c1-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="a64c1-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="a64c1-109">詳細については、「[次の手順](#next-steps)」のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a64c1-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="a64c1-110">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="a64c1-110">Prerequisites</span></span>

* <span data-ttu-id="a64c1-111">ASP.NET Core 1.1 (1.0 では動作しません)</span><span class="sxs-lookup"><span data-stu-id="a64c1-111">ASP.NET Core 1.1 (doesn't run on 1.0)</span></span>
* <span data-ttu-id="a64c1-112">ASP.NET Core を実行できる次の任意の OS:</span><span class="sxs-lookup"><span data-stu-id="a64c1-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="a64c1-113">Windows 7 / Windows Server 2008 以降</span><span class="sxs-lookup"><span data-stu-id="a64c1-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="a64c1-114">Linux</span><span class="sxs-lookup"><span data-stu-id="a64c1-114">Linux</span></span>
  * <span data-ttu-id="a64c1-115">macOS</span><span class="sxs-lookup"><span data-stu-id="a64c1-115">macOS</span></span>

* <span data-ttu-id="a64c1-116">**例外**: アプリが IIS または WebListener を含む Windows 上で実行される場合、次を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a64c1-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="a64c1-117">Windows 8 / Windows Server 2012 以降</span><span class="sxs-lookup"><span data-stu-id="a64c1-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="a64c1-118">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="a64c1-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="a64c1-119">IIS で WebSocket を有効にする必要があります</span><span class="sxs-lookup"><span data-stu-id="a64c1-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="a64c1-120">サポートされているブラウザーについては、http://caniuse.com/#feat=websockets を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a64c1-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="a64c1-121">使用する状況</span><span class="sxs-lookup"><span data-stu-id="a64c1-121">When to use it</span></span>

<span data-ttu-id="a64c1-122">Websocket は、ソケット接続を直接使用する必要がある場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="a64c1-123">たとえば、リアルタイムのゲームで最高のパフォーマンスが必要な場合などです。</span><span class="sxs-lookup"><span data-stu-id="a64c1-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="a64c1-124">リアルタイムの機能には、[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) の方がアプリケーション モデルとしてはより充実していますが、これは ASP.NET でのみ実行できるものであり、ASP.NET Core では実行できません。</span><span class="sxs-lookup"><span data-stu-id="a64c1-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="a64c1-125">SignalR の Core バージョンは現在開発中です。進捗を確認するには、「[GitHub repository for SignalR Core](https://github.com/aspnet/SignalR)」 (SignalR Core の GitHub リポジトリ) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a64c1-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="a64c1-126">SignalR Core を待つことができない場合は、そのまま Websocket を使用できます。</span><span class="sxs-lookup"><span data-stu-id="a64c1-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="a64c1-127">ただし、次などの SignalR が提供する機能を開発する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="a64c1-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="a64c1-128">他の転送方法への自動フォールバックが使用された、より多くのブラウザ バージョンのサポート。</span><span class="sxs-lookup"><span data-stu-id="a64c1-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="a64c1-129">切断された場合の自動接続。</span><span class="sxs-lookup"><span data-stu-id="a64c1-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="a64c1-130">サーバー上のクライアント、またはこの逆の呼び出しメソッドのサポート。</span><span class="sxs-lookup"><span data-stu-id="a64c1-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="a64c1-131">複数のサーバーへのスケーリングのサポート。</span><span class="sxs-lookup"><span data-stu-id="a64c1-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="a64c1-132">使用方法</span><span class="sxs-lookup"><span data-stu-id="a64c1-132">How to use it</span></span>

* <span data-ttu-id="a64c1-133">[Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="a64c1-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="a64c1-134">ミドルウェアを構成します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-134">Configure the middleware.</span></span>
* <span data-ttu-id="a64c1-135">WebSocket の要求を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="a64c1-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="a64c1-136">メッセージを送受信します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="a64c1-137">ミドルウェアの構成</span><span class="sxs-lookup"><span data-stu-id="a64c1-137">Configure the middleware</span></span>

<span data-ttu-id="a64c1-138">`Startup` クラスの `Configure` メソッドに、Websocket ミドルウェアを追加します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="a64c1-139">次の設定を構成できます。</span><span class="sxs-lookup"><span data-stu-id="a64c1-139">The following settings can be configured:</span></span>

* <span data-ttu-id="a64c1-140">`KeepAliveInterval`: プロキシの接続の維持を保証する、クライアントに "ping" フレームを送信する頻度。</span><span class="sxs-lookup"><span data-stu-id="a64c1-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="a64c1-141">`ReceiveBufferSize`: データの受信に使用されるバッファーのサイズ。</span><span class="sxs-lookup"><span data-stu-id="a64c1-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="a64c1-142">これは、上級ユーザーのみが、データのサイズに応じたパフォーマンス調整のために変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a64c1-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="a64c1-143">WebSocket の要求の受け入れ</span><span class="sxs-lookup"><span data-stu-id="a64c1-143">Accept WebSocket requests</span></span>

<span data-ttu-id="a64c1-144">以降の要求ライフサイクルのどこかで (たとえば、以降の `Configure` メソッドまたは MVC アクションで)、それが WebSocket 要求であるかを確認し、WebSocket 要求を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="a64c1-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="a64c1-145">この例は、以降の `Configure` メソッドから抜粋したものです。</span><span class="sxs-lookup"><span data-stu-id="a64c1-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="a64c1-146">WebSocket 要求はどの URL からも受け取る場合がありますが、このサンプル コードでは `/ws` の要求のみを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="a64c1-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="a64c1-147">メッセージの送受信</span><span class="sxs-lookup"><span data-stu-id="a64c1-147">Send and receive messages</span></span>

<span data-ttu-id="a64c1-148">`AcceptWebSocketAsync` メソッドは、TCP 接続を WebSocket 接続にアップグレードし、[WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) オブジェクトを渡します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="a64c1-149">メッセージの送受信に、WebSocket オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="a64c1-150">前に示した、WebSocket 要求を受け入れるコードが、`WebSocket` オブジェクトを `Echo` メソッドに渡します (ここでは、`Echo` メソッドです)。</span><span class="sxs-lookup"><span data-stu-id="a64c1-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="a64c1-151">このコードは、メッセージを受信し、同じメッセージをすぐに送信します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="a64c1-152">クライアントが接続を閉じるまで、これを継続しループ内に存在し続けます。</span><span class="sxs-lookup"><span data-stu-id="a64c1-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="a64c1-153">このループを開始する前に、WebSocket を受け入れた場合、ミドルウェア パイプラインは終了します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="a64c1-154">ソケットを閉じると、パイプラインはアンワインドされます。</span><span class="sxs-lookup"><span data-stu-id="a64c1-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="a64c1-155">つまり、WebSocket 受け入れると、たとえば、MVC アクションにヒットしたときのように、要求がパイプラインで前進しなくなります。</span><span class="sxs-lookup"><span data-stu-id="a64c1-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="a64c1-156">ただし、このループを終了し、ソケットを閉じると、要求はパイプラインを遡って進みます。</span><span class="sxs-lookup"><span data-stu-id="a64c1-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a64c1-157">次の手順</span><span class="sxs-lookup"><span data-stu-id="a64c1-157">Next steps</span></span>

<span data-ttu-id="a64c1-158">この記事に付属している[サンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)は、単純なエコー アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="a64c1-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="a64c1-159">これには、WebSocket 接続を作成する Web ページがあり、サーバーが受け取るすべてのメッセージを単純にクライアントに再送信します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="a64c1-160">これをコマンド プロンプトから実行し (IIS Express を使用した Visual Studio からは実行するように設定されていません)、http://localhost:5000 に移動します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="a64c1-161">Web ページの左上に、接続の状態が示されます。</span><span class="sxs-lookup"><span data-stu-id="a64c1-161">The web page shows connection status at the upper left:</span></span>

![Web ページの初期状態](websockets/_static/start.png)

<span data-ttu-id="a64c1-163">**[接続]** を選択し、表示されている URL に WebSocket 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="a64c1-164">テスト メッセージを入力し、**[送信]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="a64c1-165">完了したら、**[Close Socket]\(ソケットを閉じる\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a64c1-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="a64c1-166">**[Communication Log]\(コミュニケーション ログ\)** セクションに、発生した各オープン、送信、クローズのアクションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a64c1-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Web ページの初期状態](websockets/_static/end.png)
