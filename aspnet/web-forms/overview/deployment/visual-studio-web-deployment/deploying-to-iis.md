---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: Visual Studio を使用して ASP.NET Web の展開:テストへのデプロイ |Microsoft Docs
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: riande
ms.date: 01/16/2019
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: d49dfad368ca4b81bb865103a99ec223a1cc66df
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396338"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a><span data-ttu-id="a1fc9-103">Visual Studio を使用して ASP.NET Web の展開:テストへのデプロイ</span><span class="sxs-lookup"><span data-stu-id="a1fc9-103">ASP.NET Web Deployment using Visual Studio: Deploying to Test</span></span>
====================
<span data-ttu-id="a1fc9-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a1fc9-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="a1fc9-105">このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、または Visual Studio 2017 を使用して、サード パーティのホスティング プロバイダーにします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-105">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider using Visual Studio 2017.</span></span> <span data-ttu-id="a1fc9-106">系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-106">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="a1fc9-107">概要</span><span class="sxs-lookup"><span data-stu-id="a1fc9-107">Overview</span></span>

<span data-ttu-id="a1fc9-108">このチュートリアルでは、ローカル コンピューターにインターネット インフォメーション サーバー (IIS) に ASP.NET web アプリケーションをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-108">In this tutorial, you'll deploy an ASP.NET web application to Internet Information Server (IIS) on your local computer.</span></span>

<span data-ttu-id="a1fc9-109">一般にアプリケーションを開発するときにを実行し、Visual Studio でテストします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-109">Generally when you develop an application, you run it and test it in Visual Studio.</span></span> <span data-ttu-id="a1fc9-110">既定では、Visual Studio 2017 での web アプリケーション プロジェクトを使用して、IIS Express 開発 web サーバーとします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-110">By default, web application projects in Visual Studio 2017 use IIS Express as the development web server.</span></span> <span data-ttu-id="a1fc9-111">Visual Studio 開発サーバー (Cassini とも呼ばれます)、既定で Visual Studio 2017 を使用するよりも完全な IIS のように、IIS Express がよりは動作します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-111">IIS Express behaves more like full IIS than the Visual Studio Development Server (also known as Cassini), which Visual Studio 2017 uses by default.</span></span> <span data-ttu-id="a1fc9-112">どちらも開発 web サーバーが IIS とまったく同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-112">But neither development web server works exactly like IIS.</span></span> <span data-ttu-id="a1fc9-113">その結果、実行しますが、Visual Studio で正しくテストしてとを IIS に配置されるときに失敗するアプリがでしたとします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-113">Consequently, an app could run and test correctly in Visual Studio but fail when it's deployed to IIS.</span></span>

<span data-ttu-id="a1fc9-114">2 つの方法でアプリケーションを確実にテストできます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-114">You can reliably test your application in two ways:</span></span>

1. <span data-ttu-id="a1fc9-115">後で、運用環境にデプロイに使用するのと同じプロセスを使用して開発用コンピューターに IIS にアプリケーションをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-115">Deploy your application to IIS on your development computer using the same process that you'll use later to deploy it to your production environment.</span></span>

   <span data-ttu-id="a1fc9-116">Web プロジェクトを実行するが、デプロイ プロセスをテストするはときに、IIS を使用する Visual Studio を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-116">You can configure Visual Studio to use IIS when you run a web project, but that wouldn't test your deployment process.</span></span> <span data-ttu-id="a1fc9-117">このメソッドは、デプロイ プロセスを検証し、IIS 下で、アプリケーションが正常に実行します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-117">This method validates your deployment process and that your application runs correctly under IIS.</span></span>

2. <span data-ttu-id="a1fc9-118">運用環境のようなテスト環境にアプリケーションをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-118">Deploy your application to a test environment similar to your production environment.</span></span> 
 
   <span data-ttu-id="a1fc9-119">これらのチュートリアルの実稼働環境では、Azure App Service で Web アプリが。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-119">The production environment for these tutorials is Web Apps in Azure App Service.</span></span> <span data-ttu-id="a1fc9-120">理想的なテスト環境では、Azure サービスで作成された追加の web アプリです。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-120">The ideal test environment is an additional web app created in the Azure Service.</span></span> <span data-ttu-id="a1fc9-121">運用 web アプリと同様の設定は、テストに対してのみ使用が。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-121">Though it would be set up the same way as a production web app, you would only use it for testing.</span></span>

<span data-ttu-id="a1fc9-122">オプション 2 は、テストする最も信頼性の高い方法です。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-122">Option 2 is the most reliable way to test.</span></span> <span data-ttu-id="a1fc9-123">オプション 2 を使用する場合は、オプション 1 を使用する必要はありませんとは限りません。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-123">If you use option 2, you don't necessarily need to use option 1.</span></span> <span data-ttu-id="a1fc9-124">ただし、サードパーティにデプロイする場合、ホスティング プロバイダー、オプション 2 できない場合もありますか、または、高価なため、このチュートリアル シリーズは、両方の方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-124">However if you're deploying to a third-party hosting provider, option 2 might not be feasible or might be expensive, so this tutorial series shows both methods.</span></span> <span data-ttu-id="a1fc9-125">オプション 2 のガイダンスがで提供される、[実稼働環境に展開する](deploying-to-production.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-125">Guidance for option 2 is provided in the [Deploying to the Production Environment](deploying-to-production.md) tutorial.</span></span>

<span data-ttu-id="a1fc9-126">Visual Studio で web サーバーの使用に関する詳細については、次を参照してください。 [ASP.NET Web プロジェクト用の Visual Studio で Web サーバー](https://msdn.microsoft.com/library/58wxa9w5.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-126">For more information about using web servers in Visual Studio, see [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx).</span></span>

<span data-ttu-id="a1fc9-127">リマインダー:エラー メッセージが表示される、または、チュートリアルを進めるときに機能しない、するを確認してください、[トラブルシューティング ページ](troubleshooting.md)します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-127">Reminder: If you receive an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="download-the-contoso-university-starter-project"></a><span data-ttu-id="a1fc9-128">Contoso University のスタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-128">Download the Contoso University starter project</span></span>

<span data-ttu-id="a1fc9-129">ダウンロードし、Contoso University の Visual Studio スタート ソリューションとプロジェクトをインストールします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-129">Download and install the Contoso University Visual Studio starter solution and project.</span></span> <span data-ttu-id="a1fc9-130">このソリューションには、完了したチュートリアルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-130">This solution contains the completed tutorial.</span></span> 

[<span data-ttu-id="a1fc9-131">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-131">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

## <a name="install-iis"></a><span data-ttu-id="a1fc9-132">IIS をインストールします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-132">Install IIS</span></span>

<span data-ttu-id="a1fc9-133">開発用コンピューターに IIS に展開するには、IIS および Web Deploy がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-133">To deploy to IIS on your development computer, confirm that IIS and Web Deploy are installed.</span></span> <span data-ttu-id="a1fc9-134">既定では、Visual Studio では、Web Deploy がインストールされますが、IIS が既定の Windows 10、Windows 8、または Windows 7 の構成に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-134">By default, Visual Studio installs Web Deploy, but IIS isn't included in the default Windows 10, Windows 8, or Windows 7 configuration.</span></span> <span data-ttu-id="a1fc9-135">場合は IIS をインストールしてあると、既定のアプリケーション プールが既に .NET 4 に設定されて、 [、次のセクション](#sqlexpress)します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-135">If you've already installed IIS and the default application pool is already set to .NET 4, skip to [the next section](#sqlexpress).</span></span>

1. <span data-ttu-id="a1fc9-136">使用することをお勧め、 [Web プラットフォーム インストーラー (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) IIS と Web Deploy をインストールします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-136">It's recommended you use the [Web Platform Installer (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) to install IIS and Web Deploy.</span></span> <span data-ttu-id="a1fc9-137">WPI では、必要な場合は、IIS と Web デプロイの前提条件を含む、推奨される IIS 構成をインストールします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-137">WPI installs a recommended IIS configuration that includes IIS and Web Deploy prerequisites if necessary.</span></span>

   <span data-ttu-id="a1fc9-138">IIS、Web デプロイ、または、必要なコンポーネントのいずれかをインストールしてある場合は、不足しているだけ、WPI がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-138">If you've already installed IIS, Web Deploy, or any of their required components, the WPI installs only what is missing.</span></span>

   * <span data-ttu-id="a1fc9-139">IIS と Web Deploy をインストールするのにには、Web Platform Installer を使用します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-139">Use the Web Platform Installer to install IIS and Web Deploy:</span></span>
    
     ![WPI を使用して、IIS をインストールします。](deploying-to-iis/_static/image30.png)

     ![Web 配置 WPI を使用してをインストールします。](deploying-to-iis/_static/image31.png)

     <span data-ttu-id="a1fc9-142">IIS 7 をインストールすることを示すメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-142">You'll see messages indicating that IIS 7 will be installed.</span></span> <span data-ttu-id="a1fc9-143">リンクが Windows 8 で IIS 8 の機能します。ただし、Windows 8 以降では ASP.NET 4.7 がインストールされていることを確認する次の手順を進めます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-143">The link works for IIS 8 in Windows 8; but for Windows 8 and later, go through the following steps to make sure that ASP.NET 4.7 is installed:</span></span>

   * <span data-ttu-id="a1fc9-144">開いている\*\*コントロール パネルの \*\* > **プログラム** > **プログラムと機能** > **オンまたはオフにする Windows の機能**.</span><span class="sxs-lookup"><span data-stu-id="a1fc9-144">Open **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off**.</span></span>

   * <span data-ttu-id="a1fc9-145">展開**インターネット インフォメーション サービス**、 **World Wide Web サービス**、および**アプリケーション開発機能**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-145">Expand **Internet Information Services**, **World Wide Web Services**, and **Application Development Features**.</span></span>
   
   * <span data-ttu-id="a1fc9-146">確認します**ASP.NET 4.7**が選択されています。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-146">Confirm that **ASP.NET 4.7** is selected.</span></span>

     ![ASP.NET 4.7 を選択します。](deploying-to-iis/_static/image1a.png)

   * <span data-ttu-id="a1fc9-148">確認します**World Wide Web サービス**と**IIS 管理コンソール**が選択されています。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-148">Confirm that **World Wide Web Services** and **IIS Management Console** is selected.</span></span> <span data-ttu-id="a1fc9-149">これは、IIS および IIS マネージャーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-149">This installs IIS and IIS Manager.</span></span>
    
     ![World Wide Web サービスを選択します。](deploying-to-iis/_static/image24.png)    
  
   * <span data-ttu-id="a1fc9-151">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-151">Select **OK**.</span></span> <span data-ttu-id="a1fc9-152">インストールが実行を示すダイアログ ボックスのメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-152">Dialog box messages indicating installation is taking place appear.</span></span>

<span data-ttu-id="a1fc9-153">IIS をインストールすると、実行**IIS Manager** .NET Framework version 4 が既定のアプリケーション プールに割り当てられているかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-153">After installing IIS, run **IIS Manager** to make sure that the .NET Framework version 4 is assigned to the default application pool.</span></span>

1. <span data-ttu-id="a1fc9-154">開くには、WINDOWS + R キーを押して、**実行** ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-154">Press WINDOWS+R to open the **Run** dialog box.</span></span>

   <span data-ttu-id="a1fc9-155">(Windows 8 以降、"run"で入力、**開始**ページ。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-155">(On Windows 8 or later, enter "run" on the **Start** page.</span></span> <span data-ttu-id="a1fc9-156">Windows 7 では、次のように選択します。**実行**から、**開始**メニュー。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-156">In Windows 7, select **Run** from the **Start** menu.</span></span> <span data-ttu-id="a1fc9-157">場合**実行**に含まれていない、**開始** メニューの タスク バーを右クリックし、選択**プロパティ**を選択、 **スタート メニュー** の選択タブで、**カスタマイズ**、選び**コマンドを実行して**)。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-157">If **Run** isn't in the **Start** menu, right-click the taskbar, select **Properties**, select the **Start Menu** tab, select **Customize**, and select **Run command**.)</span></span>

2. <span data-ttu-id="a1fc9-158">"Inetmgr"を入力し、 **OK**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-158">Enter "inetmgr" and select **OK**.</span></span>

3. <span data-ttu-id="a1fc9-159">**接続**ウィンドウで、サーバー ノードを展開し、選択**アプリケーション プール**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-159">In the **Connections** pane, expand the server node and select **Application Pools**.</span></span> <span data-ttu-id="a1fc9-160">**アプリケーション プール**ウィンドウ場合**DefaultAppPool**を .NET framework version 4 を次の図のように割り当てられている、次のセクションに進んでください。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-160">In the **Application Pools** pane if **DefaultAppPool** is assigned to the .NET framework version 4 as in the following illustration, skip to the next section.</span></span>

   ![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image5a.png)

4. <span data-ttu-id="a1fc9-162">2 つだけのアプリケーション プールを参照してください、.NET Framework 2.0 に設定する場合は、IIS で ASP.NET 4 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-162">If you see only two application pools and both are set to .NET Framework 2.0, install ASP.NET 4 in IIS.</span></span>

   <span data-ttu-id="a1fc9-163">Windows 8 以降がインストールされていることを ASP.NET 4.7 を確認して、前のセクションの手順を参照してくださいまたはを参照してください[Windows 8 および Windows Server 2012 で ASP.NET 4.5 をインストールする方法](https://support.microsoft.com/kb/2736284)します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-163">For Windows 8 or later, see the previous section's instructions for making sure that ASP.NET 4.7 is installed or see [How to install ASP.NET 4.5 on Windows 8 and Windows Server 2012](https://support.microsoft.com/kb/2736284).</span></span> <span data-ttu-id="a1fc9-164">Windows 7 を右クリックし、コマンド プロンプト ウィンドウを開きます。**コマンド プロンプト**、Windows で**開始**メニュー**管理者として実行**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-164">For Windows 7, open a command prompt window by right-clicking **Command Prompt** in the Windows **Start** menu and selecting **Run as Administrator**.</span></span> <span data-ttu-id="a1fc9-165">実行[aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx)次のコマンドを使用して、IIS で ASP.NET 4 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-165">Run [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) to install ASP.NET 4 in IIS using the following commands.</span></span> <span data-ttu-id="a1fc9-166">(32 ビット システムでは、「フレームワーク」と"Framework64"を置き換えてください)。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-166">(In 32-bit systems, replace "Framework64" with "Framework".)</span></span>

   [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

   <span data-ttu-id="a1fc9-167">このコマンドは、新しいアプリケーション、.NET Framework 4 用のプールが既定のアプリケーション プールが残ります 2.0 に設定を作成します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-167">This command creates new application pools for the .NET Framework 4, but the default application pool will remain set to 2.0.</span></span> <span data-ttu-id="a1fc9-168">そのアプリケーション プールには、.NET 4 を対象が、.NET 4 に、アプリケーション プールをので変更すること、アプリケーションをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-168">You're deploying an application that targets .NET 4 to that application pool, so change the application pool to .NET 4.</span></span>

5. <span data-ttu-id="a1fc9-169">閉じた場合**IIS Manager**、もう一度実行、サーバー ノードを展開および選択**アプリケーション プール**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-169">If you closed **IIS Manager**, run it again, expand the server node, and select **Application Pools**.</span></span>

6. <span data-ttu-id="a1fc9-170">**アプリケーション プール**ペインで、 **DefaultAppPool**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-170">In the **Application Pools** pane, select **DefaultAppPool**.</span></span> <span data-ttu-id="a1fc9-171">**アクション**ペインで、**基本設定**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-171">In the **Actions** pane, select **Basic Settings**.</span></span>

   ![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image25.png)

7. <span data-ttu-id="a1fc9-173">**アプリケーション プールの編集** ダイアログ ボックスで、変更、 **.NET CLR バージョン**に **.NET CLR v4.0.30319**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-173">In the **Edit Application Pool** dialog box, change the **.NET CLR version** to **.NET CLR v4.0.30319**.</span></span> <span data-ttu-id="a1fc9-174">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-174">Select **OK**.</span></span>

   ![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image6a.png)

<span data-ttu-id="a1fc9-176">IIS に web アプリケーションを発行する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-176">You're now ready to publish a web application to IIS.</span></span> <span data-ttu-id="a1fc9-177">最初に、ただし、テスト用データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-177">First, however, create databases for testing.</span></span>

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a><span data-ttu-id="a1fc9-178">SQL Server Express をインストールします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-178">Install SQL Server Express</span></span>

<span data-ttu-id="a1fc9-179">LocalDB ので、テスト環境、SQL Server Express をインストールして、IIS で動作するものではありません。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-179">LocalDB isn't designed to work in IIS, so your test environment has to have SQL Server Express installed.</span></span> <span data-ttu-id="a1fc9-180">Visual Studio 2010 SQL Server Express を使用している場合は、既定では既にインストールされています。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-180">If you're using Visual Studio 2010 SQL Server Express, it's already installed by default.</span></span> <span data-ttu-id="a1fc9-181">Visual Studio 2012 またはそれ以降を使用している場合は、SQL Server Express をインストールします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-181">If you're using Visual Studio 2012 or later, install SQL Server Express.</span></span>

<span data-ttu-id="a1fc9-182">SQL Server Express をインストールするには、ダウンロードしてからインストール[ダウンロード センター。Microsoft SQL Server 2017 Express edition](https://www.microsoft.com/sql-server/sql-server-editions-express)します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-182">To install SQL Server Express, download and install it from [Download Center: Microsoft SQL Server 2017 Express edition](https://www.microsoft.com/sql-server/sql-server-editions-express).</span></span> 

<span data-ttu-id="a1fc9-183">SQL Server インストール センターの最初のページで次のように選択します。 **SQL Server の新規スタンドアロン インストールまたは既存のインストールに機能の追加**既定の選択を受け入れ、指示に従います。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-183">On the first page of the SQL Server Installation Center, select **New SQL Server stand-alone installation or add features to an existing installation** and follow the instructions accepting the default choices.</span></span> <span data-ttu-id="a1fc9-184">インストール ウィザードでは、既定の設定をそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-184">In the installation wizard, accept the default settings.</span></span> <span data-ttu-id="a1fc9-185">インストール オプションの詳細については、次を参照してください。[インストール ウィザード (セットアップ) からの SQL Server のインストール](https://msdn.microsoft.com/library/ms143219.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-185">For more information about installation options, see [Install SQL Server from the Installation Wizard (Setup)](https://msdn.microsoft.com/library/ms143219.aspx).</span></span>

## <a name="create-sql-server-express-databases-for-the-test-environment"></a><span data-ttu-id="a1fc9-186">テスト環境用の SQL Server Express データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-186">Create SQL Server Express databases for the test environment</span></span>

<span data-ttu-id="a1fc9-187">Contoso University アプリケーションには、2 つのデータベースがあります。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-187">The Contoso University application has two databases:</span></span> 

1. <span data-ttu-id="a1fc9-188">メンバーシップ データベース</span><span class="sxs-lookup"><span data-stu-id="a1fc9-188">Membership database</span></span> 
2. <span data-ttu-id="a1fc9-189">アプリケーション データベース</span><span class="sxs-lookup"><span data-stu-id="a1fc9-189">Application database</span></span> 

<span data-ttu-id="a1fc9-190">2 つの異なるデータベースまたは単一のデータベースには、これらのデータベースをデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-190">You can deploy these databases to two separate databases or to a single database.</span></span> <span data-ttu-id="a1fc9-191">それらを組み合わせることにより、簡単にそれらの間のデータベースの結合です。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-191">Combining them makes database joins between them easier.</span></span> 

<span data-ttu-id="a1fc9-192">サード パーティのホスティング プロバイダーにデプロイする場合、ホスティング プランがすることもそれらを結合する理由。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-192">If you're deploying to a third-party hosting provider, your hosting plan might also give you a reason to combine them.</span></span> <span data-ttu-id="a1fc9-193">たとえば、プロバイダーは、複数のデータベースの詳細は課金可能性があります。 または複数のデータベースをも許可しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-193">For example, the provider might charge more for multiple databases or might not even allow more than one database.</span></span>

<span data-ttu-id="a1fc9-194">このチュートリアルでは、テスト環境で 2 つのデータベースとステージングと運用環境で 1 つのデータベースをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-194">In this tutorial, you'll deploy to two databases in the test environment and to one database in the staging and production environments.</span></span>

<span data-ttu-id="a1fc9-195">**ビュー**  メニューの選択 Visual Studio で**サーバー エクスプ ローラー** (**データベース エクスプ ローラー** Visual Web Developer で)。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-195">From the **View** menu in Visual Studio, select **Server Explorer** (**Database Explorer** in Visual Web Developer).</span></span> <span data-ttu-id="a1fc9-196">右クリック**データ接続**選択**新しい SQL Server データベースの作成**です。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-196">Right-click **Data Connections** and select **Create New SQL Server Database**.</span></span>

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

<span data-ttu-id="a1fc9-198">**新しい SQL Server データベースの作成** ダイアログ ボックスに、入力". \SQLExpress"で、**サーバー名**ボックスと"aspnet ContosoUniversity"で、**新しいデータベース名**ボックス。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-198">In the **Create New SQL Server Database** dialog box, enter ".\SQLExpress" in the **Server name** box and "aspnet-ContosoUniversity" in the **New database name** box.</span></span> <span data-ttu-id="a1fc9-199">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-199">Select **OK**.</span></span>

![Aspnet ContosoUniversity を作成します。](deploying-to-iis/_static/image9.png)

<span data-ttu-id="a1fc9-201">という名前の新しい SQL Server Express の School データベースを作成する同じ手順に従って`ContosoUniversity`します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-201">Follow the same procedure to create a new SQL Server Express School database named `ContosoUniversity`.</span></span>

<span data-ttu-id="a1fc9-202">**サーバー エクスプ ローラー** 2 つの新しいデータベースを示しています。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-202">**Server Explorer** shows the two new databases.</span></span>

![サーバー エクスプ ローラーで新しいデータベース](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a><span data-ttu-id="a1fc9-204">新しいデータベースの許可スクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-204">Create a grant script for the new databases</span></span>

<span data-ttu-id="a1fc9-205">アプリケーションを開発用コンピューターに IIS で実行すると、アプリケーションは、データベースにアクセスするのに既定のアプリケーション プールの資格情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-205">When the application runs in IIS on your development computer, the application uses the default application pool's credentials to access the database.</span></span> <span data-ttu-id="a1fc9-206">ただし、既定では、アプリケーション プールでは、データベースを開くアクセス許可がありません。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-206">However, by default, the application pool doesn't have permission to open the databases.</span></span> <span data-ttu-id="a1fc9-207">これは、アクセス許可を付与するスクリプトを実行する必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-207">This means you need to run a script to grant that permission.</span></span> <span data-ttu-id="a1fc9-208">このセクションではそのスクリプトを作成し、アプリケーションは IIS で実行時に、データベースを開くことができるかどうかを確認するには、後で実行します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-208">In this section, you'll create that script and run it later to make sure that the application can open the databases when it runs in IIS.</span></span>

<span data-ttu-id="a1fc9-209">テキスト エディターで新しいファイルに次の SQL コマンドをコピーし、として保存*Grant.sql*します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-209">In a text editor, copy the following SQL commands into a new file and save it as *Grant.sql*.</span></span> 

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

<span data-ttu-id="a1fc9-210">Visual Studio で、Contoso University のソリューションを開きます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-210">In Visual Studio, open the Contoso University solution.</span></span> <span data-ttu-id="a1fc9-211">(いないプロジェクトの 1 つ、)、ソリューションを右クリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-211">Right-click the solution (not one of the projects), and select **Add**.</span></span> <span data-ttu-id="a1fc9-212">選択**既存項目の**を参照する*Grant.sql*、し、開きます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-212">Select **Existing Item**, browse to *Grant.sql*, and open it.</span></span>

> [!NOTE]
> <span data-ttu-id="a1fc9-213">このチュートリアルで指定されているとして Windows 10、Windows 8、または Windows 7 で IIS 設定とおよび SQL Server Express 2012 と連携して、またはそれ以降にこのスクリプトが設計います。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-213">This script is designed to work with SQL Server Express 2012 or later and with the IIS settings in Windows 10, Windows 8, or Windows 7 as they are specified in this tutorial.</span></span> <span data-ttu-id="a1fc9-214">別のバージョンの SQL Server または Windows を使用している場合、または IIS を設定するコンピューターに異なる場合は、このスクリプトへの変更が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-214">If you're using a different version of SQL Server or Windows, or if you set up IIS on your computer differently, changes to this script might be required.</span></span> <span data-ttu-id="a1fc9-215">SQL Server スクリプトの詳細については、次を参照してください。 [SQL Server オンライン ブックの「](https://go.microsoft.com/fwlink/?LinkId=132511)します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-215">For more information about SQL Server scripts, see [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).</span></span>

> [!NOTE] 
> <span data-ttu-id="a1fc9-216">**セキュリティに関する注意**このスクリプトは、 `db_owner` 、運用環境でがありますが、実行時にデータベースにアクセスするユーザーへのアクセス許可。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-216">**Security Note** This script gives `db_owner` permissions to the user that accesses the database at run time, which is what you'll have in the production environment.</span></span> <span data-ttu-id="a1fc9-217">一部のシナリオでは、展開に対してのみアクセス許可を更新し、実行時のデータを読み書きするのみのアクセス許可を持つ別のユーザーを指定します。 データベースの完全スキーマを持つユーザーを指定する場合があります。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-217">In some scenarios, you might want to specify a user that has full database schema update permissions only for deployment and specify for run time a different user that has permissions only to read and write data.</span></span> <span data-ttu-id="a1fc9-218">詳細については、次を参照してください。 [Code First Migrations に対する自動の Web.config の変更をレビュー](#reviewingmigrations)このチュートリアルで後述します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-218">For more information, see [Reviewing the Automatic Web.config Changes for Code First Migrations](#reviewingmigrations) later in this tutorial.</span></span>

<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a><span data-ttu-id="a1fc9-219">アプリケーションのデータベースで許可スクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-219">Run the grant script in the application database</span></span>

<span data-ttu-id="a1fc9-220">そのデータベースの配置を使用するためのデプロイ中に、メンバーシップ データベース内の許可スクリプトを実行する発行プロファイルを構成することができます、 [dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing)プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-220">You can configure the publish profile to run the grant script in the membership database during deployment because that database deployment uses the [dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing) provider.</span></span> <span data-ttu-id="a1fc9-221">アプリケーション データベースをデプロイする方法は、Code First Migrations の配置時にスクリプトを実行することはできません。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-221">You can't run scripts during Code First Migrations deployment, which is how you're deploying the application database.</span></span> <span data-ttu-id="a1fc9-222">つまり、アプリケーション データベースに展開する前にスクリプトを手動で実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-222">This means you have to manually run the script before deployment in the application database.</span></span>

1. <span data-ttu-id="a1fc9-223">Visual Studio で開く、 *Grant.sql*先ほど作成したファイル。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-223">In Visual Studio, open the *Grant.sql* file that you created earlier.</span></span>

2. <span data-ttu-id="a1fc9-224">選択**接続**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-224">Select **Connect**.</span></span> 

    ![[接続] ボタン](deploying-to-iis/_static/image11.png)

3. <span data-ttu-id="a1fc9-226">**サーバーへの接続** ダイアログ ボックスに、入力 *. \SQLExpress*として、**サーバー名**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-226">In the **Connect to Server** dialog box, enter *.\SQLExpress* as the **Server Name**.</span></span> <span data-ttu-id="a1fc9-227">選択**接続**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-227">Select **Connect**.</span></span>

4. <span data-ttu-id="a1fc9-228">データベースのドロップダウン リストで選択**ContosoUniversity**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-228">In the database drop-down list select **ContosoUniversity**.</span></span> <span data-ttu-id="a1fc9-229">選択**実行**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-229">Select **Execute**.</span></span> 

   ![](deploying-to-iis/_static/image12.png)

<span data-ttu-id="a1fc9-230">既定のアプリケーション プール id で、アプリケーションの実行時に、データベース テーブルを作成する Code First Migrations に対するアプリケーションのデータベースに十分なアクセス許可できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-230">The default application pool identity now has sufficient permissions in the application database for Code First Migrations to create the database tables when the application runs.</span></span>

## <a name="publish-to-iis"></a><span data-ttu-id="a1fc9-231">IIS に公開する</span><span class="sxs-lookup"><span data-stu-id="a1fc9-231">Publish to IIS</span></span>

<span data-ttu-id="a1fc9-232">Visual Studio と Web Deploy を使用して IIS に配置できるいくつかの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-232">There are several ways you can deploy to IIS using Visual Studio and Web Deploy:</span></span>

* <span data-ttu-id="a1fc9-233">Visual Studio のワンクリック発行を使用します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-233">Use Visual Studio one-click publish.</span></span>
* <span data-ttu-id="a1fc9-234">コマンドラインから発行します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-234">Publish from the command line.</span></span>
* <span data-ttu-id="a1fc9-235">展開パッケージを作成し、IIS マネージャーを使用してインストールします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-235">Create a deployment package and install it using IIS Manager.</span></span> <span data-ttu-id="a1fc9-236">パッケージには、すべてのファイルと IIS にサイトをインストールに必要なメタデータで .zip ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-236">The package has a .zip file with all the files and metadata required to install a site in IIS.</span></span>
* <span data-ttu-id="a1fc9-237">展開パッケージを作成し、コマンドラインを使用してインストールします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-237">Create a deployment package and install it using the command line.</span></span>

<span data-ttu-id="a1fc9-238">プロセス自動化する Visual Studio を設定する前のチュートリアルでデプロイ タスクは、これらのメソッドのすべてに適用されます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-238">The process you went through in the previous tutorials to set up Visual Studio to automate deployment tasks applies to all of these methods.</span></span> <span data-ttu-id="a1fc9-239">これらのチュートリアルでは、最初の 2 つのメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-239">In these tutorials, you'll use the first two methods.</span></span> <span data-ttu-id="a1fc9-240">展開パッケージの使用方法の詳細については、次を参照してください。[作成し、web 配置パッケージをインストールした web アプリケーションの配置](https://go.microsoft.com/fwlink/p/?LinkId=282413#package)for Visual Studio および ASP.NET Web 配置コンテンツ マップでします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-240">For information about using deployment packages, see [Deploying a web application by creating and installing a web deployment package](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) in the Web Deployment Content Map for Visual Studio and ASP.NET.</span></span>

<span data-ttu-id="a1fc9-241">発行前に、管理者モードで Visual Studio を実行していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-241">Before publishing, make sure that you're running Visual Studio in administrator mode.</span></span> <span data-ttu-id="a1fc9-242">表示されない場合 **(管理者)** タイトル バーで、Visual Studio を閉じます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-242">If you don't see **(Administrator)** in the title bar, close Visual Studio.</span></span> <span data-ttu-id="a1fc9-243">Windows 8 (またはそれ以降)**開始**ページまたは Windows 7**開始**] メニューの [Visual Studio アイコンを右クリックし、選択**管理者として実行**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-243">In the Windows 8 (or later) **Start** page or the Windows 7 **Start** menu, right-click the Visual Studio icon and select **Run as Administrator**.</span></span> <span data-ttu-id="a1fc9-244">管理者モードはローカル コンピューターの IIS に発行するときに公開するために必要です。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-244">Administrator mode is only required for publishing when you're publishing to IIS on the local computer.</span></span>

### <a name="create-the-publish-profile"></a><span data-ttu-id="a1fc9-245">発行プロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-245">Create the publish profile</span></span>

1. <span data-ttu-id="a1fc9-246">**ソリューション エクスプ ローラー**を右クリックし、 **ContosoUniversity**プロジェクト (いない、 **ContosoUniversity.DAL**プロジェクト)。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-246">In **Solution Explorer**, right-click the **ContosoUniversity** project (not the **ContosoUniversity.DAL** project).</span></span> <span data-ttu-id="a1fc9-247">**[発行]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-247">Select **Publish**.</span></span> <span data-ttu-id="a1fc9-248">**発行**ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-248">The **Publish** page appears.</span></span>

2. <span data-ttu-id="a1fc9-249">選択**新しいプロファイル**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-249">Select **New Profile**.</span></span> <span data-ttu-id="a1fc9-250">**発行先を選択** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-250">The **Pick a publish target** dialog box appears.</span></span>

3. <span data-ttu-id="a1fc9-251">選択**IIS、FTP など**します。**[プロファイルの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-251">Select **IIS, FTP, etc**. Select **Create Profile**.</span></span> <span data-ttu-id="a1fc9-252">**発行**ウィザードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-252">The **Publish** wizard appears.</span></span>

   ![発行ウィザード プロファイル タブ](deploying-to-iis/_static/image26.png)

4. <span data-ttu-id="a1fc9-254">**メソッドを公開**ドロップダウン メニューで、 **Web Deploy**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-254">From the **Publish method** drop-down menu, select **Web Deploy**.</span></span>

5. <span data-ttu-id="a1fc9-255">**Server**、入力*localhost*します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-255">For **Server**, enter *localhost*.</span></span>

6. <span data-ttu-id="a1fc9-256">**サイト名**、入力*既定の Web サイト/ContosoUniversity*します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-256">For **Site name**, enter *Default Web Site/ContosoUniversity*.</span></span>

7. <span data-ttu-id="a1fc9-257">**送信先 URL**、入力 *http://localhost/ContosoUniversity*します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-257">For **Destination URL**, enter *http://localhost/ContosoUniversity*.</span></span>

   <span data-ttu-id="a1fc9-258">**送信先 URL**設定は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-258">The **Destination URL** setting isn't required.</span></span> <span data-ttu-id="a1fc9-259">Visual Studio では、アプリケーションのデプロイが完了したら、この URL を既定のブラウザーが自動的に開きます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-259">When Visual Studio finishes deploying the application, it automatically opens your default browser to this URL.</span></span> <span data-ttu-id="a1fc9-260">展開した後に自動的に開くブラウザーをしたくない場合は、このボックスを空白のままにします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-260">If you don't want the browser to open automatically after deployment, leave this box blank.</span></span>

8. <span data-ttu-id="a1fc9-261">選択**接続の検証**設定が正しいことと、ローカル コンピューターの IIS に接続できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-261">Select **Validate Connection** to verify that the settings are correct and you can connect to IIS on the local computer.</span></span>

   <span data-ttu-id="a1fc9-262">緑色のチェック マークは、接続が成功したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-262">A green check mark verifies that the connection is successful.</span></span>

   ![公開 Web ウィザードの [接続] タブ](deploying-to-iis/_static/image27.png)

9. <span data-ttu-id="a1fc9-264">選択**次**に進めておく、**設定**タブ。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-264">Select **Next** to advance to the **Settings** tab.</span></span>

10. <span data-ttu-id="a1fc9-265">**構成** ボックスの一覧を展開するビルド構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-265">The **Configuration** drop-down box specifies the build configuration to deploy.</span></span> <span data-ttu-id="a1fc9-266">既定値のままに**リリース**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-266">Leave it set to the default value of **Release**.</span></span> <span data-ttu-id="a1fc9-267">このチュートリアルではデバッグ ビルドが配置されません。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-267">You won't be deploying Debug builds in this tutorial.</span></span>

11. <span data-ttu-id="a1fc9-268">展開**ファイル発行オプション**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-268">Expand **File Publish Options**.</span></span> <span data-ttu-id="a1fc9-269">選択**アプリからファイルを除外する\_データ フォルダー**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-269">Select **Exclude files from the App\_Data folder**.</span></span>

    <span data-ttu-id="a1fc9-270">テスト環境でアプリケーションにアクセスし、ローカル SQL Server Express インスタンスに .mdf ファイルではなくで作成したデータベース、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-270">In the test environment, the application accesses the databases that you created in the local SQL Server Express instance, not the .mdf files in the *App\_Data* folder.</span></span>

12. <span data-ttu-id="a1fc9-271">ままに、**発行中にプリコンパイル**と**転送先に追加のファイルを削除**チェック ボックスをオフします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-271">Leave the **Precompile during publishing** and **Remove additional files at destination** check boxes cleared.</span></span>

    ![[設定] タブでファイル発行オプション](deploying-to-iis/_static/image15a.png)

    <span data-ttu-id="a1fc9-273">プリコンパイルは、主に大規模なサイトのために役立つオプションです。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-273">Precompiling is an option that is useful mainly for large sites.</span></span> <span data-ttu-id="a1fc9-274">起動時間、サイトをパブリッシュした後、ページが要求された最初の時間短縮できることができます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-274">It can reduce startup time the first time a page is requested after the site is published.</span></span>

    <span data-ttu-id="a1fc9-275">これは、最初のデプロイがすべてのファイル変換先のフォルダーにまだ追加ファイルを削除する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-275">You don't need to remove additional files since this is your first deployment and there won't be any files in the destination folder yet.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a1fc9-276">選択した場合**転送先に追加のファイルを削除**の後続の配置が同じサイトに表示されるよう事前に展開する前に削除するファイルは、プレビュー機能を使用することを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-276">If you select **Remove additional files at destination** for a subsequent deployment to the same site, make sure that you use the preview feature so that you see in advance which files will be deleted before you deploy.</span></span> <span data-ttu-id="a1fc9-277">想定される動作は、Web 配置と、プロジェクトの削除が移行先サーバー上のファイルが削除されます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-277">The expected behavior is that Web Deploy will delete files on the destination server that you have deleted in your project.</span></span> <span data-ttu-id="a1fc9-278">ただし、元とコピー先フォルダー の下のフォルダー全体の構造体と比較されます。一部のシナリオで Web Deploy 可能性がありますファイルと削除を削除する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-278">However, the entire folder structure under the source and destination folders is compared; and in some scenarios, Web Deploy might delete files you don't want to delete.</span></span>
    > 
    > <span data-ttu-id="a1fc9-279">たとえば、ルート フォルダーにプロジェクトを配置するときに、サーバー上のサブフォルダーに、web アプリケーションをした場合、サブフォルダーは削除されます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-279">For example if you have a web application in a subfolder on the server when you deploy a project to the root folder, the subfolder will be deleted.</span></span> <span data-ttu-id="a1fc9-280">Contoso.com でメイン サイトの 1 つのプロジェクトとブログは contoso.com/blog の別のプロジェクトがあります。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-280">You might have one project for the main site at contoso.com and another project for a blog at contoso.com/blog.</span></span> <span data-ttu-id="a1fc9-281">ブログ アプリケーションがサブフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-281">The blog application is in a subfolder.</span></span> <span data-ttu-id="a1fc9-282">選択した場合**転送先に追加のファイルを削除**メイン サイトを展開すると、ブログ アプリケーションが削除されます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-282">If you select **Remove additional files at destination** when you deploy the main site, the blog application will be deleted.</span></span>
    > 
    > <span data-ttu-id="a1fc9-283">別の例では、アプリの\_データ フォルダーが予期せず削除可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-283">For another example, your App\_Data folder might get deleted unexpectedly.</span></span> <span data-ttu-id="a1fc9-284">SQL Server Compact などの特定のデータベースでは、アプリのデータベース ファイルを保存\_データ フォルダー。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-284">Certain databases such as SQL Server Compact store database files in the App\_Data folder.</span></span> <span data-ttu-id="a1fc9-285">初期のデプロイ後を選ぶことが、後続の配置でデータベース ファイルのコピーを保持したくない**を除外するアプリ\_データ**[パッケージ/web の発行] タブ。ある場合にその後**転送先に追加のファイルを削除**選択すると、データベース ファイルとアプリ\_次回に発行するときに、データ フォルダー自体が削除されます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-285">After the initial deployment, you don't want to keep copying the database files in subsequent deployments, so you select  **Exclude App\_Data** on the Package/Publish Web tab. After you do that if you have **Remove additional files at destination** selected, your database files and the App\_Data folder itself will be deleted the next time you publish.</span></span>

### <a name="configure-deployment-for-the-membership-database"></a><span data-ttu-id="a1fc9-286">メンバーシップ データベースの展開を構成します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-286">Configure deployment for the membership database</span></span>

<span data-ttu-id="a1fc9-287">次の手順に適用されます、 **DefaultConnection**  ダイアログ ボックスでのデータベース**データベース**セクション。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-287">The following steps apply to the **DefaultConnection** database in the dialog box's **Databases** section.</span></span>

1. <span data-ttu-id="a1fc9-288">**リモート接続文字列**ボックスに、SQL Server Express の新しいメンバーシップ データベースを指す次の接続文字列を入力します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-288">In the **Remote connection string** box, enter the following connection string that points to the new SQL Server Express membership database.</span></span>

   [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

   <span data-ttu-id="a1fc9-289">展開プロセスがデプロイした Web.config ファイルでこの接続文字列を格納**実行時にこの接続文字列を使用して**が選択されています。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-289">The deployment process puts this connection string in the deployed Web.config file because **Use this connection string at runtime** is selected.</span></span>

    <span data-ttu-id="a1fc9-290">接続文字列を取得することができますも**サーバー エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-290">You can also get the connection string from **Server Explorer**.</span></span> <span data-ttu-id="a1fc9-291">**サーバー エクスプ ローラー**、展開**データ接続**を選択し、  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity**データベースから、**プロパティ**ウィンドウのコピー、**接続文字列**値。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-291">In **Server Explorer**, expand **Data Connections** and select the **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** database, then from the **Properties** window copy the **Connection String** value.</span></span> <span data-ttu-id="a1fc9-292">接続文字列が削除できる追加設定を 1 つにある:`Pooling=False`します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-292">That connection string will have one additional setting that you can delete: `Pooling=False`.</span></span>

2. <span data-ttu-id="a1fc9-293">選択**Update database**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-293">Select **Update database**.</span></span>

   <span data-ttu-id="a1fc9-294">これにより、デプロイ中に転送先データベース内に作成するデータベースのスキーマです。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-294">This causes the database schema to be created in the destination database during deployment.</span></span> <span data-ttu-id="a1fc9-295">次の手順で実行する必要がある追加のスクリプトを指定する: 既定のアプリケーション プール、データを展開する 1 つのデータベース アクセスを許可する 1 つ。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-295">In next steps, you specify the additional scripts that you need to run: one to grant database access to the default application pool and one to deploy data.</span></span>

3. <span data-ttu-id="a1fc9-296">選択**データベース更新を構成する**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-296">Select **Configure database updates**.</span></span>
 
4. <span data-ttu-id="a1fc9-297">**データベース更新を構成する**ダイアログ ボックスで、 **SQL スクリプトの追加**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-297">In the **Configure Database Updates** dialog box, select **Add SQL Script**.</span></span> <span data-ttu-id="a1fc9-298">移動し、 *Grant.sql*ソリューション フォルダーに保存したスクリプト。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-298">Navigate to the *Grant.sql* script that you saved earlier in the solution folder.</span></span>

5. <span data-ttu-id="a1fc9-299">追加するプロセスを繰り返し、 *aspnet-データ-dev.sql*スクリプト。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-299">Repeat the process to add the *aspnet-data-dev.sql* script.</span></span>

   ![メンバーシップ データベースのデータベースの更新プログラムを構成します。](deploying-to-iis/_static/image16.png)

6. <span data-ttu-id="a1fc9-301">選択**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-301">Select **Close**.</span></span>

### <a name="configure-deployment-for-the-application-database"></a><span data-ttu-id="a1fc9-302">アプリケーション データベースの展開を構成します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-302">Configure deployment for the application database</span></span>

<span data-ttu-id="a1fc9-303">Visual Studio で、Entity Framework が検出した場合`DbContext`でエントリを作成しますが、クラス、**データベース**セクションを**Code First Migrations の実行**チェック ボックスの代わりに、 **データベースを更新する**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-303">When Visual Studio detects an Entity Framework `DbContext` class, it creates an entry in the **Databases** section that has an **Execute Code First Migrations** check box instead of an **Update Database** check box.</span></span> <span data-ttu-id="a1fc9-304">このチュートリアルでは、Code First Migrations のデプロイを指定して、そのチェック ボックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-304">For this tutorial, you'll use that check box to specify Code First Migrations deployment.</span></span>

<span data-ttu-id="a1fc9-305">一部のシナリオで使用している、`DbContext`データベースが移行ではなく dbDacFx プロバイダーを使用してデータベースを配置します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-305">In some scenarios, you might be using a `DbContext` database but you want to use the dbDacFx provider instead of Migrations to deploy the database.</span></span> <span data-ttu-id="a1fc9-306">その場合を参照してください[移行せず、Code First のデータベースをデプロイする方法でしょうか。](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) msdn ASP.NET Web 配置の faq。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-306">In that case, see [How do I deploy a Code First database without Migrations?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) in the ASP.NET Web Deployment FAQ on MSDN.</span></span>

<span data-ttu-id="a1fc9-307">次の手順に適用されます、 **SchoolContext**  ダイアログ ボックスでのデータベース**データベース**セクション。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-307">The following steps apply to the **SchoolContext** database in the dialog box's **Databases** section.</span></span>

1. <span data-ttu-id="a1fc9-308">**リモート接続文字列**ボックスに、新しい SQL Server Express アプリケーション データベースを指す次の接続文字列を入力します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-308">In the **Remote connection string** box, enter the following connection string that points to the new SQL Server Express application database.</span></span>

   [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

   <span data-ttu-id="a1fc9-309">展開プロセスがデプロイした Web.config ファイルでこの接続文字列を格納**実行時にこの接続文字列を使用して**が選択されています。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-309">The deployment process puts this connection string in the deployed Web.config file because **Use this connection string at runtime** is selected.</span></span>

   <span data-ttu-id="a1fc9-310">アプリケーション データベースの接続文字列を取得することができますも**サーバー エクスプ ローラー**と同様に、メンバーシップ データベースの接続文字列を取得しました。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-310">You can also get the application database connection string from **Server Explorer** in the same way you got the membership database connection string.</span></span>

2. <span data-ttu-id="a1fc9-311">選択**実行 Code First Migrations (アプリケーションの起動時に実行)** します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-311">Select **Execute Code First Migrations (runs on application start)**.</span></span>

   <span data-ttu-id="a1fc9-312">このオプションを指定する配置の Web.config ファイルを構成する展開プロセスでは、`MigrateDatabaseToLatestVersion`初期化子。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-312">This option causes the deployment process to configure the deployed Web.config file to specify the `MigrateDatabaseToLatestVersion` initializer.</span></span> <span data-ttu-id="a1fc9-313">この初期化子は、アプリケーションの展開後に最初に、データベースにアクセスするときに自動的にデータベースと最新バージョンに更新します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-313">This initializer automatically updates the database to the latest version when the application accesses the database for the first time after deployment.</span></span>

### <a name="configure-publish-profile-transforms"></a><span data-ttu-id="a1fc9-314">構成プロファイルの変換の発行</span><span class="sxs-lookup"><span data-stu-id="a1fc9-314">Configure publish profile transforms</span></span>

1. <span data-ttu-id="a1fc9-315">選択**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-315">Select **Close**.</span></span> <span data-ttu-id="a1fc9-316">選択**はい**されたら変更を保存するかどうか。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-316">Select **Yes** when you are asked if you want to save changes.</span></span>

2. <span data-ttu-id="a1fc9-317">**ソリューション エクスプ ローラー**、展開**プロパティ**、展開**PublishProfiles**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-317">In **Solution Explorer**, expand **Properties**, expand **PublishProfiles**.</span></span>

3. <span data-ttu-id="a1fc9-318">右クリック*CustomProfile.pubxml*名前を変更および*Test.pubxml*します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-318">Right-click *CustomProfile.pubxml* and rename it *Test.pubxml*.</span></span>

4. <span data-ttu-id="a1fc9-319">右クリックして*Test.pubxml*します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-319">Right-click *Test.pubxml*.</span></span> <span data-ttu-id="a1fc9-320">選択**Config 変換を追加**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-320">Select **Add Config Transform**.</span></span>

   ![Config 変換 メニューを追加します。](deploying-to-iis/_static/image17.png)

   <span data-ttu-id="a1fc9-322">Visual Studio によって作成、 *Web.Test.config*変換ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-322">Visual Studio creates the *Web.Test.config* transform file and opens it.</span></span>

5. <span data-ttu-id="a1fc9-323">*Web.Test.config*ファイルの変換を開始した直後に次のコードを挿入構成タグ。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-323">In the *Web.Test.config* transform file, insert the following code immediately after the opening configuration tag.</span></span>

   [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    <span data-ttu-id="a1fc9-324">テストを使用する場合、発行プロファイルのこの変換は、"Test"に環境のインジケーターを設定します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-324">When you use the Test publish profile, this transform sets the environment indicator to "Test".</span></span> <span data-ttu-id="a1fc9-325">デプロイされたサイトでは、"Contoso University"H1 見出し"(Test)"を確認します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-325">In the deployed site, you'll see "(Test)" after the "Contoso University" H1 heading.</span></span>

6. <span data-ttu-id="a1fc9-326">ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-326">Save and close the file.</span></span>

7. <span data-ttu-id="a1fc9-327">右クリックし、 *Web.Test.config*ファイルおよび選択**プレビュー変換**を組み込んだ変換が期待どおりの変更を生成するかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-327">Right-click the *Web.Test.config* file and select **Preview Transform** to make sure that the transform you coded produces the expected changes.</span></span>

    <span data-ttu-id="a1fc9-328">**Web.config プレビュー**ウィンドウには、両方を適用した結果が表示されます、 *Web.Release.config*変換と*Web.Test.config*を変換します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-328">The **Web.config Preview** window shows the result of applying both the *Web.Release.config* transforms and the *Web.Test.config* transforms.</span></span>

### <a name="preview-the-deployment-updates"></a><span data-ttu-id="a1fc9-329">展開の更新プログラムをプレビューします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-329">Preview the deployment updates</span></span>

1. <span data-ttu-id="a1fc9-330">開く、 **Web の発行**ウィザードをもう一度 (ContosoUniversity プロジェクトを右クリックして**発行**、し**プレビュー**)。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-330">Open the **Publish Web** wizard again (right-click the ContosoUniversity project, select **Publish**, then **Preview**).</span></span>

2. <span data-ttu-id="a1fc9-331">**プレビュー**ダイアログ ボックスで、**プレビューの開始**にコピーされるファイルの一覧を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-331">In the **Preview** dialog box, select **Start Preview** to see a list of the files that will be copied.</span></span> 

   ![発行のプレビュー](deploying-to-iis/_static/image29.png)

   <span data-ttu-id="a1fc9-333">選択することも、**プレビュー データベース**メンバーシップ データベースで実行されるスクリプトを表示するリンク。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-333">You can also select the **Preview database** link to see the scripts that will run in the membership database.</span></span> <span data-ttu-id="a1fc9-334">(スクリプトは実行されません Code First Migrations の展開のため、アプリケーション データベースをプレビューするものはありません。)</span><span class="sxs-lookup"><span data-stu-id="a1fc9-334">(No scripts are run for Code First Migrations deployment, so there's nothing to preview for the application database.)</span></span>

3. <span data-ttu-id="a1fc9-335">**[発行]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-335">Select **Publish**.</span></span>

   <span data-ttu-id="a1fc9-336">管理者モードで Visual Studio でない場合は、アクセス許可のエラー メッセージを取得可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-336">If Visual Studio is not in administrator mode, you might get a permissions error message.</span></span> <span data-ttu-id="a1fc9-337">その場合は、Visual Studio を閉じます、管理者モードで開くし、再度パブリッシュしてください。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-337">In that case, close Visual Studio, open it in administrator mode, and try to publish again.</span></span>

   <span data-ttu-id="a1fc9-338">Visual Studio が管理者モードである場合、**出力**ウィンドウのレポートが正常にビルドして発行します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-338">If Visual Studio is in administrator mode, the **Output** window reports successful build and publish.</span></span>

   ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

   <span data-ttu-id="a1fc9-340">内の URL を入力した場合、**送信先 URL**発行プロファイルでボックス**接続** タブで、ブラウザーは自動的にコンピューターに IIS で実行されている Contoso University のホーム ページが開きます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-340">If you entered the URL in the **Destination URL** box on the publish profile **Connection** tab, the browser automatically opens to the Contoso University Home page running in IIS on your computer.</span></span>

## <a name="test-in-the-test-environment"></a><span data-ttu-id="a1fc9-341">テスト環境でテストします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-341">Test in the test environment</span></span>

<span data-ttu-id="a1fc9-342">「(テスト)」の環境のインジケーターを示すことに注意してください。"(Dev)、"ではなくすることを示しています、 *Web.config*環境インジケーターの変換が成功しました。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-342">Notice that the environment indicator shows "(Test)" instead of "(Dev)," which shows that the *Web.config* transformation for the environment indicator was successful.</span></span>

<span data-ttu-id="a1fc9-343">実行、 **Instructors** Code First シード インストラクター データでデータベースを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-343">Run the **Instructors** page to verify that Code First seeded the database with instructor data.</span></span> <span data-ttu-id="a1fc9-344">Code First のデータベースを作成しを実行するため、読み込みに数分かかる場合がありますこのページを選択すると、`Seed`メソッド。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-344">When you select this page, it may take a few minutes to load because Code First creates the database and then runs the `Seed` method.</span></span> <span data-ttu-id="a1fc9-345">(しないをまだデータベースにアクセスするアプリケーションを試行していないので、ホーム ページで行ったときにします。)</span><span class="sxs-lookup"><span data-stu-id="a1fc9-345">(It didn't do that when you were on the home page because the application didn't try to access the database yet.)</span></span>

<span data-ttu-id="a1fc9-346">選択、**学生** タブに配置されているデータベースに学生がないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-346">Select the **Students** tab to verify that the deployed database has no students.</span></span>

<span data-ttu-id="a1fc9-347">選択**受講者の追加**から、**学生**メニュー。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-347">Select **Add Students** from the **Students** menu.</span></span> <span data-ttu-id="a1fc9-348">、生徒を追加し、新しい学生を表示、**学生**ページ。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-348">Add a student, and then view the new student in the **Students** page.</span></span> <span data-ttu-id="a1fc9-349">これは、データベースに正常に記述できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-349">This verifies that you can successfully write to the database.</span></span>

<span data-ttu-id="a1fc9-350">**コース**メニューの **更新クレジット**します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-350">From the **Courses** menu, select **Update Credits**.</span></span> <span data-ttu-id="a1fc9-351">**更新クレジット**ページには、管理者のアクセス許可が必要ですので、**ログで**ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-351">The **Update Credits** page requires administrator permissions, so the **Log In** page is displayed.</span></span> <span data-ttu-id="a1fc9-352">以前 ("admin"および"devpwd") で作成した管理者アカウントの資格情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-352">Enter the administrator account credentials that you created earlier ("admin" and "devpwd").</span></span> <span data-ttu-id="a1fc9-353">**更新クレジット**ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-353">The **Update Credits** page is displayed.</span></span> <span data-ttu-id="a1fc9-354">これは、前のチュートリアルで作成した管理者アカウントがテスト環境に正しくデプロイされたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-354">This verifies that the administrator account that you created in the previous tutorial was correctly deployed to the test environment.</span></span>

<span data-ttu-id="a1fc9-355">いることを確認、 *ELMAH*フォルダーに存在します、 *c:\inetpub\wwwroot\ContosoUniversity*フォルダー内のプレース ホルダー ファイルのみです。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-355">Verify that an *ELMAH* folder exists in the *c:\inetpub\wwwroot\ContosoUniversity* folder with only the placeholder file in it.</span></span>

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a><span data-ttu-id="a1fc9-356">Code First Migrations に対する自動の Web.config の変更を確認してください。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-356">Review the automatic Web.config changes for Code First Migrations</span></span>

<span data-ttu-id="a1fc9-357">開く、 *Web.config*にデプロイされたアプリケーション内でファイル*C:\inetpub\wwwroot\ContosoUniversity*場所に Code First Migrations の構成、展開プロセスに自動的に確認できます最新バージョンにデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-357">Open the *Web.config* file in the deployed application at *C:\inetpub\wwwroot\ContosoUniversity* and you can see where the deployment process configured Code First Migrations to automatically update the database to the latest version.</span></span>

![](deploying-to-iis/_static/image21.png)

<span data-ttu-id="a1fc9-358">展開プロセスには、データベース スキーマの更新専用に使用する Code First Migrations に対する新しい接続文字列も作成されます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-358">The deployment process also created a new connection string for Code First Migrations to use exclusively for updating the database schema:</span></span>

![Database_Publish connection string](deploying-to-iis/_static/image22.png)

<span data-ttu-id="a1fc9-360">この追加の接続文字列では、データベース スキーマの更新プログラムの 1 つのユーザー アカウントとアプリケーションのデータ アクセス用の別のユーザー アカウントを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-360">This additional connection string enables you to specify one user account for database schema updates and a different user account for application data access.</span></span> <span data-ttu-id="a1fc9-361">たとえば、割り当てたり、 **db\_所有者**Code First Migrations をロールと**db\_datareader**で**db\_datawriterの各**アプリケーションにロール。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-361">For example, you could assign the **db\_owner** role to Code First Migrations and **db\_datareader** with **db\_datawriter** roles to the application.</span></span> <span data-ttu-id="a1fc9-362">これは、多層防御の一般的なパターンを妨げる可能性のある悪意のあるコードからデータベース スキーマを変更するアプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-362">This is a common defense-in-depth pattern that prevents potentially malicious code in the application from changing the database schema.</span></span> <span data-ttu-id="a1fc9-363">(たとえば、これで行われる処理 SQL インジェクション攻撃が成功します。)これらのチュートリアルでは、このパターンを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-363">(For example, this might happen in a successful SQL injection attack.) These tutorials don't use this pattern.</span></span> <span data-ttu-id="a1fc9-364">自分のシナリオでこのパターンを実装するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-364">To implement this pattern in your scenario, take these steps:</span></span>

1. <span data-ttu-id="a1fc9-365">**Web の発行**ウィザード [、**設定**] タブで、データベースの完全スキーマの更新権限を持つユーザーを指定する接続文字列を入力します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-365">In the **Publish Web** wizard under the **Settings** tab, enter the connection string that specifies a user with full database schema update permissions.</span></span> <span data-ttu-id="a1fc9-366">クリア、**実行時にこの接続文字列を使用して**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-366">Clear the **Use this connection string at runtime** check box.</span></span> <span data-ttu-id="a1fc9-367">これは、配置の Web.config ファイルで、`DatabasePublish`接続文字列。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-367">In the deployed Web.config file, this becomes the `DatabasePublish` connection string.</span></span>

2. <span data-ttu-id="a1fc9-368">アプリケーションは、実行時に使用する接続文字列を Web.config ファイルの変換を作成します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-368">Create a Web.config file transformation for the connection string that you want the application to use at run time.</span></span>

## <a name="summary"></a><span data-ttu-id="a1fc9-369">まとめ</span><span class="sxs-lookup"><span data-stu-id="a1fc9-369">Summary</span></span>

<span data-ttu-id="a1fc9-370">開発用コンピューターに IIS にアプリケーションをデプロイし、存在テストしましたようになりました。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-370">You've now deployed your application to IIS on your development computer and tested it there.</span></span>

![テストでのホーム ページ](deploying-to-iis/_static/image23.png)

<span data-ttu-id="a1fc9-372">これは、展開プロセスを適切な場所 (配置しないファイルを除く)、アプリケーションのコンテンツをコピーしてもその Web Deploy IIS が正しく構成されて展開中にことを検証します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-372">This verifies that the deployment process copied the application's content to the right location (excluding the files that you did not want to deploy) and also that Web Deploy configured IIS correctly during deployment.</span></span> <span data-ttu-id="a1fc9-373">次のチュートリアルがまだ実行されている展開のタスクを検索する 1 つ以上のテストを実行します。 上のフォルダーのアクセス許可の設定、 *Elm ah*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-373">In the next tutorial, you'll run one more test that finds a deployment task that has not yet been done: setting folder permissions on the *Elm ah* folder.</span></span>

## <a name="more-information"></a><span data-ttu-id="a1fc9-374">詳細情報</span><span class="sxs-lookup"><span data-stu-id="a1fc9-374">More information</span></span>

<span data-ttu-id="a1fc9-375">Visual Studio での IIS または IIS Express の実行方法の詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-375">For information about running IIS or IIS Express in Visual Studio, see the following resources:</span></span>

- <span data-ttu-id="a1fc9-376">[IIS Express の概要](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net サイト。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-376">[IIS Express Overview](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) on the IIS.net site.</span></span>
- <span data-ttu-id="a1fc9-377">[IIS Express の概要](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx)Scott Guthrie のブログ。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-377">[Introducing IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) on Scott Guthrie's blog.</span></span>
- <span data-ttu-id="a1fc9-378">[Web では、ASP.NET Web プロジェクトを Visual Studio でサーバー](https://msdn.microsoft.com/library/58wxa9w5.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-378">[Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx).</span></span>
- <span data-ttu-id="a1fc9-379">[コアの相違点の間で IIS と ASP.NET 開発サーバー](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET サイト。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-379">[Core Differences Between IIS and the ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) on the ASP.NET site.</span></span>

<span data-ttu-id="a1fc9-380">どのような問題については、中程度の信頼でアプリケーションを実行するときに発生する可能性が、参照してください[中程度の信頼で ASP.NET アプリケーションをホストしている](http://www.4guysfromrolla.com/articles/100307-1.aspx)Rolla サイトから次の 4 つ Guys にします。</span><span class="sxs-lookup"><span data-stu-id="a1fc9-380">For information about what issues might arise when your application runs in medium trust, see [Hosting ASP.NET Applications in Medium Trust](http://www.4guysfromrolla.com/articles/100307-1.aspx) on the four Guys from Rolla site.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a1fc9-381">[前へ](project-properties.md)
> [次へ](setting-folder-permissions.md)</span><span class="sxs-lookup"><span data-stu-id="a1fc9-381">[Previous](project-properties.md)
[Next](setting-folder-permissions.md)</span></span>
