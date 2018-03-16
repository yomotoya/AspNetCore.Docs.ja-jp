---
title: "Visual Studio で Azure と ASP.NET Core での Git に継続的なデプロイ"
author: rick-anderson
description: "Visual Studio で ASP.NET Core Web アプリを作成し、それを Azure App Service に配置する方法について説明します。Git を利用し、継続的に配置します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 7302de1ace62dba53b317039aac7f4763314aa19
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="35b1a-103">Visual Studio で Azure と ASP.NET Core での Git に継続的なデプロイ</span><span class="sxs-lookup"><span data-stu-id="35b1a-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="35b1a-104">作成者: [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="35b1a-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="35b1a-105">このチュートリアルでは、継続的なデプロイを使用して Visual Studio を使用して ASP.NET Core web アプリを作成し、Visual Studio から Azure App Service に配置する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="35b1a-106">「[Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)」 (VSTS と継続的配置で Azure Web アプリをビルドし、公開する) も併せてご覧ください。Visual Studio Team Services を利用した、[Azure App Service](/azure/app-service/app-service-web-overview) の継続的配信 (CD) ワークフローの構成方法を紹介しています。</span><span class="sxs-lookup"><span data-stu-id="35b1a-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="35b1a-107">Team Services で azure の継続的な配信には、Azure App Service でホストされているアプリの更新プログラムを発行する配置の堅牢パイプラインの設定が簡略化します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="35b1a-108">パイプラインは、ビルド、テストを実行する、ステージング スロットにデプロイし、実稼働環境に展開するには、Azure ポータルから構成できます。</span><span class="sxs-lookup"><span data-stu-id="35b1a-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="35b1a-109">このチュートリアルを完了するには、Microsoft Azure アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="35b1a-110">アカウントを取得する[MSDN サブスクライバー特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F)または[無料試用版にサインアップする](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="35b1a-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="35b1a-111">Prerequisites</span></span>

<span data-ttu-id="35b1a-112">このチュートリアルでは、次のソフトウェアがインストールされている前提としています。</span><span class="sxs-lookup"><span data-stu-id="35b1a-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="35b1a-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35b1a-113">Visual Studio</span></span>](https://www.visualstudio.com)
* <span data-ttu-id="35b1a-114">[.NET core SDK](https://www.microsoft.com/net/download/core) (ランタイムおよび tooling)</span><span class="sxs-lookup"><span data-stu-id="35b1a-114">[.NET Core SDK](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>
* <span data-ttu-id="35b1a-115">[Git](https://git-scm.com/downloads) for Windows</span><span class="sxs-lookup"><span data-stu-id="35b1a-115">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="35b1a-116">ASP.NET Core Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="35b1a-116">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="35b1a-117">Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-117">Start Visual Studio.</span></span>

1. <span data-ttu-id="35b1a-118">**[ファイル]** メニューで、**[新規作成]**、**[プロジェクト]** の順に作成します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-118">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="35b1a-119">**[ASP.NET Core Web アプリケーション]** プロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-119">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="35b1a-120">このテンプレートは、**[インストール済み]** > **[テンプレート]** > **[Visual C#]** > **[.NET Core]** の下にあります。</span><span class="sxs-lookup"><span data-stu-id="35b1a-120">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="35b1a-121">プロジェクトに `SampleWebAppDemo` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="35b1a-121">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="35b1a-122">選択、**新しい Git リポジトリの作成**オプションし、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-122">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![[新しいプロジェクト] ダイアログ](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="35b1a-124">**[New ASP.NET Core Project]\(新しい ASP.NET Core プロジェクト\)** ダイアログで、ASP.NET Core の **[空]** テンプレートを選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="35b1a-124">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![[新しい ASP.NET プロジェクト] ダイアログ](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="35b1a-126">.NET Core の最新のリリースは 2.0 です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-126">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="35b1a-127">Web アプリのローカル実行</span><span class="sxs-lookup"><span data-stu-id="35b1a-127">Running the web app locally</span></span>

1. <span data-ttu-id="35b1a-128">Visual Studio でアプリの作成が完了したら、**[デバッグ]**、**[デバッグの開始]** の順に選択し、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="35b1a-129">代わりに、キーを押して**f5 キーを押して**です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-129">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="35b1a-130">Visual Studio と新しいアプリの初期化に時間がかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="35b1a-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="35b1a-131">これが完了したら、ブラウザーは、実行中のアプリを示します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-131">Once it's complete, the browser shows the running app.</span></span>

   ![ブラウザー ウィンドウの実行中のアプリに 'Hello World!' と表示されます。](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="35b1a-133">実行中の Web アプリを確認すると、ブラウザーを終了し、アプリを停止する Visual Studio のツールバーで、「デバッグの停止」アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-133">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="35b1a-134">Azure Portal で Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="35b1a-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="35b1a-135">次の手順では、Azure ポータルで web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-135">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="35b1a-136">ログインに、 [Azure ポータル](https://portal.azure.com)です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-136">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="35b1a-137">選択**新規**でポータルのインターフェイスの左上です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-137">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="35b1a-138">選択**Web + モバイル** > **Web アプリ**です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-138">Select **Web + Mobile** > **Web App**.</span></span>

   ![Microsoft Azure Portal: [新規作成] ボタン: [Marketplace] の [Web + モバイル]: [おすすめアプリ] の [Web アプリ] ボタン](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="35b1a-140">**[Web アプリ]** ブレードに **[App Service の名前]** の一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![[Web アプリ] ブレード](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="35b1a-142">**アプリ サービス名**名前は一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="35b1a-142">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="35b1a-143">ポータルは、名前を指定した場合、このルールを適用します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-143">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="35b1a-144">別の値を提供する場合の発生するたびにその値に置き換えます**SampleWebAppDemo**このチュートリアルでします。</span><span class="sxs-lookup"><span data-stu-id="35b1a-144">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="35b1a-145">**[Web アプリ]** ブレードではまた、既存の **[App Service プラン/場所]** を選択するか、新しい App Service プラン/場所を作成できます。</span><span class="sxs-lookup"><span data-stu-id="35b1a-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="35b1a-146">新しいプランを作成する場合は、価格レベル、場所、およびその他のオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-146">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="35b1a-147">App Service プランの詳細については、次を参照してください。 [Azure App Service プランの詳細な概要](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-147">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="35b1a-148">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-148">Select **Create**.</span></span> <span data-ttu-id="35b1a-149">Azure がプロビジョニングされ、web アプリが開始されます。</span><span class="sxs-lookup"><span data-stu-id="35b1a-149">Azure will provision and start the web app.</span></span>

   ![Azure Portal: サンプル Web アプリ デモ 01 の [要点] ブレード](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="35b1a-151">新しい Web アプリに対して Git 公開を有効にする</span><span class="sxs-lookup"><span data-stu-id="35b1a-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="35b1a-152">Git は、Azure App Service web アプリを配置するために使用する分散型バージョン管理システムです。</span><span class="sxs-lookup"><span data-stu-id="35b1a-152">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="35b1a-153">Web アプリ コードが、ローカルの Git リポジトリに格納されているし、コードがリモート リポジトリにプッシュして Azure に展開します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-153">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="35b1a-154">ログイン、 [Azure ポータル](https://portal.azure.com)です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-154">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="35b1a-155">選択**App Services** Azure サブスクリプションに関連付けられているアプリ サービスの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-155">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="35b1a-156">このチュートリアルの前のセクションで作成された web アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-156">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="35b1a-157">**[デプロイ]** ブレードで、**[デプロイ オプション]** > **[ソースの選択]** > **[ローカル Git リポジトリ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-157">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![[設定] ブレード: [展開元] ブレード: [ソースの選択] ブレード](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="35b1a-159">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-159">Select **OK**.</span></span>

1. <span data-ttu-id="35b1a-160">場合は、web アプリまたはその他の App Service アプリを発行のデプロイ資格情報を設定していない、ここで設定します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-160">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="35b1a-161">選択**設定** > **デプロイ資格情報**です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-161">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="35b1a-162">**デプロイ資格情報を設定**ブレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="35b1a-162">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="35b1a-163">ユーザー名とパスワードを作成します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-163">Create a user name and password.</span></span> <span data-ttu-id="35b1a-164">Git を設定するときは、後で使用するパスワードを保存します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-164">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="35b1a-165">**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-165">Select **Save**.</span></span>

1. <span data-ttu-id="35b1a-166">**Web アプリ**ブレードで、**設定** > **プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-166">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="35b1a-167">展開するリモート Git リポジトリの URL が下に表示された**GIT URL**です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-167">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="35b1a-168">**GIT URL** の値をコピーします。後で使用します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-168">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure Portal: アプリケーションの [プロパティ] ブレード](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="35b1a-170">Azure App Service に web アプリを公開します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-170">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="35b1a-171">このセクションを使用して Visual Studio とプッシュ リポジトリから Azure に web アプリを展開して、ローカル Git リポジトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-171">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="35b1a-172">実行する必要がある手順は、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="35b1a-172">The steps involved include the following:</span></span>

* <span data-ttu-id="35b1a-173">値を使用して、GIT の URL、ローカル リポジトリを Azure にデプロイできるようにリモート リポジトリの設定を追加します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-173">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="35b1a-174">プロジェクトの変更をコミットします。</span><span class="sxs-lookup"><span data-stu-id="35b1a-174">Commit project changes.</span></span>
* <span data-ttu-id="35b1a-175">Azure で、プロジェクトの変更をローカル リポジトリからリモート リポジトリにプッシュします。</span><span class="sxs-lookup"><span data-stu-id="35b1a-175">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="35b1a-176">**ソリューション エクスプローラー**で **[ソリューション 'SampleWebAppDemo']** を右クリックし、**[コミット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-176">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="35b1a-177">**チーム エクスプ ローラー**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="35b1a-177">The **Team Explorer** is displayed.</span></span>

   ![[チーム エクスプローラー接続] タブ](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="35b1a-179">**チーム エクスプローラー**で、**[ホーム]** (ホーム アイコン)、**[設定]**、**[リポジトリ設定]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-179">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="35b1a-180">**リモコン**のセクションで、**レポジトリ設定****追加**です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-180">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="35b1a-181">**リモート追加** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="35b1a-181">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="35b1a-182">リモートの **[名前]** を **[Azure-SampleApp]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-182">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="35b1a-183">値を設定**フェッチ**を**Git URL**このチュートリアルで前に、Azure からコピーします。</span><span class="sxs-lookup"><span data-stu-id="35b1a-183">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="35b1a-184">これは **.git** で終わる URL であることにご注意ください。</span><span class="sxs-lookup"><span data-stu-id="35b1a-184">Note that this is the URL that ends with **.git**.</span></span>

   ![[リモートの編集] ダイアログ](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="35b1a-186">代わりに、リモート リポジトリからの指定、**コマンド ウィンドウ**を開いて、**コマンド ウィンドウ**、プロジェクト ディレクトリに変更して、コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-186">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="35b1a-187">例:</span><span class="sxs-lookup"><span data-stu-id="35b1a-187">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="35b1a-188">**[ホーム]** (ホーム アイコン)、**[設定]**、**[グローバル設定]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-188">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="35b1a-189">名前と電子メール アドレスが設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-189">Confirm that the name and email address are set.</span></span> <span data-ttu-id="35b1a-190">選択**更新**必要な場合です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-190">Select **Update** if required.</span></span>

1. <span data-ttu-id="35b1a-191">**[ホーム]**、**[変更]** の順に選択し、**[変更]** ビューに戻ります。</span><span class="sxs-lookup"><span data-stu-id="35b1a-191">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="35b1a-192">コミット メッセージを入力します。**最初プッシュ 1**選択**コミット**です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-192">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="35b1a-193">この操作を作成、*コミット*ローカルです。</span><span class="sxs-lookup"><span data-stu-id="35b1a-193">This action creates a *commit* locally.</span></span>

   ![[チーム エクスプローラー接続] タブ](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="35b1a-195">コミットに変更する代わりに、**コマンド ウィンドウ**を開いて、**コマンド ウィンドウ**、プロジェクト ディレクトリに変更して、git コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-195">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="35b1a-196">例:</span><span class="sxs-lookup"><span data-stu-id="35b1a-196">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="35b1a-197">**[ホーム]**、**[同期]**、**[アクション]**、**[コマンド プロンプトを開く]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-197">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="35b1a-198">プロジェクト ディレクトリに、コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="35b1a-198">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="35b1a-199">コマンド ウィンドウで次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-199">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="35b1a-200">Azure の入力**デプロイ資格情報**パスワードを Azure の前半で作成します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-200">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="35b1a-201">このコマンドは、ローカルのプロジェクト ファイルを Azure にプッシュするというプロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-201">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="35b1a-202">上記のコマンドからの出力は、展開が成功したメッセージで終了します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-202">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="35b1a-203">プロジェクトで共同作業が必要な場合へのプッシュを検討して[GitHub](https://github.com)を Azure にプッシュする前にします。</span><span class="sxs-lookup"><span data-stu-id="35b1a-203">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="35b1a-204">アクティブ デプロイを検証する</span><span class="sxs-lookup"><span data-stu-id="35b1a-204">Verify the Active Deployment</span></span>

<span data-ttu-id="35b1a-205">ローカルの環境から Azure に、web アプリ転送が成功したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-205">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="35b1a-206">[Azure Portal](https://portal.azure.com)、web アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-206">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="35b1a-207">選択**展開** > **展開オプション**です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-207">Select **Deployment** > **Deployment options**.</span></span>

![Azure Portal: [設定] ブレード: [デプロイ] ブレードにデプロイ成功が表示されています](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="35b1a-209">Azure でアプリを実行する</span><span class="sxs-lookup"><span data-stu-id="35b1a-209">Run the app in Azure</span></span>

<span data-ttu-id="35b1a-210">Azure に web アプリを配置すると、これで、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-210">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="35b1a-211">これは、2 つの方法で実行できます。</span><span class="sxs-lookup"><span data-stu-id="35b1a-211">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="35b1a-212">Azure ポータルでは、web アプリ、web アプリのブレードを探します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-212">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="35b1a-213">選択**参照**を既定のブラウザーでアプリを表示します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-213">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="35b1a-214">ブラウザーを開き、web アプリの URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-214">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="35b1a-215">例 : `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="35b1a-215">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="35b1a-216">Web アプリを更新および再パブリッシュ</span><span class="sxs-lookup"><span data-stu-id="35b1a-216">Update the web app and republish</span></span>

<span data-ttu-id="35b1a-217">ローカルのコードに変更を行った後、再パブリッシュします。</span><span class="sxs-lookup"><span data-stu-id="35b1a-217">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="35b1a-218">Visual Studio の**ソリューション エクスプローラー**で、*Startup.cs* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="35b1a-218">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="35b1a-219">`Configure` メソッドで、次のように表示されるように `Response.WriteAsync` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-219">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="35b1a-220">変更を保存*Startup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-220">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="35b1a-221">**ソリューション エクスプローラー**で **[ソリューション 'SampleWebAppDemo']** を右クリックし、**[コミット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-221">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="35b1a-222">**チーム エクスプ ローラー**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="35b1a-222">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="35b1a-223">コミット メッセージを入力します。`Update #2`です。</span><span class="sxs-lookup"><span data-stu-id="35b1a-223">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="35b1a-224">**[コミット]** ボタンを押し、プロジェクトの変更をコミットします。</span><span class="sxs-lookup"><span data-stu-id="35b1a-224">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="35b1a-225">**[ホーム]**、**[同期]**、**[アクション]**、**[プッシュ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-225">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="35b1a-226">代わりからの変更をプッシュ、**コマンド ウィンドウ**を開いて、**コマンド ウィンドウ**、プロジェクト ディレクトリに変更して、git コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="35b1a-226">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="35b1a-227">例:</span><span class="sxs-lookup"><span data-stu-id="35b1a-227">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="35b1a-228">Azure で更新後の Web アプリを表示する</span><span class="sxs-lookup"><span data-stu-id="35b1a-228">View the updated web app in Azure</span></span>

<span data-ttu-id="35b1a-229">選択して、更新された web アプリを表示**参照**またはブラウザーを開き、web アプリの URL を入力して、Azure ポータルでは、web アプリのブレードからです。</span><span class="sxs-lookup"><span data-stu-id="35b1a-229">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="35b1a-230">例 : `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="35b1a-230">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="35b1a-231">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="35b1a-231">Additional resources</span></span>

* [<span data-ttu-id="35b1a-232">VSTS を使用してビルドし、継続的なデプロイを Azure Web アプリを公開するには</span><span class="sxs-lookup"><span data-stu-id="35b1a-232">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="35b1a-233">プロジェクト Kudu</span><span class="sxs-lookup"><span data-stu-id="35b1a-233">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
