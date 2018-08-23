---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: ASP.NET SignalR ハブ API ガイド - .NET クライアント (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: このドキュメントでは、Windows ストア (WinRT)、WPF、Silverlight、および短所など、.NET クライアントでは、バージョン 2 の SignalR のハブ API を使用して紹介しています.
ms.author: riande
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 5889429645ea1c682ea43c4b17afb3745318e32d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837120"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="611e4-103">ASP.NET SignalR ハブ API ガイド - .NET クライアント (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="611e4-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>
====================
<span data-ttu-id="611e4-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="611e4-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="611e4-105">このドキュメントでは、SignalR など、Windows ストア (WinRT)、WPF、Silverlight、およびコンソール アプリケーションの .NET クライアントのバージョン 2 の Hubs API の使用の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="611e4-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="611e4-106">SignalR ハブの API では、サーバーからに接続されているクライアントとサーバーのクライアントからのリモート プロシージャ コール (Rpc) を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="611e4-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="611e4-107">サーバー コードで、クライアントから呼び出すことができるメソッドを定義して、クライアント上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="611e4-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="611e4-108">クライアント コードで、サーバーから呼び出すことができるメソッドを定義して、サーバー上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="611e4-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="611e4-109">SignalR は、のすべてのクライアントとサーバーが処理されます。</span><span class="sxs-lookup"><span data-stu-id="611e4-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="611e4-110">SignalR では、永続的な接続と呼ばれる下位レベル API も提供します。</span><span class="sxs-lookup"><span data-stu-id="611e4-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="611e4-111">概要については、SignalR、ハブ、および永続的な接続は、または完全な SignalR アプリケーションを構築する方法を示すチュートリアルについてを参照してください。 [SignalR - Getting Started](../getting-started/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="611e4-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="611e4-112">概要</span><span class="sxs-lookup"><span data-stu-id="611e4-112">Overview</span></span>

<span data-ttu-id="611e4-113">このドキュメントは、次のトピックに分かれています。</span><span class="sxs-lookup"><span data-stu-id="611e4-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="611e4-114">クライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="611e4-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="611e4-115">接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="611e4-116">Silverlight クライアントからドメイン間の接続</span><span class="sxs-lookup"><span data-stu-id="611e4-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="611e4-117">接続を構成する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="611e4-118">WPF クライアントでの同時接続の最大数を設定する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="611e4-119">クエリ文字列パラメーターを指定する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="611e4-120">トランスポート メソッドを指定する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="611e4-121">HTTP ヘッダーを指定する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="611e4-122">クライアント証明書を指定する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="611e4-123">ハブ プロキシを作成する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="611e4-124">サーバーが呼び出すことができるクライアントでメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="611e4-125">パラメーターなしのメソッド</span><span class="sxs-lookup"><span data-stu-id="611e4-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="611e4-126">パラメーターの型を指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="611e4-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="611e4-127">パラメーターの動的オブジェクトを指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="611e4-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="611e4-128">ハンドラーを削除する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="611e4-129">クライアントからサーバーのメソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="611e4-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="611e4-130">接続の有効期間イベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="611e4-131">エラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="611e4-132">クライアント側のログ記録を有効にする方法</span><span class="sxs-lookup"><span data-stu-id="611e4-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="611e4-133">WPF、Silverlight、およびコンソール アプリケーションのコード サンプル、サーバーが呼び出すことができるクライアント メソッド</span><span class="sxs-lookup"><span data-stu-id="611e4-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="611e4-134">サンプル .NET クライアント プロジェクトでは、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="611e4-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="611e4-135">[gustavo armenta/SignalR サンプル](https://github.com/gustavo-armenta/SignalR-Samples)github.com (WinRT、Silverlight、コンソール アプリの例)。</span><span class="sxs-lookup"><span data-stu-id="611e4-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="611e4-136">[DamianEdwards/SignalR MoveShapeDemo/MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) github.com (WPF など)。</span><span class="sxs-lookup"><span data-stu-id="611e4-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="611e4-137">[SignalR/Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) github.com (コンソール アプリなど)。</span><span class="sxs-lookup"><span data-stu-id="611e4-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="611e4-138">サーバーや JavaScript クライアントをプログラミングする方法に関するドキュメントについては、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="611e4-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="611e4-139">SignalR ハブ API ガイド - サーバー</span><span class="sxs-lookup"><span data-stu-id="611e4-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="611e4-140">SignalR ハブ API ガイド - JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="611e4-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="611e4-141">.NET 4.5 バージョンの API は API のリファレンス トピックへのリンクです。</span><span class="sxs-lookup"><span data-stu-id="611e4-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="611e4-142">.NET 4 を使用している場合は、次を参照してください。 [.NET 4 のバージョンを API のトピックの](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="611e4-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="611e4-143">クライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="611e4-143">Client setup</span></span>

<span data-ttu-id="611e4-144">インストール、 [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet パッケージ (いない、 [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr)パッケージ)。</span><span class="sxs-lookup"><span data-stu-id="611e4-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="611e4-145">このパッケージは、.NET 4 および .NET 4.5 の両方の WinRT、Silverlight、WPF、コンソール アプリケーション、および Windows Phone のクライアントをサポートします。</span><span class="sxs-lookup"><span data-stu-id="611e4-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="611e4-146">SignalR が存在するクライアントのバージョンがサーバー上にあるバージョンと異なる場合は、SignalR は、違いに適応することが多くの場合。</span><span class="sxs-lookup"><span data-stu-id="611e4-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="611e4-147">たとえば、SignalR バージョン 2.0 がリリースされ、サーバーをインストールする、サーバーは 1.1.x および 2.0 がインストールされているクライアントをインストールしているクライアントをサポートします。</span><span class="sxs-lookup"><span data-stu-id="611e4-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="611e4-148">サーバー上のバージョンと、クライアントのバージョンの間の差が大きすぎる場合は、SignalR がスローされます、`InvalidOperationException`クライアントが接続を確立しようとした場合に例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="611e4-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="611e4-149">エラー メッセージは"`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`"。</span><span class="sxs-lookup"><span data-stu-id="611e4-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="611e4-150">接続を確立する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-150">How to establish a connection</span></span>

<span data-ttu-id="611e4-151">作成する必要が接続を確立する前に、`HubConnection`オブジェクトし、プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="611e4-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="611e4-152">接続を確立するために呼び出す、`Start`メソッドを`HubConnection`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="611e4-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="611e4-153">JavaScript クライアントには、呼び出す前に少なくとも 1 つのイベント ハンドラーを登録する必要が、`Start`メソッドは、接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="611e4-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="611e4-154">これは、.NET クライアントの必要はありません。</span><span class="sxs-lookup"><span data-stu-id="611e4-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="611e4-155">JavaScript クライアントは、生成されたプロキシ コードに自動的に存在するすべてのハブ プロキシをサーバーを作成、およびハンドラーの登録は、どのハブを指定する方法、クライアントが使用します。</span><span class="sxs-lookup"><span data-stu-id="611e4-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="611e4-156">.NET クライアントのハブ プロキシ手動で作成するため、SignalR のプロキシを作成するのいずれかのハブを使用することを想定しています。</span><span class="sxs-lookup"><span data-stu-id="611e4-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="611e4-157">サンプル コードは、既定値を使用して"/signalr"SignalR サービスに接続するための URL。</span><span class="sxs-lookup"><span data-stu-id="611e4-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="611e4-158">別の基本 URL を指定する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバー -/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)します。</span><span class="sxs-lookup"><span data-stu-id="611e4-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="611e4-159">`Start`メソッドを非同期的に実行します。</span><span class="sxs-lookup"><span data-stu-id="611e4-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="611e4-160">後続行のコードは、接続が確立された後まで実行されないようにするには、次のように使用します。 `await` ASP.NET 4.5 の非同期メソッドまたは`.Wait()`同期メソッドにします。</span><span class="sxs-lookup"><span data-stu-id="611e4-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="611e4-161">使用しない`.Wait()`WinRT クライアント。</span><span class="sxs-lookup"><span data-stu-id="611e4-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="611e4-162">`HubConnection` クラスはスレッド セーフです。</span><span class="sxs-lookup"><span data-stu-id="611e4-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="611e4-163">Silverlight クライアントからドメイン間の接続</span><span class="sxs-lookup"><span data-stu-id="611e4-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="611e4-164">Silverlight クライアントからドメイン間の接続を有効にする方法については、次を参照してください。[使用可能なドメインの境界を越えてサービスを行う](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="611e4-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="611e4-165">接続を構成する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-165">How to configure the connection</span></span>

<span data-ttu-id="611e4-166">接続を確立する前に、次のオプションのいずれかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="611e4-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="611e4-167">同時接続数を制限します。</span><span class="sxs-lookup"><span data-stu-id="611e4-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="611e4-168">クエリ文字列パラメーター。</span><span class="sxs-lookup"><span data-stu-id="611e4-168">Query string parameters.</span></span>
- <span data-ttu-id="611e4-169">トランスポート メソッド。</span><span class="sxs-lookup"><span data-stu-id="611e4-169">The transport method.</span></span>
- <span data-ttu-id="611e4-170">HTTP ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="611e4-170">HTTP headers.</span></span>
- <span data-ttu-id="611e4-171">クライアント証明書。</span><span class="sxs-lookup"><span data-stu-id="611e4-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="611e4-172">WPF クライアントでの同時接続の最大数を設定する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="611e4-173">、WPF のクライアントでは、2 の既定値からの同時接続の最大数を増やす必要があります。</span><span class="sxs-lookup"><span data-stu-id="611e4-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="611e4-174">推奨値には 10 です。</span><span class="sxs-lookup"><span data-stu-id="611e4-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="611e4-175">詳細については、次を参照してください。 [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="611e4-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="611e4-176">クエリ文字列パラメーターを指定する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-176">How to specify query string parameters</span></span>

<span data-ttu-id="611e4-177">クライアントが接続するときに、サーバーにデータを送信する場合は、接続オブジェクトをクエリ文字列パラメーターを追加できます。</span><span class="sxs-lookup"><span data-stu-id="611e4-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="611e4-178">次の例では、クライアント コードで、クエリ文字列パラメーターを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="611e4-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="611e4-179">次の例では、サーバー コードでクエリ文字列パラメーターを読み取る方法を示します。</span><span class="sxs-lookup"><span data-stu-id="611e4-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="611e4-180">トランスポート メソッドを指定する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-180">How to specify the transport method</span></span>

<span data-ttu-id="611e4-181">接続するプロセスの一環として、SignalR クライアントは、通常サーバーとクライアントの両方でサポートされている最適なトランスポートを決定する、サーバーとネゴシエートします。</span><span class="sxs-lookup"><span data-stu-id="611e4-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="611e4-182">使用するトランスポートを既に知っている場合は、このネゴシエーション プロセスをバイパスできます。</span><span class="sxs-lookup"><span data-stu-id="611e4-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="611e4-183">トランスポートの方法を指定するには、Start メソッドにトランスポート オブジェクトで渡します。</span><span class="sxs-lookup"><span data-stu-id="611e4-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="611e4-184">次の例では、クライアント コードでトランスポート メソッドを指定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="611e4-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="611e4-185">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx)名前空間には、トランスポートを指定する際、次のクラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="611e4-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="611e4-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="611e4-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="611e4-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="611e4-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="611e4-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (サーバーとクライアントの両方が .NET 4.5 を使用した場合にのみ表示されます)。</span><span class="sxs-lookup"><span data-stu-id="611e4-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="611e4-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (クライアントとサーバーの両方でサポートされている最適なトランスポートを自動的に選択します。</span><span class="sxs-lookup"><span data-stu-id="611e4-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="611e4-190">これは、既定のトランスポートです。</span><span class="sxs-lookup"><span data-stu-id="611e4-190">This is the default transport.</span></span> <span data-ttu-id="611e4-191">渡すをこれには、`Start`メソッドが何もで渡されていないのと同じ効果です)。</span><span class="sxs-lookup"><span data-stu-id="611e4-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="611e4-192">ブラウザーでのみ使用されているために、ForeverFrame トランスポートはこの一覧に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="611e4-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="611e4-193">サーバー コードでの転送方法を確認する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーのコンテキスト プロパティからのクライアントに関する情報を取得する方法](../guide-to-the-api/hubs-api-guide-server.md#contextproperty)します。</span><span class="sxs-lookup"><span data-stu-id="611e4-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="611e4-194">トランスポートとフォールバックの詳細については、次を参照してください。 [SignalR のトランスポートとフォールバックの概要](../getting-started/introduction-to-signalr.md#transports)します。</span><span class="sxs-lookup"><span data-stu-id="611e4-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="611e4-195">HTTP ヘッダーを指定する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-195">How to specify HTTP headers</span></span>

<span data-ttu-id="611e4-196">HTTP ヘッダーを設定するには、使用、`Headers`接続オブジェクトのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="611e4-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="611e4-197">次の例では、HTTP ヘッダーを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="611e4-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="611e4-198">クライアント証明書を指定する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-198">How to specify client certificates</span></span>

<span data-ttu-id="611e4-199">クライアント証明書を追加するには、使用、`AddClientCertificate`接続オブジェクトのメソッド。</span><span class="sxs-lookup"><span data-stu-id="611e4-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="611e4-200">ハブ プロキシを作成する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-200">How to create the Hub proxy</span></span>

<span data-ttu-id="611e4-201">ハブは、サーバーから呼び出すことができるクライアントでメソッドを定義するとで、サーバーでのハブ メソッドを呼び出すには、呼び出すことによって、ハブのプロキシの作成`CreateHubProxy`接続オブジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="611e4-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="611e4-202">文字列に渡す`CreateHubProxy`は、ハブ クラスの名前またはで指定された名前、`HubName`属性が 1 つは、サーバーで使用されていた場合。</span><span class="sxs-lookup"><span data-stu-id="611e4-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="611e4-203">大文字と小文字が一致する名前です。</span><span class="sxs-lookup"><span data-stu-id="611e4-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="611e4-204">**サーバー上のハブ クラス**</span><span class="sxs-lookup"><span data-stu-id="611e4-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="611e4-205">**ハブ クラス用のクライアント プロキシを作成します。**</span><span class="sxs-lookup"><span data-stu-id="611e4-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="611e4-206">使用してハブ クラスを修飾する場合、`HubName`属性、その名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="611e4-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="611e4-207">**サーバー上のハブ クラス**</span><span class="sxs-lookup"><span data-stu-id="611e4-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="611e4-208">**ハブ クラス用のクライアント プロキシを作成します。**</span><span class="sxs-lookup"><span data-stu-id="611e4-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="611e4-209">プロキシ オブジェクトは、スレッド セーフです。</span><span class="sxs-lookup"><span data-stu-id="611e4-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="611e4-210">実際には、呼び出した場合`HubConnection.CreateHubProxy`で複数回、同じ`hubName`、キャッシュされた同じ`IHubProxy`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="611e4-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="611e4-211">サーバーが呼び出すことができるクライアントでメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="611e4-212">サーバーが呼び出すことができるメソッドを定義するプロキシを使用して、`On`イベント ハンドラーを登録するメソッド。</span><span class="sxs-lookup"><span data-stu-id="611e4-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="611e4-213">大文字と小文字が一致するメソッドの名前です。</span><span class="sxs-lookup"><span data-stu-id="611e4-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="611e4-214">たとえば、`Clients.All.UpdateStockPrice`サーバーで実行`updateStockPrice`、 `updatestockprice`、または`UpdateStockPrice`クライアント。</span><span class="sxs-lookup"><span data-stu-id="611e4-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="611e4-215">別のクライアント プラットフォームでは、UI を更新するメソッドのコードを記述する方法のさまざまな要件があります。</span><span class="sxs-lookup"><span data-stu-id="611e4-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="611e4-216">WinRT (Windows ストア .NET) クライアントの表示例を示します。</span><span class="sxs-lookup"><span data-stu-id="611e4-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="611e4-217">WPF、Silverlight、およびコンソール アプリケーションの例がで提供される[別のセクションでは、このトピックで後述](#wpfsl)します。</span><span class="sxs-lookup"><span data-stu-id="611e4-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="611e4-218">パラメーターなしのメソッド</span><span class="sxs-lookup"><span data-stu-id="611e4-218">Methods without parameters</span></span>

<span data-ttu-id="611e4-219">処理しているメソッドにパラメーターがあるない場合は、非ジェネリック オーバー ロードを使用して、`On`メソッド。</span><span class="sxs-lookup"><span data-stu-id="611e4-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="611e4-220">**パラメーターなしのクライアント メソッドを呼び出すサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="611e4-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="611e4-221">**パラメーターなしのサーバーからメソッドの WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照してください](#wpfsl))。**</span><span class="sxs-lookup"><span data-stu-id="611e4-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="611e4-222">パラメーターの型を指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="611e4-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="611e4-223">処理しているメソッドにパラメーターがある場合は、パラメーターの型を指定のジェネリック型として、`On`メソッド。</span><span class="sxs-lookup"><span data-stu-id="611e4-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="611e4-224">ジェネリック オーバー ロードがある、`On`メソッドを使用すると、最大 8 個のパラメーター (Windows Phone 7 では 4) を指定します。</span><span class="sxs-lookup"><span data-stu-id="611e4-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="611e4-225">次の例に 1 つのパラメーターを送信、`UpdateStockPrice`メソッド。</span><span class="sxs-lookup"><span data-stu-id="611e4-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="611e4-226">**サーバー コードのパラメーターを持つクライアント メソッドを呼び出す**</span><span class="sxs-lookup"><span data-stu-id="611e4-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="611e4-227">**パラメーターに使用される Stock クラス**</span><span class="sxs-lookup"><span data-stu-id="611e4-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="611e4-228">**パラメーターを持つサーバーからメソッドの WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照してください](#wpfsl))。**</span><span class="sxs-lookup"><span data-stu-id="611e4-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="611e4-229">パラメーターの動的オブジェクトを指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="611e4-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="611e4-230">ジェネリック型としてパラメーターを指定する代わりに、`On`メソッドでは、動的オブジェクトとしてパラメーターを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="611e4-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="611e4-231">**サーバー コードのパラメーターを持つクライアント メソッドを呼び出す**</span><span class="sxs-lookup"><span data-stu-id="611e4-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="611e4-232">**パラメーターに使用される Stock クラス**</span><span class="sxs-lookup"><span data-stu-id="611e4-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="611e4-233">**パラメーターの動的オブジェクトを使用して、パラメーターを持つサーバーからメソッドの WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照してください](#wpfsl))。**</span><span class="sxs-lookup"><span data-stu-id="611e4-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="611e4-234">ハンドラーを削除する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-234">How to remove a handler</span></span>

<span data-ttu-id="611e4-235">ハンドラーを削除するには、呼び出すその`Dispose`メソッド。</span><span class="sxs-lookup"><span data-stu-id="611e4-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="611e4-236">**サーバーから呼び出されるメソッドのクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="611e4-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="611e4-237">**クライアントのコード ハンドラーを削除するには**</span><span class="sxs-lookup"><span data-stu-id="611e4-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="611e4-238">クライアントからサーバーのメソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="611e4-238">How to call server methods from the client</span></span>

<span data-ttu-id="611e4-239">サーバー上のメソッドを呼び出すを使用して、`Invoke`ハブ プロキシのメソッド。</span><span class="sxs-lookup"><span data-stu-id="611e4-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="611e4-240">サーバー メソッドが戻り値を持たない場合は、非ジェネリック オーバー ロードを使用して、`Invoke`メソッド。</span><span class="sxs-lookup"><span data-stu-id="611e4-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="611e4-241">**サーバー コードのメソッドの戻り値がありません。**</span><span class="sxs-lookup"><span data-stu-id="611e4-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="611e4-242">**戻り値を持たないメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="611e4-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="611e4-243">サーバー メソッドの戻り値の場合は、戻り値の型を指定のジェネリック型として、`Invoke`メソッド。</span><span class="sxs-lookup"><span data-stu-id="611e4-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="611e4-244">**サーバー コードのメソッドの戻り値があり、複雑な型パラメーターを受け取る**</span><span class="sxs-lookup"><span data-stu-id="611e4-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="611e4-245">**パラメーターと戻り値に使用される Stock クラス**</span><span class="sxs-lookup"><span data-stu-id="611e4-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="611e4-246">**ASP.NET 4.5 の非同期メソッドで複合型のパラメーターを受け取って戻り値を持つメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="611e4-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="611e4-247">**戻り値を同期メソッドで複合型のパラメーターを受け取るメソッドを呼び出すクライアント コード**</span><span class="sxs-lookup"><span data-stu-id="611e4-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="611e4-248">`Invoke`メソッドが非同期で実行し、返します、`Task`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="611e4-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="611e4-249">指定しない場合は`await`または`.Wait()`、呼び出すメソッドの実行が完了する前に次のコード行が実行されます。</span><span class="sxs-lookup"><span data-stu-id="611e4-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="611e4-250">接続の有効期間イベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="611e4-251">SignalR は、次の接続に処理できる有効期間イベントを提供します。</span><span class="sxs-lookup"><span data-stu-id="611e4-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="611e4-252">`Received`: 接続でのデータが受信したときに発生します。</span><span class="sxs-lookup"><span data-stu-id="611e4-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="611e4-253">受信したデータを提供します。</span><span class="sxs-lookup"><span data-stu-id="611e4-253">Provides the received data.</span></span>
- <span data-ttu-id="611e4-254">`ConnectionSlow`: クライアントが低速または削除が頻繁に接続を検出したときに発生します。</span><span class="sxs-lookup"><span data-stu-id="611e4-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="611e4-255">`Reconnecting`: 基になるトランスポートの再接続を開始するときに発生します。</span><span class="sxs-lookup"><span data-stu-id="611e4-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="611e4-256">`Reconnected`: 基になるトランスポートが再接続されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="611e4-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="611e4-257">`StateChanged`: 接続状態が変更されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="611e4-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="611e4-258">以前の状態と新しい状態を提供します。</span><span class="sxs-lookup"><span data-stu-id="611e4-258">Provides the old state and the new state.</span></span> <span data-ttu-id="611e4-259">接続に関する情報の状態の値を参照してください[ConnectionState 列挙](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="611e4-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="611e4-260">`Closed`: 接続が切断されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="611e4-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="611e4-261">たとえば、致命的ではありませんが、断続的な接続の問題が発生するエラーを警告メッセージを表示する場合は、パフォーマンスの低下や頻繁になど、接続の削除を処理、`ConnectionSlow`イベント。</span><span class="sxs-lookup"><span data-stu-id="611e4-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="611e4-262">詳細については、次を参照してください。 [SignalR の接続の有効期間イベントの処理と理解](../guide-to-the-api/handling-connection-lifetime-events.md)します。</span><span class="sxs-lookup"><span data-stu-id="611e4-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="611e4-263">エラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="611e4-263">How to handle errors</span></span>

<span data-ttu-id="611e4-264">場合は、サーバー上の詳細なエラー メッセージを明示的に有効にしない、SignalR が返すエラーが発生した例外オブジェクトには、エラーに関する最小限の情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="611e4-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="611e4-265">呼び出しなど`newContosoChatMessage`失敗した場合、エラー オブジェクトにエラー メッセージが含まれています"`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`"送信の詳細なエラー メッセージを有効にする場合は、セキュリティ上の理由の詳細なエラー メッセージを運用環境でクライアントには推奨されませんトラブルシューティングのため、サーバーで次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="611e4-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="611e4-266">SignalR が発生したエラーを処理するためのハンドラーを追加できる、`Error`接続オブジェクトのイベント。</span><span class="sxs-lookup"><span data-stu-id="611e4-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="611e4-267">メソッドの呼び出しからエラーを処理するには、try-catch ブロックでコードをラップします。</span><span class="sxs-lookup"><span data-stu-id="611e4-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="611e4-268">クライアント側のログ記録を有効にする方法</span><span class="sxs-lookup"><span data-stu-id="611e4-268">How to enable client-side logging</span></span>

<span data-ttu-id="611e4-269">クライアント側のログ記録を有効にするには設定、`TraceLevel`と`TraceWriter`接続オブジェクトのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="611e4-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="611e4-270">WPF、Silverlight、およびコンソール アプリケーションのコード サンプル、サーバーが呼び出すことができるクライアント メソッド</span><span class="sxs-lookup"><span data-stu-id="611e4-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="611e4-271">サーバーが呼び出すことができるクライアント メソッドを定義する前に示したコード サンプルは、WinRT クライアントに適用されます。</span><span class="sxs-lookup"><span data-stu-id="611e4-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="611e4-272">次のサンプルでは、WPF、Silverlight、およびコンソール アプリケーションのクライアントの同等のコードを表示します。</span><span class="sxs-lookup"><span data-stu-id="611e4-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="611e4-273">パラメーターなしのメソッド</span><span class="sxs-lookup"><span data-stu-id="611e4-273">Methods without parameters</span></span>

<span data-ttu-id="611e4-274">**パラメーターなしのサーバーから呼び出されるメソッドの WPF クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="611e4-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="611e4-275">**パラメーターなしのサーバーから呼び出されるメソッドの Silverlight クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="611e4-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="611e4-276">**メソッドのコンソール アプリケーションのクライアント コードは、パラメーターなしのサーバーから呼び出されます**</span><span class="sxs-lookup"><span data-stu-id="611e4-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="611e4-277">パラメーターの型を指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="611e4-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="611e4-278">**パラメーターを持つサーバーから呼び出されるメソッドの WPF クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="611e4-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="611e4-279">**パラメーターを持つサーバーから呼び出されるメソッドの Silverlight クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="611e4-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="611e4-280">**パラメーターを持つサーバーからメソッドのコンソール アプリケーションのクライアント コードが呼び出されます**</span><span class="sxs-lookup"><span data-stu-id="611e4-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="611e4-281">パラメーターの動的オブジェクトを指定するパラメーターを持つメソッド</span><span class="sxs-lookup"><span data-stu-id="611e4-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="611e4-282">**パラメーターの動的オブジェクトを使用して、パラメーターを持つサーバーから呼び出されるメソッドの WPF クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="611e4-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="611e4-283">**パラメーターの動的オブジェクトを使用して、パラメーターを持つサーバーから呼び出されるメソッドの Silverlight クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="611e4-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="611e4-284">**パラメーターの動的オブジェクトを使用して、パラメーターを持つサーバーからメソッドのコンソール アプリケーションのクライアント コードが呼び出されます**</span><span class="sxs-lookup"><span data-stu-id="611e4-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
