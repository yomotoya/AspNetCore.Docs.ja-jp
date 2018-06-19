---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: 特定のビルドを展開する |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、web のパッケージとステージングまたは運用環境と同様に、新しい変換先に、特定の以前のビルドからデータベース スクリプトを展開する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 271d084b3c69016df5be28ada032973bf7fd5a49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880038"
---
<a name="deploying-a-specific-build"></a><span data-ttu-id="1662d-103">特定のビルドを展開します。</span><span class="sxs-lookup"><span data-stu-id="1662d-103">Deploying a Specific Build</span></span>
====================
<span data-ttu-id="1662d-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="1662d-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="1662d-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="1662d-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="1662d-106">このトピックでは、web のパッケージと、ステージング環境または運用環境のように、新しい変換先に、特定の以前のビルドからデータベース スクリプトを展開する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1662d-106">This topic describes how to deploy web packages and database scripts from a specific previous build to a new destination, like a staging or production environment.</span></span>


<span data-ttu-id="1662d-107">このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="1662d-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="1662d-108">説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によってビルドおよび配置プロセスが制御されるは、2 つのプロジェクト ファイル&#x2014;いずれか各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用されるビルドの手順を含むです。</span><span class="sxs-lookup"><span data-stu-id="1662d-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="1662d-109">ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="1662d-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="1662d-110">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="1662d-110">Task Overview</span></span>

<span data-ttu-id="1662d-111">これまで、このチュートリアルの一連のトピックがビルド、パッケージ化、および web アプリケーションとデータベースをシングル ステップの一部として展開する方法に重点を置きますか、自動プロセス。</span><span class="sxs-lookup"><span data-stu-id="1662d-111">Until now, the topics in this tutorial set have focused on how to build, package, and deploy web applications and databases as part of a single-step or automated process.</span></span> <span data-ttu-id="1662d-112">ただし、一般的なシナリオでにしておく、ドロップ フォルダーに含めるビルドの一覧からデプロイするリソースを選択します。</span><span class="sxs-lookup"><span data-stu-id="1662d-112">However, in some common scenarios, you'll want to select the resources that you deploy from a list of builds in a drop folder.</span></span> <span data-ttu-id="1662d-113">つまり、最新のビルドには、目的のビルドを配置することがありますできません。</span><span class="sxs-lookup"><span data-stu-id="1662d-113">In other words, the latest build may not be the build you want to deploy.</span></span>

<span data-ttu-id="1662d-114">前のトピックで説明されている継続的インテグレーション (CI) シナリオについて考えてみます[展開を作成するビルド定義をサポートしている](creating-a-build-definition-that-supports-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="1662d-114">Consider the continuous integration (CI) scenario described in the previous topic, [Creating a Build Definition That Supports Deployment](creating-a-build-definition-that-supports-deployment.md).</span></span> <span data-ttu-id="1662d-115">Team Foundation Server (TFS) 2010 のビルド定義を作成しました。</span><span class="sxs-lookup"><span data-stu-id="1662d-115">You've created a build definition in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="1662d-116">たびに、開発者が TFS にコードをチェックイン、チーム ビルドは、コードをビルド、ビルド プロセスの一部として web のパッケージとデータベースのスクリプトを作成、他の単体テストを実行およびテスト環境に、リソースを展開します。</span><span class="sxs-lookup"><span data-stu-id="1662d-116">Every time a developer checks code into TFS, Team Build will build your code, create web packages and database scripts as part of the build process, run any unit tests, and deploy your resources to a test environment.</span></span> <span data-ttu-id="1662d-117">ポリシーによっては、保有期間のビルド定義を作成するときに構成した TFS が前のビルド数を保持します。</span><span class="sxs-lookup"><span data-stu-id="1662d-117">Depending on the retention policy you configured when you created the build definition, TFS will retain a certain number of previous builds.</span></span>

![](deploying-a-specific-build/_static/image1.png)

<span data-ttu-id="1662d-118">ここで、検証を実行したのかと、テスト環境にビルドをこれらのいずれかに対してテスト検証とステージング環境にアプリケーションを配置する準備ができたらをします。</span><span class="sxs-lookup"><span data-stu-id="1662d-118">Now, suppose you've performed verification and validation testing against one of these builds in your test environment, and you're ready to deploy your application to a staging environment.</span></span> <span data-ttu-id="1662d-119">その間は、開発者は、新しいコードでチェックしたことがあります。</span><span class="sxs-lookup"><span data-stu-id="1662d-119">In the meantime, developers may have checked in new code.</span></span> <span data-ttu-id="1662d-120">ソリューションをリビルドして、ステージング環境に展開して、最新のビルドをステージング環境に展開したくないです。</span><span class="sxs-lookup"><span data-stu-id="1662d-120">You don't want to rebuild the solution and deploy to the staging environment, and you don't want to deploy the latest build to the staging environment.</span></span> <span data-ttu-id="1662d-121">代わりに、いることを確認し、テスト サーバー上で検証した特定のビルドを展開します。</span><span class="sxs-lookup"><span data-stu-id="1662d-121">Instead, you want to deploy the specific build that you've verified and validated on the test servers.</span></span>

<span data-ttu-id="1662d-122">これを実現するには、Microsoft Build Engine (MSBuild) の web のパッケージと、特定のビルドが生成されるデータベース スクリプトを検索する場所を指示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1662d-122">To accomplish this, you need to tell the Microsoft Build Engine (MSBuild) where to find the web packages and database scripts that a specific build generated.</span></span>

## <a name="overriding-the-outputroot-property"></a><span data-ttu-id="1662d-123">OutputRoot プロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="1662d-123">Overriding the OutputRoot Property</span></span>

<span data-ttu-id="1662d-124">[サンプル ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)、 *Publish.proj*ファイルという名前のプロパティを宣言して**OutputRoot**です。</span><span class="sxs-lookup"><span data-stu-id="1662d-124">In the [sample solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), the *Publish.proj* file declares a property named **OutputRoot**.</span></span> <span data-ttu-id="1662d-125">名前が示すように、ビルド プロセスを生成するすべてのアイテムを含むルート フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="1662d-125">As the name suggests, this is the root folder that contains everything that the build process generates.</span></span> <span data-ttu-id="1662d-126">*Publish.proj*ファイルを確認できますを**OutputRoot**プロパティはすべての展開のリソースのルートの場所を参照します。</span><span class="sxs-lookup"><span data-stu-id="1662d-126">In the *Publish.proj* file, you can see that the **OutputRoot** property refers to the root location for all deployment resources.</span></span>

> [!NOTE]
> <span data-ttu-id="1662d-127">**OutputRoot**一般的に使用されるプロパティの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="1662d-127">**OutputRoot** is a commonly used property name.</span></span> <span data-ttu-id="1662d-128">Visual c# および Visual Basic のプロジェクト ファイルでは、すべてのビルド出力のルートの場所を格納するには、このプロパティも宣言します。</span><span class="sxs-lookup"><span data-stu-id="1662d-128">Visual C# and Visual Basic project files also declare this property to store the root location for all build outputs.</span></span>


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


<span data-ttu-id="1662d-129">Web パッケージを展開し、データベースを別の場所からスクリプトには、プロジェクト ファイルの場合&#x2014;などの以前の TFS ビルドの出力&#x2014;だけをオーバーライドする必要があります、 **OutputRoot**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="1662d-129">If you want your project file to deploy web packages and database scripts from a different location&#x2014;like the outputs of a previous TFS build&#x2014;you simply need to override the **OutputRoot** property.</span></span> <span data-ttu-id="1662d-130">チーム ビルド サーバーに関連するビルド フォルダーにプロパティの値を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1662d-130">You should set the property value to the relevant build folder on the Team Build server.</span></span> <span data-ttu-id="1662d-131">値を指定する場合は、コマンドラインから MSBuild を実行していた、でした**OutputRoot**コマンドライン引数として。</span><span class="sxs-lookup"><span data-stu-id="1662d-131">If you were running MSBuild from the command line, you could specify a value for **OutputRoot** as a command-line argument:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


<span data-ttu-id="1662d-132">実際には、ただしはもスキップする、**ビルド**ターゲット&#x2014;ビルド出力を使用する予定がない場合、ソリューションのビルド場所はありません。</span><span class="sxs-lookup"><span data-stu-id="1662d-132">In practice, however, you'd also want to skip the **Build** target&#x2014;there's no point in building your solution if you don't plan to use the build outputs.</span></span> <span data-ttu-id="1662d-133">コマンドラインから実行するターゲットを指定して、これを行う可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1662d-133">You could do this by specifying the targets you want to execute from the command line:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


<span data-ttu-id="1662d-134">ただし、ほとんどの場合にしておく TFS ビルド定義に、デプロイ ロジックを構築します。</span><span class="sxs-lookup"><span data-stu-id="1662d-134">However, in most cases, you'll want to build your deployment logic into a TFS build definition.</span></span> <span data-ttu-id="1662d-135">これにより、ユーザーと、**ビルドをキューに**TFS サーバーに接続を Visual Studio のインストールからデプロイを開始する権限です。</span><span class="sxs-lookup"><span data-stu-id="1662d-135">This enables users with the **Queue builds** permission to trigger the deployment from any Visual Studio installation with a connection to the TFS server.</span></span>

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a><span data-ttu-id="1662d-136">ビルド、ビルド定義を展開する専用の作成</span><span class="sxs-lookup"><span data-stu-id="1662d-136">Creating a Build Definition to Deploy Specific Builds</span></span>

<span data-ttu-id="1662d-137">次の手順では、ユーザーが 1 つのコマンドを使用してステージング環境に展開をトリガーにできるビルド定義を作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1662d-137">The next procedure describes how to create a build definition that enables users to trigger deployments to a staging environment with a single command.</span></span>

<span data-ttu-id="1662d-138">この場合、実際に何も作成するビルド定義をたくない&#x2014;する、カスタムのプロジェクト ファイルでデプロイ ロジックを実行するようにします。</span><span class="sxs-lookup"><span data-stu-id="1662d-138">In this case, you don't want the build definition to actually build anything&#x2014;you just want it to execute the deployment logic in your custom project file.</span></span> <span data-ttu-id="1662d-139">*Publish.proj*ファイルにはスキップする条件ロジックが含まれています、**ビルド**ファイルがチーム ビルドで実行されている場合は対象にします。</span><span class="sxs-lookup"><span data-stu-id="1662d-139">The *Publish.proj* file includes conditional logic that skips the **Build** target if the file is running in Team Build.</span></span> <span data-ttu-id="1662d-140">これは、組み込みを評価することによって、 **BuildingInTeamBuild**に自動的に設定されているプロパティ**true**チーム ビルドでは、プロジェクト ファイルを実行する場合。</span><span class="sxs-lookup"><span data-stu-id="1662d-140">It does this by evaluating the built-in **BuildingInTeamBuild** property, which is automatically set to **true** if you run your project file in Team Build.</span></span> <span data-ttu-id="1662d-141">その結果、ビルド処理をスキップし、単に既存のビルドを配置するプロジェクト ファイルを実行できます。</span><span class="sxs-lookup"><span data-stu-id="1662d-141">As a result, you can skip the build process and simply run the project file to deploy an existing build.</span></span>

<span data-ttu-id="1662d-142">**展開を手動でトリガーするビルド定義を作成するには**</span><span class="sxs-lookup"><span data-stu-id="1662d-142">**To create a build definition to trigger deployment manually**</span></span>

1. <span data-ttu-id="1662d-143">Visual Studio 2010 での**チーム エクスプ ローラー**ウィンドウで、チーム プロジェクト ノードを展開しを右クリックして**ビルド**、順にクリック**ビルド定義の新規**です。</span><span class="sxs-lookup"><span data-stu-id="1662d-143">In Visual Studio 2010, in the **Team Explorer** window, expand your team project node, right-click **Builds**, and then click **New Build Definition**.</span></span>

    ![](deploying-a-specific-build/_static/image2.png)
2. <span data-ttu-id="1662d-144">**全般** タブで、ビルド定義の名前 (たとえば、 **DeployToStaging**) とオプションの説明。</span><span class="sxs-lookup"><span data-stu-id="1662d-144">On the **General** tab, give the build definition a name (for example, **DeployToStaging**) and an optional description.</span></span>
3. <span data-ttu-id="1662d-145">**トリガー** ] タブで [**手動-チェックインのビルドをトリガーしない、新しい**です。</span><span class="sxs-lookup"><span data-stu-id="1662d-145">On the **Trigger** tab, select **Manual – Check-ins do not trigger a new build**.</span></span>
4. <span data-ttu-id="1662d-146">**ビルドの既定値** タブで、**ビルド出力を次の格納フォルダーにコピー**ボックスで、ドロップ フォルダーの汎用名前付け規則 (UNC) パスを入力 (たとえば、  **\\TFSBUILD\Drops**)。</span><span class="sxs-lookup"><span data-stu-id="1662d-146">On the **Build Defaults** tab, in the **Copy build output to the following drop folder** box, type the Universal Naming Convention (UNC) path of your drop folder (for example, **\\TFSBUILD\Drops**).</span></span>

    ![](deploying-a-specific-build/_static/image3.png)
5. <span data-ttu-id="1662d-147">**プロセス**] タブの [、**ビルド プロセス ファイル**ドロップダウン リストのままにして**DefaultTemplate.xaml**選択します。</span><span class="sxs-lookup"><span data-stu-id="1662d-147">On the **Process** tab, in the **Build process file** dropdown list, leave **DefaultTemplate.xaml** selected.</span></span> <span data-ttu-id="1662d-148">これは、すべての新しいチーム プロジェクトに追加される既定のビルド プロセス テンプレートのいずれかです。</span><span class="sxs-lookup"><span data-stu-id="1662d-148">This is one of the default build process templates that get added to all new team projects.</span></span>
6. <span data-ttu-id="1662d-149">**ビルド プロセス パラメーター**テーブルで、をクリックして、**ビルドする項目**行をクリックして、**省略記号**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1662d-149">In the **Build process parameters** table, click in the **Items to Build** row, and then click the **ellipsis** button.</span></span>

    ![](deploying-a-specific-build/_static/image4.png)
7. <span data-ttu-id="1662d-150">**ビルドする項目**ダイアログ ボックスで、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="1662d-150">In the **Items to Build** dialog box, click **Add**.</span></span>
8. <span data-ttu-id="1662d-151">**型の項目を**ドロップダウン リストで、 **MSBuild プロジェクト ファイル**です。</span><span class="sxs-lookup"><span data-stu-id="1662d-151">In the **Items of type** dropdown list, select **MSBuild Project files**.</span></span>
9. <span data-ttu-id="1662d-152">使用する展開プロセスを制御、ファイルを選択してをクリックして、カスタムのプロジェクト ファイルの場所を参照**OK**です。</span><span class="sxs-lookup"><span data-stu-id="1662d-152">Browse to the location of the custom project file with which you control the deployment process, select the file, and then click **OK**.</span></span>

    ![](deploying-a-specific-build/_static/image5.png)
10. <span data-ttu-id="1662d-153">**ビルドする項目**ダイアログ ボックスで、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="1662d-153">In the **Items to Build** dialog box, click **OK**.</span></span>
11. <span data-ttu-id="1662d-154">**ビルド プロセス パラメーター**テーブルで、展開、**詳細**セクションです。</span><span class="sxs-lookup"><span data-stu-id="1662d-154">In the **Build process parameters** table, expand the **Advanced** section.</span></span>
12. <span data-ttu-id="1662d-155">**の MSBuild 引数**行、環境固有のプロジェクト ファイルの場所を指定して、ビルド フォルダーの場所を表すプレース ホルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="1662d-155">In the **MSBuild Arguments** row, specify the location of your environment-specific project file and add a placeholder for the location of your build folder:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > <span data-ttu-id="1662d-156">オーバーライドする必要があります、 **OutputRoot**たびにビルドをキューの値します。</span><span class="sxs-lookup"><span data-stu-id="1662d-156">You'll need to override the **OutputRoot** value every time you queue a build.</span></span> <span data-ttu-id="1662d-157">これについては、次の手順で説明します。</span><span class="sxs-lookup"><span data-stu-id="1662d-157">This is covered in the next procedure.</span></span>
13. <span data-ttu-id="1662d-158">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1662d-158">Click **Save**.</span></span>

<span data-ttu-id="1662d-159">ビルドをトリガーするときに更新する必要があります、 **OutputRoot**プロパティを展開するビルドをポイントします。</span><span class="sxs-lookup"><span data-stu-id="1662d-159">When you trigger a build, you need to update the **OutputRoot** property to point to the build you want to deploy.</span></span>

<span data-ttu-id="1662d-160">**ビルド定義から特定のビルドを配置するには**</span><span class="sxs-lookup"><span data-stu-id="1662d-160">**To deploy a specific build from a build definition**</span></span>

1. <span data-ttu-id="1662d-161">**チーム エクスプ ローラー**  ウィンドウでは、ビルド定義を右クリックし、をクリックして**新しいビルドをキュー**です。</span><span class="sxs-lookup"><span data-stu-id="1662d-161">In the **Team Explorer** window, right-click the build definition, and then click **Queue New Build**.</span></span>

    ![](deploying-a-specific-build/_static/image7.png)
2. <span data-ttu-id="1662d-162">**ビルドをキュー**ダイアログ ボックスの**パラメーター**  タブで、展開、**詳細**セクションです。</span><span class="sxs-lookup"><span data-stu-id="1662d-162">In the **Queue Build** dialog box, on the **Parameters** tab, expand the **Advanced** section.</span></span>
3. <span data-ttu-id="1662d-163">**の MSBuild 引数**行の値を置き換える、 **OutputRoot**ビルド フォルダーの場所を持つプロパティです。</span><span class="sxs-lookup"><span data-stu-id="1662d-163">In the **MSBuild Arguments** row, replace the value of the **OutputRoot** property with the location of your build folder.</span></span> <span data-ttu-id="1662d-164">例えば:</span><span class="sxs-lookup"><span data-stu-id="1662d-164">For example:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="1662d-165">ビルド フォルダーへのパスの最後に末尾のスラッシュを含めることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1662d-165">Be sure to include a trailing slash at the end of the path to your build folder.</span></span>
4. <span data-ttu-id="1662d-166">をクリックして**キュー**です。</span><span class="sxs-lookup"><span data-stu-id="1662d-166">Click **Queue**.</span></span>

<span data-ttu-id="1662d-167">ビルド キューに配置し、プロジェクト ファイルは、データベース スクリプトを展開し、web パッケージ化、ビルド格納フォルダーからで指定したときに、 **OutputRoot**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="1662d-167">When you queue the build, the project file will deploy the database scripts and web packages from the build drop folder you specified in the **OutputRoot** property.</span></span>

## <a name="conclusion"></a><span data-ttu-id="1662d-168">まとめ</span><span class="sxs-lookup"><span data-stu-id="1662d-168">Conclusion</span></span>

<span data-ttu-id="1662d-169">このトピックでは、web のパッケージとデータベースのスクリプトと同様に、展開のリソースを公開、前の特定のビルド分割プロジェクト ファイルの配置モデルを使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1662d-169">This topic described how to publish deployment resources, like web packages and database scripts, from a specific previous build using the split project file deployment model.</span></span> <span data-ttu-id="1662d-170">オーバーライドする方法が説明されている、 **OutputRoot**ビルド定義のプロパティと、TFS にデプロイ ロジックを組み込む方法です。</span><span class="sxs-lookup"><span data-stu-id="1662d-170">It explained how to override the **OutputRoot** property and how to incorporate the deployment logic into a TFS build definition.</span></span>

## <a name="further-reading"></a><span data-ttu-id="1662d-171">関連項目</span><span class="sxs-lookup"><span data-stu-id="1662d-171">Further Reading</span></span>

<span data-ttu-id="1662d-172">ビルド定義を作成する方法の詳細については、次を参照してください。[基本的なビルド定義を作成する](https://msdn.microsoft.com/library/ms181716.aspx)と[ビルド プロセスの定義](https://msdn.microsoft.com/library/ms181715.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="1662d-172">For more information on creating build definitions, see [Create a Basic Build Definition](https://msdn.microsoft.com/library/ms181716.aspx) and [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx).</span></span> <span data-ttu-id="1662d-173">キュー ビルドの詳細については、次を参照してください。[ビルドをキュー](https://msdn.microsoft.com/library/ms181722.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="1662d-173">For more guidance on queuing builds, see [Queue a Build](https://msdn.microsoft.com/library/ms181722.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1662d-174">[前へ](creating-a-build-definition-that-supports-deployment.md)
> [次へ](configuring-permissions-for-team-build-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="1662d-174">[Previous](creating-a-build-definition-that-supports-deployment.md)
[Next](configuring-permissions-for-team-build-deployment.md)</span></span>
