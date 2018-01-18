---
title: "Visual Studio を使用して Azure に ASP.NET Core アプリを発行する"
author: rick-anderson
description: "Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: e8de630c4fa8cff5395f7246b91384d4725e4ca6
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2018
---
<span data-ttu-id="f2d02-104">/ja-jp</span><span class="sxs-lookup"><span data-stu-id="f2d02-104">/en-us</span></span>

# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="f2d02-105">Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する</span><span class="sxs-lookup"><span data-stu-id="f2d02-105">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="f2d02-106">[Rick Anderson](https://twitter.com/RickAndMSFT)、[Cesar Blum Silveira](https://github.com/cesarbs)、[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="f2d02-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="f2d02-107">Mac を使用している場合は、[Mac での Visual Studio から Azure への公開](https://blog.xamarin.com/publish-azure-visual-studio-mac/)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f2d02-107">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="f2d02-108">設定</span><span class="sxs-lookup"><span data-stu-id="f2d02-108">Set up</span></span>

* <span data-ttu-id="f2d02-109">Azure アカウントをお持ちでない場合は、[Azure 無料アカウント](https://aka.ms/K5y5yh)を開きます。</span><span class="sxs-lookup"><span data-stu-id="f2d02-109">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="f2d02-110">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="f2d02-110">Create a web app</span></span>

<span data-ttu-id="f2d02-111">Visual Studio のスタート ページで、**[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![[ファイル] メニュー](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="f2d02-113">**[新しいプロジェクト]** ダイアログで次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="f2d02-114">左側のウィンドウで、**[.NET Core]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-114">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="f2d02-115">中央のウィンドウで、**[ASP.NET Core Web Application]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f2d02-116">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-116">Select **OK**.</span></span>

![[新しいプロジェクト] ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="f2d02-118">**[新しい ASP.NET Core Web アプリケーション]** ダイアログで次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="f2d02-119">**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-119">Select **Web Application**.</span></span>
* <span data-ttu-id="f2d02-120">**[認証の変更]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-120">Select **Change Authentication**.</span></span>

![[新しいプロジェクト] ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="f2d02-122">**[認証の変更]** ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f2d02-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="f2d02-123">**[個人のユーザー アカウント]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-123">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="f2d02-124">**[OK]** を選択して **[新しい ASP.NET Core Web アプリケーション]** に戻り、もう一度 **[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![新しい ASP.NET Core Web 認証ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="f2d02-126">Visual Studio によってソリューションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="f2d02-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="f2d02-127">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="f2d02-127">Run the app</span></span>

* <span data-ttu-id="f2d02-128">Ctrl キーを押しながら F5 キーを押してプロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-128">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="f2d02-129">**[About]** リンクと **[Contact]** リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="f2d02-129">Test the **About** and **Contact** links.</span></span>

![Microsoft Edge で開いているローカルホストの Web アプリケーション](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="f2d02-131">ユーザーを登録する</span><span class="sxs-lookup"><span data-stu-id="f2d02-131">Register a user</span></span>

* <span data-ttu-id="f2d02-132">**[登録]** を選択して、新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-132">Select **Register** and register a new user.</span></span> <span data-ttu-id="f2d02-133">架空の電子メール アドレスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f2d02-133">You can use a fictitious email address.</span></span> <span data-ttu-id="f2d02-134">送信すると、ページに次のエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f2d02-134">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="f2d02-135">*Internal Server Error: A database operation failed while processing the request. /(内部サーバーエラー: 要求の処理中にデータベースの操作に失敗しました。/)SQL exception: Cannot open the database. /(SQL 例外: データベースを開くことができません。/)Applying existing migrations for Application DB context may resolve this issue. /(アプリケーション DB コンテキストの既存の移行を適用すると問題が解決する場合があります。/)*</span><span class="sxs-lookup"><span data-stu-id="f2d02-135">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="f2d02-136">**[Apply Migrations]/(移行を適用する/)** を選択し、移行が完了したら、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-136">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Internal Server Error: A database operation failed while processing the request./(内部サーバーエラー: 要求の処理中にデータベースの操作に失敗しました。/)](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="f2d02-140">アプリには、新しいユーザーの登録に使用した電子メールと **[ログアウト]** リンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f2d02-140">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Microsoft Edge で開いている Web アプリケーション。](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="f2d02-143">Azure にアプリを配置する</span><span class="sxs-lookup"><span data-stu-id="f2d02-143">Deploy the app to Azure</span></span>

<span data-ttu-id="f2d02-144">ソリューション エクスプローラーでプロジェクトを右クリックし、**[発行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![[発行] リンクが選択された状態でコンテキスト メニューが開きます](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="f2d02-146">**[発行]** ダイアログで、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="f2d02-146">In the **Publish** dialog:</span></span>

* <span data-ttu-id="f2d02-147">**[Microsoft Azure App Service]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-147">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="f2d02-148">歯車アイコンを選択してから **[プロファイルの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-148">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="f2d02-149">**[プロファイルの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-149">Select **Create Profile**.</span></span>

![[発行] ダイアログ](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="f2d02-151">Azure リソースを作成する</span><span class="sxs-lookup"><span data-stu-id="f2d02-151">Create Azure resources</span></span>

<span data-ttu-id="f2d02-152">**[App Service の作成]** ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f2d02-152">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="f2d02-153">ご自分のサブスクリプションを入力します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-153">Enter your subscription.</span></span>
* <span data-ttu-id="f2d02-154">**[アプリ名]**、**[リソース グループ]**、**[App Service プラン]** の各入力フィールドに値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="f2d02-154">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="f2d02-155">これらの名前を保持することも、変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="f2d02-155">You can keep these names or change them.</span></span>

![[App Service] ダイアログ](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="f2d02-157">**[サービス]** タブを選択して、新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-157">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="f2d02-158">緑色の **+** アイコンを選択して新しい SQL Database を作成します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-158">Select the green **+** icon to create a new SQL Database</span></span>

![新しい SQL Database](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="f2d02-160">**[SQL Database の構成]** ダイアログの **[新規作成]** を選択して新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-160">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![新しい SQL Database とサーバー](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="f2d02-162">**[SQL Server の構成]** ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f2d02-162">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="f2d02-163">管理者のユーザー名とパスワードを入力し、**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-163">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="f2d02-164">既定の **[サーバー名]** をそのまま使用できます。</span><span class="sxs-lookup"><span data-stu-id="f2d02-164">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="f2d02-165">管理者のユーザー名に "admin" は使用できません。</span><span class="sxs-lookup"><span data-stu-id="f2d02-165">"admin" is not allowed as the administrator user name.</span></span>

![[SQL Server の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="f2d02-167">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-167">Select **OK**.</span></span>

<span data-ttu-id="f2d02-168">Visual Studio が **[App Service の作成]** ダイアログに戻ります。</span><span class="sxs-lookup"><span data-stu-id="f2d02-168">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="f2d02-169">**[App Service の作成]** ダイアログで **[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-169">Select **Create** on the **Create App Service** dialog.</span></span>

![[SQL Database の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="f2d02-171">Visual Studio は、Azure で Web アプリと SQL Server を作成します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-171">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="f2d02-172">このステップには数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f2d02-172">This step can take a few minutes.</span></span> <span data-ttu-id="f2d02-173">作成されるリソースについては、「[追加のリソース](#additonal-resources)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f2d02-173">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="f2d02-174">配置が完了したら、**[設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-174">When deployment completes, select **Settings**:</span></span>

![[SQL Server の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="f2d02-176">**[発行]** ダイアログの **[設定]** ページで次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-176">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="f2d02-177">**[データベース]** を展開し、**[この接続文字列を実行時に使用する]** をオンにします。</span><span class="sxs-lookup"><span data-stu-id="f2d02-177">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="f2d02-178">**[Entity Framework の移行]** を展開し、**[発行時にこの移行を適用する]** をオンにします。</span><span class="sxs-lookup"><span data-stu-id="f2d02-178">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="f2d02-179">**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-179">Select **Save**.</span></span> <span data-ttu-id="f2d02-180">Visual Studio が **[発行]** ダイアログに戻ります。</span><span class="sxs-lookup"><span data-stu-id="f2d02-180">Visual Studio returns to the **Publish** dialog.</span></span> 

![[発行] ダイアログ: [設定] パネル](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="f2d02-182">**[発行]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2d02-182">Click **Publish**.</span></span> <span data-ttu-id="f2d02-183">Visual Studio が Azure にアプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-183">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="f2d02-184">配置が完了すると、ブラウザーでアプリが開きます。</span><span class="sxs-lookup"><span data-stu-id="f2d02-184">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="f2d02-185">Azure でアプリをテストする</span><span class="sxs-lookup"><span data-stu-id="f2d02-185">Test your app in Azure</span></span>

* <span data-ttu-id="f2d02-186">**[About]** リンクと **[Contact]** リンクをテストします</span><span class="sxs-lookup"><span data-stu-id="f2d02-186">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="f2d02-187">新しいユーザーを登録します</span><span class="sxs-lookup"><span data-stu-id="f2d02-187">Register a new user</span></span>

![Microsoft Edge で開かれている Azure App Service の Web アプリケーション](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="f2d02-189">アプリを更新する</span><span class="sxs-lookup"><span data-stu-id="f2d02-189">Update the app</span></span>

* <span data-ttu-id="f2d02-190">*Pages/About.cshtml* Razor ページを編集して、その内容を変更します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-190">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="f2d02-191">たとえば、次のように、"Hello ASP.NET Core!" と表示されるように段落を修正できます。[!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="f2d02-191">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="f2d02-192">プロジェクトを右クリックし、**[発行]** をもう一度選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-192">Right-click on the project and select **Publish...** again.</span></span>

![[発行] リンクが選択された状態でコンテキスト メニューが開きます](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="f2d02-194">アプリが発行されたら、Azure に変更内容が反映されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-194">After the app is published, verify the changes you made are available on Azure.</span></span>

![タスクの完了を確認します。](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="f2d02-196">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="f2d02-196">Clean up</span></span>

<span data-ttu-id="f2d02-197">アプリのテストが完了したら、[Azure Portal](https://portal.azure.com/) に移動し、アプリを削除します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-197">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="f2d02-198">**[リソース グループ]** を選択し、作成したリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-198">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: サイドバー メニューの [リソース グループ]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="f2d02-200">**[リソース グループ]** ページで、**[削除]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-200">In the **Resource groups** page, select **Delete**.</span></span>

![Azure Portal: [リソース グループ] ページ](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="f2d02-202">リソース グループ名を入力し、**[削除]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2d02-202">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="f2d02-203">このチュートリアルで作成されたアプリとその他すべてのリソースが Azure から削除されます。</span><span class="sxs-lookup"><span data-stu-id="f2d02-203">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="f2d02-204">次の手順</span><span class="sxs-lookup"><span data-stu-id="f2d02-204">Next steps</span></span>

* [<span data-ttu-id="f2d02-205">Visual Studio と Git による Azure への継続的配置</span><span class="sxs-lookup"><span data-stu-id="f2d02-205">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="f2d02-206">追加のリソース</span><span class="sxs-lookup"><span data-stu-id="f2d02-206">Additonal resources</span></span>

* [<span data-ttu-id="f2d02-207">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f2d02-207">Azure App Service</span></span>](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="f2d02-208">Azure リソース グループ</span><span class="sxs-lookup"><span data-stu-id="f2d02-208">Azure resource groups</span></span>](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="f2d02-209">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="f2d02-209">Azure SQL Database</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/)