---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: コマンド ファイルの作成と展開を実行 |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、ファイルを構築するコマンドの 1 つの手順として、Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して再展開を実行するために使用する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: e5fb034a67bc9f2ea549af269eae51a49acc4d98
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="9792c-103">作成と展開コマンド ファイルの実行</span><span class="sxs-lookup"><span data-stu-id="9792c-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="9792c-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="9792c-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="9792c-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="9792c-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="9792c-106">このトピックでは、1 ステップの再現可能なプロセスとして Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して展開を実行するために使用するコマンド ファイルを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="9792c-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="9792c-107">このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、[連絡先のマネージャー](the-contact-manager-solution.md)ソリューション&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="9792c-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="9792c-108">説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[ビルド プロセスの理解](understanding-the-build-process.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。</span><span class="sxs-lookup"><span data-stu-id="9792c-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="9792c-109">ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="9792c-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="9792c-110">プロセスの概要</span><span class="sxs-lookup"><span data-stu-id="9792c-110">Process Overview</span></span>

<span data-ttu-id="9792c-111">このトピックでは、作成し、これらのプロジェクト ファイルを使用して、対象となる環境への反復可能な展開を実行するコマンド ファイルを実行する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="9792c-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="9792c-112">基本的には、コマンド ファイル単純にする必要があります、MSBuild コマンドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9792c-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="9792c-113">環境にもとらわれないを実行する MSBuild に指示*Publish.proj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="9792c-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="9792c-114">通知、 *Publish.proj*ファイルのどのファイルには、環境固有のプロジェクト設定とそれを検索する場所が含まれています。</span><span class="sxs-lookup"><span data-stu-id="9792c-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="9792c-115">MSBuild のコマンドを作成します。</span><span class="sxs-lookup"><span data-stu-id="9792c-115">Create an MSBuild Command</span></span>

<span data-ttu-id="9792c-116">」の説明に従って[ビルド プロセスの理解](understanding-the-build-process.md)、環境固有のプロジェクト ファイル&#x2014;など*Env Dev.proj*&#x2014;環境に柔軟なにインポートするよう設計されています。*Publish.proj*ビルド時にファイル。</span><span class="sxs-lookup"><span data-stu-id="9792c-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="9792c-117">さらに、これら 2 つのファイルには、MSBuild のビルドし、ソリューションを展開する方法を指示する命令の完全なセットが提供されます。</span><span class="sxs-lookup"><span data-stu-id="9792c-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="9792c-118">*Publish.proj*ファイルの使用、**インポート**環境固有のプロジェクト ファイルをインポートする要素。</span><span class="sxs-lookup"><span data-stu-id="9792c-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="9792c-119">そのため、MSBuild.exe を使用してビルドして、連絡先のマネージャー ソリューションを配置するときにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="9792c-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="9792c-120">MSBuild.exe を実行、 *Publish.proj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="9792c-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="9792c-121">という名前のコマンド ライン パラメーターを指定することによって、環境固有のプロジェクト ファイルの場所の指定**TargetEnvPropsFile**です。</span><span class="sxs-lookup"><span data-stu-id="9792c-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="9792c-122">これを行うには、MSBuild コマンドのようにこの必要があります。</span><span class="sxs-lookup"><span data-stu-id="9792c-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="9792c-123">ここでは、これは、反復可能な単一ステップ デプロイメントに移動する簡単な手順です。</span><span class="sxs-lookup"><span data-stu-id="9792c-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="9792c-124">必要な操作すべては、.cmd ファイルを MSBuild コマンドを追加します。</span><span class="sxs-lookup"><span data-stu-id="9792c-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="9792c-125">ソリューションでは、連絡先のマネージャー、発行フォルダーには、という名前のファイルが含まれる*発行 Dev.cmd*正確にこれを行うことです。</span><span class="sxs-lookup"><span data-stu-id="9792c-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="9792c-126">**/Fl**という名前のログ ファイルを作成するスイッチが msbuild *msbuild.log になります*MSBuild.exe が呼び出された作業ディレクトリにします。</span><span class="sxs-lookup"><span data-stu-id="9792c-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="9792c-127">連絡先のマネージャー ソリューションを再配置を配置するには、行う必要がある実行、*発行 Dev.cmd*ファイル。</span><span class="sxs-lookup"><span data-stu-id="9792c-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="9792c-128">MSBuild は、ファイルを実行するときに行います。</span><span class="sxs-lookup"><span data-stu-id="9792c-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="9792c-129">ソリューション内のすべてのプロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="9792c-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="9792c-130">Web アプリケーション プロジェクトの配置可能な web パッケージを生成します。</span><span class="sxs-lookup"><span data-stu-id="9792c-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="9792c-131">データベース プロジェクトの .dbschema および .deploymanifest ファイルを生成します。</span><span class="sxs-lookup"><span data-stu-id="9792c-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="9792c-132">Web サーバーに web パッケージを展開します。</span><span class="sxs-lookup"><span data-stu-id="9792c-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="9792c-133">データベース サーバーにデータベースを展開します。</span><span class="sxs-lookup"><span data-stu-id="9792c-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="9792c-134">展開を実行します。</span><span class="sxs-lookup"><span data-stu-id="9792c-134">Run the Deployment</span></span>

<span data-ttu-id="9792c-135">ターゲット環境には、コマンド ファイルを作成したら、単にファイルを実行して、全体の展開を完了することができます。</span><span class="sxs-lookup"><span data-stu-id="9792c-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="9792c-136">**連絡先のマネージャー ソリューション、テスト環境を配置するには**</span><span class="sxs-lookup"><span data-stu-id="9792c-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="9792c-137">開発者のワークステーションで Windows エクスプ ローラーを開きの場所を参照し、*発行 Dev.cmd*ファイル。</span><span class="sxs-lookup"><span data-stu-id="9792c-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="9792c-138">これを実行するファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="9792c-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="9792c-139">場合、**ファイルを開く-セキュリティ警告** ダイアログ ボックスが表示されたら、をクリックして**実行**です。</span><span class="sxs-lookup"><span data-stu-id="9792c-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="9792c-140">かどうか、構成設定およびテスト サーバーが設定されて正しく、コマンド プロンプト ウィンドウが表示されます、**ビルドに成功しました**メッセージの MSBuild がプロジェクト ファイルの処理を完了します。</span><span class="sxs-lookup"><span data-stu-id="9792c-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="9792c-141">この環境にソリューションを配置した初めての場合は、テスト web server マシン アカウントを追加する必要があります、 **db\_datawriter**と**db\_datareader**上の役割、 **ContactManager**データベース。</span><span class="sxs-lookup"><span data-stu-id="9792c-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="9792c-142">この手順で説明されて[Web 配置発行のデータベース サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)です。</span><span class="sxs-lookup"><span data-stu-id="9792c-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="9792c-143">データベースを作成するときに、これらのアクセス許可を割り当てる必要があるだけです。</span><span class="sxs-lookup"><span data-stu-id="9792c-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="9792c-144">既定では、ビルド プロセスは再作成されませんのそれぞれの配置上のデータベース&#x2014;代わりを最新のスキーマに既存のデータベースを比較し、必要な変更のみを作成します。</span><span class="sxs-lookup"><span data-stu-id="9792c-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="9792c-145">結果として、初めてにソリューションを配置するときにこれらのデータベース ロールをマップする必要がありますのみ必要。</span><span class="sxs-lookup"><span data-stu-id="9792c-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="9792c-146">Internet Explorer を開き、連絡先のマネージャー アプリケーションの URL に移動 (たとえば、 `http://testweb1:85/ContactManager/`)。</span><span class="sxs-lookup"><span data-stu-id="9792c-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="9792c-147">アプリケーションが期待どおりに動作することを確認し、連絡先を追加することができました。</span><span class="sxs-lookup"><span data-stu-id="9792c-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="9792c-148">まとめ</span><span class="sxs-lookup"><span data-stu-id="9792c-148">Conclusion</span></span>

<span data-ttu-id="9792c-149">MSBuild 指示を含むコマンド ファイルを作成するビルドと特定の送信先の環境にマルチ プロジェクト ソリューションを展開の迅速かつ簡単な方法を提供しています。</span><span class="sxs-lookup"><span data-stu-id="9792c-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="9792c-150">繰り返し、ソリューションを複数の展開先環境を配置する必要がある場合は、複数のコマンド ファイルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="9792c-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="9792c-151">各コマンド ファイルで、MSBuild のコマンドは、同じユニバーサル プロジェクト ファイルのビルド、けれども、別の環境に固有のプロジェクト ファイルを指定できます。</span><span class="sxs-lookup"><span data-stu-id="9792c-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="9792c-152">たとえば、またはテスト環境で、開発者に公開するコマンド ファイルには、この MSBuild コマンドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="9792c-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="9792c-153">コマンド ファイル ステージング環境に公開するにはには、この MSBuild コマンドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="9792c-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="9792c-154">サーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)です。</span><span class="sxs-lookup"><span data-stu-id="9792c-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="9792c-155">プロパティをオーバーライドするか、MSBuild コマンドで他の各種のスイッチを設定して各環境のビルド プロセスをカスタマイズすることもできます。</span><span class="sxs-lookup"><span data-stu-id="9792c-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="9792c-156">詳細については、次を参照してください。 [MSBuild コマンド ライン リファレンス](https://msdn.microsoft.com/library/ms164311.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="9792c-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9792c-157">[前へ](deploying-database-projects.md)
> [次へ](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="9792c-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
