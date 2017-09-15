---
title: "Visual Studio を使用して Azure に ASP.NET Core アプリを発行する"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: c4df5f4551760fbc4ed4b11362d249b24f186e00
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="71e17-103">Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する</span><span class="sxs-lookup"><span data-stu-id="71e17-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="71e17-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Cesar Blum Silveira](https://github.com/cesarbs)</span><span class="sxs-lookup"><span data-stu-id="71e17-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Cesar Blum Silveira](https://github.com/cesarbs)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="71e17-105">開発環境を設定する</span><span class="sxs-lookup"><span data-stu-id="71e17-105">Set up the development environment</span></span>

* <span data-ttu-id="71e17-106">最新の [Azure SDK for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="71e17-106">Install the latest [Azure SDK for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs).</span></span> <span data-ttu-id="71e17-107">Visual Studio をまだインストールしていない場合は、この SDK でインストールされます。</span><span class="sxs-lookup"><span data-stu-id="71e17-107">The SDK installs Visual Studio if you don't already have it.</span></span>

> [!NOTE]
> <span data-ttu-id="71e17-108">コンピューターに多数の依存関係がない場合でも、SDK のインストール時間は 30 分を超える可能性があります。</span><span class="sxs-lookup"><span data-stu-id="71e17-108">The SDK installation can take more than 30 minutes if your machine doesn't have many of the dependencies.</span></span>

* <span data-ttu-id="71e17-109">[.NET Core + Visual Studio ツール](http://go.microsoft.com/fwlink/?LinkID=798306)をインストールします。</span><span class="sxs-lookup"><span data-stu-id="71e17-109">Install [.NET Core + Visual Studio tooling](http://go.microsoft.com/fwlink/?LinkID=798306)</span></span>

* <span data-ttu-id="71e17-110">[Azure アカウント](https://portal.azure.com/)を確認します。</span><span class="sxs-lookup"><span data-stu-id="71e17-110">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="71e17-111">[無料の Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/)か、[Visual Studio サブスクライバー向けの特典をアクティブ化する](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)ことができます。</span><span class="sxs-lookup"><span data-stu-id="71e17-111">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="71e17-112">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="71e17-112">Create a web app</span></span>

<span data-ttu-id="71e17-113">Visual Studio のスタート ページで **[新しいプロジェクト]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="71e17-113">In the Visual Studio Start Page, tap **New Project...**.</span></span>

![スタート ページ](publish-to-azure-webapp-using-vs/_static/new_project.png)

<span data-ttu-id="71e17-115">または、メニューを使用して新しいプロジェクトを作成できます。</span><span class="sxs-lookup"><span data-stu-id="71e17-115">Alternatively, you can use the menus to create a new project.</span></span> <span data-ttu-id="71e17-116">**[ファイル]、[新規]、[プロジェクト]** の順にタップします。</span><span class="sxs-lookup"><span data-stu-id="71e17-116">Tap **File > New > Project...**.</span></span>

![[ファイル] メニュー](publish-to-azure-webapp-using-vs/_static/alt_new_project.png)

<span data-ttu-id="71e17-118">**[新しいプロジェクト]** ダイアログで次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="71e17-118">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="71e17-119">左側のウィンドウで、**[Web]** をタップします</span><span class="sxs-lookup"><span data-stu-id="71e17-119">In the left pane, tap **Web**</span></span>

* <span data-ttu-id="71e17-120">中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** をタップします</span><span class="sxs-lookup"><span data-stu-id="71e17-120">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>

* <span data-ttu-id="71e17-121">**[OK]** をタップします</span><span class="sxs-lookup"><span data-stu-id="71e17-121">Tap **OK**</span></span>

![[新しいプロジェクト] ダイアログ](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="71e17-123">**[新しい ASP.NET Core Web アプリケーション (.NET Core)]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="71e17-123">In the **New ASP.NET Core Web Application (.NET Core)** dialog:</span></span>

* <span data-ttu-id="71e17-124">**[Web アプリケーション]** をタップします</span><span class="sxs-lookup"><span data-stu-id="71e17-124">Tap **Web Application**</span></span>

* <span data-ttu-id="71e17-125">**[認証]** が **[個人のユーザー アカウント]** に設定されていることを確認します</span><span class="sxs-lookup"><span data-stu-id="71e17-125">Verify **Authentication** is set to **Individual User Accounts**</span></span>

* <span data-ttu-id="71e17-126">**[クラウドにホストする]** が**オフ**であることを確認します</span><span class="sxs-lookup"><span data-stu-id="71e17-126">Verify **Host in the cloud** is **not** checked</span></span>

* <span data-ttu-id="71e17-127">**[OK]** をタップします</span><span class="sxs-lookup"><span data-stu-id="71e17-127">Tap **OK**</span></span>

![[新しい ASP.NET Core Web アプリケーション (.NET Core)] ダイアログ](publish-to-azure-webapp-using-vs/_static/noath.png)

## <a name="test-the-app-locally"></a><span data-ttu-id="71e17-129">アプリをローカルでテストする</span><span class="sxs-lookup"><span data-stu-id="71e17-129">Test the app locally</span></span>

* <span data-ttu-id="71e17-130">アプリをローカルで実行するには、**Ctrl キーを押しながら F5 キー**を押します。</span><span class="sxs-lookup"><span data-stu-id="71e17-130">Press **Ctrl-F5** to run the app locally</span></span>

* <span data-ttu-id="71e17-131">**[About]** リンクと **[Contact]** リンクをタップします。</span><span class="sxs-lookup"><span data-stu-id="71e17-131">Tap the **About** and **Contact** links.</span></span> <span data-ttu-id="71e17-132">デバイスのサイズによっては、ナビゲーション アイコンをタップしてリンクを表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="71e17-132">Depending on the size of your device, you might need to tap the navigation icon to show the links</span></span>

![Microsoft Edge で開いているローカルホストの Web アプリケーション](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="71e17-134">**[Register]/(登録/)** をタップし、新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="71e17-134">Tap **Register** and register a new user.</span></span> <span data-ttu-id="71e17-135">架空の電子メール アドレスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="71e17-135">You can use a fictitious email address.</span></span> <span data-ttu-id="71e17-136">送信すると、次のエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="71e17-136">When you submit, you'll get the following error:</span></span>

![Internal Server Error: A database operation failed while processing the request./(内部サーバーエラー: 要求の処理中にデータベースの操作に失敗しました。/)](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="71e17-140">この問題は、2 つの方法のいずれかで修復できます。</span><span class="sxs-lookup"><span data-stu-id="71e17-140">You can fix the problem in two different ways:</span></span>

* <span data-ttu-id="71e17-141">**[Apply Migrations]/(移行を適用する/)** をタップし、移行が完了したら、ページを更新します。または</span><span class="sxs-lookup"><span data-stu-id="71e17-141">Tap **Apply Migrations** and, once the page updates, refresh the page; or</span></span>

* <span data-ttu-id="71e17-142">プロジェクトのディレクトリでコマンド プロンプトから以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="71e17-142">Run the following from a command prompt in the project's directory:</span></span>

  <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

  ```
  dotnet ef database update
     ```

<span data-ttu-id="71e17-143">アプリには、新しいユーザーの登録に使用した電子メールと **[Log off]/(ログオフ/)** リンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="71e17-143">The app displays the email used to register the new user and a **Log off** link.</span></span>

![Microsoft Edge で開いている Web アプリケーション。](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="71e17-146">Azure にアプリを配置する</span><span class="sxs-lookup"><span data-stu-id="71e17-146">Deploy the app to Azure</span></span>

<span data-ttu-id="71e17-147">配置用に発行したアプリが実行されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="71e17-147">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="71e17-148">アプリが実行中は、*publish* フォルダー内のファイルがロックされます。</span><span class="sxs-lookup"><span data-stu-id="71e17-148">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="71e17-149">ロックされているファイルはコピーできないため、配置は行われません。</span><span class="sxs-lookup"><span data-stu-id="71e17-149">Deployment can't occur because locked files can't be copied.</span></span>

<span data-ttu-id="71e17-150">ソリューション エクスプローラーでプロジェクトを右クリックし、**[発行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="71e17-150">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![[発行] リンクが選択された状態でコンテキスト メニューが開きます](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="71e17-152">**[発行]** ダイアログで、**[Microsoft Azure App Service]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="71e17-152">In the **Publish** dialog, tap **Microsoft Azure App Service**.</span></span>

![[発行] ダイアログ](publish-to-azure-webapp-using-vs/_static/maas1.png)

<span data-ttu-id="71e17-154">**[新規作成]** をタップして新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="71e17-154">Tap **New...** to create a new resource group.</span></span> <span data-ttu-id="71e17-155">新しいリソース グループを作成すると、このチュートリアルで作成するすべての Azure リソースを削除しやすくなります。</span><span class="sxs-lookup"><span data-stu-id="71e17-155">Creating a new resource group will make it easier to delete all the Azure resources you create in this tutorial.</span></span>

![[App Service] ダイアログ](publish-to-azure-webapp-using-vs/_static/newrg1.png)

<span data-ttu-id="71e17-157">新しいリソース グループとアプリ サービス プランを作成します。</span><span class="sxs-lookup"><span data-stu-id="71e17-157">Create a new resource group and app service plan:</span></span>

* <span data-ttu-id="71e17-158">リソース グループの **[新規作成]** をタップし、新しいリソース グループの名前を入力します</span><span class="sxs-lookup"><span data-stu-id="71e17-158">Tap **New...** for the resource group and enter a name for the new resource group</span></span>

* <span data-ttu-id="71e17-159">アプリ サービス プランの **[新規作成]** をタップし、近くの場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="71e17-159">Tap **New...** for the  app service plan and select a location near you.</span></span> <span data-ttu-id="71e17-160">既定で生成された名前をそのまま使用できます</span><span class="sxs-lookup"><span data-stu-id="71e17-160">You can keep the default generated name</span></span>

* <span data-ttu-id="71e17-161">**[ほかの Azure サービスを探す]** をタップして新しいデータベースを作成します</span><span class="sxs-lookup"><span data-stu-id="71e17-161">Tap **Explore additional Azure services** to create a new database</span></span>

![[新しいリソース グループ] ダイアログ: [ホスティング] パネル](publish-to-azure-webapp-using-vs/_static/cas.png)

* <span data-ttu-id="71e17-163">緑色の **+** アイコンをタップして新しい SQL Database を作成します</span><span class="sxs-lookup"><span data-stu-id="71e17-163">Tap the green **+** icon to create a new SQL Database</span></span>

![[新しいリソース グループ] ダイアログ: [サービス] パネル](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="71e17-165">**[SQL Database の構成]** ダイアログの **[新規作成]** をタップして新しいデータベース サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="71e17-165">Tap **New...** on the **Configure SQL Database** dialog to create a new database server.</span></span>

![[SQL Database の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf.png)

* <span data-ttu-id="71e17-167">管理者のユーザー名とパスワードを入力し、**[OK]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="71e17-167">Enter an administrator user name and password, and then tap **OK**.</span></span> <span data-ttu-id="71e17-168">この手順で作成したユーザー名とパスワードは忘れないでください。</span><span class="sxs-lookup"><span data-stu-id="71e17-168">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="71e17-169">既定の **[サーバー名]** をそのまま使用できます</span><span class="sxs-lookup"><span data-stu-id="71e17-169">You can keep the default **Server Name**</span></span>

![[SQL Server の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

> [!NOTE]
> <span data-ttu-id="71e17-171">管理者のユーザー名に "admin" は使用できません。</span><span class="sxs-lookup"><span data-stu-id="71e17-171">"admin" is not allowed as the administrator user name.</span></span>

* <span data-ttu-id="71e17-172">**[SQL Database の構成]** ダイアログの **[OK]** をタップします</span><span class="sxs-lookup"><span data-stu-id="71e17-172">Tap **OK** on the  **Configure SQL Database** dialog</span></span>

![[SQL Database の構成] ダイアログ](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="71e17-174">**[App Service の作成]** ダイアログの **[作成]** をタップします</span><span class="sxs-lookup"><span data-stu-id="71e17-174">Tap **Create** on the **Create App Service** dialog</span></span>

![[App Service の作成] ダイアログ](publish-to-azure-webapp-using-vs/_static/create_as.png)

* <span data-ttu-id="71e17-176">**[発行]** ダイアログの **[次へ]** をタップします</span><span class="sxs-lookup"><span data-stu-id="71e17-176">Tap **Next** in the **Publish** dialog</span></span>

![[発行] ダイアログ: [接続] パネル](publish-to-azure-webapp-using-vs/_static/pubc.png)

* <span data-ttu-id="71e17-178">**[発行]** ダイアログの **[設定]** ステージで次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="71e17-178">On the **Settings** stage of the **Publish** dialog:</span></span>

  * <span data-ttu-id="71e17-179">**[データベース]** を展開し、**[この接続文字列を実行時に使用する]** をオンにします</span><span class="sxs-lookup"><span data-stu-id="71e17-179">Expand **Databases** and check **Use this connection string at runtime**</span></span>

  * <span data-ttu-id="71e17-180">**[Entity Framework の移行]** を展開し、**[発行時にこの移行を適用する]** をオンにします</span><span class="sxs-lookup"><span data-stu-id="71e17-180">Expand **Entity Framework Migrations** and check **Apply this migration on publish**</span></span>

* <span data-ttu-id="71e17-181">**[発行]** をタップし、Visual Studio でアプリの発行が完了するまで待ちます</span><span class="sxs-lookup"><span data-stu-id="71e17-181">Tap **Publish** and wait until Visual Studio finishes publishing your app</span></span>

![[発行] ダイアログ: [設定] パネル](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="71e17-183">Visual Studio から Azure にアプリが発行され、ブラウザーでクラウド アプリが起動します。</span><span class="sxs-lookup"><span data-stu-id="71e17-183">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="71e17-184">Azure でアプリをテストする</span><span class="sxs-lookup"><span data-stu-id="71e17-184">Test your app in Azure</span></span>

* <span data-ttu-id="71e17-185">**[About]** リンクと **[Contact]** リンクをテストします</span><span class="sxs-lookup"><span data-stu-id="71e17-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="71e17-186">新しいユーザーを登録します</span><span class="sxs-lookup"><span data-stu-id="71e17-186">Register a new user</span></span>

![Microsoft Edge で開かれている Azure App Service の Web アプリケーション](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="update-the-app"></a><span data-ttu-id="71e17-188">アプリを更新する</span><span class="sxs-lookup"><span data-stu-id="71e17-188">Update the app</span></span>

* <span data-ttu-id="71e17-189">`Views/Home/About.cshtml` Razor ビュー ファイルを編集し、その内容を変更します。</span><span class="sxs-lookup"><span data-stu-id="71e17-189">Edit the `Views/Home/About.cshtml` Razor view file and change its contents.</span></span> <span data-ttu-id="71e17-190">例:</span><span class="sxs-lookup"><span data-stu-id="71e17-190">For example:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [7]}} -->

```html
@{
       ViewData["Title"] = "About";
   }
   <h2>@ViewData["Title"].</h2>
   <h3>@ViewData["Message"]</h3>

   <p>My updated about page.</p>
   ```

* <span data-ttu-id="71e17-191">プロジェクトを右クリックし、**[発行]** をもう一度タップします。</span><span class="sxs-lookup"><span data-stu-id="71e17-191">Right-click on the project and tap **Publish...** again</span></span>

![[発行] リンクが選択された状態でコンテキスト メニューが開きます](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="71e17-193">アプリが発行されたら、Azure に変更内容が反映されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="71e17-193">After the app is published, verify the changes you made are available on Azure</span></span>

### <a name="clean-up"></a><span data-ttu-id="71e17-194">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="71e17-194">Clean up</span></span>

<span data-ttu-id="71e17-195">アプリのテストが完了したら、[Azure Portal](https://portal.azure.com/) に移動し、アプリを削除します。</span><span class="sxs-lookup"><span data-stu-id="71e17-195">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="71e17-196">**[リソース グループ]** を選択し、作成したリソース グループをタップします</span><span class="sxs-lookup"><span data-stu-id="71e17-196">Select **Resource groups**, then tap the resource group you created</span></span>

![Azure Portal: サイドバー メニューの [リソース グループ]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="71e17-198">**[リソース グループ]** ブレードで **[削除]** をタップします</span><span class="sxs-lookup"><span data-stu-id="71e17-198">In the **Resource group** blade, tap **Delete**</span></span>

![Azure Portal: [リソース グループ] ブレード](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="71e17-200">リソース グループ名を入力し、**[削除]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="71e17-200">Enter the name of the resource group and tap **Delete**.</span></span> <span data-ttu-id="71e17-201">このチュートリアルで作成されたアプリとその他すべてのリソースが Azure から削除されます</span><span class="sxs-lookup"><span data-stu-id="71e17-201">Your app and all other resources created in this tutorial are now deleted from Azure</span></span>

### <a name="next-steps"></a><span data-ttu-id="71e17-202">次のステップ</span><span class="sxs-lookup"><span data-stu-id="71e17-202">Next steps</span></span>

* [<span data-ttu-id="71e17-203">ASP.NET Core MVC と Visual Studio の概要</span><span class="sxs-lookup"><span data-stu-id="71e17-203">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="71e17-204">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="71e17-204">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="71e17-205">ASP.NET Core の基礎の概要</span><span class="sxs-lookup"><span data-stu-id="71e17-205">Fundamentals</span></span>](../fundamentals/index.md)
