---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Azure Web ロールで SignalR パフォーマンス カウンターの使用 |Microsoft Docs
author: guardrex
description: インストールして Azure Web ロールで SignalR パフォーマンス カウンターを使用する方法。
keywords: ASP.NET,signalr,performance カウンター、azure の web ロール
ms.author: riande
ms.date: 02/11/2017
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: acc9836535466a801f1f46ec18d05937e2e42af2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828924"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="68eed-104">Azure Web ロールで SignalR パフォーマンス カウンターの使用</span><span class="sxs-lookup"><span data-stu-id="68eed-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="68eed-105">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="68eed-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="68eed-106">SignalR パフォーマンス カウンターは、Azure Web ロールで、アプリのパフォーマンスを監視するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="68eed-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="68eed-107">カウンターは、Microsoft Azure Diagnostics によってキャプチャされます。</span><span class="sxs-lookup"><span data-stu-id="68eed-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="68eed-108">Azure で SignalR パフォーマンス カウンターをインストールする*signalr.exe*を同じツールをスタンドアロンまたはオンプレミスのアプリで使用します。</span><span class="sxs-lookup"><span data-stu-id="68eed-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="68eed-109">Azure ロールでは、一時的なものであるために、アプリをインストールし、起動時に SignalR パフォーマンス カウンターの登録を構成します。</span><span class="sxs-lookup"><span data-stu-id="68eed-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="68eed-110">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="68eed-110">Prerequisites</span></span>

* [<span data-ttu-id="68eed-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="68eed-111">Visual Studio 2015</span></span>](https://www.visualstudio.com/vs/visual-studio-express/)
* <span data-ttu-id="68eed-112">[Microsoft の Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **注: SDK のインストール後、コンピューターを再起動します。**</span><span class="sxs-lookup"><span data-stu-id="68eed-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="68eed-113">Microsoft Azure サブスクリプション: Azure の無料試用版アカウントのサインアップを参照してください。 [Azure 無料試用版](https://azure.microsoft.com/free/)します。</span><span class="sxs-lookup"><span data-stu-id="68eed-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="68eed-114">SignalR パフォーマンス カウンターを公開する Azure Web ロールのアプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="68eed-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="68eed-115">Visual Studio 2015 を開きます。</span><span class="sxs-lookup"><span data-stu-id="68eed-115">Open Visual Studio 2015.</span></span>

2. <span data-ttu-id="68eed-116">Visual Studio 2015 では、次のように選択します。**ファイル** > **新規** > **プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-116">In Visual Studio 2015, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="68eed-117">**テンプレート**のウィンドウ、**新しいプロジェクト**] ウィンドウの [、 **Visual c#** ノードを選択、**クラウド**ノードと選択、 **Azure クラウド サービス**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="68eed-117">In the **Templates** pane of the **New Project** window under the **Visual C#** node, select the **Cloud** node and select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="68eed-118">アプリの名前を付けます**SignalRPerfCounters**選択 **[ok]** します。</span><span class="sxs-lookup"><span data-stu-id="68eed-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![新しいクラウド アプリケーション](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. <span data-ttu-id="68eed-120">**新しい Microsoft Azure クラウド サービス**ダイアログ ボックスで、 **ASP.NET Web ロール**を選択し、>、ロールをプロジェクトに追加するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="68eed-120">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="68eed-121">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="68eed-121">Select **OK**.</span></span>

   ![ASP.NET Web ロールを追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. <span data-ttu-id="68eed-123">**新しい ASP.NET Web アプリケーション - WebRole1**ダイアログ ボックスで、 **MVC**テンプレート、および選択し**OK**。</span><span class="sxs-lookup"><span data-stu-id="68eed-123">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![MVC と Web API を追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. <span data-ttu-id="68eed-125">**ソリューション エクスプ ローラー**、オープン、 *diagnostics.wadcfgx*ファイル**WebRole1**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-125">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![ソリューション エクスプ ローラーの場合は diagnostics.wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. <span data-ttu-id="68eed-127">ファイルの内容を置き換えて、次の構成をファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="68eed-127">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. <span data-ttu-id="68eed-128">開く、**パッケージ マネージャー コンソール**から**ツール** > **NuGet パッケージ マネージャー**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-128">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="68eed-129">SignalR、SignalR ユーティリティ パッケージの最新バージョンをインストールするには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="68eed-129">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. <span data-ttu-id="68eed-130">起動時やがリサイクルされるときに、ロール インスタンスに SignalR パフォーマンス カウンターをインストールするアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="68eed-130">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="68eed-131">**ソリューション エクスプ ローラー**を右クリックし、 **WebRole1**順に選択して**追加** > **新しいフォルダー**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-131">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="68eed-132">新しいフォルダーの名前*スタートアップ*します。</span><span class="sxs-lookup"><span data-stu-id="68eed-132">Name the new folder *Startup*.</span></span>

   ![スタートアップ フォルダーを追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. <span data-ttu-id="68eed-134">コピー、 *signalr.exe*ファイル (を使用して追加、 **Microsoft.AspNet.SignalR.Utils**パッケージ) から\<プロジェクト フォルダー >/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils\< 。バージョン >/するツール、*スタートアップ*前の手順で作成したフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="68eed-134">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="68eed-135">**ソリューション エクスプ ローラー**を右クリックし、*スタートアップ*フォルダーと選択**追加** > **既存項目の**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-135">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="68eed-136">表示されるダイアログ ボックスで、次のように選択します。 *signalr.exe*選択**追加**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-136">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Signalr.exe をプロジェクトに追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. <span data-ttu-id="68eed-138">右クリックし、*スタートアップ*フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="68eed-138">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="68eed-139">**[追加]** > **[新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="68eed-139">Select **Add** > **New Item**.</span></span> <span data-ttu-id="68eed-140">選択、**全般**ノードを選択**テキスト ファイル**、新しい項目の名前と*SignalRPerfCounterInstall.cmd*します。</span><span class="sxs-lookup"><span data-stu-id="68eed-140">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="68eed-141">このコマンド ファイルは、SignalR パフォーマンス カウンターを web ロールにインストールされます。</span><span class="sxs-lookup"><span data-stu-id="68eed-141">This command file will install the SignalR performance counters into the web role.</span></span>

    ![SignalR パフォーマンス カウンターのインストール バッチ ファイルを作成します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. <span data-ttu-id="68eed-143">Visual Studio で作成すると、 *SignalRPerfCounterInstall.cmd*ファイルが自動的に開きますのメイン ウィンドウにします。</span><span class="sxs-lookup"><span data-stu-id="68eed-143">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="68eed-144">次のスクリプトを使用して、ファイルの内容を交換し、保存して、ファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="68eed-144">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="68eed-145">このスクリプトが実行される*signalr.exe*、ロール インスタンスに SignalR パフォーマンス カウンターを追加します。</span><span class="sxs-lookup"><span data-stu-id="68eed-145">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. <span data-ttu-id="68eed-146">選択、 *signalr.exe*ファイル**ソリューション エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-146">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="68eed-147">ファイルの**プロパティ**設定**出力ディレクトリにコピー**に**常にコピー**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-147">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![コピーを常にコピーする出力ディレクトリに設定します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. <span data-ttu-id="68eed-149">前の手順を繰り返して、 *SignalRPerfCounterInstall.cmd*ファイル。</span><span class="sxs-lookup"><span data-stu-id="68eed-149">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

    
16. <span data-ttu-id="68eed-150">右クリックし、 *SignalRPerfCounterInstall.cmd*ファイルおよび選択**プログラムから開く**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-150">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="68eed-151">表示されるダイアログ ボックスで、次のように選択します。**バイナリ エディター**選択と**OK**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-151">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![バイナリ エディターで開く](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. <span data-ttu-id="68eed-153">バイナリ エディターでは、ファイルの先頭バイトを選択し、削除します。</span><span class="sxs-lookup"><span data-stu-id="68eed-153">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="68eed-154">ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="68eed-154">Save and close the file.</span></span>

    ![先頭バイトを削除します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. <span data-ttu-id="68eed-156">開いている*ServiceDefinition.csdef*を実行するスタートアップ タスクを追加して、 *SignalrPerfCounterInstall.cmd*ファイル サービスの起動時に。</span><span class="sxs-lookup"><span data-stu-id="68eed-156">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. <span data-ttu-id="68eed-157">開いている`Views/Shared/_Layout.cshtml`と jQuery のバンドルのスクリプト ファイルの末尾から削除します。</span><span class="sxs-lookup"><span data-stu-id="68eed-157">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. <span data-ttu-id="68eed-158">継続的に呼び出す JavaScript クライアントを追加、`increment`サーバー上のメソッド。</span><span class="sxs-lookup"><span data-stu-id="68eed-158">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="68eed-159">開いている`Views/Home/Index.cshtml`内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="68eed-159">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. <span data-ttu-id="68eed-160">新しいフォルダーを作成、 **WebRole1**という名前のプロジェクト*Hubs*します。</span><span class="sxs-lookup"><span data-stu-id="68eed-160">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="68eed-161">右クリックし、 *Hubs*フォルダー**ソリューション エクスプ ローラー**を選択します**Web** > **SignalR**、選び**SignalR ハブ クラス (v2)** します。</span><span class="sxs-lookup"><span data-stu-id="68eed-161">Right-click the *Hubs* folder in **Solution Explorer**, select **Web** > **SignalR**, and select **SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="68eed-162">新しいハブの名前を付けます*MyHub.cs*選択**追加**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-162">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![[新しい項目の追加] ダイアログ ボックスのハブ フォルダーに追加する SignalR ハブ クラス](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="68eed-164">*MyHub.cs*のメイン ウィンドウが自動的に開きます。</span><span class="sxs-lookup"><span data-stu-id="68eed-164">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="68eed-165">内容を次のコードに置き換えますを保存し、ファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="68eed-165">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. <span data-ttu-id="68eed-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* 接続密度テスト SignalR コードベースで提供されるツールです。</span><span class="sxs-lookup"><span data-stu-id="68eed-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="68eed-167">Crank は、永続的な接続を必要とするために追加するサイトを使用してテストするときにします。</span><span class="sxs-lookup"><span data-stu-id="68eed-167">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="68eed-168">新しいフォルダーを追加、 **WebRole1**という名前のプロジェクト*PersistentConnections*します。</span><span class="sxs-lookup"><span data-stu-id="68eed-168">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="68eed-169">このフォルダーを右クリックして**追加** > **クラス**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-169">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="68eed-170">新しいクラス ファイルに名前*MyPersistentConnections.cs*選択**追加**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-170">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="68eed-171">Visual Studio が開き、 *MyPersistentConnections.cs*メイン ウィンドウ内のファイル。</span><span class="sxs-lookup"><span data-stu-id="68eed-171">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="68eed-172">内容を次のコードに置き換えますを保存し、ファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="68eed-172">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. <span data-ttu-id="68eed-173">使用して、`Startup`クラス、SignalR オブジェクトは、OWIN の起動時を起動します。</span><span class="sxs-lookup"><span data-stu-id="68eed-173">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="68eed-174">開くか作成*Startup.cs*内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="68eed-174">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    <span data-ttu-id="68eed-175">上記のコードで、`OwinStartup`属性は OWIN を開始するには、このクラスをマークします。</span><span class="sxs-lookup"><span data-stu-id="68eed-175">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="68eed-176">`Configuration`メソッドは、SignalR を開始します。</span><span class="sxs-lookup"><span data-stu-id="68eed-176">The `Configuration` method starts SignalR.</span></span>
    
26. <span data-ttu-id="68eed-177">Microsoft Azure エミュレーターでキーを押してアプリケーションをテスト**F5**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-177">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="68eed-178">発生した場合、 **FileLoadException**で**MapSignalR**でバインド リダイレクトを変更する*web.config*次。</span><span class="sxs-lookup"><span data-stu-id="68eed-178">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. <span data-ttu-id="68eed-179">約 1 分間待機します。</span><span class="sxs-lookup"><span data-stu-id="68eed-179">Wait about one minute.</span></span> <span data-ttu-id="68eed-180">Visual Studio で Cloud Explorer ツール ウィンドウを開きます (**ビュー** > **Cloud Explorer**) パスを展開および`(Local)/Storage Accounts/(Development)/Tables`します。</span><span class="sxs-lookup"><span data-stu-id="68eed-180">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="68eed-181">ダブルクリック**WADPerformanceCountersTable**します。</span><span class="sxs-lookup"><span data-stu-id="68eed-181">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="68eed-182">テーブルのデータで SignalR カウンターが表示されます。</span><span class="sxs-lookup"><span data-stu-id="68eed-182">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="68eed-183">テーブルが表示されない場合は、Azure ストレージの資格情報を再入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="68eed-183">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="68eed-184">選択する必要があります、**更新**ボタンをクリックして、テーブルで**Cloud Explorer**または選択、**更新**テーブル内のデータを表示するテーブルを開くウィンドウのボタン。</span><span class="sxs-lookup"><span data-stu-id="68eed-184">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Visual Studio Cloud Explorer で WAD パフォーマンス カウンター テーブルを選択します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![WAD パフォーマンス カウンターのテーブルで収集されたカウンターの表示](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. <span data-ttu-id="68eed-187">クラウドでアプリケーションをテストするには、更新、 **ServiceConfiguration.Cloud.cscfg**ファイルし、設定、`Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`有効な Azure Storage アカウント接続文字列にします。</span><span class="sxs-lookup"><span data-stu-id="68eed-187">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="68eed-188">Azure サブスクリプションにアプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="68eed-188">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="68eed-189">アプリケーションを Azure にデプロイする方法の詳細については、次を参照してください。[を作成して、クラウド サービスをデプロイする方法](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)します。</span><span class="sxs-lookup"><span data-stu-id="68eed-189">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="68eed-190">数分待ってから。</span><span class="sxs-lookup"><span data-stu-id="68eed-190">Wait a few minutes.</span></span> <span data-ttu-id="68eed-191">**Cloud Explorer**先ほど構成したストレージ アカウントを見つけて、検索、`WADPerformanceCountersTable`内のテーブル。</span><span class="sxs-lookup"><span data-stu-id="68eed-191">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="68eed-192">テーブルのデータで SignalR カウンターが表示されます。</span><span class="sxs-lookup"><span data-stu-id="68eed-192">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="68eed-193">テーブルが表示されない場合は、Azure ストレージの資格情報を再入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="68eed-193">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="68eed-194">選択する必要があります、**更新**ボタンをクリックして、テーブルで**Cloud Explorer**または選択、**更新**テーブル内のデータを表示するテーブルを開くウィンドウのボタン。</span><span class="sxs-lookup"><span data-stu-id="68eed-194">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="68eed-195">感謝します。 特別な[Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard)の元のコンテンツをこのチュートリアルで使用します。</span><span class="sxs-lookup"><span data-stu-id="68eed-195">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
