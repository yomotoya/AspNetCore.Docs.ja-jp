---
title: "Visual Studio を使用して Azure に ASP.NET Core アプリを発行する"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/01/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 0c0ec1c7c1408b0460c594a47a3e5738cd170d5f
ms.sourcegitcommit: d022d4b96795ee473fa3847a1d8a8c7430423a86
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="81117-103">Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する</span><span class="sxs-lookup"><span data-stu-id="81117-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="81117-104">[Rick Anderson](https://twitter.com/RickAndMSFT)、[Cesar Blum Silveira](https://github.com/cesarbs)、[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="81117-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="81117-105">開発環境を設定する</span><span class="sxs-lookup"><span data-stu-id="81117-105">Set up the development environment</span></span>

* <span data-ttu-id="81117-106">最新の [Azure SDK for Visual Studio](https://www.visualstudio.com/vs/azure-tools/) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="81117-106">Install the latest [Azure SDK for Visual Studio](https://www.visualstudio.com/vs/azure-tools/).</span></span> <span data-ttu-id="81117-107">Visual Studio をまだインストールしていない場合は、この SDK でインストールされます。</span><span class="sxs-lookup"><span data-stu-id="81117-107">The SDK installs Visual Studio if you don't already have it.</span></span>

* <span data-ttu-id="81117-108">[Azure アカウント](https://portal.azure.com/)を確認します。</span><span class="sxs-lookup"><span data-stu-id="81117-108">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="81117-109">[無料の Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/)か、[Visual Studio サブスクライバー向けの特典をアクティブ化する](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)ことができます。</span><span class="sxs-lookup"><span data-stu-id="81117-109">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="81117-110">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="81117-110">Create a web app</span></span>

<span data-ttu-id="81117-111">Visual Studio のスタート ページで、**[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![[ファイル] メニュー](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="81117-113">**[新しいプロジェクト]** ダイアログで次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="81117-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="81117-114">左側のウィンドウで、**[.NET Core]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-114">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="81117-115">中央のウィンドウで、**[ASP.NET Core Web Application]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="81117-116">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-116">Select **OK**.</span></span>

![[新しいプロジェクト] ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="81117-118">**[新しい ASP.NET Core Web アプリケーション]** ダイアログで次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="81117-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="81117-119">**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-119">Select **Web Application**.</span></span>

* <span data-ttu-id="81117-120">**[認証の変更]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-120">Select **Change Authentication**.</span></span>

![[新しいプロジェクト] ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="81117-122">**[認証の変更]** ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="81117-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="81117-123">**[個人のユーザー アカウント]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-123">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="81117-124">**[OK]** を選択して **[新しい ASP.NET Core Web アプリケーション]** に戻り、もう一度 **[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![新しい ASP.NET Core Web 認証ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="81117-126">Visual Studio によってソリューションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="81117-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="81117-127">アプリをローカルで実行する</span><span class="sxs-lookup"><span data-stu-id="81117-127">Run the app locally</span></span>

* <span data-ttu-id="81117-128">**[デバッグ]**、**[デバッグなしで開始]** の順にクリックして、アプリをローカルで実行します。</span><span class="sxs-lookup"><span data-stu-id="81117-128">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="81117-129">**[バージョン情報]** リンクと [**連絡先**] リンクをクリックして、Web アプリケーションが機能することを確認します。</span><span class="sxs-lookup"><span data-stu-id="81117-129">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Microsoft Edge で開いているローカルホストの Web アプリケーション](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="81117-131">**[登録]** を選択して、新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="81117-131">Select **Register** and register a new user.</span></span> <span data-ttu-id="81117-132">架空の電子メール アドレスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="81117-132">You can use a fictitious email address.</span></span> <span data-ttu-id="81117-133">送信すると、ページに次のエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="81117-133">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="81117-134">*Internal Server Error: A database operation failed while processing the request. /(内部サーバーエラー: 要求の処理中にデータベースの操作に失敗しました。/)SQL exception: Cannot open the database. /(SQL 例外: データベースを開くことができません。/)Applying existing migrations for Application DB context may resolve this issue. /(アプリケーション DB コンテキストの既存の移行を適用すると問題が解決する場合があります。/)*</span><span class="sxs-lookup"><span data-stu-id="81117-134">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="81117-135">**[Apply Migrations]/(移行を適用する/)** を選択し、移行が完了したら、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="81117-135">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Internal Server Error: A database operation failed while processing the request./(内部サーバーエラー: 要求の処理中にデータベースの操作に失敗しました。/)](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="81117-139">アプリには、新しいユーザーの登録に使用した電子メールと **[ログアウト]** リンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="81117-139">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Microsoft Edge で開いている Web アプリケーション。](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="81117-142">Azure にアプリを配置する</span><span class="sxs-lookup"><span data-stu-id="81117-142">Deploy the app to Azure</span></span>

<span data-ttu-id="81117-143">Web ページを閉じて Visual Studio に戻り、**[デバッグ]** メニューから **[デバッグの停止]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-143">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="81117-144">ソリューション エクスプローラーでプロジェクトを右クリックし、**[発行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![[発行] リンクが選択された状態でコンテキスト メニューが開きます](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="81117-146">**[発行]** ダイアログで、**[Microsoft Azure App Service]** を選択し、**[発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="81117-146">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![[発行] ダイアログ](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="81117-148">アプリに一意の名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="81117-148">Name the app a unique name.</span></span> 

* <span data-ttu-id="81117-149">サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-149">Select a subscription.</span></span>

* <span data-ttu-id="81117-150">リソース グループの **[新規作成]** を選択し、新しいリソース グループの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="81117-150">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="81117-151">アプリ サービス プランの **[新規作成]** を選択し、近くの場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-151">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="81117-152">既定で生成された名前をそのまま使用できます。</span><span class="sxs-lookup"><span data-stu-id="81117-152">You can keep the name that is generated by default.</span></span>

![[App Service] ダイアログ](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="81117-154">**[サービス]** タブを選択して、新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="81117-154">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="81117-155">緑色の **+** アイコンを選択して新しい SQL Database を作成します。</span><span class="sxs-lookup"><span data-stu-id="81117-155">Select the green **+** icon to create a new SQL Database</span></span>

![新しい SQL Database](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="81117-157">**[SQL Database の構成]** ダイアログの **[新規作成]** を選択して新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="81117-157">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![新しい SQL Database とサーバー](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="81117-159">**[SQL Server の構成]** ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="81117-159">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="81117-160">管理者のユーザー名とパスワードを入力し、**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-160">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="81117-161">この手順で作成したユーザー名とパスワードは忘れないでください。</span><span class="sxs-lookup"><span data-stu-id="81117-161">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="81117-162">既定の **[サーバー名]** をそのまま使用できます。</span><span class="sxs-lookup"><span data-stu-id="81117-162">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="81117-163">データベースの名前と接続文字列を入力します。</span><span class="sxs-lookup"><span data-stu-id="81117-163">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="81117-164">管理者のユーザー名に "admin" は使用できません。</span><span class="sxs-lookup"><span data-stu-id="81117-164">"admin" is not allowed as the administrator user name.</span></span>

![[SQL Server の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="81117-166">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-166">Select **OK**.</span></span>

<span data-ttu-id="81117-167">Visual Studio が **[App Service の作成]** ダイアログに戻ります。</span><span class="sxs-lookup"><span data-stu-id="81117-167">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="81117-168">**[App Service の作成]** ダイアログで **[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-168">Select **Create** on the **Create App Service** dialog.</span></span>

![[SQL Database の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="81117-170">**[発行]** ダイアログの **[設定]** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="81117-170">Click the **Settings** link in the **Publish** dialog.</span></span>

![[発行] ダイアログ: [接続] パネル](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="81117-172">**[発行]** ダイアログの **[設定]** ページで次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="81117-172">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="81117-173">**[データベース]** を展開し、**[この接続文字列を実行時に使用する]** をオンにします。</span><span class="sxs-lookup"><span data-stu-id="81117-173">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="81117-174">**[Entity Framework の移行]** を展開し、**[発行時にこの移行を適用する]** をオンにします。</span><span class="sxs-lookup"><span data-stu-id="81117-174">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="81117-175">**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-175">Select **Save**.</span></span> <span data-ttu-id="81117-176">Visual Studio が **[発行]** ダイアログに戻ります。</span><span class="sxs-lookup"><span data-stu-id="81117-176">Visual Studio returns to the **Publish** dialog.</span></span> 

![[発行] ダイアログ: [設定] パネル](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="81117-178">**[発行]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="81117-178">Click **Publish**.</span></span> <span data-ttu-id="81117-179">Visual Studio から Azure にアプリが発行され、ブラウザーでクラウド アプリが起動します。</span><span class="sxs-lookup"><span data-stu-id="81117-179">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="81117-180">Azure でアプリをテストする</span><span class="sxs-lookup"><span data-stu-id="81117-180">Test your app in Azure</span></span>

* <span data-ttu-id="81117-181">**[About]** リンクと **[Contact]** リンクをテストします</span><span class="sxs-lookup"><span data-stu-id="81117-181">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="81117-182">新しいユーザーを登録します</span><span class="sxs-lookup"><span data-stu-id="81117-182">Register a new user</span></span>

![Microsoft Edge で開かれている Azure App Service の Web アプリケーション](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="81117-184">アプリを更新する</span><span class="sxs-lookup"><span data-stu-id="81117-184">Update the app</span></span>

* <span data-ttu-id="81117-185">*Pages/About.cshtml* Razor ページを編集して、その内容を変更します。</span><span class="sxs-lookup"><span data-stu-id="81117-185">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="81117-186">たとえば、"Hello ASP.NET Core!" と表示されるように段落を修正できます。</span><span class="sxs-lookup"><span data-stu-id="81117-186">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="81117-187">プロジェクトを右クリックし、**[発行]** をもう一度選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-187">Right-click on the project and select **Publish...** again.</span></span>

![[発行] リンクが選択された状態でコンテキスト メニューが開きます](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="81117-189">アプリが発行されたら、Azure に変更内容が反映されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="81117-189">After the app is published, verify the changes you made are available on Azure.</span></span>

![タスクの完了を確認します。](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="81117-191">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="81117-191">Clean up</span></span>

<span data-ttu-id="81117-192">アプリのテストが完了したら、[Azure Portal](https://portal.azure.com/) に移動し、アプリを削除します。</span><span class="sxs-lookup"><span data-stu-id="81117-192">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="81117-193">**[リソース グループ]** を選択し、作成したリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-193">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: サイドバー メニューの [リソース グループ]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="81117-195">**[リソース グループ]** ページで、**[削除]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-195">In the **Resource groups** page, select **Delete**.</span></span>

![Azure Portal: [リソース グループ] ページ](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="81117-197">リソース グループ名を入力し、**[削除]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="81117-197">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="81117-198">このチュートリアルで作成されたアプリとその他すべてのリソースが Azure から削除されます。</span><span class="sxs-lookup"><span data-stu-id="81117-198">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="81117-199">次のステップ</span><span class="sxs-lookup"><span data-stu-id="81117-199">Next steps</span></span>

* [<span data-ttu-id="81117-200">ASP.NET Core MVC と Visual Studio の概要</span><span class="sxs-lookup"><span data-stu-id="81117-200">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="81117-201">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="81117-201">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="81117-202">ASP.NET Core の基礎の概要</span><span class="sxs-lookup"><span data-stu-id="81117-202">Fundamentals</span></span>](../fundamentals/index.md)
