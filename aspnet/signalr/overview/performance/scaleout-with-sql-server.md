---
uid: signalr/overview/performance/scaleout-with-sql-server
title: SQL Server での SignalR スケール アウト |Microsoft ドキュメント
author: MikeWasson
description: ここ Visual Studio 2013 .NET 4.5 SignalR で使用するソフトウェアのバージョンはの以前のバージョンについてはこのトピックの以前のバージョンをバージョン 2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: b3189c36fc076333c0c6007bd039b12e03d63bc8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="80211-103">SQL Server での SignalR スケール アウト</span><span class="sxs-lookup"><span data-stu-id="80211-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="80211-104">によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="80211-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="80211-105">このトピックで使用されているソフトウェア バージョン</span><span class="sxs-lookup"><span data-stu-id="80211-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="80211-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="80211-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="80211-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="80211-107">.NET 4.5</span></span>
> - <span data-ttu-id="80211-108">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="80211-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="80211-109">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="80211-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="80211-110">SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="80211-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="80211-111">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="80211-111">Questions and comments</span></span>
> 
> <span data-ttu-id="80211-112">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="80211-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="80211-113">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="80211-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="80211-114">このチュートリアルでは SQL Server を使用して 2 つの独立した IIS インスタンスに配置されている SignalR アプリケーション間でメッセージを配布します。</span><span class="sxs-lookup"><span data-stu-id="80211-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="80211-115">1 つのテスト コンピューターで、このチュートリアルを実行することもできますが、効果を得るには、次の 2 つまたは複数のサーバーに SignalR アプリケーションを配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="80211-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="80211-116">いずれかのサーバーまたは別の専用サーバーでは、SQL Server をインストールすることも必要があります。</span><span class="sxs-lookup"><span data-stu-id="80211-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="80211-117">Azure で Vm を使用してチュートリアルを実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="80211-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="80211-118">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="80211-118">Prerequisites</span></span>

<span data-ttu-id="80211-119">Microsoft SQL Server 2005 以降。</span><span class="sxs-lookup"><span data-stu-id="80211-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="80211-120">バック プレーンには、SQL Server のデスクトップとサーバーの両方のエディションがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="80211-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="80211-121">これは、SQL Server Compact Edition または Azure SQL Database にはサポートしません。</span><span class="sxs-lookup"><span data-stu-id="80211-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="80211-122">(アプリケーションが Azure でホストされている場合を検討 Service Bus バック プレーン代わりにします。)</span><span class="sxs-lookup"><span data-stu-id="80211-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="80211-123">概要</span><span class="sxs-lookup"><span data-stu-id="80211-123">Overview</span></span>

<span data-ttu-id="80211-124">詳細なチュートリアルを始める前に何を行うの簡単な概要を次に示します。</span><span class="sxs-lookup"><span data-stu-id="80211-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="80211-125">新しい空のデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="80211-125">Create a new empty database.</span></span> <span data-ttu-id="80211-126">バック プレーンには、このデータベースに必要なテーブルは作成します。</span><span class="sxs-lookup"><span data-stu-id="80211-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="80211-127">これらの NuGet パッケージをアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="80211-127">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="80211-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="80211-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="80211-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="80211-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="80211-130">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="80211-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="80211-131">Startup.cs バック プレーンを構成するには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="80211-131">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="80211-132">このコードの既定値でのバック プレーンの構成[TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx)と[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="80211-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="80211-133">これらの値を変更する方法については、次を参照してください。 [SignalR パフォーマンス: スケール アウト メトリック](signalr-performance.md#scaleout_metrics)です。</span><span class="sxs-lookup"><span data-stu-id="80211-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span> 

## <a name="configure-the-database"></a><span data-ttu-id="80211-134">データベースを構成します。</span><span class="sxs-lookup"><span data-stu-id="80211-134">Configure the Database</span></span>

<span data-ttu-id="80211-135">かどうか、アプリケーションが Windows 認証または SQL Server 認証に使用、データベースへのアクセスを決定します。</span><span class="sxs-lookup"><span data-stu-id="80211-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="80211-136">関係なく、データベース ユーザーがログイン、スキーマを作成し、テーブルを作成するアクセス許可を確認します。</span><span class="sxs-lookup"><span data-stu-id="80211-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="80211-137">使用するバック プレーンの新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="80211-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="80211-138">データベースには、任意の名前を付けることができます。</span><span class="sxs-lookup"><span data-stu-id="80211-138">You can give the database any name.</span></span> <span data-ttu-id="80211-139">データベースの任意のテーブルを作成する必要はありません。バック プレーンでは、必要なテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="80211-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="80211-140">Service Broker を有効にします。</span><span class="sxs-lookup"><span data-stu-id="80211-140">Enable Service Broker</span></span>

<span data-ttu-id="80211-141">バック プレーンのデータベースの Service Broker を有効にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="80211-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="80211-142">Service Broker は、メッセージングとバック プレーンの更新プログラムをより効率的に受信できるようにする SQL Server のキューに対するネイティブ サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="80211-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="80211-143">(ただし、バック プレーンでも Service Broker なし。)</span><span class="sxs-lookup"><span data-stu-id="80211-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="80211-144">Service Broker が有効になっているかどうかを確認するには、クエリ、**は\_broker\_有効になっている**内の列、 **sys.databases**カタログ ビューです。</span><span class="sxs-lookup"><span data-stu-id="80211-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="80211-145">Service Broker を有効にするには、次の SQL クエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="80211-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="80211-146">このクエリは、デッドロックを起こすことを確認するが表示された場合、DB に接続されているアプリケーションはありません。</span><span class="sxs-lookup"><span data-stu-id="80211-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="80211-147">トレースを有効にした場合、トレースが表示されます Service Broker が有効になっているかどうか。</span><span class="sxs-lookup"><span data-stu-id="80211-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="80211-148">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="80211-148">Create a SignalR Application</span></span>

<span data-ttu-id="80211-149">SignalR アプリケーションを作成するには、次のこれらのチュートリアルのいずれか。</span><span class="sxs-lookup"><span data-stu-id="80211-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="80211-150">SignalR 2.0 の概要</span><span class="sxs-lookup"><span data-stu-id="80211-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="80211-151">SignalR 2.0 と MVC 5 の概要</span><span class="sxs-lookup"><span data-stu-id="80211-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="80211-152">次に、SQL Server でスケール アウトをサポートするためにチャット アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="80211-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="80211-153">まず、SignalR.SqlServer NuGet パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="80211-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="80211-154">Visual Studio から、**ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="80211-154">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="80211-155">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="80211-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="80211-156">次に、Startup.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="80211-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="80211-157">次のコードを追加、**構成**メソッド。</span><span class="sxs-lookup"><span data-stu-id="80211-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="80211-158">展開し、アプリケーションを実行</span><span class="sxs-lookup"><span data-stu-id="80211-158">Deploy and Run the Application</span></span>

<span data-ttu-id="80211-159">SignalR アプリケーションを展開する、Windows Server のインスタンスを準備します。</span><span class="sxs-lookup"><span data-stu-id="80211-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="80211-160">IIS の役割を追加します。</span><span class="sxs-lookup"><span data-stu-id="80211-160">Add the IIS role.</span></span> <span data-ttu-id="80211-161">WebSocket プロトコルを含む、「アプリケーション開発」機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="80211-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="80211-162">([管理ツール] 下に表示)、管理サービスがあります。</span><span class="sxs-lookup"><span data-stu-id="80211-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="80211-163">**インストールの Web Deploy 3.0。**</span><span class="sxs-lookup"><span data-stu-id="80211-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="80211-164">IIS マネージャーを実行するとき、Microsoft Web プラットフォームをインストールするように求め ことができます[ダウンロード、intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)です。</span><span class="sxs-lookup"><span data-stu-id="80211-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="80211-165">Platform Installer で Web Deploy を検索し、Web Deploy 3.0 をインストール</span><span class="sxs-lookup"><span data-stu-id="80211-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="80211-166">Web 管理サービスが実行されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="80211-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="80211-167">それ以外の場合は、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="80211-167">If not, start the service.</span></span> <span data-ttu-id="80211-168">(Web 管理サービスは、Windows サービスの一覧に表示されない場合、は、IIS の役割を追加するときに、管理サービスがインストールされていることを確認) します。</span><span class="sxs-lookup"><span data-stu-id="80211-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="80211-169">最後に、TCP のポート 8172 を開きます。</span><span class="sxs-lookup"><span data-stu-id="80211-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="80211-170">これは、Web Deploy ツールを使用するポートです。</span><span class="sxs-lookup"><span data-stu-id="80211-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="80211-171">Visual Studio プロジェクトを開発用コンピューターからサーバーを配置する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="80211-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="80211-172">ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックして**発行**です。</span><span class="sxs-lookup"><span data-stu-id="80211-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="80211-173">Web 展開に関するドキュメントの詳細を参照してください。 [Visual Studio と ASP.NET の Web 展開コンテンツ マップ](../../../whitepapers/aspnet-web-deployment-content-map.md)です。</span><span class="sxs-lookup"><span data-stu-id="80211-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="80211-174">2 つのサーバーにアプリケーションを展開する場合は、別のブラウザー ウィンドウで各インスタンスを開くし、一方の SignalR メッセージを受信互いを参照してください。</span><span class="sxs-lookup"><span data-stu-id="80211-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="80211-175">(当然ながら、実稼働環境で 2 つのサーバー放置ロード バランサーの背後にします。)</span><span class="sxs-lookup"><span data-stu-id="80211-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="80211-176">アプリケーションを実行した後は、SignalR が自動的に作成されたことのテーブル、データベース内を確認できます。</span><span class="sxs-lookup"><span data-stu-id="80211-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="80211-177">SignalR では、テーブルを管理します。</span><span class="sxs-lookup"><span data-stu-id="80211-177">SignalR manages the tables.</span></span> <span data-ttu-id="80211-178">アプリケーションが展開されている限り、しない行を削除、テーブルの変更など。</span><span class="sxs-lookup"><span data-stu-id="80211-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
