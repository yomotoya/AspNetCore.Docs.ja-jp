---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: "チュートリアル: サーバーは signalr 2 ブロードキャスト |Microsoft ドキュメント"
author: tdykstra
description: "このチュートリアルでは、ASP.NET SignalR 2 を使用して、サーバーのブロードキャストの機能を提供する web アプリケーションを作成する方法を示します。 サーバーのブロードキャストでは、その commun を意味しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 98a7ce4991d58181177cf56976888e9fd1526987
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="32eb4-104">チュートリアル: SignalR 2 でサーバーのブロードキャスト</span><span class="sxs-lookup"><span data-stu-id="32eb4-104">Tutorial: Server Broadcast with SignalR 2</span></span>
====================
<span data-ttu-id="32eb4-105">によって[Tom Dykstra](https://github.com/tdykstra)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="32eb4-105">by [Tom Dykstra](https://github.com/tdykstra), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="32eb4-106">このチュートリアルでは、ASP.NET SignalR 2 を使用して、サーバーのブロードキャストの機能を提供する web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-106">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="32eb4-107">サーバーのブロードキャストでは、サーバーによってクライアントに送信される通信が開始されたことを意味します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-107">Server broadcast means that communications sent to clients are initiated by the server.</span></span> <span data-ttu-id="32eb4-108">このシナリオでは、クライアントに送信される通信が 1 つまたは複数のクライアントによって開始された、チャット アプリケーションなどのピア ツー ピア シナリオは異なるプログラミング方法が必要です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-108">This scenario requires a different programming approach than peer-to-peer scenarios such as chat applications, in which communications sent to clients are initiated by one or more of the clients.</span></span>
> 
> <span data-ttu-id="32eb4-109">このチュートリアルで作成するアプリケーションでは、株価情報、サーバー機能をブロードキャストの一般的なシナリオをシミュレートします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-109">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span>
> 
> <span data-ttu-id="32eb4-110">このトピックは、Patrick Fletcher によって最初書き込まれました。</span><span class="sxs-lookup"><span data-stu-id="32eb4-110">This topic was originally written by Patrick Fletcher.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="32eb4-111">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="32eb4-111">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="32eb4-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="32eb4-112">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="32eb4-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="32eb4-113">.NET 4.5</span></span>
> - <span data-ttu-id="32eb4-114">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="32eb4-114">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="32eb4-115">このチュートリアルで Visual Studio 2012 を使用します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-115">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="32eb4-116">このチュートリアルでは、Visual Studio 2012 を使用するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="32eb4-116">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="32eb4-117">更新プログラム、 [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget)最新バージョンにします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-117">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="32eb4-118">インストール、 [Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-118">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="32eb4-119">Web Platform Installer で検索し、インストール**ASP.NET と Visual Studio 2012 用 Web ツール 2013.1**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-119">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="32eb4-120">SignalR クラス用の Visual Studio テンプレートなどのインストールはこの**ハブ**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-120">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="32eb4-121">一部のテンプレート (など**OWIN スタートアップ クラス**) できなくなります。 これらの場合には、クラス ファイルを代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-121">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="32eb4-122">チュートリアルのバージョン</span><span class="sxs-lookup"><span data-stu-id="32eb4-122">Tutorial Versions</span></span>
> 
> <span data-ttu-id="32eb4-123">SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-123">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="32eb4-124">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="32eb4-124">Questions and comments</span></span>
> 
> <span data-ttu-id="32eb4-125">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="32eb4-125">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="32eb4-126">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-126">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="32eb4-127">概要</span><span class="sxs-lookup"><span data-stu-id="32eb4-127">Overview</span></span>

<span data-ttu-id="32eb4-128">このチュートリアルでは定期的に「プッシュ」するリアルタイム アプリケーションの担当者は、株価表示器のアプリケーションを作成したりする、接続されているすべてのクライアントにサーバーからの通知をブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-128">In this tutorial, you'll create a stock ticker application that is representative of real-time applications in which you want to periodically "push," or broadcast, notifications from the server to all connected clients.</span></span> <span data-ttu-id="32eb4-129">このチュートリアルの最初の部分では、最初からそのアプリケーションの簡素化されたバージョンを作成します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-129">In the first part of this tutorial, you'll create a simplified version of that application from scratch.</span></span> <span data-ttu-id="32eb4-130">チュートリアルの残りの部分を追加の機能を含む NuGet パッケージをインストールし、それらの機能のコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-130">In the remainder of the tutorial, you'll install a NuGet package that contains additional features, and review the code for those features.</span></span>

<span data-ttu-id="32eb4-131">このチュートリアルの最初の部分でビルドしたアプリケーションには、株価データを持つグリッドが表示されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-131">The application that you'll build in the first part of this tutorial displays a grid with stock data.</span></span>

![StockTicker 初期バージョン](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="32eb4-133">定期的にサーバーをランダムに株価を更新し、更新プログラムをすべて接続されているクライアントにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-133">Periodically the server randomly updates stock prices and pushes the updates to all connected clients.</span></span> <span data-ttu-id="32eb4-134">ブラウザーの数値および内のシンボルで、**変更**と **%** 列は、サーバーからの通知に応答で動的に変化します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-134">In the browser the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="32eb4-135">同じ URL に他のブラウザーを開いた場合、同じデータとデータへの同じ変更を同時に、それらはすべて表示します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-135">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

<span data-ttu-id="32eb4-136">このチュートリアルでは、次のセクションでは、含まれています。</span><span class="sxs-lookup"><span data-stu-id="32eb4-136">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="32eb4-137">前提条件</span><span class="sxs-lookup"><span data-stu-id="32eb4-137">Prerequisites</span></span>](#prerequisites)
- [<span data-ttu-id="32eb4-138">プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-138">Create the project</span></span>](#createproject)
- [<span data-ttu-id="32eb4-139">サーバー コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-139">Set up the server code</span></span>](#server)
- [<span data-ttu-id="32eb4-140">クライアント コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-140">Set up the client code</span></span>](#client)
- [<span data-ttu-id="32eb4-141">アプリケーションをテストします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-141">Test the application</span></span>](#test)
- [<span data-ttu-id="32eb4-142">ログ記録を有効にします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-142">Enable logging</span></span>](#enablelogging)
- [<span data-ttu-id="32eb4-143">インストールし、完全なサンプルを StockTicker を確認</span><span class="sxs-lookup"><span data-stu-id="32eb4-143">Install and review the full StockTicker sample</span></span>](#fullsample)
- [<span data-ttu-id="32eb4-144">次のステップ</span><span class="sxs-lookup"><span data-stu-id="32eb4-144">Next steps</span></span>](#nextsteps)

<span data-ttu-id="32eb4-145">[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet パッケージは、Visual Studio プロジェクトにシミュレートされたサンプルの株価表示器のアプリケーションをインストールします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-145">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet package installs a sample simulated stock ticker application in a Visual Studio project.</span></span>

> [!NOTE]
> <span data-ttu-id="32eb4-146">アプリケーションのビルドの手順を実行しない場合は、新しい空の ASP.NET Web アプリケーション プロジェクトで SignalR.Sample パッケージをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-146">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="32eb4-147">このチュートリアルの手順を実行せず、NuGet パッケージをインストールした場合**readme.txt ファイルの指示に従う必要があります**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-147">If you install the NuGet package without performing the steps in this tutorial, **you must follow the instructions in the readme.txt file**.</span></span> <span data-ttu-id="32eb4-148">パッケージを実行するには、インストールされているパッケージで ConfigureSignalR メソッドを呼び出す OWIN スタートアップ クラスを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="32eb4-148">To run the package you need to add an OWIN startup class which calls the ConfigureSignalR method in the installed package.</span></span> <span data-ttu-id="32eb4-149">OWIN 起動クラスを追加しない場合、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-149">You will receive an error if you do not add the OWIN startup class.</span></span>


<a id="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="32eb4-150">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="32eb4-150">Prerequisites</span></span>

<span data-ttu-id="32eb4-151">開始する前に、コンピューターにインストールされている Visual Studio 2013 があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-151">Before you start, make sure that you have Visual Studio 2013 installed on your computer.</span></span> <span data-ttu-id="32eb4-152">Visual Studio をお持ちでない場合は、次を参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)、空き Visual Studio 2013 Express を取得します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-152">If you don't have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express.</span></span>

<a id="createproject"></a>

## <a name="create-the-project"></a><span data-ttu-id="32eb4-153">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="32eb4-153">Create the project</span></span>

1. <span data-ttu-id="32eb4-154">**ファイル** メニューをクリックして**新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-154">From the **File** menu, click **New Project**.</span></span>
2. <span data-ttu-id="32eb4-155">**新しいプロジェクト**] ダイアログ ボックスで、展開**c#** [**テンプレート**選択と**Web**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-155">In the **New Project** dialog box, expand **C#** under **Templates** and select **Web**.</span></span>
3. <span data-ttu-id="32eb4-156">選択、**空の ASP.NET Web アプリケーション**名では、プロジェクト テンプレートは、 *SignalR.StockTicker*、 をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-156">Select the **ASP.NET Empty Web Application** template, name the project *SignalR.StockTicker*, and click **OK**.</span></span>

    ![[新しいプロジェクト] ダイアログ ボックス](tutorial-server-broadcast-with-signalr/_static/image2.png)
4. <span data-ttu-id="32eb4-158">**新しい ASP.NET**プロジェクト ウィンドウのままにして**空**を選択し、をクリックして**プロジェクトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-158">In the **New ASP.NET** Project window, leave **Empty** selected and click **Create Project**.</span></span>

    ![新しい ASP プロジェクト ダイアログ ボックス](tutorial-server-broadcast-with-signalr/_static/image3.png)

<a id="server"></a>

## <a name="set-up-the-server-code"></a><span data-ttu-id="32eb4-160">サーバー コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-160">Set up the server code</span></span>

<span data-ttu-id="32eb4-161">このセクションでは、サーバーで実行されるコードを設定します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-161">In this section you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="32eb4-162">ストック クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-162">Create the Stock class</span></span>

<span data-ttu-id="32eb4-163">保存し、株式に関する情報を送信するときに使用する株価モデル クラスを作成して開始するとします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-163">You begin by creating the Stock model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="32eb4-164">プロジェクト フォルダーに新しいクラス ファイルを作成、名前を付けます*Stock.cs*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-164">Create a new class file in the project folder, name it *Stock.cs*, and then replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="32eb4-165">株式を作成するときに設定する 2 つのプロパティは、シンボル (たとえば、Microsoft の MSFT) 価格。</span><span class="sxs-lookup"><span data-stu-id="32eb4-165">The two properties that you'll set when you create stocks are the Symbol (for example, MSFT for Microsoft) and the Price.</span></span> <span data-ttu-id="32eb4-166">その他のプロパティは、価格を設定する方法とタイミングに依存します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-166">The other properties depend on how and when you set Price.</span></span> <span data-ttu-id="32eb4-167">初めての価格を設定する値は取得 DayOpen に反映されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-167">The first time you set Price, the value gets propagated to DayOpen.</span></span> <span data-ttu-id="32eb4-168">価格、変更を設定して PercentChange プロパティ値を計算するときに 2 回目以降は、価格と DayOpen の違いに基づいています。</span><span class="sxs-lookup"><span data-stu-id="32eb4-168">Subsequent times when you set Price, the Change and PercentChange property values are calculated based on the difference between Price and DayOpen.</span></span>

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a><span data-ttu-id="32eb4-169">StockTicker と StockTickerHub のクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-169">Create the StockTicker and StockTickerHub classes</span></span>

<span data-ttu-id="32eb4-170">サーバーからクライアントへの対話を処理するのにには、SignalR ハブの API を使用します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-170">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="32eb4-171">SignalR ハブ クラスから派生した StockTickerHub クラスでは、クライアントから接続し、メソッド呼び出しの受信を処理します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-171">A StockTickerHub class that derives from the SignalR Hub class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="32eb4-172">また、株価データを維持し、クライアント接続とは無関係に、価格の更新プログラムを定期的にトリガーするタイマー オブジェクトを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="32eb4-172">You also need to maintain stock data and run a Timer object to periodically trigger price updates, independently of client connections.</span></span> <span data-ttu-id="32eb4-173">ハブ インスタンスを一時的なために、ハブ クラスでこれらの関数を配置することはできません。</span><span class="sxs-lookup"><span data-stu-id="32eb4-173">You can't put these functions in a Hub class, because Hub instances are transient.</span></span> <span data-ttu-id="32eb4-174">ハブで、接続と、クライアントからサーバーへの呼び出しなどの操作ごとに、ハブ クラスのインスタンスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-174">A Hub class instance is created for each operation on the hub, such as connections and calls from the client to the server.</span></span> <span data-ttu-id="32eb4-175">株価データを保持、価格を更新し、価格の更新プログラムをブロードキャストするメカニズム StockTicker 名前別のクラスで実行するようにします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-175">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class, which you'll name StockTicker.</span></span>

![StockTicker からブロードキャスト](tutorial-server-broadcast-with-signalr/_static/image5.png)

<span data-ttu-id="32eb4-177">のみを行う各 StockTickerHub インスタンスからシングルトン StockTicker インスタンスへの参照を設定する必要がありますので、サーバー上で実行する StockTicker クラスの 1 つのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="32eb4-177">You only want one instance of the StockTicker class to run on the server, so you'll need to set up a reference from each StockTickerHub instance to the singleton StockTicker instance.</span></span> <span data-ttu-id="32eb4-178">StockTicker がハブ クラスではありませんが、株価データを持ちの更新をトリガーするために、クライアントにブロードキャストできる StockTicker クラスにあります。</span><span class="sxs-lookup"><span data-stu-id="32eb4-178">The StockTicker class has to be able to broadcast to clients because it has the stock data and triggers updates, but StockTicker is not a Hub class.</span></span> <span data-ttu-id="32eb4-179">したがって、SignalR ハブ接続のコンテキストのオブジェクトへの参照を取得する StockTicker クラスがあります。</span><span class="sxs-lookup"><span data-stu-id="32eb4-179">Therefore, the StockTicker class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="32eb4-180">SignalR 接続コンテキスト オブジェクトを使用して、クライアントにブロードキャストをそのことができます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-180">It can then use the SignalR connection context object to broadcast to clients.</span></span>

1. <span data-ttu-id="32eb4-181">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして、**追加 |SignalR ハブ クラス (v2)**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-181">In **Solution Explorer**, right-click the project and click **Add | SignalR Hub Class (v2)**.</span></span>
2. <span data-ttu-id="32eb4-182">新しいハブの名前を付けます*StockTickerHub.cs*、クリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-182">Name the new hub *StockTickerHub.cs*, and then click **Add**.</span></span> <span data-ttu-id="32eb4-183">SignalR の NuGet パッケージは、プロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-183">SignalR NuGet packages will be added to your project.</span></span>
3. <span data-ttu-id="32eb4-184">テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-184">Replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

    <span data-ttu-id="32eb4-185">[ハブ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)クライアントがサーバーで呼び出せるメソッドを定義するクラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-185">The [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class is used to define methods the clients can call on the server.</span></span> <span data-ttu-id="32eb4-186">1 つのメソッドを定義する:`GetAllStocks()`です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-186">You are defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="32eb4-187">クライアントは、最初に、サーバーに接続するとき、現在の価格の株式のすべての一覧を取得するには、このメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-187">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="32eb4-188">メソッドは同期的に実行でき、返す`IEnumerable<Stock>`メモリからデータを返すことがあるためです。</span><span class="sxs-lookup"><span data-stu-id="32eb4-188">The method can execute synchronously and return `IEnumerable<Stock>` because it is returning data from memory.</span></span> <span data-ttu-id="32eb4-189">データベースの参照など、web サービス呼び出しの待機も含まれるものの手順を実行してデータを取得するメソッドが持っているかどうかは指定`Task<IEnumerable<Stock>>`非同期処理を有効にする戻り値として。</span><span class="sxs-lookup"><span data-stu-id="32eb4-189">If the method had to get the data by doing something that would involve waiting, such as a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="32eb4-190">詳細については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーの非同期的に実行するタイミング](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods)です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-190">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

    <span data-ttu-id="32eb4-191">HubName 属性は、クライアントでの JavaScript コードでのハブの参照方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-191">The HubName attribute specifies how the Hub will be referenced in JavaScript code on the client.</span></span> <span data-ttu-id="32eb4-192">この属性を使用しない場合、クライアントに既定の名前は、ここではある stockTickerHub クラス名の camel 形式のバージョンです。</span><span class="sxs-lookup"><span data-stu-id="32eb4-192">The default name on the client if you don't use this attribute is a camel-cased version of the class name, which in this case would be stockTickerHub.</span></span>

    <span data-ttu-id="32eb4-193">StockTicker クラスを作成するときに、後で表示されますと静的インスタンス プロパティにそのクラスのシングルトン インスタンスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-193">As you'll see later when you create the StockTicker class, a singleton instance of that class is created in its static Instance property.</span></span> <span data-ttu-id="32eb4-194">StockTicker のシングルトン インスタンスはクライアントの数が接続または切断してに関係なくメモリに残りますをそのインスタンスが現在の在庫情報を返す GetAllStocks メソッドの使用します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-194">That singleton instance of StockTicker remains in memory no matter how many clients connect or disconnect, and that instance is what the GetAllStocks method uses to return current stock information.</span></span>
4. <span data-ttu-id="32eb4-195">プロジェクト フォルダーに新しいクラス ファイルを作成、名前を付けます*StockTicker.cs*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-195">Create a new class file in the project folder, name it *StockTicker.cs*, and then replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

    <span data-ttu-id="32eb4-196">複数のスレッドが StockTicker コードの同じインスタンスを実行するため、スレッド セーフである StockTicker クラスがあります。</span><span class="sxs-lookup"><span data-stu-id="32eb4-196">Since multiple threads will be running the same instance of StockTicker code, the StockTicker class has to be threadsafe.</span></span>

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="32eb4-197">静的フィールドに、シングルトン インスタンスを格納します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-197">Storing the singleton instance in a static field</span></span>

    <span data-ttu-id="32eb4-198">コードは、初期化、静的な\_コンス トラクターがプライベートとしてマークされているために、クラス、およびこれのインスタンスとインスタンスのプロパティをバックするインスタンス フィールドが作成できるクラスの唯一のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="32eb4-198">The code initializes the static \_instance field that backs the Instance property with an instance of the class, and this is the only instance of the class that can be created, because the constructor is marked as private.</span></span> <span data-ttu-id="32eb4-199">[遅延初期化](https://msdn.microsoft.com/library/dd997286.aspx)の使用、\_するインスタンスの作成がスレッド セーフであることを確認しますが、パフォーマンス上の理由は、インスタンス フィールドです。</span><span class="sxs-lookup"><span data-stu-id="32eb4-199">[Lazy initialization](https://msdn.microsoft.com/library/dd997286.aspx) is used for the \_instance field, not for performance reasons but to ensure that the instance creation is threadsafe.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

    <span data-ttu-id="32eb4-200">クライアントがサーバーに接続するたびに個別のスレッドで実行される StockTickerHub クラスの新しいインスタンスする StockTickerHub クラスで既に説明したとおり StockTicker シングルトン インスタンスが、StockTicker.Instance 静的プロパティから取得します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-200">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the StockTicker.Instance static property, as you saw earlier in the StockTickerHub class.</span></span>

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="32eb4-201">ConcurrentDictionary の株価データを格納します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-201">Storing stock data in a ConcurrentDictionary</span></span>

    <span data-ttu-id="32eb4-202">コンス トラクターは、\_のいくつかのサンプルの株価データと GetAllStocks 株式コレクションには、株式が返されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-202">The constructor initializes the \_stocks collection with some sample stock data, and GetAllStocks returns the stocks.</span></span> <span data-ttu-id="32eb4-203">前述の株式のこのコレクションがさらに、サーバー クラスのメソッド、ハブ クライアントが呼び出すことができるは StockTickerHub.GetAllStocks によって返されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-203">As you saw earlier, this collection of stocks is in turn returned by StockTickerHub.GetAllStocks which is a server method in the Hub class that clients can call.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

    <span data-ttu-id="32eb4-204">株式コレクションとは見なさ、 [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx)スレッド セーフの型。</span><span class="sxs-lookup"><span data-stu-id="32eb4-204">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="32eb4-205">代わりに、使用する可能性があります、[ディクショナリ](https://msdn.microsoft.com/library/xfhwa508.aspx)オブジェクトを明示的にそれを変更するときに、ディクショナリをロックします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-205">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

    <span data-ttu-id="32eb4-206">このサンプル アプリケーションは [ok] をメモリ内でアプリケーション データを格納し、StockTicker インスタンスが破棄されるときに、データが失われるです。</span><span class="sxs-lookup"><span data-stu-id="32eb4-206">For this sample application, it's OK to store application data in memory and to lose the data when the StockTicker instance is disposed.</span></span> <span data-ttu-id="32eb4-207">実際のアプリケーションでは、データベースなどのバックエンド データ ストアで作業する場合します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-207">In a real application you would work with a back-end data store such as a database.</span></span>

    ### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="32eb4-208">株価を定期的に更新</span><span class="sxs-lookup"><span data-stu-id="32eb4-208">Periodically updating stock prices</span></span>

    <span data-ttu-id="32eb4-209">コンス トラクターを定期的にランダムに株価を更新するメソッドを呼び出すタイマー オブジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-209">The constructor starts up a Timer object that periodically calls methods that update stock prices on a random basis.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

    <span data-ttu-id="32eb4-210">UpdateStockPrices は state パラメーターに null を渡すと、タイマーによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-210">UpdateStockPrices is called by the Timer, which passes in null in the state parameter.</span></span> <span data-ttu-id="32eb4-211">価格を更新する前に、ロックを取得、 \_updateStockPricesLock オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="32eb4-211">Before updating prices, a lock is taken on the \_updateStockPricesLock object.</span></span> <span data-ttu-id="32eb4-212">コードは、別のスレッドが価格を更新中で既にし、一覧には、各銘柄の TryUpdateStockPrice が呼び出すかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-212">The code checks if another thread is already updating prices, and then it calls TryUpdateStockPrice on each stock in the list.</span></span> <span data-ttu-id="32eb4-213">TryUpdateStockPrice メソッドは、株価を変更するかどうかを決定し、これを変更する量。</span><span class="sxs-lookup"><span data-stu-id="32eb4-213">The TryUpdateStockPrice method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="32eb4-214">株価を変更すると、株式の価格の変更をすべて接続されているクライアントにブロードキャストする BroadcastStockPrice が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-214">If the stock price is changed, BroadcastStockPrice is called to broadcast the stock price change to all connected clients.</span></span>

    <span data-ttu-id="32eb4-215">\_UpdatingStockPrices フラグ マークが付いている[揮発性](https://msdn.microsoft.com/library/x13ttww7.aspx)へのアクセスがスレッド セーフであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-215">The \_updatingStockPrices flag is marked as [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to ensure that access to it is threadsafe.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

    <span data-ttu-id="32eb4-216">TryUpdateStockPrice メソッドで実際のアプリケーションで、価格を検索する web サービスの呼び出しはこのコードでは、ランダムに変更するのに乱数ジェネレーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-216">In a real application, the TryUpdateStockPrice method would call a web service to look up the price; in this code it uses a random number generator to make changes randomly.</span></span>

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="32eb4-217">StockTicker クラスは、クライアントにブロードキャストできるように、SignalR コンテキストを取得します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-217">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

    <span data-ttu-id="32eb4-218">価格の変動は発信 StockTicker オブジェクトで次に、ためこれは、接続されているすべてのクライアントで、updateStockPrice メソッドを呼び出す必要があるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="32eb4-218">Because the price changes originate here in the StockTicker object, this is the object that needs to call an updateStockPrice method on all connected clients.</span></span> <span data-ttu-id="32eb4-219">ハブ クラスで、クライアントのメソッド呼び出しの API があるが、StockTicker ハブ クラスから派生していないと、ハブ オブジェクトへの参照はありません。</span><span class="sxs-lookup"><span data-stu-id="32eb4-219">In a Hub class you have an API for calling client methods, but StockTicker does not derive from the Hub class and does not have a reference to any Hub object.</span></span> <span data-ttu-id="32eb4-220">したがって、接続しているクライアントにブロードキャストするためには、StockTickerHub クラスの SignalR コンテキストのインスタンスを取得し、クライアントでメソッドの呼び出しに使用するに StockTicker クラスがあります。</span><span class="sxs-lookup"><span data-stu-id="32eb4-220">Therefore, in order to broadcast to connected clients, the StockTicker class has to get the SignalR context instance for the StockTickerHub class and use that to call methods on clients.</span></span>

    <span data-ttu-id="32eb4-221">コードが、コンス トラクターにパスを参照する、シングルトン クラス インスタンスを作成するとき、SignalR コンテキストへの参照を取得し、コンス トラクターは、クライアントのプロパティに格納します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-221">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the Clients property.</span></span>

    <span data-ttu-id="32eb4-222">2 つの理由がコンテキストの 1 回だけ取得する理由がある: コンテキストを取得することはコストのかかる操作、および 1 回取得はによりそのクライアントに送信されたメッセージの目的の順序は維持されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-222">There are two reasons why you want to get the context just once: getting the context is an expensive operation, and getting it once ensures that the intended order of messages sent to clients is preserved.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

    <span data-ttu-id="32eb4-223">コンテキストのクライアントのプロパティを取得して、StockTickerClient プロパティに配置したり、ハブ クラスと同じように見える同じメソッドを呼び出すクライアント コードを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-223">Getting the Clients property of the context and putting it in the StockTickerClient property lets you write code to call client methods that looks the same as it would in a Hub class.</span></span> <span data-ttu-id="32eb4-224">たとえば、すべてのクライアントにブロードキャストする Clients.All.updateStockPrice(stock) を記述できます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-224">For instance, to broadcast to all clients you can write Clients.All.updateStockPrice(stock).</span></span>

    <span data-ttu-id="32eb4-225">BroadcastStockPrice で呼び出している updateStockPrice メソッドがまだ存在しません。後で追加するクライアントで実行されるコードを記述するときにします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-225">The updateStockPrice method that you are calling in BroadcastStockPrice doesn't exist yet; you'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="32eb4-226">Clients.All は動的で、実行時に式が評価されることを意味するので、updateStockPrice ここを参照できます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-226">You can refer to updateStockPrice here because Clients.All is dynamic, which means the expression will be evaluated at runtime.</span></span> <span data-ttu-id="32eb4-227">このメソッドの呼び出しが実行されると、SignalR はクライアントに送信メソッド名とパラメーターの値、および updateStockPrice をという名前のメソッドで、クライアントは、そのメソッドが呼び出されることに渡されるパラメーターの値。</span><span class="sxs-lookup"><span data-stu-id="32eb4-227">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named updateStockPrice, that method will be called and the parameter value will be passed to it.</span></span>

    <span data-ttu-id="32eb4-228">Clients.All では、すべてのクライアントに送信を意味します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-228">Clients.All means send to all clients.</span></span> <span data-ttu-id="32eb4-229">SignalR では、その他のオプションのクライアントまたはクライアントに送信するグループを指定できます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-229">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="32eb4-230">詳細については、次を参照してください。 [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-230">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="32eb4-231">SignalR のルートを登録します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-231">Register the SignalR route</span></span>

<span data-ttu-id="32eb4-232">サーバーを途中受信、および SignalR への直接 URL を知っている必要があります。</span><span class="sxs-lookup"><span data-stu-id="32eb4-232">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="32eb4-233">追加する操作と OWIN スタートアップ クラスします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-233">To do that you'll add and OWIN startup class.</span></span>

1. <span data-ttu-id="32eb4-234">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |OWIN 起動クラス**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-234">In **Solution Explorer**, right-click the project, and then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="32eb4-235">クラスの名前を付けます**Startup.cs**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-235">Name the class **Startup.cs**.</span></span>
2. <span data-ttu-id="32eb4-236">コードに置き換えます**Startup.cs**以下にします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-236">Replace the code in **Startup.cs** with the following.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="32eb4-237">サーバー コードのセットアップが完了しました。</span><span class="sxs-lookup"><span data-stu-id="32eb4-237">You have now completed setting up the server code.</span></span> <span data-ttu-id="32eb4-238">次のセクションでは、クライアントを設定します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-238">In the next section you'll set up the client.</span></span>

<a id="client"></a>

## <a name="set-up-the-client-code"></a><span data-ttu-id="32eb4-239">クライアント コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-239">Set up the client code</span></span>

1. <span data-ttu-id="32eb4-240">プロジェクト フォルダーに新しい HTML ファイルを作成し、名前*StockTicker.html*です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-240">Create a new HTML file in the project folder, and name it *StockTicker.html*.</span></span>
2. <span data-ttu-id="32eb4-241">テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-241">Replace the template code with the following code.</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html)]

    <span data-ttu-id="32eb4-242">HTML では、5 つの列、ヘッダー行では、5 つすべての列にまたがる 1 つのセルを持つデータ行とテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-242">The HTML creates a table with 5 columns, a header row, and a data row with a single cell that spans all 5 columns.</span></span> <span data-ttu-id="32eb4-243">データ行は、「読み込み中...」が表示され、アプリケーションの起動時にすぐにのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-243">The data row displays "loading..." and will only be shown momentarily when the application starts.</span></span> <span data-ttu-id="32eb4-244">JavaScript コードはその行を削除して、サーバーから取得した株価データをその場所の行に追加します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-244">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="32eb4-245">スクリプト タグは、jQuery スクリプト ファイル、SignalR core スクリプト ファイル、SignalR プロキシ スクリプト ファイル、および後で作成する StockTicker スクリプト ファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-245">The script tags specify the jQuery script file, the SignalR core script file, the SignalR proxies script file, and a StockTicker script file that you'll create later.</span></span> <span data-ttu-id="32eb4-246">「/Signalr ハブ」URL を指定するには、SignalR プロキシ スクリプト ファイルは、動的に生成され、StockTickerHub.GetAllStocks をここでは、ハブ クラスのメソッドのプロキシ メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-246">The SignalR proxies script file, which specifies the "/signalr/hubs" URL, is dynamically generated and defines proxy methods for the methods on the Hub class, in this case for StockTickerHub.GetAllStocks.</span></span> <span data-ttu-id="32eb4-247">使用してこの JavaScript ファイルが手動で生成できる場合は、 [SignalR ユーティリティ](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)MapHubs メソッドの呼び出しでの動的なファイルの作成を無効にするとします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-247">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) and disable dynamic file creation in the MapHubs method call.</span></span>
3. > [!IMPORTANT]
 > <span data-ttu-id="32eb4-248">JavaScript ファイルを参照するかどうかを確認*StockTicker.html*が正しい。</span><span class="sxs-lookup"><span data-stu-id="32eb4-248">Make sure that the JavaScript file references in *StockTicker.html* are correct.</span></span> <span data-ttu-id="32eb4-249">必ず、スクリプト タグ (1.10.2 の例) で jQuery のバージョンがプロジェクトの jQuery のバージョンと同じ*スクリプト*フォルダー、し、スクリプト タグの SignalR バージョンが、SignalR と同じであるかどうかを確認プロジェクトのバージョン*スクリプト*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="32eb4-249">That is, make sure that the jQuery version in your script tag (1.10.2 in the example) is the same as the jQuery version in your project's *Scripts* folder, and make sure that the SignalR version in your script tag is the same as the SignalR version in your project's *Scripts* folder.</span></span> <span data-ttu-id="32eb4-250">必要な場合は、スクリプト タグのファイル名を変更します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-250">Change the file names in the script tags if necessary.</span></span>
4. <span data-ttu-id="32eb4-251">**ソリューション エクスプ ローラー**を右クリックして*StockTicker.html*、クリックして**スタート ページとして設定**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-251">In **Solution Explorer**, right-click *StockTicker.html*, and then click **Set as Start Page**.</span></span>
5. <span data-ttu-id="32eb4-252">プロジェクト フォルダーで新しい JavaScript ファイルを作成し、名前*StockTicker.js*.</span><span class="sxs-lookup"><span data-stu-id="32eb4-252">Create a new JavaScript file in the project folder and name it *StockTicker.js*..</span></span>
6. <span data-ttu-id="32eb4-253">テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-253">Replace the template code with the following code:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

    <span data-ttu-id="32eb4-254">$.connection は、SignalR のプロキシを参照します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-254">$.connection refers to the SignalR proxies.</span></span> <span data-ttu-id="32eb4-255">コードでは、StockTickerHub クラスのプロキシへの参照を取得し、ティッカー変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-255">The code gets a reference to the proxy for the StockTickerHub class and puts it in the ticker variable.</span></span> <span data-ttu-id="32eb4-256">プロキシの名前は、[HubName] 属性によって設定された名前を示します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-256">The proxy name is the name that was set by the [HubName] attribute:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

    <span data-ttu-id="32eb4-257">すべての変数と関数を定義したら、ファイル内のコードの最後の行 SignalR start 関数を呼び出すことによって、SignalR 接続を初期化します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-257">After all the variables and functions are defined, the last line of code in the file initializes the SignalR connection by calling the SignalR start function.</span></span> <span data-ttu-id="32eb4-258">Start 関数が非同期的に実行しを返します、 [jQuery 延期オブジェクト](http://api.jquery.com/category/deferred-object/)、することができる非同期操作が完了したときに呼び出す関数を指定する元に戻す関数を呼び出すことができます.</span><span class="sxs-lookup"><span data-stu-id="32eb4-258">The start function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means you can call the done function to specify the function to call when the asynchronous operation is completed..</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

    <span data-ttu-id="32eb4-259">Init 関数では、サーバーで getAllStocks 関数を呼び出し、在庫テーブルを更新するサーバーが返される情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-259">The init function calls the getAllStocks function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="32eb4-260">既定では、ある camel 形式大文字小文字の区別、クライアントで、メソッド名は pascal 形式のサーバーで使用することを確認します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-260">Notice that by default, you have to use camel casing on the client although the method name is pascal-cased on the server.</span></span> <span data-ttu-id="32eb4-261">Camel 形式の大文字と小文字の規則は、メソッド、オブジェクトではなくのみに適用されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-261">The camel-casing rule only applies to methods, not objects.</span></span> <span data-ttu-id="32eb4-262">たとえば、在庫を参照してください。シンボルと在庫です。価格、いない stock.symbol または stock.price です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-262">For example, you refer to stock.Symbol and stock.Price, not stock.symbol or stock.price.</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

    <span data-ttu-id="32eb4-263">クライアントでは、pascal 文字種を使用するかと同じ方法 HubMethodName 属性を持つハブ メソッドを装飾する可能性がありますが完全に別のメソッド名を使用する場合は、HubName 属性を持つハブ クラス自体が修飾されています。</span><span class="sxs-lookup"><span data-stu-id="32eb4-263">If you wanted to use pascal casing on the client, or if you wanted to use a completely different method name, you could decorate the Hub method with the HubMethodName attribute the same way you decorated the Hub class itself with the HubName attribute.</span></span>

    <span data-ttu-id="32eb4-264">ストックのオブジェクトの書式設定のプロパティを呼び出し元 formatStock によって、サーバーから受け取ったストック オブジェクトごとに、初期化メソッドで、テーブルの行の HTML が作成され、置き換えるを呼び出した (の上部に定義されている*StockTicker.js*) オブジェクトのストック プロパティの値を持つ rowTemplate 変数内のプレース ホルダーを置換します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-264">In the init method, HTML for a table row is created for each stock object received from the server by calling formatStock to format properties of the stock object, and then by calling supplant (which is defined at the top of *StockTicker.js*) to replace placeholders in the rowTemplate variable with the stock object property values.</span></span> <span data-ttu-id="32eb4-265">結果の HTML は、株価のテーブルに追加されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-265">The resulting HTML is then appended to the stock table.</span></span>

    <span data-ttu-id="32eb4-266">非同期開始関数が完了した後に実行されるコールバック関数として渡すことによって初期化を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-266">You call init by passing it in as a callback function that executes after the asynchronous start function completes.</span></span> <span data-ttu-id="32eb4-267">Init を呼び出すと、開始の呼び出し後に別の JavaScript ステートメントは start 関数を接続の確立を完了を待たずにすぐに実行があるため、関数は失敗します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-267">If you called init as a separate JavaScript statement after calling start, the function would fail because it would execute immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="32eb4-268">その場合は、初期化関数を関数を呼び出す getAllStocks サーバー接続が確立される前に試行します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-268">In that case, the init function would try to call the getAllStocks function before the server connection is established.</span></span>

    <span data-ttu-id="32eb4-269">サーバーには、株式の価格が変更された、ときに、接続しているクライアントで、updateStockPrice を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-269">When the server changes a stock's price, it calls the updateStockPrice on connected clients.</span></span> <span data-ttu-id="32eb4-270">関数は、使用できるように、呼び出しに、サーバーから stockTicker プロキシのクライアントのプロパティに追加されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-270">The function is added to the client property of the stockTicker proxy in order to make it available to calls from the server.</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

    <span data-ttu-id="32eb4-271">UpdateStockPrice 関数は、テーブルの行に初期化関数と同じよう、サーバーから受け取ったストック オブジェクトを書式設定します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-271">The updateStockPrice function formats a stock object received from the server into a table row the same way as in the init function.</span></span> <span data-ttu-id="32eb4-272">ただし、テーブルに行を追加するのではなく、テーブル内の株式の現在の行を検索し、その行を新しいものに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-272">However, instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

<a id="test"></a>

## <a name="test-the-application"></a><span data-ttu-id="32eb4-273">アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="32eb4-273">Test the application</span></span>

1. <span data-ttu-id="32eb4-274">F5 キーを押してアプリケーションをデバッグ モードで実行します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-274">Press F5 to run the application in debug mode.</span></span>

    <span data-ttu-id="32eb4-275">ストックの表は最初に、「読み込み中...」行、し、初期の株価データを表示すると、短い遅延後に表示し、株式の価格を変更します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-275">The stock table initially displays the "loading..." line, then after a short delay the initial stock data is displayed, and then the stock prices start to change.</span></span>

    ![[読み込み中]](tutorial-server-broadcast-with-signalr/_static/image6.png)

    ![初期の在庫テーブル](tutorial-server-broadcast-with-signalr/_static/image7.png)

    ![サーバーからの変更を受け取るストック テーブル](tutorial-server-broadcast-with-signalr/_static/image8.png)
2. <span data-ttu-id="32eb4-279">ブラウザーのアドレス バーから URL をコピーし、1 つまたは複数の新しいブラウザー ウィンドウに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-279">Copy the URL from the browser address bar and paste it into one or more new browser window(s).</span></span>

    <span data-ttu-id="32eb4-280">最初の株価表示は、最初のブラウザーと同じとの変更が同時に発生します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-280">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>
3. <span data-ttu-id="32eb4-281">すべてのブラウザーを閉じて新しいブラウザー ウィンドウで、し同じ URL に移動します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-281">Close all browsers and open a new browser, then go to the same URL.</span></span>

    <span data-ttu-id="32eb4-282">StockTicker シングルトン オブジェクトは、在庫テーブルが表示された、株式が変更を続けているため、サーバーで実行を続けています。</span><span class="sxs-lookup"><span data-stu-id="32eb4-282">The StockTicker singleton object has continued to run in the server, so the stock table display shows that the stocks have continued to change.</span></span> <span data-ttu-id="32eb4-283">(図を変更する 0 に初期のテーブルを参照しない)。</span><span class="sxs-lookup"><span data-stu-id="32eb4-283">(You don't see the initial table with zero change figures.)</span></span>
4. <span data-ttu-id="32eb4-284">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-284">Close the browser.</span></span>

<a id="enablelogging"></a>

## <a name="enable-logging"></a><span data-ttu-id="32eb4-285">ログの有効化</span><span class="sxs-lookup"><span data-stu-id="32eb4-285">Enable logging</span></span>

<span data-ttu-id="32eb4-286">SignalR には、トラブルシューティングに支援するためにクライアントを有効にできる組み込みのログ記録機能があります。</span><span class="sxs-lookup"><span data-stu-id="32eb4-286">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="32eb4-287">このセクションでは、ログ記録を有効にして、ログ確認方法を次のトランスポート メソッドの SignalR を使用するかを示す例を参照してください。</span><span class="sxs-lookup"><span data-stu-id="32eb4-287">In this section you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

- <span data-ttu-id="32eb4-288">[Websocket](http://en.wikipedia.org/wiki/WebSocket)IIS 8 と現在のブラウザーでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="32eb4-288">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>
- <span data-ttu-id="32eb4-289">[サーバー送信イベント](http://en.wikipedia.org/wiki/Server-sent_events)、Internet Explorer 以外のブラウザーでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="32eb4-289">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>
- <span data-ttu-id="32eb4-290">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)、Internet Explorer でサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="32eb4-290">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>
- <span data-ttu-id="32eb4-291">[Ajax ロング ポーリング](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)、すべてのブラウザーでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="32eb4-291">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="32eb4-292">特定の接続には、SignalR は、サーバーとクライアントの両方をサポートする最適な転送方法を選択します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-292">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="32eb4-293">開いている*StockTicker.js*し、ファイルの最後の接続を初期化するコードの直前にログ記録を有効にするコードの行を追加します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-293">Open *StockTicker.js* and add a line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js)]
2. <span data-ttu-id="32eb4-294">F5 キーを押してプロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-294">Press F5 to run the project.</span></span>
3. <span data-ttu-id="32eb4-295">ブラウザーの開発者ツール ウィンドウを開き、ログを表示するコンソールを選択します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-295">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="32eb4-296">新しい接続の転送方法をネゴシエートする Signalr のログを表示するページを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="32eb4-296">You might have to refresh the page to see the logs of Signalr negotiating the transport method for a new connection.</span></span>

    <span data-ttu-id="32eb4-297">Windows 8 (IIS 8) で Internet Explorer 10 を実行している場合は、Websocket はトランスポート メソッドです。</span><span class="sxs-lookup"><span data-stu-id="32eb4-297">If you are running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is WebSockets.</span></span>

    ![IE 10 IIS 8 をコンソールします。](tutorial-server-broadcast-with-signalr/_static/image9.png)

    <span data-ttu-id="32eb4-299">Windows 7 (IIS 7.5) で Internet Explorer 10 を実行している場合は、iframe はトランスポート メソッドです。</span><span class="sxs-lookup"><span data-stu-id="32eb4-299">If you are running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is iframe.</span></span>

    ![IE 10 コンソールで、IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image10.png)

    <span data-ttu-id="32eb4-301">Firefox の場合、Firebug アドインのインストールをコンソール ウィンドウを取得します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-301">In Firefox, install the Firebug add-in to get a Console window.</span></span> <span data-ttu-id="32eb4-302">Windows 8 (IIS 8) で Firefox 19 を実行している場合は、Websocket はトランスポート メソッドです。</span><span class="sxs-lookup"><span data-stu-id="32eb4-302">If you are running Firefox 19 on Windows 8 (IIS 8), the transport method is WebSockets.</span></span>

    ![Firefox 19 IIS 8 Websocket](tutorial-server-broadcast-with-signalr/_static/image11.png)

    <span data-ttu-id="32eb4-304">Windows 7 (IIS 7.5) で Firefox 19 を実行している場合、トランスポート メソッドは、サーバーによって送信されるイベントです。</span><span class="sxs-lookup"><span data-stu-id="32eb4-304">If you are running Firefox 19 on Windows 7 (IIS 7.5), the transport method is server-sent events.</span></span>

    ![Firefox 19 IIS 7.5 コンソールします。](tutorial-server-broadcast-with-signalr/_static/image12.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a><span data-ttu-id="32eb4-306">インストールし、完全なサンプルを StockTicker を確認</span><span class="sxs-lookup"><span data-stu-id="32eb4-306">Install and review the full StockTicker sample</span></span>

<span data-ttu-id="32eb4-307">インストールされている StockTicker アプリケーション、 [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet パッケージには、最初から作成した簡略化されたバージョンより多くの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="32eb4-307">The StockTicker application that is installed by the [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet package includes more features than the simplified version that you just created from scratch.</span></span> <span data-ttu-id="32eb4-308">チュートリアルのこのセクションでは、NuGet パッケージをインストールし、新機能とそれらを実装するコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-308">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span> <span data-ttu-id="32eb4-309">このチュートリアルの前半の手順を実行せず、パッケージをインストールする場合は、プロジェクトに OWIN スタートアップ クラスを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="32eb4-309">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="32eb4-310">この手順は、NuGet パッケージの readme.txt ファイルで説明します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-310">This step is explained in the readme.txt file for the NuGet package.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="32eb4-311">SignalR.Sample NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-311">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="32eb4-312">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして、 **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-312">In **Solution Explorer**, right-click the project and click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="32eb4-313">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**オンライン**、入力*SignalR.Sample*で、**オンラインで検索**ボックスし、[ ]をクリックして**インストール**で、 **SignalR.Sample**パッケージです。</span><span class="sxs-lookup"><span data-stu-id="32eb4-313">In the **Manage NuGet Packages** dialog box, click **Online**, enter *SignalR.Sample* in the **Search Online** box, and then click **Install** in the **SignalR.Sample** package.</span></span>

    ![SignalR.Sample パッケージをインストールします。](tutorial-server-broadcast-with-signalr/_static/image13.png)
3. <span data-ttu-id="32eb4-315">**ソリューション エクスプ ローラー**、展開、 *SignalR.Sample* SignalR.Sample パッケージをインストールすることによって作成されたフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="32eb4-315">In **Solution Explorer**, expand the *SignalR.Sample* folder which was created by installing the SignalR.Sample package.</span></span>
4. <span data-ttu-id="32eb4-316">*SignalR.Sample*フォルダーを右クリックして*StockTicker.html*、順にクリック**スタート ページとして設定**です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-316">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then click **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="32eb4-317">SignalR.Sample NuGet をインストールするパッケージは内にある jQuery のバージョンを変更可能性があります、*スクリプト*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="32eb4-317">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="32eb4-318">新しい*StockTicker.html*でパッケージをインストールするファイル、 *SignalR.Sample*フォルダーは、元のを実行する場合は、パッケージでインストールされるjQueryのバージョンとの同期になります*StockTicker.html*ファイルを再び、まず、スクリプト タグ内の jQuery 参照を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="32eb4-318">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="32eb4-319">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="32eb4-319">Run the application</span></span>

1. <span data-ttu-id="32eb4-320">F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-320">Press F5 to run the application.</span></span>

    <span data-ttu-id="32eb4-321">先ほど見たグリッド、だけでなく完全株価表示器のアプリケーションは同じ株価データを表示するウィンドウを水平方向にスクロールを表示します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-321">In addition to the grid that you saw earlier, the full stock ticker application shows a horizontally scrolling window that displays the same stock data.</span></span> <span data-ttu-id="32eb4-322">初めてアプリケーションを実行して「市場」は"closed"静的グリッドとスクロールのないするティッカー ウィンドウを参照してください。</span><span class="sxs-lookup"><span data-stu-id="32eb4-322">When you run the application for the first time, the "market" is "closed" and you see a static grid and a ticker window that isn't scrolling.</span></span>

    ![StockTicker 画面の開始](tutorial-server-broadcast-with-signalr/_static/image14.png)

    <span data-ttu-id="32eb4-324">クリックすると**Open Market**、 **Live ストック ティッカー**を水平方向にスクロール ボックスを起動し、ランダムに株価変更を定期的にブロードキャストするサーバーを起動します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-324">When you click **Open Market**, the **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span> <span data-ttu-id="32eb4-325">株価されるたびに変更されると、両方、**在庫表のライブ**グリッドと**Live 株式相場表示**ボックスの一覧を更新します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-325">Each time a stock price changes, both the **Live Stock Table** grid and the **Live Stock Ticker** box are updated.</span></span> <span data-ttu-id="32eb4-326">背景が緑色の株式の価格の変化が正の値と、在庫は表示し、背景が赤の在庫が示すように変更が負の場合は、します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-326">When a stock's price change is positive, the stock is shown with a green background, and when the change is negative, the stock is shown with a red background.</span></span>

    ![StockTicker アプリ市場を開く](tutorial-server-broadcast-with-signalr/_static/image15.png)

    <span data-ttu-id="32eb4-328">**閉じる市場**ボタンは、変更を停止し、ティッカー スクロールが停止され、**リセット**価格の変更開始する前に、ボタンが初期状態にすべての株価データをリセットします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-328">The **Close Market** button stops the changes and stops the ticker scrolling, and the **Reset** button resets all stock data to the initial state before price changes started.</span></span> <span data-ttu-id="32eb4-329">複数のブラウザー ウィンドウを開き、同じ URL に移動すると、各ブラウザーで同時に動的に更新される、同じデータを参照してください。</span><span class="sxs-lookup"><span data-stu-id="32eb4-329">If you open more browser windows and go to the same URL, you see the same data dynamically updated at the same time in each browser.</span></span> <span data-ttu-id="32eb4-330">ボタンをクリックすると、すべてのブラウザーは、同時に同じ方法を応答します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-330">When you click one of the buttons, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="32eb4-331">ライブの株式相場表示</span><span class="sxs-lookup"><span data-stu-id="32eb4-331">Live Stock Ticker display</span></span>

<span data-ttu-id="32eb4-332">**Live ストック ティッカー**表示は、単一行に CSS スタイルでフォーマットされた div 要素の順序なしのリスト。</span><span class="sxs-lookup"><span data-stu-id="32eb4-332">The **Live Stock Ticker** display is an unordered list in a div element that is formatted into a single line by CSS styles.</span></span> <span data-ttu-id="32eb4-333">ティッカーは初期化され、テーブルと同様の更新: 内のプレース ホルダーを置き換えることで、 &lt;li&gt;テンプレート文字列と動的に追加する、 &lt;li&gt;要素を&lt;ul&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="32eb4-333">The ticker is initialized and updated the same way as the table: by replacing placeholders in a &lt;li&gt; template string and dynamically adding the &lt;li&gt; elements to the &lt;ul&gt; element.</span></span> <span data-ttu-id="32eb4-334">スクロールは、位置の div 内で順序付けられていないリストの左余白を変更するため、jQuery アニメーション関数を使用して実行します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-334">The scrolling is performed by using the jQuery animate function to vary the margin-left of the unordered list within the div.</span></span>

<span data-ttu-id="32eb4-335">株価表示器 HTML で:</span><span class="sxs-lookup"><span data-stu-id="32eb4-335">The stock ticker HTML:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

<span data-ttu-id="32eb4-336">株価表示器 CSS で:</span><span class="sxs-lookup"><span data-stu-id="32eb4-336">The stock ticker CSS:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

<span data-ttu-id="32eb4-337">これは、jQuery コードがスクロールします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-337">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="32eb4-338">クライアントが呼び出すことができるサーバーの追加のメソッド</span><span class="sxs-lookup"><span data-stu-id="32eb4-338">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="32eb4-339">StockTickerHub クラスでは、クライアントが呼び出すことができる 4 つの追加方法を定義します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-339">The StockTickerHub class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="32eb4-340">OpenMarket、CloseMarket、およびリセットは、ページの上部にあるボタンへの応答で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-340">OpenMarket, CloseMarket, and Reset are called in response to the buttons at the top of the page.</span></span> <span data-ttu-id="32eb4-341">これらには、すべてのクライアントにすぐに反映される状態の変更をトリガーする 1 台のクライアントのパターンを示しています。</span><span class="sxs-lookup"><span data-stu-id="32eb4-341">They demonstrate the pattern of one client triggering a change in state that is immediately propagated to all clients.</span></span> <span data-ttu-id="32eb4-342">これらの各メソッドのメソッドを呼び出す StockTicker クラス市場の状態を変更して、新しい状態をブロードキャストし、その効果。</span><span class="sxs-lookup"><span data-stu-id="32eb4-342">Each of these methods calls a method in the StockTicker class that effects the market state change and then broadcasts the new state.</span></span>

<span data-ttu-id="32eb4-343">StockTicker クラスでは、市場の状態は MarketState 列挙型値を返す MarketState プロパティで確保されます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-343">In the StockTicker class, the state of the market is maintained by a MarketState property that returns a MarketState enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="32eb4-344">各市場の状態を変更する方法は、ロック ブロックの内部 StockTicker クラスにはスレッド セーフであるため。</span><span class="sxs-lookup"><span data-stu-id="32eb4-344">Each of the methods that change the market state do so inside a lock block because the StockTicker class has to be threadsafe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="32eb4-345">このコードが、スレッド セーフであることを確認、 \_MarketState プロパティをバックする marketState フィールドが、volatile としてマークされています。</span><span class="sxs-lookup"><span data-stu-id="32eb4-345">To ensure that this code is threadsafe, the \_marketState field that backs the MarketState property is marked as volatile,</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="32eb4-346">BroadcastMarketStateChange と BroadcastMarketReset メソッドは、点を除いて、クライアント側で定義されているさまざまなメソッドを呼び出すことが既に説明した BroadcastStockPrice メソッドに似ています。</span><span class="sxs-lookup"><span data-stu-id="32eb4-346">The BroadcastMarketStateChange and BroadcastMarketReset methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="32eb4-347">サーバーが呼び出すことができるクライアントの追加の関数</span><span class="sxs-lookup"><span data-stu-id="32eb4-347">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="32eb4-348">UpdateStockPrice 関数は、グリッドと相場表示の両方を処理を使用して jQuery.Color を赤と緑の色をフラッシュします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-348">The updateStockPrice function now handles both the grid and the ticker display, and it uses jQuery.Color to flash red and green colors.</span></span>

<span data-ttu-id="32eb4-349">新しい関数*SignalR.StockTicker.js*の有効化および無効にするボタンがに基づいて市場の状態や、停止や、ティッカー ウィンドウ水平方向のスクロールを開始します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-349">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state, and they stop or start the ticker window horizontal scrolling.</span></span> <span data-ttu-id="32eb4-350">複数の関数は ticker.client に追加されているため、 [jQuery 拡張関数](http://api.jquery.com/jQuery.extend/)に追加するために使用します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-350">Since multiple functions are being added to ticker.client, the [jQuery extend function](http://api.jquery.com/jQuery.extend/) is used to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="32eb4-351">接続を確立した後の他のクライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="32eb4-351">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="32eb4-352">いくつか追加の作業を行うには、クライアント接続を確立した後: 相場が開くか、適切な marketOpened または marketClosed 関数を呼び出すし、ボタンにサーバーのメソッド呼び出しをアタッチするために閉じるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="32eb4-352">After the client establishes the connection, it has some additional work to do: find out if the market is open or closed in order to call the appropriate marketOpened or marketClosed function, and attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="32eb4-353">サーバー メソッドがないワイヤード (有線) までボタンまで接続が確立されると、メソッド呼び出しは、サーバーは使用前にしようとすることはできません、コードをします。</span><span class="sxs-lookup"><span data-stu-id="32eb4-353">The server methods are not wired up to the buttons until after the connection is established, so that the code can't try to call the server methods before they are available.</span></span>

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="32eb4-354">次の手順</span><span class="sxs-lookup"><span data-stu-id="32eb4-354">Next steps</span></span>

<span data-ttu-id="32eb4-355">このチュートリアルでは、接続されているすべてのクライアント、定期的と任意のクライアントからの通知に応答の両方に、サーバーからメッセージをブロードキャストする SignalR アプリケーションをプログラミングする方法を学びました。</span><span class="sxs-lookup"><span data-stu-id="32eb4-355">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients, both on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="32eb4-356">マルチ スレッドのシングルトン インスタンスを使用して、サーバーの状態を保持するパターンも、マルチ プレーヤーのオンライン ゲーム シナリオの使用もできます。</span><span class="sxs-lookup"><span data-stu-id="32eb4-356">The pattern of using a multi-threaded singleton instance to maintain server state can also be also used in multi-player online game scenarios.</span></span> <span data-ttu-id="32eb4-357">例については、次を参照してください。 [SignalR に基づいている ShootR ゲーム](https://github.com/NTaylorMullen/ShootR)です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-357">For an example, see [the ShootR game that is based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="32eb4-358">ピア ツー ピア通信シナリオを示すチュートリアルについては、次を参照してください。 [SignalR の概要](introduction-to-signalr.md)と[SignalR でリアルタイムの更新](tutorial-high-frequency-realtime-with-signalr.md)です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-358">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="32eb4-359">SignalR 開発のより高度な概念については、SignalR のソース コードおよびリソースの次のサイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="32eb4-359">To learn more advanced SignalR development concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="32eb4-360">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="32eb4-360">ASP.NET SignalR</span></span>](../../index.md)
- [<span data-ttu-id="32eb4-361">SignalR Project</span><span class="sxs-lookup"><span data-stu-id="32eb4-361">SignalR Project</span></span>](http://signalr.net/)
- [<span data-ttu-id="32eb4-362">SignalR Github とサンプル</span><span class="sxs-lookup"><span data-stu-id="32eb4-362">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="32eb4-363">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="32eb4-363">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

<span data-ttu-id="32eb4-364">SignalR アプリケーションを Azure にデプロイする方法のチュートリアルについては、次を参照してください。 [Azure App service Web アプリを使用してを使用して SignalR](../deployment/using-signalr-with-azure-web-sites.md)です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-364">For a walkthrough on how to deploy a SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="32eb4-365">Visual Studio web プロジェクトを Windows Azure Web サイトを展開する方法の詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。</span><span class="sxs-lookup"><span data-stu-id="32eb4-365">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
