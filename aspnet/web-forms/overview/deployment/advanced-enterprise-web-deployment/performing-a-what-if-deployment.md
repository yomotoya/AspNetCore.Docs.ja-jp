---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: 場合、何を実行する配置 |Microsoft ドキュメント
author: jrjlee
description: このトピック 'what-if' を実行する方法について説明します (またはシミュレート) V、インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) を使用して展開しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: c1a13f38c8e629bcd615190b00104109e25fb289
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="performing-a-what-if-deployment"></a><span data-ttu-id="dd638-103">"What If"配置の実行</span><span class="sxs-lookup"><span data-stu-id="dd638-103">Performing a "What If" Deployment</span></span>
====================
<span data-ttu-id="dd638-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="dd638-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="dd638-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="dd638-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="dd638-106">このトピックの内容"what-if"を実行する方法について説明します (またはシミュレート) VSDBCMD、インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) を使用して展開します。</span><span class="sxs-lookup"><span data-stu-id="dd638-106">This topic describes how to perform "what if" (or simulated) deployments using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) and VSDBCMD.</span></span> <span data-ttu-id="dd638-107">これにより、アプリケーションを実際に展開する前に、特定のターゲット環境で、デプロイ ロジックの結果が決定できます。</span><span class="sxs-lookup"><span data-stu-id="dd638-107">This lets you determine the effects of your deployment logic on a particular target environment before you actually deploy your application.</span></span>


<span data-ttu-id="dd638-108">このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="dd638-108">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="dd638-109">説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によってビルドおよび配置プロセスが制御されるは、2 つのプロジェクト ファイル&#x2014;いずれか各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用されるビルドの手順を含むです。</span><span class="sxs-lookup"><span data-stu-id="dd638-109">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="dd638-110">ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="dd638-110">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="performing-a-what-if-deployment-for-web-packages"></a><span data-ttu-id="dd638-111">Web パッケージを"What If"の展開を実行します。</span><span class="sxs-lookup"><span data-stu-id="dd638-111">Performing a "What If" Deployment for Web Packages</span></span>

<span data-ttu-id="dd638-112">Web Deploy には、"what-if"で展開を実行するための機能 (または試用版) が含まれています。 モード。</span><span class="sxs-lookup"><span data-stu-id="dd638-112">Web Deploy includes functionality that lets you perform deployments in "what if" (or trial) mode.</span></span> <span data-ttu-id="dd638-113">"What-if"モードでの成果物を展開するときにの展開を実行したが、実際には変化しません、移行先サーバーにログ ファイルを生成する Web 配置します。</span><span class="sxs-lookup"><span data-stu-id="dd638-113">When you deploy artifacts in "what if" mode, Web Deploy generates a log file as if you had performed the deployment, but it doesn't actually change anything on the destination server.</span></span> <span data-ttu-id="dd638-114">ログ ファイルを確認すると、デプロイは移行先サーバーで具体的にはどのような影響を理解することができます。</span><span class="sxs-lookup"><span data-stu-id="dd638-114">Reviewing the log file can help you to understand what impact your deployment will have on the destination server, in particular:</span></span>

- <span data-ttu-id="dd638-115">何が追加取得されます。</span><span class="sxs-lookup"><span data-stu-id="dd638-115">What will get added.</span></span>
- <span data-ttu-id="dd638-116">新機能が更新されます。</span><span class="sxs-lookup"><span data-stu-id="dd638-116">What will get updated.</span></span>
- <span data-ttu-id="dd638-117">取得とは、削除されます。</span><span class="sxs-lookup"><span data-stu-id="dd638-117">What will get deleted.</span></span>

<span data-ttu-id="dd638-118">"What-if"展開実際には変化しません移行先サーバーで、新機能が常にできないため、展開が成功するかどうかを予測します。</span><span class="sxs-lookup"><span data-stu-id="dd638-118">Because a "what if" deployment doesn't actually change anything on the destination server, what it can't always do is predict whether a deployment will succeed.</span></span>

<span data-ttu-id="dd638-119">」の説明に従って[Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)、2 つの方法で Web Deploy を使用して web パッケージを展開する&#x2014;直接、またはを実行して MSDeploy.exe コマンド ライン ユーティリティを使用して、 *. deploy.cmd*ファイルビルド プロセスのことを生成します。</span><span class="sxs-lookup"><span data-stu-id="dd638-119">As described in [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md), you can deploy web packages using Web Deploy in two ways&#x2014;by using the MSDeploy.exe command-line utility directly or by running the *.deploy.cmd* file that the build process generates.</span></span>

<span data-ttu-id="dd638-120">追加することで、"what-if"展開を実行するには MSDeploy.exe を直接使用している場合、 **-whatif**には、コマンド フラグ。</span><span class="sxs-lookup"><span data-stu-id="dd638-120">If you're using MSDeploy.exe directly, you can run a "what if" deployment by adding the **–whatif** flag to your command.</span></span> <span data-ttu-id="dd638-121">たとえば、ContactManager.Mvc.zip パッケージをステージング環境に展開した場合になるかを評価する MSDeploy コマンドのようになりますこの。</span><span class="sxs-lookup"><span data-stu-id="dd638-121">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package to a staging environment, the MSDeploy command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


<span data-ttu-id="dd638-122">削除することができます、"what-if"展開の結果に満足したら、 **-whatif**ライブの展開を実行するフラグ。</span><span class="sxs-lookup"><span data-stu-id="dd638-122">When you're satisfied with the results of your "what if" deployment, you can remove the **–whatif** flag to run a live deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="dd638-123">MSDeploy.exe のコマンド ライン オプションの詳細については、次を参照してください。 [Web Deploy 操作 Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="dd638-123">For more information on command-line options for MSDeploy.exe, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span>


<span data-ttu-id="dd638-124">使用する場合、 *. deploy.cmd*ファイルを含めることによって、"what-if"展開を実行することができます、 **/t**フラグ (試用モード) のフラグの代わりに、 **/y**フラグ ("yes"、または更新モード)コマンド。</span><span class="sxs-lookup"><span data-stu-id="dd638-124">If you're using the *.deploy.cmd* file, you can run a "what if" deployment by including the **/t** flag (trial mode) flag instead of the **/y** flag ("yes," or update mode) in your command.</span></span> <span data-ttu-id="dd638-125">たとえばを実行して ContactManager.Mvc.zip パッケージを展開する場合になるかを評価するため、 *. deploy.cmd*ファイル、このコマンドのようになります。</span><span class="sxs-lookup"><span data-stu-id="dd638-125">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package by running the *.deploy.cmd* file, your command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


<span data-ttu-id="dd638-126">「試用モード」配置の結果に満足したら、置き換えることができます、 **/t**でフラグを設定、 **/y**ライブの展開を実行するフラグ。</span><span class="sxs-lookup"><span data-stu-id="dd638-126">When you're satisfied with the results of your "trial mode" deployment, you can replace the **/t** flag with a **/y** flag to run a live deployment:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="dd638-127">コマンド ライン オプションについて*. deploy.cmd*ファイルを参照してください[する方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="dd638-127">For more information on command-line options for *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="dd638-128">実行する場合、 *. deploy.cmd*ファイル、コマンド プロンプトを任意のフラグを指定することがなく使用可能なフラグの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="dd638-128">If you run the *.deploy.cmd* file without specifying any flags, the command prompt will display a list of available flags.</span></span>


## <a name="performing-a-what-if-deployment-for-databases"></a><span data-ttu-id="dd638-129">データベースの"What If"展開を実行します。</span><span class="sxs-lookup"><span data-stu-id="dd638-129">Performing a "What If" Deployment for Databases</span></span>

<span data-ttu-id="dd638-130">このセクションでは、VSDBCMD ユーティリティを使用して、増分、スキーマ ベースのデータベースの配置を実行していることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="dd638-130">This section assumes that you're using the VSDBCMD utility to perform incremental, schema-based database deployment.</span></span> <span data-ttu-id="dd638-131">このアプローチがで詳しく説明されている[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)です。</span><span class="sxs-lookup"><span data-stu-id="dd638-131">This approach is described in more detail in [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="dd638-132">理解このトピックでは、ここで説明する概念を適用する前にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="dd638-132">We recommend that you familiarize yourself with this topic before you apply the concepts described here.</span></span>

<span data-ttu-id="dd638-133">VSDBCMD を使用する場合**展開**モードでは、行うこともできます、 **/dd** (または**/DeployToDatabase**) VSDBCMD が実際には、データベースを展開またはだけを生成するかどうかを制御するフラグを設定します。配置スクリプト。</span><span class="sxs-lookup"><span data-stu-id="dd638-133">When you use VSDBCMD in **Deploy** mode, you can use the **/dd** (or **/DeployToDatabase**) flag to control whether VSDBCMD actually deploys the database or just generates a deployment script.</span></span> <span data-ttu-id="dd638-134">.Dbschema ファイルを配置する場合、動作を示します。</span><span class="sxs-lookup"><span data-stu-id="dd638-134">If you're deploying a .dbschema file, this is the behavior:</span></span>

- <span data-ttu-id="dd638-135">指定した場合**/dd+**または**/dd**VSDBCMD は配置スクリプトを生成し、データベースを展開します。</span><span class="sxs-lookup"><span data-stu-id="dd638-135">If you specify **/dd+** or **/dd**, VSDBCMD will generate a deployment script and deploy the database.</span></span>
- <span data-ttu-id="dd638-136">指定した場合**/dd-**またはスイッチは省略、VSDBCMD のみ配置スクリプトが生成されます。</span><span class="sxs-lookup"><span data-stu-id="dd638-136">If you specify **/dd-** or omit the switch, VSDBCMD will generate a deployment script only.</span></span>

> [!NOTE]
> <span data-ttu-id="dd638-137">.Dbschema ファイルの動作ではなく、.deploymanifest ファイルを配置する場合、 **/dd**スイッチがはるかに複雑です。</span><span class="sxs-lookup"><span data-stu-id="dd638-137">If you're deploying a .deploymanifest file rather than a .dbschema file, the behavior of the **/dd** switch is a lot more complicated.</span></span> <span data-ttu-id="dd638-138">VSDBCMD 基本的には、値は無視されます、 **/dd**切り替える .deploymanifest ファイルが含まれる場合、 **DeployToDatabase**の値を持つ要素**True**です。</span><span class="sxs-lookup"><span data-stu-id="dd638-138">Essentially, VSDBCMD will ignore the value of the **/dd** switch if the .deploymanifest file includes a **DeployToDatabase** element with a value of **True**.</span></span> <span data-ttu-id="dd638-139">[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)完全には、この動作について説明します。</span><span class="sxs-lookup"><span data-stu-id="dd638-139">[Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md) describes this behavior in full.</span></span>


<span data-ttu-id="dd638-140">たとえば、配置スクリプトを生成するため、 **ContactManager**この実際には、データベース、VSDBCMD コマンドを配置することがなくデータベースのようになります。</span><span class="sxs-lookup"><span data-stu-id="dd638-140">For example, to generate a deployment script for the **ContactManager** database without actually deploying the database, your VSDBCMD command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


<span data-ttu-id="dd638-141">VSDBCMD は、データベースの差分の展開、およびツールを 1 つ存在する場合、指定されたスキーマに、現在のデータベースを更新するために必要なすべての SQL コマンドを格納する、配置スクリプトが動的に生成されるようです。</span><span class="sxs-lookup"><span data-stu-id="dd638-141">VSDBCMD is a differential database deployment tool, and as such the deployment script is dynamically generated to contain all the SQL commands necessary to update the current database, if one exists, to the specified schema.</span></span> <span data-ttu-id="dd638-142">配置スクリプトを確認することは、デプロイに影響を与えるものを決定する便利な方法は、現在のデータベースとが含まれているデータです。</span><span class="sxs-lookup"><span data-stu-id="dd638-142">Reviewing the deployment script is a useful way to determine what impact your deployment will have on the current database and the data it contains.</span></span> <span data-ttu-id="dd638-143">たとえばを確認します。</span><span class="sxs-lookup"><span data-stu-id="dd638-143">For example, you might want to determine:</span></span>

- <span data-ttu-id="dd638-144">既存のテーブルを削除するかどうかと、データが失われるとなるかどうか。</span><span class="sxs-lookup"><span data-stu-id="dd638-144">Whether any existing tables will be removed, and whether that will result in data loss.</span></span>
- <span data-ttu-id="dd638-145">操作の順序はたとえば、データ損失のリスクを実行するかどうか、分割またはテーブルを結合する場合は。</span><span class="sxs-lookup"><span data-stu-id="dd638-145">Whether the order of operations carries a risk of data loss, for example, if you're splitting or merging tables.</span></span>

<span data-ttu-id="dd638-146">配置スクリプトに満足したら場合に、VSDBCMD を繰り返すことができます、 **/dd+**フラグの変更を行います。</span><span class="sxs-lookup"><span data-stu-id="dd638-146">If you're happy with the deployment script, you can repeat the VSDBCMD with a **/dd+** flag to make the changes.</span></span> <span data-ttu-id="dd638-147">また、お客様の要件を満たしているし、データベース サーバーに手動で実行するには、配置スクリプトを編集できます。</span><span class="sxs-lookup"><span data-stu-id="dd638-147">Alternatively, you can edit the deployment script to meet your requirements and then execute it manually on the database server.</span></span>

## <a name="integrating-what-if-functionality-into-custom-project-files"></a><span data-ttu-id="dd638-148">カスタム プロジェクト ファイルへの"What If"機能の統合</span><span class="sxs-lookup"><span data-stu-id="dd638-148">Integrating "What If" Functionality into Custom Project Files</span></span>

<span data-ttu-id="dd638-149">複雑な展開シナリオでおくを」の説明に従って、カスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して、ビルドおよび配置ロジックをカプセル化する[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)です。</span><span class="sxs-lookup"><span data-stu-id="dd638-149">In more complex deployment scenarios, you'll want to use a custom Microsoft Build Engine (MSBuild) project file to encapsulate your build and deployment logic, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="dd638-150">たとえば、[連絡先のマネージャー](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューションを*Publish.proj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="dd638-150">For example, in the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, the *Publish.proj* file:</span></span>

- <span data-ttu-id="dd638-151">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="dd638-151">Builds the solution.</span></span>
- <span data-ttu-id="dd638-152">パッケージ化し、ContactManager.Mvc アプリケーションを配置するには、Web Deploy を使用します。</span><span class="sxs-lookup"><span data-stu-id="dd638-152">Uses Web Deploy to package and deploy the ContactManager.Mvc application.</span></span>
- <span data-ttu-id="dd638-153">パッケージ化し、ContactManager.Service アプリケーションを配置するには、Web Deploy を使用します。</span><span class="sxs-lookup"><span data-stu-id="dd638-153">Uses Web Deploy to package and deploy the ContactManager.Service application.</span></span>
- <span data-ttu-id="dd638-154">展開、 **ContactManager**データベース。</span><span class="sxs-lookup"><span data-stu-id="dd638-154">Deploys the **ContactManager** database.</span></span>

<span data-ttu-id="dd638-155">この方法で 1 つの手順を複数の web のパッケージやデータベースの配置を統合するときにすることも、オプションの"what-if"モードで配置全体を実行します。</span><span class="sxs-lookup"><span data-stu-id="dd638-155">When you integrate the deployment of multiple web packages and/or databases into a single-step process in this way, you may also want the option of performing the entire deployment in a "what if" mode.</span></span>

<span data-ttu-id="dd638-156">*Publish.proj*ファイルでは、この操作方法を示します。</span><span class="sxs-lookup"><span data-stu-id="dd638-156">The *Publish.proj* file demonstrates how you can do this.</span></span> <span data-ttu-id="dd638-157">最初に、"what-if"の値を格納するプロパティを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd638-157">First, you need to create a property to store the "what if" value:</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


<span data-ttu-id="dd638-158">この場合、という名前のプロパティを作成した**WhatIf**で、既定値は**false**です。</span><span class="sxs-lookup"><span data-stu-id="dd638-158">In this case, you've created a property named **WhatIf** with a default value of **false**.</span></span> <span data-ttu-id="dd638-159">ユーザーは、プロパティを設定してこの値を上書きできます**true** 、コマンド ライン パラメーターでも後ほどです。</span><span class="sxs-lookup"><span data-stu-id="dd638-159">Users can override this value by setting the property to **true** in a command-line parameter, as you'll see shortly.</span></span>

<span data-ttu-id="dd638-160">次のステージでは、Web Deploy をパラメーター化し、VSDBCMD フラグを反映するためのコマンド、 **WhatIf**プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="dd638-160">The next stage is to parameterize any Web Deploy and VSDBCMD commands so that the flags reflect the **WhatIf** property value.</span></span> <span data-ttu-id="dd638-161">たとえば、[次へ] のターゲット (から取得した、 *Publish.proj*ファイルおよび簡素化されます) を実行、 *. deploy.cmd* web パッケージを配置するファイル。</span><span class="sxs-lookup"><span data-stu-id="dd638-161">For example, the next target (taken from the *Publish.proj* file and simplified) runs the *.deploy.cmd* file to deploy a web package.</span></span> <span data-ttu-id="dd638-162">既定では、コマンドは、含まれています、 **/Y** ("yes"または更新モード) スイッチ。</span><span class="sxs-lookup"><span data-stu-id="dd638-162">By default, the command includes a **/Y** switch ("yes," or update mode).</span></span> <span data-ttu-id="dd638-163">場合**WhatIf**に設定されている**true**で置き換えられます、 **/T** (評価版、または"what-if"モード) スイッチ。</span><span class="sxs-lookup"><span data-stu-id="dd638-163">If **WhatIf** is set to **true**, this is replaced by a **/T** switch (trial, or "what if" mode).</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


<span data-ttu-id="dd638-164">同様に、次のターゲットは、データベースを配置するのに VSDBCMD ユーティリティを使用します。</span><span class="sxs-lookup"><span data-stu-id="dd638-164">Similarly, the next target uses the VSDBCMD utility to deploy a database.</span></span> <span data-ttu-id="dd638-165">既定では、 **/dd**スイッチは含まれません。</span><span class="sxs-lookup"><span data-stu-id="dd638-165">By default, a **/dd** switch is not included.</span></span> <span data-ttu-id="dd638-166">つまり、VSDBCMD では、配置スクリプトが生成されますが、データベースに展開できなくなります&#x2014;言い換えると、"what-if"シナリオです。</span><span class="sxs-lookup"><span data-stu-id="dd638-166">This means that VSDBCMD will generate a deployment script but will not deploy the database&#x2014;in other words, a "what if" scenario.</span></span> <span data-ttu-id="dd638-167">場合、 **WhatIf**プロパティに設定されていない**true**、 **/dd**スイッチが追加され、VSDBCMD は、データベースを展開します。</span><span class="sxs-lookup"><span data-stu-id="dd638-167">If the **WhatIf** property is not set to **true**, a **/dd** switch is added and VSDBCMD will deploy the database.</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


<span data-ttu-id="dd638-168">同じアプローチを使用するには、プロジェクト ファイルですべての関連するコマンドをパラメーター化します。</span><span class="sxs-lookup"><span data-stu-id="dd638-168">You can use the same approach to parameterize all the relevant commands in your project file.</span></span> <span data-ttu-id="dd638-169">"What-if"展開を実行する場合は、単に提供できます、 **WhatIf**コマンドラインからプロパティ値。</span><span class="sxs-lookup"><span data-stu-id="dd638-169">When you want to run a "what if" deployment, you can then simply provide a **WhatIf** property value from the command line:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


<span data-ttu-id="dd638-170">この方法で、プロジェクトのすべてのコンポーネントをシングル ステップで"what-if"展開を実行できます。</span><span class="sxs-lookup"><span data-stu-id="dd638-170">In this way, you can run a "what if" deployment for all your project components in a single step.</span></span>

## <a name="conclusion"></a><span data-ttu-id="dd638-171">まとめ</span><span class="sxs-lookup"><span data-stu-id="dd638-171">Conclusion</span></span>

<span data-ttu-id="dd638-172">このトピックでは、Web デプロイ、VSDBCMD、および MSBuild を使用する展開"what-if"を実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="dd638-172">This topic described how to run "what if" deployments using Web Deploy, VSDBCMD, and MSBuild.</span></span> <span data-ttu-id="dd638-173">"What-if"展開では、変更する前に実際には、送信先の環境には、提案された展開の影響を評価することができます。</span><span class="sxs-lookup"><span data-stu-id="dd638-173">A "what if" deployment lets you evaluate the impact of a proposed deployment before you actually make any changes to the destination environment.</span></span>

## <a name="further-reading"></a><span data-ttu-id="dd638-174">関連項目</span><span class="sxs-lookup"><span data-stu-id="dd638-174">Further Reading</span></span>

<span data-ttu-id="dd638-175">Web Deploy のコマンドライン構文の詳細については、次を参照してください。 [Web Deploy 操作 Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="dd638-175">For more information on Web Deploy command-line syntax, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span> <span data-ttu-id="dd638-176">コマンド ライン オプションを使用するときのガイダンスについては、 *. deploy.cmd*ファイルを参照してください[する方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="dd638-176">For guidance on command-line options when you use the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="dd638-177">VSDBCMD コマンドライン構文については、次を参照してください。 [VSDBCMD のコマンド ライン リファレンスです。EXE (配置とスキーマのインポート)](https://msdn.microsoft.com/library/dd193283.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="dd638-177">For guidance on VSDBCMD command-line syntax, see [Command-Line Reference for VSDBCMD.EXE (Deployment and Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dd638-178">[前へ](advanced-enterprise-web-deployment.md)
> [次へ](customizing-database-deployments-for-multiple-environments.md)</span><span class="sxs-lookup"><span data-stu-id="dd638-178">[Previous](advanced-enterprise-web-deployment.md)
[Next](customizing-database-deployments-for-multiple-environments.md)</span></span>
