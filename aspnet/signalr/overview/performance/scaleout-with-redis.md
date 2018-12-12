---
uid: signalr/overview/performance/scaleout-with-redis
title: Redis による SignalR スケール アウト |Microsoft Docs
author: MikeWasson
description: このトピックの「Visual Studio 2013 .NET 4.5 SignalR 使用されるソフトウェアのバージョンは以前のバージョンについてはこのトピック以前バージョンをバージョン 2.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 5151c718c82408fdcb75de16211b55488ca513d2
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287742"
---
<a name="signalr-scaleout-with-redis"></a><span data-ttu-id="7d552-103">Redis による SignalR スケール アウト</span><span class="sxs-lookup"><span data-stu-id="7d552-103">SignalR Scaleout with Redis</span></span>
====================
<span data-ttu-id="7d552-104">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7d552-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="7d552-105">このトピックで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="7d552-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="7d552-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7d552-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="7d552-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7d552-107">.NET 4.5</span></span>
> - <span data-ttu-id="7d552-108">SignalR バージョン 2.4</span><span class="sxs-lookup"><span data-stu-id="7d552-108">SignalR version 2.4</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="7d552-109">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="7d552-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="7d552-110">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="7d552-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="7d552-111">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="7d552-111">Questions and comments</span></span>
>
> <span data-ttu-id="7d552-112">このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。</span><span class="sxs-lookup"><span data-stu-id="7d552-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="7d552-113">チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。</span><span class="sxs-lookup"><span data-stu-id="7d552-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="7d552-114">このチュートリアルでは使用して[Redis](http://redis.io/) 2 つの IIS インスタンスに配置されている SignalR アプリケーション間でメッセージを配信します。</span><span class="sxs-lookup"><span data-stu-id="7d552-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="7d552-115">Redis はメモリ内のキー値ストアです。</span><span class="sxs-lookup"><span data-stu-id="7d552-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="7d552-116">パブリッシュ/サブスクライブ モデルでのメッセージング システムもサポートしています。</span><span class="sxs-lookup"><span data-stu-id="7d552-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="7d552-117">SignalR で Redis バック プレーンでは、パブリッシュ/サブスクライブ機能を使用して、他のサーバーにメッセージを転送します。</span><span class="sxs-lookup"><span data-stu-id="7d552-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="7d552-118">このチュートリアルでは、3 台のサーバーを使用します。</span><span class="sxs-lookup"><span data-stu-id="7d552-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="7d552-119">SignalR アプリケーションをデプロイするのに使用する、Windows を実行している 2 台のサーバー。</span><span class="sxs-lookup"><span data-stu-id="7d552-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="7d552-120">Redis の実行に使用するには、Linux を実行しているサーバーの 1 つ。</span><span class="sxs-lookup"><span data-stu-id="7d552-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="7d552-121">このチュートリアルではスクリーン ショットは、Ubuntu 12.04 TLS を使用しました。</span><span class="sxs-lookup"><span data-stu-id="7d552-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="7d552-122">使用する 3 つの物理サーバーを持っていない場合は、HYPER-V で Vm を作成できます。</span><span class="sxs-lookup"><span data-stu-id="7d552-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="7d552-123">別のオプションでは、Azure で Vm を作成します。</span><span class="sxs-lookup"><span data-stu-id="7d552-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="7d552-124">このチュートリアルでは、公式の Redis の実装はまた、 [Redis のポートを Windows](https://github.com/MSOpenTech/redis) MSOpenTech から。</span><span class="sxs-lookup"><span data-stu-id="7d552-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="7d552-125">セットアップと構成が異なるが、それ以外の場合、手順は同じです。</span><span class="sxs-lookup"><span data-stu-id="7d552-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE]
>
> <span data-ttu-id="7d552-126">Redis による SignalR スケール アウトは、Redis クラスターをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="7d552-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="7d552-127">概要</span><span class="sxs-lookup"><span data-stu-id="7d552-127">Overview</span></span>

<span data-ttu-id="7d552-128">詳細なチュートリアルを始める前に、作業内容の簡単な概要を示します。</span><span class="sxs-lookup"><span data-stu-id="7d552-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="7d552-129">Redis をインストールし、Redis サーバーを起動します。</span><span class="sxs-lookup"><span data-stu-id="7d552-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="7d552-130">これらの NuGet パッケージをアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="7d552-130">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="7d552-131">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="7d552-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="7d552-132">Microsoft.AspNet.SignalR.StackExchangeRedis</span><span class="sxs-lookup"><span data-stu-id="7d552-132">Microsoft.AspNet.SignalR.StackExchangeRedis</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. <span data-ttu-id="7d552-133">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="7d552-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="7d552-134">Startup.cs バック プレーンを構成するには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="7d552-134">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="7d552-135">Hyper V 上の Ubuntu</span><span class="sxs-lookup"><span data-stu-id="7d552-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="7d552-136">Windows HYPER-V を使用して、Windows Server 上の Ubuntu VM を簡単に作成できます。</span><span class="sxs-lookup"><span data-stu-id="7d552-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="7d552-137">ISO をダウンロードする Ubuntu から[ http://www.ubuntu.com](http://www.ubuntu.com/)します。</span><span class="sxs-lookup"><span data-stu-id="7d552-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="7d552-138">HYPER-V では、新しい VM を追加します。</span><span class="sxs-lookup"><span data-stu-id="7d552-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="7d552-139">**仮想ハード_ディスクの接続**手順で、**仮想ハード ディスクを作成する**します。</span><span class="sxs-lookup"><span data-stu-id="7d552-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="7d552-140">**インストール オプション**手順で、**イメージ ファイル (.iso)**、 をクリックして**参照**Ubuntu のインストール ISO を参照します。</span><span class="sxs-lookup"><span data-stu-id="7d552-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="7d552-141">Redis をインストールします。</span><span class="sxs-lookup"><span data-stu-id="7d552-141">Install Redis</span></span>

<span data-ttu-id="7d552-142">手順に従う[ http://redis.io/download ](http://redis.io/download)をダウンロードして、Redis をビルドします。</span><span class="sxs-lookup"><span data-stu-id="7d552-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="7d552-143">Redis バイナリ ビルド、`src`ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="7d552-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="7d552-144">既定では、Redis では、パスワードは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="7d552-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="7d552-145">パスワードを設定するには、編集、`redis.conf`ファイルで、ソース コードのルート ディレクトリにあります。</span><span class="sxs-lookup"><span data-stu-id="7d552-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="7d552-146">(バックアップ コピーを作成、ファイルの編集する前に!)次のディレクティブを追加`redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="7d552-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="7d552-147">Redis サーバーを今すぐ開始するには。</span><span class="sxs-lookup"><span data-stu-id="7d552-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="7d552-148">Redis の既定のポートが開かれたポート 6379 をリッスンします。</span><span class="sxs-lookup"><span data-stu-id="7d552-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="7d552-149">(構成ファイルでポート番号を変更できます)。</span><span class="sxs-lookup"><span data-stu-id="7d552-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="7d552-150">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="7d552-150">Create the SignalR Application</span></span>

<span data-ttu-id="7d552-151">これらのチュートリアルのいずれかを次で SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="7d552-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="7d552-152">SignalR 2.0 の概要</span><span class="sxs-lookup"><span data-stu-id="7d552-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="7d552-153">SignalR 2.0 と MVC 5 の概要</span><span class="sxs-lookup"><span data-stu-id="7d552-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="7d552-154">次に、私たち Redis によるスケール アウトをサポートするために、チャット アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="7d552-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="7d552-155">最初に、追加、`Microsoft.AspNet.SignalR.StackExchangeRedis`をプロジェクトに NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="7d552-155">First, add the `Microsoft.AspNet.SignalR.StackExchangeRedis` NuGet package to your project.</span></span> <span data-ttu-id="7d552-156">Visual Studio から、**ツール**メニューの  **NuGet パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="7d552-156">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="7d552-157">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="7d552-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="7d552-158">次に、Startup.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="7d552-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="7d552-159">次のコードを追加、**構成**メソッド。</span><span class="sxs-lookup"><span data-stu-id="7d552-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="7d552-160">"server"は、Redis を実行しているサーバーの名前です。</span><span class="sxs-lookup"><span data-stu-id="7d552-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="7d552-161">*ポート*ポート番号</span><span class="sxs-lookup"><span data-stu-id="7d552-161">*port* is the port number</span></span>
- <span data-ttu-id="7d552-162">"password"では、redis.conf についてファイルで定義されているパスワードです。</span><span class="sxs-lookup"><span data-stu-id="7d552-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="7d552-163">"AppName"は、任意の文字列です。</span><span class="sxs-lookup"><span data-stu-id="7d552-163">"AppName" is any string.</span></span> <span data-ttu-id="7d552-164">SignalR では、この名前の Redis のパブリッシュ/サブスクライブ チャンネルを作成します。</span><span class="sxs-lookup"><span data-stu-id="7d552-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="7d552-165">例:</span><span class="sxs-lookup"><span data-stu-id="7d552-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="7d552-166">展開し、アプリケーションを実行</span><span class="sxs-lookup"><span data-stu-id="7d552-166">Deploy and Run the Application</span></span>

<span data-ttu-id="7d552-167">SignalR アプリケーションをデプロイするには、Windows Server インスタンスを準備します。</span><span class="sxs-lookup"><span data-stu-id="7d552-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="7d552-168">IIS の役割を追加します。</span><span class="sxs-lookup"><span data-stu-id="7d552-168">Add the IIS role.</span></span> <span data-ttu-id="7d552-169">WebSocket プロトコルを含む、「アプリケーションの開発」機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7d552-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="7d552-170">管理サービス ([管理ツール] の下に表示) とも含まれます。</span><span class="sxs-lookup"><span data-stu-id="7d552-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="7d552-171">**インストール Web Deploy 3.0 です。**</span><span class="sxs-lookup"><span data-stu-id="7d552-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="7d552-172">IIS マネージャーを実行するとき、Microsoft Web プラットフォームをインストールするように求められますできます[ダウンロード、intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)します。</span><span class="sxs-lookup"><span data-stu-id="7d552-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="7d552-173">プラットフォーム インストーラーで Web Deploy を検索し、Web Deploy 3.0 をインストール</span><span class="sxs-lookup"><span data-stu-id="7d552-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="7d552-174">Web 管理サービスが実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7d552-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="7d552-175">それ以外の場合は、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="7d552-175">If not, start the service.</span></span> <span data-ttu-id="7d552-176">(Web 管理サービスで Windows サービスの一覧が表示されない場合は、IIS の役割を追加したときに、管理サービスがインストールされていることを確認)。</span><span class="sxs-lookup"><span data-stu-id="7d552-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="7d552-177">既定では、Web 管理サービスは TCP ポート 8172 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="7d552-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="7d552-178">Windows ファイアウォールでポート 8172 で TCP トラフィックを許可する場合は、新しい受信規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="7d552-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="7d552-179">詳細については、次を参照してください。[ファイアウォール規則を構成する](https://technet.microsoft.com/library/dd448559(WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7d552-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="7d552-180">(Azure 上の Vm をホストしている場合はこれを行う、Azure portal で直接します。</span><span class="sxs-lookup"><span data-stu-id="7d552-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="7d552-181">参照してください[仮想マシンにエンドポイントを設定する方法](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/))。</span><span class="sxs-lookup"><span data-stu-id="7d552-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="7d552-182">サーバーに、開発コンピューターから Visual Studio プロジェクトを配置する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="7d552-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="7d552-183">ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックして**発行**します。</span><span class="sxs-lookup"><span data-stu-id="7d552-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="7d552-184">Web デプロイに関するドキュメントの詳細を参照してください。 [for Visual Studio および ASP.NET の Web 配置コンテンツ マップ](../../../whitepapers/aspnet-web-deployment-content-map.md)します。</span><span class="sxs-lookup"><span data-stu-id="7d552-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="7d552-185">2 つのサーバー アプリケーションをデプロイする場合は、別のブラウザー ウィンドウで各インスタンスを開くし、もう一方の SignalR メッセージを受信互いを参照してください。</span><span class="sxs-lookup"><span data-stu-id="7d552-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="7d552-186">(もちろん、実稼働環境で 2 つのサーバー放置ロード バランサーの背後にします。)</span><span class="sxs-lookup"><span data-stu-id="7d552-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="7d552-187">使用することができます、Redis に送信されるメッセージを表示する関心がある場合、 **redis cli**クライアントで、Redis と共にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="7d552-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
