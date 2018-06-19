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
<a name="katana-samples"></a>Katana サンプル
====================
によって[Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Katana サンプル

**ASP.NET ルーティング サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
一部のアプリケーションでは、非 OWIN コンポーネントのサイド バイ サイドでの Asp.Net ルート テーブル内の OWIN コンポーネントをフックするためにすることができます。 このサンプルでは、MapOwinPath と Microsoft.Owin.Host.SystemWeb によって提供される MapOwinRoute RouteCollection 拡張メソッドを使用する方法を示します。

**サンプル パイプラインを分岐して判別** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
OWIN 要求処理パイプラインは線形である必要はありませんは、さまざまな方法で要求の処理が分岐された時間ことができます。 このサンプルでは、要求のパスやヘッダーなどの他の要求データに基づいて分岐パイプラインを構築する方法を示します。 これらのコンポーネントは Microsoft.Owin.Mapping nuget パッケージで使用できます。

**カスタム サーバー サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
セルフホストされているときに、カスタムの OWIN サーバーを使用する方法を示します OWIN です。

**サンプルを埋め込み** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
OWIN サーバーによっては、独自のプロセス内で実行できます (&quot;セルフホスト&quot;)。 このサンプルでは、OWIN を Microsoft.Owin.Hosting nuget パッケージによって提供されるツールを使用してアプリケーションを起動する方法を示します。

**HelloWorld サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN は、HTTP サーバーをさまざまなサーバー間でアプリケーションの移植性を有効にする API の抽象化です。 このサンプルがいくつかを使用して、Hello World アプリケーションを記述する方法を示します**単純なラッパー** ASP.NET などの web サーバー上に、生の OWIN の抽象化と実行を囲む。

**生の OWIN の hello World サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
このサンプルを使用して Hello World アプリケーションを記述する方法、**生**OWIN の抽象化し、Asp.Net のような web サーバーで実行します。

**SignalR サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
OWIN を使用して SignalR を自己ホストする方法を示します/Katana です。 自己ホストの SignalR の詳細については、次を参照してください。[チュートリアル: 自己ホスト SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)です。

**静的ファイルのサンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
OWIN を使用して静的ファイルの HTTP 要求をサポートする方法を示します/Katana です。

**Web API** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
このサンプルでは、IIS で OWIN をホストし、Web API OWIN パイプラインを追加する方法を示します。

**Web ソケット サンプル** | [ソース コード](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
OWIN でを使用して Web ソケットをサポートする方法を示しています、 [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx)クラスです。
