---
title: ASP.NET Core での Websocket のサポート
author: rick-anderson
description: ASP.NET Core で Websocket の使用を開始する方法を説明します。
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: b1e2180ed8dc93e2474ecca371d386830b7f3a9f
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348456"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="bca4c-103">ASP.NET Core での Websocket のサポート</span><span class="sxs-lookup"><span data-stu-id="bca4c-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="bca4c-104">作成者: [Tom Dykstra](https://github.com/tdykstra) および [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="bca4c-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="bca4c-105">この記事では、ASP.NET Core で Websocket の使用を開始する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="bca4c-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) は、TCP 接続を使用した双方向の永続的通信チャネルを有効にするプロトコルです。</span><span class="sxs-lookup"><span data-stu-id="bca4c-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="bca4c-107">このプロトコルは、チャット、ダッシュボード、ゲーム アプリなど、高速かつリアルタイムのコミュニケーションを活用するアプリで使用されます。</span><span class="sxs-lookup"><span data-stu-id="bca4c-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="bca4c-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="bca4c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="bca4c-109">詳細については、「[次の手順](#next-steps)」のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="bca4c-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bca4c-110">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="bca4c-110">Prerequisites</span></span>

* <span data-ttu-id="bca4c-111">ASP.NET Core 1.1 以降</span><span class="sxs-lookup"><span data-stu-id="bca4c-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="bca4c-112">ASP.NET Core をサポートする任意の OS:</span><span class="sxs-lookup"><span data-stu-id="bca4c-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="bca4c-113">Windows 7 / Windows Server 2008 以降</span><span class="sxs-lookup"><span data-stu-id="bca4c-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="bca4c-114">Linux</span><span class="sxs-lookup"><span data-stu-id="bca4c-114">Linux</span></span>
  * <span data-ttu-id="bca4c-115">macOS</span><span class="sxs-lookup"><span data-stu-id="bca4c-115">macOS</span></span>
  
* <span data-ttu-id="bca4c-116">アプリが IIS を含む Windows 上で実行されている場合:</span><span class="sxs-lookup"><span data-stu-id="bca4c-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="bca4c-117">Windows 8 / Windows Server 2012 以降</span><span class="sxs-lookup"><span data-stu-id="bca4c-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="bca4c-118">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="bca4c-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="bca4c-119">WebSockets を有効にする必要があります (「[IIS/IIS Express のサポート](#iisiis-express-support)」セクションを参照してください)。</span><span class="sxs-lookup"><span data-stu-id="bca4c-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="bca4c-120">アプリが [HTTP.sys](xref:fundamentals/servers/httpsys) で実行されている場合:</span><span class="sxs-lookup"><span data-stu-id="bca4c-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="bca4c-121">Windows 8 / Windows Server 2012 以降</span><span class="sxs-lookup"><span data-stu-id="bca4c-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="bca4c-122">サポートされているブラウザーについては、 https://caniuse.com/#feat=websockets を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bca4c-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="bca4c-123">WebSockets を使用する場合</span><span class="sxs-lookup"><span data-stu-id="bca4c-123">When to use WebSockets</span></span>

<span data-ttu-id="bca4c-124">Websocket を使用して、ソケット接続を直接使用します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="bca4c-125">たとえば、リアルタイムのゲームで最高のパフォーマンスが必要な場合に WebSockets を使用します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="bca4c-126">[ASP.NET Core SignalR](xref:signalr/introduction) は、アプリへのリアルタイム Web 機能の追加を簡単にするライブラリです。</span><span class="sxs-lookup"><span data-stu-id="bca4c-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="bca4c-127">可能なかぎり、WebSocket が使用されます。</span><span class="sxs-lookup"><span data-stu-id="bca4c-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="bca4c-128">WebSocket の使用方法</span><span class="sxs-lookup"><span data-stu-id="bca4c-128">How to use WebSockets</span></span>

* <span data-ttu-id="bca4c-129">[Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="bca4c-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="bca4c-130">ミドルウェアを構成します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-130">Configure the middleware.</span></span>
* <span data-ttu-id="bca4c-131">WebSocket の要求を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="bca4c-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="bca4c-132">メッセージを送受信します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="bca4c-133">ミドルウェアの構成</span><span class="sxs-lookup"><span data-stu-id="bca4c-133">Configure the middleware</span></span>

<span data-ttu-id="bca4c-134">`Startup` クラスの `Configure` メソッドに、Websocket ミドルウェアを追加します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

<span data-ttu-id="bca4c-135">次の設定を構成できます。</span><span class="sxs-lookup"><span data-stu-id="bca4c-135">The following settings can be configured:</span></span>

* <span data-ttu-id="bca4c-136">`KeepAliveInterval`: プロキシの接続の維持を保証する、クライアントに "ping" フレームを送信する頻度。</span><span class="sxs-lookup"><span data-stu-id="bca4c-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="bca4c-137">`ReceiveBufferSize`: データの受信に使用されるバッファーのサイズ。</span><span class="sxs-lookup"><span data-stu-id="bca4c-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="bca4c-138">これは、上級ユーザーが、データのサイズに応じたパフォーマンス調整のために変更する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="bca4c-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="bca4c-139">WebSocket の要求の受け入れ</span><span class="sxs-lookup"><span data-stu-id="bca4c-139">Accept WebSocket requests</span></span>

<span data-ttu-id="bca4c-140">以降の要求ライフサイクルのどこかで (たとえば、以降の `Configure` メソッドまたは MVC アクションで)、それが WebSocket 要求であるかを確認し、WebSocket 要求を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="bca4c-140">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="bca4c-141">次の例は、以降の `Configure` メソッドから抜粋したものです。</span><span class="sxs-lookup"><span data-stu-id="bca4c-141">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="bca4c-142">WebSocket 要求はどの URL からも受け取る場合がありますが、このサンプル コードでは `/ws` の要求のみを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="bca4c-142">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="bca4c-143">メッセージの送受信</span><span class="sxs-lookup"><span data-stu-id="bca4c-143">Send and receive messages</span></span>

<span data-ttu-id="bca4c-144">`AcceptWebSocketAsync` メソッドは、TCP 接続を WebSocket 接続にアップグレードし、[WebSocket](/dotnet/core/api/system.net.websockets.websocket) オブジェクトを提供します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-144">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="bca4c-145">メッセージの送受信に、`WebSocket` オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-145">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="bca4c-146">前に示した、WebSocket 要求を受け入れるコードが、`WebSocket` オブジェクトを `Echo` メソッドに渡します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-146">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="bca4c-147">このコードは、メッセージを受信し、同じメッセージをすぐに送信します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-147">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="bca4c-148">クライアントが接続を閉じるまで、メッセージがループで送受信されます。</span><span class="sxs-lookup"><span data-stu-id="bca4c-148">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="bca4c-149">このループを開始する前に、WebSocket 接続を受け入れた場合、ミドルウェア パイプラインは終了します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-149">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="bca4c-150">ソケットを閉じると、パイプラインはアンワインドされます。</span><span class="sxs-lookup"><span data-stu-id="bca4c-150">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="bca4c-151">つまり、WebSocket を受け入れると、要求はパイプラインでの先への移動を中止します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-151">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="bca4c-152">ループを終了し、ソケットを閉じた場合、要求はパイプラインのバックアップを続けます。</span><span class="sxs-lookup"><span data-stu-id="bca4c-152">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="bca4c-153">IIS/IIS Express のサポート</span><span class="sxs-lookup"><span data-stu-id="bca4c-153">IIS/IIS Express support</span></span>

<span data-ttu-id="bca4c-154">IIS/IIS Express 8 以降を含む、Windows Server 2012 以降および Windows 8 以降では、WebSocket プロトコルをサポートします。</span><span class="sxs-lookup"><span data-stu-id="bca4c-154">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="bca4c-155">IIS Express を使用する場合、WebSockets は常に有効になります。</span><span class="sxs-lookup"><span data-stu-id="bca4c-155">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="bca4c-156">IIS での WebSockets の有効化</span><span class="sxs-lookup"><span data-stu-id="bca4c-156">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="bca4c-157">Windows Server 2012 以降で WebSocket プロトコルのサポートを有効にするには</span><span class="sxs-lookup"><span data-stu-id="bca4c-157">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="bca4c-158">これらの手順は、IIS Express を使用する場合は必要ありません</span><span class="sxs-lookup"><span data-stu-id="bca4c-158">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="bca4c-159">**[管理]** メニューから**役割と機能の追加**ウィザードを使用するか、**サーバー マネージャー**にあるリンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-159">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="bca4c-160">**[役割ベースまたは機能ベースのインストール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-160">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="bca4c-161">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-161">Select **Next**.</span></span>
1. <span data-ttu-id="bca4c-162">適切なサーバーを選択します (既定では、ローカル サーバーが選択されます)。</span><span class="sxs-lookup"><span data-stu-id="bca4c-162">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="bca4c-163">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-163">Select **Next**.</span></span>
1. <span data-ttu-id="bca4c-164">**[役割]** ツリーで **[Web サーバー (IIS)]** を展開し、**[Web サーバー]**、**[アプリケーション開発]** の順に展開します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-164">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="bca4c-165">**[WebSocket プロトコル]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-165">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="bca4c-166">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-166">Select **Next**.</span></span>
1. <span data-ttu-id="bca4c-167">追加機能が不要な場合は、**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-167">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="bca4c-168">**[インストール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-168">Select **Install**.</span></span>
1. <span data-ttu-id="bca4c-169">インストールが完了したら、**[閉じる]** を選択してウィザードを終了します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-169">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="bca4c-170">Windows 8 以降で WebSocket プロトコルのサポートを有効にするには</span><span class="sxs-lookup"><span data-stu-id="bca4c-170">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="bca4c-171">これらの手順は、IIS Express を使用する場合は必要ありません</span><span class="sxs-lookup"><span data-stu-id="bca4c-171">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="bca4c-172">**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-172">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="bca4c-173">次のノード: **[インターネット インフォメーション サービス]** > **[World Wide Web サービス]** > **[アプリケーション開発機能]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="bca4c-173">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="bca4c-174">**[WebSocket プロトコル]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-174">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="bca4c-175">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-175">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="bca4c-176">Node.js で socket.io を使用する場合に WebSocket を無効にする</span><span class="sxs-lookup"><span data-stu-id="bca4c-176">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="bca4c-177">[Node.js](https://nodejs.org/) の [socket.io](https://socket.io/) で WebSocket サポートを使用する場合、`webSocket` 要素を使用する既定の WebSocket モジュールを *web.config* または *applicationHost.config* で無効にします。この手順が実行されない場合、IIS WebSocket モジュールは Node.js とアプリ以外の WebSocket コミュニケーションを処理しようとします。</span><span class="sxs-lookup"><span data-stu-id="bca4c-177">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="bca4c-178">次の手順</span><span class="sxs-lookup"><span data-stu-id="bca4c-178">Next steps</span></span>

<span data-ttu-id="bca4c-179">この記事に添えられている[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples)は、エコー アプリです。</span><span class="sxs-lookup"><span data-stu-id="bca4c-179">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="bca4c-180">これには、WebSocket 接続を作成する Web ページがあり、サーバーが受け取るすべてのメッセージをクライアントに再送信します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-180">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="bca4c-181">コマンド プロンプトからアプリを実行し (IIS Express を使用した Visual Studio からは実行するように設定されていません)、 http://localhost:5000 に移動します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-181">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="bca4c-182">Web ページの左上に、接続の状態が示されます。</span><span class="sxs-lookup"><span data-stu-id="bca4c-182">The web page shows the connection status in the upper left:</span></span>

![Web ページの初期状態](websockets/_static/start.png)

<span data-ttu-id="bca4c-184">**[接続]** を選択し、表示されている URL に WebSocket 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-184">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="bca4c-185">テスト メッセージを入力し、**[送信]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-185">Enter a test message and select **Send**.</span></span> <span data-ttu-id="bca4c-186">完了したら、**[Close Socket]\(ソケットを閉じる\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bca4c-186">When done, select **Close Socket**.</span></span> <span data-ttu-id="bca4c-187">**[Communication Log]\(コミュニケーション ログ\)** セクションに、発生した各オープン、送信、クローズのアクションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bca4c-187">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Web ページの初期状態](websockets/_static/end.png)
