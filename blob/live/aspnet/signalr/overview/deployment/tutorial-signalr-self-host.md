---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: "チュートリアル: 自己ホスト SignalR |Microsoft ドキュメント"
author: pfletcher
description: "このチュートリアルでは、自己ホスト型の SignalR 2 サーバーを作成する方法と、JavaScript クライアントでそれに接続する方法を説明します。 ソフトウェアのバージョンが V チュートリアルで使用しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 997756ff8d48e41da981491d6154f3107ec7a051
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="a37de-104">チュートリアル: SignalR の自己ホスト</span><span class="sxs-lookup"><span data-stu-id="a37de-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="a37de-105">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="a37de-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="a37de-106">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="a37de-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="a37de-107">このチュートリアルでは、自己ホスト型の SignalR 2 サーバーを作成する方法と、JavaScript クライアントでそれに接続する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="a37de-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a37de-108">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="a37de-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="a37de-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a37de-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="a37de-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a37de-110">.NET 4.5</span></span>
> - <span data-ttu-id="a37de-111">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="a37de-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="a37de-112">このチュートリアルで Visual Studio 2012 を使用します。</span><span class="sxs-lookup"><span data-stu-id="a37de-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="a37de-113">このチュートリアルでは、Visual Studio 2012 を使用するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="a37de-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="a37de-114">更新プログラム、 [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget)最新バージョンにします。</span><span class="sxs-lookup"><span data-stu-id="a37de-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="a37de-115">インストール、 [Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="a37de-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="a37de-116">Web Platform Installer で検索し、インストール**ASP.NET と Visual Studio 2012 用 Web ツール 2013.1**です。</span><span class="sxs-lookup"><span data-stu-id="a37de-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="a37de-117">SignalR クラス用の Visual Studio テンプレートなどのインストールはこの**ハブ**です。</span><span class="sxs-lookup"><span data-stu-id="a37de-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="a37de-118">一部のテンプレート (など**OWIN スタートアップ クラス**) できなくなります。 これらの場合には、クラス ファイルを代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="a37de-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="a37de-119">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="a37de-119">Questions and comments</span></span>
> 
> <span data-ttu-id="a37de-120">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="a37de-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="a37de-121">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="a37de-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="a37de-122">概要</span><span class="sxs-lookup"><span data-stu-id="a37de-122">Overview</span></span>

<span data-ttu-id="a37de-123">SignalR のサーバーは通常、IIS で ASP.NET アプリケーションでホストされているが、自己ホスト型こともできます (このようなコンソール アプリケーションまたは Windows サービスと同様に) 自己ホスト ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="a37de-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="a37de-124">SignalR 2 のと同様に、このライブラリは OWIN 上に構築 ([.NET 用 Web インターフェイスを開く](http://owin.org))。</span><span class="sxs-lookup"><span data-stu-id="a37de-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="a37de-125">OWIN では、.NET の web サーバーおよび web アプリケーション間の抽象化を定義します。</span><span class="sxs-lookup"><span data-stu-id="a37de-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="a37de-126">OWIN には、web アプリケーション、サーバーは、OWIN のため、IIS の外部で独自のプロセス内の web アプリケーションを自己ホストするために最適ですから切り離します。</span><span class="sxs-lookup"><span data-stu-id="a37de-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="a37de-127">IIS でホストしていない理由は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a37de-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="a37de-128">IIS が行われていない IIS せず、既存のサーバー ファームなど、むしろ望ましい場合も使用可能な環境です。</span><span class="sxs-lookup"><span data-stu-id="a37de-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="a37de-129">IIS のパフォーマンスのオーバーヘッドを回避する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a37de-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="a37de-130">SignalR の機能は、Windows サービス、Azure ワーカー ロール、またはその他のプロセスで実行されている既存のアプリケーションに追加するのには。</span><span class="sxs-lookup"><span data-stu-id="a37de-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="a37de-131">場合は、ソリューションは、自己ホスト パフォーマンス上の理由として開発中は、ことをお勧めもテストに IIS でホストされているアプリケーション パフォーマンスの利点を決定します。</span><span class="sxs-lookup"><span data-stu-id="a37de-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="a37de-132">このチュートリアルでは、次のセクションでは、含まれています。</span><span class="sxs-lookup"><span data-stu-id="a37de-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="a37de-133">サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a37de-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="a37de-134">JavaScript クライアントとサーバーへのアクセス</span><span class="sxs-lookup"><span data-stu-id="a37de-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="a37de-135">サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a37de-135">Creating the server</span></span>

<span data-ttu-id="a37de-136">このチュートリアルでは、コンソール アプリケーションでホストされているサーバーを作成しますが、あらゆる種類の Windows サービスまたは Azure ワーカー ロールなどのプロセスで、サーバーをホストできます。</span><span class="sxs-lookup"><span data-stu-id="a37de-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="a37de-137">SignalR のサーバーに Windows サービスをホストするためのサンプル コードを参照してください。 [Windows サービスで Self-Hosting SignalR](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3)です。</span><span class="sxs-lookup"><span data-stu-id="a37de-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="a37de-138">管理者特権で Visual Studio 2013 を開きます。</span><span class="sxs-lookup"><span data-stu-id="a37de-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="a37de-139">選択**ファイル**、**新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="a37de-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="a37de-140">選択**Windows**下にある、 **Visual c#**内のノード、**テンプレート**ペイン、および選択、**コンソール アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="a37de-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="a37de-141">新しいプロジェクトの名前を"SignalRSelfHost"し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="a37de-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="a37de-142">選択して、ライブラリ パッケージ マネージャー コンソールを開きます**ツール**、**ライブラリ パッケージ マネージャー**、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="a37de-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="a37de-143">パッケージ マネージャー コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="a37de-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="a37de-144">このコマンドは、SignalR 2 Self-Host ライブラリをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="a37de-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="a37de-145">パッケージ マネージャー コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="a37de-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="a37de-146">このコマンドは、Microsoft.Owin.Cors ライブラリをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="a37de-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="a37de-147">クロス ドメインのサポートは、SignalR と別のドメインで web ページのクライアントをホストするアプリケーションに必要なは、このライブラリが使用されます。</span><span class="sxs-lookup"><span data-stu-id="a37de-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="a37de-148">つまり、SignalR のサーバーと異なるポートを web クライアントがホストするが、ので、クロス ドメインは、これらのコンポーネント間の通信に有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a37de-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="a37de-149">Program.cs の内容を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a37de-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="a37de-150">上記のコードには、3 つのクラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a37de-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="a37de-151">**プログラム**など、 **Main**メソッドの実行のプライマリ パスを定義します。</span><span class="sxs-lookup"><span data-stu-id="a37de-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="a37de-152">このメソッドでは、型の web アプリケーションで**スタートアップ**が url で指定した開始 (`http://localhost:8080`)。</span><span class="sxs-lookup"><span data-stu-id="a37de-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="a37de-153">セキュリティが必要な場合、エンドポイントで、SSL を実装することができます。</span><span class="sxs-lookup"><span data-stu-id="a37de-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="a37de-154">参照してください[する方法: SSL 証明書でポートを構成する](https://msdn.microsoft.com/en-us/library/ms733791.aspx)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="a37de-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/en-us/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="a37de-155">**スタートアップ**、SignalR のサーバーの構成を含むクラス (このチュートリアルではのみの構成への呼び出しは、 `UseCors`)、および呼び出し`MapSignalR`プロジェクト内のすべてのハブ オブジェクト ルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="a37de-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="a37de-156">**MyHub**アプリケーションをクライアントに提供する SignalR ハブ クラスです。</span><span class="sxs-lookup"><span data-stu-id="a37de-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="a37de-157">このクラスには 1 つのメソッド、**送信**、接続されている他のすべてのクライアントにメッセージをブロードキャストにクライアントが呼び出すことです。</span><span class="sxs-lookup"><span data-stu-id="a37de-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="a37de-158">アプリケーションをコンパイルして実行します。</span><span class="sxs-lookup"><span data-stu-id="a37de-158">Compile and run the application.</span></span> <span data-ttu-id="a37de-159">コンソール ウィンドウで、サーバーが実行されているアドレスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a37de-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="a37de-160">例外によって実行が失敗した場合`System.Reflection.TargetInvocationException was unhandled`、管理者特権で Visual Studio を再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a37de-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="a37de-161">次のセクションに進む前にアプリケーションを停止します。</span><span class="sxs-lookup"><span data-stu-id="a37de-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="a37de-162">JavaScript クライアントとサーバーへのアクセス</span><span class="sxs-lookup"><span data-stu-id="a37de-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="a37de-163">同じ JavaScript クライアントから使用するこのセクションで、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)です。</span><span class="sxs-lookup"><span data-stu-id="a37de-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="a37de-164">1 つの変更はハブ URL を明示的に定義すると、クライアントにのみしましょう。</span><span class="sxs-lookup"><span data-stu-id="a37de-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="a37de-165">自己ホスト型アプリケーションでは、サーバー必ずしも (ため、リバース プロキシおよびロード バランサーを使用)、接続の URL と同じアドレスにあるためを明示的に定義する URL 必要があります。</span><span class="sxs-lookup"><span data-stu-id="a37de-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="a37de-166">**ソリューション エクスプ ローラー**ソリューションを右クリックし、選択、**追加**、**新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="a37de-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="a37de-167">選択、 **Web**ノードをクリックし、 **ASP.NET Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="a37de-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="a37de-168">プロジェクト"JavascriptClient"の名前し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="a37de-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="a37de-169">選択、**空**テンプレート、および残りのオプションがオフのままにします。</span><span class="sxs-lookup"><span data-stu-id="a37de-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="a37de-170">選択**プロジェクトを作成する**です。</span><span class="sxs-lookup"><span data-stu-id="a37de-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="a37de-171">パッケージ マネージャー コンソールで、"JavascriptClient"でプロジェクトを選択、**既定のプロジェクト**ドロップダウンでは、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a37de-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="a37de-172">このコマンドは、クライアントにする必要があります、SignalR と JQuery ライブラリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="a37de-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="a37de-173">クリックして、プロジェクトを右クリックして**追加**、**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="a37de-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="a37de-174">選択、 **Web**ノード、および HTML ページを選択します。</span><span class="sxs-lookup"><span data-stu-id="a37de-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="a37de-175">ページの名前を付けます**Default.html**です。</span><span class="sxs-lookup"><span data-stu-id="a37de-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="a37de-176">新しい HTML ページの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a37de-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="a37de-177">ここで、スクリプト参照がプロジェクトの Scripts フォルダー内のスクリプトを一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="a37de-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="a37de-178">(上記のコード例で強調表示されます)、次のコードは、(だけでなく、コードの SignalR バージョン 2 のベータ版にアップグレード) の起動を取得するチュートリアルで使用されるクライアントに対して行った追加です。</span><span class="sxs-lookup"><span data-stu-id="a37de-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="a37de-179">次のコード行は明示的に、サーバーで、SignalR の基本的な接続 URL を設定します。</span><span class="sxs-lookup"><span data-stu-id="a37de-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="a37de-180">ソリューションを右クリックし **スタートアップ プロジェクトを設定しています.**.選択、**マルチ スタートアップ プロジェクト**ラジオ ボタン、および両方のプロジェクトの設定**アクション**に**開始**です。</span><span class="sxs-lookup"><span data-stu-id="a37de-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="a37de-181">"Default.html"を右クリックし、選択**スタート ページとして設定**です。</span><span class="sxs-lookup"><span data-stu-id="a37de-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="a37de-182">アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="a37de-182">Run the application.</span></span> <span data-ttu-id="a37de-183">サーバーおよびページが起動します。</span><span class="sxs-lookup"><span data-stu-id="a37de-183">The server and page will launch.</span></span> <span data-ttu-id="a37de-184">Web ページを再読み込みする必要があります (または選択**続行**デバッガーで) サーバーが開始される前に、ページが読み込まれる場合。</span><span class="sxs-lookup"><span data-stu-id="a37de-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="a37de-185">ブラウザーで表示されたら、ユーザー名を提供します。</span><span class="sxs-lookup"><span data-stu-id="a37de-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="a37de-186">別のブラウザー タブまたはウィンドウに、ページの URL をコピーし、別のユーザー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="a37de-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="a37de-187">チュートリアル入門のように、他の 1 つのブラウザー ウィンドウからメッセージを送信することができます。</span><span class="sxs-lookup"><span data-stu-id="a37de-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
