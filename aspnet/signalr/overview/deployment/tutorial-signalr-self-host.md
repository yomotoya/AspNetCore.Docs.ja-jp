---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'チュートリアル: SignalR セルフホスト |Microsoft Docs'
author: pfletcher
description: このチュートリアルでは、SignalR 2 の自己ホスト型サーバーを作成する方法と、JavaScript クライアントで接続する方法を説明します。 ソフトウェアのバージョンが V のチュートリアルで使用しています.
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 71fb121377a49bb741ebff098ff20ec82e85c82a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821785"
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="4183c-104">チュートリアル: SignalR セルフホスト</span><span class="sxs-lookup"><span data-stu-id="4183c-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="4183c-105">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="4183c-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="4183c-106">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="4183c-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="4183c-107">このチュートリアルでは、SignalR 2 の自己ホスト型サーバーを作成する方法と、JavaScript クライアントで接続する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="4183c-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4183c-108">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="4183c-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="4183c-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4183c-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="4183c-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="4183c-110">.NET 4.5</span></span>
> - <span data-ttu-id="4183c-111">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="4183c-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="4183c-112">このチュートリアルで Visual Studio 2012 の使用</span><span class="sxs-lookup"><span data-stu-id="4183c-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="4183c-113">このチュートリアルでは、Visual Studio 2012 を使用するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="4183c-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="4183c-114">更新プログラム、[パッケージ マネージャー](http://docs.nuget.org/docs/start-here/installing-nuget)最新バージョンにします。</span><span class="sxs-lookup"><span data-stu-id="4183c-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="4183c-115">インストール、 [Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4183c-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="4183c-116">Web Platform Installer で検索してインストール**ASP.NET と Visual Studio 2012 for Web Tools 2013.1**します。</span><span class="sxs-lookup"><span data-stu-id="4183c-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="4183c-117">SignalR クラスの Visual Studio テンプレートなどのインストールはこの**ハブ**します。</span><span class="sxs-lookup"><span data-stu-id="4183c-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="4183c-118">一部のテンプレート (など**OWIN Startup クラス**) はできなくなります。 これらの場合には、クラス ファイルを代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="4183c-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="4183c-119">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="4183c-119">Questions and comments</span></span>
> 
> <span data-ttu-id="4183c-120">このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="4183c-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="4183c-121">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。</span><span class="sxs-lookup"><span data-stu-id="4183c-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="4183c-122">概要</span><span class="sxs-lookup"><span data-stu-id="4183c-122">Overview</span></span>

<span data-ttu-id="4183c-123">SignalR のサーバーは通常、IIS で ASP.NET アプリケーションでホストされているが、自己ホスト型にも (コンソール アプリケーションまたは Windows サービスなど)、自己ホスト ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="4183c-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="4183c-124">SignalR 2 のすべてと同様に、このライブラリは OWIN 上に構築 ([Open Web Interface for .NET](http://owin.org))。</span><span class="sxs-lookup"><span data-stu-id="4183c-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="4183c-125">OWIN は、.NET の web サーバーおよび web アプリケーション間の抽象化を定義します。</span><span class="sxs-lookup"><span data-stu-id="4183c-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="4183c-126">OWIN により、OWIN の IIS の外部の独自のプロセスで web アプリケーションを自己ホストするために最適ですが、サーバーから web アプリケーションを分離します。</span><span class="sxs-lookup"><span data-stu-id="4183c-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="4183c-127">IIS でホストしていない理由は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="4183c-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="4183c-128">IIS が利用できないか、IIS なしの既存のサーバー ファームなどの望ましい環境。</span><span class="sxs-lookup"><span data-stu-id="4183c-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="4183c-129">IIS のパフォーマンスのオーバーヘッドを回避する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4183c-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="4183c-130">SignalR の機能では、Windows サービス、Azure worker ロール、またはその他のプロセスで実行されている既存のアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="4183c-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="4183c-131">ソリューションは、パフォーマンス上の理由から自己ホストとして開発されているが場合、ことをお勧めもテストを IIS でホストされているアプリケーションでパフォーマンスの向上を特定します。</span><span class="sxs-lookup"><span data-stu-id="4183c-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="4183c-132">このチュートリアルには、次のセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4183c-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="4183c-133">サーバーの作成</span><span class="sxs-lookup"><span data-stu-id="4183c-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="4183c-134">JavaScript クライアントとサーバーへのアクセス</span><span class="sxs-lookup"><span data-stu-id="4183c-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="4183c-135">サーバーの作成</span><span class="sxs-lookup"><span data-stu-id="4183c-135">Creating the server</span></span>

<span data-ttu-id="4183c-136">このチュートリアルでは、コンソール アプリケーションでホストされているサーバーを作成しますが、サーバーは、あらゆる種類の Windows サービスまたは Azure ワーカー ロールなどのプロセスでホストできます。</span><span class="sxs-lookup"><span data-stu-id="4183c-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="4183c-137">Windows サービスで SignalR のサーバーをホストするためのサンプル コードを参照してください。 [Windows サービスで Self-Hosting SignalR](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3)します。</span><span class="sxs-lookup"><span data-stu-id="4183c-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="4183c-138">管理者特権で Visual Studio 2013 を開きます。</span><span class="sxs-lookup"><span data-stu-id="4183c-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="4183c-139">選択**ファイル**、**新しいプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="4183c-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="4183c-140">選択**Windows**下、 **Visual c#** 内のノード、**テンプレート**ペイン、および選択、**コンソール アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="4183c-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="4183c-141">新しいプロジェクトの名前を"SignalRSelfHost"にして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="4183c-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="4183c-142">選択して、ライブラリ パッケージ マネージャー コンソールを開きます**ツール**、**ライブラリ パッケージ マネージャー**、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="4183c-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="4183c-143">パッケージ マネージャー コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="4183c-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="4183c-144">このコマンドは、プロジェクトに SignalR 2 Self-Host ライブラリを追加します。</span><span class="sxs-lookup"><span data-stu-id="4183c-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="4183c-145">パッケージ マネージャー コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="4183c-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="4183c-146">このコマンドは、Microsoft.Owin.Cors ライブラリをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="4183c-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="4183c-147">このライブラリは、SignalR と web ページのクライアントに別のドメインをホストしているアプリケーションに必要なクロス ドメインのサポートに使用されます。</span><span class="sxs-lookup"><span data-stu-id="4183c-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="4183c-148">つまり、SignalR サーバーと異なるポートを web クライアントがホストするとします、ために、クロス ドメインは、これらのコンポーネント間の通信を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4183c-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="4183c-149">Program.cs の内容を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4183c-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="4183c-150">上記のコードには、3 つのクラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4183c-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="4183c-151">**プログラム**など、 **Main**メソッドの実行のプライマリ パスを定義します。</span><span class="sxs-lookup"><span data-stu-id="4183c-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="4183c-152">このメソッドでは、型の web アプリケーションで**スタートアップ**が開始されて、指定した URL (`http://localhost:8080`)。</span><span class="sxs-lookup"><span data-stu-id="4183c-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="4183c-153">エンドポイントのセキュリティが必要な場合は、SSL を実装できます。</span><span class="sxs-lookup"><span data-stu-id="4183c-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="4183c-154">参照してください[方法: SSL 証明書でポートを構成](https://msdn.microsoft.com/library/ms733791.aspx)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="4183c-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="4183c-155">**スタートアップ**、SignalR のサーバーの構成を含むクラス (このチュートリアルでは、のみの構成への呼び出しは、 `UseCors`) への呼び出し`MapSignalR`ハブのすべてのオブジェクトのルート プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="4183c-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="4183c-156">**MyHub**アプリケーションをクライアントに提供する SignalR ハブ クラス。</span><span class="sxs-lookup"><span data-stu-id="4183c-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="4183c-157">このクラスは 1 つのメソッド、**送信**、接続されている他のすべてのクライアントにメッセージをブロードキャストするクライアントが呼び出すことです。</span><span class="sxs-lookup"><span data-stu-id="4183c-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="4183c-158">アプリケーションをコンパイルして実行します。</span><span class="sxs-lookup"><span data-stu-id="4183c-158">Compile and run the application.</span></span> <span data-ttu-id="4183c-159">サーバーを実行しているアドレスは、コンソール ウィンドウに表示されます。</span><span class="sxs-lookup"><span data-stu-id="4183c-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="4183c-160">例外によって実行が失敗した場合`System.Reflection.TargetInvocationException was unhandled`、管理者特権で Visual Studio を再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4183c-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="4183c-161">次のセクションに進む前にアプリケーションを停止します。</span><span class="sxs-lookup"><span data-stu-id="4183c-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="4183c-162">JavaScript クライアントとサーバーへのアクセス</span><span class="sxs-lookup"><span data-stu-id="4183c-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="4183c-163">このセクションでは、同じ JavaScript クライアントから使用します、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)します。</span><span class="sxs-lookup"><span data-stu-id="4183c-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="4183c-164">これは、ハブの URL を明示的に定義すると、クライアントに 1 つの変更のみしましょう。</span><span class="sxs-lookup"><span data-stu-id="4183c-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="4183c-165">自己ホスト型アプリケーションでは、サーバー必ずしも URL (リバース プロキシとロード バランサー) のための接続と同じアドレスでこれを明示的に定義の URL は。</span><span class="sxs-lookup"><span data-stu-id="4183c-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="4183c-166">**ソリューション エクスプ ローラー**ソリューションを右クリックし、選択、**追加**、**新しいプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="4183c-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="4183c-167">選択、 **Web**ノード、および選択、 **ASP.NET Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="4183c-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="4183c-168">プロジェクトに"JavascriptClient"という名前にして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="4183c-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="4183c-169">選択、**空**テンプレート、および残りのオプションがオフのままにします。</span><span class="sxs-lookup"><span data-stu-id="4183c-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="4183c-170">選択**プロジェクトを作成する**します。</span><span class="sxs-lookup"><span data-stu-id="4183c-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="4183c-171">パッケージ マネージャー コンソールで"JavascriptClient"プロジェクトを選択、**既定のプロジェクト**ドロップダウンで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4183c-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="4183c-172">このコマンドは、クライアントにする必要があります、SignalR と JQuery ライブラリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4183c-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="4183c-173">クリックし、プロジェクトを右クリックして**追加**、**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="4183c-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="4183c-174">選択、 **Web**ノード、および HTML ページを選択します。</span><span class="sxs-lookup"><span data-stu-id="4183c-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="4183c-175">ページの名前を付けます**Default.html**します。</span><span class="sxs-lookup"><span data-stu-id="4183c-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="4183c-176">新しい HTML ページの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4183c-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="4183c-177">ここで、スクリプト参照がプロジェクトの Scripts フォルダー内のスクリプトと一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="4183c-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="4183c-178">次のコード (上記のコード例で強調表示) とは、(SignalR バージョン 2 の beta へのコードのアップグレード) に加えて、よろめき取得のチュートリアルで使用するクライアントに適用した追加です。</span><span class="sxs-lookup"><span data-stu-id="4183c-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="4183c-179">次のコードでは SignalR の基本的な接続 URL が明示的にサーバー上に設定します。</span><span class="sxs-lookup"><span data-stu-id="4183c-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="4183c-180">ソリューションを右クリックして**スタートアップ プロジェクトの設定.**.選択、**マルチ スタートアップ プロジェクト**ボタンをクリックし、両方のプロジェクトの設定**アクション**に**開始**します。</span><span class="sxs-lookup"><span data-stu-id="4183c-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="4183c-181">"Default.html"を右クリックし、選択**スタート ページとして設定**します。</span><span class="sxs-lookup"><span data-stu-id="4183c-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="4183c-182">アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="4183c-182">Run the application.</span></span> <span data-ttu-id="4183c-183">サーバーおよびページが起動します。</span><span class="sxs-lookup"><span data-stu-id="4183c-183">The server and page will launch.</span></span> <span data-ttu-id="4183c-184">Web ページを再読み込みする必要があります (または選択**続行**デバッガーで) サーバーを開始する前に、ページが読み込まれる場合。</span><span class="sxs-lookup"><span data-stu-id="4183c-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="4183c-185">ブラウザーでは、入力を求められたら、ユーザー名を提供します。</span><span class="sxs-lookup"><span data-stu-id="4183c-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="4183c-186">別のブラウザー タブまたはウィンドウに、ページの URL をコピーして、別のユーザー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="4183c-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="4183c-187">チュートリアル入門のように、他の 1 つのブラウザー ウィンドウからメッセージを送信することができます。</span><span class="sxs-lookup"><span data-stu-id="4183c-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
