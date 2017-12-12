---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: "ASP.NET SignalR ハブ API ガイド - JavaScript クライアント |Microsoft ドキュメント"
author: pfletcher
description: "このドキュメントでは、SignalR ブラウザーや Windows ストア (WinJS) applicat など、JavaScript クライアントで、バージョン 2 のハブ API を使用して紹介しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 65e369a393a8c5d2d1bba11b5c71347df8f9c69d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="0bbfe-103">ASP.NET SignalR ハブ API ガイド - JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="0bbfe-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="0bbfe-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0bbfe-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="0bbfe-105">このドキュメントでは、SignalR ブラウザーや Windows ストア (WinJS) アプリケーションなど、JavaScript クライアントで、バージョン 2 のハブ API を使用してに紹介します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-105">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="0bbfe-106">SignalR ハブ API では、クライアントを接続するようにサーバーおよびサーバーのクライアントからリモート プロシージャ コール (Rpc) を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="0bbfe-107">サーバー コードで、クライアントから呼び出すことができるメソッドを定義して、クライアント上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="0bbfe-108">クライアント コードで、サーバーから呼び出すことができるメソッドを定義して、サーバー上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="0bbfe-109">すべてのクライアントからサーバーへの組み込みの SignalR によって行われます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="0bbfe-110">SignalR では、永続的な接続と呼ばれる、下位の API も提供します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="0bbfe-111">SignalR、ハブ、および永続的な接続の概要については、次を参照してください。 [SignalR の概要](../getting-started/introduction-to-signalr.md)です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="0bbfe-112">このトピックで使用されているソフトウェア バージョン</span><span class="sxs-lookup"><span data-stu-id="0bbfe-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="0bbfe-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0bbfe-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="0bbfe-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="0bbfe-114">.NET 4.5</span></span>
> - <span data-ttu-id="0bbfe-115">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="0bbfe-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="0bbfe-116">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="0bbfe-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="0bbfe-117">SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="0bbfe-118">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="0bbfe-118">Questions and comments</span></span>
> 
> <span data-ttu-id="0bbfe-119">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="0bbfe-120">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="0bbfe-121">概要</span><span class="sxs-lookup"><span data-stu-id="0bbfe-121">Overview</span></span>

<span data-ttu-id="0bbfe-122">このドキュメントは、次のトピックに分かれています。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="0bbfe-123">生成されたプロキシとそれが何をします。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-123">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="0bbfe-124">生成されたプロキシを使用する場合</span><span class="sxs-lookup"><span data-stu-id="0bbfe-124">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="0bbfe-125">クライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="0bbfe-125">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="0bbfe-126">動的に生成されたプロキシを参照する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-126">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="0bbfe-127">生成されたプロキシの SignalR の物理ファイルを作成する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-127">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="0bbfe-128">接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-128">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="0bbfe-129">$connection.hub は同じオブジェクトのその $.hubConnection() の作成。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-129">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="0bbfe-130">Start メソッドの非同期実行</span><span class="sxs-lookup"><span data-stu-id="0bbfe-130">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="0bbfe-131">ドメイン間の接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-131">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="0bbfe-132">接続を構成する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-132">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="0bbfe-133">クエリ文字列パラメーターを指定する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-133">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="0bbfe-134">転送方法を指定する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-134">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="0bbfe-135">ハブ クラスのプロキシを取得する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-135">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="0bbfe-136">サーバーが呼び出すことができるクライアント上のメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-136">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="0bbfe-137">クライアントからサーバーのメソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="0bbfe-138">接続の有効期間イベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="0bbfe-139">エラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="0bbfe-140">クライアント側のログ記録を有効にする方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-140">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="0bbfe-141">サーバーまたは .NET のクライアントをプログラミングする方法に関するドキュメントについては、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-141">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="0bbfe-142">SignalR ハブ API ガイド - サーバー</span><span class="sxs-lookup"><span data-stu-id="0bbfe-142">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="0bbfe-143">SignalR ハブ API ガイド - .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="0bbfe-143">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="0bbfe-144">SignalR の 2 サーバー コンポーネントを .NET 4.5 でできるは、(.NET 4.0 の SignalR 2 用の .NET クライアントがある) 場合だけです。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-144">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="0bbfe-145">生成されたプロキシとそれが何をします。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-145">The generated proxy and what it does for you</span></span>

<span data-ttu-id="0bbfe-146">SignalR で生成されるプロキシの有無は、SignalR サービスと通信するために、JavaScript クライアントをプログラムすることができます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-146">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="0bbfe-147">プロキシが動作するは、接続する場合、サーバーを呼び出すと、書き込みメソッドを使用して、サーバー上のメソッドを呼び出すコードの構文を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-147">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="0bbfe-148">サーバー メソッドを呼び出すコードを記述するときに生成されたプロキシを使用すると、ローカル関数を実行した場合と同様の検索構文を使用する: 書き込めること`serverMethod(arg1, arg2)`の代わりに`invoke('serverMethod', arg1, arg2)`です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-148">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="0bbfe-149">生成されたプロキシ構文では、サーバーのメソッド名を入力し間違えた場合即時および判読クライアント側のエラーも可能です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-149">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="0bbfe-150">および server メソッドを呼び出すコードを記述するため、IntelliSense のサポートも取得できます、プロキシを定義するファイルを手動で作成する場合。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-150">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="0bbfe-151">たとえば、サーバーで次の Hub クラスがあるとします。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-151">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="0bbfe-152">呼び出すように JavaScript コードなります次のコード例を表示する、`NewContosoChatMessage`メソッドをサーバーとの呼び出しを受信、`addContosoChatMessageToPage`サーバーからのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-152">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="0bbfe-153">**生成されたプロキシと**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-153">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="0bbfe-154">**生成されたプロキシなし**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-154">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="0bbfe-155">生成されたプロキシを使用する場合</span><span class="sxs-lookup"><span data-stu-id="0bbfe-155">When to use the generated proxy</span></span>

<span data-ttu-id="0bbfe-156">サーバーを呼び出すクライアント メソッドの複数のイベント ハンドラーを登録する場合は、生成されたプロキシを使うことはできません。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-156">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="0bbfe-157">それ以外の場合、生成されたプロキシを使用することもできますか、コーディングの基本設定に基づいていません。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-157">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="0bbfe-158">これを使用しないようにする場合は、「signalr/ハブ」URL で参照することも、`script`クライアント コード内の要素。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-158">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="0bbfe-159">クライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="0bbfe-159">Client setup</span></span>

<span data-ttu-id="0bbfe-160">JavaScript クライアントには、jQuery、および SignalR core の JavaScript ファイルへの参照が必要です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-160">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="0bbfe-161">JQuery のバージョンは、1.6.4 または 1.7.2、1.8.2、1.9.1 など、主要な以降のバージョンにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-161">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="0bbfe-162">生成されたプロキシを使用する場合は、SignalR 生成されたプロキシの JavaScript ファイルへの参照も必要です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-162">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="0bbfe-163">次の例では、生成されたプロキシを使用する HTML ページに表示される、参照を示します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-163">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="0bbfe-164">この順序でこれらの参照を含める必要があります: jQuery 最初に、その後、SignalR core および SignalR プロキシ最後です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-164">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="0bbfe-165">動的に生成されたプロキシを参照する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-165">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="0bbfe-166">前の例では、SignalR 生成されたプロキシへの参照は、物理ファイルが、動的に生成された JavaScript コードがします。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-166">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="0bbfe-167">SignalR では、JavaScript コードを実行時にプロキシを作成し、「/signalr ハブ」URL への応答でクライアントに機能します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-167">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="0bbfe-168">サーバー上では、SignalR 接続に対して別の基本 URL を指定したかどうか、`MapSignalR`メソッドを動的に生成されるプロキシ ファイルの URL は、カスタムの URL に"/ハブ"が追加されます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-168">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="0bbfe-169">Windows 8 (Windows ストア) の JavaScript クライアントは、動的に生成されたものではなく、物理的なプロキシ ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-169">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="0bbfe-170">詳細については、次を参照してください。 [SignalR の物理ファイルを作成する方法については、プロキシを生成](#manualproxy)このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-170">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="0bbfe-171">ASP.NET MVC 4 または 5 の Razor ビューでは、チルダを使用して、アプリケーション ルートに、プロキシ ファイル参照を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-171">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="0bbfe-172">SignalR の MVC 5 の使用に関する詳細については、次を参照してください。 [SignalR と MVC 5 の概要](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-172">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="0bbfe-173">ASP.NET MVC 3 Razor ビューで使用して`Url.Content`プロキシ ファイル参照。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-173">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="0bbfe-174">ASP.NET Web フォーム アプリケーションで使用`ResolveClientUrl`プロキシの参照または (チルダ以降)、アプリケーション ルートの相対パスを使用して ScriptManager 経由で登録のため。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-174">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="0bbfe-175">一般的な規則としては、CSS や JavaScript ファイルに使用する「/signalr ハブ」URL を指定するため、同じメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-175">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="0bbfe-176">URL を指定するには、チルダを使用せず、一部のシナリオで、アプリケーションが正しく動作 IIS Express を使用して Visual Studio でのテストが、完全な IIS に展開するときは、404 エラーで失敗します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-176">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="0bbfe-177">詳細については、次を参照してください。**ルート レベルのリソースへの参照の解決**で[ASP.NET Web プロジェクト用の Visual Studio で Web サーバー](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx) MSDN サイトです。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-177">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="0bbfe-178">デバッグ モードで Visual Studio 2013 で web プロジェクトを実行すると、お使いのブラウザーとして Internet Explorer を使用する場合は、プロキシ ファイルを確認できます**ソリューション エクスプ ローラー** **スクリプト ドキュメント**のように、次の図。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-178">When you run a web project in Visual Studio 2013 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![ソリューション エクスプ ローラーでの JavaScript 生成されるプロキシ ファイル](hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="0bbfe-180">ファイルの内容を表示するをダブルクリック**ハブ**です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-180">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="0bbfe-181">Visual Studio 2012 または 2013 と Internet Explorer を使用しない場合、またはデバッグ モードになっていない場合は、「/signalR ハブ」URL を参照して、ファイルの内容を取得することもできます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-181">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="0bbfe-182">サイトが実行されている場合など`http://localhost:56699`に進み、`http://localhost:56699/SignalR/hubs`ブラウザーにします。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-182">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="0bbfe-183">生成されたプロキシの SignalR の物理ファイルを作成する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-183">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="0bbfe-184">動的に生成されたプロキシする代わりには、プロキシ コードを持つ物理ファイルを作成し、そのファイルを参照することができます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-184">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="0bbfe-185">そのためにキャッシュまたはバンドル化の動作を制御または server メソッドを呼び出すには、コーディングするときに IntelliSense を取得することができます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-185">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="0bbfe-186">プロキシ ファイルを作成するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-186">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="0bbfe-187">インストール、 [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-187">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="0bbfe-188">コマンド プロンプトを開きを参照、*ツール*SignalR.exe ファイルを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-188">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="0bbfe-189">ツール フォルダーは、次の場所には。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-189">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="0bbfe-190">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-190">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="0bbfe-191">パス、 *.dll*は、通常、 *bin*プロジェクト フォルダー内のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-191">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="0bbfe-192">このコマンドは、という名前のファイルを作成*server.js*と同じフォルダーに*signalr.exe*です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-192">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="0bbfe-193">Put、 *server.js*ファイル、プロジェクト内の適切なフォルダーをアプリケーションの適切な名前を変更および「signalr/ハブ」参照の代わりにへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-193">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="0bbfe-194">接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-194">How to establish a connection</span></span>

<span data-ttu-id="0bbfe-195">接続を確立することができます、前に、接続オブジェクトを作成、プロキシを作成し、サーバーから呼び出すことができるメソッドのイベント ハンドラーを登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-195">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="0bbfe-196">プロキシとイベント ハンドラーが設定されているときに呼び出すことによって、接続の確立、`start`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-196">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="0bbfe-197">生成されたプロキシを使用している場合は、独自のコードで生成されたプロキシ コードは、接続オブジェクトを作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-197">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="0bbfe-198">**(生成されたプロキシ) との接続を確立します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-198">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="0bbfe-199">**(なし、生成されたプロキシ) 接続を確立します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-199">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="0bbfe-200">既定値を使用するサンプル コード"/signalr"SignalR サービスへの接続への URL。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-200">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="0bbfe-201">別の基本 URL を指定する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバー -/signalr URL](hubs-api-guide-server.md#signalrurl)です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-201">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="0bbfe-202">既定では、ハブの場所は、現在のサーバーです。別のサーバーに接続する場合は、呼び出す前に URL を指定、`start`メソッドを次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-202">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="0bbfe-203">呼び出しの前にイベント ハンドラーを登録する通常、`start`接続を確立する方法です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-203">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="0bbfe-204">接続の確立後にいくつかのイベント ハンドラーを登録すると、ことを行うことができますが、少なくとも 1 つの呼び出しの前に、イベント ハンドラーを登録する必要があります、`start`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-204">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="0bbfe-205">この理由の 1 つがあります多くハブ アプリケーションをトリガーする必要はありません、`OnConnected`場合のみにこれらのいずれかを使用するすべてのハブのイベントです。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-205">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="0bbfe-206">SignalR をトリガーするように指示新機能が、接続が確立されると、ハブのプロキシのクライアント メソッドの存在していれば、`OnConnected`イベント。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-206">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="0bbfe-207">呼び出しの前に、イベント ハンドラーを登録しない場合、`start`メソッド、ことができます、ハブ、ハブのメソッドを呼び出す`OnConnected`メソッドを呼び出すしないし、サーバーからクライアントのメソッドは呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-207">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="0bbfe-208">$connection.hub は同じオブジェクトのその $.hubConnection() の作成。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-208">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="0bbfe-209">わかるようにの例については、生成されたプロキシを使用すると、`$.connection.hub`接続オブジェクトを参照します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-209">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="0bbfe-210">これは、同じオブジェクトを呼び出して取得するを`$.hubConnection()`生成されたプロキシを使用していない場合。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-210">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="0bbfe-211">生成されたプロキシ コードでは、次のステートメントを実行することによって自動的に接続を作成します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-211">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![生成されたプロキシ ファイルの接続を作成します。](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="0bbfe-213">生成されたプロキシを使用しているときに操作を実行すると`$.connection.hub`行えること、接続オブジェクトで生成されたプロキシを使用していないときにします。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-213">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="0bbfe-214">Start メソッドの非同期実行</span><span class="sxs-lookup"><span data-stu-id="0bbfe-214">Asynchronous execution of the start method</span></span>

<span data-ttu-id="0bbfe-215">`start`メソッドが非同期的に実行します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-215">The `start` method executes asynchronously.</span></span> <span data-ttu-id="0bbfe-216">返します、 [jQuery 延期オブジェクト](http://api.jquery.com/category/deferred-object/)などのメソッドを呼び出すことで、コールバック関数を追加できることを意味する`pipe`、 `done`、および`fail`です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-216">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="0bbfe-217">接続が確立された後に実行するコードがあれば、server メソッドを呼び出しなどには、コールバック関数にそのコードを配置またはコールバック関数から呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-217">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="0bbfe-218">`.done`接続が確立されると、内にあるコードを後後に、コールバック メソッドは実行される、`OnConnected`サーバー上のイベント ハンドラー メソッドの実行が終了します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-218">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="0bbfe-219">後のコードの次の行として前の例の「接続されているようになりました」ステートメントを配置した場合は、`start`メソッドの呼び出し (ではなく、`.done`コールバック) では、`console.log`行は、接続が確立される前に、次に示すように、実行されます例:</span><span class="sxs-lookup"><span data-stu-id="0bbfe-219">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![接続が確立された後に実行するコードを記述する方法を誤った](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="0bbfe-221">ドメイン間の接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-221">How to establish a cross-domain connection</span></span>

<span data-ttu-id="0bbfe-222">通常、ブラウザーからページを読み込む場合`http://contoso.com`、SignalR 接続は、同じドメインに`http://contoso.com/signalr`です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-222">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="0bbfe-223">場合、ページから`http://contoso.com`への接続は、`http://fabrikam.com/signalr`ドメイン間の接続されています。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-223">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="0bbfe-224">セキュリティ上の理由から、ドメイン間の接続は、既定で無効にします。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-224">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="0bbfe-225">SignalR で 1.x では、クロス ドメイン要求が単一 EnableCrossDomain フラグによって制御されます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-225">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="0bbfe-226">このフラグは、両方の JSONP および CORS 要求を制御します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-226">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="0bbfe-227">すべての CORS のサポートをより柔軟に行う、SignalR のサーバー コンポーネントから削除されました (JavaScript クライアントも CORS 通常、ブラウザーがサポートすることが検出された場合) を使用し、新しい OWIN ミドルウェアをこれらのシナリオをサポートするために使用されました。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-227">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="0bbfe-228">設定を明示的に有効にする必要があります JSONP が必要な場合、クライアントで (古いブラウザーでは、クロス ドメイン要求をサポート)、`EnableJSONP`上、`HubConfiguration`オブジェクトを`true`次のように、します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-228">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="0bbfe-229">CORS より安全であるために、既定では、JSONP が無効です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-229">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="0bbfe-230">**Microsoft.Owin.Cors をプロジェクトに追加する:**このライブラリをインストールするには、パッケージ マネージャー コンソールで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-230">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="0bbfe-231">このコマンドは、2.1.0 を追加、プロジェクトにパッケージのバージョン。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-231">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="0bbfe-232">通話 UseCors</span><span class="sxs-lookup"><span data-stu-id="0bbfe-232">Calling UseCors</span></span>

 <span data-ttu-id="0bbfe-233">次のコード スニペットでは、SignalR 2 で、ドメイン間の接続を実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-233">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span> 

<span data-ttu-id="0bbfe-234">**SignalR 2 でクロス ドメイン要求を実装します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-234">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="0bbfe-235">次のコードでは、SignalR 2 プロジェクトで CORS または JSONP を有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-235">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="0bbfe-236">このコード サンプルでは使用`Map`と`RunSignalR`の代わりに`MapSignalR`CORS のサポートを必要とする SignalR 要求に対してのみ、CORS ミドルウェアが実行されるように、(で指定されたパスにすべてのトラフィックではなく`MapSignalR`)。マップは、アプリケーション全体に対してではなく、特定の URL プレフィックスを実行する必要があるその他のすべてのミドルウェアにも使用できます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-236">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - <span data-ttu-id="0bbfe-237">設定されていない`jQuery.support.cors`コードで true に設定します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-237">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![JQuery.support.cors を true に設定されていません。](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="0bbfe-239">SignalR では、CORS の使用を処理します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-239">SignalR handles the use of CORS.</span></span> <span data-ttu-id="0bbfe-240">設定`jQuery.support.cors`SignalR、ブラウザーは、CORS をサポートするいると仮定する原因になるので true JSONP を無効にします。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-240">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="0bbfe-241">Localhost の URL に接続するときに Internet Explorer 10 は見なされません、ドメイン間の接続、アプリケーションの動作をローカルで IE 10 とサーバー上のドメイン間の接続を有効にしていない場合でも、します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-241">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="0bbfe-242">Internet Explorer 9 を使用したクロス ドメインの接続方法の詳細については、次を参照してください。[この StackOverflow スレッド](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work)です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-242">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="0bbfe-243">Chrome で、ドメイン間の接続を使用する方法については、次を参照してください。[この StackOverflow スレッド](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome)です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-243">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="0bbfe-244">既定値を使用するサンプル コード"/signalr"SignalR サービスへの接続への URL。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-244">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="0bbfe-245">別の基本 URL を指定する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバー -/signalr URL](hubs-api-guide-server.md#signalrurl)です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-245">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="0bbfe-246">接続を構成する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-246">How to configure the connection</span></span>

<span data-ttu-id="0bbfe-247">接続を確立する前にクエリ文字列パラメーターを指定したり、配送方法を指定できます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-247">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="0bbfe-248">クエリ文字列パラメーターを指定する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-248">How to specify query string parameters</span></span>

<span data-ttu-id="0bbfe-249">クライアントが接続するときに、サーバーにデータを送信する場合は、接続オブジェクトをクエリ文字列パラメーターを追加できます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-249">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="0bbfe-250">次の例では、クライアント コードでクエリ文字列パラメーターを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-250">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="0bbfe-251">**(生成されたプロキシ) の start メソッドを呼び出す前にクエリ文字列の値を設定します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-251">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="0bbfe-252">**(なし、生成されたプロキシ) start メソッドを呼び出す前にクエリ文字列の値を設定します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-252">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="0bbfe-253">次の例では、サーバー コードでクエリ文字列パラメーターを読み取る方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-253">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="0bbfe-254">転送方法を指定する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-254">How to specify the transport method</span></span>

<span data-ttu-id="0bbfe-255">接続するプロセスの一環として、SignalR クライアントは、通常サーバーとクライアントの両方でサポートされている最適なトランスポートを決定するサーバーとネゴシエートします。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-255">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="0bbfe-256">使用するトランスポートが既に把握している場合を呼び出すときに、トランスポート メソッドを指定することによってこのネゴシエーション プロセスをバイパスできます、`start`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-256">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="0bbfe-257">**クライアント コードで生成されたプロキシ) トランスポート メソッドを指定します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-257">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="0bbfe-258">**(なし、生成されたプロキシ) トランスポート メソッドを指定するクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-258">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="0bbfe-259">代わりに、しようとする SignalR 順序で複数のトランスポート メソッドを指定できます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-259">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="0bbfe-260">**クライアント コードで生成されたプロキシ) カスタム トランスポート フォールバック スキームを指定します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-260">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="0bbfe-261">**(なし、生成されたプロキシ) カスタム トランスポート フォールバック スキームを指定するクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-261">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="0bbfe-262">転送方法を指定するため、次の値を使用できます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-262">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="0bbfe-263">"Websocket"</span><span class="sxs-lookup"><span data-stu-id="0bbfe-263">"webSockets"</span></span>
- <span data-ttu-id="0bbfe-264">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="0bbfe-264">"foreverFrame"</span></span>
- <span data-ttu-id="0bbfe-265">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="0bbfe-265">"serverSentEvents"</span></span>
- <span data-ttu-id="0bbfe-266">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="0bbfe-266">"longPolling"</span></span>

<span data-ttu-id="0bbfe-267">次の例では、どのトランスポート メソッドは、接続で使用されていることを確認する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-267">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="0bbfe-268">**(生成されたプロキシ) との接続で使用されるトランスポート メソッドを表示するクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-268">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="0bbfe-269">**(なし、生成されたプロキシ) 接続で使用するトランスポート メソッドを表示するクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-269">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="0bbfe-270">サーバー コードでの転送方法を確認する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーのコンテキスト プロパティからのクライアントに関する情報を取得する方法](hubs-api-guide-server.md#contextproperty)です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-270">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="0bbfe-271">トランスポートとフォールバックの詳細については、次を参照してください。 [SignalR のトランスポートとフォールバックの概要](../getting-started/introduction-to-signalr.md#transports)です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-271">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="0bbfe-272">ハブ クラスのプロキシを取得する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-272">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="0bbfe-273">各接続オブジェクトを作成するには、1 つまたは複数の Hub クラスが含まれた SignalR サービスへの接続に関する情報がカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-273">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="0bbfe-274">ハブ クラスを通信するためにオブジェクトを使用するプロキシ (生成されたプロキシを使用していない) 場合自分で作成またはこれが生成されます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-274">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="0bbfe-275">クライアントでは、プロキシの名前は、ハブ クラス名の camel 形式のバージョンです。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-275">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="0bbfe-276">SignalR は、JavaScript コードが JavaScript の規則に従うことができるように、この変更を自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-276">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="0bbfe-277">**サーバーの hub クラス**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-277">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="0bbfe-278">**ハブの生成されたクライアント プロキシへの参照を取得します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-278">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="0bbfe-279">**(生成されたプロキシは) なしの Hub クラス用のクライアント プロキシを作成します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-279">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="0bbfe-280">使用してハブ クラスを装飾する場合、`HubName`属性は、ケースを変更することがなく正確な名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-280">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="0bbfe-281">**HubName 属性を持つサーバーの hub クラス**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-281">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="0bbfe-282">**ハブの生成されたクライアント プロキシへの参照を取得します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-282">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="0bbfe-283">**(生成されたプロキシは) なしの Hub クラス用のクライアント プロキシを作成します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-283">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="0bbfe-284">サーバーが呼び出すことができるクライアント上のメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-284">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="0bbfe-285">サーバーがハブから呼び出すことができるメソッドを定義するを使用してハブ プロキシにイベント ハンドラーを追加します。、 `client` 、生成されたプロキシ、または呼び出しのプロパティ、`on`メソッド、生成されたプロキシを使用していない場合。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-285">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="0bbfe-286">複雑なオブジェクトのパラメーターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-286">The parameters can be complex objects.</span></span>

<span data-ttu-id="0bbfe-287">呼び出す前に、イベント ハンドラーを追加、`start`接続を確立する方法です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-287">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="0bbfe-288">(を呼び出した後にイベント ハンドラーを追加する場合、`start`メソッドの注を参照[接続を確立する方法](#establishconnection)この前のドキュメント、および生成されたプロキシを使用せず、メソッドを定義するために示す構文を使用します)。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-288">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="0bbfe-289">メソッド名に一致する小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-289">Method name matching is case-insensitive.</span></span> <span data-ttu-id="0bbfe-290">たとえば、`Clients.All.addContosoChatMessageToPage`サーバーで実行`AddContosoChatMessageToPage`、 `addContosoChatMessageToPage`、または`addcontosochatmessagetopage`クライアントにします。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-290">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="0bbfe-291">**(生成されたプロキシ) を使用するクライアントでメソッドを定義します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-291">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="0bbfe-292">**別の (生成されたプロキシ) を使用するクライアントでメソッドを定義する方法**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-292">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="0bbfe-293">**生成されたプロキシなし (start メソッドを呼び出した後に追加するときに) クライアントでメソッドを定義します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-293">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="0bbfe-294">**クライアントのメソッドを呼び出すサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-294">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="0bbfe-295">次の例には、メソッド パラメーターとして複雑なオブジェクトが含まれます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-295">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="0bbfe-296">**メソッドを (生成されたプロキシ) を使用して複雑なオブジェクトを受け取るクライアントで定義します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-296">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="0bbfe-297">**メソッドは、複雑なオブジェクト (せずに、生成されたプロキシ) は、クライアントで定義します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-297">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="0bbfe-298">**複雑なオブジェクトを定義するサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-298">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="0bbfe-299">**複雑なオブジェクトを使用してクライアント メソッドを呼び出すサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-299">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="0bbfe-300">クライアントからサーバーのメソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-300">How to call server methods from the client</span></span>

<span data-ttu-id="0bbfe-301">クライアントからサーバー メソッドを呼び出すには使用、 `server` 、生成されたプロキシのプロパティまたは`invoke`生成されたプロキシを使用していない場合は、ハブ プロキシのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-301">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="0bbfe-302">戻り値またはパラメーターには、複雑なオブジェクトを指定できます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-302">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="0bbfe-303">ハブのメソッド名の camel 形式でバージョンを渡します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-303">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="0bbfe-304">SignalR は、JavaScript コードが JavaScript の規則に従うことができるように、この変更を自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-304">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="0bbfe-305">次の例では、戻り値を持たない server メソッドを呼び出す方法および戻り値が含まれている server メソッドを呼び出す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-305">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="0bbfe-306">**HubMethodName 属性を持たないサーバー メソッド**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-306">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="0bbfe-307">**パラメーターに渡される複雑なオブジェクトを定義するサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-307">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="0bbfe-308">**(生成されたプロキシ) を使用して server メソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-308">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="0bbfe-309">**(なし、生成されたプロキシ) server メソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-309">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="0bbfe-310">ハブを使用してメソッドを装飾する場合、`HubMethodName`属性、ケースを変更することがなく、その名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-310">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="0bbfe-311">**サーバー メソッド**HubMethodName 属性を持つ</span><span class="sxs-lookup"><span data-stu-id="0bbfe-311">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="0bbfe-312">**(生成されたプロキシ) を使用して server メソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-312">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="0bbfe-313">**(なし、生成されたプロキシ) server メソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-313">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="0bbfe-314">上記の例では、戻り値を持たない server メソッドを呼び出す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-314">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="0bbfe-315">次の例では、戻り値を持つサーバー メソッドを呼び出す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-315">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="0bbfe-316">**メソッドの戻り値を持つサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-316">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="0bbfe-317">**使用される Stock クラス、**戻り値</span><span class="sxs-lookup"><span data-stu-id="0bbfe-317">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="0bbfe-318">**(生成されたプロキシ) を使用して server メソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-318">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="0bbfe-319">**(なし、生成されたプロキシ) server メソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-319">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="0bbfe-320">接続の有効期間イベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-320">How to handle connection lifetime events</span></span>

<span data-ttu-id="0bbfe-321">SignalR では、有効期間イベントを処理することができますを次の接続を提供します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-321">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="0bbfe-322">`starting`: すべてのデータは、接続経由で送信する前に発生します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-322">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="0bbfe-323">`received`: 接続ですべてのデータが受信したときに発生します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-323">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="0bbfe-324">受信したデータを提供します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-324">Provides the received data.</span></span>
- <span data-ttu-id="0bbfe-325">`connectionSlow`: クライアントが低速または頻繁に削除の接続を検出したときに発生します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-325">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="0bbfe-326">`reconnecting`: 基になるトランスポートの再接続を開始するときに発生します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-326">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="0bbfe-327">`reconnected`: 基になるトランスポートが再接続されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-327">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="0bbfe-328">`stateChanged`: 接続の状態が変更されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-328">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="0bbfe-329">以前の状態と新しい状態 (接続、接続、再接続、または切断) を提供します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-329">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="0bbfe-330">`disconnected`: 接続を切断したときに発生します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-330">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="0bbfe-331">たとえば、顕著な遅延を引き起こす可能性のある接続の問題がある場合に警告メッセージを表示する場合、処理、`connectionSlow`イベント。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-331">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="0bbfe-332">**ハンドル (生成されたプロキシ) を持つ connectionSlow イベント**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-332">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="0bbfe-333">**イベントを処理します connectionSlow (、生成されたプロキシなし)**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-333">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="0bbfe-334">詳細については、次を参照してください。 [SignalR で接続の有効期間イベントの処理と理解](handling-connection-lifetime-events.md)です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-334">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="0bbfe-335">エラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-335">How to handle errors</span></span>

<span data-ttu-id="0bbfe-336">SignalR JavaScript クライアントは、提供、`error`イベントのハンドラーを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-336">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="0bbfe-337">Fail メソッドを使用して、サーバーのメソッドの呼び出しに起因するエラーのハンドラーを追加することができますも。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-337">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="0bbfe-338">場合は、サーバー上の詳細なエラー メッセージを明示的に有効にしない、エラーが発生した SignalR を返す例外オブジェクトには、エラーについての最小限の情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-338">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="0bbfe-339">たとえばへの呼び出し`newContosoChatMessage`失敗した場合、エラー オブジェクト内のエラー メッセージが含まれています"`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`"送信の詳細なエラー メッセージを有効にする場合は、セキュリティ上の理由の詳細なエラー メッセージを実稼働環境でクライアントには推奨されませんトラブルシューティングの目的では、サーバーで次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-339">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="0bbfe-340">次の例では、エラー イベントのハンドラーを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-340">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="0bbfe-341">**(生成されたプロキシ) を使用して、エラー ハンドラーを追加します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-341">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="0bbfe-342">**(なし、生成されたプロキシ) エラー ハンドラーを追加します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-342">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="0bbfe-343">次の例では、メソッドの呼び出しからエラーを処理する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-343">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="0bbfe-344">**ハンドル (生成されたプロキシ) のメソッドの呼び出しからのエラー**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-344">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="0bbfe-345">**(なし、生成されたプロキシ) のメソッドの呼び出しからエラーを処理します。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-345">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="0bbfe-346">メソッドの呼び出しに失敗した場合、`error`もイベントはためで自分のコード、`error`メソッド ハンドラーし、、`.fail`メソッド コールバックを実行します。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-346">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="0bbfe-347">クライアント側のログ記録を有効にする方法</span><span class="sxs-lookup"><span data-stu-id="0bbfe-347">How to enable client-side logging</span></span>

<span data-ttu-id="0bbfe-348">接続でクライアント側のログ記録を有効にするには設定、`logging`を呼び出す前に、接続オブジェクトのプロパティを`start`接続を確立する方法です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-348">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="0bbfe-349">**(生成されたプロキシ) を使用してログ記録を有効にします。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-349">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="0bbfe-350">**(なし、生成されたプロキシ) のログ記録を有効にします。**</span><span class="sxs-lookup"><span data-stu-id="0bbfe-350">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="0bbfe-351">ログを表示するには、ブラウザーの developer tools を開きコンソール タブに移動します。示す詳細な手順とスクリーン ショットこれを行う方法を示すチュートリアルでは、次を参照してください。[サーバーは、ASP.NET Signalr - ログ記録を有効にするとブロードキャスト](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging)です。</span><span class="sxs-lookup"><span data-stu-id="0bbfe-351">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span></span>
