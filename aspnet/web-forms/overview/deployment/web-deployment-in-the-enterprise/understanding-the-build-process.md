---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: ビルド プロセスの理解 |Microsoft Docs
author: jrjlee
description: このトピックでは、エンタープライズ規模のビルドと展開プロセスのチュートリアルを示します。 このトピックで説明したアプローチでは、Microsoft のカスタム ビルド エンジニアを使用しています.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 9df145b281b086f546c55d0b26a8b0e44e896bb0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837395"
---
<a name="understanding-the-build-process"></a><span data-ttu-id="3ad6b-104">ビルド プロセスを理解します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-104">Understanding the Build Process</span></span>
====================
<span data-ttu-id="3ad6b-105">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="3ad6b-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="3ad6b-106">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="3ad6b-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="3ad6b-107">このトピックでは、エンタープライズ規模のビルドと展開プロセスのチュートリアルを示します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-107">This topic provides a walkthrough of an enterprise-scale build and deployment process.</span></span> <span data-ttu-id="3ad6b-108">このトピックで説明した方法では、プロセスのすべての側面の詳細に制御を提供するのにカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-108">The approach described in this topic uses custom Microsoft Build Engine (MSBuild) project files to provide fine-grained control over every aspect of the process.</span></span> <span data-ttu-id="3ad6b-109">プロジェクト ファイル内では、カスタム MSBuild のターゲットは、インターネット インフォメーション サービス (IIS) Web 配置ツール (MSDeploy.exe) などの展開ユーティリティと VSDBCMD.exe データベース配置ユーティリティの実行に使用されます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-109">Within the project files, custom MSBuild targets are used to run deployment utilities like the Internet Information Services (IIS) Web Deployment Tool (MSDeploy.exe) and the database deployment utility VSDBCMD.exe.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="3ad6b-110">前のトピックでは、[プロジェクト ファイルを理解する](understanding-the-project-file.md)、MSBuild プロジェクト ファイルの主要なコンポーネントを説明し、複数のターゲット環境に展開をサポートするプロジェクト ファイルの分割の概念が導入されました。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-110">The previous topic, [Understanding the Project File](understanding-the-project-file.md), described the key components of an MSBuild project file and introduced the concept of split project files to support deployment to multiple target environments.</span></span> <span data-ttu-id="3ad6b-111">確認する必要がありますなら既にこれらの概念について、[プロジェクト ファイルを理解する](understanding-the-project-file.md)前に、このトピックで作業します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-111">If you're not already familiar with these concepts, you should review [Understanding the Project File](understanding-the-project-file.md) before you work through this topic.</span></span>


<span data-ttu-id="3ad6b-112">このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-112">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="3ad6b-113">これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-113">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="3ad6b-114">ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-114">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="build-and-deployment-overview"></a><span data-ttu-id="3ad6b-115">ビルドと展開の概要</span><span class="sxs-lookup"><span data-stu-id="3ad6b-115">Build and Deployment Overview</span></span>

<span data-ttu-id="3ad6b-116">[連絡先マネージャー ソリューション](the-contact-manager-solution.md)、3 つのファイルがビルドおよび配置プロセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-116">In the [Contact Manager solution](the-contact-manager-solution.md), three files control the build and deployment process:</span></span>

- <span data-ttu-id="3ad6b-117">A*ユニバーサル プロジェクト ファイル*(*Publish.proj*)。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-117">A *universal project file* (*Publish.proj*).</span></span> <span data-ttu-id="3ad6b-118">これには、移行先の環境間で変更されないビルドおよび展開の説明が含まれています。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-118">This contains build and deployment instructions that do not change between destination environments.</span></span>
- <span data-ttu-id="3ad6b-119">*環境固有のプロジェクト ファイル*(*Env Dev.proj*)。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-119">An *environment-specific project file* (*Env-Dev.proj*).</span></span> <span data-ttu-id="3ad6b-120">これには、特定の送信先の環境に固有のビルドと配置の設定が含まれています。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-120">This contains build and deployment settings that are specific to a particular destination environment.</span></span> <span data-ttu-id="3ad6b-121">たとえば、使用する、 *Env Dev.proj*開発者またはテスト環境の設定を行い、という名前の別のファイルを作成するファイル*Env Stage.proj*ステージングの設定を指定環境。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-121">For example, you could use the *Env-Dev.proj* file to provide settings for a developer or test environment and create an alternative file named *Env-Stage.proj* to provide settings for a staging environment.</span></span>
- <span data-ttu-id="3ad6b-122">A*コマンド ファイル*(*発行 Dev.cmd*)。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-122">A *command file* (*Publish-Dev.cmd*).</span></span> <span data-ttu-id="3ad6b-123">これには、プロジェクト ファイルを指定するコマンドを実行する場合、MSBuild.exe が含まれています。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-123">This contains an MSBuild.exe command that specifies which project files you want to execute.</span></span> <span data-ttu-id="3ad6b-124">各ファイルが別の環境に固有のプロジェクト ファイルを指定する MSBuild.exe のコマンドが含まれているすべての変換先の環境のコマンド ファイルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-124">You can create a command file for every destination environment, where each file contains an MSBuild.exe command that specifies a different environment-specific project file.</span></span> <span data-ttu-id="3ad6b-125">これにより、開発者が適切なコマンド ファイルを実行するだけでさまざまな環境にデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-125">This lets the developer deploy to different environments simply by running the appropriate command file.</span></span>

<span data-ttu-id="3ad6b-126">サンプルのソリューションでは、これら 3 つの発行フォルダーにソリューション ファイルが見つかります。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-126">In the sample solution, you can find these three files in the Publish solution folder.</span></span>

![](understanding-the-build-process/_static/image1.png)

<span data-ttu-id="3ad6b-127">これらのファイルをさらに詳しく見ると、前にこのアプローチを使用する場合の全体的なビルド プロセスのしくみを見てをみましょう。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-127">Before you look at these files in more detail, let's take a look at how the overall build process works when you use this approach.</span></span> <span data-ttu-id="3ad6b-128">大まかに言えば、ビルドおよび配置プロセスはようになります。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-128">At a high level, the build and deployment process looks like this:</span></span>

![](understanding-the-build-process/_static/image2.png)

<span data-ttu-id="3ad6b-129">最初に行われるは、2 つのプロジェクト ファイル&#x2014;ユニバーサルのビルドと配置の手順を含む 1 つ、1 つの環境に固有の設定を含む&#x2014;1 つのプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-129">The first thing that happens is that the two project files&#x2014;one containing universal build and deployment instructions, and one containing environment-specific settings&#x2014;are merged into a single project file.</span></span> <span data-ttu-id="3ad6b-130">MSBuild はプロジェクト ファイル内の命令を動作します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-130">MSBuild then works through the instructions in the project file.</span></span> <span data-ttu-id="3ad6b-131">各プロジェクトごとに、プロジェクト ファイルを使用して、ソリューションでプロジェクトを構築します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-131">It builds each of the projects in the solution, using the project file for each project.</span></span> <span data-ttu-id="3ad6b-132">これは、後に、Web Deploy (MSDeploy.exe) などの他のツールと、web コンテンツおよびデータベースを対象となる環境にデプロイする VSDBCMD ユーティリティ呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-132">It then calls out to other tools, like Web Deploy (MSDeploy.exe) and the VSDBCMD utility to deploy your web content and databases to the target environment.</span></span>

<span data-ttu-id="3ad6b-133">[完了] を [スタート] からは、ビルドおよび配置プロセスは、これらのタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-133">From start to finish, the build and deployment process performs these tasks:</span></span>

1. <span data-ttu-id="3ad6b-134">新しいビルドの準備として、出力ディレクトリの内容を削除します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-134">It deletes the contents of the output directory, in preparation for a fresh build.</span></span>
2. <span data-ttu-id="3ad6b-135">ソリューション内の各プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-135">It builds each project in the solution:</span></span>

    1. <span data-ttu-id="3ad6b-136">Web プロジェクトの&#x2014;ここでは、ASP.NET MVC web アプリケーションと WCF web サービス&#x2014;ビルド プロセスは、各プロジェクトの web 展開パッケージを作成します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-136">For web projects&#x2014;in this case, an ASP.NET MVC web application and a WCF web service&#x2014;the build process creates a web deployment package for each project.</span></span>
    2. <span data-ttu-id="3ad6b-137">データベース プロジェクトの場合は、ビルド プロセスは、各プロジェクトの配置マニフェスト (.deploymanifest ファイル) を作成します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-137">For database projects, the build process creates a deployment manifest (.deploymanifest file) for each project.</span></span>
3. <span data-ttu-id="3ad6b-138">VSDBCMD.exe ユーティリティを使用して、プロジェクト ファイルからさまざまなプロパティを使用して、ソリューション内の各データベース プロジェクトのデプロイを&#x2014;ターゲット接続文字列とデータベース名を&#x2014;.deploymanifest ファイルと共にします。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-138">It uses the VSDBCMD.exe utility to deploy each database project in the solution, using various properties from the project files&#x2014;a target connection string and a database name&#x2014;together with the .deploymanifest file.</span></span>
4. <span data-ttu-id="3ad6b-139">MSDeploy.exe ユーティリティを使用して、プロジェクト ファイルから展開プロセスを制御するさまざまなプロパティを使用して、ソリューション内の各 web プロジェクトをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-139">It uses the MSDeploy.exe utility to deploy each web project in the solution, using various properties from the project files to control the deployment process.</span></span>

<span data-ttu-id="3ad6b-140">サンプル ソリューションを使用して、このプロセスの詳細をトレースすることができます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-140">You can use the sample solution to trace this process in more detail.</span></span>

> [!NOTE]
> <span data-ttu-id="3ad6b-141">独自のサーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-141">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


## <a name="invoking-the-build-and-deployment-process"></a><span data-ttu-id="3ad6b-142">ビルドと展開プロセスを呼び出す</span><span class="sxs-lookup"><span data-stu-id="3ad6b-142">Invoking the Build and Deployment Process</span></span>

<span data-ttu-id="3ad6b-143">開発者が実行連絡先マネージャー ソリューションを開発者のテスト環境をデプロイするのには、*発行 Dev.cmd*コマンド ファイルです。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-143">To deploy the Contact Manager solution to a developer test environment, the developer runs the *Publish-Dev.cmd* command file.</span></span> <span data-ttu-id="3ad6b-144">MSBuild.exe を起動を指定する*Publish.proj*として実行するプロジェクト ファイルと*Env Dev.proj*パラメーター値として。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-144">This invokes MSBuild.exe, specifying *Publish.proj* as the project file to execute and *Env-Dev.proj* as a parameter value.</span></span>


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> <span data-ttu-id="3ad6b-145">**/Fl**スイッチ (短縮形 **/fileLogger**) という名前のファイルのビルド出力をログに記録*msbuild.log になります*現在のディレクトリで。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-145">The **/fl** switch (short for **/fileLogger**) logs the build output to a file named *msbuild.log* in the current directory.</span></span> <span data-ttu-id="3ad6b-146">詳細については、次を参照してください。、 [MSBuild コマンド ライン リファレンス](https://msdn.microsoft.com/library/ms164311.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-146">For more information, see the [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>


<span data-ttu-id="3ad6b-147">この時点では、MSBuild の実行を開始、読み込み、 *Publish.proj*ファイル、およびその中の手順の処理を開始します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-147">At this point, MSBuild starts running, loads the *Publish.proj* file, and starts processing the instructions within it.</span></span> <span data-ttu-id="3ad6b-148">最初の命令が MSBuild プロジェクトをインポートするように指示するファイル、 **TargetEnvPropsFile**パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-148">The first instruction tells MSBuild to import the project file that the **TargetEnvPropsFile** parameter specifies.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


<span data-ttu-id="3ad6b-149">**TargetEnvPropsFile**パラメーターを指定します、 *Env Dev.proj*ファイル、MSBuild の内容をマージするため、 *Env Dev.proj*ファイルを*Publish.proj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-149">The **TargetEnvPropsFile** parameter specifies the *Env-Dev.proj* file, so MSBuild merges the contents of the *Env-Dev.proj* file into the *Publish.proj* file.</span></span>

<span data-ttu-id="3ad6b-150">MSBuild は、マージされたプロジェクト ファイル内で検出された次の要素は、プロパティ グループです。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-150">The next elements that MSBuild encounters in the merged project file are property groups.</span></span> <span data-ttu-id="3ad6b-151">プロパティは、ファイルに出現する順序で処理されます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-151">Properties are processed in the order in which they appear in the file.</span></span> <span data-ttu-id="3ad6b-152">MSBuild は、指定した条件を満たしていることを提供する、各プロパティのキー/値ペアを作成します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-152">MSBuild creates a key-value pair for each property, providing that any specified conditions are met.</span></span> <span data-ttu-id="3ad6b-153">後で定義されているファイルのプロパティと同じ名前のファイルで既に定義済みプロパティが上書きされます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-153">Properties defined later in the file will overwrite any properties with the same name defined earlier in the file.</span></span> <span data-ttu-id="3ad6b-154">たとえば、 **OutputRoot**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-154">For example, consider the **OutputRoot** properties.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


<span data-ttu-id="3ad6b-155">MSBuild が最初に処理と**OutputRoot**要素、同様に名前付きパラメーターを提供することが指定されていませんの値を設定、 **OutputRoot**プロパティを **.\Publish\Out**します。2 番目に到達したときに**OutputRoot**要素に、条件が評価される場合は、 **true**の値が上書きされます、 **OutputRoot**プロパティの値と、**OutDir**パラメーター。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-155">When MSBuild processes the first **OutputRoot** element, providing a similarly named parameter has not been provided, it sets the value of the **OutputRoot** property to **..\Publish\Out**. When it encounters the second **OutputRoot** element, if the condition evaluates to **true**, it will overwrite the value of the **OutputRoot** property with the value of the **OutDir** parameter.</span></span>

<span data-ttu-id="3ad6b-156">MSBuild が発生する次の要素がという名前の項目を含む、1 つの項目グループ**ProjectsToBuild**します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-156">The next element that MSBuild encounters is a single item group, containing an item named **ProjectsToBuild**.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


<span data-ttu-id="3ad6b-157">MSBuild では、この命令を処理という名前の項目リストを作成することにより**ProjectsToBuild**します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-157">MSBuild processes this instruction by building an item list named **ProjectsToBuild**.</span></span> <span data-ttu-id="3ad6b-158">ここでは、項目リストが 1 つの値が含まれます&#x2014;ソリューション ファイルのファイル名とパス。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-158">In this case, the item list contains a single value&#x2014;the path and filename of the solution file.</span></span>

<span data-ttu-id="3ad6b-159">この時点で、残りの要素は、ターゲットです。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-159">At this point, the remaining elements are targets.</span></span> <span data-ttu-id="3ad6b-160">プロパティと項目からターゲットを異なる方法で処理が&#x2014;は明示的に、ユーザーが指定したか、プロジェクト ファイル内で別のコンス トラクターによって呼び出される場合を除き、基本的には、ターゲットは処理されません。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-160">Targets are processed differently from properties and items&#x2014;essentially, targets are not processed unless they are either explicitly specified by the user or invoked by another construct within the project file.</span></span> <span data-ttu-id="3ad6b-161">いることを思い出してください開始**プロジェクト**タグが含まれています、 **DefaultTargets**属性。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-161">Recall that the opening **Project** tag includes a **DefaultTargets** attribute.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


<span data-ttu-id="3ad6b-162">これにより、MSBuild を呼び出す、 **FullPublish** MSBuild.exe が呼び出されるときにターゲットがない場合は、ターゲットが指定されています。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-162">This instructs MSBuild to invoke the **FullPublish** target, if targets are not specified when MSBuild.exe is invoked.</span></span> <span data-ttu-id="3ad6b-163">**FullPublish**ターゲットにはすべてのタスクが含まれていません。 代わりに依存関係の一覧を単純に指定します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-163">The **FullPublish** target doesn't contain any tasks; instead it simply specifies a list of dependencies.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


<span data-ttu-id="3ad6b-164">この依存関係は MSBuild を実行する順序で、 **FullPublish**をこの一覧の表示順序でターゲットを呼び出す必要が、ターゲット。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-164">This dependency tells MSBuild that in order to execute the **FullPublish** target, it needs to invoke this list of targets in the order provided:</span></span>

1. <span data-ttu-id="3ad6b-165">呼び出す必要がありますが、**クリーン**ターゲット。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-165">It must invoke the **Clean** target.</span></span>
2. <span data-ttu-id="3ad6b-166">呼び出す必要がありますが、 **BuildProjects**ターゲット。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-166">It must invoke the **BuildProjects** target.</span></span>
3. <span data-ttu-id="3ad6b-167">呼び出す必要がありますが、 **GatherPackagesForPublishing**ターゲット。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-167">It must invoke the **GatherPackagesForPublishing** target.</span></span>
4. <span data-ttu-id="3ad6b-168">呼び出す必要がありますが、 **PublishDbPackages**ターゲット。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-168">It must invoke the **PublishDbPackages** target.</span></span>
5. <span data-ttu-id="3ad6b-169">呼び出す必要がありますが、 **PublishWebPackages**ターゲット。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-169">It must invoke the **PublishWebPackages** target.</span></span>

### <a name="the-clean-target"></a><span data-ttu-id="3ad6b-170">Clean ターゲット</span><span class="sxs-lookup"><span data-stu-id="3ad6b-170">The Clean Target</span></span>

<span data-ttu-id="3ad6b-171">**クリーン**ターゲットは、出力ディレクトリとそのすべての内容を基本的に削除すると、新しいビルドの準備として。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-171">The **Clean** target basically deletes the output directory and all its contents, as preparation for a fresh build.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


<span data-ttu-id="3ad6b-172">ターゲットが含まれていますが、 **ItemGroup**要素。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-172">Notice that the target includes an **ItemGroup** element.</span></span> <span data-ttu-id="3ad6b-173">プロパティまたは内の項目を定義するとき、**ターゲット**要素を作成している*動的*プロパティと項目。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-173">When you define properties or items within a **Target** element, you're creating *dynamic* properties and items.</span></span> <span data-ttu-id="3ad6b-174">つまり、プロパティまたは項目はターゲットが実行されるまでに処理されません。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-174">In other words, the properties or items aren't processed until the target is executed.</span></span> <span data-ttu-id="3ad6b-175">出力ディレクトリが存在しないか、ビルドするには、ビルド プロセスが開始されるまですべてのファイルを含む、  **\_FilesToDelete**静的アイテムとして一覧表示は、実行が進行中になるまで待機する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-175">The output directory might not exist or contain any files until the build process begins, so you can't build the **\_FilesToDelete** list as a static item; you have to wait until execution is underway.</span></span> <span data-ttu-id="3ad6b-176">そのため、リストを作成するには、ターゲット内の動的なアイテムとして。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-176">As such, you build the list as a dynamic item within the target.</span></span>

> [!NOTE]
> <span data-ttu-id="3ad6b-177">この場合は、ため、**クリーン**ターゲットは、最初に実行される、動的なグループを使用する実際の必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-177">In this case, because the **Clean** target is the first to be executed, there's no real need to use a dynamic item group.</span></span> <span data-ttu-id="3ad6b-178">ただし、ある時点で異なる順序でターゲットを実行するようにこの種類のシナリオでは、動的なプロパティと項目を使用することをお勧めを勧めします。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-178">However, it's good practice to use dynamic properties and items in this type of scenario, as you might want to execute targets in a different order at some point.</span></span>  
> <span data-ttu-id="3ad6b-179">使用しない項目を宣言することを回避するためにも目指します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-179">You should also aim to avoid declaring items that will never be used.</span></span> <span data-ttu-id="3ad6b-180">特定のターゲットでのみ使用される項目があれば、ビルド プロセスで、不要なオーバーヘッドを削除するターゲット内に配置することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-180">If you have items that will only be used by a specific target, consider placing them inside the target to remove any unnecessary overhead on the build process.</span></span>


<span data-ttu-id="3ad6b-181">項目を動的、**クリーン**ターゲットはとても簡単ですし、組み込みの使用**メッセージ**、**削除**、および**RemoveDir**へのタスクします。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-181">Dynamic items aside, the **Clean** target is fairly straightforward and makes use of the built-in **Message**, **Delete**, and **RemoveDir** tasks to:</span></span>

1. <span data-ttu-id="3ad6b-182">Logger にメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-182">Send a message to the logger.</span></span>
2. <span data-ttu-id="3ad6b-183">削除するファイルの一覧を作成します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-183">Build a list of files to delete.</span></span>
3. <span data-ttu-id="3ad6b-184">ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-184">Delete the files.</span></span>
4. <span data-ttu-id="3ad6b-185">出力ディレクトリを削除します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-185">Remove the output directory.</span></span>

### <a name="the-buildprojects-target"></a><span data-ttu-id="3ad6b-186">BuildProjects ターゲット</span><span class="sxs-lookup"><span data-stu-id="3ad6b-186">The BuildProjects Target</span></span>

<span data-ttu-id="3ad6b-187">**BuildProjects**ターゲットは基本的に、サンプル ソリューション内のすべてのプロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-187">The **BuildProjects** target basically builds all the projects in the sample solution.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


<span data-ttu-id="3ad6b-188">このターゲットは、前のトピックで詳しく説明した[プロジェクト ファイルを理解する](understanding-the-project-file.md)タスクとターゲット プロパティと項目の参照方法を説明するためにします。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-188">This target was described in some detail in the previous topic, [Understanding the Project File](understanding-the-project-file.md), to illustrate how tasks and targets reference properties and items.</span></span> <span data-ttu-id="3ad6b-189">この時点では、主に関心、 **MSBuild**タスク。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-189">At this point, you're mainly interested in the **MSBuild** task.</span></span> <span data-ttu-id="3ad6b-190">このタスクを使用すると、複数のプロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-190">You can use this task to build multiple projects.</span></span> <span data-ttu-id="3ad6b-191">タスクが MSBuild.exe; の新しいインスタンスを作成できません。各プロジェクトをビルド、実行中の現在のインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-191">The task does not create a new instance of MSBuild.exe; it uses the current running instance to build each project.</span></span> <span data-ttu-id="3ad6b-192">この例では関心のある重要な点は、展開プロパティを示します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-192">The key points of interest in this example are the deployment properties:</span></span>

- <span data-ttu-id="3ad6b-193">**DeployOnBuild**プロパティには、各プロジェクトのビルドが完了すると、プロジェクトの設定で、デプロイの手順を実行するために MSBuild がよう指示します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-193">The **DeployOnBuild** property instructs MSBuild to run any deployment instructions in the project settings when the build of each project is complete.</span></span>
- <span data-ttu-id="3ad6b-194">**DeployTarget**プロパティは、プロジェクトのビルド後に起動するターゲットを識別します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-194">The **DeployTarget** property identifies the target that you want to invoke after the project is built.</span></span> <span data-ttu-id="3ad6b-195">ここで、**パッケージ**ターゲットは、配置可能な web のパッケージにプロジェクトの出力をビルドします。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-195">In this case, the **Package** target builds the project output into a deployable web package.</span></span>

> [!NOTE]
> <span data-ttu-id="3ad6b-196">**パッケージ**ターゲットによって呼び出された Web 発行パイプライン (WPP)、MSBuild および Web デプロイ間の統合を提供します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-196">The **Package** target invokes the Web Publishing Pipeline (WPP), which provides integration between MSBuild and Web Deploy.</span></span> <span data-ttu-id="3ad6b-197">見てレビュー、組み込みターゲット、WPP が提供する場合、 *Microsoft.Web.Publishing.targets* %PROGRAMFILES (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web フォルダーにファイル。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-197">If you want to take a look at the built-in targets that the WPP provides, review the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span>


### <a name="the-gatherpackagesforpublishing-target"></a><span data-ttu-id="3ad6b-198">GatherPackagesForPublishing ターゲット</span><span class="sxs-lookup"><span data-stu-id="3ad6b-198">The GatherPackagesForPublishing Target</span></span>

<span data-ttu-id="3ad6b-199">調査する場合、 **GatherPackagesForPublishing**ターゲット、実際にすべてのタスクを含んでいないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-199">If you study the **GatherPackagesForPublishing** target, you'll notice that it doesn't actually contain any tasks.</span></span> <span data-ttu-id="3ad6b-200">代わりに、3 つの動的な項目を定義する 1 つの項目グループが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-200">Instead, it contains a single item group that defines three dynamic items.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


<span data-ttu-id="3ad6b-201">これらの項目が際に作成された展開パッケージを参照してください、 **BuildProjects**ターゲットが実行されました。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-201">These items refer to the deployment packages that were created when the **BuildProjects** target was executed.</span></span> <span data-ttu-id="3ad6b-202">でしたで定義するこれらの項目静的に、プロジェクト ファイルまで、項目が参照するファイルが存在しないため、 **BuildProjects**ターゲットを実行します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-202">You couldn't define these items statically in the project file, because the files to which the items refer don't exist until the **BuildProjects** target is executed.</span></span> <span data-ttu-id="3ad6b-203">代わりに、項目定義する必要が動的にまでは呼び出されませんターゲット内で後に、 **BuildProjects**ターゲットを実行します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-203">Instead, the items must be defined dynamically within a target that is not invoked until after the **BuildProjects** target is executed.</span></span>

<span data-ttu-id="3ad6b-204">このターゲット内では、項目は使用されません&#x2014;項目と各項目の値に関連付けられているメタデータ単にこのターゲットをビルドします。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-204">The items are not used within this target&#x2014;this target simply builds the items and the metadata associated with each item value.</span></span> <span data-ttu-id="3ad6b-205">これらの要素が処理されると、 **PublishPackages**項目が 2 つの値へのパスを含む、 *ContactManager.Mvc.deploy.cmd*ファイルとパスを*ContactManager.Service.deploy.cmd*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-205">Once these elements are processed, the **PublishPackages** item will contain two values, the path to the *ContactManager.Mvc.deploy.cmd* file and the path to the *ContactManager.Service.deploy.cmd* file.</span></span> <span data-ttu-id="3ad6b-206">Web Deploy は、プロジェクトごとに web パッケージの一部としてこれらのファイルを作成し、これらは、呼び出す必要のあるファイル、パッケージをデプロイするには、移行先サーバーにします。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-206">Web Deploy creates these files as part of the web package for each project, and these are the files that you must invoke on the destination server in order to deploy the packages.</span></span> <span data-ttu-id="3ad6b-207">これらのファイルのいずれかを開くには場合、は、さまざまなビルドに固有のパラメーター値を持つ MSDeploy.exe コマンドを基本的に表示されます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-207">If you open up one of these files, you'll basically see an MSDeploy.exe command with various build-specific parameter values.</span></span>

<span data-ttu-id="3ad6b-208">**DbPublishPackages**項目が 1 つの値へのパスを含む、 *ContactManager.Database.deploymanifest*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-208">The **DbPublishPackages** item will contain a single value, the path to the *ContactManager.Database.deploymanifest* file.</span></span>

> [!NOTE]
> <span data-ttu-id="3ad6b-209">データベース プロジェクトをビルドすると、MSBuild プロジェクト ファイルと同じスキーマを使用して、.deploymanifest ファイルが生成されます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-209">A .deploymanifest file is generated when you build a database project, and it uses the same schema as an MSBuild project file.</span></span> <span data-ttu-id="3ad6b-210">データベース スキーマ (.dbschema) の場所や配置前や配置後スクリプトの詳細など、データベースを配置するために必要なすべての情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-210">It contains all the information required to deploy a database, including the location of the database schema (.dbschema) and details of any pre-deployment and post-deployment scripts.</span></span> <span data-ttu-id="3ad6b-211">詳細については、次を参照してください。 [、概要のデータベースのビルドと展開](https://msdn.microsoft.com/library/aa833165.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-211">For more information, see [An Overview of Database Build and Deployment](https://msdn.microsoft.com/library/aa833165.aspx).</span></span>


<span data-ttu-id="3ad6b-212">デプロイ パッケージとデータベースの配置マニフェストの作成および使用方法の詳細を学習[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)と[データベース プロジェクトの配置](deploying-database-projects.md)します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-212">You'll learn more about how deployment packages and database deployment manifests are created and used in [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md) and [Deploying Database Projects](deploying-database-projects.md).</span></span>

### <a name="the-publishdbpackages-target"></a><span data-ttu-id="3ad6b-213">PublishDbPackages ターゲット</span><span class="sxs-lookup"><span data-stu-id="3ad6b-213">The PublishDbPackages Target</span></span>

<span data-ttu-id="3ad6b-214">簡単に言うと、 **PublishDbPackages**ターゲットをデプロイする VSDBCMD ユーティリティによって呼び出された、 **ContactManager**をターゲット環境にデータベース。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-214">Briefly speaking, the **PublishDbPackages** target invokes the VSDBCMD utility to deploy the **ContactManager** database to a target environment.</span></span> <span data-ttu-id="3ad6b-215">多くの意思決定と微妙な差異は、データベースの配置を構成する必要があり、この点についての詳細を学習[データベース プロジェクトの配置](deploying-database-projects.md)と[の複数の環境のデータベースの配置をカスタマイズする](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).</span><span class="sxs-lookup"><span data-stu-id="3ad6b-215">Configuring database deployment involves lots of decisions and nuances, and you'll learn more about this in [Deploying Database Projects](deploying-database-projects.md) and [Customizing Database Deployments for Multiple Environments](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).</span></span> <span data-ttu-id="3ad6b-216">このトピックでは、このターゲットが実際に動作する方法に注目します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-216">In this topic, we'll focus on how this target actually functions.</span></span>

<span data-ttu-id="3ad6b-217">最初に、開始タグが含まれることに注意してください、**出力**属性。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-217">First, notice that the opening tag includes an **Outputs** attribute.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


<span data-ttu-id="3ad6b-218">これは、例の*ターゲット バッチ処理*します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-218">This is an example of *target batching*.</span></span> <span data-ttu-id="3ad6b-219">MSBuild プロジェクト ファイルでは、バッチ処理は、コレクションを反復処理するための手法です。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-219">In MSBuild project files, batching is a technique for iterating over collections.</span></span> <span data-ttu-id="3ad6b-220">値、**出力**属性、 **「% (DbPublishPackages.Identity)」** を参照、 **Identity**のメタデータのプロパティ、 **DbPublishPackages**項目のリスト。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-220">The value of the **Outputs** attribute, **"%(DbPublishPackages.Identity)"**, refers to the **Identity** metadata property of the **DbPublishPackages** item list.</span></span> <span data-ttu-id="3ad6b-221">この表記法では、**Outputs=%***(ItemList.ItemMetadataName)*、として変換されます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-221">This notation, **Outputs=%***(ItemList.ItemMetadataName)*, is translated as:</span></span>

- <span data-ttu-id="3ad6b-222">内の項目を分割**DbPublishPackages**同じが含まれている項目のバッチに**Identity**メタデータ値。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-222">Split the items in **DbPublishPackages** into batches of items that contain the same **Identity** metadata value.</span></span>
- <span data-ttu-id="3ad6b-223">バッチごとに 1 回のターゲットを実行します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-223">Execute the target once per batch.</span></span>

> [!NOTE]
> <span data-ttu-id="3ad6b-224">**Identity**の 1 つ、[組み込みメタデータ値](https://msdn.microsoft.com/library/ms164313.aspx)作成時にすべての項目に割り当てられています。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-224">**Identity** is one of the [built-in metadata values](https://msdn.microsoft.com/library/ms164313.aspx) that is assigned to every item on creation.</span></span> <span data-ttu-id="3ad6b-225">値を参照して、 **Include**属性、**項目**要素&#x2014;つまり、パスと、項目のファイル名。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-225">It refers to the value of the **Include** attribute in the **Item** element&#x2014;in other words, the path and filename of the item.</span></span>


<span data-ttu-id="3ad6b-226">この場合、同じパスとファイル名では、複数の項目はならない、ため、本質的に取り組んでいますに 1 つのバッチ サイズ。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-226">In this case, because there should never be more than one item with the same path and filename, we're essentially working with batch sizes of one.</span></span> <span data-ttu-id="3ad6b-227">ターゲットは、データベース パッケージごとに 1 回実行されます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-227">The target is executed once for every database package.</span></span>

<span data-ttu-id="3ad6b-228">同様の表記法を確認できます、  **\_Cmd**プロパティで、適切なスイッチ VSDBCMD コマンドを構築します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-228">You can see a similar notation in the **\_Cmd** property, which builds a VSDBCMD command with the appropriate switches.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


<span data-ttu-id="3ad6b-229">この場合、 **%(DbPublishPackages.DatabaseConnectionString)**、 **%(DbPublishPackages.TargetDatabase)**、および **%(DbPublishPackages.FullPath)** すべてを参照してくださいメタデータの値、 **DbPublishPackages**項目コレクション。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-229">In this case, **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, and **%(DbPublishPackages.FullPath)** all refer to metadata values of the **DbPublishPackages** item collection.</span></span> <span data-ttu-id="3ad6b-230">**\_Cmd**プロパティを使って、 **Exec**タスクは、コマンドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-230">The **\_Cmd** property is used by the **Exec** task, which invokes the command.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


<span data-ttu-id="3ad6b-231">この表記法では、結果として、 **Exec**の一意の組み合わせに基づいてバッチを作成するタスク、 **DatabaseConnectionString**、 **TargetDatabase**、および**FullPath**メタデータの値、およびタスクは、バッチごとに 1 回実行します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-231">As a result of this notation, the **Exec** task will create batches based on unique combinations of the **DatabaseConnectionString**, **TargetDatabase**, and **FullPath** metadata values, and the task will execute once for each batch.</span></span> <span data-ttu-id="3ad6b-232">これは、例の*タスク バッチ処理*します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-232">This is an example of *task batching*.</span></span> <span data-ttu-id="3ad6b-233">ただし、ターゲット レベルのバッチ処理では、項目のコレクションを単一項目のバッチに既に分割があるため、 **Exec**タスクが 1 回とターゲットの繰り返しごとに 1 回だけ実行されます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-233">However, because the target-level batching has already divided our item collection into single-item batches, the **Exec** task will run once and only once for each iteration of the target.</span></span> <span data-ttu-id="3ad6b-234">つまり、このタスクは、ソリューション内の各データベース パッケージに対して 1 回、VSDBCMD ユーティリティを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-234">In other words, this task invokes the VSDBCMD utility once for each database package in the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="3ad6b-235">ターゲットとタスクのバッチ処理の詳細については、MSBuild を参照してください。[バッチ処理](https://msdn.microsoft.com/library/ms171473.aspx)、[ターゲットのバッチの項目メタデータ](https://msdn.microsoft.com/library/ms228229.aspx)、および[タスクのバッチの項目メタデータ](https://msdn.microsoft.com/library/ms171474.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-235">For more information on target and task batching, see MSBuild [Batching](https://msdn.microsoft.com/library/ms171473.aspx), [Item Metadata in Target Batching](https://msdn.microsoft.com/library/ms228229.aspx), and [Item Metadata in Task Batching](https://msdn.microsoft.com/library/ms171474.aspx).</span></span>


### <a name="the-publishwebpackages-target"></a><span data-ttu-id="3ad6b-236">PublishWebPackages ターゲット</span><span class="sxs-lookup"><span data-stu-id="3ad6b-236">The PublishWebPackages Target</span></span>

<span data-ttu-id="3ad6b-237">この段階では、呼び出した、 **BuildProjects**ターゲットで、サンプル ソリューションでは、各プロジェクトの web 配置パッケージを生成します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-237">By this point, you've invoked the **BuildProjects** target, which generates a web deployment package for each project in the sample solution.</span></span> <span data-ttu-id="3ad6b-238">各パッケージに付属する、 *deploy.cmd*ファイルで、ターゲット環境にパッケージを展開するために必要な MSDeploy.exe コマンドが含まれていると*SetParameters.xml*ファイルを指定します、ターゲット環境の必要な詳細情報。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-238">Accompanying each package is a *deploy.cmd* file, which contains the MSDeploy.exe commands required to deploy the package to the target environment, and a *SetParameters.xml* file, which specifies the necessary details of the target environment.</span></span> <span data-ttu-id="3ad6b-239">呼び出したも、 **GatherPackagesForPublishing**ターゲットを含む項目コレクションが生成されますが、 *deploy.cmd*ファイルに関心があります。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-239">You've also invoked the **GatherPackagesForPublishing** target, which generates an item collection containing the *deploy.cmd* files you're interested in.</span></span> <span data-ttu-id="3ad6b-240">基本的には、 **PublishWebPackages**ターゲットは、これらの関数を実行します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-240">Essentially, the **PublishWebPackages** target performs these functions:</span></span>

- <span data-ttu-id="3ad6b-241">操作し、 *SetParameters.xml*ターゲット環境では、適切な詳細を含めるには、各パッケージのファイルを使用して、 **XmlPoke**タスク。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-241">It manipulates the *SetParameters.xml* file for each package to include the correct details for the target environment, using the **XmlPoke** task.</span></span>
- <span data-ttu-id="3ad6b-242">呼び出す、 *deploy.cmd*適切なスイッチを使用して、各パッケージのファイル。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-242">It invokes the *deploy.cmd* file for each package, using the appropriate switches.</span></span>

<span data-ttu-id="3ad6b-243">同じように、 **PublishDbPackages** 、ターゲット、 **PublishWebPackages**ターゲットでターゲット バッチ処理を使用して、ターゲットは、web パッケージごとに 1 回実行されます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-243">Just like the **PublishDbPackages** target, the **PublishWebPackages** target uses target batching to ensure that the target is executed once for each web package.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


<span data-ttu-id="3ad6b-244">ターゲット内で、 **Exec**タスクが実行に使用される、 *deploy.cmd* web パッケージごとにファイル。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-244">Within the target, the **Exec** task is used to run the *deploy.cmd* file for each web package.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


<span data-ttu-id="3ad6b-245">Web パッケージの展開の構成の詳細については、次を参照してください。[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)します。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-245">For more information on configuring the deployment of web packages, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>

## <a name="conclusion"></a><span data-ttu-id="3ad6b-246">まとめ</span><span class="sxs-lookup"><span data-stu-id="3ad6b-246">Conclusion</span></span>

<span data-ttu-id="3ad6b-247">このトピックでは、プロジェクト ファイルの分割を使用して最初から最後の連絡先のマネージャーのサンプル ソリューションのビルドと展開プロセスを制御する方法のチュートリアルが用意されています。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-247">This topic provided a walkthrough of how split project files are used to control the build and deployment process from start to finish for the Contact Manager sample solution.</span></span> <span data-ttu-id="3ad6b-248">このアプローチを使用するを実行する複雑なエンタープライズ規模の展開、反復可能な 1 つの手順では、ファイルを環境に固有のコマンドを実行するだけでできます。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-248">Using this approach lets you run complex, enterprise-scale deployments in a single, repeatable step, simply by running an environment-specific command file.</span></span>

## <a name="further-reading"></a><span data-ttu-id="3ad6b-249">関連項目</span><span class="sxs-lookup"><span data-stu-id="3ad6b-249">Further Reading</span></span>

<span data-ttu-id="3ad6b-250">プロジェクト ファイルと、WPP をより詳細な概要については、次を参照してください。[内で、Microsoft Build Engine: Using MSBuild and Team Foundation ビルド](http://amzn.com/0735645248)Sayed Ibrahim Hashimi、William Bartholomew、ISBN: 978-0-7356-4524-0。</span><span class="sxs-lookup"><span data-stu-id="3ad6b-250">For a more in-depth introduction to project files and the WPP, see [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://amzn.com/0735645248) by Sayed Ibrahim Hashimi and William Bartholomew, ISBN: 978-0-7356-4524-0.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3ad6b-251">[前へ](understanding-the-project-file.md)
> [次へ](building-and-packaging-web-application-projects.md)</span><span class="sxs-lookup"><span data-stu-id="3ad6b-251">[Previous](understanding-the-project-file.md)
[Next](building-and-packaging-web-application-projects.md)</span></span>
