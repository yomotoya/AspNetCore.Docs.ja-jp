---
uid: signalr/overview/performance/scaleout-with-sql-server
title: SQL Server による SignalR スケール アウト |Microsoft Docs
author: MikeWasson
description: このトピックの「Visual Studio 2013 .NET 4.5 SignalR 使用されるソフトウェアのバージョンは以前のバージョンについてはこのトピック以前バージョンをバージョン 2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: a105d4f3e9fc366eeec2dc42dd0eb73946432fc3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391474"
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="21c9d-103">SQL Server による SignalR スケール アウト</span><span class="sxs-lookup"><span data-stu-id="21c9d-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="21c9d-104">によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="21c9d-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="21c9d-105">このトピックで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="21c9d-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="21c9d-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="21c9d-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="21c9d-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="21c9d-107">.NET 4.5</span></span>
> - <span data-ttu-id="21c9d-108">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="21c9d-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="21c9d-109">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="21c9d-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="21c9d-110">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="21c9d-111">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="21c9d-111">Questions and comments</span></span>
> 
> <span data-ttu-id="21c9d-112">このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="21c9d-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="21c9d-113">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="21c9d-114">このチュートリアルでは、2 つの IIS インスタンスで配置されている SignalR アプリケーション間でメッセージを配布するのに SQL Server を使用します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="21c9d-115">1 つのテスト コンピューターで、このチュートリアルを実行することもできますが、完全な効果を取得するには SignalR アプリケーションを 2 つまたは複数のサーバーをデプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="21c9d-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="21c9d-116">サーバーのいずれか、または別の専用サーバーでは、SQL Server をインストールすることもする必要があります。</span><span class="sxs-lookup"><span data-stu-id="21c9d-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="21c9d-117">別のオプションでは、Azure で Vm を使用してチュートリアルを実行します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="21c9d-118">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="21c9d-118">Prerequisites</span></span>

<span data-ttu-id="21c9d-119">Microsoft SQL Server 2005 以降。</span><span class="sxs-lookup"><span data-stu-id="21c9d-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="21c9d-120">バック プレーンには、SQL Server のデスクトップとサーバーの両方のエディションがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="21c9d-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="21c9d-121">SQL Server Compact Edition または Azure SQL Database はサポートされません。</span><span class="sxs-lookup"><span data-stu-id="21c9d-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="21c9d-122">(場合は、アプリケーションは、Azure でホストされる、Service Bus のバック プレーン代わりに、検討します。)</span><span class="sxs-lookup"><span data-stu-id="21c9d-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="21c9d-123">概要</span><span class="sxs-lookup"><span data-stu-id="21c9d-123">Overview</span></span>

<span data-ttu-id="21c9d-124">詳細なチュートリアルを始める前に、作業内容の簡単な概要を示します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="21c9d-125">新しい空のデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-125">Create a new empty database.</span></span> <span data-ttu-id="21c9d-126">バック プレーンはこのデータベースに必要なテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="21c9d-127">これらの NuGet パッケージをアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-127">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="21c9d-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="21c9d-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="21c9d-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="21c9d-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="21c9d-130">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="21c9d-131">Startup.cs バック プレーンを構成するには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-131">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="21c9d-132">このコードでは、バック プレーンを構成の既定値を持つ[TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx)と[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="21c9d-133">これらの値を変更する方法については、次を参照してください。 [SignalR パフォーマンス: スケール アウト メトリック](signalr-performance.md#scaleout_metrics)します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span> 

## <a name="configure-the-database"></a><span data-ttu-id="21c9d-134">データベースを構成します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-134">Configure the Database</span></span>

<span data-ttu-id="21c9d-135">かどうか、アプリケーションが Windows 認証または SQL Server 認証に使用、データベースへのアクセスを決定します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="21c9d-136">関係なく、データベース ユーザーにログインし、スキーマを作成して、テーブルを作成するアクセス許可を確認します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="21c9d-137">使用するバック プレーンの新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="21c9d-138">データベースに任意の名前を付けることができます。</span><span class="sxs-lookup"><span data-stu-id="21c9d-138">You can give the database any name.</span></span> <span data-ttu-id="21c9d-139">データベースでは、テーブルを作成する必要はありません。バック プレーンでは、必要なテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="21c9d-140">Service Broker を有効にします。</span><span class="sxs-lookup"><span data-stu-id="21c9d-140">Enable Service Broker</span></span>

<span data-ttu-id="21c9d-141">バック プレーンのデータベースの Service Broker を有効にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="21c9d-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="21c9d-142">Service Broker では、メッセージングとバック プレーンのより効率的に更新プログラムを受信できるように SQL Server でのキューのネイティブ サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="21c9d-143">(ただし、バック プレーンもさせずに Service Broker。)</span><span class="sxs-lookup"><span data-stu-id="21c9d-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="21c9d-144">Service Broker が有効になっているかどうかを確認するには、クエリ、**は\_broker\_有効になっている**内の列、 **sys.databases**カタログ ビューです。</span><span class="sxs-lookup"><span data-stu-id="21c9d-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="21c9d-145">Service Broker を有効にするには、次の SQL クエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="21c9d-146">このクエリは、デッドロックを確認が表示された場合、DB に接続されているアプリケーションはありません。</span><span class="sxs-lookup"><span data-stu-id="21c9d-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="21c9d-147">トレースを有効にした場合、トレースが表示されます Service Broker が有効になっているかどうか。</span><span class="sxs-lookup"><span data-stu-id="21c9d-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="21c9d-148">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-148">Create a SignalR Application</span></span>

<span data-ttu-id="21c9d-149">これらのチュートリアルのいずれかを次で SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="21c9d-150">SignalR 2.0 の概要</span><span class="sxs-lookup"><span data-stu-id="21c9d-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="21c9d-151">SignalR 2.0 と MVC 5 の概要</span><span class="sxs-lookup"><span data-stu-id="21c9d-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="21c9d-152">次に、私たちと SQL Server のスケール アウトをサポートするために、チャット アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="21c9d-153">まず、SignalR.SqlServer NuGet パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="21c9d-154">Visual Studio から、**ツール**メニューの **ライブラリ パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-154">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="21c9d-155">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="21c9d-156">次に、Startup.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="21c9d-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="21c9d-157">次のコードを追加、**構成**メソッド。</span><span class="sxs-lookup"><span data-stu-id="21c9d-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="21c9d-158">展開し、アプリケーションを実行</span><span class="sxs-lookup"><span data-stu-id="21c9d-158">Deploy and Run the Application</span></span>

<span data-ttu-id="21c9d-159">SignalR アプリケーションをデプロイするには、Windows Server インスタンスを準備します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="21c9d-160">IIS の役割を追加します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-160">Add the IIS role.</span></span> <span data-ttu-id="21c9d-161">WebSocket プロトコルを含む、「アプリケーションの開発」機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="21c9d-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="21c9d-162">管理サービス ([管理ツール] の下に表示) とも含まれます。</span><span class="sxs-lookup"><span data-stu-id="21c9d-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="21c9d-163">**インストール Web Deploy 3.0 です。**</span><span class="sxs-lookup"><span data-stu-id="21c9d-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="21c9d-164">IIS マネージャーを実行するとき、Microsoft Web プラットフォームをインストールするように求められますできます[ダウンロード、intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="21c9d-165">プラットフォーム インストーラーで Web Deploy を検索し、Web Deploy 3.0 をインストール</span><span class="sxs-lookup"><span data-stu-id="21c9d-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="21c9d-166">Web 管理サービスが実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="21c9d-167">それ以外の場合は、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-167">If not, start the service.</span></span> <span data-ttu-id="21c9d-168">(Web 管理サービスで Windows サービスの一覧が表示されない場合は、IIS の役割を追加したときに、管理サービスがインストールされていることを確認)。</span><span class="sxs-lookup"><span data-stu-id="21c9d-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="21c9d-169">最後に、tcp ポート 8172 を開きます。</span><span class="sxs-lookup"><span data-stu-id="21c9d-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="21c9d-170">これは、Web 配置ツールで使用されるポートです。</span><span class="sxs-lookup"><span data-stu-id="21c9d-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="21c9d-171">サーバーに、開発コンピューターから Visual Studio プロジェクトを配置する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="21c9d-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="21c9d-172">ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックして**発行**します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="21c9d-173">Web デプロイに関するドキュメントの詳細を参照してください。 [for Visual Studio および ASP.NET の Web 配置コンテンツ マップ](../../../whitepapers/aspnet-web-deployment-content-map.md)します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="21c9d-174">2 つのサーバー アプリケーションをデプロイする場合は、別のブラウザー ウィンドウで各インスタンスを開くし、もう一方の SignalR メッセージを受信互いを参照してください。</span><span class="sxs-lookup"><span data-stu-id="21c9d-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="21c9d-175">(もちろん、実稼働環境で 2 つのサーバー放置ロード バランサーの背後にします。)</span><span class="sxs-lookup"><span data-stu-id="21c9d-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="21c9d-176">アプリケーションを実行した後、SignalR がデータベース内でテーブルを作成が自動的にことが確認できます。</span><span class="sxs-lookup"><span data-stu-id="21c9d-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="21c9d-177">SignalR は、テーブルを管理します。</span><span class="sxs-lookup"><span data-stu-id="21c9d-177">SignalR manages the tables.</span></span> <span data-ttu-id="21c9d-178">アプリケーションが展開されている限りしない行を削除、変更、テーブルのなど。</span><span class="sxs-lookup"><span data-stu-id="21c9d-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
