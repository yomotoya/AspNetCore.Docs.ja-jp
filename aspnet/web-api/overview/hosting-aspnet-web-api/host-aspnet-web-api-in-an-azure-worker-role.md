---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: ASP.NET Web API 2 では、Azure Worker ロールのホスト |Microsoft Docs
author: MikeWasson
description: このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストする Azure ワーカー ロールで ASP.NET Web API をホストする方法を示します。 .NET (OWIN) de の Web インターフェイスを開く.
ms.author: riande
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: cabf88e4e6c946f92a9e4534a4db5ae15dd8cae5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828885"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="72a70-104">ASP.NET Web API 2 では、Azure Worker ロールをホストします。</span><span class="sxs-lookup"><span data-stu-id="72a70-104">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>
====================
<span data-ttu-id="72a70-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="72a70-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="72a70-106">このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストする Azure ワーカー ロールで ASP.NET Web API をホストする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="72a70-106">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="72a70-107">[.NET 用 Web インターフェイスを開き](http://owin.org/)(OWIN) .NET web サーバーおよび web アプリケーション間の抽象化を定義します。</span><span class="sxs-lookup"><span data-stu-id="72a70-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="72a70-108">OWIN により、OWIN の IIS の外部の独自のプロセスで web アプリケーションを自己ホストするために最適ですが、サーバーから web アプリケーションの分離 – Azure worker ロール内など。</span><span class="sxs-lookup"><span data-stu-id="72a70-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="72a70-109">このチュートリアルでは、Microsoft.Owin.Host.HttpListener パッケージを使用します OWIN アプリケーションを自己ホストするために使用する HTTP サーバーを提供します。</span><span class="sxs-lookup"><span data-stu-id="72a70-109">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="72a70-110">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="72a70-110">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="72a70-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="72a70-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="72a70-112">Web API 2</span><span class="sxs-lookup"><span data-stu-id="72a70-112">Web API 2</span></span>
> - [<span data-ttu-id="72a70-113">Azure SDK for .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="72a70-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="72a70-114">Microsoft Azure プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="72a70-114">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="72a70-115">管理者特権で Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="72a70-115">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="72a70-116">Azure コンピューティング エミュレーターを使用してローカルでのアプリケーションをデバッグするには、管理者特権が必要です。</span><span class="sxs-lookup"><span data-stu-id="72a70-116">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="72a70-117">**ファイル** メニューのをクリックして**新規**、 をクリックし、**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="72a70-117">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="72a70-118">**インストールされたテンプレート**、Visual c# では、クリックして**クラウド** をクリックし、 **Windows Azure クラウド サービス**します。</span><span class="sxs-lookup"><span data-stu-id="72a70-118">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="72a70-119">プロジェクトとして「AzureApp」という名前にして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="72a70-119">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="72a70-120">**新しい Windows Azure クラウド サービス**ダイアログ ボックスで、ダブルクリックして**ワーカー ロール**します。</span><span class="sxs-lookup"><span data-stu-id="72a70-120">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="72a70-121">既定の名前 ("WorkerRole1") のままにします。</span><span class="sxs-lookup"><span data-stu-id="72a70-121">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="72a70-122">この手順では、ソリューションにワーカー ロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="72a70-122">This step adds a worker role to the solution.</span></span> <span data-ttu-id="72a70-123">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="72a70-123">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="72a70-124">作成された Visual Studio ソリューションには、2 つのプロジェクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="72a70-124">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="72a70-125">&quot;AzureApp&quot;ロールと Azure のアプリケーションの構成を定義します。</span><span class="sxs-lookup"><span data-stu-id="72a70-125">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="72a70-126">&quot;WorkerRole1&quot;ワーカー ロールのコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="72a70-126">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="72a70-127">一般に、Azure のアプリケーションは、このチュートリアルは、1 つのロールを使用しますが、複数のロールを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="72a70-127">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="72a70-128">Web API と OWIN パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="72a70-128">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="72a70-129">**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="72a70-129">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="72a70-130">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="72a70-130">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="72a70-131">HTTP エンドポイントを追加します。</span><span class="sxs-lookup"><span data-stu-id="72a70-131">Add an HTTP Endpoint</span></span>

<span data-ttu-id="72a70-132">ソリューション エクスプ ローラーで、AzureApp プロジェクトを展開します。</span><span class="sxs-lookup"><span data-stu-id="72a70-132">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="72a70-133">ロール ノードを展開し、WorkerRole1 を右クリックし、**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="72a70-133">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="72a70-134">クリックして**エンドポイント**、 をクリックし、**エンドポイントの追加**します。</span><span class="sxs-lookup"><span data-stu-id="72a70-134">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="72a70-135">**プロトコル**ドロップダウン リストで、"http"を選択します。</span><span class="sxs-lookup"><span data-stu-id="72a70-135">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="72a70-136">**パブリック ポート**と**プライベート ポート**80」と入力します。</span><span class="sxs-lookup"><span data-stu-id="72a70-136">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="72a70-137">これらのポート番号が異なることがあります。</span><span class="sxs-lookup"><span data-stu-id="72a70-137">These port numbers can be different.</span></span> <span data-ttu-id="72a70-138">パブリック ポートは、ロール要求を送信するときに使用するクライアントです。</span><span class="sxs-lookup"><span data-stu-id="72a70-138">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="72a70-139">用の Web API を構成する自己ホスト</span><span class="sxs-lookup"><span data-stu-id="72a70-139">Configure Web API for Self-Host</span></span>

<span data-ttu-id="72a70-140">ソリューション エクスプ ローラーで WorkerRole1 プロジェクトを右クリックし、選択**追加** / **クラス**新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="72a70-140">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="72a70-141">クラスに `Startup` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="72a70-141">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="72a70-142">このファイルの定型コードのすべて、次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="72a70-142">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="72a70-143">Web API コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="72a70-143">Add a Web API Controller</span></span>

<span data-ttu-id="72a70-144">次に、Web API コント ローラー クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="72a70-144">Next, add a Web API controller class.</span></span> <span data-ttu-id="72a70-145">WorkerRole1 プロジェクトを右クリックして**追加** / **クラス**します。</span><span class="sxs-lookup"><span data-stu-id="72a70-145">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="72a70-146">TestController クラスの名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="72a70-146">Name the class TestController.</span></span> <span data-ttu-id="72a70-147">このファイルの定型コードのすべて、次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="72a70-147">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="72a70-148">わかりやすくするため、このコント ローラーはプレーン テキストを返す 2 つの GET メソッドだけを定義します。</span><span class="sxs-lookup"><span data-stu-id="72a70-148">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="72a70-149">OWIN ホストを開始します。</span><span class="sxs-lookup"><span data-stu-id="72a70-149">Start the OWIN Host</span></span>

<span data-ttu-id="72a70-150">WorkerRole.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="72a70-150">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="72a70-151">このクラスは、ワーカー ロールが開始および停止時に実行されるコードを定義します。</span><span class="sxs-lookup"><span data-stu-id="72a70-151">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="72a70-152">次の追加ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="72a70-152">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="72a70-153">追加、 **IDisposable**メンバーを`WorkerRole`クラス。</span><span class="sxs-lookup"><span data-stu-id="72a70-153">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="72a70-154">`OnStart`メソッドでは、ホストを起動する次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="72a70-154">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="72a70-155">**WebApp.Start**メソッドは、OWIN ホストを開始します。</span><span class="sxs-lookup"><span data-stu-id="72a70-155">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="72a70-156">名前、`Startup`クラスは、メソッドの型パラメーター。</span><span class="sxs-lookup"><span data-stu-id="72a70-156">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="72a70-157">慣例により、ホストが呼び出す、`Configure`このクラスのメソッド。</span><span class="sxs-lookup"><span data-stu-id="72a70-157">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="72a70-158">上書き、`OnStop`を破棄する、 *\_アプリ*インスタンス。</span><span class="sxs-lookup"><span data-stu-id="72a70-158">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="72a70-159">WorkerRole.cs の完全なコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="72a70-159">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="72a70-160">ソリューションをビルドし、f5 キーを押して Azure コンピューティング エミュレーターでアプリケーションをローカルで実行します。</span><span class="sxs-lookup"><span data-stu-id="72a70-160">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="72a70-161">ファイアウォールの設定によっては、ファイアウォール経由のエミュレーターを許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="72a70-161">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="72a70-162">次のような例外が発生した場合を参照してください[このブログの投稿](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx)問題を回避します。</span><span class="sxs-lookup"><span data-stu-id="72a70-162">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="72a70-163">"ファイルまたはアセンブリを読み込むことができません ' Microsoft.Owin、バージョン 2.0.2.0、カルチャを = = neutral, PublicKeyToken = 31bf3856ad364e35' またはその依存関係の 1 つ。</span><span class="sxs-lookup"><span data-stu-id="72a70-163">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="72a70-164">指定したアセンブリのマニフェスト定義では、アセンブリ参照は一致しません。</span><span class="sxs-lookup"><span data-stu-id="72a70-164">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="72a70-165">(HRESULT からの例外: 0x80131040)"</span><span class="sxs-lookup"><span data-stu-id="72a70-165">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="72a70-166">コンピューティング エミュレーターでは、エンドポイントに、ローカル IP アドレスが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="72a70-166">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="72a70-167">コンピューティング エミュレーター UI を表示することによって、IP アドレスを見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="72a70-167">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="72a70-168">タスク バー通知領域のエミュレーター アイコンを右クリックして**コンピューティング エミュレーター UI**します。</span><span class="sxs-lookup"><span data-stu-id="72a70-168">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="72a70-169">サービスの展開で、サービスの詳細情報の展開 [id]、IP アドレスを検索します。</span><span class="sxs-lookup"><span data-stu-id="72a70-169">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="72a70-170">Web ブラウザーを開き、http:// に移動します<em>アドレス</em>/テスト/1、位置<em>アドレス</em>は、コンピューティング エミュレーターによって割り当てられた IP アドレスは、たとえば、 `http://127.0.0.1:80/test/1`。</span><span class="sxs-lookup"><span data-stu-id="72a70-170">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="72a70-171">Web API コント ローラーからの応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="72a70-171">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="72a70-172">Azure に配置する</span><span class="sxs-lookup"><span data-stu-id="72a70-172">Deploy to Azure</span></span>

<span data-ttu-id="72a70-173">この手順では、Azure アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="72a70-173">For this step, you must have an Azure account.</span></span> <span data-ttu-id="72a70-174">1 つをいない場合は、ほんの数分で無料試用版アカウントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="72a70-174">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="72a70-175">詳細については、次を参照してください。 [Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)します。</span><span class="sxs-lookup"><span data-stu-id="72a70-175">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="72a70-176">ソリューション エクスプ ローラーで、AzureApp プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="72a70-176">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="72a70-177">**[発行]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="72a70-177">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="72a70-178">Azure アカウントにサインインしていない場合はクリックして**サインイン**します。</span><span class="sxs-lookup"><span data-stu-id="72a70-178">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="72a70-179">サインインした後、サブスクリプションを選択およびクリックして**次**します。</span><span class="sxs-lookup"><span data-stu-id="72a70-179">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="72a70-180">クラウド サービスの名前を入力し、リージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="72a70-180">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="72a70-181">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="72a70-181">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="72a70-182">**[発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="72a70-182">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="72a70-183">Azure アクティビティ ログ ウィンドウには、展開の進行状況が表示されます。</span><span class="sxs-lookup"><span data-stu-id="72a70-183">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="72a70-184">アプリが展開されると、参照 http://appname.cloudapp.net/test/1 します。</span><span class="sxs-lookup"><span data-stu-id="72a70-184">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="72a70-185">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="72a70-185">Additional Resources</span></span>

- [<span data-ttu-id="72a70-186">プロジェクト Katana の概要</span><span class="sxs-lookup"><span data-stu-id="72a70-186">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="72a70-187">GitHub の Katana プロジェクト</span><span class="sxs-lookup"><span data-stu-id="72a70-187">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
