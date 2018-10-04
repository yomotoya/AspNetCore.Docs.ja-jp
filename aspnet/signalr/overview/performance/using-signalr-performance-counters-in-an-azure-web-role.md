---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Azure Web ロールで SignalR パフォーマンス カウンターの使用 |Microsoft Docs
author: guardrex
description: インストールして Azure Web ロールで SignalR パフォーマンス カウンターを使用する方法。
keywords: ASP.NET,signalr,performance カウンター、azure の web ロール
ms.author: riande
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 7304ff17bb53f94bdee1e90602d206bf32184e37
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795461"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="c4fec-104">Azure Web ロールで SignalR パフォーマンス カウンターの使用</span><span class="sxs-lookup"><span data-stu-id="c4fec-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="c4fec-105">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c4fec-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c4fec-106">SignalR パフォーマンス カウンターは、Azure Web ロールで、アプリのパフォーマンスを監視するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="c4fec-107">カウンターは、Microsoft Azure Diagnostics によってキャプチャされます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="c4fec-108">Azure で SignalR パフォーマンス カウンターをインストールする*signalr.exe*を同じツールをスタンドアロンまたはオンプレミスのアプリで使用します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="c4fec-109">Azure ロールでは、一時的なものであるために、アプリをインストールし、起動時に SignalR パフォーマンス カウンターの登録を構成します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4fec-110">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="c4fec-110">Prerequisites</span></span>

* <span data-ttu-id="c4fec-111">Visual Studio 2015 または[2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span><span class="sxs-lookup"><span data-stu-id="c4fec-111">Visual Studio 2015 or [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span></span>
* <span data-ttu-id="c4fec-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **注: SDK のインストール後、コンピューターを再起動します。**</span><span class="sxs-lookup"><span data-stu-id="c4fec-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="c4fec-113">Microsoft Azure サブスクリプション: Azure の無料試用版アカウントのサインアップを参照してください。 [Azure 無料試用版](https://azure.microsoft.com/free/)します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="c4fec-114">SignalR パフォーマンス カウンターを公開する Azure Web ロールのアプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="c4fec-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="c4fec-115">Visual Studio を開きます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-115">Open Visual Studio.</span></span>

2. <span data-ttu-id="c4fec-116">Visual Studio で、次のように選択します。**ファイル** > **新規** > **プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-116">In Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="c4fec-117">**新しいプロジェクト**ダイアログ ボックスで、 **Visual c#** > **クラウド**、左側のカテゴリを選び、 **Azure クラウド サービス**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="c4fec-117">In the **New Project** dialog box, select the **Visual C#** > **Cloud** category on the left, and then select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="c4fec-118">アプリの名前を付けます**SignalRPerfCounters**選択 **[ok]** します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![新しいクラウド アプリケーション](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > <span data-ttu-id="c4fec-120">表示されない場合、**クラウド**テンプレートのカテゴリまたは**Azure クラウド サービス**テンプレートをインストールする必要があります、 **Azure 開発**for Visual Studio 2017 のワークロード。</span><span class="sxs-lookup"><span data-stu-id="c4fec-120">If you don't see the **Cloud** template category or the **Azure Cloud Service** template, you need to install the **Azure development** workload for Visual Studio 2017.</span></span> <span data-ttu-id="c4fec-121">選択、 **Visual Studio インストーラーを開く**の左下にあるリンク、**新しいプロジェクト**ダイアログが Visual Studio インストーラーを開きます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-121">Choose the **Open Visual Studio Installer** link on the bottom-left side of the **New Project** dialog to open Visual Studio Installer.</span></span> <span data-ttu-id="c4fec-122">選択、 **Azure 開発**ワークロードを選び、**変更**ワークロードのインストールを開始します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-122">Select the **Azure development** workload, and then choose **Modify** to start installing the workload.</span></span>
   >
   > ![Visual Studio インストーラーでの azure 開発ワークロード](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. <span data-ttu-id="c4fec-124">**新しい Microsoft Azure クラウド サービス**ダイアログ ボックスで、 **ASP.NET Web ロール**を選択し、>、ロールをプロジェクトに追加するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="c4fec-124">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="c4fec-125">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-125">Select **OK**.</span></span>

   ![ASP.NET Web ロールを追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. <span data-ttu-id="c4fec-127">**新しい ASP.NET Web アプリケーション - WebRole1**ダイアログ ボックスで、 **MVC**テンプレート、および選択し**OK**。</span><span class="sxs-lookup"><span data-stu-id="c4fec-127">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![MVC と Web API を追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. <span data-ttu-id="c4fec-129">**ソリューション エクスプ ローラー**、オープン、 *diagnostics.wadcfgx*ファイル**WebRole1**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-129">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![ソリューション エクスプ ローラーの場合は diagnostics.wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. <span data-ttu-id="c4fec-131">ファイルの内容を置き換えて、次の構成をファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-131">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. <span data-ttu-id="c4fec-132">開く、**パッケージ マネージャー コンソール**から**ツール** > **NuGet パッケージ マネージャー**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-132">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="c4fec-133">SignalR、SignalR ユーティリティ パッケージの最新バージョンをインストールするには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-133">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. <span data-ttu-id="c4fec-134">起動時やがリサイクルされるときに、ロール インスタンスに SignalR パフォーマンス カウンターをインストールするアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-134">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="c4fec-135">**ソリューション エクスプ ローラー**を右クリックし、 **WebRole1**順に選択して**追加** > **新しいフォルダー**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-135">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="c4fec-136">新しいフォルダーの名前*スタートアップ*します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-136">Name the new folder *Startup*.</span></span>

   ![スタートアップ フォルダーを追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. <span data-ttu-id="c4fec-138">コピー、 *signalr.exe*ファイル (を使用して追加、 **Microsoft.AspNet.SignalR.Utils**パッケージ) から\<プロジェクト フォルダー >/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils\< 。バージョン >/するツール、*スタートアップ*前の手順で作成したフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="c4fec-138">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="c4fec-139">**ソリューション エクスプ ローラー**を右クリックし、*スタートアップ*フォルダーと選択**追加** > **既存項目の**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-139">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="c4fec-140">表示されるダイアログ ボックスで、次のように選択します。 *signalr.exe*選択**追加**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-140">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Signalr.exe をプロジェクトに追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. <span data-ttu-id="c4fec-142">右クリックし、*スタートアップ*フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-142">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="c4fec-143">**[追加]** > **[新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-143">Select **Add** > **New Item**.</span></span> <span data-ttu-id="c4fec-144">選択、**全般**ノードを選択**テキスト ファイル**、新しい項目の名前と*SignalRPerfCounterInstall.cmd*します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-144">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="c4fec-145">このコマンド ファイルは、SignalR パフォーマンス カウンターを web ロールにインストールされます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-145">This command file will install the SignalR performance counters into the web role.</span></span>

    ![SignalR パフォーマンス カウンターのインストール バッチ ファイルを作成します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. <span data-ttu-id="c4fec-147">Visual Studio で作成すると、 *SignalRPerfCounterInstall.cmd*ファイルが自動的に開きますのメイン ウィンドウにします。</span><span class="sxs-lookup"><span data-stu-id="c4fec-147">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="c4fec-148">次のスクリプトを使用して、ファイルの内容を交換し、保存して、ファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-148">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="c4fec-149">このスクリプトが実行される*signalr.exe*、ロール インスタンスに SignalR パフォーマンス カウンターを追加します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-149">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. <span data-ttu-id="c4fec-150">選択、 *signalr.exe*ファイル**ソリューション エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-150">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="c4fec-151">ファイルの**プロパティ**設定**出力ディレクトリにコピー**に**常にコピー**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-151">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![コピーを常にコピーする出力ディレクトリに設定します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. <span data-ttu-id="c4fec-153">前の手順を繰り返して、 *SignalRPerfCounterInstall.cmd*ファイル。</span><span class="sxs-lookup"><span data-stu-id="c4fec-153">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>


16. <span data-ttu-id="c4fec-154">右クリックし、 *SignalRPerfCounterInstall.cmd*ファイルおよび選択**プログラムから開く**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-154">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="c4fec-155">表示されるダイアログ ボックスで、次のように選択します。**バイナリ エディター**選択と**OK**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-155">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![バイナリ エディターで開く](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. <span data-ttu-id="c4fec-157">バイナリ エディターでは、ファイルの先頭バイトを選択し、削除します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-157">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="c4fec-158">ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-158">Save and close the file.</span></span>

    ![先頭バイトを削除します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. <span data-ttu-id="c4fec-160">開いている*ServiceDefinition.csdef*を実行するスタートアップ タスクを追加して、 *SignalrPerfCounterInstall.cmd*ファイル サービスの起動時に。</span><span class="sxs-lookup"><span data-stu-id="c4fec-160">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. <span data-ttu-id="c4fec-161">開いている`Views/Shared/_Layout.cshtml`と jQuery のバンドルのスクリプト ファイルの末尾から削除します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-161">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. <span data-ttu-id="c4fec-162">継続的に呼び出す JavaScript クライアントを追加、`increment`サーバー上のメソッド。</span><span class="sxs-lookup"><span data-stu-id="c4fec-162">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="c4fec-163">開いている`Views/Home/Index.cshtml`内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-163">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. <span data-ttu-id="c4fec-164">新しいフォルダーを作成、 **WebRole1**という名前のプロジェクト*Hubs*します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-164">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="c4fec-165">右クリックし、 *Hubs*フォルダー**ソリューション エクスプ ローラー**選択と**追加** > **新しい項目の**。</span><span class="sxs-lookup"><span data-stu-id="c4fec-165">Right-click the *Hubs* folder in **Solution Explorer** and select **Add** > **New Item**.</span></span> <span data-ttu-id="c4fec-166">**新しい項目の追加**ダイアログ ボックスで、 **Web** > **SignalR**カテゴリ、および選択し、 **SignalR ハブ クラス (v2)** 項目テンプレート。</span><span class="sxs-lookup"><span data-stu-id="c4fec-166">In the **Add New Item** dialog box, select the **Web** > **SignalR** category, and then select the **SignalR Hub Class (v2)** item template.</span></span> <span data-ttu-id="c4fec-167">新しいハブの名前を付けます*MyHub.cs*選択**追加**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-167">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![[新しい項目の追加] ダイアログ ボックスのハブ フォルダーに追加する SignalR ハブ クラス](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="c4fec-169">*MyHub.cs*のメイン ウィンドウが自動的に開きます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-169">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="c4fec-170">内容を次のコードに置き換えますを保存し、ファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-170">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. <span data-ttu-id="c4fec-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* 接続密度テスト SignalR コードベースで提供されるツールです。</span><span class="sxs-lookup"><span data-stu-id="c4fec-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="c4fec-172">Crank は、永続的な接続を必要とするために追加するサイトを使用してテストするときにします。</span><span class="sxs-lookup"><span data-stu-id="c4fec-172">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="c4fec-173">新しいフォルダーを追加、 **WebRole1**という名前のプロジェクト*PersistentConnections*します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-173">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="c4fec-174">このフォルダーを右クリックして**追加** > **クラス**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-174">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="c4fec-175">新しいクラス ファイルに名前*MyPersistentConnections.cs*選択**追加**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-175">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="c4fec-176">Visual Studio が開き、 *MyPersistentConnections.cs*メイン ウィンドウ内のファイル。</span><span class="sxs-lookup"><span data-stu-id="c4fec-176">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="c4fec-177">内容を次のコードに置き換えますを保存し、ファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-177">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. <span data-ttu-id="c4fec-178">使用して、`Startup`クラス、SignalR オブジェクトは、OWIN の起動時を起動します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-178">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="c4fec-179">開くか作成*Startup.cs*内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-179">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    <span data-ttu-id="c4fec-180">上記のコードで、`OwinStartup`属性は OWIN を開始するには、このクラスをマークします。</span><span class="sxs-lookup"><span data-stu-id="c4fec-180">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="c4fec-181">`Configuration`メソッドは、SignalR を開始します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-181">The `Configuration` method starts SignalR.</span></span>

26. <span data-ttu-id="c4fec-182">Microsoft Azure エミュレーターでキーを押してアプリケーションをテスト**F5**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-182">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c4fec-183">発生した場合、 **FileLoadException**で**MapSignalR**でバインド リダイレクトを変更する*web.config*次。</span><span class="sxs-lookup"><span data-stu-id="c4fec-183">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. <span data-ttu-id="c4fec-184">約 1 分間待機します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-184">Wait about one minute.</span></span> <span data-ttu-id="c4fec-185">Visual Studio で Cloud Explorer ツール ウィンドウを開きます (**ビュー** > **Cloud Explorer**) パスを展開および`(Local)/Storage Accounts/(Development)/Tables`します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-185">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="c4fec-186">ダブルクリック**WADPerformanceCountersTable**します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-186">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="c4fec-187">テーブルのデータで SignalR カウンターが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-187">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="c4fec-188">テーブルが表示されない場合は、Azure ストレージの資格情報を再入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c4fec-188">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="c4fec-189">選択する必要があります、**更新**ボタンをクリックして、テーブルで**Cloud Explorer**または選択、**更新**テーブル内のデータを表示するテーブルを開くウィンドウのボタン。</span><span class="sxs-lookup"><span data-stu-id="c4fec-189">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Visual Studio Cloud Explorer で WAD パフォーマンス カウンター テーブルを選択します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![WAD パフォーマンス カウンターのテーブルで収集されたカウンターの表示](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. <span data-ttu-id="c4fec-192">クラウドでアプリケーションをテストするには、更新、 **ServiceConfiguration.Cloud.cscfg**ファイルし、設定、`Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`有効な Azure Storage アカウント接続文字列にします。</span><span class="sxs-lookup"><span data-stu-id="c4fec-192">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="c4fec-193">Azure サブスクリプションにアプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-193">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="c4fec-194">アプリケーションを Azure にデプロイする方法の詳細については、次を参照してください。[を作成して、クラウド サービスをデプロイする方法](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-194">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="c4fec-195">数分待ってから。</span><span class="sxs-lookup"><span data-stu-id="c4fec-195">Wait a few minutes.</span></span> <span data-ttu-id="c4fec-196">**Cloud Explorer**先ほど構成したストレージ アカウントを見つけて、検索、`WADPerformanceCountersTable`内のテーブル。</span><span class="sxs-lookup"><span data-stu-id="c4fec-196">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="c4fec-197">テーブルのデータで SignalR カウンターが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c4fec-197">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="c4fec-198">テーブルが表示されない場合は、Azure ストレージの資格情報を再入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c4fec-198">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="c4fec-199">選択する必要があります、**更新**ボタンをクリックして、テーブルで**Cloud Explorer**または選択、**更新**テーブル内のデータを表示するテーブルを開くウィンドウのボタン。</span><span class="sxs-lookup"><span data-stu-id="c4fec-199">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="c4fec-200">感謝します。 特別な[Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard)の元のコンテンツをこのチュートリアルで使用します。</span><span class="sxs-lookup"><span data-stu-id="c4fec-200">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
