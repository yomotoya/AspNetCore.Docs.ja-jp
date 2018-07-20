---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'チュートリアル: SignalR 2 によるサーバーがブロードキャスト |Microsoft Docs'
author: tdykstra
description: このチュートリアルでは、ASP.NET SignalR 2 を使用してサーバー ブロードキャストの機能を提供する web アプリケーションを作成する方法を示します。 サーバー ブロードキャストでは、その commun ことを意味しています.
ms.author: aspnetcontent
ms.date: 10/13/2014
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0e86fbea9c5668e20fce7a494c76c52f9c089c09
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820698"
---
<a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="b8209-104">チュートリアル: SignalR 2 によるサーバーがブロードキャスト</span><span class="sxs-lookup"><span data-stu-id="b8209-104">Tutorial: Server Broadcast with SignalR 2</span></span>
====================
<span data-ttu-id="b8209-105">によって[Tom Dykstra](https://github.com/tdykstra)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b8209-105">by [Tom Dykstra](https://github.com/tdykstra), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b8209-106">このチュートリアルでは、ASP.NET SignalR 2 を使用してサーバー ブロードキャストの機能を提供する web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b8209-106">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="b8209-107">サーバー ブロードキャストでは、クライアントに送信される通信が、サーバーによって開始されたことを意味します。</span><span class="sxs-lookup"><span data-stu-id="b8209-107">Server broadcast means that communications sent to clients are initiated by the server.</span></span> <span data-ttu-id="b8209-108">このシナリオでは、チャット アプリケーションをクライアントに送信される通信が 1 つまたは複数のクライアントによって開始されたなどのピア ツー ピア シナリオよりもさまざまなプログラミングのアプローチが必要です。</span><span class="sxs-lookup"><span data-stu-id="b8209-108">This scenario requires a different programming approach than peer-to-peer scenarios such as chat applications, in which communications sent to clients are initiated by one or more of the clients.</span></span>
> 
> <span data-ttu-id="b8209-109">このチュートリアルで作成するアプリケーションでは、株価情報、サーバー ブロードキャストの機能の一般的なシナリオをシミュレートします。</span><span class="sxs-lookup"><span data-stu-id="b8209-109">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span>
> 
> <span data-ttu-id="b8209-110">このトピックでは、Patrick Fletcher によって作成当初されました。</span><span class="sxs-lookup"><span data-stu-id="b8209-110">This topic was originally written by Patrick Fletcher.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b8209-111">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="b8209-111">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="b8209-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b8209-112">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="b8209-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b8209-113">.NET 4.5</span></span>
> - <span data-ttu-id="b8209-114">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="b8209-114">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="b8209-115">このチュートリアルで Visual Studio 2012 の使用</span><span class="sxs-lookup"><span data-stu-id="b8209-115">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="b8209-116">このチュートリアルでは、Visual Studio 2012 を使用するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="b8209-116">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="b8209-117">更新プログラム、[パッケージ マネージャー](http://docs.nuget.org/docs/start-here/installing-nuget)最新バージョンにします。</span><span class="sxs-lookup"><span data-stu-id="b8209-117">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="b8209-118">インストール、 [Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="b8209-118">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="b8209-119">Web Platform Installer で検索してインストール**ASP.NET と Visual Studio 2012 for Web Tools 2013.1**します。</span><span class="sxs-lookup"><span data-stu-id="b8209-119">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="b8209-120">SignalR クラスの Visual Studio テンプレートなどのインストールはこの**ハブ**します。</span><span class="sxs-lookup"><span data-stu-id="b8209-120">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="b8209-121">一部のテンプレート (など**OWIN Startup クラス**) はできなくなります。 これらの場合には、クラス ファイルを代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="b8209-121">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="b8209-122">チュートリアルのバージョン</span><span class="sxs-lookup"><span data-stu-id="b8209-122">Tutorial Versions</span></span>
> 
> <span data-ttu-id="b8209-123">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="b8209-123">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="b8209-124">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="b8209-124">Questions and comments</span></span>
> 
> <span data-ttu-id="b8209-125">このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="b8209-125">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b8209-126">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。</span><span class="sxs-lookup"><span data-stu-id="b8209-126">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="b8209-127">概要</span><span class="sxs-lookup"><span data-stu-id="b8209-127">Overview</span></span>

<span data-ttu-id="b8209-128">このチュートリアルでは、リアルタイムのアプリケーションを定期的に「プッシュ」する代表的な株価表示器のアプリケーションを作成または接続されているすべてのクライアントをサーバーからの通知をブロードキャストします。 します。</span><span class="sxs-lookup"><span data-stu-id="b8209-128">In this tutorial, you'll create a stock ticker application that is representative of real-time applications in which you want to periodically "push," or broadcast, notifications from the server to all connected clients.</span></span> <span data-ttu-id="b8209-129">このチュートリアルの最初の部分では、最初からそのアプリケーションの簡略化されたバージョンを作成します。</span><span class="sxs-lookup"><span data-stu-id="b8209-129">In the first part of this tutorial, you'll create a simplified version of that application from scratch.</span></span> <span data-ttu-id="b8209-130">チュートリアルの残りの部分では、追加の機能を含む NuGet パッケージをインストールし、それらの機能のコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="b8209-130">In the remainder of the tutorial, you'll install a NuGet package that contains additional features, and review the code for those features.</span></span>

<span data-ttu-id="b8209-131">このチュートリアルの最初の部分でビルドするアプリケーションでは、株価データを持つグリッドが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-131">The application that you'll build in the first part of this tutorial displays a grid with stock data.</span></span>

![StockTicker 初期バージョン](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="b8209-133">定期的に、サーバーはランダムに株価を更新し、更新プログラムをすべて接続されているクライアントにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="b8209-133">Periodically the server randomly updates stock prices and pushes the updates to all connected clients.</span></span> <span data-ttu-id="b8209-134">ブラウザー、数字、およびシンボルで、**変更**と**%** 通知をサーバーからの応答で列を動的に変更します。</span><span class="sxs-lookup"><span data-stu-id="b8209-134">In the browser the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="b8209-135">同じ URL に他のブラウザーを開いた場合、同じデータやデータに同じ変更を同時にすべて表示します。</span><span class="sxs-lookup"><span data-stu-id="b8209-135">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

<span data-ttu-id="b8209-136">このチュートリアルには、次のセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b8209-136">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="b8209-137">前提条件</span><span class="sxs-lookup"><span data-stu-id="b8209-137">Prerequisites</span></span>](#prerequisites)
- [<span data-ttu-id="b8209-138">プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b8209-138">Create the project</span></span>](#createproject)
- [<span data-ttu-id="b8209-139">サーバー コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="b8209-139">Set up the server code</span></span>](#server)
- [<span data-ttu-id="b8209-140">クライアント コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="b8209-140">Set up the client code</span></span>](#client)
- [<span data-ttu-id="b8209-141">アプリケーションをテストします。</span><span class="sxs-lookup"><span data-stu-id="b8209-141">Test the application</span></span>](#test)
- [<span data-ttu-id="b8209-142">ログ記録を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b8209-142">Enable logging</span></span>](#enablelogging)
- [<span data-ttu-id="b8209-143">インストールして、完全な StockTicker サンプルを確認してください。</span><span class="sxs-lookup"><span data-stu-id="b8209-143">Install and review the full StockTicker sample</span></span>](#fullsample)
- [<span data-ttu-id="b8209-144">次のステップ</span><span class="sxs-lookup"><span data-stu-id="b8209-144">Next steps</span></span>](#nextsteps)

<span data-ttu-id="b8209-145">[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet パッケージは、Visual Studio プロジェクトにサンプルのシミュレートされた株価表示器アプリケーションをインストールします。</span><span class="sxs-lookup"><span data-stu-id="b8209-145">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet package installs a sample simulated stock ticker application in a Visual Studio project.</span></span>

> [!NOTE]
> <span data-ttu-id="b8209-146">アプリケーションの構築の手順を実行しない場合は、新しい空の ASP.NET Web アプリケーション プロジェクトで SignalR.Sample パッケージをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="b8209-146">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="b8209-147">このチュートリアルの手順を実行せずに NuGet パッケージをインストールした場合**readme.txt ファイルの指示に従う必要があります**します。</span><span class="sxs-lookup"><span data-stu-id="b8209-147">If you install the NuGet package without performing the steps in this tutorial, **you must follow the instructions in the readme.txt file**.</span></span> <span data-ttu-id="b8209-148">パッケージを実行するには、インストールされているパッケージで ConfigureSignalR メソッドを呼び出す OWIN startup クラスを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8209-148">To run the package you need to add an OWIN startup class which calls the ConfigureSignalR method in the installed package.</span></span> <span data-ttu-id="b8209-149">OWIN startup クラスを追加しない場合、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-149">You will receive an error if you do not add the OWIN startup class.</span></span>


<a id="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="b8209-150">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="b8209-150">Prerequisites</span></span>

<span data-ttu-id="b8209-151">開始する前に、コンピューターにインストールされている、Visual Studio 2013 があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b8209-151">Before you start, make sure that you have Visual Studio 2013 installed on your computer.</span></span> <span data-ttu-id="b8209-152">Visual Studio を持っていない場合は、次を参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)、無料の Visual Studio 2013 Express を取得します。</span><span class="sxs-lookup"><span data-stu-id="b8209-152">If you don't have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express.</span></span>

<a id="createproject"></a>

## <a name="create-the-project"></a><span data-ttu-id="b8209-153">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="b8209-153">Create the project</span></span>

1. <span data-ttu-id="b8209-154">**ファイル** メニューのをクリックして**新しいプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="b8209-154">From the **File** menu, click **New Project**.</span></span>
2. <span data-ttu-id="b8209-155">**新しいプロジェクト**] ダイアログ ボックスで、展開**c#** [**テンプレート**選択と**Web**します。</span><span class="sxs-lookup"><span data-stu-id="b8209-155">In the **New Project** dialog box, expand **C#** under **Templates** and select **Web**.</span></span>
3. <span data-ttu-id="b8209-156">選択、**空の ASP.NET Web アプリケーション**名では、プロジェクト テンプレートは、 *SignalR.StockTicker*、 をクリック**OK**します。</span><span class="sxs-lookup"><span data-stu-id="b8209-156">Select the **ASP.NET Empty Web Application** template, name the project *SignalR.StockTicker*, and click **OK**.</span></span>

    ![[新しいプロジェクト] ダイアログ ボックス](tutorial-server-broadcast-with-signalr/_static/image2.png)
4. <span data-ttu-id="b8209-158">**新しい ASP.NET** Project ウィンドウのままに**空**選択およびクリックして**プロジェクトの作成**。</span><span class="sxs-lookup"><span data-stu-id="b8209-158">In the **New ASP.NET** Project window, leave **Empty** selected and click **Create Project**.</span></span>

    ![新しい ASP プロジェクト ダイアログ ボックス](tutorial-server-broadcast-with-signalr/_static/image3.png)

<a id="server"></a>

## <a name="set-up-the-server-code"></a><span data-ttu-id="b8209-160">サーバー コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="b8209-160">Set up the server code</span></span>

<span data-ttu-id="b8209-161">このセクションでは、サーバーで実行されるコードを設定します。</span><span class="sxs-lookup"><span data-stu-id="b8209-161">In this section you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="b8209-162">在庫クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="b8209-162">Create the Stock class</span></span>

<span data-ttu-id="b8209-163">まず、格納および転送については、在庫を使用する在庫のモデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="b8209-163">You begin by creating the Stock model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="b8209-164">プロジェクト フォルダーに新しいクラス ファイルを作成、名前を付けます*Stock.cs*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b8209-164">Create a new class file in the project folder, name it *Stock.cs*, and then replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="b8209-165">素材を作成するときに設定する 2 つのプロパティは、(たとえば、Microsoft の MSFT) シンボルと価格は。</span><span class="sxs-lookup"><span data-stu-id="b8209-165">The two properties that you'll set when you create stocks are the Symbol (for example, MSFT for Microsoft) and the Price.</span></span> <span data-ttu-id="b8209-166">その他のプロパティは、価格を設定する方法とタイミングに依存します。</span><span class="sxs-lookup"><span data-stu-id="b8209-166">The other properties depend on how and when you set Price.</span></span> <span data-ttu-id="b8209-167">初めての価格を設定する値は、DayOpen に反映を取得します。</span><span class="sxs-lookup"><span data-stu-id="b8209-167">The first time you set Price, the value gets propagated to DayOpen.</span></span> <span data-ttu-id="b8209-168">価格、変更を設定していて PercentChange プロパティの値を計算以降の時間は、価格と DayOpen の違いに基づきます。</span><span class="sxs-lookup"><span data-stu-id="b8209-168">Subsequent times when you set Price, the Change and PercentChange property values are calculated based on the difference between Price and DayOpen.</span></span>

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a><span data-ttu-id="b8209-169">StockTicker と StockTickerHub クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="b8209-169">Create the StockTicker and StockTickerHub classes</span></span>

<span data-ttu-id="b8209-170">サーバーからクライアントへの対話を処理するために、SignalR Hub API を使用します。</span><span class="sxs-lookup"><span data-stu-id="b8209-170">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="b8209-171">SignalR ハブ クラスから派生した StockTickerHub クラスでは、クライアントからの受信接続とメソッドの呼び出しを処理します。</span><span class="sxs-lookup"><span data-stu-id="b8209-171">A StockTickerHub class that derives from the SignalR Hub class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="b8209-172">株価データを維持し、クライアント接続とは無関係に、価格の更新プログラムを定期的にトリガーするタイマー オブジェクトを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8209-172">You also need to maintain stock data and run a Timer object to periodically trigger price updates, independently of client connections.</span></span> <span data-ttu-id="b8209-173">ハブ インスタンスは一時的なので、ハブ クラスでこれらの関数を配置することはできません。</span><span class="sxs-lookup"><span data-stu-id="b8209-173">You can't put these functions in a Hub class, because Hub instances are transient.</span></span> <span data-ttu-id="b8209-174">接続と、クライアントからサーバーへの呼び出しなど、ハブの各操作ではハブ クラスのインスタンスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-174">A Hub class instance is created for each operation on the hub, such as connections and calls from the client to the server.</span></span> <span data-ttu-id="b8209-175">株価データを保持し、価格を更新、価格の更新プログラムにブロードキャスト メカニズム StockTicker 名前を付けます別のクラスで実行するようにします。</span><span class="sxs-lookup"><span data-stu-id="b8209-175">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class, which you'll name StockTicker.</span></span>

![StockTicker からブロードキャスト](tutorial-server-broadcast-with-signalr/_static/image5.png)

<span data-ttu-id="b8209-177">各 StockTickerHub インスタンスから、シングルトン StockTicker インスタンスへの参照を設定する必要がありますので、サーバー上で実行する StockTicker クラスのインスタンスを 1 つだけします。</span><span class="sxs-lookup"><span data-stu-id="b8209-177">You only want one instance of the StockTicker class to run on the server, so you'll need to set up a reference from each StockTickerHub instance to the singleton StockTicker instance.</span></span> <span data-ttu-id="b8209-178">株価データには、トリガーの更新プログラム、StockTicker はハブ クラスではないために、クライアントにブロードキャストできる StockTicker クラスにあります。</span><span class="sxs-lookup"><span data-stu-id="b8209-178">The StockTicker class has to be able to broadcast to clients because it has the stock data and triggers updates, but StockTicker is not a Hub class.</span></span> <span data-ttu-id="b8209-179">そのため、StockTicker クラスは、SignalR ハブの接続コンテキスト オブジェクトへの参照を取得しています。</span><span class="sxs-lookup"><span data-stu-id="b8209-179">Therefore, the StockTicker class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="b8209-180">クライアントにブロードキャストする SignalR 接続のコンテキスト オブジェクトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="b8209-180">It can then use the SignalR connection context object to broadcast to clients.</span></span>

1. <span data-ttu-id="b8209-181">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして、**追加 |SignalR ハブ クラス (v2)** します。</span><span class="sxs-lookup"><span data-stu-id="b8209-181">In **Solution Explorer**, right-click the project and click **Add | SignalR Hub Class (v2)**.</span></span>
2. <span data-ttu-id="b8209-182">新しいハブの名前を付けます*StockTickerHub.cs*、 をクリックし、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="b8209-182">Name the new hub *StockTickerHub.cs*, and then click **Add**.</span></span> <span data-ttu-id="b8209-183">SignalR の NuGet パッケージをプロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-183">SignalR NuGet packages will be added to your project.</span></span>
3. <span data-ttu-id="b8209-184">テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b8209-184">Replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

    <span data-ttu-id="b8209-185">[ハブ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)メソッドは、クライアントは、サーバー上で呼び出すことができますを定義するクラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="b8209-185">The [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class is used to define methods the clients can call on the server.</span></span> <span data-ttu-id="b8209-186">1 つのメソッドを定義する:`GetAllStocks()`します。</span><span class="sxs-lookup"><span data-stu-id="b8209-186">You are defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="b8209-187">クライアントは、最初に、サーバーに接続するときは、その現在の価格の株式のすべての一覧を取得するには、このメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b8209-187">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="b8209-188">メソッドは同期的に実行し、返す`IEnumerable<Stock>`メモリからデータを返すためです。</span><span class="sxs-lookup"><span data-stu-id="b8209-188">The method can execute synchronously and return `IEnumerable<Stock>` because it is returning data from memory.</span></span> <span data-ttu-id="b8209-189">指定したかどうか、メソッドがデータベース検索など、web サービス呼び出しの待機を含むは何かの手順を実行してデータを取得する必要がある`Task<IEnumerable<Stock>>`非同期処理を有効にする戻り値として。</span><span class="sxs-lookup"><span data-stu-id="b8209-189">If the method had to get the data by doing something that would involve waiting, such as a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="b8209-190">詳細については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーの非同期的に実行するタイミング](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods)します。</span><span class="sxs-lookup"><span data-stu-id="b8209-190">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

    <span data-ttu-id="b8209-191">HubName 属性は、クライアントでの JavaScript コードでのハブの参照方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="b8209-191">The HubName attribute specifies how the Hub will be referenced in JavaScript code on the client.</span></span> <span data-ttu-id="b8209-192">この属性を使用しない場合、クライアントに既定の名前は、ここで stockTickerHub 可能性のあるクラス名の camel 形式のバージョンです。</span><span class="sxs-lookup"><span data-stu-id="b8209-192">The default name on the client if you don't use this attribute is a camel-cased version of the class name, which in this case would be stockTickerHub.</span></span>

    <span data-ttu-id="b8209-193">StockTicker クラスを作成するときに、後で表示されます、としては、そのクラスのシングルトン インスタンスがその静的インスタンス プロパティに作成されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-193">As you'll see later when you create the StockTicker class, a singleton instance of that class is created in its static Instance property.</span></span> <span data-ttu-id="b8209-194">StockTicker のシングルトン インスタンスまたは切断するには、接続のクライアントの数に関係なくメモリに残りますをそのインスタンスが GetAllStocks メソッドを使用して、現在の株価情報が返されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-194">That singleton instance of StockTicker remains in memory no matter how many clients connect or disconnect, and that instance is what the GetAllStocks method uses to return current stock information.</span></span>
4. <span data-ttu-id="b8209-195">プロジェクト フォルダーに新しいクラス ファイルを作成、名前を付けます*StockTicker.cs*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b8209-195">Create a new class file in the project folder, name it *StockTicker.cs*, and then replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

    <span data-ttu-id="b8209-196">複数のスレッドが StockTicker コードの同じインスタンスを実行するため、スレッド セーフにする StockTicker クラスがあります。</span><span class="sxs-lookup"><span data-stu-id="b8209-196">Since multiple threads will be running the same instance of StockTicker code, the StockTicker class has to be threadsafe.</span></span>

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="b8209-197">静的フィールドに、シングルトン インスタンスを格納します。</span><span class="sxs-lookup"><span data-stu-id="b8209-197">Storing the singleton instance in a static field</span></span>

    <span data-ttu-id="b8209-198">静的な初期化\_コンス トラクターがプライベートとしてマークされているために、クラスでのインスタンスとインスタンスのプロパティをバックするインスタンス フィールドが作成できるクラスの唯一のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="b8209-198">The code initializes the static \_instance field that backs the Instance property with an instance of the class, and this is the only instance of the class that can be created, because the constructor is marked as private.</span></span> <span data-ttu-id="b8209-199">[遅延初期化](https://msdn.microsoft.com/library/dd997286.aspx)の使用は、\_するインスタンスの作成がスレッド セーフであることを確認しますが、パフォーマンス上の理由は、インスタンス フィールドです。</span><span class="sxs-lookup"><span data-stu-id="b8209-199">[Lazy initialization](https://msdn.microsoft.com/library/dd997286.aspx) is used for the \_instance field, not for performance reasons but to ensure that the instance creation is threadsafe.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

    <span data-ttu-id="b8209-200">クライアントがサーバーに接続するたびに個別のスレッドで実行される StockTickerHub クラスの新しいインスタンスする StockTickerHub クラスで既に見たよう StockTicker のシングルトン インスタンスが StockTicker.Instance の静的プロパティから取得します。</span><span class="sxs-lookup"><span data-stu-id="b8209-200">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the StockTicker.Instance static property, as you saw earlier in the StockTickerHub class.</span></span>

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="b8209-201">ConcurrentDictionary の株価データを格納します。</span><span class="sxs-lookup"><span data-stu-id="b8209-201">Storing stock data in a ConcurrentDictionary</span></span>

    <span data-ttu-id="b8209-202">コンス トラクターによって初期化、\_いくつかのサンプルの株価データ、および GetAllStocks stocks コレクションが、株式を返します。</span><span class="sxs-lookup"><span data-stu-id="b8209-202">The constructor initializes the \_stocks collection with some sample stock data, and GetAllStocks returns the stocks.</span></span> <span data-ttu-id="b8209-203">既に説明したよう株式のこのコレクションがさらにクライアントが呼び出すことができる、ハブ クラスにサーバーのメソッドである StockTickerHub.GetAllStocks によって返されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-203">As you saw earlier, this collection of stocks is in turn returned by StockTickerHub.GetAllStocks which is a server method in the Hub class that clients can call.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

    <span data-ttu-id="b8209-204">Stocks コレクションとは見なさ、 [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx)スレッド セーフの型。</span><span class="sxs-lookup"><span data-stu-id="b8209-204">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="b8209-205">別の方法として使用できます、[ディクショナリ](https://msdn.microsoft.com/library/xfhwa508.aspx)オブジェクトし、それを変更するときに明示的にディクショナリをロックします。</span><span class="sxs-lookup"><span data-stu-id="b8209-205">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

    <span data-ttu-id="b8209-206">このサンプル アプリケーションは OK StockTicker インスタンスが破棄されたときに、データが失われるとアプリケーション データをメモリに格納します。</span><span class="sxs-lookup"><span data-stu-id="b8209-206">For this sample application, it's OK to store application data in memory and to lose the data when the StockTicker instance is disposed.</span></span> <span data-ttu-id="b8209-207">実際のアプリケーションでは、データベースなどのバックエンド データ ストアと動作します。</span><span class="sxs-lookup"><span data-stu-id="b8209-207">In a real application you would work with a back-end data store such as a database.</span></span>

    ### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="b8209-208">株価を定期的に更新</span><span class="sxs-lookup"><span data-stu-id="b8209-208">Periodically updating stock prices</span></span>

    <span data-ttu-id="b8209-209">コンス トラクターは、ランダムな単位で株価を更新するメソッドを定期的に呼び出すタイマー オブジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="b8209-209">The constructor starts up a Timer object that periodically calls methods that update stock prices on a random basis.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

    <span data-ttu-id="b8209-210">UpdateStockPrices は、state パラメーターで null を渡すと、タイマーによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-210">UpdateStockPrices is called by the Timer, which passes in null in the state parameter.</span></span> <span data-ttu-id="b8209-211">価格を更新する前にロックを取得、 \_updateStockPricesLock オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b8209-211">Before updating prices, a lock is taken on the \_updateStockPricesLock object.</span></span> <span data-ttu-id="b8209-212">別のスレッドが価格を更新中で既にと、各株式の一覧で TryUpdateStockPrice を呼び出すかどうか、コードを確認します。</span><span class="sxs-lookup"><span data-stu-id="b8209-212">The code checks if another thread is already updating prices, and then it calls TryUpdateStockPrice on each stock in the list.</span></span> <span data-ttu-id="b8209-213">TryUpdateStockPrice メソッドは、株価を変更するかどうかを決定し、これを変更する量。</span><span class="sxs-lookup"><span data-stu-id="b8209-213">The TryUpdateStockPrice method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="b8209-214">株式の価格が変更された場合は、接続されているすべてのクライアントに株価の変更をブロードキャストする BroadcastStockPrice が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-214">If the stock price is changed, BroadcastStockPrice is called to broadcast the stock price change to all connected clients.</span></span>

    <span data-ttu-id="b8209-215">\_UpdatingStockPrices フラグがマーク[揮発性](https://msdn.microsoft.com/library/x13ttww7.aspx)へのアクセスがスレッド セーフであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b8209-215">The \_updatingStockPrices flag is marked as [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to ensure that access to it is threadsafe.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

    <span data-ttu-id="b8209-216">実際のアプリケーションで TryUpdateStockPrice メソッドが、価格を検索する web サービスを呼び出すとこのコードでは、ランダムに変更するのに乱数ジェネレーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="b8209-216">In a real application, the TryUpdateStockPrice method would call a web service to look up the price; in this code it uses a random number generator to make changes randomly.</span></span>

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="b8209-217">StockTicker クラスは、クライアントにブロードキャストできるように、SignalR のコンテキストを取得します。</span><span class="sxs-lookup"><span data-stu-id="b8209-217">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

    <span data-ttu-id="b8209-218">料金の変更は、StockTicker オブジェクトでは、ここに送信される、ためこれは、接続されているすべてのクライアントで、updateStockPrice メソッドを呼び出す必要があるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b8209-218">Because the price changes originate here in the StockTicker object, this is the object that needs to call an updateStockPrice method on all connected clients.</span></span> <span data-ttu-id="b8209-219">クライアントのメソッドを呼び出すための API のあるハブ クラスでは、StockTicker ハブ クラスから派生していないと、任意のハブ オブジェクトへの参照はありません。</span><span class="sxs-lookup"><span data-stu-id="b8209-219">In a Hub class you have an API for calling client methods, but StockTicker does not derive from the Hub class and does not have a reference to any Hub object.</span></span> <span data-ttu-id="b8209-220">そのため、接続されているクライアントにブロードキャストするためには、StockTickerHub クラスの SignalR コンテキストのインスタンスを取得し、クライアントでメソッドの呼び出しに使用するに StockTicker クラスがあります。</span><span class="sxs-lookup"><span data-stu-id="b8209-220">Therefore, in order to broadcast to connected clients, the StockTicker class has to get the SignalR context instance for the StockTickerHub class and use that to call methods on clients.</span></span>

    <span data-ttu-id="b8209-221">コンス トラクターに渡すを参照する、シングルトン クラス インスタンスを作成するときに、コードが SignalR コンテキストへの参照を取得し、コンス トラクターは、クライアントのプロパティ内に配置します。</span><span class="sxs-lookup"><span data-stu-id="b8209-221">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the Clients property.</span></span>

    <span data-ttu-id="b8209-222">2 つの理由の 1 回だけ、コンテキストを取得する理由がある: 負荷の高い操作では、コンテキストを取得して、1 回取得することにより、クライアントに送信されるメッセージの目的の順序が保持されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-222">There are two reasons why you want to get the context just once: getting the context is an expensive operation, and getting it once ensures that the intended order of messages sent to clients is preserved.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

    <span data-ttu-id="b8209-223">コンテキストのクライアント プロパティを取得し、StockTickerClient プロパティで、ハブ クラスの場合と同様、同じ検索するメソッドを呼び出すクライアント コードを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="b8209-223">Getting the Clients property of the context and putting it in the StockTickerClient property lets you write code to call client methods that looks the same as it would in a Hub class.</span></span> <span data-ttu-id="b8209-224">たとえば、すべてのクライアントにブロードキャストする Clients.All.updateStockPrice(stock) を記述できます。</span><span class="sxs-lookup"><span data-stu-id="b8209-224">For instance, to broadcast to all clients you can write Clients.All.updateStockPrice(stock).</span></span>

    <span data-ttu-id="b8209-225">BroadcastStockPrice で呼び出している updateStockPrice メソッドがまだ存在しません。後で追加するクライアントで実行されるコードを記述するときにします。</span><span class="sxs-lookup"><span data-stu-id="b8209-225">The updateStockPrice method that you are calling in BroadcastStockPrice doesn't exist yet; you'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="b8209-226">Clients.All は動的で、実行時に、式が評価されることを意味しているために、updateStockPrice ここを参照できます。</span><span class="sxs-lookup"><span data-stu-id="b8209-226">You can refer to updateStockPrice here because Clients.All is dynamic, which means the expression will be evaluated at runtime.</span></span> <span data-ttu-id="b8209-227">このメソッドの呼び出しが実行されると、SignalR はクライアントに送信メソッド名とパラメーターの値、およびクライアントに updateStockPrice という名前のメソッドがある場合は、そのメソッドが呼び出され、それに渡されるパラメーターの値。</span><span class="sxs-lookup"><span data-stu-id="b8209-227">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named updateStockPrice, that method will be called and the parameter value will be passed to it.</span></span>

    <span data-ttu-id="b8209-228">Clients.All では、すべてのクライアントに送信を意味します。</span><span class="sxs-lookup"><span data-stu-id="b8209-228">Clients.All means send to all clients.</span></span> <span data-ttu-id="b8209-229">SignalR では、その他のオプションのクライアントまたはクライアントに送信するグループを指定できます。</span><span class="sxs-lookup"><span data-stu-id="b8209-229">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="b8209-230">詳細については、次を参照してください。 [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="b8209-230">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="b8209-231">SignalR のルートを登録します。</span><span class="sxs-lookup"><span data-stu-id="b8209-231">Register the SignalR route</span></span>

<span data-ttu-id="b8209-232">サーバーは、途中受信および SignalR への直接する URL を把握する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8209-232">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="b8209-233">そのためには、OWIN startup クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="b8209-233">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="b8209-234">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |OWIN Startup クラス**します。</span><span class="sxs-lookup"><span data-stu-id="b8209-234">In **Solution Explorer**, right-click the project, and then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="b8209-235">クラスの名前**Startup.cs**します。</span><span class="sxs-lookup"><span data-stu-id="b8209-235">Name the class **Startup.cs**.</span></span>
2. <span data-ttu-id="b8209-236">コードに置き換えます**Startup.cs**次です。</span><span class="sxs-lookup"><span data-stu-id="b8209-236">Replace the code in **Startup.cs** with the following.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="b8209-237">サーバー コードのセットアップが完了しました。</span><span class="sxs-lookup"><span data-stu-id="b8209-237">You have now completed setting up the server code.</span></span> <span data-ttu-id="b8209-238">次のセクションでは、クライアントを設定します。</span><span class="sxs-lookup"><span data-stu-id="b8209-238">In the next section you'll set up the client.</span></span>

<a id="client"></a>

## <a name="set-up-the-client-code"></a><span data-ttu-id="b8209-239">クライアント コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="b8209-239">Set up the client code</span></span>

1. <span data-ttu-id="b8209-240">プロジェクト フォルダーに新しい HTML ファイルを作成し、名前*StockTicker.html*します。</span><span class="sxs-lookup"><span data-stu-id="b8209-240">Create a new HTML file in the project folder, and name it *StockTicker.html*.</span></span>
2. <span data-ttu-id="b8209-241">テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b8209-241">Replace the template code with the following code.</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html)]

    <span data-ttu-id="b8209-242">HTML では、5 つの列、ヘッダー行では、5 つすべての列にまたがる単一のセルにデータ行とテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="b8209-242">The HTML creates a table with 5 columns, a header row, and a data row with a single cell that spans all 5 columns.</span></span> <span data-ttu-id="b8209-243">データ行は、「読み込み中...」が表示され、アプリケーションの起動時に一時的にのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-243">The data row displays "loading..." and will only be shown momentarily when the application starts.</span></span> <span data-ttu-id="b8209-244">JavaScript コードはその行を削除して、サーバーから取得した株価データでその場所の行に追加します。</span><span class="sxs-lookup"><span data-stu-id="b8209-244">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="b8209-245">スクリプト タグは、jQuery スクリプト ファイル、SignalR core のスクリプト ファイル、SignalR プロキシ スクリプト ファイル、および後で作成する StockTicker スクリプト ファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="b8209-245">The script tags specify the jQuery script file, the SignalR core script file, the SignalR proxies script file, and a StockTicker script file that you'll create later.</span></span> <span data-ttu-id="b8209-246">「/Signalr ハブ」の URL を指定するには、SignalR プロキシ スクリプト ファイルが動的に生成され、StockTickerHub.GetAllStocks をここで、ハブ クラス上のメソッドをプロキシのメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="b8209-246">The SignalR proxies script file, which specifies the "/signalr/hubs" URL, is dynamically generated and defines proxy methods for the methods on the Hub class, in this case for StockTickerHub.GetAllStocks.</span></span> <span data-ttu-id="b8209-247">使用してこの JavaScript ファイルが手動で生成できる場合は、 [SignalR ユーティリティ](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)MapHubs メソッドの呼び出しで動的ファイルの作成を無効にするとします。</span><span class="sxs-lookup"><span data-stu-id="b8209-247">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) and disable dynamic file creation in the MapHubs method call.</span></span>
3. > [!IMPORTANT]
   > <span data-ttu-id="b8209-248">JavaScript ファイルを参照しているかどうかを確認*StockTicker.html*が正しい。</span><span class="sxs-lookup"><span data-stu-id="b8209-248">Make sure that the JavaScript file references in *StockTicker.html* are correct.</span></span> <span data-ttu-id="b8209-249">つまり、jQuery バージョン、スクリプト タグ (例では 1.10.2) では、プロジェクトの jQuery バージョンと同じであることを確認*スクリプト*フォルダー、スクリプト タグで SignalR バージョンは、SignalR と同じかどうかを確認してプロジェクトのバージョン*スクリプト*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="b8209-249">That is, make sure that the jQuery version in your script tag (1.10.2 in the example) is the same as the jQuery version in your project's *Scripts* folder, and make sure that the SignalR version in your script tag is the same as the SignalR version in your project's *Scripts* folder.</span></span> <span data-ttu-id="b8209-250">必要な場合は、スクリプト タグ内のファイル名を変更します。</span><span class="sxs-lookup"><span data-stu-id="b8209-250">Change the file names in the script tags if necessary.</span></span>
4. <span data-ttu-id="b8209-251">**ソリューション エクスプ ローラー**、右クリック*StockTicker.html*、 をクリックし、**スタート ページとして設定**します。</span><span class="sxs-lookup"><span data-stu-id="b8209-251">In **Solution Explorer**, right-click *StockTicker.html*, and then click **Set as Start Page**.</span></span>
5. <span data-ttu-id="b8209-252">プロジェクト フォルダーで新しい JavaScript ファイルを作成し、名前*StockTicker.js*.</span><span class="sxs-lookup"><span data-stu-id="b8209-252">Create a new JavaScript file in the project folder and name it *StockTicker.js*..</span></span>
6. <span data-ttu-id="b8209-253">テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b8209-253">Replace the template code with the following code:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

    <span data-ttu-id="b8209-254">$.connection は SignalR プロキシを表します。</span><span class="sxs-lookup"><span data-stu-id="b8209-254">$.connection refers to the SignalR proxies.</span></span> <span data-ttu-id="b8209-255">コードでは、StockTickerHub クラスのプロキシへの参照を取得し、ティッカー変数内に配置します。</span><span class="sxs-lookup"><span data-stu-id="b8209-255">The code gets a reference to the proxy for the StockTickerHub class and puts it in the ticker variable.</span></span> <span data-ttu-id="b8209-256">プロキシの名前は、[HubName] 属性によって設定された名前を示します。</span><span class="sxs-lookup"><span data-stu-id="b8209-256">The proxy name is the name that was set by the [HubName] attribute:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

    <span data-ttu-id="b8209-257">すべての変数と関数を定義した後、ファイル内のコードの最後の行は SignalR の start 関数を呼び出すことによって、SignalR 接続を初期化します。</span><span class="sxs-lookup"><span data-stu-id="b8209-257">After all the variables and functions are defined, the last line of code in the file initializes the SignalR connection by calling the SignalR start function.</span></span> <span data-ttu-id="b8209-258">Start 関数が非同期的に実行し、返します、 [jQuery 遅延オブジェクト](http://api.jquery.com/category/deferred-object/)、することができる非同期操作が完了したときに呼び出す関数を指定する元に戻す関数を呼び出すことができます.</span><span class="sxs-lookup"><span data-stu-id="b8209-258">The start function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means you can call the done function to specify the function to call when the asynchronous operation is completed..</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

    <span data-ttu-id="b8209-259">Init 関数は、ストックのテーブルを更新するサーバーが返す情報を使用して、サーバーで getAllStocks 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b8209-259">The init function calls the getAllStocks function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="b8209-260">既定では、あるメソッド名は pascal 形式で表記するサーバーがクライアントで camel 規約に従った大文字を使用することを確認します。</span><span class="sxs-lookup"><span data-stu-id="b8209-260">Notice that by default, you have to use camel casing on the client although the method name is pascal-cased on the server.</span></span> <span data-ttu-id="b8209-261">キャメル形式の規則は、オブジェクトではなく、メソッドにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-261">The camel-casing rule only applies to methods, not objects.</span></span> <span data-ttu-id="b8209-262">たとえば、在庫を参照してください。シンボルと在庫です。価格、stock.symbol または stock.price します。</span><span class="sxs-lookup"><span data-stu-id="b8209-262">For example, you refer to stock.Symbol and stock.Price, not stock.symbol or stock.price.</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

    <span data-ttu-id="b8209-263">クライアントでは、pascal 形式の大文字と小文字を使用するかは HubMethodName 属性でのハブ メソッドを修飾することがまったく異なるメソッドの名前を使用する場合は、HubName 属性を持つハブ クラス自体を装飾します。</span><span class="sxs-lookup"><span data-stu-id="b8209-263">If you wanted to use pascal casing on the client, or if you wanted to use a completely different method name, you could decorate the Hub method with the HubMethodName attribute the same way you decorated the Hub class itself with the HubName attribute.</span></span>

    <span data-ttu-id="b8209-264">Init メソッドでの HTML テーブルの行はストックのオブジェクトの書式設定のプロパティを呼び出し元 formatStock によってサーバーから受け取った各株オブジェクトに対して作成され、脅かすを呼び出した (の上部で定義されている*StockTicker.js*) オブジェクトのストック プロパティの値を持つ rowTemplate 変数内のプレース ホルダーを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b8209-264">In the init method, HTML for a table row is created for each stock object received from the server by calling formatStock to format properties of the stock object, and then by calling supplant (which is defined at the top of *StockTicker.js*) to replace placeholders in the rowTemplate variable with the stock object property values.</span></span> <span data-ttu-id="b8209-265">結果の HTML は、株価のテーブルに追加されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-265">The resulting HTML is then appended to the stock table.</span></span>

    <span data-ttu-id="b8209-266">非同期の start 関数の完了後に実行されるコールバック関数として渡すことによって、init を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b8209-266">You call init by passing it in as a callback function that executes after the asynchronous start function completes.</span></span> <span data-ttu-id="b8209-267">Init を呼び出すと、開始を呼び出した後、別の JavaScript ステートメントとして、接続の確立を完了するのには start 関数を待たずにすぐに実行があるため、関数は失敗します。</span><span class="sxs-lookup"><span data-stu-id="b8209-267">If you called init as a separate JavaScript statement after calling start, the function would fail because it would execute immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="b8209-268">その場合は、init 関数では、サーバー接続が確立される前に、getAllStocks 関数を呼び出すとします。</span><span class="sxs-lookup"><span data-stu-id="b8209-268">In that case, the init function would try to call the getAllStocks function before the server connection is established.</span></span>

    <span data-ttu-id="b8209-269">サーバー、株の価格が変更されると、接続されているクライアントで、updateStockPrice を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b8209-269">When the server changes a stock's price, it calls the updateStockPrice on connected clients.</span></span> <span data-ttu-id="b8209-270">関数は、使用できるように、呼び出しに、サーバーから stockTicker プロキシのクライアントのプロパティに追加されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-270">The function is added to the client property of the stockTicker proxy in order to make it available to calls from the server.</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

    <span data-ttu-id="b8209-271">UpdateStockPrice 関数は、テーブルの行に init 関数と同様、サーバーから受け取ったストック オブジェクトを書式設定します。</span><span class="sxs-lookup"><span data-stu-id="b8209-271">The updateStockPrice function formats a stock object received from the server into a table row the same way as in the init function.</span></span> <span data-ttu-id="b8209-272">ただし、テーブルに行を追加することではなく、表内の株式の現在の行を検索し、その行を新しいものに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b8209-272">However, instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

<a id="test"></a>

## <a name="test-the-application"></a><span data-ttu-id="b8209-273">アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="b8209-273">Test the application</span></span>

1. <span data-ttu-id="b8209-274">F5 キーを押して、デバッグ モードでアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="b8209-274">Press F5 to run the application in debug mode.</span></span>

    <span data-ttu-id="b8209-275">ストックの表に、最初に表示されます、「読み込み中...」の行、初期の株価データを表示すると、短い遅延の後し、株式の価格を変更します。</span><span class="sxs-lookup"><span data-stu-id="b8209-275">The stock table initially displays the "loading..." line, then after a short delay the initial stock data is displayed, and then the stock prices start to change.</span></span>

    ![[読み込み中]](tutorial-server-broadcast-with-signalr/_static/image6.png)

    ![初期の在庫テーブル](tutorial-server-broadcast-with-signalr/_static/image7.png)

    ![サーバーからの変更を受け取るストック テーブル](tutorial-server-broadcast-with-signalr/_static/image8.png)
2. <span data-ttu-id="b8209-279">ブラウザーのアドレス バーから URL をコピーして、1 つまたは複数の新しいブラウザー ウィンドウに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="b8209-279">Copy the URL from the browser address bar and paste it into one or more new browser window(s).</span></span>

    <span data-ttu-id="b8209-280">初期の株価表示は、最初のブラウザーと同じと同時に変更が行われます。</span><span class="sxs-lookup"><span data-stu-id="b8209-280">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>
3. <span data-ttu-id="b8209-281">すべてのブラウザーを閉じるし、新しいブラウザーを開いて、同じ URL に移動します。</span><span class="sxs-lookup"><span data-stu-id="b8209-281">Close all browsers and open a new browser, then go to the same URL.</span></span>

    <span data-ttu-id="b8209-282">StockTicker シングルトン オブジェクトは、在庫表の表示が、株式を変更し続けていることを示しています、サーバーで実行を続けています。</span><span class="sxs-lookup"><span data-stu-id="b8209-282">The StockTicker singleton object has continued to run in the server, so the stock table display shows that the stocks have continued to change.</span></span> <span data-ttu-id="b8209-283">(図形を変更、初期テーブルのゼロは表示されません)。</span><span class="sxs-lookup"><span data-stu-id="b8209-283">(You don't see the initial table with zero change figures.)</span></span>
4. <span data-ttu-id="b8209-284">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="b8209-284">Close the browser.</span></span>

<a id="enablelogging"></a>

## <a name="enable-logging"></a><span data-ttu-id="b8209-285">ログの有効化</span><span class="sxs-lookup"><span data-stu-id="b8209-285">Enable logging</span></span>

<span data-ttu-id="b8209-286">SignalR では、トラブルシューティングで支援するために、クライアントで有効にできる組み込みのログ記録関数があります。</span><span class="sxs-lookup"><span data-stu-id="b8209-286">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="b8209-287">このセクションでは、ログ記録を有効にし、ログを知る方法を次のトランスポート メソッドの SignalR を使用して表示する例を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b8209-287">In this section you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

- <span data-ttu-id="b8209-288">[Websocket](http://en.wikipedia.org/wiki/WebSocket)IIS 8 と現在のブラウザーでサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="b8209-288">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>
- <span data-ttu-id="b8209-289">[サーバー送信イベント](http://en.wikipedia.org/wiki/Server-sent_events)、Internet Explorer 以外のブラウザーでサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="b8209-289">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>
- <span data-ttu-id="b8209-290">[フレームの永久](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)、Internet Explorer でサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="b8209-290">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>
- <span data-ttu-id="b8209-291">[長いポーリングの Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)、すべてのブラウザーでサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="b8209-291">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="b8209-292">特定の接続では、SignalR は、サーバーとクライアントの両方をサポートする最適なトランスポートの方法を選択します。</span><span class="sxs-lookup"><span data-stu-id="b8209-292">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="b8209-293">開いている*StockTicker.js*し、ファイルの最後に、接続を初期化するコードの直前のログ記録を有効にするコードの行を追加します。</span><span class="sxs-lookup"><span data-stu-id="b8209-293">Open *StockTicker.js* and add a line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js)]
2. <span data-ttu-id="b8209-294">F5 キーを押してプロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="b8209-294">Press F5 to run the project.</span></span>
3. <span data-ttu-id="b8209-295">ブラウザーの開発者ツール ウィンドウを開きし、ログを表示するコンソールを選択します。</span><span class="sxs-lookup"><span data-stu-id="b8209-295">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="b8209-296">新しい接続の転送方法をネゴシエートする Signalr のログを表示するページを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8209-296">You might have to refresh the page to see the logs of Signalr negotiating the transport method for a new connection.</span></span>

    <span data-ttu-id="b8209-297">Windows 8 (IIS 8) で Internet Explorer 10 を実行している場合は、Websocket は、トランスポート メソッド。</span><span class="sxs-lookup"><span data-stu-id="b8209-297">If you are running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is WebSockets.</span></span>

    ![IE 10 IIS 8 コンソールします。](tutorial-server-broadcast-with-signalr/_static/image9.png)

    <span data-ttu-id="b8209-299">Windows 7 (IIS 7.5) で Internet Explorer 10 を実行している場合は、iframe はトランスポート メソッドです。</span><span class="sxs-lookup"><span data-stu-id="b8209-299">If you are running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is iframe.</span></span>

    ![IE 10 コンソールで、IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image10.png)

    <span data-ttu-id="b8209-301">Firefox の場合、コンソール ウィンドウを取得する Firebug アドインのインストールします。</span><span class="sxs-lookup"><span data-stu-id="b8209-301">In Firefox, install the Firebug add-in to get a Console window.</span></span> <span data-ttu-id="b8209-302">Firefox 19 を Windows 8 (IIS 8) を実行する場合は、Websocket はトランスポート メソッドです。</span><span class="sxs-lookup"><span data-stu-id="b8209-302">If you are running Firefox 19 on Windows 8 (IIS 8), the transport method is WebSockets.</span></span>

    ![Firefox 19 IIS 8 Websocket](tutorial-server-broadcast-with-signalr/_static/image11.png)

    <span data-ttu-id="b8209-304">Firefox 19 を Windows 7 (IIS 7.5) を実行する場合は、サーバー送信イベントが、トランスポート メソッド。</span><span class="sxs-lookup"><span data-stu-id="b8209-304">If you are running Firefox 19 on Windows 7 (IIS 7.5), the transport method is server-sent events.</span></span>

    ![Firefox 19 IIS 7.5 コンソールします。](tutorial-server-broadcast-with-signalr/_static/image12.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a><span data-ttu-id="b8209-306">インストールして、完全な StockTicker サンプルを確認してください。</span><span class="sxs-lookup"><span data-stu-id="b8209-306">Install and review the full StockTicker sample</span></span>

<span data-ttu-id="b8209-307">StockTicker アプリケーションがインストールされている、 [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet パッケージには、最初から作成した簡略化されたバージョンより多くの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b8209-307">The StockTicker application that is installed by the [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet package includes more features than the simplified version that you just created from scratch.</span></span> <span data-ttu-id="b8209-308">チュートリアルのこのセクションでは、NuGet パッケージをインストールし、新機能とそれらを実装するコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="b8209-308">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span> <span data-ttu-id="b8209-309">このチュートリアルの前の手順を実行せず、パッケージをインストールする場合は、プロジェクトに OWIN startup クラスを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8209-309">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="b8209-310">この手順は、NuGet パッケージの readme.txt ファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="b8209-310">This step is explained in the readme.txt file for the NuGet package.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="b8209-311">SignalR.Sample NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="b8209-311">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="b8209-312">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="b8209-312">In **Solution Explorer**, right-click the project and click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="b8209-313">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**オンライン**、入力*SignalR.Sample*で、**オンライン検索**ボックス、および順にクリックします**インストール**で、 **SignalR.Sample**パッケージ。</span><span class="sxs-lookup"><span data-stu-id="b8209-313">In the **Manage NuGet Packages** dialog box, click **Online**, enter *SignalR.Sample* in the **Search Online** box, and then click **Install** in the **SignalR.Sample** package.</span></span>

    ![SignalR.Sample パッケージをインストールします。](tutorial-server-broadcast-with-signalr/_static/image13.png)
3. <span data-ttu-id="b8209-315">**ソリューション エクスプ ローラー**、展開、 *SignalR.Sample* SignalR.Sample パッケージをインストールすることによって作成されたフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b8209-315">In **Solution Explorer**, expand the *SignalR.Sample* folder which was created by installing the SignalR.Sample package.</span></span>
4. <span data-ttu-id="b8209-316">*SignalR.Sample*フォルダーを右クリックして*StockTicker.html*、 をクリックし、**スタート ページとして設定**。</span><span class="sxs-lookup"><span data-stu-id="b8209-316">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then click **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b8209-317">SignalR.Sample NuGet のインストール パッケージは内にある jQuery のバージョンを変更可能性があります、*スクリプト*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="b8209-317">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="b8209-318">新しい*StockTicker.html*でパッケージをインストールするファイル、 *SignalR.Sample*フォルダーが、元のを実行する場合は、パッケージをインストールするjQueryバージョンとの同期に*StockTicker.html*ファイルを再び、最初にスクリプト タグ内の jQuery 参照を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8209-318">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="b8209-319">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="b8209-319">Run the application</span></span>

1. <span data-ttu-id="b8209-320">F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="b8209-320">Press F5 to run the application.</span></span>

    <span data-ttu-id="b8209-321">先ほど見たグリッド、だけでなくは、完全な株価表示器のアプリケーションには、同じの株価データを表示する水平方向にスクロール ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-321">In addition to the grid that you saw earlier, the full stock ticker application shows a horizontally scrolling window that displays the same stock data.</span></span> <span data-ttu-id="b8209-322">最初にアプリケーションを実行して、「市場」は"closed"静的グリッドとティッカー ウィンドウ スクロールはありませんを参照してください。</span><span class="sxs-lookup"><span data-stu-id="b8209-322">When you run the application for the first time, the "market" is "closed" and you see a static grid and a ticker window that isn't scrolling.</span></span>

    ![StockTicker 画面の開始](tutorial-server-broadcast-with-signalr/_static/image14.png)

    <span data-ttu-id="b8209-324">クリックすると**オープン市場**、**株式ティッカーの Live**を水平方向にスクロール ボックスを起動し、サーバーがランダムに株価の変更を定期的にブロードキャストする開始します。</span><span class="sxs-lookup"><span data-stu-id="b8209-324">When you click **Open Market**, the **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span> <span data-ttu-id="b8209-325">株価たびに変更する場合に、両方、**在庫表の Live**グリッドと**株式ティッカーの Live**ボックスが更新されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-325">Each time a stock price changes, both the **Live Stock Table** grid and the **Live Stock Ticker** box are updated.</span></span> <span data-ttu-id="b8209-326">株式の価格の変更が正、素材は緑の背景で表示され、背景が赤の素材が示すように、変更は、負の値が、場合。</span><span class="sxs-lookup"><span data-stu-id="b8209-326">When a stock's price change is positive, the stock is shown with a green background, and when the change is negative, the stock is shown with a red background.</span></span>

    ![StockTicker アプリ市場を開く](tutorial-server-broadcast-with-signalr/_static/image15.png)

    <span data-ttu-id="b8209-328">**閉じる市場**ボタンは、変更を停止し、ティッカー スクロールが停止し、**リセット**の料金の変更を開始する前にボタンが初期状態にすべての株価データをリセットします。</span><span class="sxs-lookup"><span data-stu-id="b8209-328">The **Close Market** button stops the changes and stops the ticker scrolling, and the **Reset** button resets all stock data to the initial state before price changes started.</span></span> <span data-ttu-id="b8209-329">複数のブラウザー ウィンドウを開くし、同じ URL に移動する場合、各ブラウザーで同時に動的に更新される同じデータを参照してください。</span><span class="sxs-lookup"><span data-stu-id="b8209-329">If you open more browser windows and go to the same URL, you see the same data dynamically updated at the same time in each browser.</span></span> <span data-ttu-id="b8209-330">クリックすると、ボタンのいずれかのブラウザーと同時に同じ方法もあります。</span><span class="sxs-lookup"><span data-stu-id="b8209-330">When you click one of the buttons, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="b8209-331">ライブの株式相場表示</span><span class="sxs-lookup"><span data-stu-id="b8209-331">Live Stock Ticker display</span></span>

<span data-ttu-id="b8209-332">**株式ティッカーの Live**ディスプレイが CSS スタイルによって 1 行に書式設定されている div 要素の順序なしのリスト。</span><span class="sxs-lookup"><span data-stu-id="b8209-332">The **Live Stock Ticker** display is an unordered list in a div element that is formatted into a single line by CSS styles.</span></span> <span data-ttu-id="b8209-333">ティッカーは初期化され、テーブルと同様の更新: 内のプレース ホルダーを置き換えることで、 &lt;li&gt;テンプレート文字列と動的に追加する、 &lt;li&gt;要素を&lt;ul&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="b8209-333">The ticker is initialized and updated the same way as the table: by replacing placeholders in a &lt;li&gt; template string and dynamically adding the &lt;li&gt; elements to the &lt;ul&gt; element.</span></span> <span data-ttu-id="b8209-334">Div 内で順序付けられていない一覧の左余白を変更するため、jQuery アニメーション化する関数を使用して、実行は、スクロール</span><span class="sxs-lookup"><span data-stu-id="b8209-334">The scrolling is performed by using the jQuery animate function to vary the margin-left of the unordered list within the div.</span></span>

<span data-ttu-id="b8209-335">株価情報 HTML:</span><span class="sxs-lookup"><span data-stu-id="b8209-335">The stock ticker HTML:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

<span data-ttu-id="b8209-336">株価表示器 CSS で:</span><span class="sxs-lookup"><span data-stu-id="b8209-336">The stock ticker CSS:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

<span data-ttu-id="b8209-337">JQuery コードがスクロールします。</span><span class="sxs-lookup"><span data-stu-id="b8209-337">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="b8209-338">クライアントが呼び出すことができる、サーバーで追加のメソッド</span><span class="sxs-lookup"><span data-stu-id="b8209-338">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="b8209-339">StockTickerHub クラスには、クライアントが呼び出すことができる 4 つの追加のメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="b8209-339">The StockTickerHub class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="b8209-340">OpenMarket、CloseMarket、およびリセットは、ページの上部にあるボタンへの応答で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-340">OpenMarket, CloseMarket, and Reset are called in response to the buttons at the top of the page.</span></span> <span data-ttu-id="b8209-341">これらは、すべてのクライアントにすぐに反映される状態の変化をトリガーする 1 つのクライアントのパターンを示します。</span><span class="sxs-lookup"><span data-stu-id="b8209-341">They demonstrate the pattern of one client triggering a change in state that is immediately propagated to all clients.</span></span> <span data-ttu-id="b8209-342">これらの各メソッドでメソッドを呼び出し、StockTicker クラス市場の状態を変更して、新しい状態をブロードキャストし、その効果。</span><span class="sxs-lookup"><span data-stu-id="b8209-342">Each of these methods calls a method in the StockTicker class that effects the market state change and then broadcasts the new state.</span></span>

<span data-ttu-id="b8209-343">StockTicker クラスで MarketState 列挙型の値を返す MarketState プロパティで、市場の状態が維持されます。</span><span class="sxs-lookup"><span data-stu-id="b8209-343">In the StockTicker class, the state of the market is maintained by a MarketState property that returns a MarketState enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="b8209-344">各市場の状態を変更する方法は、ロック ブロックの内部 StockTicker クラスがあるスレッド セーフのためです。</span><span class="sxs-lookup"><span data-stu-id="b8209-344">Each of the methods that change the market state do so inside a lock block because the StockTicker class has to be threadsafe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="b8209-345">このコードが、スレッド セーフであることを確認する、 \_MarketState プロパティをバックする marketState フィールドは、volatile としてマーク</span><span class="sxs-lookup"><span data-stu-id="b8209-345">To ensure that this code is threadsafe, the \_marketState field that backs the MarketState property is marked as volatile,</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="b8209-346">BroadcastMarketStateChange と BroadcastMarketReset メソッドは、クライアントで定義されている別のメソッドを呼び出す点を既に確認した BroadcastStockPrice メソッドに似ています。</span><span class="sxs-lookup"><span data-stu-id="b8209-346">The BroadcastMarketStateChange and BroadcastMarketReset methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="b8209-347">サーバーが呼び出すことができるクライアントの追加の関数</span><span class="sxs-lookup"><span data-stu-id="b8209-347">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="b8209-348">UpdateStockPrice 関数は、グリッドとティッカーの表示の両方を処理して、赤、緑の色をフラッシュする jQuery.Color を使用してください。</span><span class="sxs-lookup"><span data-stu-id="b8209-348">The updateStockPrice function now handles both the grid and the ticker display, and it uses jQuery.Color to flash red and green colors.</span></span>

<span data-ttu-id="b8209-349">新しい関数で*SignalR.StockTicker.js*有効にして、ボタンがに基づいて無効にする市場の状態や、停止やティッカー ウィンドウ水平スクロールを開始します。</span><span class="sxs-lookup"><span data-stu-id="b8209-349">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state, and they stop or start the ticker window horizontal scrolling.</span></span> <span data-ttu-id="b8209-350">Ticker.client に複数の関数が追加されているため、 [jQuery 関数を拡張する](http://api.jquery.com/jQuery.extend/)に追加するために使用します。</span><span class="sxs-lookup"><span data-stu-id="b8209-350">Since multiple functions are being added to ticker.client, the [jQuery extend function](http://api.jquery.com/jQuery.extend/) is used to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="b8209-351">接続の確立後に追加のクライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="b8209-351">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="b8209-352">追加の作業を行うには、クライアント接続を確立した後: オープンかクローズ、適切な marketOpened または marketClosed 関数を呼び出すし、ボタンにサーバー メソッドの呼び出しをアタッチするには、市場がかどうかかを確認します。</span><span class="sxs-lookup"><span data-stu-id="b8209-352">After the client establishes the connection, it has some additional work to do: find out if the market is open or closed in order to call the appropriate marketOpened or marketClosed function, and attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="b8209-353">サーバー メソッドがないワイヤード (有線) までボタンまで、接続が確立されると、利用される前にサーバー メソッドを呼び出すしようとすることはできません、コードを。</span><span class="sxs-lookup"><span data-stu-id="b8209-353">The server methods are not wired up to the buttons until after the connection is established, so that the code can't try to call the server methods before they are available.</span></span>

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="b8209-354">次の手順</span><span class="sxs-lookup"><span data-stu-id="b8209-354">Next steps</span></span>

<span data-ttu-id="b8209-355">このチュートリアルでは、接続されているすべてのクライアント、定期的と任意のクライアントからの通知に応答の両方に、サーバーからメッセージをブロードキャストする SignalR アプリケーションをプログラミングする方法を学習できました。</span><span class="sxs-lookup"><span data-stu-id="b8209-355">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients, both on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="b8209-356">マルチ スレッドのシングルトン インスタンスを使用して、サーバーの状態を保持するパターンも、マルチ プレーヤー オンライン ゲーム シナリオで使用こともできます。</span><span class="sxs-lookup"><span data-stu-id="b8209-356">The pattern of using a multi-threaded singleton instance to maintain server state can also be also used in multi-player online game scenarios.</span></span> <span data-ttu-id="b8209-357">例については、次を参照してください。 [SignalR に基づいている ShootR ゲーム](https://github.com/NTaylorMullen/ShootR)します。</span><span class="sxs-lookup"><span data-stu-id="b8209-357">For an example, see [the ShootR game that is based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="b8209-358">ピア ツー ピア通信シナリオを示すチュートリアルについては、次を参照してください。 [SignalR の概要](introduction-to-signalr.md)と[SignalR によるリアルタイムの更新](tutorial-high-frequency-realtime-with-signalr.md)します。</span><span class="sxs-lookup"><span data-stu-id="b8209-358">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="b8209-359">SignalR 開発のより高度な概念については、SignalR のソース コードおよびリソースは、次のサイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="b8209-359">To learn more advanced SignalR development concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="b8209-360">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="b8209-360">ASP.NET SignalR</span></span>](../../index.md)
- [<span data-ttu-id="b8209-361">SignalR プロジェクト</span><span class="sxs-lookup"><span data-stu-id="b8209-361">SignalR Project</span></span>](http://signalr.net/)
- [<span data-ttu-id="b8209-362">SignalR Github とサンプル</span><span class="sxs-lookup"><span data-stu-id="b8209-362">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="b8209-363">SignalR の Wiki</span><span class="sxs-lookup"><span data-stu-id="b8209-363">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

<span data-ttu-id="b8209-364">SignalR アプリケーションを Azure にデプロイする方法のチュートリアルは、次を参照してください。 [Azure App Service Web apps を使用して SignalR](../deployment/using-signalr-with-azure-web-sites.md)します。</span><span class="sxs-lookup"><span data-stu-id="b8209-364">For a walkthrough on how to deploy a SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="b8209-365">Visual Studio web プロジェクトを Windows Azure の Web サイトにデプロイする方法の詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。</span><span class="sxs-lookup"><span data-stu-id="b8209-365">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
