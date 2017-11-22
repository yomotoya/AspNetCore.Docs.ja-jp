---
title: "Visual Studio を使用して Azure に ASP.NET Core アプリを発行する"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 6f697ed4d8876a19cd058533e4f6a5d4f7cdc2fb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="20ccf-103">Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する</span><span class="sxs-lookup"><span data-stu-id="20ccf-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="20ccf-104">[Rick Anderson](https://twitter.com/RickAndMSFT)、[Cesar Blum Silveira](https://github.com/cesarbs)、[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="20ccf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="20ccf-105">Mac を使用している場合は、[Mac での Visual Studio から Azure への公開](https://blog.xamarin.com/publish-azure-visual-studio-mac/)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="20ccf-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="20ccf-106">設定</span><span class="sxs-lookup"><span data-stu-id="20ccf-106">Set up</span></span>

* <span data-ttu-id="20ccf-107">Azure アカウントをお持ちでない場合は、[Azure 無料アカウント](https://aka.ms/K5y5yh)を開きます。</span><span class="sxs-lookup"><span data-stu-id="20ccf-107">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="20ccf-108">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="20ccf-108">Create a web app</span></span>

<span data-ttu-id="20ccf-109">Visual Studio のスタート ページで、**[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-109">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![[ファイル] メニュー](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="20ccf-111">**[新しいプロジェクト]** ダイアログで次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-111">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="20ccf-112">左側のウィンドウで、**[.NET Core]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-112">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="20ccf-113">中央のウィンドウで、**[ASP.NET Core Web Application]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-113">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="20ccf-114">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-114">Select **OK**.</span></span>

![[新しいプロジェクト] ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="20ccf-116">**[新しい ASP.NET Core Web アプリケーション]** ダイアログで次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-116">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="20ccf-117">**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-117">Select **Web Application**.</span></span>

* <span data-ttu-id="20ccf-118">**[認証の変更]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-118">Select **Change Authentication**.</span></span>

![[新しいプロジェクト] ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="20ccf-120">**[認証の変更]** ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="20ccf-120">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="20ccf-121">**[個人のユーザー アカウント]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-121">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="20ccf-122">**[OK]** を選択して **[新しい ASP.NET Core Web アプリケーション]** に戻り、もう一度 **[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-122">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![新しい ASP.NET Core Web 認証ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="20ccf-124">Visual Studio によってソリューションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="20ccf-124">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="20ccf-125">アプリをローカルで実行する</span><span class="sxs-lookup"><span data-stu-id="20ccf-125">Run the app locally</span></span>

* <span data-ttu-id="20ccf-126">**[デバッグ]**、**[デバッグなしで開始]** の順にクリックして、アプリをローカルで実行します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-126">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="20ccf-127">**[バージョン情報]** リンクと **[連絡先]** リンクをクリックして、Web アプリケーションが機能することを確認します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-127">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Microsoft Edge で開いているローカルホストの Web アプリケーション](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="20ccf-129">**[登録]** を選択して、新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-129">Select **Register** and register a new user.</span></span> <span data-ttu-id="20ccf-130">架空の電子メール アドレスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="20ccf-130">You can use a fictitious email address.</span></span> <span data-ttu-id="20ccf-131">送信すると、ページに次のエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="20ccf-131">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="20ccf-132">*Internal Server Error: A database operation failed while processing the request. /(内部サーバーエラー: 要求の処理中にデータベースの操作に失敗しました。/)SQL exception: Cannot open the database. /(SQL 例外: データベースを開くことができません。/)Applying existing migrations for Application DB context may resolve this issue. /(アプリケーション DB コンテキストの既存の移行を適用すると問題が解決する場合があります。/)*</span><span class="sxs-lookup"><span data-stu-id="20ccf-132">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="20ccf-133">**[Apply Migrations]/(移行を適用する/)** を選択し、移行が完了したら、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-133">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Internal Server Error: A database operation failed while processing the request./(内部サーバーエラー: 要求の処理中にデータベースの操作に失敗しました。/)](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="20ccf-137">アプリには、新しいユーザーの登録に使用した電子メールと **[ログアウト]** リンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="20ccf-137">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Microsoft Edge で開いている Web アプリケーション。](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="20ccf-140">Azure にアプリを配置する</span><span class="sxs-lookup"><span data-stu-id="20ccf-140">Deploy the app to Azure</span></span>

<span data-ttu-id="20ccf-141">Web ページを閉じて Visual Studio に戻り、**[デバッグ]** メニューから **[デバッグの停止]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-141">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="20ccf-142">ソリューション エクスプローラーでプロジェクトを右クリックし、**[発行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-142">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![[発行] リンクが選択された状態でコンテキスト メニューが開きます](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="20ccf-144">**[発行]** ダイアログで、**[Microsoft Azure App Service]** を選択し、**[発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="20ccf-144">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![[発行] ダイアログ](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="20ccf-146">アプリに一意の名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="20ccf-146">Name the app a unique name.</span></span> 

* <span data-ttu-id="20ccf-147">サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-147">Select a subscription.</span></span>

* <span data-ttu-id="20ccf-148">リソース グループの **[新規作成]** を選択し、新しいリソース グループの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-148">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="20ccf-149">アプリ サービス プランの **[新規作成]** を選択し、近くの場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-149">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="20ccf-150">既定で生成された名前をそのまま使用できます。</span><span class="sxs-lookup"><span data-stu-id="20ccf-150">You can keep the name that is generated by default.</span></span>

![[App Service] ダイアログ](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="20ccf-152">**[サービス]** タブを選択して、新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-152">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="20ccf-153">緑色の **+** アイコンを選択して新しい SQL Database を作成します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-153">Select the green **+** icon to create a new SQL Database</span></span>

![新しい SQL Database](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="20ccf-155">**[SQL Database の構成]** ダイアログの **[新規作成]** を選択して新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-155">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![新しい SQL Database とサーバー](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="20ccf-157">**[SQL Server の構成]** ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="20ccf-157">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="20ccf-158">管理者のユーザー名とパスワードを入力し、**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-158">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="20ccf-159">この手順で作成したユーザー名とパスワードは忘れないでください。</span><span class="sxs-lookup"><span data-stu-id="20ccf-159">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="20ccf-160">既定の **[サーバー名]** をそのまま使用できます。</span><span class="sxs-lookup"><span data-stu-id="20ccf-160">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="20ccf-161">データベースの名前と接続文字列を入力します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-161">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="20ccf-162">管理者のユーザー名に "admin" は使用できません。</span><span class="sxs-lookup"><span data-stu-id="20ccf-162">"admin" is not allowed as the administrator user name.</span></span>

![[SQL Server の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="20ccf-164">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-164">Select **OK**.</span></span>

<span data-ttu-id="20ccf-165">Visual Studio が **[App Service の作成]** ダイアログに戻ります。</span><span class="sxs-lookup"><span data-stu-id="20ccf-165">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="20ccf-166">**[App Service の作成]** ダイアログで **[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-166">Select **Create** on the **Create App Service** dialog.</span></span>

![[SQL Database の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="20ccf-168">**[発行]** ダイアログの **[設定]** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="20ccf-168">Click the **Settings** link in the **Publish** dialog.</span></span>

![[発行] ダイアログ: [接続] パネル](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="20ccf-170">**[発行]** ダイアログの **[設定]** ページで次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-170">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="20ccf-171">**[データベース]** を展開し、**[この接続文字列を実行時に使用する]** をオンにします。</span><span class="sxs-lookup"><span data-stu-id="20ccf-171">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="20ccf-172">**[Entity Framework の移行]** を展開し、**[発行時にこの移行を適用する]** をオンにします。</span><span class="sxs-lookup"><span data-stu-id="20ccf-172">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="20ccf-173">**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-173">Select **Save**.</span></span> <span data-ttu-id="20ccf-174">Visual Studio が **[発行]** ダイアログに戻ります。</span><span class="sxs-lookup"><span data-stu-id="20ccf-174">Visual Studio returns to the **Publish** dialog.</span></span> 

![[発行] ダイアログ: [設定] パネル](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="20ccf-176">**[発行]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="20ccf-176">Click **Publish**.</span></span> <span data-ttu-id="20ccf-177">Visual Studio から Azure にアプリが発行され、ブラウザーでクラウド アプリが起動します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-177">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="20ccf-178">Azure でアプリをテストする</span><span class="sxs-lookup"><span data-stu-id="20ccf-178">Test your app in Azure</span></span>

* <span data-ttu-id="20ccf-179">**[About]** リンクと **[Contact]** リンクをテストします</span><span class="sxs-lookup"><span data-stu-id="20ccf-179">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="20ccf-180">新しいユーザーを登録します</span><span class="sxs-lookup"><span data-stu-id="20ccf-180">Register a new user</span></span>

![Microsoft Edge で開かれている Azure App Service の Web アプリケーション](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="20ccf-182">アプリを更新する</span><span class="sxs-lookup"><span data-stu-id="20ccf-182">Update the app</span></span>

* <span data-ttu-id="20ccf-183">*Pages/About.cshtml* Razor ページを編集して、その内容を変更します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-183">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="20ccf-184">たとえば、"Hello ASP.NET Core!" と表示されるように段落を修正できます。</span><span class="sxs-lookup"><span data-stu-id="20ccf-184">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="20ccf-185">プロジェクトを右クリックし、**[発行]** をもう一度選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-185">Right-click on the project and select **Publish...** again.</span></span>

![[発行] リンクが選択された状態でコンテキスト メニューが開きます](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="20ccf-187">アプリが発行されたら、Azure に変更内容が反映されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-187">After the app is published, verify the changes you made are available on Azure.</span></span>

![タスクの完了を確認します。](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="20ccf-189">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="20ccf-189">Clean up</span></span>

<span data-ttu-id="20ccf-190">アプリのテストが完了したら、[Azure Portal](https://portal.azure.com/) に移動し、アプリを削除します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-190">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="20ccf-191">**[リソース グループ]** を選択し、作成したリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-191">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: サイドバー メニューの [リソース グループ]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="20ccf-193">**[リソース グループ]** ページで、**[削除]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-193">In the **Resource groups** page, select **Delete**.</span></span>

![Azure Portal: [リソース グループ] ページ](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="20ccf-195">リソース グループ名を入力し、**[削除]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ccf-195">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="20ccf-196">このチュートリアルで作成されたアプリとその他すべてのリソースが Azure から削除されます。</span><span class="sxs-lookup"><span data-stu-id="20ccf-196">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="20ccf-197">次のステップ</span><span class="sxs-lookup"><span data-stu-id="20ccf-197">Next steps</span></span>

* [<span data-ttu-id="20ccf-198">Visual Studio と Git による Azure への継続的配置</span><span class="sxs-lookup"><span data-stu-id="20ccf-198">Continuous Deployment to Azure with Visual Studio and Git</span></span>](../publishing/azure-continuous-deployment.md)
