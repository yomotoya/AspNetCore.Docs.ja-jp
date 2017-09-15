---
title: "Visual Studio と Git による Azure への継続的配置"
author: rick-anderson
description: "Visual Studio で ASP.NET Core Web アプリを作成し、それを Azure App Service に配置する方法について説明します。Git を利用し、継続的に配置します。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: b576ef6bce3b211afe7465f33dfe62c25dac1f62
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="40071-104">Visual Studio と Git による ASP.NET Core 向け Azure への継続的配置</span><span class="sxs-lookup"><span data-stu-id="40071-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="40071-105">作成者: [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="40071-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="40071-106">このチュートリアルでは、Visual Studio で ASP.NET Core Web アプリを作成し、それを Visual Studio から Azure App Service に継続的に配置する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="40071-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="40071-107">「[Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)」 (VSTS と継続的配置で Azure Web アプリをビルドし、公開する) も併せてご覧ください。Visual Studio Team Services を利用した、[Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) の継続的配信 (CD) ワークフローの構成方法を紹介しています。</span><span class="sxs-lookup"><span data-stu-id="40071-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="40071-108">チーム サービスの Azure 継続的配信は、Azure App Service にアプリの更新プログラムを公開するための堅牢な配置パイプラインを簡単に設定できるようにします。</span><span class="sxs-lookup"><span data-stu-id="40071-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="40071-109">Azure ポータルで、このパイプラインのビルド、テスト実行、ステージング スロットへの展開、運用への展開を構成できます。</span><span class="sxs-lookup"><span data-stu-id="40071-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="40071-110">このチュートリアルを完了するには、Microsoft Azure アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="40071-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="40071-111">アカウントをお持ち出ない場合、[MSDN サブスクライバー特典を有効にする](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)か、[無料試用版にサインアップ](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)できます。</span><span class="sxs-lookup"><span data-stu-id="40071-111">If you don't have an account, you can [activate your MSDN subscriber benefits](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40071-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="40071-112">Prerequisites</span></span>

<span data-ttu-id="40071-113">このチュートリアルでは、既に以下をインストールしていることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="40071-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="40071-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40071-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="40071-115">[ASP.NET Core](http://go.microsoft.com/fwlink/?LinkId=627627) (ランタイムとツール)</span><span class="sxs-lookup"><span data-stu-id="40071-115">[ASP.NET Core](http://go.microsoft.com/fwlink/?LinkId=627627) (runtime and tooling)</span></span>

* <span data-ttu-id="40071-116">[Git](http://git-scm.com/downloads) for Windows</span><span class="sxs-lookup"><span data-stu-id="40071-116">[Git](http://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="40071-117">ASP.NET Core Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="40071-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="40071-118">Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="40071-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="40071-119">**[ファイル]** メニューで、**[新規作成]**、**[プロジェクト]** の順に作成します。</span><span class="sxs-lookup"><span data-stu-id="40071-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="40071-120">**[ASP.NET Web アプリケーション]** プロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-120">Select the **ASP.NET Web Application** project template.</span></span> <span data-ttu-id="40071-121">**[インストール]**、**[テンプレート]**、**[Visual C#]**、**[Web]** の下に表示されます。</span><span class="sxs-lookup"><span data-stu-id="40071-121">It appears under **Installed** > **Templates** > **Visual C#** > **Web**.</span></span> <span data-ttu-id="40071-122">プロジェクトに `SampleWebAppDemo` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="40071-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="40071-123">**[Create new Git repository]\(新しい Git リポジトリを作成する\)** オプションを選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40071-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![[新しいプロジェクト] ダイアログ](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="40071-125">**[新しい ASP.NET プロジェクト]** ダイアログで、ASP.NET Core の **[空]** テンプレートを選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40071-125">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![[新しい ASP.NET プロジェクト] ダイアログ](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a><span data-ttu-id="40071-127">Web アプリのローカル実行</span><span class="sxs-lookup"><span data-stu-id="40071-127">Running the web app locally</span></span>

1. <span data-ttu-id="40071-128">Visual Studio でアプリの作成が完了したら、**[デバッグ]**、**[デバッグの開始]** の順に選択し、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="40071-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="40071-129">あるいは、**F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="40071-129">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="40071-130">Visual Studio と新しいアプリの初期化に時間がかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="40071-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="40071-131">完了すると、ブラウザーに実行中のアプリが表示されます。</span><span class="sxs-lookup"><span data-stu-id="40071-131">Once it is complete, the browser will show the running app.</span></span>

   ![ブラウザー ウィンドウの実行中のアプリに 'Hello World!' と表示されます。](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="40071-133">実行中の Web アプリを確認したら、ブラウザーを閉じ、Visual Studio のツール バーにある "デバッグの停止" アイコンをクリックしてアプリを停止します。</span><span class="sxs-lookup"><span data-stu-id="40071-133">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="40071-134">Azure Portal で Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="40071-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="40071-135">次の手順は、Azure Portal で Web アプリを作成する方法です。</span><span class="sxs-lookup"><span data-stu-id="40071-135">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="40071-136">[Azure Portal](https://portal.azure.com) にログインします。</span><span class="sxs-lookup"><span data-stu-id="40071-136">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="40071-137">ポータルの左上にある **[新規作成]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="40071-137">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="40071-138">**[Web + モバイル]**、**[Web アプリ]** の順にタップします。</span><span class="sxs-lookup"><span data-stu-id="40071-138">TAP **Web + Mobile** > **Web App**</span></span>

    ![Microsoft Azure Portal: [新規作成] ボタン: [Marketplace] の [Web + モバイル]: [おすすめアプリ] の [Web アプリ] ボタン](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="40071-140">**[Web アプリ]** ブレードに **[App Service の名前]** の一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="40071-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![[Web アプリ] ブレード](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="40071-142">**[App Service の名前]** の名前は一意にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="40071-142">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="40071-143">ポータルでは、名前を入力しようとすると、この規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="40071-143">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="40071-144">別の値を入力したら、このチュートリアルで **SampleWebAppDemo** が出てくるたびに、その値を代わりに用いる必要があります。</span><span class="sxs-lookup"><span data-stu-id="40071-144">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="40071-145">**[Web アプリ]** ブレードではまた、既存の **[App Service プラン/場所]** を選択するか、新しい App Service プラン/場所を作成できます。</span><span class="sxs-lookup"><span data-stu-id="40071-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="40071-146">新しいプランを作成した場合、価格レベル、場所、その他のオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-146">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="40071-147">App Service プランに関する詳細については、「[Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/)」 (Azure App Service プランの詳細) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="40071-147">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="40071-148">
              **[作成]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40071-148">Click **Create**.</span></span> <span data-ttu-id="40071-149">Web アプリがプロビジョニングされ、起動します。</span><span class="sxs-lookup"><span data-stu-id="40071-149">Azure will provision and start your web app.</span></span>

    ![Azure Portal: サンプル Web アプリ デモ 01 の [要点] ブレード](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="40071-151">新しい Web アプリに対して Git 公開を有効にする</span><span class="sxs-lookup"><span data-stu-id="40071-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="40071-152">Git は分散型バージョン管理システムであり、これを利用して Azure App Service Web アプリを展開できます。</span><span class="sxs-lookup"><span data-stu-id="40071-152">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="40071-153">Web アプリのために記述したコードをローカル Git リポジトリに保存し、リモート リポジトリにプッシュすることでコードを Azure に展開します。</span><span class="sxs-lookup"><span data-stu-id="40071-153">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="40071-154">まだログインしていない場合、[Azure Portal](https://portal.azure.com) にログインします。</span><span class="sxs-lookup"><span data-stu-id="40071-154">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="40071-155">ナビゲーション ウィンドウの下にある **[参照]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40071-155">Click **Browse**, located at the bottom of the navigation pane.</span></span>

3. <span data-ttu-id="40071-156">**[Web アプリ]** をクリックし、Azure サブスクリプションに関連付けられている Web アプリの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="40071-156">Click **Web Apps** to view a list of the web apps associated with your Azure subscription.</span></span>

4. <span data-ttu-id="40071-157">このチュートリアルの前のセクションで作成した Web アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-157">Select the web app you created in the previous section of this tutorial.</span></span>

5. <span data-ttu-id="40071-158">**[設定]** ブレードが表示されない場合、**[Web アプリ]** ブレードの **[設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-158">If the **Settings** blade is not shown, select **Settings** in the **Web App** blade.</span></span>

6. <span data-ttu-id="40071-159">**[設定]** ブレードで、**[展開元]**、**[ソースの選択]**、**[ローカル Git リポジトリ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-159">In the **Settings** blade, select **Deployment source** > **Choose Source** > **Local Git Repository**.</span></span>

   ![[設定] ブレード: [展開元] ブレード: [ソースの選択] ブレード](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. <span data-ttu-id="40071-161">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40071-161">Click **OK**.</span></span>

8. <span data-ttu-id="40071-162">Web アプリや他の App Service アプリを公開するための展開資格情報を設定していない場合、ここで設定してください。</span><span class="sxs-lookup"><span data-stu-id="40071-162">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="40071-163">**[設定]**、**[デプロイ資格情報]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="40071-163">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="40071-164">**[デプロイ資格情報の設定]** ブレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="40071-164">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="40071-165">ユーザー名とパスワードを作成します。</span><span class="sxs-lookup"><span data-stu-id="40071-165">Create a user name and password.</span></span>  <span data-ttu-id="40071-166">後で Git を設定するとき、このパスワードが必要になります。</span><span class="sxs-lookup"><span data-stu-id="40071-166">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="40071-167">**[保存]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40071-167">Click **Save**.</span></span>

9. <span data-ttu-id="40071-168">**[Web アプリ]** ブレードで、**[設定]**、**[プロパティ]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="40071-168">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="40071-169">展開するリモート Git リポジトリの URL が **[GIT URL]** の下に表示されます。</span><span class="sxs-lookup"><span data-stu-id="40071-169">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

10. <span data-ttu-id="40071-170">**GIT URL** の値をコピーします。後で使用します。</span><span class="sxs-lookup"><span data-stu-id="40071-170">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure Portal: アプリケーションの [プロパティ] ブレード](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="40071-172">Azure App Service に Web アプリを公開する</span><span class="sxs-lookup"><span data-stu-id="40071-172">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="40071-173">このセクションでは、Visual Studio でローカル Git リポジトリを作成し、そのリポジトリから Azure にプッシュし、Web アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="40071-173">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="40071-174">実行する必要がある手順は、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="40071-174">The steps involved include the following:</span></span>

   * <span data-ttu-id="40071-175">Azure にローカル リポジトリを展開できるように、GIT URL 値を利用してリモート リポジトリ設定を追加します。</span><span class="sxs-lookup"><span data-stu-id="40071-175">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="40071-176">プロジェクトの変更をコミットします。</span><span class="sxs-lookup"><span data-stu-id="40071-176">Commit your project changes.</span></span>

   * <span data-ttu-id="40071-177">ローカル リポジトリから Azure のリモート リポジトリにプロジェクトの変更をプッシュします。</span><span class="sxs-lookup"><span data-stu-id="40071-177">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="40071-178">**ソリューション エクスプローラー**で **[ソリューション 'SampleWebAppDemo']** を右クリックし、**[コミット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-178">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="40071-179">**チーム エクスプローラー**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="40071-179">The **Team Explorer** will be displayed.</span></span>

    ![[チーム エクスプローラー接続] タブ](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="40071-181">**チーム エクスプローラー**で、**[ホーム]** (ホーム アイコン)、**[設定]**、**[リポジトリ設定]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-181">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="40071-182">**[リポジトリ設定]** の **[リモート]** セクションで、**[追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-182">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="40071-183">**[リモートの追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="40071-183">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="40071-184">リモートの **[名前]** を **[Azure-SampleApp]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="40071-184">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="40071-185">**[フェッチ]** の値を先にこのチュートリアルで Azure からコピーした **Git URL** に設定します。</span><span class="sxs-lookup"><span data-stu-id="40071-185">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="40071-186">これは **.git** で終わる URL であることにご注意ください。</span><span class="sxs-lookup"><span data-stu-id="40071-186">Note that this is the URL that ends with **.git**.</span></span>

    ![[リモートの編集] ダイアログ](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="40071-188">あるいは、**コマンド ウィンドウ**からリモート リポジトリを指定できます。**コマンド ウィンドウ**を開き、プロジェクト ディレクトリに移動し、コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="40071-188">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="40071-189">例:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="40071-189">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="40071-190">**[ホーム]** (ホーム アイコン)、**[設定]**、**[グローバル設定]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-190">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="40071-191">自分の名前とメール アドレスが設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="40071-191">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="40071-192">場合によっては、**[更新]** を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="40071-192">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="40071-193">**[ホーム]**、**[変更]** の順に選択し、**[変更]** ビューに戻ります。</span><span class="sxs-lookup"><span data-stu-id="40071-193">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="40071-194">「**Initial Push #1**」のようなコミット メッセージを入力し、**[コミット]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40071-194">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="40071-195">このアクションによりローカルで*コミット*されます。</span><span class="sxs-lookup"><span data-stu-id="40071-195">This action will create a *commit* locally.</span></span> <span data-ttu-id="40071-196">次に、Azure と*同期*する必要があります。</span><span class="sxs-lookup"><span data-stu-id="40071-196">Next, you need to *sync* with Azure.</span></span>

    ![[チーム エクスプローラー接続] タブ](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="40071-198">あるいは、**コマンド ウィンドウ**から変更をコミットできます。**コマンド ウィンドウ**を開き、プロジェクト ディレクトリに移動し、git コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="40071-198">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="40071-199">例:</span><span class="sxs-lookup"><span data-stu-id="40071-199">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="40071-200">**[ホーム]**、**[同期]**、**[アクション]**、**[コマンド プロンプトを開く]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-200">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="40071-201">コマンド プロンプトが開きます。ディレクトリはプロジェクト ディレクトリになります。</span><span class="sxs-lookup"><span data-stu-id="40071-201">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="40071-202">コマンド ウィンドウで次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="40071-202">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="40071-203">Azure で先に作成した Azure **デプロイ資格情報**パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="40071-203">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="40071-204">入力時、パスワードは表示されません。</span><span class="sxs-lookup"><span data-stu-id="40071-204">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="40071-205">このコマンドによりローカル プロジェクト ファイルを Azure にプッシュするプロセスが開始されます。</span><span class="sxs-lookup"><span data-stu-id="40071-205">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="40071-206">上記のコマンドからの出力は、展開が成功したというメッセージで終わります。</span><span class="sxs-lookup"><span data-stu-id="40071-206">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="40071-207">プロジェクトで共同作業を行う必要がある場合、Azure へのプッシュの間に [GitHub](https://github.com) にプッシュすることを検討してください。</span><span class="sxs-lookup"><span data-stu-id="40071-207">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="40071-208">アクティブ デプロイを検証する</span><span class="sxs-lookup"><span data-stu-id="40071-208">Verify the Active Deployment</span></span>

<span data-ttu-id="40071-209">ローカル環境から Azure に Web アプリを転送したことを確認できます。</span><span class="sxs-lookup"><span data-stu-id="40071-209">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="40071-210">デプロイ成功の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="40071-210">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="40071-211">[Azure Portal](https://portal.azure.com) で、Web アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-211">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="40071-212">**[設定]**、**[継続的配置]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-212">Then, select **Settings** > **Continuous deployment**.</span></span>

   ![Azure Portal: [設定] ブレード: [デプロイ] ブレードにデプロイ成功が表示されています](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="40071-214">Azure でアプリを実行する</span><span class="sxs-lookup"><span data-stu-id="40071-214">Run the app in Azure</span></span>

<span data-ttu-id="40071-215">Web アプリを Azure にデプロイしたので、アプリを実行できます。</span><span class="sxs-lookup"><span data-stu-id="40071-215">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="40071-216">これは、次の 2 つの方法で行うことができます。</span><span class="sxs-lookup"><span data-stu-id="40071-216">This can be done in two ways:</span></span>

* <span data-ttu-id="40071-217">Azure Portal で、Web アプリの Web アプリ ブレードを見つけ、**[参照]** をクリックしてアプリを既定のブラウザーで表示します。</span><span class="sxs-lookup"><span data-stu-id="40071-217">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="40071-218">ブラウザーを開き、Web アプリの URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="40071-218">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="40071-219">例:</span><span class="sxs-lookup"><span data-stu-id="40071-219">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="40071-220">Web アプリを更新し、再発行する</span><span class="sxs-lookup"><span data-stu-id="40071-220">Update your web app and republish</span></span>

<span data-ttu-id="40071-221">ローカル コードを変更したら、再発行できます。</span><span class="sxs-lookup"><span data-stu-id="40071-221">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="40071-222">Visual Studio の**ソリューション エクスプローラー**で、*Startup.cs* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="40071-222">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="40071-223">`Configure` メソッドで、次のように表示されるように `Response.WriteAsync` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="40071-223">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="40071-224">変更を *Startup.cs* に保存します。</span><span class="sxs-lookup"><span data-stu-id="40071-224">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="40071-225">**ソリューション エクスプローラー**で **[ソリューション 'SampleWebAppDemo']** を右クリックし、**[コミット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-225">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="40071-226">**チーム エクスプローラー**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="40071-226">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="40071-227">次のようなコミット メッセージを入力します。</span><span class="sxs-lookup"><span data-stu-id="40071-227">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="40071-228">**[コミット]** ボタンを押し、プロジェクトの変更をコミットします。</span><span class="sxs-lookup"><span data-stu-id="40071-228">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="40071-229">**[ホーム]**、**[同期]**、**[アクション]**、**[プッシュ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="40071-229">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="40071-230">あるいは、**コマンド ウィンドウ**から変更をプッシュできます。**コマンド ウィンドウ**を開き、プロジェクト ディレクトリに移動し、git コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="40071-230">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="40071-231">例:</span><span class="sxs-lookup"><span data-stu-id="40071-231">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="40071-232">Azure で更新後の Web アプリを表示する</span><span class="sxs-lookup"><span data-stu-id="40071-232">View the updated web app in Azure</span></span>

<span data-ttu-id="40071-233">更新後の Web アプリを表示します。Azure Portal の Web アプリ ブレードから **[参照]** を選択するか、ブラウザーを開き、Web アプリの URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="40071-233">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="40071-234">例:</span><span class="sxs-lookup"><span data-stu-id="40071-234">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="40071-235">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="40071-235">Additional Resources</span></span>

* [<span data-ttu-id="40071-236">発行と配置</span><span class="sxs-lookup"><span data-stu-id="40071-236">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="40071-237">プロジェクト Kudu</span><span class="sxs-lookup"><span data-stu-id="40071-237">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
