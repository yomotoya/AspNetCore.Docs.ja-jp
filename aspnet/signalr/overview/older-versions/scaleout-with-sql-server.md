---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: SQL Server による SignalR スケール アウト (SignalR 1.x) |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 682aa837ed991cbf5d78dcb304e2c1bce905c52c
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287534"
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="e16ba-102">SQL Server による SignalR スケール アウト (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="e16ba-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="e16ba-103">によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="e16ba-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="e16ba-104">このチュートリアルでは、2 つの IIS インスタンスで配置されている SignalR アプリケーション間でメッセージを配布するのに SQL Server を使用します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="e16ba-105">1 つのテスト コンピューターで、このチュートリアルを実行することもできますが、完全な効果を取得するには SignalR アプリケーションを 2 つまたは複数のサーバーをデプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e16ba-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="e16ba-106">サーバーのいずれか、または別の専用サーバーでは、SQL Server をインストールすることもする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e16ba-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="e16ba-107">別のオプションでは、Azure で Vm を使用してチュートリアルを実行します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="e16ba-108">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="e16ba-108">Prerequisites</span></span>

<span data-ttu-id="e16ba-109">Microsoft SQL Server 2005 以降。</span><span class="sxs-lookup"><span data-stu-id="e16ba-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="e16ba-110">バック プレーンには、SQL Server のデスクトップとサーバーの両方のエディションがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="e16ba-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="e16ba-111">SQL Server Compact Edition または Azure SQL Database はサポートされません。</span><span class="sxs-lookup"><span data-stu-id="e16ba-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="e16ba-112">(場合は、アプリケーションは、Azure でホストされる、Service Bus のバック プレーン代わりに、検討します。)</span><span class="sxs-lookup"><span data-stu-id="e16ba-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="e16ba-113">概要</span><span class="sxs-lookup"><span data-stu-id="e16ba-113">Overview</span></span>

<span data-ttu-id="e16ba-114">詳細なチュートリアルを始める前に、作業内容の簡単な概要を示します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="e16ba-115">新しい空のデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-115">Create a new empty database.</span></span> <span data-ttu-id="e16ba-116">バック プレーンはこのデータベースに必要なテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="e16ba-117">これらの NuGet パッケージをアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="e16ba-118">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="e16ba-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="e16ba-119">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="e16ba-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="e16ba-120">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="e16ba-121">Global.asax バック プレーンを構成するには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="e16ba-122">データベースを構成します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-122">Configure the Database</span></span>

<span data-ttu-id="e16ba-123">かどうか、アプリケーションが Windows 認証または SQL Server 認証に使用、データベースへのアクセスを決定します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="e16ba-124">関係なく、データベース ユーザーにログインし、スキーマを作成して、テーブルを作成するアクセス許可を確認します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="e16ba-125">使用するバック プレーンの新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="e16ba-126">データベースに任意の名前を付けることができます。</span><span class="sxs-lookup"><span data-stu-id="e16ba-126">You can give the database any name.</span></span> <span data-ttu-id="e16ba-127">データベースでは、テーブルを作成する必要はありません。バック プレーンでは、必要なテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="e16ba-128">Service Broker を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e16ba-128">Enable Service Broker</span></span>

<span data-ttu-id="e16ba-129">バック プレーンのデータベースの Service Broker を有効にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e16ba-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="e16ba-130">Service Broker では、メッセージングとバック プレーンのより効率的に更新プログラムを受信できるように SQL Server でのキューのネイティブ サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="e16ba-131">(ただし、バック プレーンもさせずに Service Broker。)</span><span class="sxs-lookup"><span data-stu-id="e16ba-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="e16ba-132">Service Broker が有効になっているかどうかを確認するには、クエリ、**は\_broker\_有効になっている**内の列、 **sys.databases**カタログ ビューです。</span><span class="sxs-lookup"><span data-stu-id="e16ba-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="e16ba-133">Service Broker を有効にするには、次の SQL クエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="e16ba-134">このクエリは、デッドロックを確認が表示された場合、DB に接続されているアプリケーションはありません。</span><span class="sxs-lookup"><span data-stu-id="e16ba-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="e16ba-135">トレースを有効にした場合、トレースが表示されます Service Broker が有効になっているかどうか。</span><span class="sxs-lookup"><span data-stu-id="e16ba-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="e16ba-136">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-136">Create a SignalR Application</span></span>

<span data-ttu-id="e16ba-137">これらのチュートリアルのいずれかを次で SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="e16ba-138">SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="e16ba-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="e16ba-139">SignalR と MVC 4 の概要</span><span class="sxs-lookup"><span data-stu-id="e16ba-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="e16ba-140">次に、私たちと SQL Server のスケール アウトをサポートするために、チャット アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="e16ba-141">まず、SignalR.SqlServer NuGet パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="e16ba-142">Visual Studio から、**ツール**メニューの  **NuGet パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-142">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="e16ba-143">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="e16ba-144">次に、Global.asax ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="e16ba-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="e16ba-145">次のコードを追加、**アプリケーション\_開始**メソッド。</span><span class="sxs-lookup"><span data-stu-id="e16ba-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="e16ba-146">展開し、アプリケーションを実行</span><span class="sxs-lookup"><span data-stu-id="e16ba-146">Deploy and Run the Application</span></span>

<span data-ttu-id="e16ba-147">SignalR アプリケーションをデプロイするには、Windows Server インスタンスを準備します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="e16ba-148">IIS の役割を追加します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-148">Add the IIS role.</span></span> <span data-ttu-id="e16ba-149">WebSocket プロトコルを含む、「アプリケーションの開発」機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="e16ba-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="e16ba-150">管理サービス ([管理ツール] の下に表示) とも含まれます。</span><span class="sxs-lookup"><span data-stu-id="e16ba-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="e16ba-151">**インストール Web Deploy 3.0 です。**</span><span class="sxs-lookup"><span data-stu-id="e16ba-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="e16ba-152">IIS マネージャーを実行するとき、Microsoft Web プラットフォームをインストールするように求められますできます[ダウンロード、intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="e16ba-153">プラットフォーム インストーラーで Web Deploy を検索し、Web Deploy 3.0 をインストール</span><span class="sxs-lookup"><span data-stu-id="e16ba-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="e16ba-154">Web 管理サービスが実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="e16ba-155">それ以外の場合は、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-155">If not, start the service.</span></span> <span data-ttu-id="e16ba-156">(Web 管理サービスで Windows サービスの一覧が表示されない場合は、IIS の役割を追加したときに、管理サービスがインストールされていることを確認)。</span><span class="sxs-lookup"><span data-stu-id="e16ba-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="e16ba-157">最後に、tcp ポート 8172 を開きます。</span><span class="sxs-lookup"><span data-stu-id="e16ba-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="e16ba-158">これは、Web 配置ツールで使用されるポートです。</span><span class="sxs-lookup"><span data-stu-id="e16ba-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="e16ba-159">サーバーに、開発コンピューターから Visual Studio プロジェクトを配置する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="e16ba-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="e16ba-160">ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックして**発行**します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="e16ba-161">Web デプロイに関するドキュメントの詳細を参照してください。 [for Visual Studio および ASP.NET の Web 配置コンテンツ マップ](../../../whitepapers/aspnet-web-deployment-content-map.md)します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="e16ba-162">2 つのサーバー アプリケーションをデプロイする場合は、別のブラウザー ウィンドウで各インスタンスを開くし、もう一方の SignalR メッセージを受信互いを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e16ba-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="e16ba-163">(もちろん、実稼働環境で 2 つのサーバー放置ロード バランサーの背後にします。)</span><span class="sxs-lookup"><span data-stu-id="e16ba-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="e16ba-164">アプリケーションを実行した後、SignalR がデータベース内でテーブルを作成が自動的にことが確認できます。</span><span class="sxs-lookup"><span data-stu-id="e16ba-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="e16ba-165">SignalR は、テーブルを管理します。</span><span class="sxs-lookup"><span data-stu-id="e16ba-165">SignalR manages the tables.</span></span> <span data-ttu-id="e16ba-166">アプリケーションが展開されている限りしない行を削除、変更、テーブルのなど。</span><span class="sxs-lookup"><span data-stu-id="e16ba-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
