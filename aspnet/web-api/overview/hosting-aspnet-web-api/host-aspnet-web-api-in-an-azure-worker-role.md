---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: ASP.NET Web API 2 の Azure ワーカー ロールをホスト |Microsoft ドキュメント
author: MikeWasson
description: このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストする Azure ワーカー ロールで ASP.NET Web API をホストする方法を示します。 .NET (OWIN) de 用 Web インターフェイスを開く.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 7ba1dc850e2f9d9c88e6ddf263a796e1867a98be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="0281e-104">ASP.NET Web API 2 の Azure ワーカー ロールをホストします。</span><span class="sxs-lookup"><span data-stu-id="0281e-104">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>
====================
<span data-ttu-id="0281e-105">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0281e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="0281e-106">このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストする Azure ワーカー ロールで ASP.NET Web API をホストする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0281e-106">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="0281e-107">[.NET 用 Web インターフェイスを開く](http://owin.org/)(OWIN) .NET web サーバーと web アプリケーション間で抽象型を定義します。</span><span class="sxs-lookup"><span data-stu-id="0281e-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="0281e-108">OWIN OWIN を自己ホスト型 IIS の外部で独自のプロセス内の web アプリケーションに最適ですが、サーバーから web アプリケーションの分離 – Azure ワーカー ロール内などです。</span><span class="sxs-lookup"><span data-stu-id="0281e-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="0281e-109">このチュートリアルではあります Microsoft.Owin.Host.HttpListener パッケージを使用して自己 OWIN アプリケーションをホストするために使用する HTTP サーバーを提供します。</span><span class="sxs-lookup"><span data-stu-id="0281e-109">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0281e-110">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="0281e-110">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="0281e-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0281e-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="0281e-112">Web API 2</span><span class="sxs-lookup"><span data-stu-id="0281e-112">Web API 2</span></span>
> - [<span data-ttu-id="0281e-113">Azure SDK for .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="0281e-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="0281e-114">Microsoft Azure プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="0281e-114">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="0281e-115">管理者特権で Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="0281e-115">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="0281e-116">Azure コンピューティング エミュレーターを使用してローカルで、アプリケーションをデバッグするには、管理者特権が必要です。</span><span class="sxs-lookup"><span data-stu-id="0281e-116">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="0281e-117">**ファイル** メニューのをクリックして**新規**をクリックし、**プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="0281e-117">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="0281e-118">**インストールされたテンプレート**、Visual c# をクリックして**クラウド** をクリックし、 **Windows Azure クラウド サービス**です。</span><span class="sxs-lookup"><span data-stu-id="0281e-118">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="0281e-119">プロジェクト"AzureApp"の名前し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="0281e-119">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="0281e-120">**新しい Windows Azure のクラウド サービス**ダイアログ ボックスをダブルクリックして**ワーカー ロール**です。</span><span class="sxs-lookup"><span data-stu-id="0281e-120">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="0281e-121">既定の名前 ("WorkerRole1") のままにします。</span><span class="sxs-lookup"><span data-stu-id="0281e-121">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="0281e-122">この手順は、ワーカー ロールをソリューションに追加します。</span><span class="sxs-lookup"><span data-stu-id="0281e-122">This step adds a worker role to the solution.</span></span> <span data-ttu-id="0281e-123">**[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0281e-123">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="0281e-124">Visual Studio ソリューション作成するにはには、2 つのプロジェクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="0281e-124">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="0281e-125">&quot;AzureApp&quot;ロールと Azure アプリケーションの構成を定義します。</span><span class="sxs-lookup"><span data-stu-id="0281e-125">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="0281e-126">&quot;WorkerRole1&quot;ワーカー ロールのコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="0281e-126">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="0281e-127">一般に、Azure アプリケーションは、このチュートリアルは、1 つの役割を使用していますが、複数のロールを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="0281e-127">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="0281e-128">Web API と OWIN パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="0281e-128">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="0281e-129">**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**をクリックし、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="0281e-129">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="0281e-130">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="0281e-130">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="0281e-131">HTTP エンドポイントを追加します。</span><span class="sxs-lookup"><span data-stu-id="0281e-131">Add an HTTP Endpoint</span></span>

<span data-ttu-id="0281e-132">ソリューション エクスプ ローラーで、AzureApp プロジェクトを展開します。</span><span class="sxs-lookup"><span data-stu-id="0281e-132">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="0281e-133">ロール ノードを展開し、WorkerRole1 を右クリックし **プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="0281e-133">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="0281e-134">をクリックして**エンドポイント**、クリックして**エンドポイントの追加**です。</span><span class="sxs-lookup"><span data-stu-id="0281e-134">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="0281e-135">**プロトコル**ドロップダウン リストで、"http"を選択します。</span><span class="sxs-lookup"><span data-stu-id="0281e-135">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="0281e-136">**パブリック ポート**と**プライベート ポート**80」と入力します。</span><span class="sxs-lookup"><span data-stu-id="0281e-136">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="0281e-137">これらのポート番号は別にすることはできます。</span><span class="sxs-lookup"><span data-stu-id="0281e-137">These port numbers can be different.</span></span> <span data-ttu-id="0281e-138">パブリック ポートが、クライアントがロールに、要求を送信する際に使用します。</span><span class="sxs-lookup"><span data-stu-id="0281e-138">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="0281e-139">Web API を構成する自己ホスト</span><span class="sxs-lookup"><span data-stu-id="0281e-139">Configure Web API for Self-Host</span></span>

<span data-ttu-id="0281e-140">ソリューション エクスプ ローラーで、WorkerRole1 プロジェクトを右クリックし、選択**追加** / **クラス**新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="0281e-140">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="0281e-141">クラスに `Startup` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="0281e-141">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="0281e-142">次のようにすべてのこのファイルに定型コードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0281e-142">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="0281e-143">Web API コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="0281e-143">Add a Web API Controller</span></span>

<span data-ttu-id="0281e-144">次に、Web API コント ローラー クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="0281e-144">Next, add a Web API controller class.</span></span> <span data-ttu-id="0281e-145">WorkerRole1 プロジェクトを右クリックし **追加** / **クラス**です。</span><span class="sxs-lookup"><span data-stu-id="0281e-145">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="0281e-146">TestController クラスの名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="0281e-146">Name the class TestController.</span></span> <span data-ttu-id="0281e-147">次のようにすべてのこのファイルに定型コードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0281e-147">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="0281e-148">わかりやすくするため、このコント ローラーにはプレーン テキストを返す 2 つの GET メソッドだけを定義します。</span><span class="sxs-lookup"><span data-stu-id="0281e-148">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="0281e-149">OWIN ホストを起動します。</span><span class="sxs-lookup"><span data-stu-id="0281e-149">Start the OWIN Host</span></span>

<span data-ttu-id="0281e-150">WorkerRole.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="0281e-150">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="0281e-151">このクラスは、ワーカー ロールが開始および停止時に実行されるコードを定義します。</span><span class="sxs-lookup"><span data-stu-id="0281e-151">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="0281e-152">次の追加ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="0281e-152">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="0281e-153">追加、 **IDisposable**メンバーを`WorkerRole`クラス。</span><span class="sxs-lookup"><span data-stu-id="0281e-153">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="0281e-154">`OnStart`メソッド、ホストを起動する次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="0281e-154">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="0281e-155">**WebApp.Start**メソッドは、OWIN ホストを開始します。</span><span class="sxs-lookup"><span data-stu-id="0281e-155">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="0281e-156">名前、`Startup`クラスは、型パラメーター、メソッドにします。</span><span class="sxs-lookup"><span data-stu-id="0281e-156">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="0281e-157">慣例により、ホストを呼び出すが、`Configure`このクラスのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="0281e-157">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="0281e-158">上書き、`OnStop`を破棄する、 *\_アプリ*インスタンス。</span><span class="sxs-lookup"><span data-stu-id="0281e-158">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="0281e-159">WorkerRole.cs の完全なコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="0281e-159">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="0281e-160">ソリューションをビルドし、Azure コンピューティング エミュレーターでアプリケーションをローカルで実行する場合は F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="0281e-160">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="0281e-161">ファイアウォールの設定によっては、エミュレーター、ファイアウォールを通過を許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0281e-161">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="0281e-162">次のような例外が発生した場合を参照してください[このブログの投稿](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx)問題を回避します。</span><span class="sxs-lookup"><span data-stu-id="0281e-162">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="0281e-163">"ファイルまたはアセンブリを読み込めませんでした ' Microsoft.Owin, Version = 2.0.2.0、カルチャ = neutral, PublicKeyToken = 31bf3856ad364e35' またはその依存関係の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="0281e-163">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="0281e-164">見つかったアセンブリのマニフェスト定義は、アセンブリ参照と一致しません。</span><span class="sxs-lookup"><span data-stu-id="0281e-164">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="0281e-165">(HRESULT からの例外: 0x80131040)"</span><span class="sxs-lookup"><span data-stu-id="0281e-165">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="0281e-166">コンピューティング エミュレーターは、エンドポイントに、ローカル IP アドレスを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="0281e-166">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="0281e-167">コンピューティング エミュレーター UI を表示することによって IP アドレスを見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="0281e-167">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="0281e-168">タスク バー通知領域に表示されたエミュレーター アイコンを右クリックし  **コンピューティング エミュレーター UI**です。</span><span class="sxs-lookup"><span data-stu-id="0281e-168">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="0281e-169">サービスの展開、展開 [id]、サービスの詳細情報の下の IP アドレスを検索します。</span><span class="sxs-lookup"><span data-stu-id="0281e-169">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="0281e-170">Web ブラウザーを開き、http:// に移動<em>アドレス</em>/テスト/1、ここで<em>アドレス</em>;、コンピューティング エミュレーターによって割り当てられた IP アドレスは、たとえば、`http://127.0.0.1:80/test/1`です。</span><span class="sxs-lookup"><span data-stu-id="0281e-170">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="0281e-171">Web API コント ローラーからの応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0281e-171">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="0281e-172">Azure への配置します。</span><span class="sxs-lookup"><span data-stu-id="0281e-172">Deploy to Azure</span></span>

<span data-ttu-id="0281e-173">この手順では、Azure アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="0281e-173">For this step, you must have an Azure account.</span></span> <span data-ttu-id="0281e-174">既に持っていない場合は、ほんの数分で無料の試用アカウントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="0281e-174">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="0281e-175">詳細については、「 [Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)です。</span><span class="sxs-lookup"><span data-stu-id="0281e-175">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="0281e-176">ソリューション エクスプ ローラーで、AzureApp プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="0281e-176">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="0281e-177">**[発行]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="0281e-177">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="0281e-178">Azure アカウントにサインインしていない場合はクリックして**サイン イン**です。</span><span class="sxs-lookup"><span data-stu-id="0281e-178">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="0281e-179">サブスクリプションを選択しをクリックするサインイン後、**次**です。</span><span class="sxs-lookup"><span data-stu-id="0281e-179">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="0281e-180">クラウド サービスの名前を入力し、地域を選択します。</span><span class="sxs-lookup"><span data-stu-id="0281e-180">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="0281e-181">**[作成]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0281e-181">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="0281e-182">**[発行]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0281e-182">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="0281e-183">Azure のアクティビティ ログ ウィンドウでは、展開の進行状況を示します。</span><span class="sxs-lookup"><span data-stu-id="0281e-183">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="0281e-184">アプリが展開されると、参照http://appname.cloudapp.net/test/1です。</span><span class="sxs-lookup"><span data-stu-id="0281e-184">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="0281e-185">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="0281e-185">Additional Resources</span></span>

- [<span data-ttu-id="0281e-186">プロジェクト Katana の概要</span><span class="sxs-lookup"><span data-stu-id="0281e-186">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="0281e-187">GitHub の Katana プロジェクト</span><span class="sxs-lookup"><span data-stu-id="0281e-187">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
