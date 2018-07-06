---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: ASP.NET SignalR ハブ API ガイド - サーバー (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: このドキュメントでは、コード サンプル demonstratin と version 1.1 SignalR for ASP.NET SignalR ハブの API のサーバー側のプログラミングを紹介しています.
ms.author: aspnetcontent
ms.date: 04/17/2013
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: 15d2f4e9eaff0d47a3bdce489dfc1d19177423f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826414"
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="f0c27-103">ASP.NET SignalR ハブ API ガイド - サーバー (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="f0c27-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="f0c27-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f0c27-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="f0c27-105">このドキュメントでは、一般的なオプションを示すコード サンプルを使用したバージョン 1.1 では、SignalR のサーバー側の ASP.NET SignalR ハブの API をプログラミングの概要を示します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="f0c27-106">SignalR ハブの API では、サーバーからに接続されているクライアントとサーバーのクライアントからのリモート プロシージャ コール (Rpc) を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="f0c27-107">サーバー コードで、クライアントから呼び出すことができるメソッドを定義して、クライアント上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="f0c27-108">クライアント コードで、サーバーから呼び出すことができるメソッドを定義して、サーバー上で実行されるメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="f0c27-109">SignalR は、のすべてのクライアントとサーバーが処理されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="f0c27-110">SignalR では、永続的な接続と呼ばれる下位レベル API も提供します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="f0c27-111">概要については、SignalR、ハブ、および永続的な接続は、または完全な SignalR アプリケーションを構築する方法を示すチュートリアルについてを参照してください。 [SignalR - Getting Started](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="f0c27-112">概要</span><span class="sxs-lookup"><span data-stu-id="f0c27-112">Overview</span></span>

<span data-ttu-id="f0c27-113">このドキュメントは、次のトピックに分かれています。</span><span class="sxs-lookup"><span data-stu-id="f0c27-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="f0c27-114">SignalR のルートを登録し、SignalR のオプションを構成する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="f0c27-115">/Signalr URL</span><span class="sxs-lookup"><span data-stu-id="f0c27-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="f0c27-116">SignalR のオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="f0c27-117">作成し、ハブ クラスを使用する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="f0c27-118">ハブ オブジェクトの有効期間</span><span class="sxs-lookup"><span data-stu-id="f0c27-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="f0c27-119">JavaScript クライアントではハブの名前のキャメル</span><span class="sxs-lookup"><span data-stu-id="f0c27-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="f0c27-120">複数のハブ</span><span class="sxs-lookup"><span data-stu-id="f0c27-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="f0c27-121">クライアントが呼び出すことができる、ハブ クラスにメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="f0c27-122">JavaScript クライアント内のメソッド名の大文字小文字のキャメル形式</span><span class="sxs-lookup"><span data-stu-id="f0c27-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="f0c27-123">非同期的に実行するタイミング</span><span class="sxs-lookup"><span data-stu-id="f0c27-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="f0c27-124">オーバー ロードを定義します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="f0c27-125">Hub クラスからのクライアント メソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="f0c27-126">クライアントを選択すると、RPC が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="f0c27-127">メソッド名のコンパイル時検証なし</span><span class="sxs-lookup"><span data-stu-id="f0c27-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="f0c27-128">大文字のメソッド名に一致します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="f0c27-129">非同期実行</span><span class="sxs-lookup"><span data-stu-id="f0c27-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="f0c27-130">Hub クラスからグループ メンバーシップを管理する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="f0c27-131">Add および Remove メソッドの非同期実行</span><span class="sxs-lookup"><span data-stu-id="f0c27-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="f0c27-132">グループ メンバーシップの永続化</span><span class="sxs-lookup"><span data-stu-id="f0c27-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="f0c27-133">シングル ユーザー グループ</span><span class="sxs-lookup"><span data-stu-id="f0c27-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="f0c27-134">ハブ クラスでの接続の有効期間イベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="f0c27-135">OnConnected、OnDisconnected、OnReconnected を呼び出すときに</span><span class="sxs-lookup"><span data-stu-id="f0c27-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="f0c27-136">設定されていない呼び出し元の状態</span><span class="sxs-lookup"><span data-stu-id="f0c27-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="f0c27-137">コンテキスト プロパティからのクライアントに関する情報を取得する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="f0c27-138">クライアントとハブ クラス間で状態を渡す方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="f0c27-139">ハブ クラスのエラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="f0c27-140">クライアントにメソッドを呼び出し、ハブ クラスの外部からグループを管理する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="f0c27-141">クライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="f0c27-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="f0c27-142">グループ メンバーシップを管理します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="f0c27-143">トレースを有効にする方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="f0c27-144">ハブのパイプラインをカスタマイズする方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="f0c27-145">クライアントのプログラム方法については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f0c27-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="f0c27-146">SignalR ハブ API ガイド - JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="f0c27-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="f0c27-147">SignalR ハブ API ガイド - .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="f0c27-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="f0c27-148">.NET 4.5 バージョンの API は API のリファレンス トピックへのリンクです。</span><span class="sxs-lookup"><span data-stu-id="f0c27-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="f0c27-149">.NET 4 を使用している場合は、次を参照してください。 [.NET 4 のバージョンを API のトピックの](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="f0c27-150">SignalR のルートを登録し、SignalR のオプションを構成する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="f0c27-151">クライアントがハブへの接続に使用するルートを定義するには、呼び出し、 [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx)メソッド、アプリケーションの起動時にします。</span><span class="sxs-lookup"><span data-stu-id="f0c27-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="f0c27-152">`MapHubs` [拡張メソッド](https://msdn.microsoft.com/library/vstudio/bb383977.aspx)の`System.Web.Routing.RouteCollection`クラス。</span><span class="sxs-lookup"><span data-stu-id="f0c27-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="f0c27-153">次の例は、SignalR ハブ ルートを定義する方法を示します、 *Global.asax*ファイル。</span><span class="sxs-lookup"><span data-stu-id="f0c27-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="f0c27-154">ASP.NET MVC アプリケーションに SignalR の機能を追加する場合は、SignalR のルートが、他のルートの前に追加されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="f0c27-155">詳細については、次を参照してください。[チュートリアル: SignalR と MVC 4 の概要](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="f0c27-156">/Signalr URL</span><span class="sxs-lookup"><span data-stu-id="f0c27-156">The /signalr URL</span></span>

<span data-ttu-id="f0c27-157">既定では、クライアントがハブへの接続に使用するルートの URL は"/signalr"。</span><span class="sxs-lookup"><span data-stu-id="f0c27-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="f0c27-158">(この URL は自動的に生成された JavaScript ファイルの「/signalr ハブ」の url を混同しないでください。</span><span class="sxs-lookup"><span data-stu-id="f0c27-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="f0c27-159">生成されたプロキシの詳細については、次を参照してください[SignalR ハブ API ガイド - JavaScript クライアントで生成されたプロキシとは何が](index.md)。)。</span><span class="sxs-lookup"><span data-stu-id="f0c27-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="f0c27-160">SignalR; に対しては使用できませんのベース URL を構成した特殊な状況である可能性があります。たとえば、という名前のプロジェクトのフォルダーがある*signalr*名前を変更するとします。</span><span class="sxs-lookup"><span data-stu-id="f0c27-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="f0c27-161">次の例に示すように、ベースの URL を変更する場合、(置換"/signalr"で、目的の URL を使用してサンプル コード)。</span><span class="sxs-lookup"><span data-stu-id="f0c27-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="f0c27-162">**サーバー コード、URL を指定します。**</span><span class="sxs-lookup"><span data-stu-id="f0c27-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="f0c27-163">**JavaScript クライアント コード (生成されたプロキシ) を使用して URL を指定します。**</span><span class="sxs-lookup"><span data-stu-id="f0c27-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="f0c27-164">**JavaScript クライアント コード (せずに、生成されたプロキシ) URL を指定します。**</span><span class="sxs-lookup"><span data-stu-id="f0c27-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="f0c27-165">**.NET クライアント コード、URL を指定します。**</span><span class="sxs-lookup"><span data-stu-id="f0c27-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="f0c27-166">SignalR のオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-166">Configuring SignalR Options</span></span>

<span data-ttu-id="f0c27-167">オーバー ロード、`MapHubs`メソッドを使用すると、カスタムの URL、カスタム依存関係競合回避モジュール、および、次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="f0c27-168">ブラウザー クライアントからのクロス ドメイン呼び出しを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f0c27-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="f0c27-169">通常、ブラウザーがからページを読み込む場合`http://contoso.com`、SignalR 接続が同じドメイン内では`http://contoso.com/signalr`します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="f0c27-170">場合、ページから`http://contoso.com`への接続は、`http://fabrikam.com/signalr`ドメイン間の接続されています。</span><span class="sxs-lookup"><span data-stu-id="f0c27-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="f0c27-171">セキュリティ上の理由から、ドメイン間の接続が既定で無効になります。</span><span class="sxs-lookup"><span data-stu-id="f0c27-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="f0c27-172">詳細については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - JavaScript クライアントのドメイン間の接続を確立する方法](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="f0c27-173">詳細なエラー メッセージを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f0c27-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="f0c27-174">エラーが発生すると、SignalR の既定の動作が発生した事象に関する詳細なしの通知メッセージをクライアントに送信するは。</span><span class="sxs-lookup"><span data-stu-id="f0c27-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="f0c27-175">詳細なエラー情報をクライアントに送信することは推奨されません、運用環境で悪意のあるユーザーは、アプリケーションに対する攻撃の情報を使用できない可能性があるため。</span><span class="sxs-lookup"><span data-stu-id="f0c27-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="f0c27-176">トラブルシューティングについては、一時的に詳しいエラー報告を有効にするのにこのオプションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="f0c27-177">自動的に生成された JavaScript プロキシ ファイルを無効にします。</span><span class="sxs-lookup"><span data-stu-id="f0c27-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="f0c27-178">既定では、URL「/signalr ハブ」への応答で、ハブ クラスのプロキシを持つ JavaScript ファイルが生成されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="f0c27-179">JavaScript プロキシを使用する場合、または手動でこのファイルを生成し、クライアントの物理ファイルを参照する場合は、プロキシの生成を無効にするのにこのオプションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="f0c27-180">詳細については、次を参照してください。 [SignalR ハブ API ガイド - JavaScript クライアント - SignalR の物理ファイルを作成する方法は、プロキシを生成](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="f0c27-181">次の例への呼び出しで SignalR 接続の URL とこれらのオプションを指定する方法を示しています、`MapHubs`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f0c27-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="f0c27-182">カスタム URL を指定するには、置き換える"/signalr"を使用する URL の例です。</span><span class="sxs-lookup"><span data-stu-id="f0c27-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="f0c27-183">作成し、ハブ クラスを使用する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-183">How to create and use Hub classes</span></span>

<span data-ttu-id="f0c27-184">ハブを作成するから派生したクラスを作成[Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="f0c27-185">次の例では、チャット アプリケーションの単純なハブ クラスを示します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="f0c27-186">この例では、接続されているクライアントが呼び出すことができます、`NewContosoChatMessage`メソッドでは、接続されているすべてのクライアントに受信したデータをブロードキャストするときと。</span><span class="sxs-lookup"><span data-stu-id="f0c27-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="f0c27-187">ハブ オブジェクトの有効期間</span><span class="sxs-lookup"><span data-stu-id="f0c27-187">Hub object lifetime</span></span>

<span data-ttu-id="f0c27-188">ハブ クラスをインスタンス化したり、サーバー上のコードからメソッドを呼び出すしません。これらすべてはの SignalR ハブ パイプラインによって行われます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="f0c27-189">SignalR は、ときに、クライアントが接続切断されると、またはサーバー メソッドの呼び出しを行ったなど、ハブの操作を処理する必要があるたびに、ハブ クラスの新しいインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="f0c27-190">ハブ クラスのインスタンスは、一時的なであるため、次の 1 つのメソッド呼び出しからの状態を維持するために使用できません。</span><span class="sxs-lookup"><span data-stu-id="f0c27-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="f0c27-191">たびに、サーバー メッセージを受信メソッド呼び出しをクライアントでは、ハブ クラスのプロセスの新しいインスタンス。</span><span class="sxs-lookup"><span data-stu-id="f0c27-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="f0c27-192">を介して複数の接続とメソッドの呼び出しの状態を維持するために、ハブ クラスまたは別のクラスから派生していないデータベース、または静的変数などの他のメソッドを使用して、`Hub`します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="f0c27-193">メモリ内のデータを永続化する場合、ハブ クラスに静的変数などのメソッドを使用して、データは失われますアプリ ドメインがリサイクルされる場合。</span><span class="sxs-lookup"><span data-stu-id="f0c27-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="f0c27-194">ハブ クラス外部で実行されるコードからクライアントにメッセージを送信する場合、ハブ クラスのインスタンスをインスタンス化して行うことはできませんが、ハブ クラスの SignalR コンテキスト オブジェクトへの参照を取得することによって行うことができます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="f0c27-195">詳細については、次を参照してください。[クライアント メソッドを呼び出すと、ハブ クラスの外部からグループを管理する方法](#callfromoutsidehub)このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="f0c27-196">JavaScript クライアントではハブの名前のキャメル</span><span class="sxs-lookup"><span data-stu-id="f0c27-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="f0c27-197">既定では、JavaScript クライアントがハブにクラス名の camel 形式のバージョンを使用して参照します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="f0c27-198">SignalR では、JavaScript コードは、JavaScript の規則に従うことができるように、この変更が自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="f0c27-199">前の例と呼ばれる`contosoChatHub`JavaScript コードにします。</span><span class="sxs-lookup"><span data-stu-id="f0c27-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="f0c27-200">**サーバー**</span><span class="sxs-lookup"><span data-stu-id="f0c27-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="f0c27-201">**生成されたプロキシを使用して JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="f0c27-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="f0c27-202">クライアントを使用して、追加するための別の名前を指定するかどうか、`HubName`属性。</span><span class="sxs-lookup"><span data-stu-id="f0c27-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="f0c27-203">使用すると、`HubName`属性は、JavaScript クライアントにキャメル ケースに名前の変更はありません。</span><span class="sxs-lookup"><span data-stu-id="f0c27-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="f0c27-204">**サーバー**</span><span class="sxs-lookup"><span data-stu-id="f0c27-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="f0c27-205">**生成されたプロキシを使用して JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="f0c27-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="f0c27-206">複数のハブ</span><span class="sxs-lookup"><span data-stu-id="f0c27-206">Multiple Hubs</span></span>

<span data-ttu-id="f0c27-207">アプリケーションでは、複数のハブ クラスを定義できます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="f0c27-208">そうと、接続は共有されますが、グループは別。</span><span class="sxs-lookup"><span data-stu-id="f0c27-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="f0c27-209">すべてのクライアントは、サービスと SignalR の接続を確立するために、同じ URL を使用 ("/signalr"またはいずれかを指定した場合、カスタムの URL)、すべてのハブの接続を使用して、サービスで定義されています。</span><span class="sxs-lookup"><span data-stu-id="f0c27-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="f0c27-210">比較する 1 つのクラスでハブのすべての機能を定義すると、複数のハブのパフォーマンスの違いはありません。</span><span class="sxs-lookup"><span data-stu-id="f0c27-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="f0c27-211">すべてのハブでは、同じ HTTP 要求情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="f0c27-212">すべてのハブが同じ接続を共有するため、サーバーを取得する HTTP 要求情報は、SignalR の接続を確立する元の HTTP 要求で何が。</span><span class="sxs-lookup"><span data-stu-id="f0c27-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="f0c27-213">クエリ文字列を指定することで、クライアントからサーバーに情報を渡すために、接続要求を使用する場合は、別のハブに別のクエリ文字列を提供できません。</span><span class="sxs-lookup"><span data-stu-id="f0c27-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="f0c27-214">すべてのハブと同じ情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="f0c27-215">JavaScript プロキシの生成されたファイルは、1 つのファイルですべてのハブ プロキシが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="f0c27-216">JavaScript プロキシの詳細については、次を参照してください。 [SignalR ハブ API ガイド - JavaScript クライアントで生成されたプロキシとは何が](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="f0c27-217">グループは、ハブ内で定義されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="f0c27-218">SignalR を定義することで接続されているクライアントのサブセットにブロードキャストするグループの名前。</span><span class="sxs-lookup"><span data-stu-id="f0c27-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="f0c27-219">グループは、各ハブとは別に保持されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="f0c27-220">たとえば、"Administrators"という名前のグループはクライアントの 1 つのセットを追加、`ContosoChatHub`クラスと同じグループ名は参照されている場合にクライアントを別のセットに、`StockTickerHub`クラス。</span><span class="sxs-lookup"><span data-stu-id="f0c27-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="f0c27-221">クライアントが呼び出すことができる、ハブ クラスにメソッドを定義する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="f0c27-222">クライアントから呼び出せるようにするハブのメソッドを公開するには、次の例に示すように、パブリック メソッドを宣言します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="f0c27-223">戻り値の型と複合型と配列を含む c# メソッドの場合と、パラメーターを指定できます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="f0c27-224">パラメーターが表示されるか、呼び出し元に返すデータは、JSON を使用して、クライアントとサーバー間通信し、SignalR は複雑なオブジェクトのバインドとオブジェクトの配列を自動的に処理します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="f0c27-225">JavaScript クライアント内のメソッド名の大文字小文字のキャメル形式</span><span class="sxs-lookup"><span data-stu-id="f0c27-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="f0c27-226">既定では、メソッド名の camel 形式のバージョンを使用して JavaScript クライアントがハブ メソッドを参照します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="f0c27-227">SignalR では、JavaScript コードは、JavaScript の規則に従うことができるように、この変更が自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="f0c27-228">**サーバー**</span><span class="sxs-lookup"><span data-stu-id="f0c27-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="f0c27-229">**生成されたプロキシを使用して JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="f0c27-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="f0c27-230">クライアントを使用して、追加するための別の名前を指定するかどうか、`HubMethodName`属性。</span><span class="sxs-lookup"><span data-stu-id="f0c27-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="f0c27-231">**サーバー**</span><span class="sxs-lookup"><span data-stu-id="f0c27-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="f0c27-232">**生成されたプロキシを使用して JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="f0c27-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="f0c27-233">非同期的に実行するタイミング</span><span class="sxs-lookup"><span data-stu-id="f0c27-233">When to execute asynchronously</span></span>

<span data-ttu-id="f0c27-234">データベース検索など、web サービス呼び出しの待機を含むのハブ メソッドを返すことによって非同期にする場合がある実行時間の長いメソッドまたは作業を実行する必要がありますは、[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)(の代わりに`void`を返す) または[タスク&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx)オブジェクト (の代わりに`T`型を返す)。</span><span class="sxs-lookup"><span data-stu-id="f0c27-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="f0c27-235">戻ると、`Task`オブジェクト、メソッドでは、SignalR を待ちます、`Task`を完了するし、そのラップ解除された結果、クライアントに戻るため、クライアントでメソッドの呼び出しをコーディングする方法に違いはありません。</span><span class="sxs-lookup"><span data-stu-id="f0c27-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="f0c27-236">ハブ メソッドを行う非同期回避 WebSocket トランスポートを使用する場合は、接続をブロックしています。</span><span class="sxs-lookup"><span data-stu-id="f0c27-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="f0c27-237">ハブ メソッドを同期的に実行すると、トランスポートが WebSocket のハブ メソッドが完了するまでに、同じクライアントからハブのメソッドの後続の呼び出しがブロックされます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="f0c27-238">次の例と同じメソッドが同期的に実行するコード化されたまたはいずれかのバージョンを呼び出すために動作する JavaScript クライアント コードの後に、非同期的を示します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="f0c27-239">**同期**</span><span class="sxs-lookup"><span data-stu-id="f0c27-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="f0c27-240">**非同期の ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="f0c27-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="f0c27-241">**生成されたプロキシを使用して JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="f0c27-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="f0c27-242">ASP.NET 4.5 で非同期メソッドを使用する方法の詳細については、次を参照してください。 [ASP.NET MVC 4 での非同期のメソッドを使用して](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="f0c27-243">オーバー ロードを定義します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-243">Defining Overloads</span></span>

<span data-ttu-id="f0c27-244">メソッドのオーバー ロードを定義する場合は、各オーバー ロードのパラメーターの数が異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f0c27-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="f0c27-245">さまざまなパラメーターの型を指定するだけでオーバー ロードを区別する場合は、ハブ クラスはコンパイルされますが、SignalR サービスはオーバー ロードのいずれかの呼び出しにクライアントがしようとするいると、実行時に例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="f0c27-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="f0c27-246">Hub クラスからのクライアント メソッドを呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="f0c27-247">サーバーからクライアント メソッドを呼び出すには、使用、`Clients`ハブ クラス内のメソッドのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f0c27-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="f0c27-248">次の例では、サーバー コードを呼び出す`addNewMessageToPage`接続されているすべてのクライアント、および JavaScript クライアント内でメソッドを定義するクライアント コードにします。</span><span class="sxs-lookup"><span data-stu-id="f0c27-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="f0c27-249">**サーバー**</span><span class="sxs-lookup"><span data-stu-id="f0c27-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="f0c27-250">**生成されたプロキシを使用して JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="f0c27-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="f0c27-251">クライアントのメソッドから戻り値を取得できません。などの構文`int x = Clients.All.add(1,1)`は機能しません。</span><span class="sxs-lookup"><span data-stu-id="f0c27-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="f0c27-252">複合型とパラメーターの配列を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="f0c27-253">次の例では、複合型をメソッドのパラメーターに、クライアントに渡します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="f0c27-254">**サーバー コードを複雑なオブジェクトを使用してクライアント メソッドを呼び出す**</span><span class="sxs-lookup"><span data-stu-id="f0c27-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="f0c27-255">**複雑なオブジェクトを定義するサーバー コード**</span><span class="sxs-lookup"><span data-stu-id="f0c27-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="f0c27-256">**生成されたプロキシを使用して JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="f0c27-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="f0c27-257">クライアントを選択すると、RPC が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="f0c27-258">クライアントのプロパティを返します、 [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) RPC 受信するクライアントを指定するためのいくつかのオプションを提供するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f0c27-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="f0c27-259">接続されているすべてのクライアント。</span><span class="sxs-lookup"><span data-stu-id="f0c27-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="f0c27-260">呼び出し元のクライアントのみです。</span><span class="sxs-lookup"><span data-stu-id="f0c27-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="f0c27-261">呼び出し元のクライアントを除くすべてのクライアント。</span><span class="sxs-lookup"><span data-stu-id="f0c27-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="f0c27-262">接続 ID で識別される特定のクライアント</span><span class="sxs-lookup"><span data-stu-id="f0c27-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="f0c27-263">この例では`addContosoChatMessageToPage`呼び出し元のクライアントで使用すると同じ効果があり`Clients.Caller`します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="f0c27-264">接続されているすべてのクライアント接続 ID で識別される、指定されたクライアントを除く</span><span class="sxs-lookup"><span data-stu-id="f0c27-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="f0c27-265">すべてには、指定したグループ内のクライアントが接続されています。</span><span class="sxs-lookup"><span data-stu-id="f0c27-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="f0c27-266">接続 ID で識別される、指定されたクライアントを除く、指定したグループに接続されているすべてのクライアント</span><span class="sxs-lookup"><span data-stu-id="f0c27-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="f0c27-267">指定したグループ内のすべての接続されているクライアントは、呼び出し元のクライアントを除きます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="f0c27-268">メソッド名のコンパイル時検証なし</span><span class="sxs-lookup"><span data-stu-id="f0c27-268">No compile-time validation for method names</span></span>

<span data-ttu-id="f0c27-269">指定したメソッド名は、IntelliSense またはコンパイル時検証がないことを意味の動的オブジェクトとして解釈されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="f0c27-270">式は、実行時に評価されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="f0c27-271">メソッドの呼び出しを実行するときにそのメソッドが呼び出される SignalR メソッド名とパラメーターの値をクライアントに送信し、名前に一致する場合は、クライアントがある、メソッド、およびパラメーターの値は渡されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="f0c27-272">クライアントに一致するメソッドが見つからない場合、エラーは発生しません。</span><span class="sxs-lookup"><span data-stu-id="f0c27-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="f0c27-273">SignalR は、クライアント メソッドを呼び出すと、バック グラウンドでクライアントに送信するデータの形式の詳細については、次を参照してください。 [SignalR 入門](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="f0c27-274">大文字のメソッド名に一致します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="f0c27-275">大文字と小文字が一致するメソッドの名前です。</span><span class="sxs-lookup"><span data-stu-id="f0c27-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="f0c27-276">たとえば、`Clients.All.addContosoChatMessageToPage`サーバーで実行`AddContosoChatMessageToPage`、 `addcontosochatmessagetopage`、または`addContosoChatMessageToPage`クライアント。</span><span class="sxs-lookup"><span data-stu-id="f0c27-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="f0c27-277">非同期実行</span><span class="sxs-lookup"><span data-stu-id="f0c27-277">Asynchronous execution</span></span>

<span data-ttu-id="f0c27-278">メソッドを呼び出すことは非同期的に実行します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="f0c27-279">SignalR の後続行のコードをすることを指定しない限り、クライアントにデータの送信を完了するを待たず、クライアントへのメソッド呼び出しはすぐに実行後に付属している任意のコードでは、メソッドの完了を待つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0c27-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="f0c27-280">クライアントの 2 つの方法を順番に実行する方法を表示する次のコード例を使用して .NET 4.5 で動作するコードを使用して .NET 4 で動作するコードです。</span><span class="sxs-lookup"><span data-stu-id="f0c27-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="f0c27-281">**.NET 4.5 の例**</span><span class="sxs-lookup"><span data-stu-id="f0c27-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="f0c27-282">**.NET 4 の例**</span><span class="sxs-lookup"><span data-stu-id="f0c27-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="f0c27-283">使用する場合`await`または`ContinueWith`クライアント メソッドは、次のコード行が実行される前に完了するまで待機するわけクライアントが実際にメッセージが表示される前に、次のコード行を実行します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="f0c27-284">のみのクライアント メソッドの呼び出しには、「完了」は、SignalR がすべてのメッセージを送信するために必要な終了することを意味します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="f0c27-285">クライアントがメッセージを受信することの確認が必要な場合、そのメカニズムを自分でプログラムがあります。</span><span class="sxs-lookup"><span data-stu-id="f0c27-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="f0c27-286">たとえば、コーディングする可能性があります、`MessageReceived`メソッドと、ハブで、`addContosoChatMessageToPage`メソッドを呼び出すことも、クライアントを`MessageReceived`その後はすべて使用するクライアントで実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0c27-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="f0c27-287">`MessageReceived`ハブでどのような作業は、実際のクライアントの受信と、元のメソッド呼び出しの処理によって異なります。 を実行できます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="f0c27-288">メソッドの名前として文字列変数を使用する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="f0c27-289">キャスト、メソッド名と文字列変数を使用してクライアント メソッドを呼び出す場合`Clients.All`(または`Clients.Others`、`Clients.Caller`など) に`IClientProxy`を呼び出して[Invoke (methodName, args…)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="f0c27-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="f0c27-290">Hub クラスからグループ メンバーシップを管理する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="f0c27-291">SignalR でグループは、接続されているクライアントのサブセットを指定するメッセージをブロードキャストする方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="f0c27-292">グループは、クライアントの任意の数を持つことができ、クライアントは任意の数のグループのメンバーであることができます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="f0c27-293">グループのメンバーシップを管理するを使用して、[追加](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)と[削除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)によって提供されるメソッド、`Groups`ハブ クラスのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f0c27-293">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="f0c27-294">次の例は、`Groups.Add`と`Groups.Remove`呼び出し元となる JavaScript クライアント コードの後に、クライアント コードによって呼び出されるハブ メソッドで使用されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="f0c27-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="f0c27-295">**サーバー**</span><span class="sxs-lookup"><span data-stu-id="f0c27-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="f0c27-296">**生成されたプロキシを使用して JavaScript クライアント**</span><span class="sxs-lookup"><span data-stu-id="f0c27-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="f0c27-297">グループを明示的に作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="f0c27-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="f0c27-298">有効なグループが初めての呼び出しでその名前を指定する自動的に作成`Groups.Add`、そのメンバーシップから最後の接続を削除すると削除されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="f0c27-299">グループ メンバーシップの一覧またはグループの一覧を取得するための API はありません。</span><span class="sxs-lookup"><span data-stu-id="f0c27-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="f0c27-300">SignalR では、クライアントとグループを基にメッセージを送信する[パブリッシュ/サブスクライブ モデル](http://en.wikipedia.org/wiki/Publish/subscribe)、し、サーバーがグループまたはグループ メンバーシップの一覧を保持しません。</span><span class="sxs-lookup"><span data-stu-id="f0c27-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="f0c27-301">こうため、SignalR を保持する任意の状態が新しいノードに適用するのには web ファームにノードを追加するたびに、スケーラビリティを最大化します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="f0c27-302">Add および Remove メソッドの非同期実行</span><span class="sxs-lookup"><span data-stu-id="f0c27-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="f0c27-303">`Groups.Add`と`Groups.Remove`メソッドが非同期的に実行します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="f0c27-304">クライアント グループを追加して、すぐに、グループを使用して、クライアントにメッセージを送信する場合は、ことを確認する必要がある、`Groups.Add`メソッドが最初に終了しました。</span><span class="sxs-lookup"><span data-stu-id="f0c27-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="f0c27-305">次のコード例は、その .NET 4.5、および .NET 4 で動作するコードを使用していずれかで動作するコードを使用して 1 つの方法を表示します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="f0c27-306">**.NET 4.5 の例**</span><span class="sxs-lookup"><span data-stu-id="f0c27-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="f0c27-307">**.NET 4 の例**</span><span class="sxs-lookup"><span data-stu-id="f0c27-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="f0c27-308">グループ メンバーシップの永続化</span><span class="sxs-lookup"><span data-stu-id="f0c27-308">Group membership persistence</span></span>

<span data-ttu-id="f0c27-309">SignalR 接続の追跡、ユーザーが同じグループにユーザーを確立するたびに接続するユーザーではなく、その場合を呼び出す必要が`Groups.Add`たびに、ユーザーが新しい接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="f0c27-310">接続の一時的な喪失、後に場合があります SignalR ことができます、接続を自動的に復元します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="f0c27-311">その場合は、SignalR は、同じ接続を復元は、新しい接続を確立できませんし、そのため、クライアントのグループのメンバーシップが自動的に復元します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="f0c27-312">これは可能なサーバーの再起動または失敗の結果が場合でも、一時的な中断がグループのメンバーシップを含む、各クライアントの接続状態をクライアントにラウンドト リップします。</span><span class="sxs-lookup"><span data-stu-id="f0c27-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="f0c27-313">サーバーが停止して、接続がタイムアウトする前に、新しいサーバーに置換される場合、クライアントは、新しいサーバーに自動的に再接続し、再登録するグループのメンバーであります。</span><span class="sxs-lookup"><span data-stu-id="f0c27-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="f0c27-314">接続は、接続の喪失後に自動的に復元できない場合にまたは接続がタイムアウトしたときに、(たとえば、ブラウザーは、新しいページに移動します) 場合、クライアントが切断されたときは、グループ メンバーシップは失われます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="f0c27-315">次に、ユーザーが接続したときは、新しい接続になります。</span><span class="sxs-lookup"><span data-stu-id="f0c27-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="f0c27-316">を、同じユーザーが新しい接続が確立されるときのグループ メンバーシップを維持するには、アプリケーションは、ユーザーとグループの間の関連付けを追跡し、ユーザーが新しい接続を確立するたびにグループ メンバーシップを復元するがします。</span><span class="sxs-lookup"><span data-stu-id="f0c27-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="f0c27-317">接続と再接続に関する詳細については、次を参照してください。[ハブ クラスでの接続の有効期間イベントを処理する方法](#connectionlifetime)このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="f0c27-318">シングル ユーザー グループ</span><span class="sxs-lookup"><span data-stu-id="f0c27-318">Single-user groups</span></span>

<span data-ttu-id="f0c27-319">どのユーザーがメッセージを送信し、どのユーザーにメッセージを受信する必要がありますを知るためには、ユーザーと接続間の関連付けを追跡する必要は通常、SignalR を使用するアプリケーション。</span><span class="sxs-lookup"><span data-stu-id="f0c27-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="f0c27-320">グループは、これを行うための 2 つの一般的に使用されるパターンのいずれかで使用されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="f0c27-321">シングル ユーザーのグループ。</span><span class="sxs-lookup"><span data-stu-id="f0c27-321">Single-user groups.</span></span>

    <span data-ttu-id="f0c27-322">グループ名としてユーザー名を指定でき、ユーザーが接続または再接続するたびに、現在の接続 ID をグループに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="f0c27-323">ユーザー、グループに送信するには、メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="f0c27-324">この方法の欠点は、グループを調べるかどうか、ユーザーはオンラインまたはオフラインにするに提供します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="f0c27-325">ユーザー名と接続 Id の間の関連付けを追跡します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="f0c27-326">ディクショナリまたはデータベース内の各ユーザー名と 1 つまたは複数の接続 Id 間のアソシエーションを格納し、ユーザーが接続または切断するたびに、格納されたデータを更新できます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="f0c27-327">ユーザーにメッセージを送信するには、接続 Id を指定します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="f0c27-328">この方法の欠点より多くのメモリがかかることです。</span><span class="sxs-lookup"><span data-stu-id="f0c27-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="f0c27-329">ハブ クラスでの接続の有効期間イベントを処理する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="f0c27-330">接続の有効期間イベントを処理するための一般的な原因としてとユーザー名と接続 Id 間の関連付けを追跡するか、ユーザーが接続されているかどうかを追跡するのには。</span><span class="sxs-lookup"><span data-stu-id="f0c27-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="f0c27-331">クライアントが接続または切断ときは、独自のコードを実行するには、オーバーライド、 `OnConnected`、 `OnDisconnected`、および`OnReconnected`ハブの仮想メソッドをクラスの次の例で示す。</span><span class="sxs-lookup"><span data-stu-id="f0c27-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="f0c27-332">OnConnected、OnDisconnected、OnReconnected を呼び出すときに</span><span class="sxs-lookup"><span data-stu-id="f0c27-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="f0c27-333">ブラウザーが新しいページに移動するたびに新しい接続でが確立されているため、SignalR は実行、`OnDisconnected`メソッドに続けて、`OnConnected`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f0c27-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="f0c27-334">SignalR は、新しい接続が確立されたときに常に新しい接続 ID を作成します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="f0c27-335">`OnReconnected`メソッドが呼び出されますが、一時中断 SignalR がときに、ケーブルが一時的に切断され、再接続、接続がタイムアウトする前になどから、回復できるように自動的に接続します。`OnDisconnected`クライアントを切断して SignalR 自動的に再接続できないなど、ブラウザーが新しいページに移動するときにメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="f0c27-336">そのため、特定のクライアントのイベントの可能なシーケンスは`OnConnected`、 `OnReconnected`、 `OnDisconnected`; または`OnConnected`、`OnDisconnected`します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="f0c27-337">シーケンスは表示されません`OnConnected`、 `OnDisconnected`、`OnReconnected`特定の接続。</span><span class="sxs-lookup"><span data-stu-id="f0c27-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="f0c27-338">`OnDisconnected`メソッドなど、サーバーがダウンしたときに、一部のシナリオで呼び出されませんまたはアプリケーション ドメインが再利用されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="f0c27-339">別のサーバーがオンラインにするか、アプリ ドメインのリサイクルが完了すると、一部のクライアントが再接続して起動できるようになることがあります、`OnReconnected`イベント。</span><span class="sxs-lookup"><span data-stu-id="f0c27-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="f0c27-340">詳細については、次を参照してください。 [SignalR の接続の有効期間イベントの処理と理解](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="f0c27-341">設定されていない呼び出し元の状態</span><span class="sxs-lookup"><span data-stu-id="f0c27-341">Caller state not populated</span></span>

<span data-ttu-id="f0c27-342">任意の状態に格納されていることを意味するサーバーからの接続の有効期間イベント ハンドラー メソッドが呼び出される、`state`で、クライアント上のオブジェクトは設定されません、`Caller`サーバーのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f0c27-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="f0c27-343">については、`state`オブジェクトと`Caller`プロパティを参照してください[クライアントとハブ クラス間で状態を渡す方法](#passstate)このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="f0c27-344">コンテキスト プロパティからのクライアントに関する情報を取得する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="f0c27-345">クライアントに関する情報を取得する、`Context`ハブ クラスのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f0c27-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="f0c27-346">`Context`プロパティが返す、 [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx)次の情報へのアクセスを提供するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f0c27-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="f0c27-347">呼び出し元のクライアントの接続 ID。</span><span class="sxs-lookup"><span data-stu-id="f0c27-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="f0c27-348">接続 ID は、SignalR (値は、独自のコードでは指定できません) によって割り当てられた GUID です。</span><span class="sxs-lookup"><span data-stu-id="f0c27-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="f0c27-349">同じ接続 ID は、アプリケーションで複数のハブがある場合、すべてのハブで使用し、各接続の 1 つの接続 ID があります。</span><span class="sxs-lookup"><span data-stu-id="f0c27-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="f0c27-350">HTTP ヘッダーのデータ。</span><span class="sxs-lookup"><span data-stu-id="f0c27-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="f0c27-351">HTTP ヘッダーを取得することもできます。`Context.Headers`します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="f0c27-352">多重参照を同じことには、その理由は`Context.Headers`が最初に、作成された、`Context.Request`プロパティが追加された後と`Context.Headers`旧バージョンと互換性が保持されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="f0c27-353">文字列データのクエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="f0c27-354">クエリの文字列データを取得することも`Context.QueryString`します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="f0c27-355">このプロパティで取得するクエリ文字列確立された SignalR 接続 HTTP 要求で使用されていたです。</span><span class="sxs-lookup"><span data-stu-id="f0c27-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="f0c27-356">クライアントでは、クライアントからサーバーにクライアントに関するデータを渡すに便利ですが、接続を構成することでクエリ文字列パラメーターを追加できます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="f0c27-357">次の例では、生成されたプロキシを使用すると、JavaScript クライアント内でクエリ文字列を追加する 1 つの方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="f0c27-358">クエリ文字列パラメーターを設定する方法についての詳細については、API ガイドを参照してください、 [JavaScript](index.md)と[.NET](index.md)クライアント。</span><span class="sxs-lookup"><span data-stu-id="f0c27-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="f0c27-359">SignalR によって内部的に使用されるその他のいくつかの値と共に、クエリ文字列のデータの接続に使用されるトランスポート メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="f0c27-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="f0c27-360">値`transportMethod`"Websocket"、"serverSentEvents"、"foreverFrame"または"longPolling"になります。</span><span class="sxs-lookup"><span data-stu-id="f0c27-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="f0c27-361">この値を確認する場合、`OnConnected`イベント ハンドラー メソッドでは、一部のシナリオで、接続のネゴシエートされたトランスポートの最終的なメソッドではないトランスポートの値を取得する可能性があります最初にします。</span><span class="sxs-lookup"><span data-stu-id="f0c27-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="f0c27-362">その場合は、メソッドは、例外がスローされ、最終的なトランスポート メソッドが確立されたときに後でもう一度呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="f0c27-363">クッキー。</span><span class="sxs-lookup"><span data-stu-id="f0c27-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="f0c27-364">Cookie を取得することもできます。`Context.RequestCookies`します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="f0c27-365">ユーザー情報。</span><span class="sxs-lookup"><span data-stu-id="f0c27-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="f0c27-366">要求の HttpContext オブジェクト:</span><span class="sxs-lookup"><span data-stu-id="f0c27-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="f0c27-367">このメソッドを使用して取得する代わりに`HttpContext.Current`を取得する、 `HttpContext` SignalR 接続のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f0c27-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="f0c27-368">クライアントとハブ クラス間で状態を渡す方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="f0c27-369">クライアント プロキシを提供する`state`オブジェクトの各メソッドの呼び出しで、サーバーに送信するデータを保存できます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="f0c27-370">サーバー上でこのデータにアクセスすることができます、`Clients.Caller`クライアントによって呼び出されるハブ メソッドでプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f0c27-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="f0c27-371">`Clients.Caller`プロパティは設定されませんが、接続の有効期間イベントのハンドラー メソッド`OnConnected`、 `OnDisconnected`、および`OnReconnected`します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="f0c27-372">作成または更新でのデータ、`state`オブジェクトと`Clients.Caller`プロパティが双方向で機能します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="f0c27-373">サーバーで値を更新して、クライアントに渡されます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="f0c27-374">次の例では、すべてのメソッド呼び出しで、サーバーに送信する状態を格納する JavaScript クライアント コードを示します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="f0c27-375">次の例では、.NET クライアントでの同等のコードを示します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="f0c27-376">ハブ クラス内でこのデータにアクセスすることができます、`Clients.Caller`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="f0c27-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="f0c27-377">次の例では、前の例で参照される状態を取得するコードを示します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="f0c27-378">状態を保持するためには、このメカニズムはありません大量のデータをすべてを配置した後、`state`または`Clients.Caller`プロパティは、メソッド呼び出しのたびにラウンド トリップします。</span><span class="sxs-lookup"><span data-stu-id="f0c27-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="f0c27-379">ユーザー名やカウンターなどの小さい項目と便利です。</span><span class="sxs-lookup"><span data-stu-id="f0c27-379">It's useful for smaller items such as user names or counters.</span></span>


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="f0c27-380">ハブ クラスのエラーを処理する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="f0c27-381">ハブ クラスのメソッドで発生するエラーを処理するには、次のメソッドの一方または両方を使用します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="f0c27-382">Try catch ブロックでは、メソッドのコードをラップし、例外オブジェクトを記録します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="f0c27-383">デバッグの目的では、クライアントに例外を送信できますが、セキュリティ上の理由から運用環境でクライアントに詳細な情報を送信することはお勧めしません。</span><span class="sxs-lookup"><span data-stu-id="f0c27-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="f0c27-384">処理するハブ パイプライン モジュールを作成、 [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx)メソッド。</span><span class="sxs-lookup"><span data-stu-id="f0c27-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="f0c27-385">次の例では、モジュールをハブ パイプラインに挿入する Global.asax 内のコードの後に、エラー ログに記録するパイプラインのモジュールを示します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="f0c27-386">ハブ パイプライン モジュールの詳細については、次を参照してください。[ハブ パイプラインをカスタマイズする方法について](#hubpipeline)このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="f0c27-387">トレースを有効にする方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-387">How to enable tracing</span></span>

<span data-ttu-id="f0c27-388">サーバー側トレースを有効にするには、この例で示すように、Web.config ファイルに system.diagnostics 要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="f0c27-389">ログを表示するには Visual Studio でアプリケーションを実行すると、**出力**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="f0c27-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="f0c27-390">クライアントにメソッドを呼び出し、ハブ クラスの外部からグループを管理する方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="f0c27-391">ハブ クラスよりも別のクラスからクライアント メソッドを呼び出すにハブの SignalR コンテキスト オブジェクトへの参照を取得およびクライアントのメソッドを呼び出すか、グループを管理するに使用します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="f0c27-392">次のサンプル`StockTicker`クラス コンテキスト オブジェクトを取得、クラスのインスタンスに格納、静的なプロパティで、クラスのインスタンスを格納およびシングルトン クラスのインスタンスからコンテキストを使用して呼び出す、`updateStockPrice`クライアント上のメソッドという名前のハブに接続されている`StockTickerHub`します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="f0c27-393">有効期間が長いオブジェクト コンテキストの複数回を使用する必要がある場合は、1 回の参照を取得しよりも、毎回の取得を保存します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="f0c27-394">コンテキストを 1 回取得すると、SignalR がメッセージをハブのメソッド、クライアントのメソッドの呼び出しと同じシーケンス内のクライアントに送信するようにします。</span><span class="sxs-lookup"><span data-stu-id="f0c27-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="f0c27-395">ハブの SignalR コンテキストを使用する方法を示すチュートリアルについては、次を参照してください。 [ASP.NET SignalR によるサーバー ブロードキャスト](index.md)します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="f0c27-396">クライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="f0c27-396">Calling client methods</span></span>

<span data-ttu-id="f0c27-397">RPC を受信するクライアントを指定できますが、ハブ クラスから呼び出す場合よりも少ないオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="f0c27-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="f0c27-398">この理由は、すべてのメソッドを必要とする現在の接続 ID のサポート技術情報など、コンテキストがクライアントから、特定の呼び出しに関連付けられていない`Clients.Others`、または`Clients.Caller`、または`Clients.OthersInGroup`は使用できません。</span><span class="sxs-lookup"><span data-stu-id="f0c27-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="f0c27-399">次のオプションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-399">The following options are available:</span></span>

- <span data-ttu-id="f0c27-400">接続されているすべてのクライアント。</span><span class="sxs-lookup"><span data-stu-id="f0c27-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="f0c27-401">接続 ID で識別される特定のクライアント</span><span class="sxs-lookup"><span data-stu-id="f0c27-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="f0c27-402">接続されているすべてのクライアント接続 ID で識別される、指定されたクライアントを除く</span><span class="sxs-lookup"><span data-stu-id="f0c27-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="f0c27-403">すべてには、指定したグループ内のクライアントが接続されています。</span><span class="sxs-lookup"><span data-stu-id="f0c27-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="f0c27-404">接続 ID で識別される、指定されたクライアントを除く、指定したグループに接続されているすべてのクライアント</span><span class="sxs-lookup"><span data-stu-id="f0c27-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="f0c27-405">呼び出す場合、非ハブ クラスにメソッドから、ハブ クラスで、現在の接続 ID を渡すし、を使用して`Clients.Client`、 `Clients.AllExcept`、または`Clients.Group`をシミュレートする`Clients.Caller`、 `Clients.Others`、または`Clients.OthersInGroup`します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="f0c27-406">次の例では、`MoveShapeHub`クラスに合格する接続 ID、`Broadcaster`クラスように、`Broadcaster`クラスをシミュレートできます`Clients.Others`します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="f0c27-407">グループ メンバーシップを管理します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-407">Managing group membership</span></span>

<span data-ttu-id="f0c27-408">グループの管理のハブ クラスの場合と同じオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="f0c27-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="f0c27-409">クライアントをグループに追加します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="f0c27-410">グループからのクライアントを削除します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="f0c27-411">ハブのパイプラインをカスタマイズする方法</span><span class="sxs-lookup"><span data-stu-id="f0c27-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="f0c27-412">SignalR では、独自のコードをハブ パイプラインに挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="f0c27-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="f0c27-413">次の例は、クライアントとクライアントで呼び出されるメソッド呼び出しの送信から受信した受信した各メソッド呼び出しのログは、カスタム ハブ パイプライン モジュールを示しています。</span><span class="sxs-lookup"><span data-stu-id="f0c27-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="f0c27-414">次のコードで、 *Global.asax*ファイルをハブ パイプラインで実行するモジュールを登録します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="f0c27-415">上書き可能なさまざまな方法があります。</span><span class="sxs-lookup"><span data-stu-id="f0c27-415">There are many different methods that you can override.</span></span> <span data-ttu-id="f0c27-416">完全な一覧についてを参照してください。 [HubPipelineModule メソッド](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="f0c27-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
