---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Redis による SignalR スケール アウト (SignalR 1.x) |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 4f587b129a1a22e64625d2ab0fc7655984262ebe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826342"
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="9484f-102">Redis による SignalR スケール アウト (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="9484f-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>
====================
<span data-ttu-id="9484f-103">によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="9484f-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="9484f-104">このチュートリアルでは使用して[Redis](http://redis.io/) 2 つの IIS インスタンスに配置されている SignalR アプリケーション間でメッセージを配信します。</span><span class="sxs-lookup"><span data-stu-id="9484f-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="9484f-105">Redis はメモリ内のキー値ストアです。</span><span class="sxs-lookup"><span data-stu-id="9484f-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="9484f-106">パブリッシュ/サブスクライブ モデルでのメッセージング システムもサポートしています。</span><span class="sxs-lookup"><span data-stu-id="9484f-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="9484f-107">SignalR で Redis バック プレーンでは、パブリッシュ/サブスクライブ機能を使用して、他のサーバーにメッセージを転送します。</span><span class="sxs-lookup"><span data-stu-id="9484f-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="9484f-108">このチュートリアルでは、3 台のサーバーを使用します。</span><span class="sxs-lookup"><span data-stu-id="9484f-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="9484f-109">SignalR アプリケーションをデプロイするのに使用する、Windows を実行している 2 台のサーバー。</span><span class="sxs-lookup"><span data-stu-id="9484f-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="9484f-110">Redis の実行に使用するには、Linux を実行しているサーバーの 1 つ。</span><span class="sxs-lookup"><span data-stu-id="9484f-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="9484f-111">このチュートリアルではスクリーン ショットは、Ubuntu 12.04 TLS を使用しました。</span><span class="sxs-lookup"><span data-stu-id="9484f-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="9484f-112">使用する 3 つの物理サーバーを持っていない場合は、HYPER-V で Vm を作成できます。</span><span class="sxs-lookup"><span data-stu-id="9484f-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="9484f-113">別のオプションでは、Azure で Vm を作成します。</span><span class="sxs-lookup"><span data-stu-id="9484f-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="9484f-114">このチュートリアルでは、公式の Redis の実装はまた、 [Redis のポートを Windows](https://github.com/MSOpenTech/redis) MSOpenTech から。</span><span class="sxs-lookup"><span data-stu-id="9484f-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="9484f-115">セットアップと構成が異なるが、それ以外の場合、手順は同じです。</span><span class="sxs-lookup"><span data-stu-id="9484f-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9484f-116">Redis による SignalR スケール アウトは、Redis クラスターをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="9484f-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="9484f-117">概要</span><span class="sxs-lookup"><span data-stu-id="9484f-117">Overview</span></span>

<span data-ttu-id="9484f-118">詳細なチュートリアルを始める前に、作業内容の簡単な概要を示します。</span><span class="sxs-lookup"><span data-stu-id="9484f-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="9484f-119">Redis をインストールし、Redis サーバーを起動します。</span><span class="sxs-lookup"><span data-stu-id="9484f-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="9484f-120">これらの NuGet パッケージをアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="9484f-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="9484f-121">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="9484f-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="9484f-122">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="9484f-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="9484f-123">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="9484f-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="9484f-124">Global.asax バック プレーンを構成するには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="9484f-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="9484f-125">Hyper V 上の Ubuntu</span><span class="sxs-lookup"><span data-stu-id="9484f-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="9484f-126">Windows HYPER-V を使用して、Windows Server 上の Ubuntu VM を簡単に作成できます。</span><span class="sxs-lookup"><span data-stu-id="9484f-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="9484f-127">ISO をダウンロードする Ubuntu から[ http://www.ubuntu.com](http://www.ubuntu.com/)します。</span><span class="sxs-lookup"><span data-stu-id="9484f-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="9484f-128">HYPER-V では、新しい VM を追加します。</span><span class="sxs-lookup"><span data-stu-id="9484f-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="9484f-129">**仮想ハード_ディスクの接続**手順で、**仮想ハード ディスクを作成する**します。</span><span class="sxs-lookup"><span data-stu-id="9484f-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="9484f-130">**インストール オプション**手順で、**イメージ ファイル (.iso)**、 をクリックして**参照**Ubuntu のインストール ISO を参照します。</span><span class="sxs-lookup"><span data-stu-id="9484f-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="9484f-131">Redis をインストールします。</span><span class="sxs-lookup"><span data-stu-id="9484f-131">Install Redis</span></span>

<span data-ttu-id="9484f-132">手順に従う[ http://redis.io/download ](http://redis.io/download)をダウンロードして、Redis をビルドします。</span><span class="sxs-lookup"><span data-stu-id="9484f-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="9484f-133">Redis バイナリ ビルド、`src`ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="9484f-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="9484f-134">既定では、Redis では、パスワードは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="9484f-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="9484f-135">パスワードを設定するには、編集、`redis.conf`ファイルで、ソース コードのルート ディレクトリにあります。</span><span class="sxs-lookup"><span data-stu-id="9484f-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="9484f-136">(バックアップ コピーを作成、ファイルの編集する前に!)次のディレクティブを追加`redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="9484f-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="9484f-137">Redis サーバーを今すぐ開始するには。</span><span class="sxs-lookup"><span data-stu-id="9484f-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="9484f-138">Redis の既定のポートが開かれたポート 6379 をリッスンします。</span><span class="sxs-lookup"><span data-stu-id="9484f-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="9484f-139">(構成ファイルでポート番号を変更できます)。</span><span class="sxs-lookup"><span data-stu-id="9484f-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="9484f-140">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="9484f-140">Create the SignalR Application</span></span>

<span data-ttu-id="9484f-141">これらのチュートリアルのいずれかを次で SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="9484f-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="9484f-142">SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="9484f-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="9484f-143">SignalR と MVC 4 の概要</span><span class="sxs-lookup"><span data-stu-id="9484f-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="9484f-144">次に、私たち Redis によるスケール アウトをサポートするために、チャット アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="9484f-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="9484f-145">まず、SignalR.Redis NuGet パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="9484f-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="9484f-146">Visual Studio から、**ツール**メニューの **ライブラリ パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="9484f-146">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="9484f-147">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="9484f-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="9484f-148">次に、Global.asax ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="9484f-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="9484f-149">次のコードを追加、**アプリケーション\_開始**メソッド。</span><span class="sxs-lookup"><span data-stu-id="9484f-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="9484f-150">"server"は、Redis を実行しているサーバーの名前です。</span><span class="sxs-lookup"><span data-stu-id="9484f-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="9484f-151">*ポート*ポート番号</span><span class="sxs-lookup"><span data-stu-id="9484f-151">*port* is the port number</span></span>
- <span data-ttu-id="9484f-152">"password"では、redis.conf についてファイルで定義されているパスワードです。</span><span class="sxs-lookup"><span data-stu-id="9484f-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="9484f-153">"AppName"は、任意の文字列です。</span><span class="sxs-lookup"><span data-stu-id="9484f-153">"AppName" is any string.</span></span> <span data-ttu-id="9484f-154">SignalR では、この名前の Redis のパブリッシュ/サブスクライブ チャンネルを作成します。</span><span class="sxs-lookup"><span data-stu-id="9484f-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="9484f-155">例えば:</span><span class="sxs-lookup"><span data-stu-id="9484f-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="9484f-156">展開し、アプリケーションを実行</span><span class="sxs-lookup"><span data-stu-id="9484f-156">Deploy and Run the Application</span></span>

<span data-ttu-id="9484f-157">SignalR アプリケーションをデプロイするには、Windows Server インスタンスを準備します。</span><span class="sxs-lookup"><span data-stu-id="9484f-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="9484f-158">IIS の役割を追加します。</span><span class="sxs-lookup"><span data-stu-id="9484f-158">Add the IIS role.</span></span> <span data-ttu-id="9484f-159">WebSocket プロトコルを含む、「アプリケーションの開発」機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="9484f-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="9484f-160">管理サービス ([管理ツール] の下に表示) とも含まれます。</span><span class="sxs-lookup"><span data-stu-id="9484f-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="9484f-161">**インストール Web Deploy 3.0 です。**</span><span class="sxs-lookup"><span data-stu-id="9484f-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="9484f-162">IIS マネージャーを実行するとき、Microsoft Web プラットフォームをインストールするように求められますできます[ダウンロード、intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)します。</span><span class="sxs-lookup"><span data-stu-id="9484f-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="9484f-163">プラットフォーム インストーラーで Web Deploy を検索し、Web Deploy 3.0 をインストール</span><span class="sxs-lookup"><span data-stu-id="9484f-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="9484f-164">Web 管理サービスが実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="9484f-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="9484f-165">それ以外の場合は、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="9484f-165">If not, start the service.</span></span> <span data-ttu-id="9484f-166">(Web 管理サービスで Windows サービスの一覧が表示されない場合は、IIS の役割を追加したときに、管理サービスがインストールされていることを確認)。</span><span class="sxs-lookup"><span data-stu-id="9484f-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="9484f-167">既定では、Web 管理サービスは TCP ポート 8172 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="9484f-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="9484f-168">Windows ファイアウォールでポート 8172 で TCP トラフィックを許可する場合は、新しい受信規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="9484f-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="9484f-169">詳細については、次を参照してください。[ファイアウォール規則を構成する](https://technet.microsoft.com/library/dd448559(WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="9484f-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="9484f-170">(Azure 上の Vm をホストしている場合はこれを行う、Azure portal で直接します。</span><span class="sxs-lookup"><span data-stu-id="9484f-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="9484f-171">参照してください[仮想マシンにエンドポイントを設定する方法](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/))。</span><span class="sxs-lookup"><span data-stu-id="9484f-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="9484f-172">サーバーに、開発コンピューターから Visual Studio プロジェクトを配置する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="9484f-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="9484f-173">ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックして**発行**します。</span><span class="sxs-lookup"><span data-stu-id="9484f-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="9484f-174">Web デプロイに関するドキュメントの詳細を参照してください。 [for Visual Studio および ASP.NET の Web 配置コンテンツ マップ](../../../whitepapers/aspnet-web-deployment-content-map.md)します。</span><span class="sxs-lookup"><span data-stu-id="9484f-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="9484f-175">2 つのサーバー アプリケーションをデプロイする場合は、別のブラウザー ウィンドウで各インスタンスを開くし、もう一方の SignalR メッセージを受信互いを参照してください。</span><span class="sxs-lookup"><span data-stu-id="9484f-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="9484f-176">(もちろん、実稼働環境で 2 つのサーバー放置ロード バランサーの背後にします。)</span><span class="sxs-lookup"><span data-stu-id="9484f-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="9484f-177">使用することができます、Redis に送信されるメッセージを表示する関心がある場合、 **redis cli**クライアントで、Redis と共にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="9484f-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
