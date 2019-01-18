---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: ASP.NET SignalR ハブ API ガイド - .NET クライアント (c#) |Microsoft Docs
author: pfletcher
description: このドキュメントでは、Windows ストア (WinRT)、WPF、Silverlight、および短所など、.NET クライアントでは、バージョン 2 の SignalR のハブ API を使用して紹介しています.
ms.author: riande
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 9981b8b91be3395b1a3aa7e0cabb1b7f455d47be
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396182"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="4320e-103">ASP.NET SignalR ハブ API ガイド - .NET クライアント (c#)</span><span class="sxs-lookup"><span data-stu-id="4320e-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>
====================

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="4320e-104">このドキュメントでは、SignalR など、Windows ストア (WinRT)、WPF、Silverlight、およびコンソール アプリケーションの .NET クライアントのバージョン 2 の Hubs API の使用の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="4320e-104">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
>
> <span data-ttu-id="4320e-105">SignalR ハブの API では、サーバーからに接続されているクライアントとサーバーのクライアントからのリモート プロシージャ コール (Rpc) を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="4320e-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="4320e-106">サーバー コードで、クライアントから呼び出すことができるメソッドを定義して、クライアント上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4320e-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="4320e-107">クライアント コードで、サーバーから呼び出すことができるメソッドを定義して、サーバー上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4320e-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="4320e-108">SignalR は、のすべてのクライアントとサーバーが処理されます。</span><span class="sxs-lookup"><span data-stu-id="4320e-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="4320e-109">SignalR では、永続的な接続と呼ばれる下位レベル API も提供します。</span><span class="sxs-lookup"><span data-stu-id="4320e-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="4320e-110">概要については、SignalR、ハブ、および永続的な接続は、または完全な SignalR アプリケーションを構築する方法を示すチュートリアルについてを参照してください。 [SignalR - Getting Started](../getting-started/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="4320e-110">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="4320e-111">このトピックで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="4320e-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="4320e-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="4320e-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="4320e-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="4320e-113">.NET 4.5</span></span>
> - <span data-ttu-id="4320e-114">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="4320e-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="4320e-115">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="4320e-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="4320e-116">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="4320e-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="4320e-117">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="4320e-117">Questions and comments</span></span>
>
> <span data-ttu-id="4320e-118">このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。</span><span class="sxs-lookup"><span data-stu-id="4320e-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="4320e-119">チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。</span><span class="sxs-lookup"><span data-stu-id="4320e-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="4320e-120">概要</span><span class="sxs-lookup"><span data-stu-id="4320e-120">Overview</span></span>

<span data-ttu-id="4320e-121">このドキュメントは、次のトピックに分かれています。</span><span class="sxs-lookup"><span data-stu-id="4320e-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="4320e-122">クライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="4320e-122">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="4320e-123">接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-123">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="4320e-124">Silverlight クライアントからドメイン間の接続</span><span class="sxs-lookup"><span data-stu-id="4320e-124">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="4320e-125">接続を構成する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-125">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="4320e-126">WPF クライアントでの同時接続の最大数を設定する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-126">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="4320e-127">クエリ文字列パラメーターを指定する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-127">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="4320e-128">トランスポート メソッドを指定する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-128">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="4320e-129">HTTP ヘッダーを指定する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-129">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="4320e-130">クライアント証明書を指定する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-130">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="4320e-131">ハブ プロキシを作成する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-131">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="4320e-132">サーバーが呼び出すことができるクライアントでメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-132">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="4320e-133">パラメーターなしのメソッド</span><span class="sxs-lookup"><span data-stu-id="4320e-133">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="4320e-134">パラメーターの型を指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="4320e-134">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="4320e-135">パラメーターの動的オブジェクトを指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="4320e-135">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="4320e-136">ハンドラーを削除する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-136">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="4320e-137">クライアントからサーバーのメソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="4320e-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="4320e-138">接続の有効期間イベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="4320e-139">エラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="4320e-140">クライアント側のログ記録を有効にする方法</span><span class="sxs-lookup"><span data-stu-id="4320e-140">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="4320e-141">WPF、Silverlight、およびコンソール アプリケーションのコード サンプル、サーバーが呼び出すことができるクライアント メソッド</span><span class="sxs-lookup"><span data-stu-id="4320e-141">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="4320e-142">サンプル .NET クライアント プロジェクトでは、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4320e-142">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="4320e-143">[gustavo armenta/SignalR サンプル](https://github.com/gustavo-armenta/SignalR-Samples)github.com (WinRT、Silverlight、コンソール アプリの例)。</span><span class="sxs-lookup"><span data-stu-id="4320e-143">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="4320e-144">[DamianEdwards/SignalR MoveShapeDemo/MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) github.com (WPF など)。</span><span class="sxs-lookup"><span data-stu-id="4320e-144">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="4320e-145">[SignalR/Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) github.com (コンソール アプリなど)。</span><span class="sxs-lookup"><span data-stu-id="4320e-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="4320e-146">サーバーや JavaScript クライアントをプログラミングする方法に関するドキュメントについては、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4320e-146">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="4320e-147">SignalR ハブ API ガイド - サーバー</span><span class="sxs-lookup"><span data-stu-id="4320e-147">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="4320e-148">SignalR ハブ API ガイド - JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="4320e-148">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="4320e-149">.NET 4.5 バージョンの API は API のリファレンス トピックへのリンクです。</span><span class="sxs-lookup"><span data-stu-id="4320e-149">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="4320e-150">.NET 4 を使用している場合は、次を参照してください。 [.NET 4 のバージョンを API のトピックの](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4320e-150">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="4320e-151">クライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="4320e-151">Client setup</span></span>

<span data-ttu-id="4320e-152">インストール、 [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet パッケージ (いない、 [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr)パッケージ)。</span><span class="sxs-lookup"><span data-stu-id="4320e-152">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="4320e-153">このパッケージは、.NET 4 および .NET 4.5 の両方の WinRT、Silverlight、WPF、コンソール アプリケーション、および Windows Phone のクライアントをサポートします。</span><span class="sxs-lookup"><span data-stu-id="4320e-153">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="4320e-154">SignalR が存在するクライアントのバージョンがサーバー上にあるバージョンと異なる場合は、SignalR は、違いに適応することが多くの場合。</span><span class="sxs-lookup"><span data-stu-id="4320e-154">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="4320e-155">たとえば、SignalR バージョン 2 を実行しているサーバーでは、1.1.x がインストールされているクライアントだけでなくバージョン 2 がインストールされているクライアントをサポートは。</span><span class="sxs-lookup"><span data-stu-id="4320e-155">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="4320e-156">かどうか、サーバー上のバージョンと、クライアントのバージョンの間の差が多すぎる、またはクライアントがサーバーよりも新しい場合は、SignalR がスローされます、`InvalidOperationException`クライアントが接続を確立しようとした場合に例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="4320e-156">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="4320e-157">エラー メッセージは"`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`"。</span><span class="sxs-lookup"><span data-stu-id="4320e-157">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="4320e-158">接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-158">How to establish a connection</span></span>

<span data-ttu-id="4320e-159">作成する必要が接続を確立する前に、`HubConnection`オブジェクトし、プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="4320e-159">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="4320e-160">接続を確立するために呼び出す、`Start`メソッドを`HubConnection`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4320e-160">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="4320e-161">JavaScript クライアントには、呼び出す前に少なくとも 1 つのイベント ハンドラーを登録する必要が、`Start`メソッドは、接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="4320e-161">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="4320e-162">これは、.NET クライアントの必要はありません。</span><span class="sxs-lookup"><span data-stu-id="4320e-162">This is not necessary for .NET clients.</span></span> <span data-ttu-id="4320e-163">JavaScript クライアントは、生成されたプロキシ コードに自動的に存在するすべてのハブ プロキシをサーバーを作成、およびハンドラーの登録は、どのハブを指定する方法、クライアントが使用します。</span><span class="sxs-lookup"><span data-stu-id="4320e-163">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="4320e-164">.NET クライアントのハブ プロキシ手動で作成するため、SignalR のプロキシを作成するのいずれかのハブを使用することを想定しています。</span><span class="sxs-lookup"><span data-stu-id="4320e-164">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="4320e-165">サンプル コードは、既定値を使用して"/signalr"SignalR サービスに接続するための URL。</span><span class="sxs-lookup"><span data-stu-id="4320e-165">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="4320e-166">別の基本 URL を指定する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバー -/signalr URL](hubs-api-guide-server.md#signalrurl)します。</span><span class="sxs-lookup"><span data-stu-id="4320e-166">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="4320e-167">`Start`メソッドを非同期的に実行します。</span><span class="sxs-lookup"><span data-stu-id="4320e-167">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="4320e-168">後続行のコードは、接続が確立された後まで実行されないようにするには、次のように使用します。 `await` ASP.NET 4.5 の非同期メソッドまたは`.Wait()`同期メソッドにします。</span><span class="sxs-lookup"><span data-stu-id="4320e-168">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="4320e-169">使用しない`.Wait()`WinRT クライアント。</span><span class="sxs-lookup"><span data-stu-id="4320e-169">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="4320e-170">Silverlight クライアントからドメイン間の接続</span><span class="sxs-lookup"><span data-stu-id="4320e-170">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="4320e-171">Silverlight クライアントからドメイン間の接続を有効にする方法については、次を参照してください。[使用可能なドメインの境界を越えてサービスを行う](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4320e-171">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="4320e-172">接続を構成する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-172">How to configure the connection</span></span>

<span data-ttu-id="4320e-173">接続を確立する前に、次のオプションのいずれかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="4320e-173">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="4320e-174">同時接続数を制限します。</span><span class="sxs-lookup"><span data-stu-id="4320e-174">Concurrent connections limit.</span></span>
- <span data-ttu-id="4320e-175">クエリ文字列パラメーター。</span><span class="sxs-lookup"><span data-stu-id="4320e-175">Query string parameters.</span></span>
- <span data-ttu-id="4320e-176">トランスポート メソッド。</span><span class="sxs-lookup"><span data-stu-id="4320e-176">The transport method.</span></span>
- <span data-ttu-id="4320e-177">HTTP ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="4320e-177">HTTP headers.</span></span>
- <span data-ttu-id="4320e-178">クライアント証明書。</span><span class="sxs-lookup"><span data-stu-id="4320e-178">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="4320e-179">WPF クライアントでの同時接続の最大数を設定する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-179">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="4320e-180">、WPF のクライアントでは、2 の既定値からの同時接続の最大数を増やす必要があります。</span><span class="sxs-lookup"><span data-stu-id="4320e-180">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="4320e-181">推奨値には 10 です。</span><span class="sxs-lookup"><span data-stu-id="4320e-181">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="4320e-182">詳細については、次を参照してください。 [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4320e-182">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="4320e-183">クエリ文字列パラメーターを指定する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-183">How to specify query string parameters</span></span>

<span data-ttu-id="4320e-184">クライアントが接続するときに、サーバーにデータを送信する場合は、接続オブジェクトをクエリ文字列パラメーターを追加できます。</span><span class="sxs-lookup"><span data-stu-id="4320e-184">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="4320e-185">次の例では、クライアント コードで、クエリ文字列パラメーターを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4320e-185">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="4320e-186">次の例では、サーバー コードでクエリ文字列パラメーターを読み取る方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4320e-186">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="4320e-187">トランスポート メソッドを指定する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-187">How to specify the transport method</span></span>

<span data-ttu-id="4320e-188">接続するプロセスの一環として、SignalR クライアントは、通常サーバーとクライアントの両方でサポートされている最適なトランスポートを決定する、サーバーとネゴシエートします。</span><span class="sxs-lookup"><span data-stu-id="4320e-188">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="4320e-189">使用するトランスポートを既に知っている場合は、このネゴシエーション プロセスをバイパスできます。</span><span class="sxs-lookup"><span data-stu-id="4320e-189">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="4320e-190">トランスポートの方法を指定するには、Start メソッドにトランスポート オブジェクトで渡します。</span><span class="sxs-lookup"><span data-stu-id="4320e-190">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="4320e-191">次の例では、クライアント コードでトランスポート メソッドを指定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4320e-191">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="4320e-192">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx)名前空間には、トランスポートを指定する際、次のクラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4320e-192">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="4320e-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="4320e-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="4320e-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="4320e-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="4320e-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (サーバーとクライアントの両方が .NET 4.5 を使用した場合にのみ表示されます)。</span><span class="sxs-lookup"><span data-stu-id="4320e-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="4320e-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (クライアントとサーバーの両方でサポートされている最適なトランスポートを自動的に選択します。</span><span class="sxs-lookup"><span data-stu-id="4320e-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="4320e-197">これは、既定のトランスポートです。</span><span class="sxs-lookup"><span data-stu-id="4320e-197">This is the default transport.</span></span> <span data-ttu-id="4320e-198">渡すをこれには、`Start`メソッドが何もで渡されていないのと同じ効果です)。</span><span class="sxs-lookup"><span data-stu-id="4320e-198">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="4320e-199">ブラウザーでのみ使用されているために、ForeverFrame トランスポートはこの一覧に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="4320e-199">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="4320e-200">サーバー コードでの転送方法を確認する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーのコンテキスト プロパティからのクライアントに関する情報を取得する方法](hubs-api-guide-server.md#contextproperty)します。</span><span class="sxs-lookup"><span data-stu-id="4320e-200">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="4320e-201">トランスポートとフォールバックの詳細については、次を参照してください。 [SignalR のトランスポートとフォールバックの概要](../getting-started/introduction-to-signalr.md#transports)します。</span><span class="sxs-lookup"><span data-stu-id="4320e-201">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="4320e-202">HTTP ヘッダーを指定する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-202">How to specify HTTP headers</span></span>

<span data-ttu-id="4320e-203">HTTP ヘッダーを設定するには、使用、`Headers`接続オブジェクトのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="4320e-203">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="4320e-204">次の例では、HTTP ヘッダーを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4320e-204">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="4320e-205">クライアント証明書を指定する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-205">How to specify client certificates</span></span>

<span data-ttu-id="4320e-206">クライアント証明書を追加するには、使用、`AddClientCertificate`接続オブジェクトのメソッド。</span><span class="sxs-lookup"><span data-stu-id="4320e-206">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="4320e-207">ハブ プロキシを作成する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-207">How to create the Hub proxy</span></span>

<span data-ttu-id="4320e-208">ハブは、サーバーから呼び出すことができるクライアントでメソッドを定義するとで、サーバーでのハブ メソッドを呼び出すには、呼び出すことによって、ハブのプロキシの作成`CreateHubProxy`接続オブジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="4320e-208">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="4320e-209">文字列に渡す`CreateHubProxy`は、ハブ クラスの名前またはで指定された名前、`HubName`属性が 1 つは、サーバーで使用されていた場合。</span><span class="sxs-lookup"><span data-stu-id="4320e-209">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="4320e-210">大文字と小文字が一致する名前です。</span><span class="sxs-lookup"><span data-stu-id="4320e-210">Name matching is case-insensitive.</span></span>

<span data-ttu-id="4320e-211">**サーバー上のハブ クラス**</span><span class="sxs-lookup"><span data-stu-id="4320e-211">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="4320e-212">**ハブ クラス用のクライアント プロキシを作成します。**</span><span class="sxs-lookup"><span data-stu-id="4320e-212">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="4320e-213">使用してハブ クラスを修飾する場合、`HubName`属性、その名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="4320e-213">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="4320e-214">**サーバー上のハブ クラス**</span><span class="sxs-lookup"><span data-stu-id="4320e-214">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="4320e-215">**ハブ クラス用のクライアント プロキシを作成します。**</span><span class="sxs-lookup"><span data-stu-id="4320e-215">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="4320e-216">呼び出す場合`HubConnection.CreateHubProxy`で複数回、同じ`hubName`、キャッシュされた同じ`IHubProxy`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4320e-216">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="4320e-217">サーバーが呼び出すことができるクライアントでメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-217">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="4320e-218">サーバーが呼び出すことができるメソッドを定義するプロキシを使用して、`On`イベント ハンドラーを登録するメソッド。</span><span class="sxs-lookup"><span data-stu-id="4320e-218">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="4320e-219">大文字と小文字が一致するメソッドの名前です。</span><span class="sxs-lookup"><span data-stu-id="4320e-219">Method name matching is case-insensitive.</span></span> <span data-ttu-id="4320e-220">たとえば、`Clients.All.UpdateStockPrice`サーバーで実行`updateStockPrice`、 `updatestockprice`、または`UpdateStockPrice`クライアント。</span><span class="sxs-lookup"><span data-stu-id="4320e-220">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="4320e-221">別のクライアント プラットフォームでは、UI を更新するメソッドのコードを記述する方法のさまざまな要件があります。</span><span class="sxs-lookup"><span data-stu-id="4320e-221">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="4320e-222">WinRT (Windows ストア .NET) クライアントの表示例を示します。</span><span class="sxs-lookup"><span data-stu-id="4320e-222">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="4320e-223">WPF、Silverlight、およびコンソール アプリケーションの例がで提供される[別のセクションでは、このトピックで後述](#wpfsl)します。</span><span class="sxs-lookup"><span data-stu-id="4320e-223">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="4320e-224">パラメーターなしのメソッド</span><span class="sxs-lookup"><span data-stu-id="4320e-224">Methods without parameters</span></span>

<span data-ttu-id="4320e-225">処理しているメソッドにパラメーターがあるない場合は、非ジェネリック オーバー ロードを使用して、`On`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4320e-225">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="4320e-226">**パラメーターなしのクライアント メソッドを呼び出すサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="4320e-226">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="4320e-227">**パラメーターなしのサーバーからメソッドの WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照してください](#wpfsl))。**</span><span class="sxs-lookup"><span data-stu-id="4320e-227">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="4320e-228">パラメーターの型を指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="4320e-228">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="4320e-229">処理しているメソッドにパラメーターがある場合は、パラメーターの型を指定のジェネリック型として、`On`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4320e-229">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="4320e-230">ジェネリック オーバー ロードがある、`On`メソッドを使用すると、最大 8 個のパラメーター (Windows Phone 7 では 4) を指定します。</span><span class="sxs-lookup"><span data-stu-id="4320e-230">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="4320e-231">次の例に 1 つのパラメーターを送信、`UpdateStockPrice`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4320e-231">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="4320e-232">**サーバー コードのパラメーターを持つクライアント メソッドを呼び出す**</span><span class="sxs-lookup"><span data-stu-id="4320e-232">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="4320e-233">**パラメーターに使用される Stock クラス**</span><span class="sxs-lookup"><span data-stu-id="4320e-233">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="4320e-234">**パラメーターを持つサーバーからメソッドの WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照してください](#wpfsl))。**</span><span class="sxs-lookup"><span data-stu-id="4320e-234">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="4320e-235">パラメーターの動的オブジェクトを指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="4320e-235">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="4320e-236">ジェネリック型としてパラメーターを指定する代わりに、`On`メソッドでは、動的オブジェクトとしてパラメーターを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="4320e-236">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="4320e-237">**サーバー コードのパラメーターを持つクライアント メソッドを呼び出す**</span><span class="sxs-lookup"><span data-stu-id="4320e-237">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="4320e-238">**パラメーターに使用される Stock クラス**</span><span class="sxs-lookup"><span data-stu-id="4320e-238">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="4320e-239">**パラメーターの動的オブジェクトを使用して、パラメーターを持つサーバーからメソッドの WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照してください](#wpfsl))。**</span><span class="sxs-lookup"><span data-stu-id="4320e-239">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="4320e-240">ハンドラーを削除する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-240">How to remove a handler</span></span>

<span data-ttu-id="4320e-241">ハンドラーを削除するには、呼び出すその`Dispose`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4320e-241">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="4320e-242">**サーバーから呼び出されるメソッドのクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="4320e-242">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="4320e-243">**クライアントのコード ハンドラーを削除するには**</span><span class="sxs-lookup"><span data-stu-id="4320e-243">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="4320e-244">クライアントからサーバーのメソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="4320e-244">How to call server methods from the client</span></span>

<span data-ttu-id="4320e-245">サーバー上のメソッドを呼び出すを使用して、`Invoke`ハブ プロキシのメソッド。</span><span class="sxs-lookup"><span data-stu-id="4320e-245">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="4320e-246">サーバー メソッドが戻り値を持たない場合は、非ジェネリック オーバー ロードを使用して、`Invoke`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4320e-246">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="4320e-247">**サーバー コードのメソッドの戻り値がありません。**</span><span class="sxs-lookup"><span data-stu-id="4320e-247">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="4320e-248">**戻り値を持たないメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="4320e-248">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="4320e-249">サーバー メソッドの戻り値の場合は、戻り値の型を指定のジェネリック型として、`Invoke`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4320e-249">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="4320e-250">**サーバー コードのメソッドの戻り値があり、複雑な型パラメーターを受け取る**</span><span class="sxs-lookup"><span data-stu-id="4320e-250">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="4320e-251">**パラメーターと戻り値に使用される Stock クラス**</span><span class="sxs-lookup"><span data-stu-id="4320e-251">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="4320e-252">**ASP.NET 4.5 の非同期メソッドで複合型のパラメーターを受け取って戻り値を持つメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="4320e-252">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="4320e-253">**戻り値を同期メソッドで複合型のパラメーターを受け取るメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="4320e-253">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="4320e-254">`Invoke`メソッドが非同期で実行し、返します、`Task`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4320e-254">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="4320e-255">指定しない場合は`await`または`.Wait()`、呼び出すメソッドの実行が完了する前に次のコード行が実行されます。</span><span class="sxs-lookup"><span data-stu-id="4320e-255">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="4320e-256">接続の有効期間イベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-256">How to handle connection lifetime events</span></span>

<span data-ttu-id="4320e-257">SignalR は、次の接続に処理できる有効期間イベントを提供します。</span><span class="sxs-lookup"><span data-stu-id="4320e-257">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="4320e-258">`Received`:接続でデータを受信したときに発生します。</span><span class="sxs-lookup"><span data-stu-id="4320e-258">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="4320e-259">受信したデータを提供します。</span><span class="sxs-lookup"><span data-stu-id="4320e-259">Provides the received data.</span></span>
- <span data-ttu-id="4320e-260">`ConnectionSlow`:クライアントが低速または削除が頻繁に接続を検出したときに発生します。</span><span class="sxs-lookup"><span data-stu-id="4320e-260">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="4320e-261">`Reconnecting`:基になるトランスポートの再接続を開始するときに発生します。</span><span class="sxs-lookup"><span data-stu-id="4320e-261">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="4320e-262">`Reconnected`:基になるトランスポートが再接続されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="4320e-262">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="4320e-263">`StateChanged`:接続状態が変更されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="4320e-263">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="4320e-264">以前の状態と新しい状態を提供します。</span><span class="sxs-lookup"><span data-stu-id="4320e-264">Provides the old state and the new state.</span></span> <span data-ttu-id="4320e-265">接続に関する情報の状態の値を参照してください[ConnectionState 列挙](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4320e-265">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="4320e-266">`Closed`:接続が切断されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="4320e-266">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="4320e-267">たとえば、致命的ではありませんが、断続的な接続の問題が発生するエラーを警告メッセージを表示する場合は、パフォーマンスの低下や頻繁になど、接続の削除を処理、`ConnectionSlow`イベント。</span><span class="sxs-lookup"><span data-stu-id="4320e-267">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="4320e-268">詳細については、次を参照してください。 [SignalR の接続の有効期間イベントの処理と理解](handling-connection-lifetime-events.md)します。</span><span class="sxs-lookup"><span data-stu-id="4320e-268">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="4320e-269">エラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="4320e-269">How to handle errors</span></span>

<span data-ttu-id="4320e-270">場合は、サーバー上の詳細なエラー メッセージを明示的に有効にしない、SignalR が返すエラーが発生した例外オブジェクトには、エラーに関する最小限の情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="4320e-270">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="4320e-271">呼び出しなど`newContosoChatMessage`失敗した場合、エラー オブジェクトにエラー メッセージが含まれています"`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`"送信の詳細なエラー メッセージを有効にする場合は、セキュリティ上の理由の詳細なエラー メッセージを運用環境でクライアントには推奨されませんトラブルシューティングのため、サーバーで次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="4320e-271">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="4320e-272">SignalR が発生したエラーを処理するためのハンドラーを追加できる、`Error`接続オブジェクトのイベント。</span><span class="sxs-lookup"><span data-stu-id="4320e-272">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="4320e-273">メソッドの呼び出しからエラーを処理するには、try-catch ブロックでコードをラップします。</span><span class="sxs-lookup"><span data-stu-id="4320e-273">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="4320e-274">クライアント側のログ記録を有効にする方法</span><span class="sxs-lookup"><span data-stu-id="4320e-274">How to enable client-side logging</span></span>

<span data-ttu-id="4320e-275">クライアント側のログ記録を有効にするには設定、`TraceLevel`と`TraceWriter`接続オブジェクトのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="4320e-275">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="4320e-276">WPF、Silverlight、およびコンソール アプリケーションのコード サンプル、サーバーが呼び出すことができるクライアント メソッド</span><span class="sxs-lookup"><span data-stu-id="4320e-276">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="4320e-277">サーバーが呼び出すことができるクライアント メソッドを定義する前に示したコード サンプルは、WinRT クライアントに適用されます。</span><span class="sxs-lookup"><span data-stu-id="4320e-277">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="4320e-278">次のサンプルでは、WPF、Silverlight、およびコンソール アプリケーションのクライアントの同等のコードを表示します。</span><span class="sxs-lookup"><span data-stu-id="4320e-278">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="4320e-279">パラメーターなしのメソッド</span><span class="sxs-lookup"><span data-stu-id="4320e-279">Methods without parameters</span></span>

<span data-ttu-id="4320e-280">**パラメーターなしのサーバーから呼び出されるメソッドの WPF クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="4320e-280">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="4320e-281">**パラメーターなしのサーバーから呼び出されるメソッドの Silverlight クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="4320e-281">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="4320e-282">**メソッドのコンソール アプリケーションのクライアント コードは、パラメーターなしのサーバーから呼び出されます**</span><span class="sxs-lookup"><span data-stu-id="4320e-282">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="4320e-283">パラメーターの型を指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="4320e-283">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="4320e-284">**パラメーターを持つサーバーから呼び出されるメソッドの WPF クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="4320e-284">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="4320e-285">**パラメーターを持つサーバーから呼び出されるメソッドの Silverlight クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="4320e-285">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="4320e-286">**パラメーターを持つサーバーからメソッドのコンソール アプリケーションのクライアント コードが呼び出されます**</span><span class="sxs-lookup"><span data-stu-id="4320e-286">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="4320e-287">パラメーターの動的オブジェクトを指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="4320e-287">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="4320e-288">**パラメーターの動的オブジェクトを使用して、パラメーターを持つサーバーから呼び出されるメソッドの WPF クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="4320e-288">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="4320e-289">**パラメーターの動的オブジェクトを使用して、パラメーターを持つサーバーから呼び出されるメソッドの Silverlight クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="4320e-289">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="4320e-290">**パラメーターの動的オブジェクトを使用して、パラメーターを持つサーバーからメソッドのコンソール アプリケーションのクライアント コードが呼び出されます**</span><span class="sxs-lookup"><span data-stu-id="4320e-290">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
