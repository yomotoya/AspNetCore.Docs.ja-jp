---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana サンプル |Microsoft ドキュメント
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 815355c00c9c15cfefa5f98dc89da676743b0390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034491"
---
<a name="katana-samples"></a><span data-ttu-id="cdfc8-102">Katana サンプル</span><span class="sxs-lookup"><span data-stu-id="cdfc8-102">Katana Samples</span></span>
====================
<span data-ttu-id="cdfc8-103">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="cdfc8-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="cdfc8-104">Katana サンプル</span><span class="sxs-lookup"><span data-stu-id="cdfc8-104">Katana Samples</span></span>

<span data-ttu-id="cdfc8-105">**ASP.NET ルーティング サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="cdfc8-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="cdfc8-106">一部のアプリケーションでは、非 OWIN コンポーネントのサイド バイ サイドでの Asp.Net ルート テーブル内の OWIN コンポーネントをフックするためにすることができます。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="cdfc8-107">このサンプルでは、MapOwinPath と Microsoft.Owin.Host.SystemWeb によって提供される MapOwinRoute RouteCollection 拡張メソッドを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="cdfc8-108">**サンプル パイプラインを分岐して判別** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="cdfc8-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="cdfc8-109">OWIN 要求処理パイプラインは線形である必要はありませんは、さまざまな方法で要求の処理が分岐された時間ことができます。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="cdfc8-110">このサンプルでは、要求のパスやヘッダーなどの他の要求データに基づいて分岐パイプラインを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="cdfc8-111">これらのコンポーネントは Microsoft.Owin.Mapping nuget パッケージで使用できます。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="cdfc8-112">**カスタム サーバー サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="cdfc8-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="cdfc8-113">セルフホストされているときに、カスタムの OWIN サーバーを使用する方法を示します OWIN です。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="cdfc8-114">**サンプルを埋め込み** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="cdfc8-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="cdfc8-115">OWIN サーバーによっては、独自のプロセス内で実行できます (&quot;セルフホスト&quot;)。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="cdfc8-116">このサンプルでは、OWIN を Microsoft.Owin.Hosting nuget パッケージによって提供されるツールを使用してアプリケーションを起動する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="cdfc8-117">**HelloWorld サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="cdfc8-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="cdfc8-118">OWIN は、HTTP サーバーをさまざまなサーバー間でアプリケーションの移植性を有効にする API の抽象化です。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="cdfc8-119">このサンプルがいくつかを使用して、Hello World アプリケーションを記述する方法を示します**単純なラッパー** ASP.NET などの web サーバー上に、生の OWIN の抽象化と実行を囲む。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="cdfc8-120">**生の OWIN の hello World サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="cdfc8-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="cdfc8-121">このサンプルを使用して Hello World アプリケーションを記述する方法、**生**OWIN の抽象化し、Asp.Net のような web サーバーで実行します。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="cdfc8-122">**SignalR サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="cdfc8-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="cdfc8-123">OWIN を使用して SignalR を自己ホストする方法を示します/Katana です。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="cdfc8-124">自己ホストの SignalR の詳細については、次を参照してください。[チュートリアル: 自己ホスト SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)です。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="cdfc8-125">**静的ファイルのサンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="cdfc8-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="cdfc8-126">OWIN を使用して静的ファイルの HTTP 要求をサポートする方法を示します/Katana です。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="cdfc8-127">**Web API** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="cdfc8-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="cdfc8-128">このサンプルでは、IIS で OWIN をホストし、Web API OWIN パイプラインを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="cdfc8-129">**Web ソケット サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="cdfc8-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="cdfc8-130">OWIN でを使用して Web ソケットをサポートする方法を示しています、 [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx)クラスです。</span><span class="sxs-lookup"><span data-stu-id="cdfc8-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
