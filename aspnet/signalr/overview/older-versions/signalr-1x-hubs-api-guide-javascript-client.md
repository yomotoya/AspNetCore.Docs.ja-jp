---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: SignalR 1.x ハブ API ガイド - JavaScript クライアント |Microsoft Docs
author: pfletcher
description: このドキュメントでは、ブラウザーや Windows ストア (WinJS) アプリなどの JavaScript クライアントではバージョン 1.1 SignalR ハブ API を使用するように紹介しています.
ms.author: aspnetcontent
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 17b1088ca71f206e08bec62f3b13c43ad4bcc158
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828933"
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="db6f5-103">SignalR 1.x ハブ API ガイド - JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="db6f5-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="db6f5-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="db6f5-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="db6f5-105">このドキュメントでは、ブラウザーや Windows ストア (WinJS) アプリケーションなどの JavaScript クライアントではバージョン 1.1 SignalR ハブ API を使用するように紹介します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="db6f5-106">SignalR ハブの API では、サーバーからに接続されているクライアントとサーバーのクライアントからのリモート プロシージャ コール (Rpc) を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="db6f5-107">サーバー コードで、クライアントから呼び出すことができるメソッドを定義して、クライアント上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="db6f5-108">クライアント コードで、サーバーから呼び出すことができるメソッドを定義して、サーバー上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="db6f5-109">SignalR は、のすべてのクライアントとサーバーが処理されます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="db6f5-110">SignalR では、永続的な接続と呼ばれる下位レベル API も提供します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="db6f5-111">概要については、SignalR、ハブ、および永続的な接続は、または完全な SignalR アプリケーションを構築する方法を示すチュートリアルについてを参照してください。 [SignalR - Getting Started](../getting-started/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="db6f5-112">概要</span><span class="sxs-lookup"><span data-stu-id="db6f5-112">Overview</span></span>

<span data-ttu-id="db6f5-113">このドキュメントは、次のトピックに分かれています。</span><span class="sxs-lookup"><span data-stu-id="db6f5-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="db6f5-114">生成されたプロキシとは何をします。</span><span class="sxs-lookup"><span data-stu-id="db6f5-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="db6f5-115">生成されたプロキシを使用する場合</span><span class="sxs-lookup"><span data-stu-id="db6f5-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="db6f5-116">クライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="db6f5-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="db6f5-117">動的に生成されたプロキシを参照する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="db6f5-118">生成されたプロキシの SignalR の物理ファイルを作成する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="db6f5-119">接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="db6f5-120">$connection.hub が同じオブジェクトのその $.hubConnection() を作成します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="db6f5-121">Start メソッドの非同期実行</span><span class="sxs-lookup"><span data-stu-id="db6f5-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="db6f5-122">ドメイン間の接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="db6f5-123">接続を構成する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="db6f5-124">クエリ文字列パラメーターを指定する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="db6f5-125">トランスポート メソッドを指定する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="db6f5-126">ハブ クラスのプロキシを取得する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="db6f5-127">サーバーが呼び出すことができるクライアントでメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="db6f5-128">クライアントからサーバーのメソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="db6f5-129">接続の有効期間イベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="db6f5-130">エラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="db6f5-131">クライアント側のログ記録を有効にする方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="db6f5-132">サーバーや .NET クライアントをプログラミングする方法に関するドキュメントについては、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="db6f5-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="db6f5-133">SignalR ハブ API ガイド - サーバー</span><span class="sxs-lookup"><span data-stu-id="db6f5-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="db6f5-134">SignalR ハブ API ガイド - .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="db6f5-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="db6f5-135">.NET 4.5 バージョンの API は API のリファレンス トピックへのリンクです。</span><span class="sxs-lookup"><span data-stu-id="db6f5-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="db6f5-136">.NET 4 を使用している場合は、次を参照してください。 [.NET 4 のバージョンを API のトピックの](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="db6f5-137">生成されたプロキシとは何をします。</span><span class="sxs-lookup"><span data-stu-id="db6f5-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="db6f5-138">SignalR で生成されるプロキシの有無は、SignalR サービスと通信する JavaScript クライアントをプログラミングできます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="db6f5-139">プロキシが何は、接続、サーバーを呼び出すと、書き込みメソッドを使用して、サーバーでメソッドを呼び出すコードの構文を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="db6f5-140">サーバー メソッドを呼び出すコードを記述する場合、生成されたプロキシを使用すると、次のローカル関数を実行した場合と同様の構文を使用: 書き込める`serverMethod(arg1, arg2)`の代わりに`invoke('serverMethod', arg1, arg2)`します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="db6f5-141">生成されたプロキシ構文では、サーバーのメソッド名を入力し間違えた場合は、イミディ エイト ウィンドウと判読クライアント側のエラーもできます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="db6f5-142">サーバーのメソッドを呼び出すコードを記述するため、IntelliSense のサポートも取得できます、プロキシを定義するファイルを手動で作成する場合。</span><span class="sxs-lookup"><span data-stu-id="db6f5-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="db6f5-143">たとえば、次のようなハブ クラスをサーバー上にあるとします。</span><span class="sxs-lookup"><span data-stu-id="db6f5-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="db6f5-144">などを呼び出すための JavaScript コードは次のコード例を表示する、`NewContosoChatMessage`サーバーとの呼び出しを受信するメソッド、`addContosoChatMessageToPage`サーバーからのメソッド。</span><span class="sxs-lookup"><span data-stu-id="db6f5-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="db6f5-145">**生成されたプロキシを使用しました。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="db6f5-146">**生成されたプロキシを使用せず**</span><span class="sxs-lookup"><span data-stu-id="db6f5-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="db6f5-147">生成されたプロキシを使用する場合</span><span class="sxs-lookup"><span data-stu-id="db6f5-147">When to use the generated proxy</span></span>

<span data-ttu-id="db6f5-148">サーバーを呼び出すクライアント メソッドの複数のイベント ハンドラーを登録する場合は、生成されたプロキシを使用できません。</span><span class="sxs-lookup"><span data-stu-id="db6f5-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="db6f5-149">それ以外の場合、生成されたプロキシを使用することができますか、コーディングの基本設定に基づいていません。</span><span class="sxs-lookup"><span data-stu-id="db6f5-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="db6f5-150">これを使用しないように選択した場合は、「signalr ハブ」の URL を参照する必要はありません、`script`クライアント コード内の要素。</span><span class="sxs-lookup"><span data-stu-id="db6f5-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="db6f5-151">クライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="db6f5-151">Client setup</span></span>

<span data-ttu-id="db6f5-152">JavaScript クライアントでは、jQuery、および SignalR core の JavaScript ファイルへの参照が必要です。</span><span class="sxs-lookup"><span data-stu-id="db6f5-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="db6f5-153">JQuery バージョンは、1.6.4 または 1.7.2、1.8.2、1.9.1 など、主要な以降のバージョンである必要があります。</span><span class="sxs-lookup"><span data-stu-id="db6f5-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="db6f5-154">生成されたプロキシを使用する場合は、SignalR が生成されたプロキシの JavaScript ファイルへの参照を必要もあります。</span><span class="sxs-lookup"><span data-stu-id="db6f5-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="db6f5-155">次の例では、生成されたプロキシを使用して HTML ページに表示される参照を示します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="db6f5-156">この順序でこれらの参照を含める必要があります: jQuery 最初に、その後、SignalR core と SignalR プロキシが最後です。</span><span class="sxs-lookup"><span data-stu-id="db6f5-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="db6f5-157">動的に生成されたプロキシを参照する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="db6f5-158">前の例では、SignalR が生成されたプロキシへの参照は、物理ファイルが、動的に生成された JavaScript コードが。</span><span class="sxs-lookup"><span data-stu-id="db6f5-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="db6f5-159">SignalR では、その場でプロキシの JavaScript コードを作成し、それを「/signalr ハブ」URL への応答でクライアントに提供します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="db6f5-160">サーバーでは、SignalR 接続の場合は、さまざまなベース URL を指定したかどうか、`MapHubs`メソッドを動的に生成されるプロキシ ファイルの URL は、カスタムの URL に"/ハブ"が追加されます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="db6f5-161">Windows 8 (Windows ストア) JavaScript クライアントは、動的に生成されたものではなく、物理プロキシ ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="db6f5-162">詳細については、次を参照してください。 [SignalR の物理ファイルを作成する方法は、プロキシを生成](#manualproxy)このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="db6f5-163">ASP.NET MVC 4 Razor ビューで、チルダを使用して、アプリケーション ルートに、プロキシ ファイルのリファレンスを参照してください。</span><span class="sxs-lookup"><span data-stu-id="db6f5-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="db6f5-164">MVC 4 で SignalR を使用する方法の詳細については、次を参照してください。 [SignalR と MVC 4 の概要](tutorial-getting-started-with-signalr-and-mvc-4.md)します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="db6f5-165">ASP.NET MVC 3 Razor ビューで使用して`Url.Content`ファイル参照用プロキシに。</span><span class="sxs-lookup"><span data-stu-id="db6f5-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="db6f5-166">ASP.NET Web フォーム アプリケーションで使用`ResolveClientUrl`プロキシ ファイルの参照または (ティルダ以降)、アプリケーション ルートの相対パスを使用して、scriptmanager コントロールを使用して登録します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="db6f5-167">一般的な規則としては、CSS または JavaScript ファイルを使用する「/signalr ハブ」の URL を指定するため、同じメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="db6f5-168">URL を指定するには、チルダを使用せず、一部のシナリオで、アプリケーションは正しく動作 IIS Express を使用して Visual Studio でのテストが完全な IIS に展開するときは、404 エラーで失敗する際にします。</span><span class="sxs-lookup"><span data-stu-id="db6f5-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="db6f5-169">詳細については、次を参照してください。**ルート レベルのリソースへの参照を解決する**で[ASP.NET Web プロジェクト用の Visual Studio で Web サーバー](https://msdn.microsoft.com/library/58wxa9w5.aspx) MSDN サイト。</span><span class="sxs-lookup"><span data-stu-id="db6f5-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="db6f5-170">デバッグ モードで Visual Studio 2012 で web プロジェクトを実行すると、お使いのブラウザーとして Internet Explorer を使用する場合は、プロキシ ファイルを確認できます**ソリューション エクスプ ローラー** **スクリプト ドキュメント**のように、次の図。</span><span class="sxs-lookup"><span data-stu-id="db6f5-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![ソリューション エクスプ ローラーでの JavaScript 生成されるプロキシ ファイル](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="db6f5-172">ファイルの内容を表示するをダブルクリックして**hubs**します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="db6f5-173">Visual Studio 2012 および Internet Explorer を使用しない場合、またはデバッグ モードでない場合は、「/signalR ハブ」の URL を参照して、ファイルの内容を取得することもできます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="db6f5-174">サイトが実行されている場合など`http://localhost:56699`に移動して、`http://localhost:56699/SignalR/hubs`お使いのブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="db6f5-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="db6f5-175">生成されたプロキシの SignalR の物理ファイルを作成する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="db6f5-176">動的に生成されたプロキシを代わりは、プロキシ コードを含む物理ファイルを作成し、そのファイルを参照します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="db6f5-177">そのためにキャッシュまたはバンドルの動作を制御する場合またはサーバー メソッドの呼び出しをコーディングするときに、IntelliSense を取得することができます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="db6f5-178">プロキシ ファイルを作成するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="db6f5-179">インストール、 [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="db6f5-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="db6f5-180">コマンド プロンプトを開きを参照、*ツール*SignalR.exe ファイルを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="db6f5-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="db6f5-181">[ツール] フォルダーは、次の場所には。</span><span class="sxs-lookup"><span data-stu-id="db6f5-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="db6f5-182">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="db6f5-183">パス、 *.dll*は通常、 *bin*プロジェクト フォルダー内のフォルダー。</span><span class="sxs-lookup"><span data-stu-id="db6f5-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="db6f5-184">このコマンドは、という名前のファイルを作成します。 *server.js*と同じフォルダーに*signalr.exe*します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="db6f5-185">配置、 *server.js*プロジェクトの適切なフォルダーにファイル、アプリケーションの適切な名前を変更および「signalr ハブ」参照の代わりにへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="db6f5-186">接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-186">How to establish a connection</span></span>

<span data-ttu-id="db6f5-187">接続を確立する前に、接続オブジェクトを作成、プロキシを作成し、サーバーから呼び出すことができるメソッドのイベント ハンドラーを登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="db6f5-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="db6f5-188">プロキシとイベント ハンドラーが設定されているときに呼び出すことによって、接続の確立、`start`メソッド。</span><span class="sxs-lookup"><span data-stu-id="db6f5-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="db6f5-189">生成されたプロキシを使用している場合は、生成されたプロキシ コードによって処理されるため、独自のコードで接続オブジェクトを作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="db6f5-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="db6f5-190">**(生成されたプロキシ) との接続を確立します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="db6f5-191">**(なし、生成されたプロキシ) の接続を確立します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="db6f5-192">サンプル コードは、既定値を使用して"/signalr"SignalR サービスに接続するための URL。</span><span class="sxs-lookup"><span data-stu-id="db6f5-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="db6f5-193">別の基本 URL を指定する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバー -/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="db6f5-194">呼び出しの前にイベント ハンドラーを登録する通常、`start`メソッドは、接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="db6f5-195">接続の確立後にいくつかのイベント ハンドラーを登録すると、それを行うことができますが、少なくとも 1 つの呼び出しの前に、イベントを負いますを登録する必要があります、`start`メソッド。</span><span class="sxs-lookup"><span data-stu-id="db6f5-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="db6f5-196">この理由の 1 つがあります多くのハブ アプリケーションをトリガーする必要はありませんが、`OnConnected`うち 1 つを使用しようとするのみの場合は、すべてハブのイベント。</span><span class="sxs-lookup"><span data-stu-id="db6f5-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="db6f5-197">ハブのプロキシのクライアント メソッドの存在では、SignalR をトリガーするコードを指定は、接続が確立されたときに、`OnConnected`イベント。</span><span class="sxs-lookup"><span data-stu-id="db6f5-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="db6f5-198">呼び出しの前に、イベント ハンドラーを登録しない場合、`start`メソッド、ことができます、ハブはハブのメソッドを呼び出す`OnConnected`メソッドは呼び出されません、サーバーからクライアントのメソッドは呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="db6f5-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="db6f5-199">$connection.hub が同じオブジェクトのその $.hubConnection() を作成します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="db6f5-200">生成されたプロキシを使用する場合の例では、ご覧のとおり`$.connection.hub`接続オブジェクトを参照します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="db6f5-201">これは、同じオブジェクトを呼び出して取得する`$.hubConnection()`生成されたプロキシを使用していない場合。</span><span class="sxs-lookup"><span data-stu-id="db6f5-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="db6f5-202">生成されたプロキシ コードは、次のステートメントを実行することによって自動的に接続を作成します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![生成されるプロキシ ファイルで接続の作成](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="db6f5-204">生成されたプロキシを使用しているときに操作を実行すると`$.connection.hub`生成されたプロキシを使用していないときに、接続オブジェクトで実行できます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="db6f5-205">Start メソッドの非同期実行</span><span class="sxs-lookup"><span data-stu-id="db6f5-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="db6f5-206">`start`メソッドを非同期的に実行します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="db6f5-207">返します、 [jQuery 遅延オブジェクト](http://api.jquery.com/category/deferred-object/)などのメソッドを呼び出すことによってコールバック関数を追加するには、つまり`pipe`、`done`と`fail`します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="db6f5-208">接続が確立された後に実行するコードがあればなど、サーバー メソッドへの呼び出し、コールバック関数でそのコードを配置またはコールバック関数から呼び出すことです。</span><span class="sxs-lookup"><span data-stu-id="db6f5-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="db6f5-209">`.done`接続が確立されているしにあるいずれかのコードを後でコールバック メソッドが実行される、`OnConnected`サーバー上のイベント ハンドラー メソッドの実行が終了します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="db6f5-210">後のコードの次の行として前の例の「接続されているようになりました」ステートメントを配置した場合、`start`メソッドの呼び出し (ではなく、`.done`コールバック)、`console.log`行は、接続が確立される前に、次に示すように、実行されます例:</span><span class="sxs-lookup"><span data-stu-id="db6f5-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![接続が確立された後に実行されるコードを記述する方法を正しくないです。](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="db6f5-212">ドメイン間の接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="db6f5-213">通常、ブラウザーがからページを読み込む場合`http://contoso.com`、SignalR 接続が同じドメイン内では`http://contoso.com/signalr`します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="db6f5-214">場合、ページから`http://contoso.com`への接続は、`http://fabrikam.com/signalr`ドメイン間の接続されています。</span><span class="sxs-lookup"><span data-stu-id="db6f5-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="db6f5-215">セキュリティ上の理由から、ドメイン間の接続が既定で無効になります。</span><span class="sxs-lookup"><span data-stu-id="db6f5-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="db6f5-216">ドメイン間の接続を確立するには、サーバーで、ドメイン間の接続が有効であるかどうかを確認し、接続オブジェクトを作成するときに、接続 URL を指定します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="db6f5-217">SignalR はテクノロジを使用して、適切なドメイン間の接続など[JSONP](http://en.wikipedia.org/wiki/JSONP)または[CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing)します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="db6f5-218">サーバーで、ドメイン間の接続を有効に呼び出すときにそのオプションを選択して、`MapHubs`メソッド。</span><span class="sxs-lookup"><span data-stu-id="db6f5-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="db6f5-219">クライアントでは、(せずに、生成されたプロキシ) 接続オブジェクトを作成するとき、または (生成されたプロキシ) を使用して start メソッドを呼び出す前に、URL を指定します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="db6f5-220">**(生成されたプロキシ) を使用してドメイン間の接続を指定するクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="db6f5-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="db6f5-221">**ドメイン間の接続 (せずに、生成されたプロキシ) を指定するクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="db6f5-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="db6f5-222">使用すると、`$.hubConnection`コンス トラクターを含める必要はありません`signalr`URL に自動的に追加されるため (指定しない限り`useDefaultUrl`として`false`)。</span><span class="sxs-lookup"><span data-stu-id="db6f5-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="db6f5-223">複数の接続をさまざまなエンドポイントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="db6f5-224">設定しない`jQuery.support.cors`コードで true に設定します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![JQuery.support.cors を true に設定しないでください。](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="db6f5-226">SignalR では、JSONP または CORS の使用を処理します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="db6f5-227">設定`jQuery.support.cors`に SignalR、ブラウザーは、CORS をサポートするいると仮定する原因になるので true JSONP を無効にします。</span><span class="sxs-lookup"><span data-stu-id="db6f5-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="db6f5-228">に localhost の URL に接続するときに Internet Explorer 10 は見なされませんが、ドメイン間の接続を、アプリケーションは、ローカルで IE 10 サーバー上のドメイン間の接続を有効にしていない場合でも、。</span><span class="sxs-lookup"><span data-stu-id="db6f5-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="db6f5-229">Internet Explorer 9 を使用したドメイン間の接続方法の詳細については、次を参照してください。[この StackOverflow スレッド](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work)します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="db6f5-230">Chrome を使用したドメイン間の接続方法の詳細については、次を参照してください。[この StackOverflow スレッド](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome)します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="db6f5-231">サンプル コードは、既定値を使用して"/signalr"SignalR サービスに接続するための URL。</span><span class="sxs-lookup"><span data-stu-id="db6f5-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="db6f5-232">別の基本 URL を指定する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバー -/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="db6f5-233">接続を構成する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-233">How to configure the connection</span></span>

<span data-ttu-id="db6f5-234">接続を確立する前に、クエリ文字列パラメーターを指定またはトランスポート メソッドを指定できます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="db6f5-235">クエリ文字列パラメーターを指定する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-235">How to specify query string parameters</span></span>

<span data-ttu-id="db6f5-236">クライアントが接続するときに、サーバーにデータを送信する場合は、接続オブジェクトをクエリ文字列パラメーターを追加できます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="db6f5-237">次の例では、クライアント コードで、クエリ文字列パラメーターを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="db6f5-238">**(生成されたプロキシ) を使用して start メソッドを呼び出す前に、クエリ文字列値を設定します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="db6f5-239">**(生成されたプロキシ) なしの start メソッドを呼び出す前に、クエリ文字列値を設定します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="db6f5-240">次の例では、サーバー コードでクエリ文字列パラメーターを読み取る方法を示します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="db6f5-241">トランスポート メソッドを指定する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-241">How to specify the transport method</span></span>

<span data-ttu-id="db6f5-242">接続するプロセスの一環として、SignalR クライアントは、通常サーバーとクライアントの両方でサポートされている最適なトランスポートを決定する、サーバーとネゴシエートします。</span><span class="sxs-lookup"><span data-stu-id="db6f5-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="db6f5-243">呼び出すときに、トランスポート メソッドを指定することによってこのネゴシエーション プロセスをバイパスするには使用するトランスポートを既に知っている場合、`start`メソッド。</span><span class="sxs-lookup"><span data-stu-id="db6f5-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="db6f5-244">**クライアント コード (生成されたプロキシ) を使用したトランスポート メソッドを指定します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="db6f5-245">**クライアント コード (せずに、生成されたプロキシ) トランスポート メソッドを指定します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="db6f5-246">代わりに、SignalR を試したい順序で複数のトランスポート メソッドを指定できます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="db6f5-247">**クライアント コード (生成されたプロキシ) を使用したカスタム トランスポート フォールバック スキームを指定します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="db6f5-248">**カスタム トランスポート フォールバック スキーム (なし、生成されたプロキシ) を指定するクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="db6f5-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="db6f5-249">トランスポート メソッドを指定するため、次の値を使用できます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="db6f5-250">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="db6f5-250">"webSockets"</span></span>
- <span data-ttu-id="db6f5-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="db6f5-251">"foreverFrame"</span></span>
- <span data-ttu-id="db6f5-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="db6f5-252">"serverSentEvents"</span></span>
- <span data-ttu-id="db6f5-253">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="db6f5-253">"longPolling"</span></span>

<span data-ttu-id="db6f5-254">次の例では、どのトランスポート メソッドは、接続で使用されていることを見つける方法を示します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="db6f5-255">**クライアント コード (生成されたプロキシ) との接続で使用されるトランスポート メソッドを表示します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="db6f5-256">**クライアント コード (せずに、生成されたプロキシ) の接続で使用されるトランスポート メソッドを表示します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="db6f5-257">サーバー コードでの転送方法を確認する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーのコンテキスト プロパティからのクライアントに関する情報を取得する方法](../guide-to-the-api/hubs-api-guide-server.md#contextproperty)します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="db6f5-258">トランスポートとフォールバックの詳細については、次を参照してください。 [SignalR のトランスポートとフォールバックの概要](../getting-started/introduction-to-signalr.md#transports)します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="db6f5-259">ハブ クラスのプロキシを取得する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="db6f5-260">各接続オブジェクトを作成するには、1 つまたは複数のハブ クラスを含む SignalR サービスへの接続に関する情報がカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="db6f5-261">ハブ クラスを通信するには、(この場合、生成されたプロキシを使用していない) 自分で作成または生成するプロキシ オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="db6f5-262">クライアントでは、プロキシの名前は、ハブ クラス名の camel 形式のバージョンです。</span><span class="sxs-lookup"><span data-stu-id="db6f5-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="db6f5-263">SignalR では、JavaScript コードは、JavaScript の規則に従うことができるように、この変更が自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="db6f5-264">**サーバー上のハブ クラス**</span><span class="sxs-lookup"><span data-stu-id="db6f5-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="db6f5-265">**ハブの生成されたクライアント プロキシへの参照を取得します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="db6f5-266">**生成されたプロキシ) を含めず、ハブ クラス用のクライアント プロキシを作成します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="db6f5-267">使用してハブ クラスを修飾する場合、`HubName`属性、ケースを変更することがなく正確な名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="db6f5-268">**HubName 属性を持つサーバー上のハブ クラス**</span><span class="sxs-lookup"><span data-stu-id="db6f5-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="db6f5-269">**ハブの生成されたクライアント プロキシへの参照を取得します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="db6f5-270">**生成されたプロキシ) を含めず、ハブ クラス用のクライアント プロキシを作成します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="db6f5-271">サーバーが呼び出すことができるクライアントでメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="db6f5-272">サーバーはハブから呼び出すことができるメソッドを定義するを使用してハブ プロキシにイベント ハンドラーを追加します。、 `client` 、生成されたプロキシ、または呼び出しのプロパティ、`on`メソッド、生成されたプロキシを使用していない場合。</span><span class="sxs-lookup"><span data-stu-id="db6f5-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="db6f5-273">パラメーターには、複雑なオブジェクトを指定できます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="db6f5-274">呼び出す前に、イベント ハンドラーを追加、`start`メソッドは、接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="db6f5-275">(呼び出した後のイベント ハンドラーを追加する場合、`start`メソッドの注を参照してください[接続を確立する方法](#establishconnection)この前のドキュメントし、生成されたプロキシを使用せず、メソッドを定義するための構文を使用します)。</span><span class="sxs-lookup"><span data-stu-id="db6f5-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="db6f5-276">大文字と小文字が一致するメソッドの名前です。</span><span class="sxs-lookup"><span data-stu-id="db6f5-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="db6f5-277">たとえば、`Clients.All.addContosoChatMessageToPage`サーバーで実行`AddContosoChatMessageToPage`、 `addContosoChatMessageToPage`、または`addcontosochatmessagetopage`クライアント。</span><span class="sxs-lookup"><span data-stu-id="db6f5-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="db6f5-278">**(生成されたプロキシ) を使用するクライアントでメソッドを定義します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="db6f5-279">**(生成されたプロキシ) を使用するクライアントでメソッドを定義する別の方法**</span><span class="sxs-lookup"><span data-stu-id="db6f5-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="db6f5-280">**生成されたプロキシなし (start メソッドを呼び出した後に追加するときに) クライアントでメソッドを定義します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="db6f5-281">**サーバー コードをクライアント メソッドを呼び出す**</span><span class="sxs-lookup"><span data-stu-id="db6f5-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="db6f5-282">次の例には、メソッド パラメーターとして、複雑なオブジェクトが含まれます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="db6f5-283">**(生成されたプロキシ) を使用した複雑なオブジェクトを取得するクライアントでメソッドを定義します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="db6f5-284">**(なし、生成されたプロキシ) の複雑なオブジェクトを取得するクライアントでメソッドを定義します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="db6f5-285">**複雑なオブジェクトを定義するサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="db6f5-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="db6f5-286">**サーバー コードを複雑なオブジェクトを使用して、クライアント メソッドを呼び出す**</span><span class="sxs-lookup"><span data-stu-id="db6f5-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="db6f5-287">クライアントからサーバーのメソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-287">How to call server methods from the client</span></span>

<span data-ttu-id="db6f5-288">クライアントからサーバー メソッドを呼び出しを使用して、 `server` 、生成されたプロキシのプロパティまたは`invoke`生成されたプロキシを使用していない場合は、ハブ プロキシのメソッド。</span><span class="sxs-lookup"><span data-stu-id="db6f5-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="db6f5-289">戻り値またはパラメーターには、複雑なオブジェクトを指定できます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="db6f5-290">ハブのメソッド名の camel 形式でバージョンを渡します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="db6f5-291">SignalR では、JavaScript コードは、JavaScript の規則に従うことができるように、この変更が自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="db6f5-292">次の例では、戻り値を持たないサーバー メソッドを呼び出す方法と戻り値がサーバー メソッドを呼び出す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="db6f5-293">**HubMethodName 属性を持たないサーバー メソッド**</span><span class="sxs-lookup"><span data-stu-id="db6f5-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="db6f5-294">**パラメーターに渡される複雑なオブジェクトを定義するサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="db6f5-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="db6f5-295">**(生成されたプロキシ) を使用して、サーバーのメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="db6f5-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="db6f5-296">**(なし、生成されたプロキシ) サーバーのメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="db6f5-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="db6f5-297">ハブ メソッドを修飾する場合、`HubMethodName`属性、ケースを変更することがなく、その名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="db6f5-298">**サーバー メソッド**HubMethodName 属性を持つ</span><span class="sxs-lookup"><span data-stu-id="db6f5-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="db6f5-299">**(生成されたプロキシ) を使用して、サーバーのメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="db6f5-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="db6f5-300">**(なし、生成されたプロキシ) サーバーのメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="db6f5-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="db6f5-301">上記の例では、戻り値がサーバー メソッドを呼び出す方法を示しません。</span><span class="sxs-lookup"><span data-stu-id="db6f5-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="db6f5-302">次の例では、戻り値を持つサーバー メソッドを呼び出す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="db6f5-303">**メソッドの戻り値を持つサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="db6f5-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="db6f5-304">**使用される Stock クラス、** 戻り値</span><span class="sxs-lookup"><span data-stu-id="db6f5-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="db6f5-305">**(生成されたプロキシ) を使用して、サーバーのメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="db6f5-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="db6f5-306">**(なし、生成されたプロキシ) サーバーのメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="db6f5-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="db6f5-307">接続の有効期間イベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="db6f5-308">SignalR は、次の接続に処理できる有効期間イベントを提供します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="db6f5-309">`starting`: すべてのデータが、接続経由で送信される前に発生します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="db6f5-310">`received`: 接続でのデータが受信したときに発生します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="db6f5-311">受信したデータを提供します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-311">Provides the received data.</span></span>
- <span data-ttu-id="db6f5-312">`connectionSlow`: クライアントが低速または削除が頻繁に接続を検出したときに発生します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="db6f5-313">`reconnecting`: 基になるトランスポートの再接続を開始するときに発生します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="db6f5-314">`reconnected`: 基になるトランスポートが再接続されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="db6f5-315">`stateChanged`: 接続状態が変更されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="db6f5-316">以前の状態と新しい状態 (接続、接続、再接続、または切断) を提供します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="db6f5-317">`disconnected`: 接続が切断されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="db6f5-318">たとえば、次のように顕著な遅延を引き起こす可能性のある接続の問題が発生する警告メッセージを表示する場合、処理、`connectionSlow`イベント。</span><span class="sxs-lookup"><span data-stu-id="db6f5-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="db6f5-319">**(生成されたプロキシ) を使用した connectionSlow イベントを処理します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="db6f5-320">**(なし、生成されたプロキシ) connectionSlow イベントを処理します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="db6f5-321">詳細については、次を参照してください。 [SignalR の接続の有効期間イベントの処理と理解](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="db6f5-322">エラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-322">How to handle errors</span></span>

<span data-ttu-id="db6f5-323">SignalR JavaScript クライアントは、提供、`error`イベントのハンドラーを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="db6f5-324">サーバー メソッドの呼び出しに起因するエラー ハンドラーを追加するのに fail メソッドを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="db6f5-325">場合は、サーバー上の詳細なエラー メッセージを明示的に有効にしない、SignalR が返すエラーが発生した例外オブジェクトには、エラーに関する最小限の情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="db6f5-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="db6f5-326">呼び出しなど`newContosoChatMessage`失敗した場合、エラー オブジェクトにエラー メッセージが含まれています"`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`"送信の詳細なエラー メッセージを有効にする場合は、セキュリティ上の理由の詳細なエラー メッセージを運用環境でクライアントには推奨されませんトラブルシューティングのため、サーバーで次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="db6f5-327">次の例では、エラー イベントのハンドラーを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="db6f5-328">**(生成されたプロキシ) を使用して、エラー ハンドラーを追加します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="db6f5-329">**生成されたプロキシ) (なし、エラー ハンドラーを追加します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="db6f5-330">次の例では、メソッドの呼び出しからエラーを処理する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="db6f5-331">**(生成されたプロキシ) を使用したメソッドの呼び出しからエラーを処理します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="db6f5-332">**(なし、生成されたプロキシ) メソッドの呼び出しからエラーを処理します。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="db6f5-333">メソッドの呼び出しに失敗した場合、`error`イベントが発生してもためで自分のコード、`error`メソッドのハンドラーと、`.fail`メソッドのコールバックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="db6f5-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="db6f5-334">クライアント側のログ記録を有効にする方法</span><span class="sxs-lookup"><span data-stu-id="db6f5-334">How to enable client-side logging</span></span>

<span data-ttu-id="db6f5-335">接続でクライアント側のログ記録を有効にするには設定、`logging`を呼び出す前に、接続オブジェクトのプロパティ、`start`メソッドは、接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="db6f5-336">**(生成されたプロキシ) を使用してログ記録を有効にします。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="db6f5-337">**(なし、生成されたプロキシ) のログ記録を有効にします。**</span><span class="sxs-lookup"><span data-stu-id="db6f5-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="db6f5-338">ログを表示するには、ブラウザーの開発者ツールを開きコンソール タブに移動します。これを行う方法を示すショット画面および詳細な手順を表示するチュートリアルについては、次を参照してください。[ログ記録を有効にする - ASP.NET Signalr によるサーバー ブロードキャスト](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="db6f5-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
