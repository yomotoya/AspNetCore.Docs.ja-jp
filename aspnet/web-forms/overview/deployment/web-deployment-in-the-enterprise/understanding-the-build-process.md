---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: ビルド プロセスを理解する |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、エンタープライズ規模のビルドおよび配置プロセスのチュートリアルを提供します。 このトピックで説明されているアプローチは、カスタムの Microsoft ビルド エンジニアを使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 4544a5e6212ea9b1247062dc35edc135ff7ca354
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888001"
---
<a name="understanding-the-build-process"></a><span data-ttu-id="91c97-104">ビルド プロセスの理解</span><span class="sxs-lookup"><span data-stu-id="91c97-104">Understanding the Build Process</span></span>
====================
<span data-ttu-id="91c97-105">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="91c97-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="91c97-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="91c97-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="91c97-107">このトピックでは、エンタープライズ規模のビルドおよび配置プロセスのチュートリアルを提供します。</span><span class="sxs-lookup"><span data-stu-id="91c97-107">This topic provides a walkthrough of an enterprise-scale build and deployment process.</span></span> <span data-ttu-id="91c97-108">このトピックで説明されているアプローチは、プロセスのすべての側面の詳細に制御を提供するのにカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="91c97-108">The approach described in this topic uses custom Microsoft Build Engine (MSBuild) project files to provide fine-grained control over every aspect of the process.</span></span> <span data-ttu-id="91c97-109">プロジェクト ファイル内では、カスタム MSBuild ターゲットは、インターネット インフォメーション サービス (IIS) Web 配置ツール (MSDeploy.exe) などの展開ユーティリティおよび VSDBCMD.exe データベース配置ユーティリティを実行に使用されます。</span><span class="sxs-lookup"><span data-stu-id="91c97-109">Within the project files, custom MSBuild targets are used to run deployment utilities like the Internet Information Services (IIS) Web Deployment Tool (MSDeploy.exe) and the database deployment utility VSDBCMD.exe.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="91c97-110">前のトピックでは、[プロジェクト ファイルを理解する](understanding-the-project-file.md)MSBuild プロジェクト ファイルの主要なコンポーネントの説明、および複数のターゲット環境に展開をサポートするプロジェクト ファイルの分割の概念が導入されました。</span><span class="sxs-lookup"><span data-stu-id="91c97-110">The previous topic, [Understanding the Project File](understanding-the-project-file.md), described the key components of an MSBuild project file and introduced the concept of split project files to support deployment to multiple target environments.</span></span> <span data-ttu-id="91c97-111">わからない場合既にこれらの概念を理解して、確認してください[プロジェクト ファイルを理解する](understanding-the-project-file.md)このトピックの内容を実行する前にします。</span><span class="sxs-lookup"><span data-stu-id="91c97-111">If you're not already familiar with these concepts, you should review [Understanding the Project File](understanding-the-project-file.md) before you work through this topic.</span></span>


<span data-ttu-id="91c97-112">このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="91c97-112">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="91c97-113">説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。</span><span class="sxs-lookup"><span data-stu-id="91c97-113">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="91c97-114">ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="91c97-114">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="build-and-deployment-overview"></a><span data-ttu-id="91c97-115">ビルドと展開の概要</span><span class="sxs-lookup"><span data-stu-id="91c97-115">Build and Deployment Overview</span></span>

<span data-ttu-id="91c97-116">[Contact Manager ソリューション](the-contact-manager-solution.md)、3 つのファイルがビルドおよび配置プロセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="91c97-116">In the [Contact Manager solution](the-contact-manager-solution.md), three files control the build and deployment process:</span></span>

- <span data-ttu-id="91c97-117">A*ユニバーサル プロジェクト ファイル*(*Publish.proj*)。</span><span class="sxs-lookup"><span data-stu-id="91c97-117">A *universal project file* (*Publish.proj*).</span></span> <span data-ttu-id="91c97-118">これには、移行先の環境間で変更されないビルドおよび展開の説明が含まれています。</span><span class="sxs-lookup"><span data-stu-id="91c97-118">This contains build and deployment instructions that do not change between destination environments.</span></span>
- <span data-ttu-id="91c97-119">*環境固有のプロジェクト ファイル*(*Env Dev.proj*)。</span><span class="sxs-lookup"><span data-stu-id="91c97-119">An *environment-specific project file* (*Env-Dev.proj*).</span></span> <span data-ttu-id="91c97-120">これには、特定の送信先の環境に固有のビルドと配置の設定が含まれています。</span><span class="sxs-lookup"><span data-stu-id="91c97-120">This contains build and deployment settings that are specific to a particular destination environment.</span></span> <span data-ttu-id="91c97-121">たとえば、使用する、 *Env Dev.proj*開発者またはテスト環境の設定を入力し、という代替ファイルを作成するファイル*Env Stage.proj*ステージングの設定を指定環境。</span><span class="sxs-lookup"><span data-stu-id="91c97-121">For example, you could use the *Env-Dev.proj* file to provide settings for a developer or test environment and create an alternative file named *Env-Stage.proj* to provide settings for a staging environment.</span></span>
- <span data-ttu-id="91c97-122">A*コマンド ファイル*(*発行 Dev.cmd*)。</span><span class="sxs-lookup"><span data-stu-id="91c97-122">A *command file* (*Publish-Dev.cmd*).</span></span> <span data-ttu-id="91c97-123">これには、プロジェクト ファイルを指定するコマンドを実行する場合、MSBuild.exe が含まれています。</span><span class="sxs-lookup"><span data-stu-id="91c97-123">This contains an MSBuild.exe command that specifies which project files you want to execute.</span></span> <span data-ttu-id="91c97-124">ファイルごとに別の環境に固有のプロジェクト ファイルを指定する MSBuild.exe コマンドが含まれています、移行先環境ごとにコマンド ファイルを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="91c97-124">You can create a command file for every destination environment, where each file contains an MSBuild.exe command that specifies a different environment-specific project file.</span></span> <span data-ttu-id="91c97-125">開発者はでき、適切なコマンド ファイルを実行するだけで、別の環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="91c97-125">This lets the developer deploy to different environments simply by running the appropriate command file.</span></span>

<span data-ttu-id="91c97-126">サンプル ソリューションでは、発行ソリューション フォルダーにこれら 3 つのファイルを検索できます。</span><span class="sxs-lookup"><span data-stu-id="91c97-126">In the sample solution, you can find these three files in the Publish solution folder.</span></span>

![](understanding-the-build-process/_static/image1.png)

<span data-ttu-id="91c97-127">これらのファイルをさらに詳しく見ると、前にこのアプローチを使用するときの全体的なビルド処理のしくみを見てをみましょう。</span><span class="sxs-lookup"><span data-stu-id="91c97-127">Before you look at these files in more detail, let's take a look at how the overall build process works when you use this approach.</span></span> <span data-ttu-id="91c97-128">大まかに言えば、次のようなビルドおよび配置プロセス。</span><span class="sxs-lookup"><span data-stu-id="91c97-128">At a high level, the build and deployment process looks like this:</span></span>

![](understanding-the-build-process/_static/image2.png)

<span data-ttu-id="91c97-129">最初に行われるは、2 つのプロジェクト ファイル&#x2014;と環境固有の設定を含む 1 つの汎用のビルドと配置の手順を含む&#x2014;1 つのプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="91c97-129">The first thing that happens is that the two project files&#x2014;one containing universal build and deployment instructions, and one containing environment-specific settings&#x2014;are merged into a single project file.</span></span> <span data-ttu-id="91c97-130">MSBuild は、プロジェクト ファイル内の命令を動作します。</span><span class="sxs-lookup"><span data-stu-id="91c97-130">MSBuild then works through the instructions in the project file.</span></span> <span data-ttu-id="91c97-131">各ソリューションでは、各プロジェクトのプロジェクト ファイルを使用してプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="91c97-131">It builds each of the projects in the solution, using the project file for each project.</span></span> <span data-ttu-id="91c97-132">するために呼び出す Web Deploy (MSDeploy.exe) など、他のツールと、web コンテンツやデータベースを対象となる環境に展開する VSDBCMD ユーティリティです。</span><span class="sxs-lookup"><span data-stu-id="91c97-132">It then calls out to other tools, like Web Deploy (MSDeploy.exe) and the VSDBCMD utility to deploy your web content and databases to the target environment.</span></span>

<span data-ttu-id="91c97-133">開始から終了までは、ビルドおよび配置プロセスは、これらのタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="91c97-133">From start to finish, the build and deployment process performs these tasks:</span></span>

1. <span data-ttu-id="91c97-134">新しいビルドの準備として、出力ディレクトリの内容を削除します。</span><span class="sxs-lookup"><span data-stu-id="91c97-134">It deletes the contents of the output directory, in preparation for a fresh build.</span></span>
2. <span data-ttu-id="91c97-135">ソリューション内の各プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="91c97-135">It builds each project in the solution:</span></span>

    1. <span data-ttu-id="91c97-136">Web プロジェクトの&#x2014;ここでは、ASP.NET MVC web アプリケーションと WCF web サービス&#x2014;ビルド処理は、各プロジェクトの web 配置パッケージを作成します。</span><span class="sxs-lookup"><span data-stu-id="91c97-136">For web projects&#x2014;in this case, an ASP.NET MVC web application and a WCF web service&#x2014;the build process creates a web deployment package for each project.</span></span>
    2. <span data-ttu-id="91c97-137">データベース プロジェクトの場合は、ビルド プロセスは、各プロジェクトの配置マニフェスト (.deploymanifest ファイル) を作成します。</span><span class="sxs-lookup"><span data-stu-id="91c97-137">For database projects, the build process creates a deployment manifest (.deploymanifest file) for each project.</span></span>
3. <span data-ttu-id="91c97-138">VSDBCMD.exe ユーティリティを使用して、プロジェクト ファイルからさまざまなプロパティを使用して、ソリューション内の各データベース プロジェクトを配置を&#x2014;ターゲット接続文字列とデータベース名の&#x2014;.deploymanifest ファイルと共にします。</span><span class="sxs-lookup"><span data-stu-id="91c97-138">It uses the VSDBCMD.exe utility to deploy each database project in the solution, using various properties from the project files&#x2014;a target connection string and a database name&#x2014;together with the .deploymanifest file.</span></span>
4. <span data-ttu-id="91c97-139">これは、MSDeploy.exe ユーティリティを使用して、ソリューションでは、プロジェクト ファイルからのさまざまなプロパティを使用して、展開プロセスを制御するには、各 web プロジェクトを配置します。</span><span class="sxs-lookup"><span data-stu-id="91c97-139">It uses the MSDeploy.exe utility to deploy each web project in the solution, using various properties from the project files to control the deployment process.</span></span>

<span data-ttu-id="91c97-140">サンプル ソリューションを使用すると、このプロセスの詳細をトレースします。</span><span class="sxs-lookup"><span data-stu-id="91c97-140">You can use the sample solution to trace this process in more detail.</span></span>

> [!NOTE]
> <span data-ttu-id="91c97-141">サーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)です。</span><span class="sxs-lookup"><span data-stu-id="91c97-141">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


## <a name="invoking-the-build-and-deployment-process"></a><span data-ttu-id="91c97-142">ビルドと展開プロセスの呼び出し</span><span class="sxs-lookup"><span data-stu-id="91c97-142">Invoking the Build and Deployment Process</span></span>

<span data-ttu-id="91c97-143">開発者が実行に連絡先のマネージャー ソリューション開発者のテスト環境を配置するのには、*発行 Dev.cmd*コマンド ファイルです。</span><span class="sxs-lookup"><span data-stu-id="91c97-143">To deploy the Contact Manager solution to a developer test environment, the developer runs the *Publish-Dev.cmd* command file.</span></span> <span data-ttu-id="91c97-144">MSBuild.exe を起動を指定する*Publish.proj* 、プロジェクト ファイルを実行すると*Env Dev.proj*パラメーターの値として。</span><span class="sxs-lookup"><span data-stu-id="91c97-144">This invokes MSBuild.exe, specifying *Publish.proj* as the project file to execute and *Env-Dev.proj* as a parameter value.</span></span>


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> <span data-ttu-id="91c97-145">**/Fl**スイッチ (の略 **/fileLogger**) ビルドの出力をという名前のファイルに記録*msbuild.log になります*現在のディレクトリにします。</span><span class="sxs-lookup"><span data-stu-id="91c97-145">The **/fl** switch (short for **/fileLogger**) logs the build output to a file named *msbuild.log* in the current directory.</span></span> <span data-ttu-id="91c97-146">詳細については、次を参照してください。、 [MSBuild コマンド ライン リファレンス](https://msdn.microsoft.com/library/ms164311.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="91c97-146">For more information, see the [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>


<span data-ttu-id="91c97-147">この時点では、MSBuild の実行を開始、ロード、 *Publish.proj*ファイル、およびその中の手順の処理を開始します。</span><span class="sxs-lookup"><span data-stu-id="91c97-147">At this point, MSBuild starts running, loads the *Publish.proj* file, and starts processing the instructions within it.</span></span> <span data-ttu-id="91c97-148">最初の命令が MSBuild プロジェクトをインポートするように指示ファイルを**TargetEnvPropsFile**パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="91c97-148">The first instruction tells MSBuild to import the project file that the **TargetEnvPropsFile** parameter specifies.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


<span data-ttu-id="91c97-149">**TargetEnvPropsFile**パラメーターを指定します、 *Env Dev.proj*ファイル、MSBuild の内容をマージするため、 *Env Dev.proj*ファイルを*Publish.proj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="91c97-149">The **TargetEnvPropsFile** parameter specifies the *Env-Dev.proj* file, so MSBuild merges the contents of the *Env-Dev.proj* file into the *Publish.proj* file.</span></span>

<span data-ttu-id="91c97-150">MSBuild がマージされたプロジェクト ファイルで発生する次の要素は、プロパティのグループです。</span><span class="sxs-lookup"><span data-stu-id="91c97-150">The next elements that MSBuild encounters in the merged project file are property groups.</span></span> <span data-ttu-id="91c97-151">プロパティは、ファイル内に出現する順序で処理されます。</span><span class="sxs-lookup"><span data-stu-id="91c97-151">Properties are processed in the order in which they appear in the file.</span></span> <span data-ttu-id="91c97-152">MSBuild では、指定した条件を満たしていることを提供する各プロパティのキー/値ペアを作成します。</span><span class="sxs-lookup"><span data-stu-id="91c97-152">MSBuild creates a key-value pair for each property, providing that any specified conditions are met.</span></span> <span data-ttu-id="91c97-153">後で定義されているファイルのプロパティ ファイルで以前に定義されている同じ名前を持つ任意のプロパティが上書きされます。</span><span class="sxs-lookup"><span data-stu-id="91c97-153">Properties defined later in the file will overwrite any properties with the same name defined earlier in the file.</span></span> <span data-ttu-id="91c97-154">たとえば、 **OutputRoot**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="91c97-154">For example, consider the **OutputRoot** properties.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


<span data-ttu-id="91c97-155">MSBuild が 1 つ目を処理するときに**OutputRoot**要素、同様に名前付きパラメーターを提供することが指定されていませんの値を設定、 **OutputRoot**プロパティを **.\Publish\Out**です。2 つ目が出現したとき**OutputRoot**に条件が評価された場合、要素**true**の値は、上書きされます、 **OutputRoot**プロパティの値と、**OutDir**パラメーター。</span><span class="sxs-lookup"><span data-stu-id="91c97-155">When MSBuild processes the first **OutputRoot** element, providing a similarly named parameter has not been provided, it sets the value of the **OutputRoot** property to **..\Publish\Out**. When it encounters the second **OutputRoot** element, if the condition evaluates to **true**, it will overwrite the value of the **OutputRoot** property with the value of the **OutDir** parameter.</span></span>

<span data-ttu-id="91c97-156">MSBuild が発生した次の要素は、という名前の項目を含む、1 つの項目グループ**ProjectsToBuild**です。</span><span class="sxs-lookup"><span data-stu-id="91c97-156">The next element that MSBuild encounters is a single item group, containing an item named **ProjectsToBuild**.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


<span data-ttu-id="91c97-157">MSBuild では、この命令を処理という名前の項目リストを構築して**ProjectsToBuild**です。</span><span class="sxs-lookup"><span data-stu-id="91c97-157">MSBuild processes this instruction by building an item list named **ProjectsToBuild**.</span></span> <span data-ttu-id="91c97-158">ここでは、項目リストが 1 つの値が含まれます&#x2014;ソリューション ファイルのファイル名とパス。</span><span class="sxs-lookup"><span data-stu-id="91c97-158">In this case, the item list contains a single value&#x2014;the path and filename of the solution file.</span></span>

<span data-ttu-id="91c97-159">この時点では、残りの要素は、ターゲットです。</span><span class="sxs-lookup"><span data-stu-id="91c97-159">At this point, the remaining elements are targets.</span></span> <span data-ttu-id="91c97-160">プロパティと項目からターゲットを異なる方法で処理されます&#x2014;明示的にユーザーによって指定またはされるプロジェクト ファイル内の別の構成体によって呼び出された場合を除き、基本的には、ターゲットは処理されません。</span><span class="sxs-lookup"><span data-stu-id="91c97-160">Targets are processed differently from properties and items&#x2014;essentially, targets are not processed unless they are either explicitly specified by the user or invoked by another construct within the project file.</span></span> <span data-ttu-id="91c97-161">注意してください、opening**プロジェクト**タグが含まれています、 **DefaultTargets**属性。</span><span class="sxs-lookup"><span data-stu-id="91c97-161">Recall that the opening **Project** tag includes a **DefaultTargets** attribute.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


<span data-ttu-id="91c97-162">これは、msbuild でを呼び出す、 **FullPublish**ターゲット、ターゲットがない場合は、MSBuild.exe が呼び出される場合を指定します。</span><span class="sxs-lookup"><span data-stu-id="91c97-162">This instructs MSBuild to invoke the **FullPublish** target, if targets are not specified when MSBuild.exe is invoked.</span></span> <span data-ttu-id="91c97-163">**FullPublish**ターゲットにはすべてのタスクが含まれていない代わりに単を指定します。 依存関係の一覧です。</span><span class="sxs-lookup"><span data-stu-id="91c97-163">The **FullPublish** target doesn't contain any tasks; instead it simply specifies a list of dependencies.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


<span data-ttu-id="91c97-164">この依存関係が MSBuild をさせるにを実行するように指示、 **FullPublish**ターゲットをこの順でターゲットの一覧を呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="91c97-164">This dependency tells MSBuild that in order to execute the **FullPublish** target, it needs to invoke this list of targets in the order provided:</span></span>

1. <span data-ttu-id="91c97-165">これを呼び出す必要があります、**クリーン**ターゲットです。</span><span class="sxs-lookup"><span data-stu-id="91c97-165">It must invoke the **Clean** target.</span></span>
2. <span data-ttu-id="91c97-166">これを呼び出す必要があります、 **BuildProjects**ターゲットです。</span><span class="sxs-lookup"><span data-stu-id="91c97-166">It must invoke the **BuildProjects** target.</span></span>
3. <span data-ttu-id="91c97-167">これを呼び出す必要があります、 **GatherPackagesForPublishing**ターゲットです。</span><span class="sxs-lookup"><span data-stu-id="91c97-167">It must invoke the **GatherPackagesForPublishing** target.</span></span>
4. <span data-ttu-id="91c97-168">これを呼び出す必要があります、 **PublishDbPackages**ターゲットです。</span><span class="sxs-lookup"><span data-stu-id="91c97-168">It must invoke the **PublishDbPackages** target.</span></span>
5. <span data-ttu-id="91c97-169">これを呼び出す必要があります、 **PublishWebPackages**ターゲットです。</span><span class="sxs-lookup"><span data-stu-id="91c97-169">It must invoke the **PublishWebPackages** target.</span></span>

### <a name="the-clean-target"></a><span data-ttu-id="91c97-170">Clean ターゲット</span><span class="sxs-lookup"><span data-stu-id="91c97-170">The Clean Target</span></span>

<span data-ttu-id="91c97-171">**クリーン**ターゲット基本的には、出力ディレクトリとそのすべての内容として削除新しくビルドを準備します。</span><span class="sxs-lookup"><span data-stu-id="91c97-171">The **Clean** target basically deletes the output directory and all its contents, as preparation for a fresh build.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


<span data-ttu-id="91c97-172">ターゲットを含む通知、 **ItemGroup**要素。</span><span class="sxs-lookup"><span data-stu-id="91c97-172">Notice that the target includes an **ItemGroup** element.</span></span> <span data-ttu-id="91c97-173">プロパティまたは内の項目を定義する場合、**ターゲット**要素を作成する*動的*プロパティと項目。</span><span class="sxs-lookup"><span data-stu-id="91c97-173">When you define properties or items within a **Target** element, you're creating *dynamic* properties and items.</span></span> <span data-ttu-id="91c97-174">つまり、プロパティまたは項目は、ターゲットが実行されるまでに処理されません。</span><span class="sxs-lookup"><span data-stu-id="91c97-174">In other words, the properties or items aren't processed until the target is executed.</span></span> <span data-ttu-id="91c97-175">出力ディレクトリが存在しないか、ビルドすることはできませんので、ビルド プロセスが開始されるまで、ファイルを含める、  **\_FilesToDelete**静的アイテムとして一覧以外の実行は進行するまで待機する必要があります。</span><span class="sxs-lookup"><span data-stu-id="91c97-175">The output directory might not exist or contain any files until the build process begins, so you can't build the **\_FilesToDelete** list as a static item; you have to wait until execution is underway.</span></span> <span data-ttu-id="91c97-176">そのため、リストを作成するには、ターゲット内の動的アイテムとして。</span><span class="sxs-lookup"><span data-stu-id="91c97-176">As such, you build the list as a dynamic item within the target.</span></span>

> [!NOTE]
> <span data-ttu-id="91c97-177">この場合、ため、**クリーン**ターゲットは、最初に実行される、実際には動的なグループを使用する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="91c97-177">In this case, because the **Clean** target is the first to be executed, there's no real need to use a dynamic item group.</span></span> <span data-ttu-id="91c97-178">ただし、お勧めこの種類のシナリオでは、動的なプロパティと項目を使用するようにいくつかの時点では異なる順序でターゲットを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="91c97-178">However, it's good practice to use dynamic properties and items in this type of scenario, as you might want to execute targets in a different order at some point.</span></span>  
> <span data-ttu-id="91c97-179">使用しないアイテムを宣言することを回避する目指す必要があります。</span><span class="sxs-lookup"><span data-stu-id="91c97-179">You should also aim to avoid declaring items that will never be used.</span></span> <span data-ttu-id="91c97-180">特定のターゲットでのみ使用される項目をある場合は、ビルド プロセスで、不要なオーバーヘッドを削除する対象の内側に配置することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="91c97-180">If you have items that will only be used by a specific target, consider placing them inside the target to remove any unnecessary overhead on the build process.</span></span>


<span data-ttu-id="91c97-181">項目を別に、動的、**クリーン**ターゲットが簡単にし、組み込みの利用**メッセージ**、**削除**、および**RemoveDir**へのタスクします。</span><span class="sxs-lookup"><span data-stu-id="91c97-181">Dynamic items aside, the **Clean** target is fairly straightforward and makes use of the built-in **Message**, **Delete**, and **RemoveDir** tasks to:</span></span>

1. <span data-ttu-id="91c97-182">ロガーにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="91c97-182">Send a message to the logger.</span></span>
2. <span data-ttu-id="91c97-183">削除するファイルの一覧を作成します。</span><span class="sxs-lookup"><span data-stu-id="91c97-183">Build a list of files to delete.</span></span>
3. <span data-ttu-id="91c97-184">ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="91c97-184">Delete the files.</span></span>
4. <span data-ttu-id="91c97-185">出力ディレクトリを削除します。</span><span class="sxs-lookup"><span data-stu-id="91c97-185">Remove the output directory.</span></span>

### <a name="the-buildprojects-target"></a><span data-ttu-id="91c97-186">BuildProjects ターゲット</span><span class="sxs-lookup"><span data-stu-id="91c97-186">The BuildProjects Target</span></span>

<span data-ttu-id="91c97-187">**BuildProjects**ターゲットは基本的には、サンプル ソリューション内のすべてのプロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="91c97-187">The **BuildProjects** target basically builds all the projects in the sample solution.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


<span data-ttu-id="91c97-188">このターゲットは、前のトピックでは、ある程度詳細に説明した[プロジェクト ファイルを理解する](understanding-the-project-file.md)タスクとコア ターゲット プロパティと項目の参照方法を説明するためにします。</span><span class="sxs-lookup"><span data-stu-id="91c97-188">This target was described in some detail in the previous topic, [Understanding the Project File](understanding-the-project-file.md), to illustrate how tasks and targets reference properties and items.</span></span> <span data-ttu-id="91c97-189">この時点では、主に関心、 **MSBuild**タスク。</span><span class="sxs-lookup"><span data-stu-id="91c97-189">At this point, you're mainly interested in the **MSBuild** task.</span></span> <span data-ttu-id="91c97-190">このタスクを使用すると、複数のプロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="91c97-190">You can use this task to build multiple projects.</span></span> <span data-ttu-id="91c97-191">タスクが MSBuild.exe; の新しいインスタンスを作成できません。各プロジェクトをビルドするのに現在の実行中のインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="91c97-191">The task does not create a new instance of MSBuild.exe; it uses the current running instance to build each project.</span></span> <span data-ttu-id="91c97-192">この例では関心のある重要な点は、展開プロパティを示します。</span><span class="sxs-lookup"><span data-stu-id="91c97-192">The key points of interest in this example are the deployment properties:</span></span>

- <span data-ttu-id="91c97-193">**DeployOnBuild**プロパティには、各プロジェクトのビルドが完了すると、プロジェクトの設定で、デプロイの手順を実行する MSBuild がよう指示します。</span><span class="sxs-lookup"><span data-stu-id="91c97-193">The **DeployOnBuild** property instructs MSBuild to run any deployment instructions in the project settings when the build of each project is complete.</span></span>
- <span data-ttu-id="91c97-194">**DeployTarget**プロパティは、プロジェクトをビルドした後、呼び出し先のターゲットを識別します。</span><span class="sxs-lookup"><span data-stu-id="91c97-194">The **DeployTarget** property identifies the target that you want to invoke after the project is built.</span></span> <span data-ttu-id="91c97-195">ここで、**パッケージ**ターゲットが配置可能な web のパッケージにプロジェクトの出力をビルドします。</span><span class="sxs-lookup"><span data-stu-id="91c97-195">In this case, the **Package** target builds the project output into a deployable web package.</span></span>

> [!NOTE]
> <span data-ttu-id="91c97-196">**パッケージ**ターゲット呼び出して、Web 発行パイプライン (WPP)、MSBuild および Web デプロイの間の統合を提供します。</span><span class="sxs-lookup"><span data-stu-id="91c97-196">The **Package** target invokes the Web Publishing Pipeline (WPP), which provides integration between MSBuild and Web Deploy.</span></span> <span data-ttu-id="91c97-197">見て確認、組み込みターゲット、WPP が提供する場合、 *Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web フォルダー内のファイルです。</span><span class="sxs-lookup"><span data-stu-id="91c97-197">If you want to take a look at the built-in targets that the WPP provides, review the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span>


### <a name="the-gatherpackagesforpublishing-target"></a><span data-ttu-id="91c97-198">GatherPackagesForPublishing ターゲット</span><span class="sxs-lookup"><span data-stu-id="91c97-198">The GatherPackagesForPublishing Target</span></span>

<span data-ttu-id="91c97-199">調査する場合、 **GatherPackagesForPublishing**ターゲットわかりますを実際にが含まれていないすべてのタスクです。</span><span class="sxs-lookup"><span data-stu-id="91c97-199">If you study the **GatherPackagesForPublishing** target, you'll notice that it doesn't actually contain any tasks.</span></span> <span data-ttu-id="91c97-200">代わりに、3 つの動的な項目を定義する 1 つの項目グループが含まれています。</span><span class="sxs-lookup"><span data-stu-id="91c97-200">Instead, it contains a single item group that defines three dynamic items.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


<span data-ttu-id="91c97-201">これらの項目は、ときに作成された展開パッケージを参照してください、 **BuildProjects**ターゲットが実行されました。</span><span class="sxs-lookup"><span data-stu-id="91c97-201">These items refer to the deployment packages that were created when the **BuildProjects** target was executed.</span></span> <span data-ttu-id="91c97-202">できませんでした。 これらの項目を静的に定義プロジェクト ファイルでまでの項目が参照するファイルが存在しないため、 **BuildProjects**ターゲットを実行します。</span><span class="sxs-lookup"><span data-stu-id="91c97-202">You couldn't define these items statically in the project file, because the files to which the items refer don't exist until the **BuildProjects** target is executed.</span></span> <span data-ttu-id="91c97-203">代わりに、項目必要がありますに動的に内で定義するまでは呼び出されませんターゲット後、 **BuildProjects**ターゲットを実行します。</span><span class="sxs-lookup"><span data-stu-id="91c97-203">Instead, the items must be defined dynamically within a target that is not invoked until after the **BuildProjects** target is executed.</span></span>

<span data-ttu-id="91c97-204">このターゲット内の項目は使用されません&#x2014;項目と各項目の値に関連付けられているメタデータは、このターゲットを単にビルドします。</span><span class="sxs-lookup"><span data-stu-id="91c97-204">The items are not used within this target&#x2014;this target simply builds the items and the metadata associated with each item value.</span></span> <span data-ttu-id="91c97-205">これらの要素を処理した後、 **PublishPackages**項目が 2 つの値へのパスを含む、 *ContactManager.Mvc.deploy.cmd*ファイルとパスを*ContactManager.Service.deploy.cmd*ファイル。</span><span class="sxs-lookup"><span data-stu-id="91c97-205">Once these elements are processed, the **PublishPackages** item will contain two values, the path to the *ContactManager.Mvc.deploy.cmd* file and the path to the *ContactManager.Service.deploy.cmd* file.</span></span> <span data-ttu-id="91c97-206">Web Deploy は、各プロジェクトの web パッケージの一部としてこれらのファイルを作成し、これらは、呼び出す必要のあるファイル、パッケージを展開するために、移行先サーバーにします。</span><span class="sxs-lookup"><span data-stu-id="91c97-206">Web Deploy creates these files as part of the web package for each project, and these are the files that you must invoke on the destination server in order to deploy the packages.</span></span> <span data-ttu-id="91c97-207">これらのファイルのいずれかを開くには場合、基本的にはさまざまなビルドに固有のパラメーター値を持つ、MSDeploy.exe コマンドが表示されます。</span><span class="sxs-lookup"><span data-stu-id="91c97-207">If you open up one of these files, you'll basically see an MSDeploy.exe command with various build-specific parameter values.</span></span>

<span data-ttu-id="91c97-208">**DbPublishPackages**項目は、単一の値へのパスを含む、 *ContactManager.Database.deploymanifest*ファイル。</span><span class="sxs-lookup"><span data-stu-id="91c97-208">The **DbPublishPackages** item will contain a single value, the path to the *ContactManager.Database.deploymanifest* file.</span></span>

> [!NOTE]
> <span data-ttu-id="91c97-209">データベース プロジェクトをビルドして、MSBuild プロジェクト ファイルと同じスキーマを使用して .deploymanifest ファイルが生成されます。</span><span class="sxs-lookup"><span data-stu-id="91c97-209">A .deploymanifest file is generated when you build a database project, and it uses the same schema as an MSBuild project file.</span></span> <span data-ttu-id="91c97-210">データベース スキーマ (.dbschema) の場所と任意の配置前や配置後スクリプトの詳細を含む、データベースを展開するために必要なすべての情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="91c97-210">It contains all the information required to deploy a database, including the location of the database schema (.dbschema) and details of any pre-deployment and post-deployment scripts.</span></span> <span data-ttu-id="91c97-211">詳細については、次を参照してください。 [、概要のデータベースのビルドと配置](https://msdn.microsoft.com/library/aa833165.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="91c97-211">For more information, see [An Overview of Database Build and Deployment](https://msdn.microsoft.com/library/aa833165.aspx).</span></span>


<span data-ttu-id="91c97-212">展開パッケージとデータベースの配置マニフェストの作成およびの使用方法の詳細を学習[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)と[データベース プロジェクトの配置](deploying-database-projects.md)です。</span><span class="sxs-lookup"><span data-stu-id="91c97-212">You'll learn more about how deployment packages and database deployment manifests are created and used in [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md) and [Deploying Database Projects](deploying-database-projects.md).</span></span>

### <a name="the-publishdbpackages-target"></a><span data-ttu-id="91c97-213">PublishDbPackages ターゲット</span><span class="sxs-lookup"><span data-stu-id="91c97-213">The PublishDbPackages Target</span></span>

<span data-ttu-id="91c97-214">簡単に言うと、 **PublishDbPackages**ターゲットは、展開する VSDBCMD ユーティリティを呼び出して、 **ContactManager**をターゲット環境にデータベース。</span><span class="sxs-lookup"><span data-stu-id="91c97-214">Briefly speaking, the **PublishDbPackages** target invokes the VSDBCMD utility to deploy the **ContactManager** database to a target environment.</span></span> <span data-ttu-id="91c97-215">決定および深入りの多くは、データベースの配置を構成して、これにはの詳細を学習[データベース プロジェクトの配置](deploying-database-projects.md)と[の複数の環境のデータベースの配置をカスタマイズする](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).</span><span class="sxs-lookup"><span data-stu-id="91c97-215">Configuring database deployment involves lots of decisions and nuances, and you'll learn more about this in [Deploying Database Projects](deploying-database-projects.md) and [Customizing Database Deployments for Multiple Environments](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).</span></span> <span data-ttu-id="91c97-216">このトピックでは、このターゲットが実際に機能する方法に注目します。</span><span class="sxs-lookup"><span data-stu-id="91c97-216">In this topic, we'll focus on how this target actually functions.</span></span>

<span data-ttu-id="91c97-217">最初に、開始タグが含まれていることを確認、**出力**属性。</span><span class="sxs-lookup"><span data-stu-id="91c97-217">First, notice that the opening tag includes an **Outputs** attribute.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


<span data-ttu-id="91c97-218">これは、例の*ターゲットのバッチ*です。</span><span class="sxs-lookup"><span data-stu-id="91c97-218">This is an example of *target batching*.</span></span> <span data-ttu-id="91c97-219">MSBuild プロジェクト ファイルでは、バッチ処理は、コレクションを反復処理するための手法です。</span><span class="sxs-lookup"><span data-stu-id="91c97-219">In MSBuild project files, batching is a technique for iterating over collections.</span></span> <span data-ttu-id="91c97-220">値、**出力**属性、 **「% (DbPublishPackages.Identity)」** を参照、 **Identity**のメタデータのプロパティ、 **DbPublishPackages**項目のリスト。</span><span class="sxs-lookup"><span data-stu-id="91c97-220">The value of the **Outputs** attribute, **"%(DbPublishPackages.Identity)"**, refers to the **Identity** metadata property of the **DbPublishPackages** item list.</span></span> <span data-ttu-id="91c97-221">この表記 **Outputs=%***(ItemList.ItemMetadataName)*、として変換されます。</span><span class="sxs-lookup"><span data-stu-id="91c97-221">This notation, **Outputs=%***(ItemList.ItemMetadataName)*, is translated as:</span></span>

- <span data-ttu-id="91c97-222">内の項目を分割**DbPublishPackages**同じが含まれる項目のバッチに**Identity**メタデータ値。</span><span class="sxs-lookup"><span data-stu-id="91c97-222">Split the items in **DbPublishPackages** into batches of items that contain the same **Identity** metadata value.</span></span>
- <span data-ttu-id="91c97-223">バッチごとに 1 回のターゲットが実行されます。</span><span class="sxs-lookup"><span data-stu-id="91c97-223">Execute the target once per batch.</span></span>

> [!NOTE]
> <span data-ttu-id="91c97-224">**Identity**の 1 つ、[組み込みメタデータ値](https://msdn.microsoft.com/library/ms164313.aspx)作成時にすべての項目に割り当てられています。</span><span class="sxs-lookup"><span data-stu-id="91c97-224">**Identity** is one of the [built-in metadata values](https://msdn.microsoft.com/library/ms164313.aspx) that is assigned to every item on creation.</span></span> <span data-ttu-id="91c97-225">値を参照して、 **Include**属性、**項目**要素&#x2014;言い換えれば、パスとファイル名は、項目の。</span><span class="sxs-lookup"><span data-stu-id="91c97-225">It refers to the value of the **Include** attribute in the **Item** element&#x2014;in other words, the path and filename of the item.</span></span>


<span data-ttu-id="91c97-226">この場合、同じパスとファイル名を持つ 2 つ以上の項目することはありません必要があります、本質的に取り組んでいますに 1 つのバッチ サイズ。</span><span class="sxs-lookup"><span data-stu-id="91c97-226">In this case, because there should never be more than one item with the same path and filename, we're essentially working with batch sizes of one.</span></span> <span data-ttu-id="91c97-227">ターゲットは、データベース パッケージごとに 1 回実行します。</span><span class="sxs-lookup"><span data-stu-id="91c97-227">The target is executed once for every database package.</span></span>

<span data-ttu-id="91c97-228">同様の表記法を表示できます、  **\_Cmd**プロパティで、適切なスイッチ VSDBCMD コマンドを構築します。</span><span class="sxs-lookup"><span data-stu-id="91c97-228">You can see a similar notation in the **\_Cmd** property, which builds a VSDBCMD command with the appropriate switches.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


<span data-ttu-id="91c97-229">この場合、 **%(DbPublishPackages.DatabaseConnectionString)**、 **%(DbPublishPackages.TargetDatabase)**、および **%(DbPublishPackages.FullPath)** すべてを参照してくださいメタデータの値、 **DbPublishPackages**項目のコレクション。</span><span class="sxs-lookup"><span data-stu-id="91c97-229">In this case, **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, and **%(DbPublishPackages.FullPath)** all refer to metadata values of the **DbPublishPackages** item collection.</span></span> <span data-ttu-id="91c97-230">**\_Cmd**プロパティを使用、 **Exec**タスクは、コマンドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="91c97-230">The **\_Cmd** property is used by the **Exec** task, which invokes the command.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


<span data-ttu-id="91c97-231">この表記では、結果として、 **Exec**タスクの一意の組み合わせに基づいて、バッチが作成されます、 **DatabaseConnectionString**、 **TargetDatabase**、および**FullPath**メタデータ値、およびタスクは、バッチごとに 1 回実行します。</span><span class="sxs-lookup"><span data-stu-id="91c97-231">As a result of this notation, the **Exec** task will create batches based on unique combinations of the **DatabaseConnectionString**, **TargetDatabase**, and **FullPath** metadata values, and the task will execute once for each batch.</span></span> <span data-ttu-id="91c97-232">これは、例の*タスクのバッチ*です。</span><span class="sxs-lookup"><span data-stu-id="91c97-232">This is an example of *task batching*.</span></span> <span data-ttu-id="91c97-233">ただし、ターゲット レベルのバッチ処理が既に分割されている、項目のコレクションを単一項目のバッチにあるため、 **Exec**タスクは、1 回とターゲットの繰り返しごとに 1 回だけ実行されます。</span><span class="sxs-lookup"><span data-stu-id="91c97-233">However, because the target-level batching has already divided our item collection into single-item batches, the **Exec** task will run once and only once for each iteration of the target.</span></span> <span data-ttu-id="91c97-234">つまり、このタスクは、ソリューション内の各データベース パッケージを 1 回 VSDBCMD ユーティリティを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="91c97-234">In other words, this task invokes the VSDBCMD utility once for each database package in the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="91c97-235">ターゲットとタスクのバッチの詳細については、MSBuild を参照してください。[バッチ処理](https://msdn.microsoft.com/library/ms171473.aspx)、[ターゲットのバッチ内の項目メタデータ](https://msdn.microsoft.com/library/ms228229.aspx)、および[タスクのバッチ内の項目メタデータ](https://msdn.microsoft.com/library/ms171474.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="91c97-235">For more information on target and task batching, see MSBuild [Batching](https://msdn.microsoft.com/library/ms171473.aspx), [Item Metadata in Target Batching](https://msdn.microsoft.com/library/ms228229.aspx), and [Item Metadata in Task Batching](https://msdn.microsoft.com/library/ms171474.aspx).</span></span>


### <a name="the-publishwebpackages-target"></a><span data-ttu-id="91c97-236">PublishWebPackages ターゲット</span><span class="sxs-lookup"><span data-stu-id="91c97-236">The PublishWebPackages Target</span></span>

<span data-ttu-id="91c97-237">この段階では、呼び出した、 **BuildProjects**ターゲットで、サンプル ソリューションの各プロジェクトの web 配置パッケージが生成されます。</span><span class="sxs-lookup"><span data-stu-id="91c97-237">By this point, you've invoked the **BuildProjects** target, which generates a web deployment package for each project in the sample solution.</span></span> <span data-ttu-id="91c97-238">各パッケージに付属する、 *deploy.cmd*を対象となる環境にパッケージを展開に必要な MSDeploy.exe コマンドを含む、ファイルと*SetParameters.xml*を指定するファイル、ターゲット環境のために必要な詳細です。</span><span class="sxs-lookup"><span data-stu-id="91c97-238">Accompanying each package is a *deploy.cmd* file, which contains the MSDeploy.exe commands required to deploy the package to the target environment, and a *SetParameters.xml* file, which specifies the necessary details of the target environment.</span></span> <span data-ttu-id="91c97-239">呼び出したも、 **GatherPackagesForPublishing**ターゲットを含む項目コレクションが生成されますが、 *deploy.cmd*ファイルに関心があります。</span><span class="sxs-lookup"><span data-stu-id="91c97-239">You've also invoked the **GatherPackagesForPublishing** target, which generates an item collection containing the *deploy.cmd* files you're interested in.</span></span> <span data-ttu-id="91c97-240">基本的には、 **PublishWebPackages**ターゲットは、これらの関数を実行します。</span><span class="sxs-lookup"><span data-stu-id="91c97-240">Essentially, the **PublishWebPackages** target performs these functions:</span></span>

- <span data-ttu-id="91c97-241">操作し、 *SetParameters.xml*ターゲット環境の適切な詳細を含めるには、各パッケージのファイルを使用して、 **XmlPoke**タスク。</span><span class="sxs-lookup"><span data-stu-id="91c97-241">It manipulates the *SetParameters.xml* file for each package to include the correct details for the target environment, using the **XmlPoke** task.</span></span>
- <span data-ttu-id="91c97-242">呼び出す、 *deploy.cmd*適切なスイッチを使用して、各パッケージ用のファイルです。</span><span class="sxs-lookup"><span data-stu-id="91c97-242">It invokes the *deploy.cmd* file for each package, using the appropriate switches.</span></span>

<span data-ttu-id="91c97-243">同じように、 **PublishDbPackages** 、ターゲット、 **PublishWebPackages**ターゲットでターゲットのバッチを使用して、ターゲットは web パッケージごとに 1 回実行します。</span><span class="sxs-lookup"><span data-stu-id="91c97-243">Just like the **PublishDbPackages** target, the **PublishWebPackages** target uses target batching to ensure that the target is executed once for each web package.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


<span data-ttu-id="91c97-244">ターゲット内、 **Exec**タスクが実行に使用される、 *deploy.cmd* web パッケージごとにファイル。</span><span class="sxs-lookup"><span data-stu-id="91c97-244">Within the target, the **Exec** task is used to run the *deploy.cmd* file for each web package.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


<span data-ttu-id="91c97-245">Web パッケージの展開を構成する方法については、次を参照してください。[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)です。</span><span class="sxs-lookup"><span data-stu-id="91c97-245">For more information on configuring the deployment of web packages, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>

## <a name="conclusion"></a><span data-ttu-id="91c97-246">まとめ</span><span class="sxs-lookup"><span data-stu-id="91c97-246">Conclusion</span></span>

<span data-ttu-id="91c97-247">このトピックでは、分割されたプロジェクト ファイルを使用して開始から終了連絡先のマネージャーのサンプル ソリューションのビルドおよび配置プロセスを制御する方法のチュートリアルが用意されています。</span><span class="sxs-lookup"><span data-stu-id="91c97-247">This topic provided a walkthrough of how split project files are used to control the build and deployment process from start to finish for the Contact Manager sample solution.</span></span> <span data-ttu-id="91c97-248">このアプローチを使用すると、環境固有のコマンド ファイルを実行するだけで実行する複合型、1 つに、反復可能な手順で、エンタープライズ規模の展開ができます。</span><span class="sxs-lookup"><span data-stu-id="91c97-248">Using this approach lets you run complex, enterprise-scale deployments in a single, repeatable step, simply by running an environment-specific command file.</span></span>

## <a name="further-reading"></a><span data-ttu-id="91c97-249">関連項目</span><span class="sxs-lookup"><span data-stu-id="91c97-249">Further Reading</span></span>

<span data-ttu-id="91c97-250">プロジェクト ファイルと、WPP をより詳細な概要については、次を参照してください。[内の Microsoft Build Engine: MSBuild を使用して、Team Foundation ビルド](http://amzn.com/0735645248)Sayed Ibrahim Hashimi して William Bartholomew、ISBN: 978-0-7356-4524-0 です。</span><span class="sxs-lookup"><span data-stu-id="91c97-250">For a more in-depth introduction to project files and the WPP, see [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://amzn.com/0735645248) by Sayed Ibrahim Hashimi and William Bartholomew, ISBN: 978-0-7356-4524-0.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="91c97-251">[前へ](understanding-the-project-file.md)
> [次へ](building-and-packaging-web-application-projects.md)</span><span class="sxs-lookup"><span data-stu-id="91c97-251">[Previous](understanding-the-project-file.md)
[Next](building-and-packaging-web-application-projects.md)</span></span>
