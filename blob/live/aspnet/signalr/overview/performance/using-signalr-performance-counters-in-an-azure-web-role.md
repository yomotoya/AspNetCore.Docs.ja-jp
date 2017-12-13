---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: "SignalR パフォーマンス カウンターを使用して、Azure Web ロールで |Microsoft ドキュメント"
author: guardrex
description: "インストールして、Azure Web ロールで SignalR パフォーマンス カウンターを使用する方法です。"
keywords: "ASP.NET,signalr,performance カウンター、azure web ロール"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 0d2717eb318d282e21e9aa8622a205f556e3a4ee
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="41658-104">SignalR パフォーマンス カウンターを使用して、Azure の Web ロール</span><span class="sxs-lookup"><span data-stu-id="41658-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="41658-105">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="41658-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="41658-106">SignalR パフォーマンス カウンターは、Azure Web ロールで、アプリのパフォーマンスの監視に使用されます。</span><span class="sxs-lookup"><span data-stu-id="41658-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="41658-107">カウンターは、Microsoft Azure Diagnostics によってキャプチャされます。</span><span class="sxs-lookup"><span data-stu-id="41658-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="41658-108">SignalR パフォーマンス カウンターを使用した Azure にインストールする*signalr.exe*、スタンドアロンまたは内部設置型のアプリに使用される、同じツールです。</span><span class="sxs-lookup"><span data-stu-id="41658-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="41658-109">Azure のロールが一時的なので、アプリをインストールし、起動時の SignalR パフォーマンス カウンターを登録を構成します。</span><span class="sxs-lookup"><span data-stu-id="41658-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="41658-110">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="41658-110">Prerequisites</span></span>

* [<span data-ttu-id="41658-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="41658-111">Visual Studio 2015</span></span>](https://www.visualstudio.com/vs/visual-studio-express/)
* <span data-ttu-id="41658-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **注: SDK をインストールした後、コンピューターを再起動します。**</span><span class="sxs-lookup"><span data-stu-id="41658-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="41658-113">Microsoft Azure サブスクリプション: 無料の試用アカウントを Azure にサインアップするには、「 [Azure 無料試用版](https://azure.microsoft.com/free/)です。</span><span class="sxs-lookup"><span data-stu-id="41658-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="41658-114">SignalR パフォーマンス カウンターを表示する Azure Web ロール アプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="41658-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="41658-115">Visual Studio 2015 を開きます。</span><span class="sxs-lookup"><span data-stu-id="41658-115">Open Visual Studio 2015.</span></span>

2. <span data-ttu-id="41658-116">Visual Studio 2015 では、次のように選択します。**ファイル&gt;新規&gt;プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="41658-116">In Visual Studio 2015, select **File &gt; New &gt; Project**.</span></span>

3. <span data-ttu-id="41658-117">**テンプレート**のペイン、**新しいプロジェクト** ウィンドウの下、 **Visual c#**ノードで、選択、**クラウド**ノードを選択、 **Azure クラウド サービス**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="41658-117">In the **Templates** pane of the **New Project** window under the **Visual C#** node, select the **Cloud** node and select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="41658-118">アプリの名前を付けます**SignalRPerfCounters**を選択し、 **OK**です。</span><span class="sxs-lookup"><span data-stu-id="41658-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![新しいクラウド アプリケーション](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. <span data-ttu-id="41658-120">**新しい Microsoft Azure クラウド サービス**ダイアログで、 **ASP.NET Web ロール**を選択し、  **&gt;** ロール プロジェクトを追加する、ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="41658-120">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the **&gt;** button to add the role to the project.</span></span> <span data-ttu-id="41658-121">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="41658-121">Select **OK**.</span></span>

   ![ASP.NET Web ロールを追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. <span data-ttu-id="41658-123">**新しい ASP.NET Web アプリケーション - WebRole1**ダイアログで、選択、 **MVC**テンプレート、および選択**OK**です。</span><span class="sxs-lookup"><span data-stu-id="41658-123">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![MVC と Web API を追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. <span data-ttu-id="41658-125">**ソリューション エクスプ ローラー**を開き、 *diagnostics.wadcfgx*下にあるファイル**WebRole1**です。</span><span class="sxs-lookup"><span data-stu-id="41658-125">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![ソリューション エクスプ ローラー diagnostics.wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. <span data-ttu-id="41658-127">次の構成で、ファイルの内容を交換し、ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="41658-127">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. <span data-ttu-id="41658-128">開く、 **Package Manager Console**から**ツール&gt;NuGet Package Manager**です。</span><span class="sxs-lookup"><span data-stu-id="41658-128">Open the **Package Manager Console** from **Tools &gt; NuGet Package Manager**.</span></span> <span data-ttu-id="41658-129">SignalR、SignalR ユーティリティ パッケージの最新バージョンをインストールするには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="41658-129">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. <span data-ttu-id="41658-130">起動時やがリサイクルされるときに、ロール インスタンスに SignalR パフォーマンス カウンターをインストールするアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="41658-130">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="41658-131">**ソリューション エクスプ ローラー**を右クリックし、 **WebRole1**プロジェクトし、選択**追加&gt;新しいフォルダー**です。</span><span class="sxs-lookup"><span data-stu-id="41658-131">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add &gt; New Folder**.</span></span> <span data-ttu-id="41658-132">新しいフォルダーの名前を付けます*スタートアップ*です。</span><span class="sxs-lookup"><span data-stu-id="41658-132">Name the new folder *Startup*.</span></span>

   ![スタートアップ フォルダーを追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. <span data-ttu-id="41658-134">コピー、 *signalr.exe*ファイル (を使用して追加、 **Microsoft.AspNet.SignalR.Utils**パッケージ) から**&lt;プロジェクト フォルダー&gt;\SignalRPerfCounters\packages\Microsoft.AspNet.SignalR.Utils です。&lt;バージョン&gt;\tools**を*スタートアップ*前の手順で作成したフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="41658-134">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from **&lt;project folder&gt;\SignalRPerfCounters\packages\Microsoft.AspNet.SignalR.Utils.&lt;version&gt;\tools** to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="41658-135">**ソリューション エクスプ ローラー**を右クリックし、*スタートアップ*フォルダーと選択**追加&gt;既存項目の**します。</span><span class="sxs-lookup"><span data-stu-id="41658-135">In **Solution Explorer**, right-click the *Startup* folder and select **Add &gt; Existing Item**.</span></span> <span data-ttu-id="41658-136">表示されるダイアログ ボックスで、次のように選択します。 *signalr.exe*選択**追加**です。</span><span class="sxs-lookup"><span data-stu-id="41658-136">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Signalr.exe をプロジェクトに追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. <span data-ttu-id="41658-138">右クリックし、*スタートアップ*フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="41658-138">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="41658-139">選択**追加&gt;新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="41658-139">Select **Add &gt; New Item**.</span></span> <span data-ttu-id="41658-140">選択、**全般**ノード、**テキスト ファイル**、し、新しい項目の名前を付けます*SignalRPerfCounterInstall.cmd*です。</span><span class="sxs-lookup"><span data-stu-id="41658-140">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="41658-141">このコマンド ファイルは、SignalR パフォーマンス カウンターを web ロールにインストールされます。</span><span class="sxs-lookup"><span data-stu-id="41658-141">This command file will install the SignalR performance counters into the web role.</span></span>

    ![SignalR パフォーマンス カウンターのインストールのバッチ ファイルを作成します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. <span data-ttu-id="41658-143">Visual Studio を作成すると、 *SignalRPerfCounterInstall.cmd*ファイルが自動的に開きますのメイン ウィンドウでします。</span><span class="sxs-lookup"><span data-stu-id="41658-143">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="41658-144">次のスクリプト ファイルの内容を置き換えますし、保存してファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="41658-144">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="41658-145">このスクリプトは実行*signalr.exe*、ロール インスタンスに SignalR パフォーマンス カウンターを追加します。</span><span class="sxs-lookup"><span data-stu-id="41658-145">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. <span data-ttu-id="41658-146">選択、 *signalr.exe*ファイル**ソリューション エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="41658-146">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="41658-147">ファイルの**プロパティ**設定、**出力ディレクトリにコピー**に**常にコピー**です。</span><span class="sxs-lookup"><span data-stu-id="41658-147">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![常にコピーする出力ディレクトリにコピーを設定します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. <span data-ttu-id="41658-149">前の手順を繰り返して、 *SignalRPerfCounterInstall.cmd*ファイル。</span><span class="sxs-lookup"><span data-stu-id="41658-149">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

    
16. <span data-ttu-id="41658-150">右クリックし、 *SignalRPerfCounterInstall.cmd*ファイルおよび選択した**ファイルを開く**です。</span><span class="sxs-lookup"><span data-stu-id="41658-150">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="41658-151">表示されるダイアログ ボックスで、次のように選択します。**バイナリ エディター**選択**OK**です。</span><span class="sxs-lookup"><span data-stu-id="41658-151">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![バイナリ エディターで開く](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. <span data-ttu-id="41658-153">バイナリ エディターで、ファイルの先頭バイトを選択し、削除します。</span><span class="sxs-lookup"><span data-stu-id="41658-153">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="41658-154">ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="41658-154">Save and close the file.</span></span>

    ![先頭バイトを削除します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. <span data-ttu-id="41658-156">開いている*ServiceDefinition.csdef*を実行するスタートアップ タスクを追加して、 *SignalrPerfCounterInstall.cmd*ファイル、サービスを開始するとき。</span><span class="sxs-lookup"><span data-stu-id="41658-156">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. <span data-ttu-id="41658-157">開いている`Views/Shared/_Layout.cshtml`と jQuery のバンドル スクリプト ファイルの末尾から削除します。</span><span class="sxs-lookup"><span data-stu-id="41658-157">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. <span data-ttu-id="41658-158">継続的に呼び出す JavaScript クライアントを追加、`increment`サーバー上のメソッドです。</span><span class="sxs-lookup"><span data-stu-id="41658-158">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="41658-159">開いている`Views/Home/Index.cshtml`し、内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="41658-159">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. <span data-ttu-id="41658-160">新しいフォルダーを作成、 **WebRole1**という名前のプロジェクト*ハブ*です。</span><span class="sxs-lookup"><span data-stu-id="41658-160">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="41658-161">右クリックし、*ハブ*フォルダー**ソリューション エクスプ ローラー** **Web &gt; SignalR**を選択し、 **SignalR ハブ クラス (v2)**.</span><span class="sxs-lookup"><span data-stu-id="41658-161">Right-click the *Hubs* folder in **Solution Explorer**, select **Web &gt; SignalR**, and select **SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="41658-162">新しいハブの名前を付けます*MyHub.cs*選択**追加**です。</span><span class="sxs-lookup"><span data-stu-id="41658-162">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![SignalR ハブ クラス フォルダーへの追加、ハブで新しい項目の追加 ダイアログ](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="41658-164">*MyHub.cs*が自動的にメイン ウィンドウで開きます。</span><span class="sxs-lookup"><span data-stu-id="41658-164">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="41658-165">内容を次のコードに置き換え、保存したファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="41658-165">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. <span data-ttu-id="41658-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  SignalR コードベースで提供されるツールのテスト接続密度がします。</span><span class="sxs-lookup"><span data-stu-id="41658-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="41658-167">クランクは、永続的な接続を必要とするため追加する 1 つを使用するサイトをテストする場合。</span><span class="sxs-lookup"><span data-stu-id="41658-167">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="41658-168">新しいフォルダーを追加、 **WebRole1**というプロジェクト*PersistentConnections*です。</span><span class="sxs-lookup"><span data-stu-id="41658-168">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="41658-169">このフォルダーを右クリックし **追加&gt;クラス**です。</span><span class="sxs-lookup"><span data-stu-id="41658-169">Right-click this folder and select **Add &gt; Class**.</span></span> <span data-ttu-id="41658-170">新しいクラス ファイルの名前を付けます*MyPersistentConnections.cs*選択**追加**です。</span><span class="sxs-lookup"><span data-stu-id="41658-170">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="41658-171">Visual Studio で開く、 *MyPersistentConnections.cs*メイン ウィンドウ内のファイルです。</span><span class="sxs-lookup"><span data-stu-id="41658-171">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="41658-172">内容を次のコードに置き換え、保存したファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="41658-172">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. <span data-ttu-id="41658-173">使用して、 `Startup` OWIN の起動時にはクラス、SignalR のオブジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="41658-173">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="41658-174">開くか作成*Startup.cs*し、内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="41658-174">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    <span data-ttu-id="41658-175">上記のコードで、`OwinStartup`属性は、OWIN を開始するには、このクラスをマークします。</span><span class="sxs-lookup"><span data-stu-id="41658-175">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="41658-176">`Configuration`メソッドは、SignalR を開始します。</span><span class="sxs-lookup"><span data-stu-id="41658-176">The `Configuration` method starts SignalR.</span></span>
    
26. <span data-ttu-id="41658-177">Microsoft Azure エミュレーターでキーを押してアプリケーションをテスト**f5 キーを押して**です。</span><span class="sxs-lookup"><span data-stu-id="41658-177">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="41658-178">発生した場合、 **FileLoadException**で**MapSignalR**でバインド リダイレクトを変更する*web.config*以下。</span><span class="sxs-lookup"><span data-stu-id="41658-178">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. <span data-ttu-id="41658-179">約 1 分間待機します。</span><span class="sxs-lookup"><span data-stu-id="41658-179">Wait about one minute.</span></span> <span data-ttu-id="41658-180">Visual Studio で、クラウド エクスプ ローラー ツール ウィンドウを開きます (**ビュー &gt; Cloud Explorer**) パスを展開および`(Local)\Storage Accounts\(Development)\Tables`です。</span><span class="sxs-lookup"><span data-stu-id="41658-180">Open the Cloud Explorer tool window in Visual Studio (**View &gt; Cloud Explorer**) and expand the path `(Local)\Storage Accounts\(Development)\Tables`.</span></span> <span data-ttu-id="41658-181">ダブルクリックして**WADPerformanceCountersTable**です。</span><span class="sxs-lookup"><span data-stu-id="41658-181">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="41658-182">SignalR カウンター テーブル データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="41658-182">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="41658-183">テーブルが見つからない場合は、Azure ストレージの資格情報を再入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="41658-183">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="41658-184">選択する必要があります、**更新**内のテーブルを表示するボタン**Cloud Explorer**かを選択して、**更新**をテーブル内のデータを表示するテーブルを開くウィンドウのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="41658-184">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Visual Studio Cloud Explorer で WAD パフォーマンス カウンター テーブルを選択します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![WAD パフォーマンス カウンター テーブルで収集されたカウンターの表示](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. <span data-ttu-id="41658-187">クラウドでアプリケーションをテストするには、更新、 **ServiceConfiguration.Cloud.cscfg**ファイルし、設定、`Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`に有効な Azure ストレージ アカウント接続文字列。</span><span class="sxs-lookup"><span data-stu-id="41658-187">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="41658-188">Azure サブスクリプションへのアプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="41658-188">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="41658-189">Azure にアプリケーションを展開する方法の詳細については、「[を作成し、クラウド サービスを展開する方法](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)です。</span><span class="sxs-lookup"><span data-stu-id="41658-189">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="41658-190">数分待ってからです。</span><span class="sxs-lookup"><span data-stu-id="41658-190">Wait a few minutes.</span></span> <span data-ttu-id="41658-191">**Cloud Explorer**上で構成したストレージ アカウントを見つけて、検索、`WADPerformanceCountersTable`内のテーブルです。</span><span class="sxs-lookup"><span data-stu-id="41658-191">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="41658-192">SignalR カウンター テーブル データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="41658-192">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="41658-193">テーブルが見つからない場合は、Azure ストレージの資格情報を再入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="41658-193">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="41658-194">選択する必要があります、**更新**内のテーブルを表示するボタン**Cloud Explorer**かを選択して、**更新**をテーブル内のデータを表示するテーブルを開くウィンドウのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="41658-194">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="41658-195">感謝の特別な[Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard)の元の内容をこのチュートリアルで使用します。</span><span class="sxs-lookup"><span data-stu-id="41658-195">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
