---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: 展開をサポートするビルド定義を作成する |Microsoft Docs
author: jrjlee
description: Team Foundation Server (TFS) 2010 での任意の種類のビルドを実行する場合は、チーム プロジェクト内のビルド定義を作成する必要があります。 このトピックの「des.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: 33ebde3074603801945c676ace64b26ca5bbf44a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826880"
---
<a name="creating-a-build-definition-that-supports-deployment"></a><span data-ttu-id="7a228-104">展開をサポートするビルド定義を作成します。</span><span class="sxs-lookup"><span data-stu-id="7a228-104">Creating a Build Definition That Supports Deployment</span></span>
====================
<span data-ttu-id="7a228-105">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="7a228-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="7a228-106">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="7a228-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="7a228-107">Team Foundation Server (TFS) 2010 での任意の種類のビルドを実行する場合は、チーム プロジェクト内のビルド定義を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7a228-107">If you want to perform any kind of build in Team Foundation Server (TFS) 2010, you need to create a build definition within your team project.</span></span> <span data-ttu-id="7a228-108">このトピックでは、TFS で新しいビルド定義を作成する方法とチーム ビルドでビルド プロセスの一部として web 配置を制御する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="7a228-108">This topic describes how to create a new build definition in TFS and how to control web deployment as part of the build process in Team Build.</span></span>


<span data-ttu-id="7a228-109">このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="7a228-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="7a228-110">これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルドおよび配置プロセスでは、2 つのプロジェクト ファイル&#x2014;いずれかすべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用されるビルドの手順を含むです。</span><span class="sxs-lookup"><span data-stu-id="7a228-110">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="7a228-111">ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="7a228-111">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="7a228-112">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="7a228-112">Task Overview</span></span>

<span data-ttu-id="7a228-113">ビルド定義は、TFS のチーム プロジェクトのビルドが実行する方法とタイミングを制御するメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="7a228-113">A build definition is the mechanism that controls how and when builds occur for team projects in TFS.</span></span> <span data-ttu-id="7a228-114">各ビルド定義を指定します。</span><span class="sxs-lookup"><span data-stu-id="7a228-114">Each build definition specifies:</span></span>

- <span data-ttu-id="7a228-115">Visual Studio ソリューション ファイルまたはカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルのように、ビルドすることです。</span><span class="sxs-lookup"><span data-stu-id="7a228-115">The things you want to build, like Visual Studio solution files or custom Microsoft Build Engine (MSBuild) project files.</span></span>
- <span data-ttu-id="7a228-116">ビルドを実行する必要がありますを決定する条件は、手動トリガー、継続的インテグレーション (CI) と同様に配置します。 または、ゲート チェックインします。</span><span class="sxs-lookup"><span data-stu-id="7a228-116">The criteria that determine when a build should take place, like manual triggers, continuous integration (CI), or gated check-ins.</span></span>
- <span data-ttu-id="7a228-117">Web パッケージとデータベースのスクリプトなどの展開の成果物を含むチームを構築する必要がありますを送信するビルドの場所を出力します。</span><span class="sxs-lookup"><span data-stu-id="7a228-117">The location to which Team Build should send build outputs, including deployment artifacts like web packages and database scripts.</span></span>
- <span data-ttu-id="7a228-118">各ビルドを保持する時間数。</span><span class="sxs-lookup"><span data-stu-id="7a228-118">The amount of time that each build should be retained.</span></span>
- <span data-ttu-id="7a228-119">ビルド プロセスのさまざまな他のパラメーター。</span><span class="sxs-lookup"><span data-stu-id="7a228-119">Various other parameters of the build process.</span></span>

> [!NOTE]
> <span data-ttu-id="7a228-120">ビルド定義の詳細については、次を参照してください。[ビルド プロセスの定義](https://msdn.microsoft.com/library/ms181715.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7a228-120">For more information on build definitions, see [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx).</span></span>


<span data-ttu-id="7a228-121">このトピックでは、開発者が新しいコンテンツでチェックするとき、ビルドがトリガーできるため、CI を使用するビルド定義を作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="7a228-121">This topic will show you how to create a build definition that uses CI, so that a build is triggered when a developer checks in new content.</span></span> <span data-ttu-id="7a228-122">ビルドが成功すると、ビルド サービスは、ソリューションをテスト環境を配置するカスタムのプロジェクト ファイルを実行します。</span><span class="sxs-lookup"><span data-stu-id="7a228-122">If the build succeeds, the build service runs a custom project file to deploy the solution to a test environment.</span></span>

<span data-ttu-id="7a228-123">ビルドをトリガーするときに、これらの操作を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="7a228-123">When you trigger a build, these actions need to happen:</span></span>

- <span data-ttu-id="7a228-124">最初に、Team Build は、ソリューションをビルドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="7a228-124">First, Team Build should build the solution.</span></span> <span data-ttu-id="7a228-125">このプロセスの一環として、チーム ビルドの各ソリューションの web アプリケーション プロジェクトの web 展開パッケージを生成する Web 発行パイプライン (WPP) が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="7a228-125">As part of this process, Team Build will invoke the Web Publishing Pipeline (WPP) to generate web deployment packages for each of the web application projects in the solution.</span></span> <span data-ttu-id="7a228-126">チーム ビルドでは、ソリューションに関連付けられているすべての単体テストも実行されます。</span><span class="sxs-lookup"><span data-stu-id="7a228-126">Team Build will also run any unit tests associated with the solution.</span></span>
- <span data-ttu-id="7a228-127">ソリューションのビルドが失敗した場合、チーム ビルドする必要があります何もしないさらにします。</span><span class="sxs-lookup"><span data-stu-id="7a228-127">If the solution build fails, Team Build should take no further action.</span></span> <span data-ttu-id="7a228-128">単体テストが失敗は、ビルド エラーとして扱う必要があります。</span><span class="sxs-lookup"><span data-stu-id="7a228-128">Unit test failures should be treated as a build failure.</span></span>
- <span data-ttu-id="7a228-129">ソリューションのビルドが成功すると、チーム ビルドと、ソリューションの展開を制御するカスタムのプロジェクト ファイルを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7a228-129">If the solution build succeeds, Team Build should run the custom project file that controls the deployment of the solution.</span></span> <span data-ttu-id="7a228-130">このプロセスの一環として、Team Build は、インターネット インフォメーション サービス (IIS) Web 配置ツールが移行先の web サーバーにパッケージ化された web アプリケーションをインストールする (Web 配置) を呼び出し、データベースの作成を実行する VSDBCMD.exe ユーティリティが呼び出されます移行先のデータベース サーバー上のスクリプトです。</span><span class="sxs-lookup"><span data-stu-id="7a228-130">As part of this process, Team Build will invoke the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to install the packaged web applications on the destination web servers, and it will invoke the VSDBCMD.exe utility to run database creation scripts on the destination database servers.</span></span>

<span data-ttu-id="7a228-131">これは、プロセスを示しています。</span><span class="sxs-lookup"><span data-stu-id="7a228-131">This illustrates the process:</span></span>

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

<span data-ttu-id="7a228-132">[Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューションには、カスタムの MSBuild プロジェクト ファイルが含まれています。 *Publish.proj*、MSBuild またはチーム ビルドから実行することができます。</span><span class="sxs-lookup"><span data-stu-id="7a228-132">The [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution includes a custom MSBuild project file, *Publish.proj*, that you can run from MSBuild or Team Build.</span></span> <span data-ttu-id="7a228-133">」の説明に従って[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)、このプロジェクト ファイルは、web のパッケージとデータベースをターゲット環境にデプロイするロジックを定義します。</span><span class="sxs-lookup"><span data-stu-id="7a228-133">As described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md), this project file defines the logic that deploys your web packages and databases to a target environment.</span></span> <span data-ttu-id="7a228-134">ファイルには、チーム ビルド、展開タスクを実行するだけのままで実行されている場合で、ビルドとパッケージ化プロセスを省略するロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="7a228-134">The file includes logic that omits the building and packaging process if it's running in Team Build, leaving just the deployment tasks to run.</span></span> <span data-ttu-id="7a228-135">これにより、この方法で展開を自動化するときに、ソリューションが正常にビルドされることを確認する通常ためにはあり、展開プロセスを開始する前に、単体テストを渡します。</span><span class="sxs-lookup"><span data-stu-id="7a228-135">This is because when you automate deployment in this way, you'll typically want to ensure that the solution builds successfully and passes any unit tests before the deployment process commences.</span></span>

<span data-ttu-id="7a228-136">次のセクションでは、新しいビルド定義を作成してこのプロセスを実装する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="7a228-136">The next section explains how to implement this process by creating a new build definition.</span></span>

> [!NOTE]
> <span data-ttu-id="7a228-137">この手順&#x2014;、1 つが自動でプロセスをビルド、テスト、およびソリューションを配置します&#x2014;テスト環境へのデプロイに最も合ったする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="7a228-137">This procedure&#x2014;in which a single automated process builds, tests, and deploys a solution&#x2014;is likely to be most suited to deployment to test environments.</span></span> <span data-ttu-id="7a228-138">ステージングおよび運用環境を既に確認され、テスト環境で検証前のビルドからコンテンツを配置する可能性の高い多くできました。</span><span class="sxs-lookup"><span data-stu-id="7a228-138">For staging and production environments you're a lot more likely to want to deploy content from a previous build that you've already verified and validated in a test environment.</span></span> <span data-ttu-id="7a228-139">この方法は次のトピックで説明されている[特定のビルドを展開する](deploying-a-specific-build.md)します。</span><span class="sxs-lookup"><span data-stu-id="7a228-139">This approach is described in the next topic, [Deploying a Specific Build](deploying-a-specific-build.md).</span></span>


### <a name="who-performs-this-procedure"></a><span data-ttu-id="7a228-140">この手順を実行しますか。</span><span class="sxs-lookup"><span data-stu-id="7a228-140">Who Performs This Procedure?</span></span>

<span data-ttu-id="7a228-141">通常は、TFS 管理者は、この手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="7a228-141">Typically, a TFS administrator performs this procedure.</span></span> <span data-ttu-id="7a228-142">場合によっては、開発者のチーム リーダーは、TFS でチーム プロジェクト コレクションの責任を負う場合があります。</span><span class="sxs-lookup"><span data-stu-id="7a228-142">In some cases, a developer team leader may take responsibility for the team project collection in TFS.</span></span> <span data-ttu-id="7a228-143">新しいビルド定義を作成するためには、メンバーである必要があります、**プロジェクト コレクション ビルド管理者**ソリューションを含むチーム プロジェクト コレクションにグループ化します。</span><span class="sxs-lookup"><span data-stu-id="7a228-143">In order to create a new build definition, you need to be a member of the **Project Collection Build Administrators** group for the team project collection that contains your solution.</span></span>

## <a name="create-a-build-definition-for-ci-and-deployment"></a><span data-ttu-id="7a228-144">CI と配置のビルド定義を作成します。</span><span class="sxs-lookup"><span data-stu-id="7a228-144">Create a Build Definition for CI and Deployment</span></span>

<span data-ttu-id="7a228-145">次の手順では、CI をトリガーするビルド定義を作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="7a228-145">The next procedure describes how to create a build definition that CI triggers.</span></span> <span data-ttu-id="7a228-146">ビルドが成功すると、カスタム MSBuild プロジェクト ファイルで、ロジックを使用して、ソリューションが配置されます。</span><span class="sxs-lookup"><span data-stu-id="7a228-146">If the build succeeds, the solution is deployed using the logic in a custom MSBuild project file.</span></span>

<span data-ttu-id="7a228-147">**CI と配置のビルド定義を作成するには**</span><span class="sxs-lookup"><span data-stu-id="7a228-147">**To create a build definition for CI and deployment**</span></span>

1. <span data-ttu-id="7a228-148">Visual Studio 2010 での**チーム エクスプ ローラー**ウィンドウで、チーム プロジェクト ノードを展開し、右クリックして**ビルド**、順にクリックします**ビルド定義の新規**します。</span><span class="sxs-lookup"><span data-stu-id="7a228-148">In Visual Studio 2010, in the **Team Explorer** window, expand your team project node, right-click **Builds**, and then click **New Build Definition**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. <span data-ttu-id="7a228-149">**全般** タブで、ビルド定義の名前 (たとえば、 **DeployToTest**) とオプションの説明。</span><span class="sxs-lookup"><span data-stu-id="7a228-149">On the **General** tab, give the build definition a name (for example, **DeployToTest**) and an optional description.</span></span>
3. <span data-ttu-id="7a228-150">**トリガー**  タブで、新しいビルドをトリガーする条件を選択します。</span><span class="sxs-lookup"><span data-stu-id="7a228-150">On the **Trigger** tab, select the criteria on which you want to trigger a new build.</span></span> <span data-ttu-id="7a228-151">たとえば、次のようにソリューションをビルドし、開発者が新しいコードをチェックインするたびに、テスト環境にデプロイする場合、次のように選択します。**継続的インテグレーション**します。</span><span class="sxs-lookup"><span data-stu-id="7a228-151">For example, if you want to build the solution and deploy to the test environment every time a developer checks in new code, select **Continuous Integration**.</span></span>
4. <span data-ttu-id="7a228-152">**ビルドの既定値** タブで、**ビルド出力を次の格納フォルダーにコピー**ボックスで、ドロップ フォルダーの汎用名前付け規則 (UNC) パスを入力 (たとえば、  **\\TFSBUILD\Drops**)。</span><span class="sxs-lookup"><span data-stu-id="7a228-152">On the **Build Defaults** tab, in the **Copy build output to the following drop folder** box, type the Universal Naming Convention (UNC) path of your drop folder (for example, **\\TFSBUILD\Drops**).</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > <span data-ttu-id="7a228-153">この格納場所は、構成したリテンション期間ポリシーによっては、いくつかのビルドを格納します。</span><span class="sxs-lookup"><span data-stu-id="7a228-153">This drop location stores several builds, depending on the retention policy you configure.</span></span> <span data-ttu-id="7a228-154">ステージング環境または運用環境に、特定のビルドからデプロイの成果物を公開する場合は、これは、それらを検索します。</span><span class="sxs-lookup"><span data-stu-id="7a228-154">When you want to publish deployment artifacts from a specific build to a staging or production environment, this is where you'll find them.</span></span>
5. <span data-ttu-id="7a228-155">**プロセス** タブで、**ビルド プロセス ファイル**ドロップダウン リストのままに**DefaultTemplate.xaml**選択します。</span><span class="sxs-lookup"><span data-stu-id="7a228-155">On the **Process** tab, in the **Build process file** dropdown list, leave **DefaultTemplate.xaml** selected.</span></span> <span data-ttu-id="7a228-156">これは、すべての新しいチーム プロジェクトに追加される既定のビルド プロセス テンプレートのいずれかです。</span><span class="sxs-lookup"><span data-stu-id="7a228-156">This is one of the default build process templates that get added to all new team projects.</span></span>
6. <span data-ttu-id="7a228-157">**ビルド プロセス パラメーター**テーブルをクリックして、**ビルドする項目**行をクリックして、**省略記号**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a228-157">In the **Build process parameters** table, click in the **Items to Build** row, and then click the **ellipsis** button.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. <span data-ttu-id="7a228-158">**ビルドする項目**ダイアログ ボックスで、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="7a228-158">In the **Items to Build** dialog box, click **Add**.</span></span>
8. <span data-ttu-id="7a228-159">ソリューション ファイルの場所を参照し、クリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="7a228-159">Browse to the location of your solution file, and then click **OK**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. <span data-ttu-id="7a228-160">**ビルドする項目**ダイアログ ボックスで、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="7a228-160">In the **Items to Build** dialog box, click **Add**.</span></span>
10. <span data-ttu-id="7a228-161">**アイテムの種類**ドロップダウン リストで、 **MSBuild プロジェクト ファイル**します。</span><span class="sxs-lookup"><span data-stu-id="7a228-161">In the **Items of type** dropdown list, select **MSBuild Project files**.</span></span>
11. <span data-ttu-id="7a228-162">これで、展開プロセスを制御、ファイルを選択をクリックしたカスタムのプロジェクト ファイルの場所を参照**OK**します。</span><span class="sxs-lookup"><span data-stu-id="7a228-162">Browse to the location of the custom project file with which you control the deployment process, select the file, and then click **OK**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. <span data-ttu-id="7a228-163">**ビルドする項目** ダイアログ ボックスでは 2 つの項目が表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="7a228-163">The **Items to Build** dialog box should now show two items.</span></span> <span data-ttu-id="7a228-164">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a228-164">Click **OK**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. <span data-ttu-id="7a228-165">**プロセス** タブで、**ビルド プロセス パラメーター**テーブルで、展開、**詳細**セクション。</span><span class="sxs-lookup"><span data-stu-id="7a228-165">On the **Process** tab, in the **Build process parameters** table, expand the **Advanced** section.</span></span>
14. <span data-ttu-id="7a228-166">**MSBuild 引数**行で、MSBuild コマンドライン引数を追加する*か*項目を構築するが必要です。</span><span class="sxs-lookup"><span data-stu-id="7a228-166">In the **MSBuild Arguments** row, add any MSBuild command-line arguments that *either* of your items to build requires.</span></span> <span data-ttu-id="7a228-167">連絡先マネージャー ソリューションでは、これらの引数が必要です。</span><span class="sxs-lookup"><span data-stu-id="7a228-167">In the Contact Manager solution scenario, these arguments are required:</span></span>

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. <span data-ttu-id="7a228-168">この例では、次のように記述されています。</span><span class="sxs-lookup"><span data-stu-id="7a228-168">In this example:</span></span>

    1. <span data-ttu-id="7a228-169">**DeployOnBuild = true**と**DeployTarget パッケージ =** 連絡先マネージャー ソリューションをビルドするときに、引数は必須です。</span><span class="sxs-lookup"><span data-stu-id="7a228-169">The **DeployOnBuild=true** and **DeployTarget=package** arguments are required when you build the Contact Manager solution.</span></span> <span data-ttu-id="7a228-170">これにより、web 配置パッケージ各 web アプリケーション プロジェクトのビルド後の説明に従って作成するために MSBuild[のビルドとパッケージ化 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)します。</span><span class="sxs-lookup"><span data-stu-id="7a228-170">This instructs MSBuild to create web deployment packages after building each web application project, as described in [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span>
    2. <span data-ttu-id="7a228-171">**TargetEnvPropsFile**をビルドするときは、引数が必要です、 *Publish.proj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7a228-171">The **TargetEnvPropsFile** argument is required when you build the *Publish.proj* file.</span></span> <span data-ttu-id="7a228-172">」の説明に従って、このプロパティは、環境固有の構成ファイルの場所を示します[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。</span><span class="sxs-lookup"><span data-stu-id="7a228-172">This property indicates the location of the environment-specific configuration file, as described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>
16. <span data-ttu-id="7a228-173">**リテンション期間ポリシー**  タブで、必要に応じて保持する各種類のビルドの数を構成します。</span><span class="sxs-lookup"><span data-stu-id="7a228-173">On the **Retention Policy** tab, configure how many builds of each type you want to retain as required.</span></span>
17. <span data-ttu-id="7a228-174">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a228-174">Click **Save**.</span></span>

## <a name="queue-a-build"></a><span data-ttu-id="7a228-175">ビルドをキューに配置する</span><span class="sxs-lookup"><span data-stu-id="7a228-175">Queue a Build</span></span>

<span data-ttu-id="7a228-176">この時点では、少なくとも 1 つの新しいビルド定義を作成しました。</span><span class="sxs-lookup"><span data-stu-id="7a228-176">At this point, you have created at least one new build definition.</span></span> <span data-ttu-id="7a228-177">ビルド プロセスを定義したビルド定義で指定したトリガーに応じて実行されます。</span><span class="sxs-lookup"><span data-stu-id="7a228-177">The build process you defined will now run according to the triggers you specified in the build definition.</span></span>

<span data-ttu-id="7a228-178">CI を使用するビルド定義を構成した場合は、2 つの方法で、ビルド定義をテストできます。</span><span class="sxs-lookup"><span data-stu-id="7a228-178">If you've configured your build definition to use CI, you can test your build definition in two ways:</span></span>

- <span data-ttu-id="7a228-179">一部のコンテンツを自動ビルドをトリガーするチーム プロジェクトをチェックインします。</span><span class="sxs-lookup"><span data-stu-id="7a228-179">Check in some content to the team project to trigger an automatic build.</span></span>
- <span data-ttu-id="7a228-180">ビルドを手動でキューします。</span><span class="sxs-lookup"><span data-stu-id="7a228-180">Queue a build manually.</span></span>

<span data-ttu-id="7a228-181">**手動でビルドをキューに**</span><span class="sxs-lookup"><span data-stu-id="7a228-181">**To queue a build manually**</span></span>

1. <span data-ttu-id="7a228-182">**チーム エクスプ ローラー**ウィンドウでは、ビルド定義を右クリックし、順にクリックします**新しいビルドをキュー**します。</span><span class="sxs-lookup"><span data-stu-id="7a228-182">In the **Team Explorer** window, right-click the build definition, and then click **Queue New Build**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. <span data-ttu-id="7a228-183">**ビルドをキュー**ダイアログ ボックスが、ビルド プロパティを確認し、クリックして**キュー**します。</span><span class="sxs-lookup"><span data-stu-id="7a228-183">In the **Queue Build** dialog box, review the build properties, and then click **Queue**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

<span data-ttu-id="7a228-184">進行状況とをビルドの結果を確認する&#x2014;かどうか手動または自動でトリガーされたに関係なく&#x2014;でビルド定義をダブルクリックして、**チーム エクスプ ローラー**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="7a228-184">To review the progress and the outcome of a build&#x2014;regardless of whether it was triggered manually or automatically&#x2014;double-click the build definition in the **Team Explorer** window.</span></span> <span data-ttu-id="7a228-185">開き、**ビルド エクスプ ローラー**タブ。</span><span class="sxs-lookup"><span data-stu-id="7a228-185">This will open a **Build Explorer** tab.</span></span>

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

<span data-ttu-id="7a228-186">ここでは、失敗したビルドをトラブルシューティングすることができます。</span><span class="sxs-lookup"><span data-stu-id="7a228-186">From here, you can troubleshoot failed builds.</span></span> <span data-ttu-id="7a228-187">個々 のビルドをダブルクリックする場合は、概要情報を表示しをクリックして詳細なログ ファイル。</span><span class="sxs-lookup"><span data-stu-id="7a228-187">If you double-click an individual build, you can view summary information and click through to detailed log files.</span></span>

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

<span data-ttu-id="7a228-188">この情報を使用して、失敗したビルドのトラブルシューティングを行うし、別のビルドを試行する前に問題に対処することができます。</span><span class="sxs-lookup"><span data-stu-id="7a228-188">You can use this information to troubleshoot failed builds and address any problems before you attempt another build.</span></span>

> [!NOTE]
> <span data-ttu-id="7a228-189">デプロイ ロジックを実行するビルドは、ビルド サーバー先の環境に必要なすべてのアクセス許可を付与が失敗する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="7a228-189">Builds that execute deployment logic are likely to fail until you have granted the build server any permissions required in the destination environment.</span></span> <span data-ttu-id="7a228-190">詳細については、次を参照してください。[チーム ビルド展開のアクセス許可を構成する](configuring-permissions-for-team-build-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="7a228-190">For more information, see [Configuring Permissions for Team Build Deployment](configuring-permissions-for-team-build-deployment.md).</span></span>


## <a name="monitor-the-build-process"></a><span data-ttu-id="7a228-191">ビルド プロセスを監視します。</span><span class="sxs-lookup"><span data-stu-id="7a228-191">Monitor the Build Process</span></span>

<span data-ttu-id="7a228-192">TFS では、さまざまなビルド プロセスを監視するのに役立つ機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="7a228-192">TFS provides a broad range of functionality to help you monitor the build process.</span></span> <span data-ttu-id="7a228-193">たとえば、TFS は、電子メールを送信またはビルドが完了したら、タスク バーの通知領域にアラートを表示できます。</span><span class="sxs-lookup"><span data-stu-id="7a228-193">For example, TFS can send you an email or display alerts in your taskbar notification area when a build has completed.</span></span> <span data-ttu-id="7a228-194">詳細については、次を参照してください。[実行とビルドの監視](https://msdn.microsoft.com/library/ms181721.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7a228-194">For more information, see [Run and Monitor Builds](https://msdn.microsoft.com/library/ms181721.aspx).</span></span>

## <a name="conclusion"></a><span data-ttu-id="7a228-195">まとめ</span><span class="sxs-lookup"><span data-stu-id="7a228-195">Conclusion</span></span>

<span data-ttu-id="7a228-196">このトピックでは、TFS でビルド定義を作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="7a228-196">This topic described how to create a build definition in TFS.</span></span> <span data-ttu-id="7a228-197">ビルド定義は、開発者がチーム プロジェクトにコンテンツをチェックインするたびに、ビルド プロセスが実行されるように、CI、構成されます。</span><span class="sxs-lookup"><span data-stu-id="7a228-197">The build definition is configured for CI, so the build process runs whenever a developer checks in content to the team project.</span></span> <span data-ttu-id="7a228-198">ビルド定義は、ターゲットのサーバー環境に web パッケージとデータベースのスクリプトを展開するカスタム MSBuild プロジェクト ファイルを実行します。</span><span class="sxs-lookup"><span data-stu-id="7a228-198">The build definition executes a custom MSBuild project file to deploy web packages and database scripts to a target server environment.</span></span>

<span data-ttu-id="7a228-199">自動化された展開のビルド プロセスの一部として成功するためには、ターゲット web サーバーとターゲットのデータベース サーバーのビルド サービス アカウントに適切なアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7a228-199">In order for an automated deployment to succeed as part of a build process, you'll need to grant appropriate permissions to the build service account on the target web servers and the target database server.</span></span> <span data-ttu-id="7a228-200">このチュートリアルでは、最後のトピック[チーム ビルド展開のアクセス許可を構成する](configuring-permissions-for-team-build-deployment.md)、識別し、チーム ビルド サーバーからの自動展開に必要なアクセス許可を構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="7a228-200">The final topic in this tutorial, [Configuring Permissions for Team Build Deployment](configuring-permissions-for-team-build-deployment.md), describes how to identify and configure the permissions required for automated deployment from a Team Build server.</span></span>

## <a name="further-reading"></a><span data-ttu-id="7a228-201">関連項目</span><span class="sxs-lookup"><span data-stu-id="7a228-201">Further Reading</span></span>

<span data-ttu-id="7a228-202">ビルド定義を作成する方法の詳細については、次を参照してください。[基本的なビルド定義を作成する](https://msdn.microsoft.com/library/ms181716.aspx)と[ビルド プロセスの定義](https://msdn.microsoft.com/library/ms181715.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7a228-202">For more information on creating build definitions, see [Create a Basic Build Definition](https://msdn.microsoft.com/library/ms181716.aspx) and [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx).</span></span> <span data-ttu-id="7a228-203">キュー ビルドの詳細については、次を参照してください。[ビルドをキュー](https://msdn.microsoft.com/library/ms181722.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7a228-203">For more guidance on queuing builds, see [Queue a Build](https://msdn.microsoft.com/library/ms181722.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7a228-204">[前へ](configuring-a-tfs-build-server-for-web-deployment.md)
> [次へ](deploying-a-specific-build.md)</span><span class="sxs-lookup"><span data-stu-id="7a228-204">[Previous](configuring-a-tfs-build-server-for-web-deployment.md)
[Next](deploying-a-specific-build.md)</span></span>
