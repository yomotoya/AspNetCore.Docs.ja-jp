---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana サンプル |Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: b8ce2b40a19e0429f1ccedb03b8f829582652d24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829873"
---
<a name="katana-samples"></a>Katana サンプル
====================
によって[Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Katana サンプル

**ASP.NET ルーティング サンプル** | [ソース コード](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)  
一部のアプリケーションでは、非 OWIN コンポーネントと共に、ASP.NET ルート テーブル内の OWIN コンポーネントをフックするでしょう。 このサンプルでは、MapOwinPath と Microsoft.Owin.Host.SystemWeb によって提供される MapOwinRoute RouteCollection 拡張メソッドを使用する方法を示します。

**サンプル パイプラインを分岐** | [ソース コード](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)  
OWIN 要求処理パイプラインが線形にする必要はありません、それらをさまざまな方法で要求を処理する分岐できます。 このサンプルでは、要求のパスやヘッダーなどの他の要求データに基づいて分岐のパイプラインを構築する方法を示します。 これらのコンポーネント、Microsoft.Owin.Mapping nuget パッケージで利用できます。

**カスタム サーバー サンプル** | [ソース コード](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer)   
自己ホストするときに、カスタムの OWIN サーバーを使用する方法を示しています。 OWIN します。

**サンプルの埋め込み** | [ソース コード](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)  
OWIN サーバーによっては、独自のプロセス内で実行できます (&quot;セルフホステッド&quot;)。 このサンプルでは、Microsoft.Owin.Hosting nuget パッケージによって提供されるツールを使用して、OWIN アプリケーションを起動する方法を示します。

**HelloWorld サンプル** | [ソース コード](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)  
OWIN は、HTTP サーバーのさまざまなサーバー間でアプリケーションの移植性をできるようにする抽象化された API です。 このサンプルは、いくつかを使用して Hello World アプリケーションを作成する方法を示します。**単純なラッパー** ASP.NET などの web サーバーで、生の OWIN 抽象化と実行を囲む。

**Hello World の生の OWIN サンプル** | [ソース コード](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)  
このサンプルを使用して Hello World アプリケーションを記述する方法、**生**OWIN 抽象化し、ASP.NET などの web サーバーで実行します。

**SignalR サンプル** | [ソース コード](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)  
OWIN を使用して SignalR を自己ホストする方法を示します/Katana します。 SignalR の自己ホストの詳細については、次を参照してください。[チュートリアル: SignalR セルフホスト](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)します。

**静的ファイルのサンプル** | [ソース コード](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)   
OWIN を使用して静的ファイルの HTTP 要求をサポートする方法を示します/Katana します。

**Web API** | [ソース コード](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi)   
このサンプルでは、IIS で OWIN をホストし、Web API を OWIN パイプラインに追加する方法を示します。

**Web ソケットのサンプル** | [ソース コード](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample)   
使用して、OWIN で Web ソケットをサポートする方法を示しています、 [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx)クラス。
