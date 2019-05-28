---
title: ASP.NET Core での Websocket のサポート
author: rick-anderson
description: ASP.NET Core で Websocket の使用を開始する方法を説明します。
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/10/2019
uid: fundamentals/websockets
ms.openlocfilehash: bba9cf051deaf57efdd82ca2fb1318fce79bd6cc
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223217"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="73ee5-103">ASP.NET Core での Websocket のサポート</span><span class="sxs-lookup"><span data-stu-id="73ee5-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="73ee5-104">作成者: [Tom Dykstra](https://github.com/tdykstra) および [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="73ee5-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="73ee5-105">この記事では、ASP.NET Core で Websocket の使用を開始する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="73ee5-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) は、TCP 接続を使用した双方向の永続的通信チャネルを有効にするプロトコルです。</span><span class="sxs-lookup"><span data-stu-id="73ee5-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="73ee5-107">このプロトコルは、チャット、ダッシュボード、ゲーム アプリなど、高速かつリアルタイムのコミュニケーションを活用するアプリで使用されます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="73ee5-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="73ee5-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="73ee5-109">詳細については、「[次の手順](#next-steps)」のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="73ee5-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73ee5-110">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="73ee5-110">Prerequisites</span></span>

* <span data-ttu-id="73ee5-111">ASP.NET Core 1.1 以降</span><span class="sxs-lookup"><span data-stu-id="73ee5-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="73ee5-112">ASP.NET Core をサポートする任意の OS:</span><span class="sxs-lookup"><span data-stu-id="73ee5-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="73ee5-113">Windows 7 / Windows Server 2008 以降</span><span class="sxs-lookup"><span data-stu-id="73ee5-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="73ee5-114">Linux</span><span class="sxs-lookup"><span data-stu-id="73ee5-114">Linux</span></span>
  * <span data-ttu-id="73ee5-115">macOS</span><span class="sxs-lookup"><span data-stu-id="73ee5-115">macOS</span></span>
  
* <span data-ttu-id="73ee5-116">アプリが IIS を含む Windows 上で実行されている場合:</span><span class="sxs-lookup"><span data-stu-id="73ee5-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="73ee5-117">Windows 8 / Windows Server 2012 以降</span><span class="sxs-lookup"><span data-stu-id="73ee5-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="73ee5-118">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="73ee5-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="73ee5-119">WebSockets を有効にする必要があります (「[IIS/IIS Express のサポート](#iisiis-express-support)」セクションを参照してください)。</span><span class="sxs-lookup"><span data-stu-id="73ee5-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="73ee5-120">アプリが [HTTP.sys](xref:fundamentals/servers/httpsys) で実行されている場合:</span><span class="sxs-lookup"><span data-stu-id="73ee5-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="73ee5-121">Windows 8 / Windows Server 2012 以降</span><span class="sxs-lookup"><span data-stu-id="73ee5-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="73ee5-122">サポートされているブラウザーについては、 https://caniuse.com/#feat=websockets を参照してください。</span><span class="sxs-lookup"><span data-stu-id="73ee5-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="73ee5-123">WebSockets を使用する場合</span><span class="sxs-lookup"><span data-stu-id="73ee5-123">When to use WebSockets</span></span>

<span data-ttu-id="73ee5-124">Websocket を使用して、ソケット接続を直接使用します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="73ee5-125">たとえば、リアルタイムのゲームで最高のパフォーマンスが必要な場合に WebSockets を使用します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="73ee5-126">[ASP.NET Core SignalR](xref:signalr/introduction) は、アプリへのリアルタイム Web 機能の追加を簡単にするライブラリです。</span><span class="sxs-lookup"><span data-stu-id="73ee5-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="73ee5-127">可能なかぎり、WebSocket が使用されます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="73ee5-128">WebSocket の使用方法</span><span class="sxs-lookup"><span data-stu-id="73ee5-128">How to use WebSockets</span></span>

* <span data-ttu-id="73ee5-129">[Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="73ee5-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="73ee5-130">ミドルウェアを構成します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-130">Configure the middleware.</span></span>
* <span data-ttu-id="73ee5-131">WebSocket の要求を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="73ee5-132">メッセージを送受信します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="73ee5-133">ミドルウェアの構成</span><span class="sxs-lookup"><span data-stu-id="73ee5-133">Configure the middleware</span></span>

<span data-ttu-id="73ee5-134">`Startup` クラスの `Configure` メソッドに、Websocket ミドルウェアを追加します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="73ee5-135">次の設定を構成できます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-135">The following settings can be configured:</span></span>

* <span data-ttu-id="73ee5-136">`KeepAliveInterval`: プロキシの接続の維持を保証する、クライアントに "ping" フレームを送信する頻度。</span><span class="sxs-lookup"><span data-stu-id="73ee5-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="73ee5-137">既定値は 2 分です。</span><span class="sxs-lookup"><span data-stu-id="73ee5-137">The default is two minutes.</span></span>
* <span data-ttu-id="73ee5-138">`ReceiveBufferSize`: データの受信に使用されるバッファーのサイズ。</span><span class="sxs-lookup"><span data-stu-id="73ee5-138">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="73ee5-139">これは、上級ユーザーが、データのサイズに応じたパフォーマンス調整のために変更する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="73ee5-139">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="73ee5-140">既定値は 4 KB です。</span><span class="sxs-lookup"><span data-stu-id="73ee5-140">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="73ee5-141">次の設定を構成できます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-141">The following settings can be configured:</span></span>

* <span data-ttu-id="73ee5-142">`KeepAliveInterval`: プロキシの接続の維持を保証する、クライアントに "ping" フレームを送信する頻度。</span><span class="sxs-lookup"><span data-stu-id="73ee5-142">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="73ee5-143">既定値は 2 分です。</span><span class="sxs-lookup"><span data-stu-id="73ee5-143">The default is two minutes.</span></span>
* <span data-ttu-id="73ee5-144">`ReceiveBufferSize`: データの受信に使用されるバッファーのサイズ。</span><span class="sxs-lookup"><span data-stu-id="73ee5-144">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="73ee5-145">これは、上級ユーザーが、データのサイズに応じたパフォーマンス調整のために変更する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="73ee5-145">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="73ee5-146">既定値は 4 KB です。</span><span class="sxs-lookup"><span data-stu-id="73ee5-146">The default is 4 KB.</span></span>
* <span data-ttu-id="73ee5-147">`AllowedOrigins` - WebSocket 要求で許可される配信元ヘッダー値の一覧。</span><span class="sxs-lookup"><span data-stu-id="73ee5-147">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="73ee5-148">既定では、すべての配信元が許可されています。</span><span class="sxs-lookup"><span data-stu-id="73ee5-148">By default, all origins are allowed.</span></span> <span data-ttu-id="73ee5-149">詳細については、下記の "WebSocket の配信元の制限" を参照してください。</span><span class="sxs-lookup"><span data-stu-id="73ee5-149">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="73ee5-150">WebSocket の要求の受け入れ</span><span class="sxs-lookup"><span data-stu-id="73ee5-150">Accept WebSocket requests</span></span>

<span data-ttu-id="73ee5-151">以降の要求ライフサイクルのどこかで (たとえば、以降の `Configure` メソッドまたは MVC アクションで)、それが WebSocket 要求であるかを確認し、WebSocket 要求を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-151">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="73ee5-152">次の例は、以降の `Configure` メソッドから抜粋したものです。</span><span class="sxs-lookup"><span data-stu-id="73ee5-152">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="73ee5-153">WebSocket 要求はどの URL からも受け取る場合がありますが、このサンプル コードでは `/ws` の要求のみを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="73ee5-153">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

<span data-ttu-id="73ee5-154">WebSocket を使用するとき、接続中、ミドルウェア パイプラインの実行を維持する**必要があります**。</span><span class="sxs-lookup"><span data-stu-id="73ee5-154">When using a WebSocket, you **must** keep the middleware pipeline running for the duration of the connection.</span></span> <span data-ttu-id="73ee5-155">ミドルウェア パイプラインの修了後に WebSocket メッセージを送信するか、受信する場合、次のような例外を受け取ることがあります。</span><span class="sxs-lookup"><span data-stu-id="73ee5-155">If you attempt to send or receive a WebSocket message after the middleware pipeline ends, you may get an exception like the following:</span></span>

```
System.Net.WebSockets.WebSocketException (0x80004005): The remote party closed the WebSocket connection without completing the close handshake. ---> System.ObjectDisposedException: Cannot write to the response body, the response has completed.
Object name: 'HttpResponseStream'.
```

<span data-ttu-id="73ee5-156">バックグラウンド サービスを利用してデータを WebSocket に書き込む場合、ミドルウェア パイプラインの実行を維持します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-156">If you're using a background service to write data to a WebSocket, make sure you keep the middleware pipeline running.</span></span> <span data-ttu-id="73ee5-157">これは <xref:System.Threading.Tasks.TaskCompletionSource%601> を使用して行います。</span><span class="sxs-lookup"><span data-stu-id="73ee5-157">Do this by using a <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span></span> <span data-ttu-id="73ee5-158">`TaskCompletionSource` をバックグラウンド サービスに渡し、WebSocket が終わったとき、それに <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> を呼び出させます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-158">Pass the `TaskCompletionSource` to your background service and have it call <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> when you finish with the WebSocket.</span></span> <span data-ttu-id="73ee5-159">次に、要求中、<xref:System.Threading.Tasks.TaskCompletionSource%601.Task> プロパティを `await` します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-159">Then `await` the <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> property during the request.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="73ee5-160">メッセージの送受信</span><span class="sxs-lookup"><span data-stu-id="73ee5-160">Send and receive messages</span></span>

<span data-ttu-id="73ee5-161">`AcceptWebSocketAsync` メソッドは、TCP 接続を WebSocket 接続にアップグレードし、[WebSocket](/dotnet/core/api/system.net.websockets.websocket) オブジェクトを提供します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-161">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="73ee5-162">メッセージの送受信に、`WebSocket` オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-162">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="73ee5-163">前に示した、WebSocket 要求を受け入れるコードが、`WebSocket` オブジェクトを `Echo` メソッドに渡します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-163">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="73ee5-164">このコードは、メッセージを受信し、同じメッセージをすぐに送信します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-164">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="73ee5-165">クライアントが接続を閉じるまで、メッセージがループで送受信されます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-165">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="73ee5-166">このループを開始する前に、WebSocket 接続を受け入れた場合、ミドルウェア パイプラインは終了します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-166">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="73ee5-167">ソケットを閉じると、パイプラインはアンワインドされます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-167">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="73ee5-168">つまり、WebSocket を受け入れると、要求はパイプラインでの先への移動を中止します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-168">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="73ee5-169">ループを終了し、ソケットを閉じた場合、要求はパイプラインのバックアップを続けます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-169">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="handle-client-disconnects"></a><span data-ttu-id="73ee5-170">クライアントの切断の処理</span><span class="sxs-lookup"><span data-stu-id="73ee5-170">Handle client disconnects</span></span>

<span data-ttu-id="73ee5-171">接続の損失によってクライアントが切断されても、サーバーに自動的に通知されるわけではありません。</span><span class="sxs-lookup"><span data-stu-id="73ee5-171">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="73ee5-172">サーバーが切断メッセージを受信するのは、クライアントがそれを送信した場合のみです。インターネット接続が失われた場合、これを実行することはできません。</span><span class="sxs-lookup"><span data-stu-id="73ee5-172">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="73ee5-173">これが発生した場合に何らかのアクションを実行したい場合は、特定の時間枠内でクライアントからの受信を待つタイムアウトを設定します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-173">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="73ee5-174">クライアントが常にメッセージを送信するとは限らず、その接続がアイドル状態になっただけでタイムアウトしたくない場合は、X 秒ごとに ping メッセージを送信するタイマーをクライアントに使用させます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-174">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="73ee5-175">サーバー上では、前のものから 2\*X 秒以内にメッセージが到着しなかった場合に、接続を終了してクライアントが切断されたことをレポートします。</span><span class="sxs-lookup"><span data-stu-id="73ee5-175">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="73ee5-176">予想される 2 倍の期間を待機することで、ping メッセージを遅らせる可能性のあるネットワークの遅延のために余分な時間を残します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-176">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="73ee5-177">WebSocket の配信元の制限</span><span class="sxs-lookup"><span data-stu-id="73ee5-177">WebSocket origin restriction</span></span>

<span data-ttu-id="73ee5-178">CORS で提供される保護は、WebSocket には適用されません。</span><span class="sxs-lookup"><span data-stu-id="73ee5-178">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="73ee5-179">ブラウザーでは以下を実行**しません**。</span><span class="sxs-lookup"><span data-stu-id="73ee5-179">Browsers do **not**:</span></span>

* <span data-ttu-id="73ee5-180">CORS の事前要求を実行する。</span><span class="sxs-lookup"><span data-stu-id="73ee5-180">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="73ee5-181">WebSocket 要求を行うときに `Access-Control` ヘッダーに指定された制限を考慮する。</span><span class="sxs-lookup"><span data-stu-id="73ee5-181">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="73ee5-182">ただし、WebSocket 要求を発行するときにはブラウザーから `Origin` ヘッダーが送信されます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-182">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="73ee5-183">予期した配信元からの WebSocket のみが許可されるように、アプリケーションでこれらのヘッダーが検証されるように構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="73ee5-183">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="73ee5-184">"https://server.com" でサーバーを、"https://client.com" でクライアントをホスティングしている場合は、検証のために "https://client.com" を WebSocket の `AllowedOrigins` 一覧に追加します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-184">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="73ee5-185">`Origin` ヘッダーは、クライアントによって制御され、`Referer` のように偽装することができます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-185">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="73ee5-186">これらのヘッダーを認証メカニズムとして使用**しないでください**。</span><span class="sxs-lookup"><span data-stu-id="73ee5-186">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="73ee5-187">IIS/IIS Express のサポート</span><span class="sxs-lookup"><span data-stu-id="73ee5-187">IIS/IIS Express support</span></span>

<span data-ttu-id="73ee5-188">IIS/IIS Express 8 以降を含む、Windows Server 2012 以降および Windows 8 以降では、WebSocket プロトコルをサポートします。</span><span class="sxs-lookup"><span data-stu-id="73ee5-188">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="73ee5-189">IIS Express を使用する場合、WebSockets は常に有効になります。</span><span class="sxs-lookup"><span data-stu-id="73ee5-189">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="73ee5-190">IIS での WebSockets の有効化</span><span class="sxs-lookup"><span data-stu-id="73ee5-190">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="73ee5-191">Windows Server 2012 以降で WebSocket プロトコルのサポートを有効にするには</span><span class="sxs-lookup"><span data-stu-id="73ee5-191">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="73ee5-192">これらの手順は、IIS Express を使用する場合は必要ありません</span><span class="sxs-lookup"><span data-stu-id="73ee5-192">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="73ee5-193">**[管理]** メニューから**役割と機能の追加**ウィザードを使用するか、**サーバー マネージャー**にあるリンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-193">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="73ee5-194">**[役割ベースまたは機能ベースのインストール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-194">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="73ee5-195">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-195">Select **Next**.</span></span>
1. <span data-ttu-id="73ee5-196">適切なサーバーを選択します (既定では、ローカル サーバーが選択されます)。</span><span class="sxs-lookup"><span data-stu-id="73ee5-196">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="73ee5-197">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-197">Select **Next**.</span></span>
1. <span data-ttu-id="73ee5-198">**[役割]** ツリーで **[Web サーバー (IIS)]** を展開し、 **[Web サーバー]** 、 **[アプリケーション開発]** の順に展開します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-198">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="73ee5-199">**[WebSocket プロトコル]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-199">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="73ee5-200">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-200">Select **Next**.</span></span>
1. <span data-ttu-id="73ee5-201">追加機能が不要な場合は、 **[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-201">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="73ee5-202">**[インストール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-202">Select **Install**.</span></span>
1. <span data-ttu-id="73ee5-203">インストールが完了したら、 **[閉じる]** を選択してウィザードを終了します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-203">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="73ee5-204">Windows 8 以降で WebSocket プロトコルのサポートを有効にするには</span><span class="sxs-lookup"><span data-stu-id="73ee5-204">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="73ee5-205">これらの手順は、IIS Express を使用する場合は必要ありません</span><span class="sxs-lookup"><span data-stu-id="73ee5-205">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="73ee5-206">**[コントロール パネル]**  >  **[プログラム]**  >  **[プログラムと機能]**  >  **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-206">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="73ee5-207">次のノード: **[インターネット インフォメーション サービス]**  >  **[World Wide Web サービス]**  >  **[アプリケーション開発機能]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-207">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="73ee5-208">**[WebSocket プロトコル]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-208">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="73ee5-209">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-209">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="73ee5-210">Node.js で socket.io を使用する場合に WebSocket を無効にする</span><span class="sxs-lookup"><span data-stu-id="73ee5-210">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="73ee5-211">[Node.js](https://nodejs.org/) の [socket.io](https://socket.io/) で WebSocket サポートを使用する場合、`webSocket` 要素を使用する既定の WebSocket モジュールを *web.config* または *applicationHost.config* で無効にします。この手順が実行されない場合、IIS WebSocket モジュールは Node.js とアプリ以外の WebSocket コミュニケーションを処理しようとします。</span><span class="sxs-lookup"><span data-stu-id="73ee5-211">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="73ee5-212">次の手順</span><span class="sxs-lookup"><span data-stu-id="73ee5-212">Next steps</span></span>

<span data-ttu-id="73ee5-213">この記事に添えられている[サンプル アプリ](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples)は、エコー アプリです。</span><span class="sxs-lookup"><span data-stu-id="73ee5-213">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="73ee5-214">これには、WebSocket 接続を作成する Web ページがあり、サーバーが受け取るすべてのメッセージをクライアントに再送信します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-214">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="73ee5-215">コマンド プロンプトからアプリを実行し (IIS Express を使用した Visual Studio からは実行するように設定されていません)、 http://localhost:5000 に移動します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-215">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="73ee5-216">Web ページの左上に、接続の状態が示されます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-216">The web page shows the connection status in the upper left:</span></span>

![Web ページの初期状態](websockets/_static/start.png)

<span data-ttu-id="73ee5-218">**[接続]** を選択し、表示されている URL に WebSocket 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-218">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="73ee5-219">テスト メッセージを入力し、 **[送信]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-219">Enter a test message and select **Send**.</span></span> <span data-ttu-id="73ee5-220">完了したら、 **[Close Socket]\(ソケットを閉じる\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="73ee5-220">When done, select **Close Socket**.</span></span> <span data-ttu-id="73ee5-221">**[Communication Log]\(コミュニケーション ログ\)** セクションに、発生した各オープン、送信、クローズのアクションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="73ee5-221">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Web ページの初期状態](websockets/_static/end.png)
