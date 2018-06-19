---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: SQL Server での SignalR スケール アウト (SignalR 1.x) |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7a589f5c6a5196444c3c616b39f501f3a7512471
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505611"
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="d8e1d-102">SQL Server での SignalR スケール アウト (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="d8e1d-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="d8e1d-103">によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d8e1d-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="d8e1d-104">このチュートリアルでは SQL Server を使用して 2 つの独立した IIS インスタンスに配置されている SignalR アプリケーション間でメッセージを配布します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="d8e1d-105">1 つのテスト コンピューターで、このチュートリアルを実行することもできますが、効果を得るには、次の 2 つまたは複数のサーバーに SignalR アプリケーションを配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="d8e1d-106">いずれかのサーバーまたは別の専用サーバーでは、SQL Server をインストールすることも必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="d8e1d-107">Azure で Vm を使用してチュートリアルを実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="d8e1d-108">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="d8e1d-108">Prerequisites</span></span>

<span data-ttu-id="d8e1d-109">Microsoft SQL Server 2005 以降。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="d8e1d-110">バック プレーンには、SQL Server のデスクトップとサーバーの両方のエディションがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="d8e1d-111">これは、SQL Server Compact Edition または Azure SQL Database にはサポートしません。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="d8e1d-112">(アプリケーションが Azure でホストされている場合を検討 Service Bus バック プレーン代わりにします。)</span><span class="sxs-lookup"><span data-stu-id="d8e1d-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="d8e1d-113">概要</span><span class="sxs-lookup"><span data-stu-id="d8e1d-113">Overview</span></span>

<span data-ttu-id="d8e1d-114">詳細なチュートリアルを始める前に何を行うの簡単な概要を次に示します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="d8e1d-115">新しい空のデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-115">Create a new empty database.</span></span> <span data-ttu-id="d8e1d-116">バック プレーンには、このデータベースに必要なテーブルは作成します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="d8e1d-117">これらの NuGet パッケージをアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="d8e1d-118">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="d8e1d-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="d8e1d-119">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="d8e1d-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="d8e1d-120">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="d8e1d-121">Global.asax バック プレーンを構成するには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="d8e1d-122">データベースを構成します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-122">Configure the Database</span></span>

<span data-ttu-id="d8e1d-123">かどうか、アプリケーションが Windows 認証または SQL Server 認証に使用、データベースへのアクセスを決定します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="d8e1d-124">関係なく、データベース ユーザーがログイン、スキーマを作成し、テーブルを作成するアクセス許可を確認します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="d8e1d-125">使用するバック プレーンの新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="d8e1d-126">データベースには、任意の名前を付けることができます。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-126">You can give the database any name.</span></span> <span data-ttu-id="d8e1d-127">データベースの任意のテーブルを作成する必要はありません。バック プレーンでは、必要なテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="d8e1d-128">Service Broker を有効にします。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-128">Enable Service Broker</span></span>

<span data-ttu-id="d8e1d-129">バック プレーンのデータベースの Service Broker を有効にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="d8e1d-130">Service Broker は、メッセージングとバック プレーンの更新プログラムをより効率的に受信できるようにする SQL Server のキューに対するネイティブ サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="d8e1d-131">(ただし、バック プレーンでも Service Broker なし。)</span><span class="sxs-lookup"><span data-stu-id="d8e1d-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="d8e1d-132">Service Broker が有効になっているかどうかを確認するには、クエリ、**は\_broker\_有効になっている**内の列、 **sys.databases**カタログ ビューです。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="d8e1d-133">Service Broker を有効にするには、次の SQL クエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="d8e1d-134">このクエリは、デッドロックを起こすことを確認するが表示された場合、DB に接続されているアプリケーションはありません。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="d8e1d-135">トレースを有効にした場合、トレースが表示されます Service Broker が有効になっているかどうか。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="d8e1d-136">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-136">Create a SignalR Application</span></span>

<span data-ttu-id="d8e1d-137">SignalR アプリケーションを作成するには、次のこれらのチュートリアルのいずれか。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="d8e1d-138">SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="d8e1d-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="d8e1d-139">SignalR と MVC 4 の概要</span><span class="sxs-lookup"><span data-stu-id="d8e1d-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="d8e1d-140">次に、SQL Server でスケール アウトをサポートするためにチャット アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="d8e1d-141">まず、SignalR.SqlServer NuGet パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="d8e1d-142">Visual Studio から、**ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-142">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d8e1d-143">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="d8e1d-144">次に、Global.asax ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="d8e1d-145">次のコードを追加、**アプリケーション\_開始**メソッド。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="d8e1d-146">展開し、アプリケーションを実行</span><span class="sxs-lookup"><span data-stu-id="d8e1d-146">Deploy and Run the Application</span></span>

<span data-ttu-id="d8e1d-147">SignalR アプリケーションを展開する、Windows Server のインスタンスを準備します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="d8e1d-148">IIS の役割を追加します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-148">Add the IIS role.</span></span> <span data-ttu-id="d8e1d-149">WebSocket プロトコルを含む、「アプリケーション開発」機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="d8e1d-150">([管理ツール] 下に表示)、管理サービスがあります。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="d8e1d-151">**インストールの Web Deploy 3.0。**</span><span class="sxs-lookup"><span data-stu-id="d8e1d-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="d8e1d-152">IIS マネージャーを実行するとき、Microsoft Web プラットフォームをインストールするように求め ことができます[ダウンロード、intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)です。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="d8e1d-153">Platform Installer で Web Deploy を検索し、Web Deploy 3.0 をインストール</span><span class="sxs-lookup"><span data-stu-id="d8e1d-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="d8e1d-154">Web 管理サービスが実行されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="d8e1d-155">それ以外の場合は、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-155">If not, start the service.</span></span> <span data-ttu-id="d8e1d-156">(Web 管理サービスは、Windows サービスの一覧に表示されない場合、は、IIS の役割を追加するときに、管理サービスがインストールされていることを確認) します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="d8e1d-157">最後に、TCP のポート 8172 を開きます。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="d8e1d-158">これは、Web Deploy ツールを使用するポートです。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="d8e1d-159">Visual Studio プロジェクトを開発用コンピューターからサーバーを配置する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="d8e1d-160">ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックして**発行**です。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="d8e1d-161">Web 展開に関するドキュメントの詳細を参照してください。 [Visual Studio と ASP.NET の Web 展開コンテンツ マップ](../../../whitepapers/aspnet-web-deployment-content-map.md)です。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="d8e1d-162">2 つのサーバーにアプリケーションを展開する場合は、別のブラウザー ウィンドウで各インスタンスを開くし、一方の SignalR メッセージを受信互いを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="d8e1d-163">(当然ながら、実稼働環境で 2 つのサーバー放置ロード バランサーの背後にします。)</span><span class="sxs-lookup"><span data-stu-id="d8e1d-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="d8e1d-164">アプリケーションを実行した後は、SignalR が自動的に作成されたことのテーブル、データベース内を確認できます。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="d8e1d-165">SignalR では、テーブルを管理します。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-165">SignalR manages the tables.</span></span> <span data-ttu-id="d8e1d-166">アプリケーションが展開されている限り、しない行を削除、テーブルの変更など。</span><span class="sxs-lookup"><span data-stu-id="d8e1d-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
