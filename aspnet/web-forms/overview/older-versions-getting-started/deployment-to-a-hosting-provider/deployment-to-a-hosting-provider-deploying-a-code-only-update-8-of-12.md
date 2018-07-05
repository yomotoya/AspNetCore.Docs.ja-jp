---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: Code-Only 更新 - 12 の 8 の展開 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 39075d2e979076f88200ea595c92f95f38f35911
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374014"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a><span data-ttu-id="e405b-103">SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: Code-Only 更新 - 12 の 8 の展開</span><span class="sxs-lookup"><span data-stu-id="e405b-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a Code-Only Update - 8 of 12</span></span>
====================
<span data-ttu-id="e405b-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e405b-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="e405b-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="e405b-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="e405b-106">この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e405b-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="e405b-107">Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="e405b-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="e405b-108">シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。</span><span class="sxs-lookup"><span data-stu-id="e405b-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="e405b-109">Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Azure App Service Web Apps にデプロイする方法を示していますチュートリアルでは、次を参照してください。 [ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="e405b-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="e405b-110">概要</span><span class="sxs-lookup"><span data-stu-id="e405b-110">Overview</span></span>

<span data-ttu-id="e405b-111">初期のデプロイ後の保守および web サイトの開発作業が引き続き発生してまで長い間は、更新プログラムを展開します。</span><span class="sxs-lookup"><span data-stu-id="e405b-111">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="e405b-112">このチュートリアルでは、アプリケーション コードへの更新プログラムの展開のプロセス説明。</span><span class="sxs-lookup"><span data-stu-id="e405b-112">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="e405b-113">この更新プログラムがデータベースの変更が含まれない次のチュートリアルでデータベースの変更の展開に関する相違点を確認します。</span><span class="sxs-lookup"><span data-stu-id="e405b-113">This update does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="e405b-114">リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)します。</span><span class="sxs-lookup"><span data-stu-id="e405b-114">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="making-a-code-change"></a><span data-ttu-id="e405b-115">コードの変更</span><span class="sxs-lookup"><span data-stu-id="e405b-115">Making a Code Change</span></span>

<span data-ttu-id="e405b-116">アプリケーションへの更新プログラムの簡単な例とに追加します、 **Instructors**ページ、選択したインストラクターが指導するコースのリスト。</span><span class="sxs-lookup"><span data-stu-id="e405b-116">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="e405b-117">実行する場合、 **Instructors**ページが表示されます**選択**グリッド内のリンクが何もしないこと以外、行の背景が灰色に変わりますが、します。</span><span class="sxs-lookup"><span data-stu-id="e405b-117">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

<span data-ttu-id="e405b-118">[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e405b-118">[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)</span></span>

<span data-ttu-id="e405b-119">追加するときに実行するコード、**選択**リンクがクリックされ、選択したインストラクター指導するコースの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="e405b-119">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

<span data-ttu-id="e405b-120">*Instructors.aspx*、次のマークアップを追加した直後に、 **ErrorMessageLabel** `Label`コントロール。</span><span class="sxs-lookup"><span data-stu-id="e405b-120">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

<span data-ttu-id="e405b-121">ページを実行し、インストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="e405b-121">Run the page and select an instructor.</span></span> <span data-ttu-id="e405b-122">そのインストラクターが指導するコースのリストを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e405b-122">You see a list of courses taught by that instructor.</span></span>

<span data-ttu-id="e405b-123">[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e405b-123">[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-code-update-to-the-test-environment"></a><span data-ttu-id="e405b-124">コードの更新をテスト環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="e405b-124">Deploying the Code Update to the Test Environment</span></span>

<span data-ttu-id="e405b-125">テスト環境に展開するが、実行 1 回のクリックの簡単な処理をもう一度発行します。</span><span class="sxs-lookup"><span data-stu-id="e405b-125">Deploying to the test environment is a simple matter of running one-click publish again.</span></span> <span data-ttu-id="e405b-126">このプロセスを短縮するためには、使用することができます、 **Web の 1 クリックして発行**ツールバー。</span><span class="sxs-lookup"><span data-stu-id="e405b-126">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

<span data-ttu-id="e405b-127">**ビュー** ] メニューの [選択**ツールバー**選び**Web の 1 クリックして発行**します。</span><span class="sxs-lookup"><span data-stu-id="e405b-127">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

<span data-ttu-id="e405b-129">**ソリューション エクスプ ローラー**、ContosoUniversity プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="e405b-129">In **Solution Explorer**, select the ContosoUniversity project.</span></span>

<span data-ttu-id="e405b-130">**Web の 1 クリックして発行**ツールバーで、選択、**テスト**発行プロファイルをクリックして**Web の発行**(左向きおよび右向きの矢印の付いたアイコン)。</span><span class="sxs-lookup"><span data-stu-id="e405b-130">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

<span data-ttu-id="e405b-132">Visual Studio が更新されたアプリケーションを配置し、ブラウザーはホーム ページに自動的に開きます。</span><span class="sxs-lookup"><span data-stu-id="e405b-132">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span> <span data-ttu-id="e405b-133">Instructors ページを実行し、更新プログラムが正常にデプロイされたことを確認するインストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="e405b-133">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="e405b-134">[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="e405b-134">[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)</span></span>

<span data-ttu-id="e405b-135">通常どおり回帰テストを行うことも (つまり、新しい変更が、既存の機能を中断していないことを確認するには、サイトの残りの部分をテストする)。</span><span class="sxs-lookup"><span data-stu-id="e405b-135">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="e405b-136">ただし、このチュートリアルではその手順をスキップし、更新プログラムを運用環境に配置を続行します。</span><span class="sxs-lookup"><span data-stu-id="e405b-136">But for this tutorial you'll skip that step and proceed to deploy the update to production.</span></span>

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a><span data-ttu-id="e405b-137">運用環境への初期データベースの状態の再デプロイを防止</span><span class="sxs-lookup"><span data-stu-id="e405b-137">Preventing Redeployment of the Initial Database State to Production</span></span>

<span data-ttu-id="e405b-138">実際のアプリケーションで、初期のデプロイ後、ユーザーが、実稼働サイトと対話し、データベースには、ライブ データが設定されます。</span><span class="sxs-lookup"><span data-stu-id="e405b-138">In a real application, users interact with your production site after your initial deployment, and the databases are populated with live data.</span></span> <span data-ttu-id="e405b-139">そのため、すべてのライブ データをワイプすると、初期の状態のメンバーシップ データベースを再デプロイしたくないです。</span><span class="sxs-lookup"><span data-stu-id="e405b-139">Therefore, you don't want to redeploy the membership database in its initial state, which would wipe out all of the live data.</span></span> <span data-ttu-id="e405b-140">SQL Server Compact データベースはファイルであるため、*アプリ\_データ*フォルダー内のファイルを展開の設定を変更することでこれを回避する必要がある、*アプリ\_データ*フォルダーデプロイはありません。</span><span class="sxs-lookup"><span data-stu-id="e405b-140">Since SQL Server Compact databases are files in the *App\_Data* folder, you have to prevent this by changing deployment settings so that files in the *App\_Data* folder aren't deployed.</span></span>

<span data-ttu-id="e405b-141">開く、**プロジェクトのプロパティ**ContosoUniversity プロジェクト、および選択のウィンドウ、**パッケージ化/発行 Web**タブ。確認、**構成**ドロップダウン ボックスに**アクティブ (リリース)** または**リリース**選択選択すると、 **アプリからファイルを除外する\_データ フォルダー**します。</span><span class="sxs-lookup"><span data-stu-id="e405b-141">Open the **Project Properties** window for the ContosoUniversity project, and select the **Package/Publish Web** tab. Make sure that the **Configuration** drop-down box has either **Active (Release)** or **Release** selected, select **Exclude files from the App\_Data folder**.</span></span>

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

<span data-ttu-id="e405b-143">デバッグ ビルドを今後の展開を決定する場合は、デバッグ ビルド構成の同じ変更を加えることをお勧め: 変更**構成**に**デバッグ**選び**を除外します。アプリからファイル\_データ フォルダー**します。</span><span class="sxs-lookup"><span data-stu-id="e405b-143">In case you decide to deploy a debug build in the future, it's a good idea to make the same change for the Debug build configuration: change **Configuration** to **Debug** and then select **Exclude files from the App\_Data folder**.</span></span>

<span data-ttu-id="e405b-144">保存して閉じます、**パッケージ化/発行 Web**タブ。</span><span class="sxs-lookup"><span data-stu-id="e405b-144">Save and close the **Package/Publish Web** tab.</span></span>

> [!NOTE] 
> 
> [!IMPORTANT]
> <span data-ttu-id="e405b-145">ないことを確認**転送先に追加のファイルを削除**発行プロファイルで選択されています。</span><span class="sxs-lookup"><span data-stu-id="e405b-145">Make sure that you don't have **Remove additional files at destination** selected in your publish profiles.</span></span> <span data-ttu-id="e405b-146">展開プロセスがアプリ内にあるデータベースを削除するオプションを選択した場合\_でデプロイされたサイトとそのデータがアプリを削除\_データ フォルダー自体。</span><span class="sxs-lookup"><span data-stu-id="e405b-146">If you select that option, the deployment process will delete the databases that you have in App\_Data in the deployed site, and it will delete the App\_Data folder itself.</span></span>


## <a name="preventing-user-access-to-the-production-site-during-update"></a><span data-ttu-id="e405b-147">更新中に、実稼働サイトへのユーザー アクセスを防止</span><span class="sxs-lookup"><span data-stu-id="e405b-147">Preventing User Access to the Production Site During Update</span></span>

<span data-ttu-id="e405b-148">デプロイする変更は、1 つのページを簡単に変更を今すぐです。</span><span class="sxs-lookup"><span data-stu-id="e405b-148">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="e405b-149">大規模な変更を展開することがあります、その場合は、サイトことができます動作妙デプロイが完了する前に、ユーザーがページを要求する場合。</span><span class="sxs-lookup"><span data-stu-id="e405b-149">But sometimes you deploy larger changes, and in that case the site can behave strangely if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="e405b-150">これを防ぐためには、使用することができます、*アプリ\_offline.htm*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e405b-150">To prevent this, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="e405b-151">という名前のファイルを配置すると*アプリ\_offline.htm*アプリケーションのルート フォルダーで IIS に自動的にファイルが表示されますそのアプリケーションを実行する代わりにします。</span><span class="sxs-lookup"><span data-stu-id="e405b-151">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="e405b-152">アクセスを防ぐため、デプロイ時に、配置するように*アプリ\_offline.htm* 、ルート フォルダーで、展開プロセスを実行し、削除*アプリ\_offline.htm*します。</span><span class="sxs-lookup"><span data-stu-id="e405b-152">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm*.</span></span>

<span data-ttu-id="e405b-153">**ソリューション エクスプ ローラー**(いない、プロジェクトの 1 つ) ソリューションを右クリックし、選択、**新しいソリューション フォルダー**します。</span><span class="sxs-lookup"><span data-stu-id="e405b-153">In **Solution Explorer**, right-click the solution (not one of the projects) and select **New Solution Folder**.</span></span>

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

<span data-ttu-id="e405b-155">フォルダーの名前*SolutionFiles*します。</span><span class="sxs-lookup"><span data-stu-id="e405b-155">Name the folder *SolutionFiles*.</span></span>

<span data-ttu-id="e405b-156">という名前の HTML ページを作成、新しいフォルダーに*アプリ\_offline.htm*します。</span><span class="sxs-lookup"><span data-stu-id="e405b-156">In the new folder create an HTML page named *app\_offline.htm*.</span></span> <span data-ttu-id="e405b-157">既存の内容を次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e405b-157">Replace the existing contents with the following markup:</span></span>

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

<span data-ttu-id="e405b-158">コピーすることができます、*アプリ\_offline.htm*ファイルを FTP 接続を使用して、サイト、または**ファイル マネージャー**ホスティング プロバイダーのコントロール パネルの ユーティリティ。</span><span class="sxs-lookup"><span data-stu-id="e405b-158">You can copy the *app\_offline.htm* file to the site by using an FTP connection or the **File Manager** utility in the hosting provider's control panel.</span></span> <span data-ttu-id="e405b-159">このチュートリアルで使用する、**ファイル マネージャー**します。</span><span class="sxs-lookup"><span data-stu-id="e405b-159">For this tutorial, you'll use the **File Manager**.</span></span>

<span data-ttu-id="e405b-160">コントロール パネルを開き、選択**ファイル マネージャー**で行ったように、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="e405b-160">Open the control panel and select **File Manager** as you did in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="e405b-161">選択**contosouniversity.com**し**wwwroot**をクリックして、アプリケーションのルート フォルダーに**アップロード**します。</span><span class="sxs-lookup"><span data-stu-id="e405b-161">Select **contosouniversity.com** and then **wwwroot** to get to your application's root folder, and then click **Upload**.</span></span>

<span data-ttu-id="e405b-162">[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="e405b-162">[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)</span></span>

<span data-ttu-id="e405b-163">**ファイルのアップロード**ダイアログ ボックスで、*アプリ\_offline.htm*ファイルをクリックして**アップロード**します。</span><span class="sxs-lookup"><span data-stu-id="e405b-163">In the **Upload File** dialog box, select the *app\_offline.htm* file and then click **Upload**.</span></span>

<span data-ttu-id="e405b-164">[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="e405b-164">[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)</span></span>

<span data-ttu-id="e405b-165">サイトの URL に移動します。</span><span class="sxs-lookup"><span data-stu-id="e405b-165">Browse to your site's URL.</span></span> <span data-ttu-id="e405b-166">参照してください、*アプリ\_offline.htm*ホーム ページではなくページが表示されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="e405b-166">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

<span data-ttu-id="e405b-167">[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="e405b-167">[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)</span></span>

<span data-ttu-id="e405b-168">運用環境にデプロイする準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="e405b-168">You are now ready to deploy to production.</span></span>

## <a name="deploying-the-code-update-to-the-production-environment"></a><span data-ttu-id="e405b-169">運用環境にコードの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="e405b-169">Deploying the Code Update to the Production Environment</span></span>

<span data-ttu-id="e405b-170">**Web の 1 クリックして発行**ツールバーで、選択、**運用**発行プロファイルをクリックして**Web の発行**します。</span><span class="sxs-lookup"><span data-stu-id="e405b-170">In the **Web One Click Publish** toolbar, choose the **Production** publish profile and then click **Publish Web**.</span></span>

<span data-ttu-id="e405b-171">Visual Studio は、更新されたアプリケーションを展開し、サイトのホーム ページをブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="e405b-171">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="e405b-172">*アプリ\_offline.htm*ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e405b-172">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="e405b-173">削除する必要がありますを正常にデプロイを確認するテストする前に、*アプリ\_offline.htm*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e405b-173">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>

<span data-ttu-id="e405b-174">戻り、**ファイル マネージャー**コントロール パネルの アプリケーション。</span><span class="sxs-lookup"><span data-stu-id="e405b-174">Return to the **File Manager** application in the control panel.</span></span> <span data-ttu-id="e405b-175">選択**contosouniversity.com**と**wwwroot**を選択します**アプリ\_offline.htm**、 をクリックし、**削除**します。</span><span class="sxs-lookup"><span data-stu-id="e405b-175">Select **contosouniversity.com** and **wwwroot**, select **app\_offline.htm**, and then click **Delete**.</span></span>

<span data-ttu-id="e405b-176">[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="e405b-176">[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)</span></span>

<span data-ttu-id="e405b-177">ブラウザーで、パブリックのサイトで、Instructors ページを開くし、更新プログラムが正常にデプロイされたことを確認する、インストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="e405b-177">In the browser, open the Instructors page in the public site, and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="e405b-178">[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="e405b-178">[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)</span></span>

<span data-ttu-id="e405b-179">これで、データベースの変更を使用してアプリケーションの更新プログラムをデプロイしました。</span><span class="sxs-lookup"><span data-stu-id="e405b-179">You've now deployed an application update that did not involve a database change.</span></span> <span data-ttu-id="e405b-180">次のチュートリアルでは、データベースの変更をデプロイする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e405b-180">The next tutorial shows you how to deploy a database change.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e405b-181">[前へ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [次へ](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="e405b-181">[Previous](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
[Next](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)</span></span>
