---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 8 - Code-Only 更新の展開 |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 6305a8cb87d9b00b6bb4c4f8fa6114bddc4eb89f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886915"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a><span data-ttu-id="de28c-103">SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 8 - Code-Only 更新の展開</span><span class="sxs-lookup"><span data-stu-id="de28c-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a Code-Only Update - 8 of 12</span></span>
====================
<span data-ttu-id="de28c-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="de28c-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="de28c-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="de28c-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="de28c-106">この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。</span><span class="sxs-lookup"><span data-stu-id="de28c-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="de28c-107">Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="de28c-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="de28c-108">系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。</span><span class="sxs-lookup"><span data-stu-id="de28c-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="de28c-109">チュートリアルについては、RC のリリースの Visual Studio 2012 以降の展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示していますし、Azure App Service Web アプリを展開する方法を示しています、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。</span><span class="sxs-lookup"><span data-stu-id="de28c-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="de28c-110">概要</span><span class="sxs-lookup"><span data-stu-id="de28c-110">Overview</span></span>

<span data-ttu-id="de28c-111">初期展開後の維持と、web サイトの開発作業を続行、および時間の長い前にする更新プログラムを展開します。</span><span class="sxs-lookup"><span data-stu-id="de28c-111">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="de28c-112">このチュートリアルでは、アプリケーション コードへの更新プログラムの展開のプロセスです。</span><span class="sxs-lookup"><span data-stu-id="de28c-112">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="de28c-113">この更新プログラムがデータベースの変更を含まない次のチュートリアルではデータベースの変更の展開の違いは何が表示されます。</span><span class="sxs-lookup"><span data-stu-id="de28c-113">This update does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="de28c-114">アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)です。</span><span class="sxs-lookup"><span data-stu-id="de28c-114">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="making-a-code-change"></a><span data-ttu-id="de28c-115">コードの変更</span><span class="sxs-lookup"><span data-stu-id="de28c-115">Making a Code Change</span></span>

<span data-ttu-id="de28c-116">単純な例として、更新プログラムをアプリケーションに、追加、**講習においてインストラクター**  ページで選択した講師コースの一覧です。</span><span class="sxs-lookup"><span data-stu-id="de28c-116">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="de28c-117">実行する場合、**講習においてインストラクター**  ページで、かがわかりますがある**選択**グリッド内のリンクを行わない make 以外の何か、行の背景が灰色に変わりますが、します。</span><span class="sxs-lookup"><span data-stu-id="de28c-117">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

<span data-ttu-id="de28c-118">[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="de28c-118">[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)</span></span>

<span data-ttu-id="de28c-119">これで、ときに実行されるコードを追加、**選択**リンクがクリックされ、選択した講師でコースの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="de28c-119">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

<span data-ttu-id="de28c-120">*Instructors.aspx*、次のマークアップを追加した直後に、 **ErrorMessageLabel** `Label`コントロール。</span><span class="sxs-lookup"><span data-stu-id="de28c-120">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

<span data-ttu-id="de28c-121">ページを実行し、インストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="de28c-121">Run the page and select an instructor.</span></span> <span data-ttu-id="de28c-122">そのインストラクターでコースの一覧を参照してください。</span><span class="sxs-lookup"><span data-stu-id="de28c-122">You see a list of courses taught by that instructor.</span></span>

<span data-ttu-id="de28c-123">[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="de28c-123">[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-code-update-to-the-test-environment"></a><span data-ttu-id="de28c-124">コードの更新をテスト環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="de28c-124">Deploying the Code Update to the Test Environment</span></span>

<span data-ttu-id="de28c-125">テスト環境に展開すると、実行中の 1 回のクリックの単純な処理をもう一度発行です。</span><span class="sxs-lookup"><span data-stu-id="de28c-125">Deploying to the test environment is a simple matter of running one-click publish again.</span></span> <span data-ttu-id="de28c-126">このプロセスを短縮するためには、使用することができます、 **Web 1 つをクリックして 発行**ツールバー。</span><span class="sxs-lookup"><span data-stu-id="de28c-126">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

<span data-ttu-id="de28c-127">**ビュー**  メニューの 選択**ツールバー**し、 **Web 1 つをクリックして 発行**です。</span><span class="sxs-lookup"><span data-stu-id="de28c-127">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

<span data-ttu-id="de28c-129">**ソリューション エクスプ ローラー**、ContosoUniversity プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="de28c-129">In **Solution Explorer**, select the ContosoUniversity project.</span></span>

<span data-ttu-id="de28c-130">**Web 1 つをクリックして 発行**ツールバーで、選択、**テスト**発行プロファイルをクリックして**Web の発行**(左向きおよび右向きの矢印のアイコン)。</span><span class="sxs-lookup"><span data-stu-id="de28c-130">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

<span data-ttu-id="de28c-132">Visual Studio 更新済みのアプリケーションを配置して、ブラウザーは、ホーム ページに自動的に開きます。</span><span class="sxs-lookup"><span data-stu-id="de28c-132">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span> <span data-ttu-id="de28c-133">講習においてインストラクター ページを実行し、更新プログラムが正常に展開されたことを確認するには、あるインストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="de28c-133">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="de28c-134">[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="de28c-134">[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)</span></span>

<span data-ttu-id="de28c-135">通常どおりも回帰テストを行う (つまり、新しい変更で、既存の機能が損なわれていないことを確認するサイトの残りの部分をテストする)。</span><span class="sxs-lookup"><span data-stu-id="de28c-135">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="de28c-136">このチュートリアルでをその手順をスキップし、実稼働環境に、更新プログラムの展開を続行します。</span><span class="sxs-lookup"><span data-stu-id="de28c-136">But for this tutorial you'll skip that step and proceed to deploy the update to production.</span></span>

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a><span data-ttu-id="de28c-137">実稼働環境に初期データベースの状態の再展開を防止</span><span class="sxs-lookup"><span data-stu-id="de28c-137">Preventing Redeployment of the Initial Database State to Production</span></span>

<span data-ttu-id="de28c-138">実際のアプリケーションでユーザーが、最初の展開後に、実稼働サイトと対話およびデータベースには、ライブ データが挿入されます。</span><span class="sxs-lookup"><span data-stu-id="de28c-138">In a real application, users interact with your production site after your initial deployment, and the databases are populated with live data.</span></span> <span data-ttu-id="de28c-139">そのため、すべてのライブ データをワイプすると、初期状態で、メンバーシップ データベースを再配置したくないです。</span><span class="sxs-lookup"><span data-stu-id="de28c-139">Therefore, you don't want to redeploy the membership database in its initial state, which would wipe out all of the live data.</span></span> <span data-ttu-id="de28c-140">SQL Server Compact データベースが内のファイルであるため、*アプリ\_データ*フォルダー内のファイルを展開の設定を変更することによってこれを防ぐにする必要がある、*アプリ\_データ*フォルダー展開されません。</span><span class="sxs-lookup"><span data-stu-id="de28c-140">Since SQL Server Compact databases are files in the *App\_Data* folder, you have to prevent this by changing deployment settings so that files in the *App\_Data* folder aren't deployed.</span></span>

<span data-ttu-id="de28c-141">開く、**プロジェクト プロパティ**を選択して ContosoUniversity プロジェクト ウィンドウ、**パッケージ化/発行 Web**  タブ。確認して、**構成**ドロップ ダウン ボックスにはいずれかの**アクティブ (リリース)** または**リリース**選択すると、選択**アプリからファイルを除外する\_データ フォルダー**です。</span><span class="sxs-lookup"><span data-stu-id="de28c-141">Open the **Project Properties** window for the ContosoUniversity project, and select the **Package/Publish Web** tab. Make sure that the **Configuration** drop-down box has either **Active (Release)** or **Release** selected, select **Exclude files from the App\_Data folder**.</span></span>

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

<span data-ttu-id="de28c-143">デバッグ ビルド構成に対して同じ変更を行うことをお勧めは、後で、デバッグ ビルドを配置する場合に備えて: 変更**構成**に**デバッグ**し、**を除外します。アプリからファイル\_データ フォルダー**です。</span><span class="sxs-lookup"><span data-stu-id="de28c-143">In case you decide to deploy a debug build in the future, it's a good idea to make the same change for the Debug build configuration: change **Configuration** to **Debug** and then select **Exclude files from the App\_Data folder**.</span></span>

<span data-ttu-id="de28c-144">保存して閉じます、**パッケージ化/発行 Web**タブです。</span><span class="sxs-lookup"><span data-stu-id="de28c-144">Save and close the **Package/Publish Web** tab.</span></span>

> [!NOTE] 
> 
> [!IMPORTANT]
> <span data-ttu-id="de28c-145">いないことを確認してください**先で追加ファイルを削除する**発行プロファイルで選択します。</span><span class="sxs-lookup"><span data-stu-id="de28c-145">Make sure that you don't have **Remove additional files at destination** selected in your publish profiles.</span></span> <span data-ttu-id="de28c-146">アプリ内にあるデータベース オプションを選択した場合、展開プロセスは削除されます\_アプリケーションの展開済みのサイトとのデータが削除されます\_データ フォルダー自体です。</span><span class="sxs-lookup"><span data-stu-id="de28c-146">If you select that option, the deployment process will delete the databases that you have in App\_Data in the deployed site, and it will delete the App\_Data folder itself.</span></span>


## <a name="preventing-user-access-to-the-production-site-during-update"></a><span data-ttu-id="de28c-147">更新中に、実稼働サイトへのユーザー アクセスを防止</span><span class="sxs-lookup"><span data-stu-id="de28c-147">Preventing User Access to the Production Site During Update</span></span>

<span data-ttu-id="de28c-148">配置する変更は、単純な変更を単一のページを今すぐです。</span><span class="sxs-lookup"><span data-stu-id="de28c-148">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="de28c-149">規模の大きな変更を展開することもありますその場合は、サイトことができますと動作と異常展開が完了する前に、ユーザーがページを要求する場合。</span><span class="sxs-lookup"><span data-stu-id="de28c-149">But sometimes you deploy larger changes, and in that case the site can behave strangely if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="de28c-150">これを防ぐためには、使用することができます、*アプリ\_offline.htm*ファイル。</span><span class="sxs-lookup"><span data-stu-id="de28c-150">To prevent this, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="de28c-151">という名前のファイルを配置すると*アプリ\_offline.htm*アプリケーションのルート フォルダーに IIS に自動的にファイルが表示されますそのアプリケーションを実行する代わりにします。</span><span class="sxs-lookup"><span data-stu-id="de28c-151">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="de28c-152">アクセスを防ぐため、配置時に、配置するように*アプリ\_offline.htm*ルート フォルダーで、展開プロセスを実行し、削除*アプリ\_offline.htm*です。</span><span class="sxs-lookup"><span data-stu-id="de28c-152">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm*.</span></span>

<span data-ttu-id="de28c-153">**ソリューション エクスプ ローラー**(いないプロジェクトの 1 つ)、ソリューションを右クリックし、選択、**新しいソリューション フォルダー**です。</span><span class="sxs-lookup"><span data-stu-id="de28c-153">In **Solution Explorer**, right-click the solution (not one of the projects) and select **New Solution Folder**.</span></span>

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

<span data-ttu-id="de28c-155">名前のフォルダー *SolutionFiles*です。</span><span class="sxs-lookup"><span data-stu-id="de28c-155">Name the folder *SolutionFiles*.</span></span>

<span data-ttu-id="de28c-156">名前付き HTML ページを作成、新しいフォルダーに*アプリ\_offline.htm*です。</span><span class="sxs-lookup"><span data-stu-id="de28c-156">In the new folder create an HTML page named *app\_offline.htm*.</span></span> <span data-ttu-id="de28c-157">既存の内容を次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="de28c-157">Replace the existing contents with the following markup:</span></span>

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

<span data-ttu-id="de28c-158">コピーすることができます、*アプリ\_offline.htm*ファイルを FTP 接続を使用して、サイト、または**ファイル マネージャー**ホスティング プロバイダーのコントロール パネルの ユーティリティです。</span><span class="sxs-lookup"><span data-stu-id="de28c-158">You can copy the *app\_offline.htm* file to the site by using an FTP connection or the **File Manager** utility in the hosting provider's control panel.</span></span> <span data-ttu-id="de28c-159">このチュートリアルでは、使用するので、**ファイル マネージャー**です。</span><span class="sxs-lookup"><span data-stu-id="de28c-159">For this tutorial, you'll use the **File Manager**.</span></span>

<span data-ttu-id="de28c-160">コントロール パネルを開き、選択**ファイル マネージャー**で行ったよう、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="de28c-160">Open the control panel and select **File Manager** as you did in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="de28c-161">選択**contosouniversity.com**し**wwwroot** 、アプリケーションのルート フォルダーを取得し、をクリックする**アップロード**です。</span><span class="sxs-lookup"><span data-stu-id="de28c-161">Select **contosouniversity.com** and then **wwwroot** to get to your application's root folder, and then click **Upload**.</span></span>

<span data-ttu-id="de28c-162">[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="de28c-162">[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)</span></span>

<span data-ttu-id="de28c-163">**ファイルのアップロード**ダイアログ ボックスで、*アプリ\_offline.htm*ファイルし、をクリックして**アップロード**です。</span><span class="sxs-lookup"><span data-stu-id="de28c-163">In the **Upload File** dialog box, select the *app\_offline.htm* file and then click **Upload**.</span></span>

<span data-ttu-id="de28c-164">[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="de28c-164">[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)</span></span>

<span data-ttu-id="de28c-165">サイトの URL を参照します。</span><span class="sxs-lookup"><span data-stu-id="de28c-165">Browse to your site's URL.</span></span> <span data-ttu-id="de28c-166">参照してください、*アプリ\_offline.htm*ホーム ページではなくページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="de28c-166">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

<span data-ttu-id="de28c-167">[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="de28c-167">[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)</span></span>

<span data-ttu-id="de28c-168">実稼働環境に展開する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="de28c-168">You are now ready to deploy to production.</span></span>

## <a name="deploying-the-code-update-to-the-production-environment"></a><span data-ttu-id="de28c-169">コードの更新を実稼働環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="de28c-169">Deploying the Code Update to the Production Environment</span></span>

<span data-ttu-id="de28c-170">**Web 1 つをクリックして 発行**ツールバーで、選択、**運用**発行プロファイルをクリックして**Web の発行**です。</span><span class="sxs-lookup"><span data-stu-id="de28c-170">In the **Web One Click Publish** toolbar, choose the **Production** publish profile and then click **Publish Web**.</span></span>

<span data-ttu-id="de28c-171">Visual Studio では、更新されたアプリケーションを展開し、サイトのホーム ページをブラウザーが開きます。</span><span class="sxs-lookup"><span data-stu-id="de28c-171">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="de28c-172">*アプリ\_offline.htm*ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="de28c-172">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="de28c-173">展開の成功を確認するテストすることができます、前に削除する必要があります、*アプリ\_offline.htm*ファイル。</span><span class="sxs-lookup"><span data-stu-id="de28c-173">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>

<span data-ttu-id="de28c-174">戻り、**ファイル マネージャー**コントロール パネルの アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="de28c-174">Return to the **File Manager** application in the control panel.</span></span> <span data-ttu-id="de28c-175">選択**contosouniversity.com**と**wwwroot**を選択**アプリ\_offline.htm**、クリックして**削除**です。</span><span class="sxs-lookup"><span data-stu-id="de28c-175">Select **contosouniversity.com** and **wwwroot**, select **app\_offline.htm**, and then click **Delete**.</span></span>

<span data-ttu-id="de28c-176">[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="de28c-176">[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)</span></span>

<span data-ttu-id="de28c-177">ブラウザーで、パブリックのサイトで講習においてインストラクター ページを開き、更新プログラムが正常に展開されたことを確認するには、あるインストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="de28c-177">In the browser, open the Instructors page in the public site, and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="de28c-178">[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="de28c-178">[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)</span></span>

<span data-ttu-id="de28c-179">データベースの変更は含まれませんが、アプリケーションの更新プログラムを展開したようになりました。</span><span class="sxs-lookup"><span data-stu-id="de28c-179">You've now deployed an application update that did not involve a database change.</span></span> <span data-ttu-id="de28c-180">次のチュートリアルでは、データベースの変更を配置する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="de28c-180">The next tutorial shows you how to deploy a database change.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="de28c-181">[前へ](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [次へ](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="de28c-181">[Previous](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
[Next](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)</span></span>
