---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: 展開をサポートするビルド定義を作成する |Microsoft ドキュメント
author: jrjlee
description: どのようなビルドの Team Foundation Server (TFS) 2010 を実行する場合は、チーム プロジェクト内でビルド定義を作成する必要があります。 このトピック des しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: c5ea0bd9f01bb57b96abd349741f304c0093d887
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="creating-a-build-definition-that-supports-deployment"></a><span data-ttu-id="1583b-104">展開をサポートするビルド定義を作成します。</span><span class="sxs-lookup"><span data-stu-id="1583b-104">Creating a Build Definition That Supports Deployment</span></span>
====================
<span data-ttu-id="1583b-105">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="1583b-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="1583b-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="1583b-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="1583b-107">どのようなビルドの Team Foundation Server (TFS) 2010 を実行する場合は、チーム プロジェクト内でビルド定義を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1583b-107">If you want to perform any kind of build in Team Foundation Server (TFS) 2010, you need to create a build definition within your team project.</span></span> <span data-ttu-id="1583b-108">このトピックでは、TFS で、新しいビルド定義を作成する方法と、チーム ビルドでのビルド プロセスの一部として web 配置を制御する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1583b-108">This topic describes how to create a new build definition in TFS and how to control web deployment as part of the build process in Team Build.</span></span>


<span data-ttu-id="1583b-109">このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="1583b-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="1583b-110">説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によってビルドおよび配置プロセスが制御されるは、2 つのプロジェクト ファイル&#x2014;いずれか各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用されるビルドの手順を含むです。</span><span class="sxs-lookup"><span data-stu-id="1583b-110">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="1583b-111">ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="1583b-111">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="1583b-112">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="1583b-112">Task Overview</span></span>

<span data-ttu-id="1583b-113">ビルド定義は、TFS のチーム プロジェクトのビルドで発生する方法とタイミングを制御するメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="1583b-113">A build definition is the mechanism that controls how and when builds occur for team projects in TFS.</span></span> <span data-ttu-id="1583b-114">各ビルド定義を指定します。</span><span class="sxs-lookup"><span data-stu-id="1583b-114">Each build definition specifies:</span></span>

- <span data-ttu-id="1583b-115">Visual Studio ソリューション ファイルまたはカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルと同様に、ビルドすることです。</span><span class="sxs-lookup"><span data-stu-id="1583b-115">The things you want to build, like Visual Studio solution files or custom Microsoft Build Engine (MSBuild) project files.</span></span>
- <span data-ttu-id="1583b-116">ビルドが実行時に決定する条件は、トリガーは手動、継続的インテグレーション (CI) のように配置します。 または、ゲート チェックインします。</span><span class="sxs-lookup"><span data-stu-id="1583b-116">The criteria that determine when a build should take place, like manual triggers, continuous integration (CI), or gated check-ins.</span></span>
- <span data-ttu-id="1583b-117">チーム ビルドが送信する web のパッケージとデータベースのスクリプトと同様に展開アーティファクトを含めて、ビルド出力の場所です。</span><span class="sxs-lookup"><span data-stu-id="1583b-117">The location to which Team Build should send build outputs, including deployment artifacts like web packages and database scripts.</span></span>
- <span data-ttu-id="1583b-118">各ビルドを保持する時間数。</span><span class="sxs-lookup"><span data-stu-id="1583b-118">The amount of time that each build should be retained.</span></span>
- <span data-ttu-id="1583b-119">ビルド プロセスのさまざまな他のパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="1583b-119">Various other parameters of the build process.</span></span>

> [!NOTE]
> <span data-ttu-id="1583b-120">ビルド定義の詳細については、次を参照してください。[ビルド プロセスの定義](https://msdn.microsoft.com/library/ms181715.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="1583b-120">For more information on build definitions, see [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx).</span></span>


<span data-ttu-id="1583b-121">このトピックでは、開発者が新しいコンテンツをチェックインする際に、ビルドがトリガーされるように、構成項目を使用するビルド定義を作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1583b-121">This topic will show you how to create a build definition that uses CI, so that a build is triggered when a developer checks in new content.</span></span> <span data-ttu-id="1583b-122">ビルドが成功すると、ビルド サービスはテスト環境にソリューションを配置するカスタム プロジェクト ファイルを実行します。</span><span class="sxs-lookup"><span data-stu-id="1583b-122">If the build succeeds, the build service runs a custom project file to deploy the solution to a test environment.</span></span>

<span data-ttu-id="1583b-123">ビルドをトリガーするときに、これらの操作を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="1583b-123">When you trigger a build, these actions need to happen:</span></span>

- <span data-ttu-id="1583b-124">最初に、チーム ビルドは、ソリューションをビルドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1583b-124">First, Team Build should build the solution.</span></span> <span data-ttu-id="1583b-125">このプロセスの一環としては、チーム ビルドすると、web アプリケーション プロジェクト、ソリューション内の各 web 展開パッケージを生成する Web 発行パイプライン (WPP) が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="1583b-125">As part of this process, Team Build will invoke the Web Publishing Pipeline (WPP) to generate web deployment packages for each of the web application projects in the solution.</span></span> <span data-ttu-id="1583b-126">チーム ビルドでは、ソリューションに関連付けられている他の単体テストも実行されます。</span><span class="sxs-lookup"><span data-stu-id="1583b-126">Team Build will also run any unit tests associated with the solution.</span></span>
- <span data-ttu-id="1583b-127">ソリューションのビルドが失敗した場合、チームを構築する必要があります操作を行わないさらにします。</span><span class="sxs-lookup"><span data-stu-id="1583b-127">If the solution build fails, Team Build should take no further action.</span></span> <span data-ttu-id="1583b-128">単体テストの失敗は、ビルド エラーとして処理されます。</span><span class="sxs-lookup"><span data-stu-id="1583b-128">Unit test failures should be treated as a build failure.</span></span>
- <span data-ttu-id="1583b-129">ソリューションのビルドが成功した場合は、チーム ビルドとソリューションの展開を制御するカスタム プロジェクト ファイルを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1583b-129">If the solution build succeeds, Team Build should run the custom project file that controls the deployment of the solution.</span></span> <span data-ttu-id="1583b-130">このプロセスの一環として、チーム ビルドと、インターネット インフォメーション サービス (IIS) Web 配置ツールが移行先の web サーバーにパッケージ化された web アプリケーションをインストールする (Web 配置) を呼び出すし、データベースの作成を実行する VSDBCMD.exe ユーティリティが実行されます。移行先のデータベース サーバーでスクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="1583b-130">As part of this process, Team Build will invoke the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to install the packaged web applications on the destination web servers, and it will invoke the VSDBCMD.exe utility to run database creation scripts on the destination database servers.</span></span>

<span data-ttu-id="1583b-131">これは、プロセスを示しています。</span><span class="sxs-lookup"><span data-stu-id="1583b-131">This illustrates the process:</span></span>

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

<span data-ttu-id="1583b-132">[連絡先のマネージャー](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューションには、カスタムの MSBuild プロジェクト ファイルが含まれています。 *Publish.proj*、MSBuild またはチーム ビルドから実行することができます。</span><span class="sxs-lookup"><span data-stu-id="1583b-132">The [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution includes a custom MSBuild project file, *Publish.proj*, that you can run from MSBuild or Team Build.</span></span> <span data-ttu-id="1583b-133">」の説明に従って[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)、このプロジェクト ファイルは、web のパッケージとデータベースを対象となる環境に展開するロジックを定義します。</span><span class="sxs-lookup"><span data-stu-id="1583b-133">As described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md), this project file defines the logic that deploys your web packages and databases to a target environment.</span></span> <span data-ttu-id="1583b-134">ファイルには、チーム ビルド、配置タスクを実行するだけのままで実行されている場合に、ビルとパッケージ化プロセスを除外するロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1583b-134">The file includes logic that omits the building and packaging process if it's running in Team Build, leaving just the deployment tasks to run.</span></span> <span data-ttu-id="1583b-135">この方法で展開を自動化するときに通常たいソリューションが正常に構築できるようにするため、展開プロセスを開始する前に、他の単体テストを渡します。</span><span class="sxs-lookup"><span data-stu-id="1583b-135">This is because when you automate deployment in this way, you'll typically want to ensure that the solution builds successfully and passes any unit tests before the deployment process commences.</span></span>

<span data-ttu-id="1583b-136">次のセクションでは、新しいビルド定義を作成することでこのプロセスを実装する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1583b-136">The next section explains how to implement this process by creating a new build definition.</span></span>

> [!NOTE]
> <span data-ttu-id="1583b-137">この手順&#x2014;、1 つが自動でプロセスをビルド、テスト、およびソリューションを配置&#x2014;テスト環境への展開に最も合ったする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1583b-137">This procedure&#x2014;in which a single automated process builds, tests, and deploys a solution&#x2014;is likely to be most suited to deployment to test environments.</span></span> <span data-ttu-id="1583b-138">ステージングと運用環境の場合よりもずっと既にことを確認して、テスト環境で検証される前のビルドからのコンテンツを展開します。</span><span class="sxs-lookup"><span data-stu-id="1583b-138">For staging and production environments you're a lot more likely to want to deploy content from a previous build that you've already verified and validated in a test environment.</span></span> <span data-ttu-id="1583b-139">この方法は、次のトピックに記載されて[特定のビルドを配置](deploying-a-specific-build.md)です。</span><span class="sxs-lookup"><span data-stu-id="1583b-139">This approach is described in the next topic, [Deploying a Specific Build](deploying-a-specific-build.md).</span></span>


### <a name="who-performs-this-procedure"></a><span data-ttu-id="1583b-140">ユーザーが、この手順を実行しますか。</span><span class="sxs-lookup"><span data-stu-id="1583b-140">Who Performs This Procedure?</span></span>

<span data-ttu-id="1583b-141">通常は、TFS 管理者は、この手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="1583b-141">Typically, a TFS administrator performs this procedure.</span></span> <span data-ttu-id="1583b-142">場合によっては、開発者チーム リーダーは、TFS でチーム プロジェクト コレクションに対して責任を負う場合があります。</span><span class="sxs-lookup"><span data-stu-id="1583b-142">In some cases, a developer team leader may take responsibility for the team project collection in TFS.</span></span> <span data-ttu-id="1583b-143">新しいビルド定義を作成するためには、メンバーである必要があります、**プロジェクト コレクション ビルド管理者**ソリューションを含むチーム プロジェクト コレクションにグループ化します。</span><span class="sxs-lookup"><span data-stu-id="1583b-143">In order to create a new build definition, you need to be a member of the **Project Collection Build Administrators** group for the team project collection that contains your solution.</span></span>

## <a name="create-a-build-definition-for-ci-and-deployment"></a><span data-ttu-id="1583b-144">CI と配置のビルド定義を作成します。</span><span class="sxs-lookup"><span data-stu-id="1583b-144">Create a Build Definition for CI and Deployment</span></span>

<span data-ttu-id="1583b-145">次の手順では、CI をトリガーするビルド定義を作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1583b-145">The next procedure describes how to create a build definition that CI triggers.</span></span> <span data-ttu-id="1583b-146">ビルドが成功すると、カスタム MSBuild プロジェクト ファイルで、ロジックを使用して、ソリューションが配置します。</span><span class="sxs-lookup"><span data-stu-id="1583b-146">If the build succeeds, the solution is deployed using the logic in a custom MSBuild project file.</span></span>

<span data-ttu-id="1583b-147">**CI と配置のビルド定義を作成するには**</span><span class="sxs-lookup"><span data-stu-id="1583b-147">**To create a build definition for CI and deployment**</span></span>

1. <span data-ttu-id="1583b-148">Visual Studio 2010 での**チーム エクスプ ローラー**ウィンドウで、チーム プロジェクト ノードを展開しを右クリックして**ビルド**、順にクリック**ビルド定義の新規**です。</span><span class="sxs-lookup"><span data-stu-id="1583b-148">In Visual Studio 2010, in the **Team Explorer** window, expand your team project node, right-click **Builds**, and then click **New Build Definition**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. <span data-ttu-id="1583b-149">**全般**] タブで、ビルド定義の名前 (たとえば、 **DeployToTest**) とオプションの説明。</span><span class="sxs-lookup"><span data-stu-id="1583b-149">On the **General** tab, give the build definition a name (for example, **DeployToTest**) and an optional description.</span></span>
3. <span data-ttu-id="1583b-150">**トリガー** ] タブで、新しいビルドをトリガーする条件を選択します。</span><span class="sxs-lookup"><span data-stu-id="1583b-150">On the **Trigger** tab, select the criteria on which you want to trigger a new build.</span></span> <span data-ttu-id="1583b-151">たとえば、次のようにソリューションをビルドし、開発者は新しいコードをチェックインするたびに、テスト環境にデプロイする場合、次のように選択します。**継続的インテグレーション**です。</span><span class="sxs-lookup"><span data-stu-id="1583b-151">For example, if you want to build the solution and deploy to the test environment every time a developer checks in new code, select **Continuous Integration**.</span></span>
4. <span data-ttu-id="1583b-152">**ビルドの既定値**] タブで、**ビルド出力を次の格納フォルダーにコピー**ボックスで、ドロップ フォルダーの汎用名前付け規則 (UNC) パスを入力 (たとえば、 ** \\TFSBUILD\Drops**)。</span><span class="sxs-lookup"><span data-stu-id="1583b-152">On the **Build Defaults** tab, in the **Copy build output to the following drop folder** box, type the Universal Naming Convention (UNC) path of your drop folder (for example, **\\TFSBUILD\Drops**).</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > <span data-ttu-id="1583b-153">この格納場所には、ポリシーによっては、保有期間を構成する複数のビルドが格納されます。</span><span class="sxs-lookup"><span data-stu-id="1583b-153">This drop location stores several builds, depending on the retention policy you configure.</span></span> <span data-ttu-id="1583b-154">ステージングまたは運用環境に、特定のビルドからの展開アイテムをパブリッシュする場合は、これは、それらを検索します。</span><span class="sxs-lookup"><span data-stu-id="1583b-154">When you want to publish deployment artifacts from a specific build to a staging or production environment, this is where you'll find them.</span></span>
5. <span data-ttu-id="1583b-155">**プロセス**] タブの [、**ビルド プロセス ファイル**ドロップダウン リストのままにして**DefaultTemplate.xaml**選択します。</span><span class="sxs-lookup"><span data-stu-id="1583b-155">On the **Process** tab, in the **Build process file** dropdown list, leave **DefaultTemplate.xaml** selected.</span></span> <span data-ttu-id="1583b-156">これは、すべての新しいチーム プロジェクトに追加される既定のビルド プロセス テンプレートのいずれかです。</span><span class="sxs-lookup"><span data-stu-id="1583b-156">This is one of the default build process templates that get added to all new team projects.</span></span>
6. <span data-ttu-id="1583b-157">**ビルド プロセス パラメーター**テーブルで、をクリックして、**ビルドする項目**行をクリックして、**省略記号**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1583b-157">In the **Build process parameters** table, click in the **Items to Build** row, and then click the **ellipsis** button.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. <span data-ttu-id="1583b-158">**ビルドする項目**ダイアログ ボックスで、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="1583b-158">In the **Items to Build** dialog box, click **Add**.</span></span>
8. <span data-ttu-id="1583b-159">ソリューション ファイルの場所を参照し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="1583b-159">Browse to the location of your solution file, and then click **OK**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. <span data-ttu-id="1583b-160">**ビルドする項目**ダイアログ ボックスで、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="1583b-160">In the **Items to Build** dialog box, click **Add**.</span></span>
10. <span data-ttu-id="1583b-161">**型の項目を**ドロップダウン リストで、 **MSBuild プロジェクト ファイル**です。</span><span class="sxs-lookup"><span data-stu-id="1583b-161">In the **Items of type** dropdown list, select **MSBuild Project files**.</span></span>
11. <span data-ttu-id="1583b-162">使用する展開プロセスを制御、ファイルを選択してをクリックして、カスタムのプロジェクト ファイルの場所を参照**OK**です。</span><span class="sxs-lookup"><span data-stu-id="1583b-162">Browse to the location of the custom project file with which you control the deployment process, select the file, and then click **OK**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. <span data-ttu-id="1583b-163">**ビルドする項目**] ダイアログ ボックスでは 2 つの項目が表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="1583b-163">The **Items to Build** dialog box should now show two items.</span></span> <span data-ttu-id="1583b-164">**[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1583b-164">Click **OK**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. <span data-ttu-id="1583b-165">**プロセス**] タブの [、**ビルド プロセス パラメーター**テーブルで、展開、**詳細**セクションです。</span><span class="sxs-lookup"><span data-stu-id="1583b-165">On the **Process** tab, in the **Build process parameters** table, expand the **Advanced** section.</span></span>
14. <span data-ttu-id="1583b-166">**の MSBuild 引数**行で、MSBuild コマンドライン引数を追加する*か*アイテムを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1583b-166">In the **MSBuild Arguments** row, add any MSBuild command-line arguments that *either* of your items to build requires.</span></span> <span data-ttu-id="1583b-167">連絡先のマネージャーのソリューション シナリオでは、これらの引数が必要です。</span><span class="sxs-lookup"><span data-stu-id="1583b-167">In the Contact Manager solution scenario, these arguments are required:</span></span>

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. <span data-ttu-id="1583b-168">この例では、次のように記述されています。</span><span class="sxs-lookup"><span data-stu-id="1583b-168">In this example:</span></span>

    1. <span data-ttu-id="1583b-169">**DeployOnBuild = true**と**DeployTarget パッケージを =** Contact Manager ソリューションをビルドするときに、引数が必要です。</span><span class="sxs-lookup"><span data-stu-id="1583b-169">The **DeployOnBuild=true** and **DeployTarget=package** arguments are required when you build the Contact Manager solution.</span></span> <span data-ttu-id="1583b-170">これにより、web 配置パッケージ各 web アプリケーション プロジェクトのビルド後の説明に従って作成するために MSBuild[パッケージ Web アプリケーション プロジェクトのビルドと](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)です。</span><span class="sxs-lookup"><span data-stu-id="1583b-170">This instructs MSBuild to create web deployment packages after building each web application project, as described in [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span>
    2. <span data-ttu-id="1583b-171">**TargetEnvPropsFile**をビルドすると、引数が必要、 *Publish.proj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="1583b-171">The **TargetEnvPropsFile** argument is required when you build the *Publish.proj* file.</span></span> <span data-ttu-id="1583b-172">」の説明に従って、このプロパティは、環境固有の構成ファイルの場所を示します[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。</span><span class="sxs-lookup"><span data-stu-id="1583b-172">This property indicates the location of the environment-specific configuration file, as described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>
16. <span data-ttu-id="1583b-173">**保有ポリシー** ] タブで、必要に応じてを保持する各種類のビルドの数を構成します。</span><span class="sxs-lookup"><span data-stu-id="1583b-173">On the **Retention Policy** tab, configure how many builds of each type you want to retain as required.</span></span>
17. <span data-ttu-id="1583b-174">**[保存]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1583b-174">Click **Save**.</span></span>

## <a name="queue-a-build"></a><span data-ttu-id="1583b-175">ビルドをキューに配置する</span><span class="sxs-lookup"><span data-stu-id="1583b-175">Queue a Build</span></span>

<span data-ttu-id="1583b-176">この時点では、少なくとも 1 つの新しいビルド定義を作成しました。</span><span class="sxs-lookup"><span data-stu-id="1583b-176">At this point, you have created at least one new build definition.</span></span> <span data-ttu-id="1583b-177">ビルド定義で指定したトリガーに従って定義した、ビルド プロセスが実行されます。</span><span class="sxs-lookup"><span data-stu-id="1583b-177">The build process you defined will now run according to the triggers you specified in the build definition.</span></span>

<span data-ttu-id="1583b-178">構成項目を使用するビルド定義を構成する場合は、2 つの方法で、ビルド定義をテストできます。</span><span class="sxs-lookup"><span data-stu-id="1583b-178">If you've configured your build definition to use CI, you can test your build definition in two ways:</span></span>

- <span data-ttu-id="1583b-179">自動ビルドをトリガーするチーム プロジェクトにいくつかのコンテンツで確認してください。</span><span class="sxs-lookup"><span data-stu-id="1583b-179">Check in some content to the team project to trigger an automatic build.</span></span>
- <span data-ttu-id="1583b-180">ビルドを手動でキューします。</span><span class="sxs-lookup"><span data-stu-id="1583b-180">Queue a build manually.</span></span>

<span data-ttu-id="1583b-181">**手動でビルドをキューに登録**</span><span class="sxs-lookup"><span data-stu-id="1583b-181">**To queue a build manually**</span></span>

1. <span data-ttu-id="1583b-182">**チーム エクスプ ローラー** ] ウィンドウでは、ビルド定義を右クリックし、をクリックして**新しいビルドをキュー**です。</span><span class="sxs-lookup"><span data-stu-id="1583b-182">In the **Team Explorer** window, right-click the build definition, and then click **Queue New Build**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. <span data-ttu-id="1583b-183">**ビルドをキュー**ダイアログ ボックスでは、ビルド プロパティを確認し、をクリックして**キュー**です。</span><span class="sxs-lookup"><span data-stu-id="1583b-183">In the **Queue Build** dialog box, review the build properties, and then click **Queue**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

<span data-ttu-id="1583b-184">進行状況とをビルドの結果を確認する&#x2014;手動または自動をトリガーにするかどうかに関係なく&#x2014;内のビルド定義をダブルクリックして、**チーム エクスプ ローラー**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="1583b-184">To review the progress and the outcome of a build&#x2014;regardless of whether it was triggered manually or automatically&#x2014;double-click the build definition in the **Team Explorer** window.</span></span> <span data-ttu-id="1583b-185">これが開き、**ビルド エクスプ ローラー**タブです。</span><span class="sxs-lookup"><span data-stu-id="1583b-185">This will open a **Build Explorer** tab.</span></span>

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

<span data-ttu-id="1583b-186">ここでは、ビルドの失敗をトラブルシューティングすることができます。</span><span class="sxs-lookup"><span data-stu-id="1583b-186">From here, you can troubleshoot failed builds.</span></span> <span data-ttu-id="1583b-187">個々 のビルドをダブルクリックすると場合、は、概要情報を表示しをクリックして詳細なログ ファイルにできます。</span><span class="sxs-lookup"><span data-stu-id="1583b-187">If you double-click an individual build, you can view summary information and click through to detailed log files.</span></span>

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

<span data-ttu-id="1583b-188">この情報を使用して、ビルドの失敗のトラブルシューティングを行うし、別のビルドを試行する前に問題に対処することができます。</span><span class="sxs-lookup"><span data-stu-id="1583b-188">You can use this information to troubleshoot failed builds and address any problems before you attempt another build.</span></span>

> [!NOTE]
> <span data-ttu-id="1583b-189">デプロイ ロジックを実行するビルドは、権限がビルド サーバーの展開先の環境で必要になるまでに失敗する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1583b-189">Builds that execute deployment logic are likely to fail until you have granted the build server any permissions required in the destination environment.</span></span> <span data-ttu-id="1583b-190">詳細については、次を参照してください。[チーム ビルドの配置用のアクセス許可を構成する](configuring-permissions-for-team-build-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="1583b-190">For more information, see [Configuring Permissions for Team Build Deployment](configuring-permissions-for-team-build-deployment.md).</span></span>


## <a name="monitor-the-build-process"></a><span data-ttu-id="1583b-191">ビルド プロセスを監視します。</span><span class="sxs-lookup"><span data-stu-id="1583b-191">Monitor the Build Process</span></span>

<span data-ttu-id="1583b-192">TFS では、ビルド プロセスの監視に役立つ機能の広範な範囲を提供します。</span><span class="sxs-lookup"><span data-stu-id="1583b-192">TFS provides a broad range of functionality to help you monitor the build process.</span></span> <span data-ttu-id="1583b-193">たとえば、TFS は、電子メールが送信またはビルドが完了したら、タスク バーの通知領域にアラートを表示します。</span><span class="sxs-lookup"><span data-stu-id="1583b-193">For example, TFS can send you an email or display alerts in your taskbar notification area when a build has completed.</span></span> <span data-ttu-id="1583b-194">詳細については、次を参照してください。[実行し、ビルドの監視](https://msdn.microsoft.com/library/ms181721.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="1583b-194">For more information, see [Run and Monitor Builds](https://msdn.microsoft.com/library/ms181721.aspx).</span></span>

## <a name="conclusion"></a><span data-ttu-id="1583b-195">まとめ</span><span class="sxs-lookup"><span data-stu-id="1583b-195">Conclusion</span></span>

<span data-ttu-id="1583b-196">このトピックでは、TFS のビルド定義を作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1583b-196">This topic described how to create a build definition in TFS.</span></span> <span data-ttu-id="1583b-197">ビルド定義は、CI の開発者は、チーム プロジェクトにコンテンツがチェックインされるたびに、ビルド プロセスが実行されるように構成されます。</span><span class="sxs-lookup"><span data-stu-id="1583b-197">The build definition is configured for CI, so the build process runs whenever a developer checks in content to the team project.</span></span> <span data-ttu-id="1583b-198">ビルド定義は、web のパッケージとデータベース スクリプトを対象サーバー環境に展開するカスタム MSBuild プロジェクト ファイルを実行します。</span><span class="sxs-lookup"><span data-stu-id="1583b-198">The build definition executes a custom MSBuild project file to deploy web packages and database scripts to a target server environment.</span></span>

<span data-ttu-id="1583b-199">自動の展開をビルド処理の一部として成功させるためには、ターゲット web サーバーとターゲット データベースのサーバー上のビルド サービス アカウントに適切なアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1583b-199">In order for an automated deployment to succeed as part of a build process, you'll need to grant appropriate permissions to the build service account on the target web servers and the target database server.</span></span> <span data-ttu-id="1583b-200">このチュートリアルの最後のトピック[チーム ビルドの配置用のアクセス許可を構成する](configuring-permissions-for-team-build-deployment.md)、識別し、チーム ビルド サーバーからの自動展開に必要なアクセス許可を構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1583b-200">The final topic in this tutorial, [Configuring Permissions for Team Build Deployment](configuring-permissions-for-team-build-deployment.md), describes how to identify and configure the permissions required for automated deployment from a Team Build server.</span></span>

## <a name="further-reading"></a><span data-ttu-id="1583b-201">関連項目</span><span class="sxs-lookup"><span data-stu-id="1583b-201">Further Reading</span></span>

<span data-ttu-id="1583b-202">ビルド定義を作成する方法の詳細については、次を参照してください。[基本的なビルド定義を作成する](https://msdn.microsoft.com/library/ms181716.aspx)と[ビルド プロセスの定義](https://msdn.microsoft.com/library/ms181715.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="1583b-202">For more information on creating build definitions, see [Create a Basic Build Definition](https://msdn.microsoft.com/library/ms181716.aspx) and [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx).</span></span> <span data-ttu-id="1583b-203">キュー ビルドの詳細については、次を参照してください。[ビルドをキュー](https://msdn.microsoft.com/library/ms181722.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="1583b-203">For more guidance on queuing builds, see [Queue a Build](https://msdn.microsoft.com/library/ms181722.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1583b-204">[前へ](configuring-a-tfs-build-server-for-web-deployment.md)
> [次へ](deploying-a-specific-build.md)</span><span class="sxs-lookup"><span data-stu-id="1583b-204">[Previous](configuring-a-tfs-build-server-for-web-deployment.md)
[Next](deploying-a-specific-build.md)</span></span>
