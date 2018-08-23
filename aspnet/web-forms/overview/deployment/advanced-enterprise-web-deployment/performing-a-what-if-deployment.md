---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: 場合、何を実行する展開 |Microsoft Docs
author: jrjlee
description: このトピックの「' what-if ' を実行する方法について説明します (またはシミュレートされた) (Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツールと V を使用してデプロイいます.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 054b15e1e58164ec7e85c77c39fb47bcb47879b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835150"
---
<a name="performing-a-what-if-deployment"></a><span data-ttu-id="a1c0c-103">"What If"展開を実行します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-103">Performing a "What If" Deployment</span></span>
====================
<span data-ttu-id="a1c0c-104">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="a1c0c-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="a1c0c-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="a1c0c-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="a1c0c-106">このトピックで"what-if"を実行する方法について説明します (またはシミュレートされた) (Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツール、VSDBCMD を使用してデプロイします。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-106">This topic describes how to perform "what if" (or simulated) deployments using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) and VSDBCMD.</span></span> <span data-ttu-id="a1c0c-107">これにより、アプリケーションを実際に展開する前に、デプロイ ロジックの特定のターゲット環境に与える影響を判断できます。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-107">This lets you determine the effects of your deployment logic on a particular target environment before you actually deploy your application.</span></span>


<span data-ttu-id="a1c0c-108">このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-108">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="a1c0c-109">これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルドおよび配置プロセスでは、2 つのプロジェクト ファイル&#x2014;いずれかすべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用されるビルドの手順を含むです。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-109">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="a1c0c-110">ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-110">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="performing-a-what-if-deployment-for-web-packages"></a><span data-ttu-id="a1c0c-111">Web パッケージを"What If"展開を実行します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-111">Performing a "What If" Deployment for Web Packages</span></span>

<span data-ttu-id="a1c0c-112">Web Deploy には、"what-if"でのデプロイを実行する機能 (または試用版) が含まれます。 モード。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-112">Web Deploy includes functionality that lets you perform deployments in "what if" (or trial) mode.</span></span> <span data-ttu-id="a1c0c-113">展開が実行される必要があるが、移行先サーバーでは何も変更が実際には、Web Deploy"what-if"モードでの成果物を配置するときにログ ファイルを生成します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-113">When you deploy artifacts in "what if" mode, Web Deploy generates a log file as if you had performed the deployment, but it doesn't actually change anything on the destination server.</span></span> <span data-ttu-id="a1c0c-114">ログ ファイルを確認することは、デプロイは具体的には、移行先サーバーでがどのような影響を理解するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-114">Reviewing the log file can help you to understand what impact your deployment will have on the destination server, in particular:</span></span>

- <span data-ttu-id="a1c0c-115">どのような追加されます。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-115">What will get added.</span></span>
- <span data-ttu-id="a1c0c-116">何が更新されます。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-116">What will get updated.</span></span>
- <span data-ttu-id="a1c0c-117">取得とは、削除します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-117">What will get deleted.</span></span>

<span data-ttu-id="a1c0c-118">"What if"展開で実際に変更できません常に何が、移行先サーバーでは何もしないため、展開が成功するかどうかを予測します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-118">Because a "what if" deployment doesn't actually change anything on the destination server, what it can't always do is predict whether a deployment will succeed.</span></span>

<span data-ttu-id="a1c0c-119">」の説明に従って[Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)、2 つの方法で Web Deploy を使用して web パッケージを展開する&#x2014;直接、またはを実行して MSDeploy.exe コマンド ライン ユーティリティを使用して、 *. deploy.cmd*ファイルビルド プロセスを生成します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-119">As described in [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md), you can deploy web packages using Web Deploy in two ways&#x2014;by using the MSDeploy.exe command-line utility directly or by running the *.deploy.cmd* file that the build process generates.</span></span>

<span data-ttu-id="a1c0c-120">MSDeploy.exe を直接使用している場合は、追加することで、"what if"展開を行うことができます、 **– whatif**コマンドにフラグ。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-120">If you're using MSDeploy.exe directly, you can run a "what if" deployment by adding the **–whatif** flag to your command.</span></span> <span data-ttu-id="a1c0c-121">たとえば、ContactManager.Mvc.zip パッケージをステージング環境にデプロイした場合、何が起こるかを評価するため、MSDeploy コマンドのようになりますこの。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-121">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package to a staging environment, the MSDeploy command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


<span data-ttu-id="a1c0c-122">削除することができます、"what if"展開の結果に満足したら、 **– whatif**ライブの展開を実行するフラグ。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-122">When you're satisfied with the results of your "what if" deployment, you can remove the **–whatif** flag to run a live deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="a1c0c-123">MSDeploy.exe のコマンド ライン オプションの詳細については、次を参照してください。 [Web 配置操作の設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-123">For more information on command-line options for MSDeploy.exe, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span>


<span data-ttu-id="a1c0c-124">使用している場合、 *. deploy.cmd*ファイルを含めることで、"what if"展開を実行することができます、 **/t**フラグ (試用版モード) のフラグの代わりに、 **/y**のフラグ ("yes"、または更新モード)コマンド。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-124">If you're using the *.deploy.cmd* file, you can run a "what if" deployment by including the **/t** flag (trial mode) flag instead of the **/y** flag ("yes," or update mode) in your command.</span></span> <span data-ttu-id="a1c0c-125">たとえばを実行して ContactManager.Mvc.zip パッケージを展開する場合に何が起こるかを評価するため、 *. deploy.cmd*ファイルでは、このコマンドのようになります。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-125">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package by running the *.deploy.cmd* file, your command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


<span data-ttu-id="a1c0c-126">「試用版モード」配置の結果に満足したら、置き換えることができます、 **/t**フラグを **/y**ライブの展開を実行するフラグ。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-126">When you're satisfied with the results of your "trial mode" deployment, you can replace the **/t** flag with a **/y** flag to run a live deployment:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="a1c0c-127">コマンド ライン オプションの詳細については *. deploy.cmd*ファイルを参照してください[方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-127">For more information on command-line options for *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="a1c0c-128">実行する場合、 *. deploy.cmd*ファイル フラグのいずれかを指定せず、コマンド プロンプトが使用可能なフラグの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-128">If you run the *.deploy.cmd* file without specifying any flags, the command prompt will display a list of available flags.</span></span>


## <a name="performing-a-what-if-deployment-for-databases"></a><span data-ttu-id="a1c0c-129">データベースの"What If"展開を実行します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-129">Performing a "What If" Deployment for Databases</span></span>

<span data-ttu-id="a1c0c-130">このセクションでは、VSDBCMD ユーティリティを使用して、増分"、"スキーマ ベースのデータベースの配置を実行していることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-130">This section assumes that you're using the VSDBCMD utility to perform incremental, schema-based database deployment.</span></span> <span data-ttu-id="a1c0c-131">このアプローチがで詳しく説明されている[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-131">This approach is described in more detail in [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="a1c0c-132">理解するおくとこのトピックにここで説明した概念を適用する前にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-132">We recommend that you familiarize yourself with this topic before you apply the concepts described here.</span></span>

<span data-ttu-id="a1c0c-133">VSDBCMD を使用すると**デプロイ**モードを使用できます、 **/dd** (または **/DeployToDatabase**) VSDBCMD が実際にデータベースを配置またはだけを生成するかどうかを制御するフラグをデプロイ スクリプト。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-133">When you use VSDBCMD in **Deploy** mode, you can use the **/dd** (or **/DeployToDatabase**) flag to control whether VSDBCMD actually deploys the database or just generates a deployment script.</span></span> <span data-ttu-id="a1c0c-134">.Dbschema ファイルを配置する場合、これは動作します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-134">If you're deploying a .dbschema file, this is the behavior:</span></span>

- <span data-ttu-id="a1c0c-135">指定した場合 **/dd+** または **/dd**VSDBCMD は配置スクリプトを生成し、データベースを展開します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-135">If you specify **/dd+** or **/dd**, VSDBCMD will generate a deployment script and deploy the database.</span></span>
- <span data-ttu-id="a1c0c-136">指定した場合 **/dd-** またはスイッチは省略、VSDBCMD 配置スクリプトのみが生成されます。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-136">If you specify **/dd-** or omit the switch, VSDBCMD will generate a deployment script only.</span></span>

> [!NOTE]
> <span data-ttu-id="a1c0c-137">.Dbschema ファイルの動作ではなく、.deploymanifest ファイルをデプロイする場合、 **/dd**スイッチはもっと複雑になります。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-137">If you're deploying a .deploymanifest file rather than a .dbschema file, the behavior of the **/dd** switch is a lot more complicated.</span></span> <span data-ttu-id="a1c0c-138">VSDBCMD は基本的には、値は無視、 **/dd** .deploymanifest ファイルが含まれている場合に切り替え、 **DeployToDatabase**の値を持つ要素**True**します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-138">Essentially, VSDBCMD will ignore the value of the **/dd** switch if the .deploymanifest file includes a **DeployToDatabase** element with a value of **True**.</span></span> <span data-ttu-id="a1c0c-139">[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)を完全には、この動作について説明します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-139">[Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md) describes this behavior in full.</span></span>


<span data-ttu-id="a1c0c-140">たとえば、配置スクリプトを生成、 **ContactManager**この実際には、VSDBCMD コマンドは、データベースを展開することがなくデータベースのようになります。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-140">For example, to generate a deployment script for the **ContactManager** database without actually deploying the database, your VSDBCMD command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


<span data-ttu-id="a1c0c-141">VSDBCMD は、データベースの差分の展開ツールと、指定したスキーマに存在する場合は、現在のデータベースを更新するために必要なすべての SQL コマンドを格納する配置スクリプトが動的に生成されるようします。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-141">VSDBCMD is a differential database deployment tool, and as such the deployment script is dynamically generated to contain all the SQL commands necessary to update the current database, if one exists, to the specified schema.</span></span> <span data-ttu-id="a1c0c-142">配置スクリプトを確認するは、配置に影響を与えるものを決定する便利な方法が、現在のデータベースとデータが含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-142">Reviewing the deployment script is a useful way to determine what impact your deployment will have on the current database and the data it contains.</span></span> <span data-ttu-id="a1c0c-143">たとえばを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-143">For example, you might want to determine:</span></span>

- <span data-ttu-id="a1c0c-144">任意の既存のテーブルを削除するかどうかと、データが失われるしまうかどうか。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-144">Whether any existing tables will be removed, and whether that will result in data loss.</span></span>
- <span data-ttu-id="a1c0c-145">操作の順序はたとえば、データ損失のリスクを実行するかどうか、分割またはテーブルをマージしている場合は。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-145">Whether the order of operations carries a risk of data loss, for example, if you're splitting or merging tables.</span></span>

<span data-ttu-id="a1c0c-146">配置スクリプトに満足したら場合に、VSDBCMD を繰り返すことができます、 **/dd+** を変更するフラグ。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-146">If you're happy with the deployment script, you can repeat the VSDBCMD with a **/dd+** flag to make the changes.</span></span> <span data-ttu-id="a1c0c-147">または、要件を満たすし、データベース サーバーに手動で実行するには、配置スクリプトを編集できます。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-147">Alternatively, you can edit the deployment script to meet your requirements and then execute it manually on the database server.</span></span>

## <a name="integrating-what-if-functionality-into-custom-project-files"></a><span data-ttu-id="a1c0c-148">カスタムのプロジェクト ファイルに"What If"機能を統合します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-148">Integrating "What If" Functionality into Custom Project Files</span></span>

<span data-ttu-id="a1c0c-149">複雑な展開シナリオでは、」の説明に従って、カスタム Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して、ビルドと配置ロジックをカプセル化する必要あります[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-149">In more complex deployment scenarios, you'll want to use a custom Microsoft Build Engine (MSBuild) project file to encapsulate your build and deployment logic, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="a1c0c-150">たとえば、 [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューションを*Publish.proj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-150">For example, in the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, the *Publish.proj* file:</span></span>

- <span data-ttu-id="a1c0c-151">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-151">Builds the solution.</span></span>
- <span data-ttu-id="a1c0c-152">パッケージ化し、ContactManager.Mvc アプリケーションをデプロイするには、Web Deploy を使用します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-152">Uses Web Deploy to package and deploy the ContactManager.Mvc application.</span></span>
- <span data-ttu-id="a1c0c-153">パッケージ化し、ContactManager.Service アプリケーションをデプロイするには、Web Deploy を使用します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-153">Uses Web Deploy to package and deploy the ContactManager.Service application.</span></span>
- <span data-ttu-id="a1c0c-154">配置、 **ContactManager**データベース。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-154">Deploys the **ContactManager** database.</span></span>

<span data-ttu-id="a1c0c-155">この方法でのシングル ステップのプロセスに複数の web のパッケージやデータベースの配置を統合するときは、"what-if"モードで展開全体を実行するオプションを必要もあります。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-155">When you integrate the deployment of multiple web packages and/or databases into a single-step process in this way, you may also want the option of performing the entire deployment in a "what if" mode.</span></span>

<span data-ttu-id="a1c0c-156">*Publish.proj*ファイルでは、これを実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-156">The *Publish.proj* file demonstrates how you can do this.</span></span> <span data-ttu-id="a1c0c-157">最初に、"what-if"の値を格納するプロパティを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-157">First, you need to create a property to store the "what if" value:</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


<span data-ttu-id="a1c0c-158">この場合、という名前のプロパティを作成した**WhatIf**の既定値を持つ**false**します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-158">In this case, you've created a property named **WhatIf** with a default value of **false**.</span></span> <span data-ttu-id="a1c0c-159">ユーザーは、プロパティを設定してこの値を上書きできます**true** 、コマンド ライン パラメーターでも後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-159">Users can override this value by setting the property to **true** in a command-line parameter, as you'll see shortly.</span></span>

<span data-ttu-id="a1c0c-160">次に、任意の Web Deploy をパラメーター化して、VSDBCMD フラグを反映するためのコマンド、 **WhatIf**プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-160">The next stage is to parameterize any Web Deploy and VSDBCMD commands so that the flags reflect the **WhatIf** property value.</span></span> <span data-ttu-id="a1c0c-161">たとえば、次のターゲット (から取得した、 *Publish.proj*ファイルし、簡略化) を実行、 *. deploy.cmd* web パッケージを配置するファイル。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-161">For example, the next target (taken from the *Publish.proj* file and simplified) runs the *.deploy.cmd* file to deploy a web package.</span></span> <span data-ttu-id="a1c0c-162">既定で、コマンドが含まれて、 **/Y**スイッチ ("yes"、または更新モード)。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-162">By default, the command includes a **/Y** switch ("yes," or update mode).</span></span> <span data-ttu-id="a1c0c-163">場合**WhatIf**に設定されている**true**で置き換えられます、 **/T**スイッチ (試用版、または"what-if"モード)。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-163">If **WhatIf** is set to **true**, this is replaced by a **/T** switch (trial, or "what if" mode).</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


<span data-ttu-id="a1c0c-164">同様に、次のターゲットは、VSDBCMD ユーティリティを使用して、データベースを配置します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-164">Similarly, the next target uses the VSDBCMD utility to deploy a database.</span></span> <span data-ttu-id="a1c0c-165">既定で、 **/dd**スイッチは含まれません。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-165">By default, a **/dd** switch is not included.</span></span> <span data-ttu-id="a1c0c-166">つまり、VSDBCMD は配置スクリプトが生成されますが、データベースに展開できなくなります&#x2014;つまり、"what-if"シナリオ。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-166">This means that VSDBCMD will generate a deployment script but will not deploy the database&#x2014;in other words, a "what if" scenario.</span></span> <span data-ttu-id="a1c0c-167">場合、 **WhatIf**プロパティに設定されていない**true**、 **/dd**スイッチが追加され、VSDBCMD はデータベースを配置します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-167">If the **WhatIf** property is not set to **true**, a **/dd** switch is added and VSDBCMD will deploy the database.</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


<span data-ttu-id="a1c0c-168">同じアプローチを使用すると、プロジェクト ファイル内のすべての関連するコマンドをパラメーター化します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-168">You can use the same approach to parameterize all the relevant commands in your project file.</span></span> <span data-ttu-id="a1c0c-169">"What if"展開を実行するときに行うことができますし、単に、 **WhatIf**コマンドラインからプロパティ値。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-169">When you want to run a "what if" deployment, you can then simply provide a **WhatIf** property value from the command line:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


<span data-ttu-id="a1c0c-170">これにより、1 つのステップですべてのプロジェクト コンポーネント、"what if"展開を実行できます。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-170">In this way, you can run a "what if" deployment for all your project components in a single step.</span></span>

## <a name="conclusion"></a><span data-ttu-id="a1c0c-171">まとめ</span><span class="sxs-lookup"><span data-stu-id="a1c0c-171">Conclusion</span></span>

<span data-ttu-id="a1c0c-172">このトピックでは、Web Deploy、VSDBCMD、MSBuild を使用してデプロイ"what-if"を実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-172">This topic described how to run "what if" deployments using Web Deploy, VSDBCMD, and MSBuild.</span></span> <span data-ttu-id="a1c0c-173">"What if"展開では、移行先の環境に実際には、変更を加える前に、提案された展開の影響を評価することができます。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-173">A "what if" deployment lets you evaluate the impact of a proposed deployment before you actually make any changes to the destination environment.</span></span>

## <a name="further-reading"></a><span data-ttu-id="a1c0c-174">関連項目</span><span class="sxs-lookup"><span data-stu-id="a1c0c-174">Further Reading</span></span>

<span data-ttu-id="a1c0c-175">Web デプロイ コマンドライン構文の詳細については、次を参照してください。 [Web 配置操作の設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-175">For more information on Web Deploy command-line syntax, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span> <span data-ttu-id="a1c0c-176">コマンド ライン オプションを使用する場合のガイダンスについては、 *. deploy.cmd*ファイルを参照してください[方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-176">For guidance on command-line options when you use the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="a1c0c-177">VSDBCMD コマンドライン構文については、次を参照してください。 [VSDBCMD のコマンド ライン リファレンス。EXE (展開およびスキーマのインポート)](https://msdn.microsoft.com/library/dd193283.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="a1c0c-177">For guidance on VSDBCMD command-line syntax, see [Command-Line Reference for VSDBCMD.EXE (Deployment and Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a1c0c-178">[前へ](advanced-enterprise-web-deployment.md)
> [次へ](customizing-database-deployments-for-multiple-environments.md)</span><span class="sxs-lookup"><span data-stu-id="a1c0c-178">[Previous](advanced-enterprise-web-deployment.md)
[Next](customizing-database-deployments-for-multiple-environments.md)</span></span>
