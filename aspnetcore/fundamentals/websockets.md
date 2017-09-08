---
title: "ASP.NET Core での Websocket のサポート"
author: tdykstra
description: "ASP.NET Core およびその使用方法をサポートして Websocket は何です。"
keywords: "ASP.NET Core、Websocket"
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.assetid: 0e0fedcd-a7b4-4479-8ae0-36eab0229d7e
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: daa36d117749868e4bb9311a941b7751d7b64744
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="fde70-104">Websocket の ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="fde70-104">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="fde70-105">によって[Tom Dykstra](https://github.com/tdykstra)と[Andrew スタントン-看護師](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="fde70-105">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="fde70-106">この記事では、ASP.NET Core で Websocket を開始する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="fde70-106">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="fde70-107">[WebSocket](https://en.wikipedia.org/wiki/WebSocket) TCP 接続を永続的な双方向の通信チャネルをできるようにするプロトコルします。</span><span class="sxs-lookup"><span data-stu-id="fde70-107">[WebSocket](https://en.wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="fde70-108">チャット、株価情報、ゲームなどのアプリケーションの使用は、web アプリケーションでのリアルタイムの機能を使用する任意の場所。</span><span class="sxs-lookup"><span data-stu-id="fde70-108">It is used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="fde70-109">[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)です。</span><span class="sxs-lookup"><span data-stu-id="fde70-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample).</span></span> <span data-ttu-id="fde70-110">参照してください、[次の手順](#next-steps)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="fde70-110">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="fde70-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="fde70-111">Prerequisites</span></span>

* <span data-ttu-id="fde70-112">ASP.NET Core 1.1 (は実行されません 1.0)</span><span class="sxs-lookup"><span data-stu-id="fde70-112">ASP.NET Core 1.1 (does not run on 1.0)</span></span>
* <span data-ttu-id="fde70-113">ASP.NET Core 上で実行される OS:</span><span class="sxs-lookup"><span data-stu-id="fde70-113">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="fde70-114">Windows 7/Windows Server 2008 以降</span><span class="sxs-lookup"><span data-stu-id="fde70-114">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="fde70-115">Linux</span><span class="sxs-lookup"><span data-stu-id="fde70-115">Linux</span></span>
  * <span data-ttu-id="fde70-116">MacOS</span><span class="sxs-lookup"><span data-stu-id="fde70-116">macOS</span></span>

* <span data-ttu-id="fde70-117">**例外**: 場合は、IIS での windows アプリの実行中または WebListener で使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fde70-117">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="fde70-118">Windows 8/Windows Server 2012 以降</span><span class="sxs-lookup"><span data-stu-id="fde70-118">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="fde70-119">IIS 8 IIS 8 Express/</span><span class="sxs-lookup"><span data-stu-id="fde70-119">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="fde70-120">IIS で WebSocket を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fde70-120">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="fde70-121">サポートされるブラウザー http://caniuse.com/#feat=websockets を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fde70-121">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="fde70-122">使用する状況</span><span class="sxs-lookup"><span data-stu-id="fde70-122">When to use it</span></span>

<span data-ttu-id="fde70-123">ソケット接続で直接作業する必要がある場合は、Websocket を使用します。</span><span class="sxs-lookup"><span data-stu-id="fde70-123">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="fde70-124">たとえば、リアルタイムのゲームを最高のパフォーマンスを必要があります。</span><span class="sxs-lookup"><span data-stu-id="fde70-124">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="fde70-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr)充実したは、リアルタイムの機能はアプリケーション モデルは、ASP.NET、ASP.NET Core いない上でのみ実行します。</span><span class="sxs-lookup"><span data-stu-id="fde70-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="fde70-126">SignalR の中核となるバージョンがあるが開発です。を実行する、進行状況を参照してください、 [SignalR Core の GitHub リポジトリ](https://github.com/aspnet/SignalR)です。</span><span class="sxs-lookup"><span data-stu-id="fde70-126">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="fde70-127">SignalR Core を待機しない場合は、できる Websocket を直接使用するようになりました。</span><span class="sxs-lookup"><span data-stu-id="fde70-127">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="fde70-128">ただし、SignalR 提供するなどの機能を開発する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fde70-128">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="fde70-129">広い範囲の自動フォールバックを代替トランスポート メソッドを使用して、ブラウザーのバージョンをサポートします。</span><span class="sxs-lookup"><span data-stu-id="fde70-129">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="fde70-130">接続のドロップと自動再接続します。</span><span class="sxs-lookup"><span data-stu-id="fde70-130">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="fde70-131">サーバー上またはその逆のメソッドを呼び出すクライアントをサポートします。</span><span class="sxs-lookup"><span data-stu-id="fde70-131">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="fde70-132">複数のサーバーに合わせたスケーリングのサポート。</span><span class="sxs-lookup"><span data-stu-id="fde70-132">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="fde70-133">使用方法</span><span class="sxs-lookup"><span data-stu-id="fde70-133">How to use it</span></span>

* <span data-ttu-id="fde70-134">インストール、 [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="fde70-134">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="fde70-135">ミドルウェアを構成します。</span><span class="sxs-lookup"><span data-stu-id="fde70-135">Configure the middleware.</span></span>
* <span data-ttu-id="fde70-136">WebSocket 要求を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="fde70-136">Accept WebSocket requests.</span></span>
* <span data-ttu-id="fde70-137">メッセージを送受信します。</span><span class="sxs-lookup"><span data-stu-id="fde70-137">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="fde70-138">ミドルウェアを構成します。</span><span class="sxs-lookup"><span data-stu-id="fde70-138">Configure the middleware</span></span>

<span data-ttu-id="fde70-139">内で Websocket ミドルウェアを追加、`Configure`のメソッド、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="fde70-139">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="fde70-140">次の設定を構成できます。</span><span class="sxs-lookup"><span data-stu-id="fde70-140">The following settings can be configured:</span></span>

* <span data-ttu-id="fde70-141">`KeepAliveInterval`-方法プロキシ接続を維持しておくため、クライアントに"ping"フレームを送信する頻度。</span><span class="sxs-lookup"><span data-stu-id="fde70-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="fde70-142">`ReceiveBufferSize`-データの受信に使用するバッファーのサイズ。</span><span class="sxs-lookup"><span data-stu-id="fde70-142">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="fde70-143">上級ユーザーだけは、そのデータのサイズに基づくパフォーマンス チューニングのため、これを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fde70-143">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="fde70-144">WebSocket 要求を受け入れる</span><span class="sxs-lookup"><span data-stu-id="fde70-144">Accept WebSocket requests</span></span>

<span data-ttu-id="fde70-145">要求のライフ サイクルの後 (後半、`Configure`メソッドまたは例については、MVC アクション内で) かどうかは WebSocket 要求を確認し、WebSocket 要求を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="fde70-145">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="fde70-146">この例は後で、`Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="fde70-146">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="fde70-147">WebSocket 要求は、任意の URL でものがありますが、このサンプル コードの要求のみを受け付けます`/ws`です。</span><span class="sxs-lookup"><span data-stu-id="fde70-147">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="fde70-148">メッセージの送受信を行う</span><span class="sxs-lookup"><span data-stu-id="fde70-148">Send and receive messages</span></span>

<span data-ttu-id="fde70-149">`AcceptWebSocketAsync`メソッドは WebSocket 接続への TCP 接続をアップグレードし、使用する、 [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket)オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="fde70-149">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="fde70-150">メッセージを送受信するには、WebSocket オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="fde70-150">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="fde70-151">WebSocket 要求を受け入れる前に示したコード渡し、`WebSocket`オブジェクトを`Echo`メソッドです。 ここでの、`Echo`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="fde70-151">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="fde70-152">コードでは、メッセージを受信し、すぐに、同じメッセージを返信します。</span><span class="sxs-lookup"><span data-stu-id="fde70-152">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="fde70-153">クライアントが接続を閉じるまでこれを行うループ内に保持します。</span><span class="sxs-lookup"><span data-stu-id="fde70-153">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="fde70-154">このループを開始する前に、WebSocket を承諾するとミドルウェア パイプラインを終了します。</span><span class="sxs-lookup"><span data-stu-id="fde70-154">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="fde70-155">ソケットを閉じると、パイプラインをアンワインドします。</span><span class="sxs-lookup"><span data-stu-id="fde70-155">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="fde70-156">つまりと同様、WebSocket を承諾すると、パイプラインに進むと、要求が停止は、たとえば MVC アクションをヒットしたときに。</span><span class="sxs-lookup"><span data-stu-id="fde70-156">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="fde70-157">ただし、このループを終了し、ソケットを閉じると場合、に、パイプラインをバックアップする要求が行われます。</span><span class="sxs-lookup"><span data-stu-id="fde70-157">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fde70-158">次のステップ</span><span class="sxs-lookup"><span data-stu-id="fde70-158">Next steps</span></span>

<span data-ttu-id="fde70-159">[サンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)に付属しているこの記事では、単純なエコー アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="fde70-159">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="fde70-160">WebSocket 接続を作成する web ページがあり、サーバーを再クライアントに受信したメッセージ。</span><span class="sxs-lookup"><span data-stu-id="fde70-160">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="fde70-161">(これがいない設定を IIS Express で Visual Studio を実行する)、コマンド プロンプトから実行し、http://localhost:5000 に移動します。</span><span class="sxs-lookup"><span data-stu-id="fde70-161">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="fde70-162">Web ページは、左上にある接続の状態を示しています。</span><span class="sxs-lookup"><span data-stu-id="fde70-162">The web page shows connection status at the upper left:</span></span>

![Web ページの初期状態](websockets/_static/start.png)

<span data-ttu-id="fde70-164">選択**接続**表示される URL に WebSocket 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="fde70-164">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="fde70-165">テスト メッセージを入力し、選択**送信**です。</span><span class="sxs-lookup"><span data-stu-id="fde70-165">Enter a test message and select **Send**.</span></span> <span data-ttu-id="fde70-166">終了したら、選択**閉じるソケット**です。</span><span class="sxs-lookup"><span data-stu-id="fde70-166">When done, select **Close Socket**.</span></span> <span data-ttu-id="fde70-167">**通信ログ**セクションは、各 open、send をレポートでありは閉じたときの操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="fde70-167">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Web ページの初期状態](websockets/_static/end.png)
