---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana サンプル |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: a26cdf144d02f0b429994df9d7863163807ac7b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827273"
---
<a name="katana-samples"></a>Katana サンプル
====================
によって[Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Katana サンプル

**ASP.NET ルーティング サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
一部のアプリケーションでは、非 OWIN コンポーネントと共に、Asp.Net ルート テーブル内の OWIN コンポーネントをフックするでしょう。 このサンプルでは、MapOwinPath と Microsoft.Owin.Host.SystemWeb によって提供される MapOwinRoute RouteCollection 拡張メソッドを使用する方法を示します。

**サンプル パイプラインを分岐** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
OWIN 要求処理パイプラインが線形にする必要はありません、それらをさまざまな方法で要求を処理する分岐できます。 このサンプルでは、要求のパスやヘッダーなどの他の要求データに基づいて分岐のパイプラインを構築する方法を示します。 これらのコンポーネント、Microsoft.Owin.Mapping nuget パッケージで利用できます。

**カスタム サーバー サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
自己ホストするときに、カスタムの OWIN サーバーを使用する方法を示しています。 OWIN します。

**サンプルの埋め込み** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
OWIN サーバーによっては、独自のプロセス内で実行できます (&quot;セルフホステッド&quot;)。 このサンプルでは、Microsoft.Owin.Hosting nuget パッケージによって提供されるツールを使用して、OWIN アプリケーションを起動する方法を示します。

**HelloWorld サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN は、HTTP サーバーのさまざまなサーバー間でアプリケーションの移植性をできるようにする抽象化された API です。 このサンプルは、いくつかを使用して Hello World アプリケーションを作成する方法を示します。**単純なラッパー** ASP.NET などの web サーバーで、生の OWIN 抽象化と実行を囲む。

**Hello World の生の OWIN サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
このサンプルを使用して Hello World アプリケーションを記述する方法、**生**OWIN 抽象化し、Asp.Net などの web サーバーで実行します。

**SignalR サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
OWIN を使用して SignalR を自己ホストする方法を示します/Katana します。 SignalR の自己ホストの詳細については、次を参照してください。[チュートリアル: SignalR セルフホスト](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)します。

**静的ファイルのサンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
OWIN を使用して静的ファイルの HTTP 要求をサポートする方法を示します/Katana します。

**Web API** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
このサンプルでは、IIS で OWIN をホストし、Web API を OWIN パイプラインに追加する方法を示します。

**Web ソケットのサンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
使用して、OWIN で Web ソケットをサポートする方法を示しています、 [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx)クラス。
