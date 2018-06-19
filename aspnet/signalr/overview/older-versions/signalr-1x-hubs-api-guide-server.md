---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: ASP.NET SignalR ハブ API ガイド - サーバー (SignalR 1.x) |Microsoft ドキュメント
author: pfletcher
description: このドキュメントでは、SignalR バージョン 1.1 では、コード サンプル demonstratin と for ASP.NET SignalR ハブ API のサーバー側のプログラミングを紹介しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: 96155b1c648e5f6092b3ba67a560197f86a593b9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044183"
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="06d6d-103">ASP.NET SignalR ハブ API ガイド - サーバー (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="06d6d-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="06d6d-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="06d6d-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="06d6d-105">このドキュメントでは、バージョン 1.1 では、SignalR の一般的なオプションを示すコード サンプルでの ASP.NET SignalR ハブ API のサーバー側のプログラミングの概要を示します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="06d6d-106">SignalR ハブ API では、クライアントを接続するようにサーバーおよびサーバーのクライアントからリモート プロシージャ コール (Rpc) を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="06d6d-107">サーバー コードで、クライアントから呼び出すことができるメソッドを定義して、クライアント上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="06d6d-108">クライアント コードで、サーバーから呼び出すことができるメソッドを定義して、サーバー上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="06d6d-109">すべてのクライアントからサーバーへの組み込みの SignalR によって行われます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="06d6d-110">SignalR では、永続的な接続と呼ばれる、下位の API も提供します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="06d6d-111">SignalR、ハブ、および永続的な接続の概要または完全な SignalR アプリケーションを構築する方法を示すチュートリアルについてを参照して[SignalR - はじめ](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="06d6d-112">概要</span><span class="sxs-lookup"><span data-stu-id="06d6d-112">Overview</span></span>

<span data-ttu-id="06d6d-113">このドキュメントは、次のトピックに分かれています。</span><span class="sxs-lookup"><span data-stu-id="06d6d-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="06d6d-114">SignalR のルートを登録し、SignalR のオプションを構成する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="06d6d-115">/Signalr URL</span><span class="sxs-lookup"><span data-stu-id="06d6d-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="06d6d-116">SignalR のオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="06d6d-117">作成し、ハブ クラスを使用する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="06d6d-118">ハブ オブジェクトの有効期間</span><span class="sxs-lookup"><span data-stu-id="06d6d-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="06d6d-119">JavaScript クライアントにハブ名の camel 形式の大文字と小文字</span><span class="sxs-lookup"><span data-stu-id="06d6d-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="06d6d-120">複数のハブ</span><span class="sxs-lookup"><span data-stu-id="06d6d-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="06d6d-121">クライアントが呼び出すことができる、ハブ クラスでメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="06d6d-122">JavaScript クライアントでメソッド名の camel 形式の大文字と小文字</span><span class="sxs-lookup"><span data-stu-id="06d6d-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="06d6d-123">非同期的に実行するタイミング</span><span class="sxs-lookup"><span data-stu-id="06d6d-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="06d6d-124">オーバー ロードを定義します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="06d6d-125">クライアントに、ハブ クラスからのメソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="06d6d-126">どのクライアントを選択すると、RPC が表示されます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="06d6d-127">メソッド名のないコンパイル時の検証</span><span class="sxs-lookup"><span data-stu-id="06d6d-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="06d6d-128">大文字と小文字のメソッドの名前の一致</span><span class="sxs-lookup"><span data-stu-id="06d6d-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="06d6d-129">非同期の実行</span><span class="sxs-lookup"><span data-stu-id="06d6d-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="06d6d-130">ハブ クラスからグループのメンバーシップを管理する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="06d6d-131">Add および Remove メソッドの非同期実行</span><span class="sxs-lookup"><span data-stu-id="06d6d-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="06d6d-132">グループ メンバーシップの永続化</span><span class="sxs-lookup"><span data-stu-id="06d6d-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="06d6d-133">シングル ユーザー グループ</span><span class="sxs-lookup"><span data-stu-id="06d6d-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="06d6d-134">ハブ クラスでの接続の有効期間のイベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="06d6d-135">OnConnected、OnDisconnected、および OnReconnected が呼び出されるタイミング</span><span class="sxs-lookup"><span data-stu-id="06d6d-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="06d6d-136">呼び出し元の状態値を返さない</span><span class="sxs-lookup"><span data-stu-id="06d6d-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="06d6d-137">コンテキスト プロパティからのクライアントに関する情報を取得する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="06d6d-138">クライアントと、ハブ クラス間で状態を渡す方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="06d6d-139">ハブ クラスのエラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="06d6d-140">クライアントのメソッドを呼び出すし、ハブ クラスの外部からグループを管理する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="06d6d-141">クライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="06d6d-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="06d6d-142">グループ メンバーシップを管理します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="06d6d-143">トレースを有効にする方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="06d6d-144">ハブ パイプラインをカスタマイズする方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="06d6d-145">クライアントのプログラム方法については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="06d6d-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="06d6d-146">SignalR ハブ API ガイド - JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="06d6d-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="06d6d-147">SignalR ハブ API ガイド - .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="06d6d-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="06d6d-148">API リファレンス トピックへのリンクは、.NET 4.5 のバージョンの API といます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="06d6d-149">.NET 4 を使用している場合は、次を参照してください。[の API のトピックは、.NET 4 バージョン](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="06d6d-150">SignalR のルートを登録し、SignalR のオプションを構成する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="06d6d-151">クライアントがハブへの接続に使用するルートを定義するには、呼び出し、 [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx)メソッド アプリケーションの起動時にします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="06d6d-152">`MapHubs`[拡張メソッド](https://msdn.microsoft.com/library/vstudio/bb383977.aspx)の`System.Web.Routing.RouteCollection`クラスです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="06d6d-153">次の例の SignalR ハブ ルートを定義する方法を示しています、 *Global.asax*ファイル。</span><span class="sxs-lookup"><span data-stu-id="06d6d-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="06d6d-154">ASP.NET MVC アプリケーションに SignalR 機能を追加する場合は、SignalR のルートが他のルートの前に追加されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="06d6d-155">詳細については、次を参照してください。[チュートリアル: SignalR と MVC 4 の概要](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="06d6d-156">/Signalr URL</span><span class="sxs-lookup"><span data-stu-id="06d6d-156">The /signalr URL</span></span>

<span data-ttu-id="06d6d-157">クライアントがハブに接続に使用するルートの URL は、既定では、"/signalr"です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="06d6d-158">(これは、自動的に生成された JavaScript ファイルの「/signalr ハブ」URL では、この URL を混同しないでください。</span><span class="sxs-lookup"><span data-stu-id="06d6d-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="06d6d-159">生成されたプロキシの詳細については、次を参照してください[SignalR ハブ API ガイド - JavaScript クライアントで生成されたプロキシとそれが何を](index.md)。)。</span><span class="sxs-lookup"><span data-stu-id="06d6d-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="06d6d-160">SignalR; のこのベース URL を使用できないようにする特殊な状況である可能性があります。たとえば、同じ名前のプロジェクトのフォルダーがある*signalr*名前を変更するとします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="06d6d-161">次の例に示すように、ベース URL を変更する場合は、(交換"/signalr"を目的の URL のサンプル コードで)。</span><span class="sxs-lookup"><span data-stu-id="06d6d-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="06d6d-162">**サーバー コード、URL を指定します。**</span><span class="sxs-lookup"><span data-stu-id="06d6d-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="06d6d-163">**JavaScript クライアント コード (生成されたプロキシ) を使用して URL を指定します。**</span><span class="sxs-lookup"><span data-stu-id="06d6d-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="06d6d-164">**JavaScript クライアント コード (せずに、生成されたプロキシ) URL を指定します。**</span><span class="sxs-lookup"><span data-stu-id="06d6d-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="06d6d-165">**.NET クライアント コード、URL を指定します。**</span><span class="sxs-lookup"><span data-stu-id="06d6d-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="06d6d-166">SignalR のオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-166">Configuring SignalR Options</span></span>

<span data-ttu-id="06d6d-167">オーバー ロードが、`MapHubs`メソッドを使用すると、カスタムの URL、カスタムの依存関係競合回避モジュール、および、次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="06d6d-168">ブラウザー クライアントからドメイン間の呼び出しを有効にします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="06d6d-169">通常、ブラウザーからページを読み込む場合`http://contoso.com`、SignalR 接続は、同じドメインに`http://contoso.com/signalr`です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="06d6d-170">場合、ページから`http://contoso.com`への接続は、`http://fabrikam.com/signalr`ドメイン間の接続されています。</span><span class="sxs-lookup"><span data-stu-id="06d6d-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="06d6d-171">セキュリティ上の理由から、ドメイン間の接続は、既定で無効にします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="06d6d-172">詳細については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - JavaScript クライアントのドメイン間の接続を確立する方法](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="06d6d-173">詳細なエラー メッセージを有効にします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="06d6d-174">エラーが発生すると、SignalR の既定の動作の変更点に関する詳細情報なしの通知メッセージをクライアントに送信を開始します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="06d6d-175">詳細なエラー情報をクライアントに送信されてために推奨されません、実稼働環境で悪意のあるユーザーは、アプリケーションに対する攻撃の情報を使用できる場合があります。</span><span class="sxs-lookup"><span data-stu-id="06d6d-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="06d6d-176">トラブルシューティングについては、一時的にもっとわかりやすいエラー報告を有効にするのには、このオプションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="06d6d-177">自動的に生成された JavaScript プロキシ ファイルを無効にします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="06d6d-178">既定では、プロキシの Hub クラス用の JavaScript ファイルが「/signalr ハブ」URL への応答で生成されます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="06d6d-179">JavaScript プロキシを使用しない場合、またはをこのファイルを手動で生成し、クライアントの物理ファイルを参照する場合は、プロキシの生成を無効にするのには、このオプションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="06d6d-180">詳細については、次を参照してください。 [SignalR ハブ API ガイド - JavaScript クライアントの SignalR の物理ファイルを作成する方法については、プロキシを生成](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="06d6d-181">次の例への呼び出しで、URL の SignalR 接続とこれらのオプションを指定する方法を示しています、`MapHubs`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="06d6d-182">カスタムの URL を指定するには、次のように置換します。"/signalr"を使用する URL の例です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="06d6d-183">作成し、ハブ クラスを使用する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-183">How to create and use Hub classes</span></span>

<span data-ttu-id="06d6d-184">ハブを作成するから派生するクラスを作成[Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="06d6d-185">次の例では、チャット アプリケーション用の単純なハブ クラスを示します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="06d6d-186">この例では、接続されたクライアントが呼び出すことができます、`NewContosoChatMessage`メソッド、受信したデータが接続されているすべてのクライアントにブロードキャストするときとします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="06d6d-187">ハブ オブジェクトの有効期間</span><span class="sxs-lookup"><span data-stu-id="06d6d-187">Hub object lifetime</span></span>

<span data-ttu-id="06d6d-188">ハブ クラスのインスタンスを作成したり、サーバーで、独自のコードからメソッドを呼び出しますしません。すべてが完了するの SignalR ハブ パイプラインによってです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="06d6d-189">SignalR は、ハブなどの操作とクライアント接続、切断されると、により、サーバーに呼び出すメソッドを処理する必要があるたびに、ハブ クラスの新しいインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="06d6d-190">ハブ クラスのインスタンスは、一時的なものであるためには 1 つのメソッド呼び出しからの状態を維持するために使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="06d6d-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="06d6d-191">たびに、サーバー メッセージを受信メソッド呼び出し、クライアントでは、ハブ クラスのプロセスの新しいインスタンス。</span><span class="sxs-lookup"><span data-stu-id="06d6d-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="06d6d-192">状態を保持する複数の接続とメソッドの呼び出しを通じて、ハブ クラス、または別のクラスから派生していないことに、データベース、または静的変数などの他の方法を使用して`Hub`です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="06d6d-193">メモリ内のデータを保存する場合、ハブ クラスに静的変数などのメソッドを使用して、データは失われますアプリケーション ドメインがリサイクルされるときにします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="06d6d-194">ハブ クラス外部で実行されるコードからクライアントにメッセージを送信する場合は、ハブ クラスのインスタンスをインスタンス化して行うことはできませんが、ハブ クラスの SignalR コンテキスト オブジェクトへの参照を取得することによって行うことができます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="06d6d-195">詳細については、次を参照してください。[クライアント メソッドを呼び出すと、ハブ クラスの外部からグループを管理する方法](#callfromoutsidehub)このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="06d6d-196">JavaScript クライアントにハブ名の camel 形式の大文字と小文字</span><span class="sxs-lookup"><span data-stu-id="06d6d-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="06d6d-197">既定では、JavaScript クライアントはクラス名の camel 形式のバージョンを使用してハブを参照します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="06d6d-198">SignalR は、JavaScript コードが JavaScript の規則に従うことができるように、この変更を自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="06d6d-199">前の例と呼ばれる`contosoChatHub`JavaScript コードにします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="06d6d-200">**サーバー**</span><span class="sxs-lookup"><span data-stu-id="06d6d-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="06d6d-201">**生成されたプロキシを使用して、JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="06d6d-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="06d6d-202">クライアントを使用して、追加する別の名前を指定するかどうか、`HubName`属性。</span><span class="sxs-lookup"><span data-stu-id="06d6d-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="06d6d-203">使用すると、`HubName`属性は、JavaScript クライアントでのキャメル ケースに名前の変更はありません。</span><span class="sxs-lookup"><span data-stu-id="06d6d-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="06d6d-204">**サーバー**</span><span class="sxs-lookup"><span data-stu-id="06d6d-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="06d6d-205">**生成されたプロキシを使用して、JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="06d6d-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="06d6d-206">複数のハブ</span><span class="sxs-lookup"><span data-stu-id="06d6d-206">Multiple Hubs</span></span>

<span data-ttu-id="06d6d-207">アプリケーションでは、複数の Hub クラスを定義できます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="06d6d-208">操作を実行すると、接続の共有が、グループは別。</span><span class="sxs-lookup"><span data-stu-id="06d6d-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="06d6d-209">すべてのクライアントは、サービスとの SignalR 接続を確立するために、同じ URL を使用 ("/signalr"またはいずれかを指定した場合、カスタムの URL)、すべてのハブの接続を使用して、サービスで定義されています。</span><span class="sxs-lookup"><span data-stu-id="06d6d-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="06d6d-210">1 つのクラスのすべてのハブ機能の定義と比較して複数のハブのパフォーマンスの違いはありません。</span><span class="sxs-lookup"><span data-stu-id="06d6d-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="06d6d-211">すべてのハブでは、同じ HTTP 要求の情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="06d6d-212">すべてのハブが同じ接続を共有するため、サーバーを取得する唯一の HTTP 要求情報は、SignalR 接続を確立する元の HTTP 要求で行うことです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="06d6d-213">クエリ文字列を指定することによって、サーバーにクライアントから情報を渡すため、接続要求を使用する場合は、さまざまなハブに別のクエリ文字列を提供できません。</span><span class="sxs-lookup"><span data-stu-id="06d6d-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="06d6d-214">すべてのハブでは、同じ情報を受信します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="06d6d-215">生成された JavaScript プロキシ ファイルは、1 つのファイルですべてのハブ プロキシが含まれます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="06d6d-216">JavaScript プロキシの詳細については、次を参照してください。 [SignalR ハブ API ガイド - JavaScript クライアントで生成されたプロキシとそれが何を](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="06d6d-217">グループは、ハブ内で定義されます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="06d6d-218">SignalR を定義することで接続しているクライアントのサブセットにブロードキャストするグループの名前。</span><span class="sxs-lookup"><span data-stu-id="06d6d-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="06d6d-219">グループは、各ハブを別々 に管理されます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="06d6d-220">たとえば、"Administrators"をという名前のグループは 1 つのクライアントのセットを追加、`ContosoChatHub`クラス、および同じグループ名が別のクライアントのセットを参照してください、`StockTickerHub`クラスです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="06d6d-221">クライアントが呼び出すことができる、ハブ クラスでメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="06d6d-222">クライアントから呼び出せるようにするハブのメソッドを公開するには、次の例に示すように、パブリック メソッドを宣言します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="06d6d-223">戻り値の型と複合型アレイなど、任意の c# のメソッドの場合と、パラメーターを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="06d6d-224">パラメーターで受信するか、呼び出し元に返すデータは、JSON を使用して、クライアントとサーバー間でやり取りされ、SignalR は複雑なオブジェクトのバインドとオブジェクトの配列を自動的に処理します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="06d6d-225">JavaScript クライアントでメソッド名の camel 形式の大文字と小文字</span><span class="sxs-lookup"><span data-stu-id="06d6d-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="06d6d-226">既定では、JavaScript クライアントは、メソッド名の camel 形式のバージョンを使用してハブ メソッドを参照します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="06d6d-227">SignalR は、JavaScript コードが JavaScript の規則に従うことができるように、この変更を自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="06d6d-228">**サーバー**</span><span class="sxs-lookup"><span data-stu-id="06d6d-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="06d6d-229">**生成されたプロキシを使用して、JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="06d6d-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="06d6d-230">クライアントを使用して、追加する別の名前を指定するかどうか、`HubMethodName`属性。</span><span class="sxs-lookup"><span data-stu-id="06d6d-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="06d6d-231">**サーバー**</span><span class="sxs-lookup"><span data-stu-id="06d6d-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="06d6d-232">**生成されたプロキシを使用して、JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="06d6d-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="06d6d-233">非同期的に実行するタイミング</span><span class="sxs-lookup"><span data-stu-id="06d6d-233">When to execute asynchronously</span></span>

<span data-ttu-id="06d6d-234">メソッドがある実行時間の長いまたは作業を実行するにする場合はデータベースの参照など、web サービス呼び出しの待機を伴うを返すことによってのハブ メソッドを非同期に作成、[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)(の代わりに`void`を返す) または[タスク&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx)オブジェクト (の代わりに`T`型を返す)。</span><span class="sxs-lookup"><span data-stu-id="06d6d-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="06d6d-235">戻るとき、`Task`メソッド、SignalR からオブジェクトを待って、`Task`完了するにはし、そのラップ解除された結果、クライアントに戻すため、クライアントのメソッド呼び出しをコーディングする方法の違いはありません。</span><span class="sxs-lookup"><span data-stu-id="06d6d-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="06d6d-236">ハブ メソッド非同期回避 WebSocket トランスポートを使用する場合は、接続をブロックします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="06d6d-237">ハブ メソッドが同期的に実行すると、トランスポートは、WebSocket、ハブのメソッドが完了するまで、同じクライアントからハブのメソッドの後続の呼び出しはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="06d6d-238">例を次に、同じメソッドが同期的に実行するようにコーディングまたはいずれかのバージョンを呼び出すための動作をクライアント コードを JavaScript の後に、非同期的にします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="06d6d-239">**同期**</span><span class="sxs-lookup"><span data-stu-id="06d6d-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="06d6d-240">**Asynchronous - ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="06d6d-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="06d6d-241">**生成されたプロキシを使用して、JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="06d6d-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="06d6d-242">ASP.NET 4.5 での非同期メソッドを使用する方法の詳細については、次を参照してください。 [ASP.NET MVC 4 での非同期のメソッドを使用して](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="06d6d-243">オーバー ロードを定義します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-243">Defining Overloads</span></span>

<span data-ttu-id="06d6d-244">メソッドのオーバー ロードを定義する場合は、各オーバー ロードのパラメーターの数が異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="06d6d-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="06d6d-245">別のパラメーターの型を指定するだけでオーバー ロードを区別する場合は、ハブ クラスがコンパイルされるが、SignalR サービス呼び出しのオーバー ロードのいずれかをクライアントのとき、実行時に例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="06d6d-246">クライアントに、ハブ クラスからのメソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="06d6d-247">サーバーからクライアント メソッドを呼び出すには、使用、`Clients`ハブ クラスにメソッドのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="06d6d-248">次の例では、サーバー コードを呼び出す`addNewMessageToPage`接続されているすべてのクライアント、および JavaScript クライアントで、メソッドを定義するクライアント コードにします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="06d6d-249">**サーバー**</span><span class="sxs-lookup"><span data-stu-id="06d6d-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="06d6d-250">**生成されたプロキシを使用して、JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="06d6d-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="06d6d-251">クライアント メソッドから戻り値を取得することはできません。などの構文`int x = Clients.All.add(1,1)`は機能しません。</span><span class="sxs-lookup"><span data-stu-id="06d6d-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="06d6d-252">複合型およびパラメーターの配列を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="06d6d-253">次の例では、複合型をメソッド パラメーターに、クライアントに渡します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="06d6d-254">**複雑なオブジェクトを使用してクライアントのメソッドを呼び出すサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="06d6d-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="06d6d-255">**複雑なオブジェクトを定義するサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="06d6d-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="06d6d-256">**生成されたプロキシを使用して、JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="06d6d-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="06d6d-257">どのクライアントを選択すると、RPC が表示されます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="06d6d-258">クライアントのプロパティから返される、 [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) RPC を受信するクライアントを指定するためのいくつかのオプションを提供するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="06d6d-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="06d6d-259">接続されているすべてのクライアントです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="06d6d-260">呼び出し元のクライアントのみです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="06d6d-261">呼び出し元のクライアントを除くすべてのクライアントです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="06d6d-262">接続 ID で識別される特定のクライアント</span><span class="sxs-lookup"><span data-stu-id="06d6d-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="06d6d-263">この例では`addContosoChatMessageToPage`呼び出し元のクライアントで使用すると同じ効果があり`Clients.Caller`です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="06d6d-264">接続 ID で識別される、指定されたクライアントを除くすべての接続されたクライアント</span><span class="sxs-lookup"><span data-stu-id="06d6d-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="06d6d-265">指定したグループ内のすべての接続されたクライアント。</span><span class="sxs-lookup"><span data-stu-id="06d6d-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="06d6d-266">接続 ID で識別される、指定されたクライアントを除く、指定したグループ内のすべての接続されたクライアント</span><span class="sxs-lookup"><span data-stu-id="06d6d-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="06d6d-267">指定されたグループ内のすべての接続されているクライアントは、呼び出し元のクライアントを除きます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="06d6d-268">メソッド名のないコンパイル時の検証</span><span class="sxs-lookup"><span data-stu-id="06d6d-268">No compile-time validation for method names</span></span>

<span data-ttu-id="06d6d-269">指定したメソッド名は、動的なオブジェクトは、IntelliSense またはのコンパイル時の検証がないことを意味として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="06d6d-270">式は、実行時に評価されます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="06d6d-271">メソッドの呼び出しを実行するときは、メソッドが呼び出される SignalR メソッド名とパラメーターの値をクライアントに送信し、名前に一致する場合は、クライアントは、メソッド、およびパラメーターの値がそれに渡されます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="06d6d-272">クライアントで一致するメソッドが見つからない場合、エラーは発生しません。</span><span class="sxs-lookup"><span data-stu-id="06d6d-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="06d6d-273">SignalR はクライアント メソッドを呼び出すと、シーンの背後にあるクライアントに送信するデータの形式については、次を参照してください。 [SignalR の概要](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="06d6d-274">大文字と小文字のメソッドの名前の一致</span><span class="sxs-lookup"><span data-stu-id="06d6d-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="06d6d-275">メソッド名に一致する小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="06d6d-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="06d6d-276">たとえば、`Clients.All.addContosoChatMessageToPage`サーバーで実行`AddContosoChatMessageToPage`、 `addcontosochatmessagetopage`、または`addContosoChatMessageToPage`クライアントにします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="06d6d-277">非同期の実行</span><span class="sxs-lookup"><span data-stu-id="06d6d-277">Asynchronous execution</span></span>

<span data-ttu-id="06d6d-278">呼び出すメソッドは、非同期的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="06d6d-279">SignalR の後続の行のコードをすることを指定しない限り、クライアントにデータの送信を完了するを待たずクライアントへのメソッド呼び出しはすぐに実行した後に付属しているすべてのコードは、メソッドの完了を待つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="06d6d-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="06d6d-280">次のコード例を次の 2 つのクライアント メソッドを順番に実行する方法を示してを使用してコードの .NET 4.5 で動作する、1 つを使用して .NET 4 での動作をコードします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="06d6d-281">**.NET 4.5 の例**</span><span class="sxs-lookup"><span data-stu-id="06d6d-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="06d6d-282">**.NET 4 の例**</span><span class="sxs-lookup"><span data-stu-id="06d6d-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="06d6d-283">使用する場合`await`または`ContinueWith`クライアント メソッドは、次のコード行が実行される前に完了するまで待機する、必ずしもクライアントが実際にメッセージが表示される次のコード行が実行される前にします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="06d6d-284">「完了」のクライアント メソッド呼び出しのでは、SignalR がすべてのメッセージを送信するために必要な終了、のみを意味します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="06d6d-285">クライアントがメッセージを受信することの確認を必要がある場合は、自分でこのメカニズムをプログラミングする必要があります。</span><span class="sxs-lookup"><span data-stu-id="06d6d-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="06d6d-286">コードでしたなど、`MessageReceived`メソッドと、ハブで、`addContosoChatMessageToPage`メソッドを呼び出すことも、クライアントを`MessageReceived`操作を行った後のクライアントで行う作業する必要です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="06d6d-287">`MessageReceived`ハブでどのような作業は、実際のクライアントの受信と、元のメソッド呼び出しの処理によって異なります。 を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="06d6d-288">メソッドの名前として文字列変数を使用する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="06d6d-289">キャスト メソッドの名前として文字列変数を使用してクライアント メソッドを呼び出す場合`Clients.All`(または`Clients.Others`、`Clients.Caller`など) に`IClientProxy`し[Invoke (methodName、args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="06d6d-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="06d6d-290">ハブ クラスからグループのメンバーシップを管理する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="06d6d-291">SignalR でグループは、接続しているクライアントの指定されたサブセットにメッセージをブロードキャストする方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="06d6d-292">グループは、クライアントの任意の数を持つことができ、クライアントは任意の数のグループのメンバーであることができます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="06d6d-293">グループ メンバーシップを管理するを使用して、[追加](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)と[削除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)によって提供されるメソッド、`Groups`ハブ クラスのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-293">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="06d6d-294">次の例は、`Groups.Add`と`Groups.Remove`これらを呼び出す JavaScript クライアント コードでクライアント コードによって呼び出されるハブ メソッドで使用されるメソッドの後にします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="06d6d-295">**サーバー**</span><span class="sxs-lookup"><span data-stu-id="06d6d-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="06d6d-296">**生成されたプロキシを使用して、JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="06d6d-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="06d6d-297">グループを明示的に作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="06d6d-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="06d6d-298">有効で、グループが初めてへの呼び出しでその名前を指定する自動的に作成`Groups.Add`、内のメンバーシップから最後の接続を削除するときに削除されます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="06d6d-299">グループ メンバーシップの一覧またはグループの一覧を取得するための API はありません。</span><span class="sxs-lookup"><span data-stu-id="06d6d-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="06d6d-300">SignalR クライアントおよびグループを基にメッセージを送信する、[パブリッシュ/サブスクライブ モデル](http://en.wikipedia.org/wiki/Publish/subscribe)、し、サーバーがグループまたはグループ メンバーシップの一覧を管理しません。</span><span class="sxs-lookup"><span data-stu-id="06d6d-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="06d6d-301">これにより、スケーラビリティを最大化 SignalR を保持するいずれかの状態は、新しいノードに反映する必要が web ファームにノードを追加するたびにできます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="06d6d-302">Add および Remove メソッドの非同期実行</span><span class="sxs-lookup"><span data-stu-id="06d6d-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="06d6d-303">`Groups.Add`と`Groups.Remove`メソッドが非同期的に実行します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="06d6d-304">クライアントをグループに追加し、すぐに、グループを使用して、クライアントにメッセージを送信する場合は、ことを確認する必要が、`Groups.Add`メソッドが最初に終了します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="06d6d-305">次のコード例は、表示を行うには、.NET 4.5、および .NET 4 で動作するコードを使用していずれかで動作するコードを使用して 1 つの方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="06d6d-306">**.NET 4.5 の例**</span><span class="sxs-lookup"><span data-stu-id="06d6d-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="06d6d-307">**.NET 4 の例**</span><span class="sxs-lookup"><span data-stu-id="06d6d-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="06d6d-308">グループ メンバーシップの永続化</span><span class="sxs-lookup"><span data-stu-id="06d6d-308">Group membership persistence</span></span>

<span data-ttu-id="06d6d-309">SignalR 接続を追跡する、接続のように、同じグループにユーザーを確立するたびにユーザーが必要なユーザーではなく、その場合を呼び出す必要がある`Groups.Add`たびに、ユーザーが新しい接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="06d6d-310">接続の一時的な障害は後、場合もあります SignalR ことができます、接続を自動的に復元します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="06d6d-311">その場合は、SignalR は、同じ接続を復元は、新しい接続を確立できませんし、そのため、クライアントのグループ メンバーシップは自動的に回復します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="06d6d-312">これには、一時的な中断が、サーバーを再起動または障害の結果が場合でも、グループ メンバーシップを含む、各クライアントの接続状態は、ラウンドト リップをクライアントにあるため。</span><span class="sxs-lookup"><span data-stu-id="06d6d-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="06d6d-313">場合は、サーバーがダウンしては、接続がタイムアウトする前に、新しいサーバーで置き換えられます、クライアントは、新しいサーバーに自動的に再接続し、グループのメンバーであることに再登録します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="06d6d-314">接続は、接続が失われた後に自動的に復元できませんまたは接続がタイムアウトしたときにしたりすると、クライアントには、(たとえば、ときに、ブラウザーは、新しいページに移動) が切断されると、グループ メンバーシップは失われます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="06d6d-315">次回ユーザーが接続を新しい接続となります。</span><span class="sxs-lookup"><span data-stu-id="06d6d-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="06d6d-316">同じユーザーが新しい接続を確立するときに、グループ メンバーシップを維持アプリケーションに、ユーザーとグループ間の関連付けを追跡し、ユーザーが新しい接続を確立するたびにグループ メンバーシップを復元します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="06d6d-317">接続と再接続に関する詳細については、次を参照してください。[ハブ クラスでの接続の有効期間のイベントを処理する方法](#connectionlifetime)このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="06d6d-318">シングル ユーザー グループ</span><span class="sxs-lookup"><span data-stu-id="06d6d-318">Single-user groups</span></span>

<span data-ttu-id="06d6d-319">通常 SignalR を使用するアプリケーションは、ユーザーはメッセージを送信し、どのユーザーにメッセージを受信する必要がありますを知るためには、ユーザーとの接続間の関連付けの追跡に必要です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="06d6d-320">グループは、これを行うため、2 つの一般的に使用されるパターンのいずれかで使用されます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="06d6d-321">シングル ユーザー グループ。</span><span class="sxs-lookup"><span data-stu-id="06d6d-321">Single-user groups.</span></span>

    <span data-ttu-id="06d6d-322">グループ名とユーザー名を指定し、ユーザーの接続または再接続するたびに、現在の接続 ID をグループに追加できます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="06d6d-323">グループに送信するユーザーにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="06d6d-324">この方法の欠点は、グループしないかどうか、ユーザーはオンラインまたはオフラインを検索する方法に提供です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="06d6d-325">ユーザー名と接続 Id の間の関連付けを追跡します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="06d6d-326">各ユーザー名と 1 つまたは複数の接続 Id の関連付けを辞書またはデータベースに格納し、ユーザーの接続または切断するたびに、格納されたデータを更新できます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="06d6d-327">ユーザーにメッセージを送信するには、接続 Id を指定します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="06d6d-328">この方法の欠点より多くのメモリを受け取ることです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="06d6d-329">ハブ クラスでの接続の有効期間のイベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="06d6d-330">接続の有効期間イベントを処理するための一般的な原因として、または、ユーザーが接続されているかどうかを追跡するユーザー名と接続 Id 間の関連付けを追跡するためです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="06d6d-331">クライアントが接続または切断ときは、独自のコードを実行するには、上書き、 `OnConnected`、 `OnDisconnected`、および`OnReconnected`仮想メソッド、ハブのクラスに、次の例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="06d6d-332">OnConnected、OnDisconnected、および OnReconnected が呼び出されるタイミング</span><span class="sxs-lookup"><span data-stu-id="06d6d-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="06d6d-333">ブラウザーが新しいページに移動するたびに、新しい接続が確立され、SignalR を実行することを意味、`OnDisconnected`メソッドに続けて、`OnConnected`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="06d6d-334">SignalR は、新しい接続が確立されたときに常に新しい接続 ID を作成します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="06d6d-335">`OnReconnected`メソッドが呼び出されてがあった一時中断 SignalR がときに、ケーブルが一時的に切断され、接続がタイムアウトする前に再接続などから、回復できるように自動的に接続します。`OnDisconnected`メソッドは、クライアントが切断され、SignalR 自動的に再接続できません、ブラウザーが新しいページに移動する場合などです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="06d6d-336">したがって、可能な一連の特定のクライアントのイベントは`OnConnected`、 `OnReconnected`、 `OnDisconnected`; または`OnConnected`、`OnDisconnected`です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="06d6d-337">シーケンスは表示されません`OnConnected`、 `OnDisconnected`、`OnReconnected`特定の接続。</span><span class="sxs-lookup"><span data-stu-id="06d6d-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="06d6d-338">`OnDisconnected`サーバーがダウンした場合など、一部のシナリオで呼び出されないメソッドまたはアプリケーション ドメインがリサイクルされるを取得します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="06d6d-339">別のサーバーがオンラインまたはアプリケーション ドメインには、そのリサイクルが完了すると、一部のクライアントできる場合がありますに再接続して、起動、`OnReconnected`イベント。</span><span class="sxs-lookup"><span data-stu-id="06d6d-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="06d6d-340">詳細については、次を参照してください。 [SignalR で接続の有効期間イベントの処理と理解](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="06d6d-341">呼び出し元の状態値を返さない</span><span class="sxs-lookup"><span data-stu-id="06d6d-341">Caller state not populated</span></span>

<span data-ttu-id="06d6d-342">いずれかの状態に格納されていることを意味すると、サーバーからの接続の有効期間のイベント ハンドラー メソッドが呼び出される、`state`で、クライアント上のオブジェクトは設定されません、`Caller`サーバーのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="06d6d-343">については、`state`オブジェクトおよび`Caller`プロパティを参照してください[クライアントと、ハブ クラス間で状態を渡す方法](#passstate)このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="06d6d-344">コンテキスト プロパティからのクライアントに関する情報を取得する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="06d6d-345">クライアントに関する情報を取得する、`Context`ハブ クラスのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="06d6d-346">`Context`プロパティから返される、 [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx)次の情報へのアクセスを提供するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="06d6d-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="06d6d-347">呼び出し元のクライアントの接続 ID。</span><span class="sxs-lookup"><span data-stu-id="06d6d-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="06d6d-348">接続 ID は、SignalR (値は、独自のコードでは指定できません) によって割り当てられた GUID です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="06d6d-349">アプリケーションに複数のハブがある場合、ID がすべてのハブで使用されて、同じ接続し、各接続の 1 つの接続 ID があります。</span><span class="sxs-lookup"><span data-stu-id="06d6d-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="06d6d-350">HTTP ヘッダーのデータ。</span><span class="sxs-lookup"><span data-stu-id="06d6d-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="06d6d-351">HTTP ヘッダーを取得することもできます。`Context.Headers`です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="06d6d-352">多重参照を同じことには、その理由は`Context.Headers`が最初に、作成された、`Context.Request`プロパティが、後で追加されたと`Context.Headers`旧バージョンとの互換性のために保持されました。</span><span class="sxs-lookup"><span data-stu-id="06d6d-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="06d6d-353">文字列データのクエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="06d6d-354">クエリの文字列データを取得することも`Context.QueryString`します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="06d6d-355">このプロパティで取得するクエリ文字列は、SignalR 接続が確立される HTTP 要求に使用されたものです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="06d6d-356">クライアントでのクエリ文字列パラメーターを追加するには、クライアントからサーバーにクライアントに関するデータを渡すための便利な方法であると、接続を構成します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="06d6d-357">次の例では、生成されたプロキシを使用すると、JavaScript クライアントで、クエリ文字列を追加する 1 つの方法を示します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="06d6d-358">クエリ文字列パラメーターの設定に関する詳細については、の API のマニュアルを参照して、 [JavaScript](index.md)と[.NET](index.md)クライアント。</span><span class="sxs-lookup"><span data-stu-id="06d6d-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="06d6d-359">SignalR によって内部的に使用されるその他のいくつかの値と共に、クエリ文字列のデータの接続に使用するトランスポート メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="06d6d-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="06d6d-360">値`transportMethod`"Websocket"、"serverSentEvents"、"foreverFrame"または"longPolling"になります。</span><span class="sxs-lookup"><span data-stu-id="06d6d-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="06d6d-361">この値を確認する場合、`OnConnected`イベント ハンドラー メソッドでは、一部のシナリオで、接続の最終的なネゴシエートされたトランスポート メソッドではないトランスポート値を取得する可能性があります最初にします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="06d6d-362">その場合はこのメソッドは例外をスローし、最終的なトランスポート メソッドが確立されたときに後でもう一度呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="06d6d-363">Cookie。</span><span class="sxs-lookup"><span data-stu-id="06d6d-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="06d6d-364">Cookie を取得することもできます。`Context.RequestCookies`です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="06d6d-365">ユーザーの情報。</span><span class="sxs-lookup"><span data-stu-id="06d6d-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="06d6d-366">要求の HttpContext オブジェクト:</span><span class="sxs-lookup"><span data-stu-id="06d6d-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="06d6d-367">取得する代わりにこのメソッドを使用して`HttpContext.Current`を取得する、 `HttpContext` SignalR 接続のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="06d6d-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="06d6d-368">クライアントと、ハブ クラス間で状態を渡す方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="06d6d-369">クライアント プロキシは、提供、`state`オブジェクトの各メソッド呼び出しを使用してサーバーに送信するデータを格納できます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="06d6d-370">サーバー上でこのデータにアクセスすることができます、`Clients.Caller`クライアントによって呼び出されるハブ メソッドでのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="06d6d-371">`Clients.Caller`接続の有効期間のイベント ハンドラー メソッドのプロパティは設定されません`OnConnected`、 `OnDisconnected`、および`OnReconnected`です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="06d6d-372">作成または内のデータを更新する、`state`オブジェクトおよび`Clients.Caller`プロパティが双方向で動作します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="06d6d-373">サーバーで値を更新することができ、それらのクライアントに渡されます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="06d6d-374">次の例では、すべてのメソッド呼び出しを使用してサーバーに送信する状態を格納する JavaScript クライアント コードを示します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="06d6d-375">次の例では、.NET クライアントで同等のコードを示します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="06d6d-376">を、ハブ クラス内でこのデータにアクセスすることができます、`Clients.Caller`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="06d6d-377">次の例では、前の例で参照される状態を取得するコードを示します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="06d6d-378">このメカニズムの状態を保持するものではありません、データの量が多い場合以降にするすべてのもの、`state`または`Clients.Caller`プロパティは、ラウンドト リップ メソッド呼び出しのたびにします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="06d6d-379">ユーザー名またはカウンターなどの小さい項目に対して便利です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-379">It's useful for smaller items such as user names or counters.</span></span>


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="06d6d-380">ハブ クラスのエラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="06d6d-381">ハブ クラスのメソッドで発生したエラーを処理するには、次のメソッドの一方または両方を使用します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="06d6d-382">Try catch ブロックでは、メソッドのコードをラップし、例外オブジェクトを記録します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="06d6d-383">デバッグの目的では、クライアントに例外を送信できますが、セキュリティ上の理由から実稼働環境でクライアントに詳細な情報を送信はお勧めしません。</span><span class="sxs-lookup"><span data-stu-id="06d6d-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="06d6d-384">処理するハブ パイプライン モジュールを作成、 [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx)メソッドです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="06d6d-385">次の例では、モジュールでは、ハブ パイプラインに挿入 Global.asax でコードを続けて、エラー ログに記録するパイプライン モジュールを示します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="06d6d-386">ハブ パイプライン モジュールに関する詳細については、次を参照してください。[をハブ パイプラインをカスタマイズする方法](#hubpipeline)このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="06d6d-387">トレースを有効にする方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-387">How to enable tracing</span></span>

<span data-ttu-id="06d6d-388">サーバー側トレースを有効にするには、この例のように、Web.config ファイルに system.diagnostics 要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="06d6d-389">内のログを表示するには Visual Studio で、アプリケーションを実行すると、**出力**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="06d6d-390">クライアントのメソッドを呼び出すし、ハブ クラスの外部からグループを管理する方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="06d6d-391">ハブ クラスよりも別のクラスからのクライアント メソッドを呼び出す、ハブの SignalR コンテキスト オブジェクトへの参照を取得を使用してクライアントのメソッドを呼び出すか、グループを管理します。、</span><span class="sxs-lookup"><span data-stu-id="06d6d-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="06d6d-392">次のサンプル`StockTicker`クラス コンテキスト オブジェクトを取得、クラスのインスタンスに格納、静的なプロパティで、クラスのインスタンスを格納およびシングルトン クラスのインスタンスからコンテキストを使用して呼び出す、`updateStockPrice`なっているクライアントでメソッドという名前のハブに接続されている`StockTickerHub`です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="06d6d-393">有効期間が長いオブジェクト内でコンテキストの複数回使用する必要がある場合は、1 回の参照を取得し、ことはなく、毎回取得を保存します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="06d6d-394">コンテキストを 1 回取得とは、SignalR が、同じシーケンスをハブ メソッドの機能によってクライアント メソッド呼び出し内のクライアントにメッセージを送信することを確認します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="06d6d-395">コンテキストを使用して、SignalR ハブの方法を示すチュートリアルでは、次を参照してください。[サーバーは、ASP.NET SignalR でブロードキャスト](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="06d6d-396">クライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="06d6d-396">Calling client methods</span></span>

<span data-ttu-id="06d6d-397">RPC を受信するクライアントを指定することができますが、ハブ クラスから呼び出す場合よりも少ないオプションです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="06d6d-398">その理由は、コンテキストがある、クライアントから特定の呼び出しに関連付けられた任意のメソッドを必要とする現在の接続 ID のサポートなど、 `Clients.Others`、または`Clients.Caller`、または`Clients.OthersInGroup`、ご利用いただけません。</span><span class="sxs-lookup"><span data-stu-id="06d6d-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="06d6d-399">次のオプションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-399">The following options are available:</span></span>

- <span data-ttu-id="06d6d-400">接続されているすべてのクライアントです。</span><span class="sxs-lookup"><span data-stu-id="06d6d-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="06d6d-401">接続 ID で識別される特定のクライアント</span><span class="sxs-lookup"><span data-stu-id="06d6d-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="06d6d-402">接続 ID で識別される、指定されたクライアントを除くすべての接続されたクライアント</span><span class="sxs-lookup"><span data-stu-id="06d6d-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="06d6d-403">指定したグループ内のすべての接続されたクライアント。</span><span class="sxs-lookup"><span data-stu-id="06d6d-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="06d6d-404">接続 ID で識別される、指定されたクライアントを除く、指定したグループ内のすべての接続されたクライアント</span><span class="sxs-lookup"><span data-stu-id="06d6d-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="06d6d-405">呼び出す場合、非ハブ クラスにメソッドからハブ クラスに、現在の接続 id を渡すし、でそれを使用`Clients.Client`、 `Clients.AllExcept`、または`Clients.Group`をシミュレートする`Clients.Caller`、 `Clients.Others`、または`Clients.OthersInGroup`です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="06d6d-406">次の例で、`MoveShapeHub`クラスに合格する接続 ID、`Broadcaster`クラスできるように、`Broadcaster`クラスをシミュレートできます`Clients.Others`です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="06d6d-407">グループ メンバーシップを管理します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-407">Managing group membership</span></span>

<span data-ttu-id="06d6d-408">グループを管理するがある同じオプション ハブ クラスの場合とします。</span><span class="sxs-lookup"><span data-stu-id="06d6d-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="06d6d-409">クライアントをグループに追加します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="06d6d-410">クライアントをグループから削除します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="06d6d-411">ハブ パイプラインをカスタマイズする方法</span><span class="sxs-lookup"><span data-stu-id="06d6d-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="06d6d-412">SignalR では、独自のコードをハブ パイプラインに挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="06d6d-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="06d6d-413">次の例では、クライアントとクライアントで呼び出された送信メソッド呼び出しから受信した各受信メソッド呼び出しをログに記録するカスタム ハブ パイプライン モジュールを示します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="06d6d-414">次のコードで、 *Global.asax*ファイルをハブ パイプラインで実行するモジュールを登録します。</span><span class="sxs-lookup"><span data-stu-id="06d6d-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="06d6d-415">オーバーライドできるさまざまな方法があります。</span><span class="sxs-lookup"><span data-stu-id="06d6d-415">There are many different methods that you can override.</span></span> <span data-ttu-id="06d6d-416">完全な一覧についてを参照してください。 [HubPipelineModule メソッド](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="06d6d-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
