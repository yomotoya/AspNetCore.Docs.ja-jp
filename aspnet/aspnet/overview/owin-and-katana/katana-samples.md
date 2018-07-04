---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana サンプル |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: e81da1e650d8dfd24a3e0fda6aa42b7f360ce12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393320"
---
<a name="katana-samples"></a><span data-ttu-id="a5732-102">Katana サンプル</span><span class="sxs-lookup"><span data-stu-id="a5732-102">Katana Samples</span></span>
====================
<span data-ttu-id="a5732-103">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a5732-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="a5732-104">Katana サンプル</span><span class="sxs-lookup"><span data-stu-id="a5732-104">Katana Samples</span></span>

<span data-ttu-id="a5732-105">**ASP.NET ルーティング サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="a5732-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="a5732-106">一部のアプリケーションでは、非 OWIN コンポーネントと共に、Asp.Net ルート テーブル内の OWIN コンポーネントをフックするでしょう。</span><span class="sxs-lookup"><span data-stu-id="a5732-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="a5732-107">このサンプルでは、MapOwinPath と Microsoft.Owin.Host.SystemWeb によって提供される MapOwinRoute RouteCollection 拡張メソッドを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a5732-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="a5732-108">**サンプル パイプラインを分岐** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="a5732-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="a5732-109">OWIN 要求処理パイプラインが線形にする必要はありません、それらをさまざまな方法で要求を処理する分岐できます。</span><span class="sxs-lookup"><span data-stu-id="a5732-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="a5732-110">このサンプルでは、要求のパスやヘッダーなどの他の要求データに基づいて分岐のパイプラインを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a5732-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="a5732-111">これらのコンポーネント、Microsoft.Owin.Mapping nuget パッケージで利用できます。</span><span class="sxs-lookup"><span data-stu-id="a5732-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="a5732-112">**カスタム サーバー サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="a5732-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="a5732-113">自己ホストするときに、カスタムの OWIN サーバーを使用する方法を示しています。 OWIN します。</span><span class="sxs-lookup"><span data-stu-id="a5732-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="a5732-114">**サンプルの埋め込み** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="a5732-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="a5732-115">OWIN サーバーによっては、独自のプロセス内で実行できます (&quot;セルフホステッド&quot;)。</span><span class="sxs-lookup"><span data-stu-id="a5732-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="a5732-116">このサンプルでは、Microsoft.Owin.Hosting nuget パッケージによって提供されるツールを使用して、OWIN アプリケーションを起動する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a5732-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="a5732-117">**HelloWorld サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="a5732-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="a5732-118">OWIN は、HTTP サーバーのさまざまなサーバー間でアプリケーションの移植性をできるようにする抽象化された API です。</span><span class="sxs-lookup"><span data-stu-id="a5732-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="a5732-119">このサンプルは、いくつかを使用して Hello World アプリケーションを作成する方法を示します。**単純なラッパー** ASP.NET などの web サーバーで、生の OWIN 抽象化と実行を囲む。</span><span class="sxs-lookup"><span data-stu-id="a5732-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="a5732-120">**Hello World の生の OWIN サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="a5732-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="a5732-121">このサンプルを使用して Hello World アプリケーションを記述する方法、**生**OWIN 抽象化し、Asp.Net などの web サーバーで実行します。</span><span class="sxs-lookup"><span data-stu-id="a5732-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="a5732-122">**SignalR サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="a5732-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="a5732-123">OWIN を使用して SignalR を自己ホストする方法を示します/Katana します。</span><span class="sxs-lookup"><span data-stu-id="a5732-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="a5732-124">SignalR の自己ホストの詳細については、次を参照してください。[チュートリアル: SignalR セルフホスト](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)します。</span><span class="sxs-lookup"><span data-stu-id="a5732-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="a5732-125">**静的ファイルのサンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="a5732-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="a5732-126">OWIN を使用して静的ファイルの HTTP 要求をサポートする方法を示します/Katana します。</span><span class="sxs-lookup"><span data-stu-id="a5732-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="a5732-127">**Web API** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="a5732-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="a5732-128">このサンプルでは、IIS で OWIN をホストし、Web API を OWIN パイプラインに追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a5732-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="a5732-129">**Web ソケットのサンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="a5732-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="a5732-130">使用して、OWIN で Web ソケットをサポートする方法を示しています、 [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx)クラス。</span><span class="sxs-lookup"><span data-stu-id="a5732-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
