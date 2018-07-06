---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Azure Service Bus による SignalR スケール アウト (SignalR 1.x) |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 751de591ec62942c2c9d8b26af40101eb2e922d5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820941"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="45f69-102">Azure Service Bus による SignalR スケール アウト (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="45f69-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="45f69-103">によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="45f69-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="45f69-104">このチュートリアルでは、Service Bus のバック プレーンを使用して各ロール インスタンスにメッセージを配信する、Windows Azure の Web ロールに SignalR アプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="45f69-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="45f69-105">必要条件:</span><span class="sxs-lookup"><span data-stu-id="45f69-105">Prerequisites:</span></span>

- <span data-ttu-id="45f69-106">Windows Azure アカウント。</span><span class="sxs-lookup"><span data-stu-id="45f69-106">A Windows Azure account.</span></span>
- <span data-ttu-id="45f69-107">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409)します。</span><span class="sxs-lookup"><span data-stu-id="45f69-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="45f69-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="45f69-108">Visual Studio 2012.</span></span>

<span data-ttu-id="45f69-109">Service bus のバック プレーンと互換性のあるも[Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)、バージョン 1.1。</span><span class="sxs-lookup"><span data-stu-id="45f69-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="45f69-110">ただし、Service Bus for Windows Server のバージョン 1.0 と互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="45f69-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="45f69-111">Pricing</span><span class="sxs-lookup"><span data-stu-id="45f69-111">Pricing</span></span>

<span data-ttu-id="45f69-112">Service Bus のバック プレーンでは、トピックを使用して、メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="45f69-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="45f69-113">最新の価格情報については、次を参照してください。 [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/)します。</span><span class="sxs-lookup"><span data-stu-id="45f69-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="45f69-114">この記事の執筆時に、1 ドル未満の 1 か月あたり 1,000,000 メッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="45f69-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="45f69-115">バック プレーンでは、SignalR のハブ メソッドの呼び出しごとの service bus メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="45f69-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="45f69-116">接続の切断、結合またはしたまま、やグループなどの一部のコントロール メッセージもあります。</span><span class="sxs-lookup"><span data-stu-id="45f69-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="45f69-117">ほとんどのアプリケーションでは、メッセージ トラフィックの大部分のハブ メソッド呼び出しとなります。</span><span class="sxs-lookup"><span data-stu-id="45f69-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="45f69-118">概要</span><span class="sxs-lookup"><span data-stu-id="45f69-118">Overview</span></span>

<span data-ttu-id="45f69-119">詳細なチュートリアルを始める前に、作業内容の簡単な概要を示します。</span><span class="sxs-lookup"><span data-stu-id="45f69-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="45f69-120">Windows Azure ポータルを使用すると、新しい Service Bus 名前空間を作成できます。</span><span class="sxs-lookup"><span data-stu-id="45f69-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="45f69-121">これらの NuGet パッケージをアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="45f69-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="45f69-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="45f69-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="45f69-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="45f69-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="45f69-124">SignalR アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="45f69-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="45f69-125">Global.asax バック プレーンを構成するには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="45f69-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="45f69-126">各アプリケーションでは、"YourAppName"を別の値を選択します。</span><span class="sxs-lookup"><span data-stu-id="45f69-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="45f69-127">複数のアプリケーションでは、同じ値を使用しません。</span><span class="sxs-lookup"><span data-stu-id="45f69-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="45f69-128">Azure サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="45f69-128">Create the Azure Services</span></span>

<span data-ttu-id="45f69-129">クラウド サービスを作成する」の説明に従って[を作成して、クラウド サービスをデプロイする方法](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)します。</span><span class="sxs-lookup"><span data-stu-id="45f69-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="45f69-130">セクションの手順に従って"する方法: 簡易作成を使用してクラウド サービスの作成"します。</span><span class="sxs-lookup"><span data-stu-id="45f69-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="45f69-131">このチュートリアルでは、証明書をアップロードする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="45f69-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="45f69-132">新しい Service Bus 名前空間を作成する」の説明に従って[方法を使用して Service Bus トピック/サブスクリプション](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)します。</span><span class="sxs-lookup"><span data-stu-id="45f69-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="45f69-133">「Service Namespace を作成する」セクションの手順をに従います。</span><span class="sxs-lookup"><span data-stu-id="45f69-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="45f69-134">クラウド サービスと Service Bus 名前空間の同じリージョンを選択してください。</span><span class="sxs-lookup"><span data-stu-id="45f69-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="45f69-135">Visual Studio プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="45f69-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="45f69-136">Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="45f69-136">Start Visual Studio.</span></span> <span data-ttu-id="45f69-137">**ファイル** メニューのをクリックして**新しいプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="45f69-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="45f69-138">**新しいプロジェクト** ダイアログ ボックスで、展開**Visual c#** します。</span><span class="sxs-lookup"><span data-stu-id="45f69-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="45f69-139">**インストールされたテンプレート**を選択します**クラウド**選び**Windows Azure クラウド サービス**します。</span><span class="sxs-lookup"><span data-stu-id="45f69-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="45f69-140">既定値を .NET Framework 4.5 を保持します。</span><span class="sxs-lookup"><span data-stu-id="45f69-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="45f69-141">ChatService アプリケーションの名前を指定し、をクリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="45f69-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="45f69-142">**新しい Windows Azure クラウド サービス**ダイアログ ボックスで、ASP.NET MVC 4 Web ロールを選択します。</span><span class="sxs-lookup"><span data-stu-id="45f69-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="45f69-143">右矢印ボタンをクリックします (**&gt;**)、ロールをソリューションに追加します。</span><span class="sxs-lookup"><span data-stu-id="45f69-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="45f69-144">マウスので、新しいロールでは、ポインターを置き、鉛筆アイコンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="45f69-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="45f69-145">ロールの名前を変更するには、このアイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="45f69-145">Click this icon to rename the role.</span></span> <span data-ttu-id="45f69-146">ロール"SignalRChat"という名前にして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="45f69-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="45f69-147">**新しい ASP.NET MVC 4 プロジェクト**ウィザードで、**インターネット アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="45f69-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="45f69-148">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="45f69-148">Click **OK**.</span></span> <span data-ttu-id="45f69-149">プロジェクト ウィザードには、2 つのプロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="45f69-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="45f69-150">ChatService: このプロジェクトは、Windows Azure アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="45f69-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="45f69-151">Azure のロールとその他の構成オプションを定義します。</span><span class="sxs-lookup"><span data-stu-id="45f69-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="45f69-152">SignalRChat: このプロジェクトは、ASP.NET MVC 4 プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="45f69-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="45f69-153">SignalR チャット アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="45f69-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="45f69-154">チャット アプリケーションを作成するには、チュートリアルの手順をに従って[SignalR と MVC 4 の概要](tutorial-getting-started-with-signalr-and-mvc-4.md)します。</span><span class="sxs-lookup"><span data-stu-id="45f69-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="45f69-155">必要なライブラリをインストールするのにには、NuGet を使用します。</span><span class="sxs-lookup"><span data-stu-id="45f69-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="45f69-156">**ツール**メニューの **ライブラリ パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="45f69-156">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="45f69-157">**パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="45f69-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="45f69-158">使用して、 `-ProjectName` Windows Azure プロジェクトではなく、ASP.NET MVC プロジェクトにパッケージをインストールするオプション。</span><span class="sxs-lookup"><span data-stu-id="45f69-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="45f69-159">バック プレーンを構成します。</span><span class="sxs-lookup"><span data-stu-id="45f69-159">Configure the Backplane</span></span>

<span data-ttu-id="45f69-160">アプリケーションの Global.asax ファイルでは、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="45f69-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="45f69-161">Service bus の接続文字列を取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="45f69-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="45f69-162">Azure portal で作成した service bus 名前空間を選択し、アクセス キー アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="45f69-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="45f69-163">接続文字列をクリップボードにコピーして貼り付けます、 *connectionString*変数。</span><span class="sxs-lookup"><span data-stu-id="45f69-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="45f69-164">Azure に配置する</span><span class="sxs-lookup"><span data-stu-id="45f69-164">Deploy to Azure</span></span>

<span data-ttu-id="45f69-165">ソリューション エクスプ ローラーで、**ロール**ChatService プロジェクト内のフォルダー。</span><span class="sxs-lookup"><span data-stu-id="45f69-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="45f69-166">SignalRChat ロールを右クリックして**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="45f69-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="45f69-167">選択、**構成**タブ。**インスタンス**2 を選択します。</span><span class="sxs-lookup"><span data-stu-id="45f69-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="45f69-168">VM のサイズを設定することもできます。**極小**します。</span><span class="sxs-lookup"><span data-stu-id="45f69-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="45f69-169">変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="45f69-169">Save the changes.</span></span>

<span data-ttu-id="45f69-170">ソリューション エクスプ ローラーで、ChatService プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="45f69-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="45f69-171">**[発行]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="45f69-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="45f69-172">Windows Azure に最初に公開する場合は、資格情報をダウンロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="45f69-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="45f69-173">**発行**ウィザード、[資格情報のダウンロードにサインイン] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="45f69-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="45f69-174">これは、操作によって、Windows Azure ポータルにサインインし、発行設定ファイルをダウンロードするように求められます。</span><span class="sxs-lookup"><span data-stu-id="45f69-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="45f69-175">クリックして**インポート**ダウンロードした発行設定ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="45f69-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="45f69-176">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="45f69-176">Click **Next**.</span></span> <span data-ttu-id="45f69-177">**発行設定**ダイアログで、**クラウド サービス**、先ほど作成したクラウド サービスを選択します。</span><span class="sxs-lookup"><span data-stu-id="45f69-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="45f69-178">**[発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="45f69-178">Click **Publish**.</span></span> <span data-ttu-id="45f69-179">アプリケーションを展開し、Vm を起動するまで数分かかることができます。</span><span class="sxs-lookup"><span data-stu-id="45f69-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="45f69-180">今すぐチャット アプリケーションを実行するときに、ロール インスタンスは、Service Bus トピックを使用して、Azure Service Bus を介して通信します。</span><span class="sxs-lookup"><span data-stu-id="45f69-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="45f69-181">トピックは、複数のサブスクライバーを許可するメッセージ キューです。</span><span class="sxs-lookup"><span data-stu-id="45f69-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="45f69-182">バック プレーンは、トピックおよびサブスクリプションに自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="45f69-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="45f69-183">サブスクリプションとメッセージ アクティビティを表示するには、Azure portal を開き、Service Bus 名前空間を選択して「トピック」をクリックします。</span><span class="sxs-lookup"><span data-stu-id="45f69-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="45f69-184">メッセージ アクティビティのダッシュ ボードに表示するまで数分かかることです。</span><span class="sxs-lookup"><span data-stu-id="45f69-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="45f69-185">SignalR では、トピックの「有効期間を管理します。</span><span class="sxs-lookup"><span data-stu-id="45f69-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="45f69-186">アプリケーションが展開されている限りは、トピックの設定を手動でのトピックを削除または変更しないでいます。</span><span class="sxs-lookup"><span data-stu-id="45f69-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
