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
ms.openlocfilehash: f7ea2e76fdee19a3d964e42053f0060a0a505e5b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="5bfe4-104">Visual Studio と Git による ASP.NET Core 向け Azure への継続的配置</span><span class="sxs-lookup"><span data-stu-id="5bfe4-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="5bfe4-105">作成者: [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="5bfe4-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="5bfe4-106">このチュートリアルでは、Visual Studio で ASP.NET Core Web アプリを作成し、それを Visual Studio から Azure App Service に継続的に配置する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="5bfe4-107">「[Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)」 (VSTS と継続的配置で Azure Web アプリをビルドし、公開する) も併せてご覧ください。Visual Studio Team Services を利用した、[Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) の継続的配信 (CD) ワークフローの構成方法を紹介しています。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="5bfe4-108">チーム サービスの Azure 継続的配信は、Azure App Service にアプリの更新プログラムを公開するための堅牢な配置パイプラインを簡単に設定できるようにします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="5bfe4-109">Azure ポータルで、このパイプラインのビルド、テスト実行、ステージング スロットへの展開、運用への展開を構成できます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="5bfe4-110">このチュートリアルを完了するには、Microsoft Azure アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="5bfe4-111">アカウントをお持ち出ない場合、[MSDN サブスクライバー特典を有効にする](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)か、[無料試用版にサインアップ](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)できます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-111">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5bfe4-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="5bfe4-112">Prerequisites</span></span>

<span data-ttu-id="5bfe4-113">このチュートリアルでは、既に以下をインストールしていることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="5bfe4-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5bfe4-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="5bfe4-115">[ASP.NET Core](https://www.microsoft.com/net/download/core) (ランタイムとツール)</span><span class="sxs-lookup"><span data-stu-id="5bfe4-115">[ASP.NET Core](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>

* <span data-ttu-id="5bfe4-116">[Git](https://git-scm.com/downloads) for Windows</span><span class="sxs-lookup"><span data-stu-id="5bfe4-116">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="5bfe4-117">ASP.NET Core Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="5bfe4-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="5bfe4-118">Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="5bfe4-119">**[ファイル]** メニューで、**[新規作成]**、**[プロジェクト]** の順に作成します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="5bfe4-120">**[ASP.NET Core Web アプリケーション]** プロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-120">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="5bfe4-121">このテンプレートは、**[インストール済み]** > **[テンプレート]** > **[Visual C#]** > **[.NET Core]** の下にあります。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-121">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="5bfe4-122">プロジェクトに `SampleWebAppDemo` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="5bfe4-123">**[Create new Git repository]\(新しい Git リポジトリを作成する\)** オプションを選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![[新しいプロジェクト] ダイアログ](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="5bfe4-125">**[New ASP.NET Core Project]\(新しい ASP.NET Core プロジェクト\)** ダイアログで、ASP.NET Core の **[空]** テンプレートを選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-125">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![[新しい ASP.NET プロジェクト] ダイアログ](azure-continuous-deployment/_static/02-web-site-template.png)

>[!NOTE]
    ><span data-ttu-id="5bfe4-127">.NET Core の最新のリリースは 2.0 です。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-127">Most recent release of .NET Core is 2.0</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="5bfe4-128">Web アプリのローカル実行</span><span class="sxs-lookup"><span data-stu-id="5bfe4-128">Running the web app locally</span></span>

1. <span data-ttu-id="5bfe4-129">Visual Studio でアプリの作成が完了したら、**[デバッグ]**、**[デバッグの開始]** の順に選択し、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-129">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="5bfe4-130">あるいは、**F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-130">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="5bfe4-131">Visual Studio と新しいアプリの初期化に時間がかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-131">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="5bfe4-132">完了すると、ブラウザーに実行中のアプリが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-132">Once it is complete, the browser will show the running app.</span></span>

   ![ブラウザー ウィンドウの実行中のアプリに 'Hello World!' と表示されます。](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="5bfe4-134">実行中の Web アプリを確認したら、ブラウザーを閉じ、Visual Studio のツール バーにある "デバッグの停止" アイコンをクリックしてアプリを停止します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-134">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="5bfe4-135">Azure Portal で Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="5bfe4-135">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="5bfe4-136">次の手順は、Azure Portal で Web アプリを作成する方法です。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-136">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="5bfe4-137">[Azure Portal](https://portal.azure.com) にログインします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-137">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="5bfe4-138">ポータルの左上にある **[新規作成]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-138">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="5bfe4-139">**[Web + モバイル]**、**[Web アプリ]** の順にタップします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-139">TAP **Web + Mobile** > **Web App**</span></span>

    ![Microsoft Azure Portal: [新規作成] ボタン: [Marketplace] の [Web + モバイル]: [おすすめアプリ] の [Web アプリ] ボタン](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="5bfe4-141">**[Web アプリ]** ブレードに **[App Service の名前]** の一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-141">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![[Web アプリ] ブレード](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="5bfe4-143">**[App Service の名前]** の名前は一意にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-143">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="5bfe4-144">ポータルでは、名前を入力しようとすると、この規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-144">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="5bfe4-145">別の値を入力したら、このチュートリアルで **SampleWebAppDemo** が出てくるたびに、その値を代わりに用いる必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-145">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="5bfe4-146">**[Web アプリ]** ブレードではまた、既存の **[App Service プラン/場所]** を選択するか、新しい App Service プラン/場所を作成できます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-146">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="5bfe4-147">新しいプランを作成した場合、価格レベル、場所、その他のオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-147">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="5bfe4-148">App Service プランに関する詳細については、「[Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/)」 (Azure App Service プランの詳細) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-148">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="5bfe4-149">
              **[作成]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-149">Click **Create**.</span></span> <span data-ttu-id="5bfe4-150">Web アプリがプロビジョニングされ、起動します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-150">Azure will provision and start your web app.</span></span>

    ![Azure Portal: サンプル Web アプリ デモ 01 の [要点] ブレード](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="5bfe4-152">新しい Web アプリに対して Git 公開を有効にする</span><span class="sxs-lookup"><span data-stu-id="5bfe4-152">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="5bfe4-153">Git は分散型バージョン管理システムであり、これを利用して Azure App Service Web アプリを展開できます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-153">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="5bfe4-154">Web アプリのために記述したコードをローカル Git リポジトリに保存し、リモート リポジトリにプッシュすることでコードを Azure に展開します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-154">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="5bfe4-155">まだログインしていない場合、[Azure Portal](https://portal.azure.com) にログインします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-155">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="5bfe4-156">**[App Services]** をクリックし、Azure サブスクリプションに関連付けられているアプリ サービスの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-156">Click **App Services** to view a list of the app services associated with your Azure subscription.</span></span>

3. <span data-ttu-id="5bfe4-157">このチュートリアルの前のセクションで作成した Web アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-157">Select the web app you created in the previous section of this tutorial.</span></span>

4. <span data-ttu-id="5bfe4-158">**[デプロイ]** ブレードで、**[デプロイ オプション]** > **[ソースの選択]** > **[ローカル Git リポジトリ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-158">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![[設定] ブレード: [展開元] ブレード: [ソースの選択] ブレード](azure-continuous-deployment/_static/deployment-options.png)

5. <span data-ttu-id="5bfe4-160">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-160">Click **OK**.</span></span>

6. <span data-ttu-id="5bfe4-161">Web アプリや他の App Service アプリを公開するための展開資格情報を設定していない場合、ここで設定してください。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-161">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="5bfe4-162">**[設定]**、**[デプロイ資格情報]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-162">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="5bfe4-163">**[デプロイ資格情報の設定]** ブレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-163">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="5bfe4-164">ユーザー名とパスワードを作成します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-164">Create a user name and password.</span></span>  <span data-ttu-id="5bfe4-165">後で Git を設定するとき、このパスワードが必要になります。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-165">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="5bfe4-166">**[保存]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-166">Click **Save**.</span></span>

7. <span data-ttu-id="5bfe4-167">**[Web アプリ]** ブレードで、**[設定]**、**[プロパティ]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-167">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="5bfe4-168">展開するリモート Git リポジトリの URL が **[GIT URL]** の下に表示されます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-168">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

8. <span data-ttu-id="5bfe4-169">**GIT URL** の値をコピーします。後で使用します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-169">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure Portal: アプリケーションの [プロパティ] ブレード](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="5bfe4-171">Azure App Service に Web アプリを公開する</span><span class="sxs-lookup"><span data-stu-id="5bfe4-171">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="5bfe4-172">このセクションでは、Visual Studio でローカル Git リポジトリを作成し、そのリポジトリから Azure にプッシュし、Web アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-172">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="5bfe4-173">実行する必要がある手順は、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-173">The steps involved include the following:</span></span>

   * <span data-ttu-id="5bfe4-174">Azure にローカル リポジトリを展開できるように、GIT URL 値を利用してリモート リポジトリ設定を追加します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-174">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="5bfe4-175">プロジェクトの変更をコミットします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-175">Commit your project changes.</span></span>

   * <span data-ttu-id="5bfe4-176">ローカル リポジトリから Azure のリモート リポジトリにプロジェクトの変更をプッシュします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-176">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="5bfe4-177">**ソリューション エクスプローラー**で **[ソリューション 'SampleWebAppDemo']** を右クリックし、**[コミット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-177">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="5bfe4-178">**チーム エクスプローラー**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-178">The **Team Explorer** will be displayed.</span></span>

    ![[チーム エクスプローラー接続] タブ](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="5bfe4-180">**チーム エクスプローラー**で、**[ホーム]** (ホーム アイコン)、**[設定]**、**[リポジトリ設定]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-180">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="5bfe4-181">**[リポジトリ設定]** の **[リモート]** セクションで、**[追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-181">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="5bfe4-182">**[リモートの追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-182">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="5bfe4-183">リモートの **[名前]** を **[Azure-SampleApp]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-183">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="5bfe4-184">**[フェッチ]** の値を先にこのチュートリアルで Azure からコピーした **Git URL** に設定します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-184">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="5bfe4-185">これは **.git** で終わる URL であることにご注意ください。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-185">Note that this is the URL that ends with **.git**.</span></span>

    ![[リモートの編集] ダイアログ](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="5bfe4-187">あるいは、**コマンド ウィンドウ**からリモート リポジトリを指定できます。**コマンド ウィンドウ**を開き、プロジェクト ディレクトリに移動し、コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-187">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="5bfe4-188">例:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="5bfe4-188">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="5bfe4-189">**[ホーム]** (ホーム アイコン)、**[設定]**、**[グローバル設定]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-189">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="5bfe4-190">自分の名前とメール アドレスが設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-190">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="5bfe4-191">場合によっては、**[更新]** を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-191">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="5bfe4-192">**[ホーム]**、**[変更]** の順に選択し、**[変更]** ビューに戻ります。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-192">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="5bfe4-193">「**Initial Push #1**」のようなコミット メッセージを入力し、**[コミット]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-193">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="5bfe4-194">このアクションによりローカルで*コミット*されます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-194">This action will create a *commit* locally.</span></span> <span data-ttu-id="5bfe4-195">次に、Azure と*同期*する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-195">Next, you need to *sync* with Azure.</span></span>

    ![[チーム エクスプローラー接続] タブ](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="5bfe4-197">あるいは、**コマンド ウィンドウ**から変更をコミットできます。**コマンド ウィンドウ**を開き、プロジェクト ディレクトリに移動し、git コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-197">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="5bfe4-198">例:</span><span class="sxs-lookup"><span data-stu-id="5bfe4-198">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="5bfe4-199">**[ホーム]**、**[同期]**、**[アクション]**、**[コマンド プロンプトを開く]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-199">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="5bfe4-200">コマンド プロンプトが開きます。ディレクトリはプロジェクト ディレクトリになります。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-200">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="5bfe4-201">コマンド ウィンドウで次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-201">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="5bfe4-202">Azure で先に作成した Azure **デプロイ資格情報**パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-202">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="5bfe4-203">入力時、パスワードは表示されません。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-203">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="5bfe4-204">このコマンドによりローカル プロジェクト ファイルを Azure にプッシュするプロセスが開始されます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-204">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="5bfe4-205">上記のコマンドからの出力は、展開が成功したというメッセージで終わります。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-205">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="5bfe4-206">プロジェクトで共同作業を行う必要がある場合、Azure へのプッシュの間に [GitHub](https://github.com) にプッシュすることを検討してください。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-206">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="5bfe4-207">アクティブ デプロイを検証する</span><span class="sxs-lookup"><span data-stu-id="5bfe4-207">Verify the Active Deployment</span></span>

<span data-ttu-id="5bfe4-208">ローカル環境から Azure に Web アプリを転送したことを確認できます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-208">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="5bfe4-209">デプロイ成功の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-209">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="5bfe4-210">[Azure Portal](https://portal.azure.com) で、Web アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-210">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="5bfe4-211">次に、**[デプロイ]** > **[デプロイ オプション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-211">Then, select **Deployment** > **Deployment options**.</span></span>

   ![Azure Portal: [設定] ブレード: [デプロイ] ブレードにデプロイ成功が表示されています](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="5bfe4-213">Azure でアプリを実行する</span><span class="sxs-lookup"><span data-stu-id="5bfe4-213">Run the app in Azure</span></span>

<span data-ttu-id="5bfe4-214">Web アプリを Azure にデプロイしたので、アプリを実行できます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-214">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="5bfe4-215">これは、次の 2 つの方法で行うことができます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-215">This can be done in two ways:</span></span>

* <span data-ttu-id="5bfe4-216">Azure Portal で、Web アプリの Web アプリ ブレードを見つけ、**[参照]** をクリックしてアプリを既定のブラウザーで表示します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-216">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="5bfe4-217">ブラウザーを開き、Web アプリの URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-217">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="5bfe4-218">例:</span><span class="sxs-lookup"><span data-stu-id="5bfe4-218">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="5bfe4-219">Web アプリを更新し、再発行する</span><span class="sxs-lookup"><span data-stu-id="5bfe4-219">Update your web app and republish</span></span>

<span data-ttu-id="5bfe4-220">ローカル コードを変更したら、再発行できます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-220">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="5bfe4-221">Visual Studio の**ソリューション エクスプローラー**で、*Startup.cs* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-221">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="5bfe4-222">`Configure` メソッドで、次のように表示されるように `Response.WriteAsync` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-222">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="5bfe4-223">変更を *Startup.cs* に保存します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-223">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="5bfe4-224">**ソリューション エクスプローラー**で **[ソリューション 'SampleWebAppDemo']** を右クリックし、**[コミット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-224">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="5bfe4-225">**チーム エクスプローラー**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-225">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="5bfe4-226">次のようなコミット メッセージを入力します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-226">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="5bfe4-227">**[コミット]** ボタンを押し、プロジェクトの変更をコミットします。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-227">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="5bfe4-228">**[ホーム]**、**[同期]**、**[アクション]**、**[プッシュ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-228">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="5bfe4-229">あるいは、**コマンド ウィンドウ**から変更をプッシュできます。**コマンド ウィンドウ**を開き、プロジェクト ディレクトリに移動し、git コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-229">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="5bfe4-230">例:</span><span class="sxs-lookup"><span data-stu-id="5bfe4-230">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="5bfe4-231">Azure で更新後の Web アプリを表示する</span><span class="sxs-lookup"><span data-stu-id="5bfe4-231">View the updated web app in Azure</span></span>

<span data-ttu-id="5bfe4-232">更新後の Web アプリを表示します。Azure Portal の Web アプリ ブレードから **[参照]** を選択するか、ブラウザーを開き、Web アプリの URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="5bfe4-232">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="5bfe4-233">例:</span><span class="sxs-lookup"><span data-stu-id="5bfe4-233">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="5bfe4-234">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="5bfe4-234">Additional Resources</span></span>

* [<span data-ttu-id="5bfe4-235">発行と配置</span><span class="sxs-lookup"><span data-stu-id="5bfe4-235">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="5bfe4-236">プロジェクト Kudu</span><span class="sxs-lookup"><span data-stu-id="5bfe4-236">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
