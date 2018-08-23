---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: データベース ロールのメンバーシップをテスト環境に展開する |Microsoft Docs
author: jrjlee
description: このトピックでは、テスト環境にデプロイするソリューションの一部として、データベース ロールにユーザー アカウントを追加する方法について説明します。 含むソリューションを展開するときにしています.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 07442b7a016ce2a32b1c9e7f44010517e40d7189
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839032"
---
<a name="deploying-database-role-memberships-to-test-environments"></a><span data-ttu-id="5a168-104">データベース ロールのメンバーシップをテスト環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="5a168-104">Deploying Database Role Memberships to Test Environments</span></span>
====================
<span data-ttu-id="5a168-105">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="5a168-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="5a168-106">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="5a168-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="5a168-107">このトピックでは、テスト環境にデプロイするソリューションの一部として、データベース ロールにユーザー アカウントを追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="5a168-107">This topic describes how to add user accounts to database roles as part of a solution deployment to a test environment.</span></span>
> 
> <span data-ttu-id="5a168-108">ステージング環境または運用環境にデータベース プロジェクトを含むソリューションを展開するときに、データベース ロールにユーザー アカウントの追加を自動化する開発者通常必要ありません。</span><span class="sxs-lookup"><span data-stu-id="5a168-108">When you deploy a solution containing a database project to a staging or production environment, you typically don't want the developer to automate the addition of user accounts to database roles.</span></span> <span data-ttu-id="5a168-109">ほとんどの場合、開発者がどのデータベース ロールに追加する必要があるユーザー アカウントを知ることができず、これらの要件は、いつでも変更できます。</span><span class="sxs-lookup"><span data-stu-id="5a168-109">In most cases, the developer won't know which user accounts need to be added to which database roles, and these requirements could change at any time.</span></span> <span data-ttu-id="5a168-110">ただし、開発中にデータベース プロジェクトを含むソリューションをデプロイまたは環境をテストするときに、状況は別ではなく通常。</span><span class="sxs-lookup"><span data-stu-id="5a168-110">However, when you deploy a solution containing a database project to a development or test environment, the situation is usually rather different:</span></span>
> 
> - <span data-ttu-id="5a168-111">開発者通常再ソリューションをデプロイを定期的に、1 日に数回多くの場合。</span><span class="sxs-lookup"><span data-stu-id="5a168-111">The developer typically re-deploys the solution on a regular basis, often several times a day.</span></span>
> - <span data-ttu-id="5a168-112">通常、データベースがデータベース ユーザーの作成し、すべてのデプロイ後のロールに追加する必要があります、つまりすべてのデプロイで再作成されます。</span><span class="sxs-lookup"><span data-stu-id="5a168-112">The database is typically re-created on every deployment, which means that database users must be created and added to roles after every deployment.</span></span>
> - <span data-ttu-id="5a168-113">通常、開発者には、ターゲット開発またはテスト環境を完全に制御があります。</span><span class="sxs-lookup"><span data-stu-id="5a168-113">The developer typically has full control over the target development or test environment.</span></span>
> 
> <span data-ttu-id="5a168-114">このシナリオで自動的にデータベース ユーザーを作成し、展開プロセスの一部としてデータベース ロールのメンバーシップを割り当てると便利です。</span><span class="sxs-lookup"><span data-stu-id="5a168-114">In this scenario, it's often beneficial to automatically create database users and assign database role memberships as part of the deployment process.</span></span>
> 
> <span data-ttu-id="5a168-115">重要な要素は、この操作する必要がある条件付きの場合、ターゲット環境に基づくです。</span><span class="sxs-lookup"><span data-stu-id="5a168-115">The key factor is that this operation needs to be conditional based on the target environment.</span></span> <span data-ttu-id="5a168-116">操作をスキップする場合は、ステージングまたは運用環境にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="5a168-116">If you're deploying to a staging or a production environment, you want to skip the operation.</span></span> <span data-ttu-id="5a168-117">操作を行うロールのメンバーシップを展開する場合は、開発者にデプロイする場合、またはテスト環境は、します。</span><span class="sxs-lookup"><span data-stu-id="5a168-117">If you're deploying to a developer or test environment, you want to deploy role memberships without further intervention.</span></span> <span data-ttu-id="5a168-118">このトピックでは、この課題に対処する 1 つの方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="5a168-118">This topic describes one approach you can use to address this challenge.</span></span>


<span data-ttu-id="5a168-119">このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="5a168-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="5a168-120">これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。</span><span class="sxs-lookup"><span data-stu-id="5a168-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="5a168-121">ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="5a168-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="5a168-122">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="5a168-122">Task Overview</span></span>

<span data-ttu-id="5a168-123">このトピックを前提とします。</span><span class="sxs-lookup"><span data-stu-id="5a168-123">This topic assumes that:</span></span>

- <span data-ttu-id="5a168-124">」の説明に従って、ソリューションのデプロイメントにプロジェクト ファイルの分割方法を使用する[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)します。</span><span class="sxs-lookup"><span data-stu-id="5a168-124">You use the split project file approach to solution deployment, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span>
- <span data-ttu-id="5a168-125">」の説明に従って、データベース プロジェクトを配置するプロジェクト ファイルから VSDBCMD を呼び出す[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。</span><span class="sxs-lookup"><span data-stu-id="5a168-125">You call VSDBCMD from the project file to deploy your database project, as described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

<span data-ttu-id="5a168-126">データベース ユーザーを作成し、テスト環境にデータベース プロジェクトを展開するときに、ロールのメンバーシップを割り当てる、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a168-126">To create database users and assign role memberships when you deploy a database project to a test environment, you'll need to:</span></span>

- <span data-ttu-id="5a168-127">必要なデータベースの変更を行うトランザクションの構造化照会言語 (TRANSACT-SQL) スクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="5a168-127">Create a Transact Structured Query Language (Transact-SQL) script that makes the necessary database changes.</span></span>
- <span data-ttu-id="5a168-128">Sqlcmd.exe ユーティリティを使用して SQL スクリプトを実行する Microsoft Build Engine (MSBuild) ターゲットを作成します。</span><span class="sxs-lookup"><span data-stu-id="5a168-128">Create a Microsoft Build Engine (MSBuild) target that uses the sqlcmd.exe utility to run the SQL script.</span></span>
- <span data-ttu-id="5a168-129">テスト環境にソリューションを配置するときに、ターゲットを呼び出すように、プロジェクト ファイルを構成します。</span><span class="sxs-lookup"><span data-stu-id="5a168-129">Configure your project files to invoke the target when you're deploying your solution to a test environment.</span></span>

<span data-ttu-id="5a168-130">このトピックでは、これらの手順を実行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="5a168-130">This topic will show you how to perform each of these procedures.</span></span>

## <a name="scripting-the-database-role-memberships"></a><span data-ttu-id="5a168-131">データベース ロールのメンバーシップをスクリプト作成</span><span class="sxs-lookup"><span data-stu-id="5a168-131">Scripting the Database Role Memberships</span></span>

<span data-ttu-id="5a168-132">Transact SQL スクリプトを作成するには、多くのさまざまな方法とでは、任意の場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="5a168-132">You can create a Transact-SQL script in a lot of different ways, and in any location you choose.</span></span> <span data-ttu-id="5a168-133">最も簡単な方法では、Visual Studio 2010 で、ソリューション内でスクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="5a168-133">The easiest approach is to create the script within your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="5a168-134">**SQL スクリプトを作成するには**</span><span class="sxs-lookup"><span data-stu-id="5a168-134">**To create a SQL script**</span></span>

1. <span data-ttu-id="5a168-135">**ソリューション エクスプ ローラー**ウィンドウで、データベース プロジェクト ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="5a168-135">In the **Solution Explorer** window, expand your database project node.</span></span>
2. <span data-ttu-id="5a168-136">右クリックし、**スクリプト**フォルダーをポイントして**追加**、 をクリックし、**新しいフォルダー**します。</span><span class="sxs-lookup"><span data-stu-id="5a168-136">Right-click the **Scripts** folder, point to **Add**, and then click **New Folder**.</span></span>
3. <span data-ttu-id="5a168-137">型**テスト**としてフォルダーの名前と Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="5a168-137">Type **Test** as the folder name, and then press Enter.</span></span>
4. <span data-ttu-id="5a168-138">右クリックし、**テスト**フォルダーをポイントして**追加**、 をクリックし、**スクリプト**します。</span><span class="sxs-lookup"><span data-stu-id="5a168-138">Right-click the **Test** folder, point to **Add**, and then click **Script**.</span></span>
5. <span data-ttu-id="5a168-139">**新しい項目の追加** ダイアログ ボックスで、スクリプトに、わかりやすい名前を付け (たとえば、 **AddRoleMemberships.sql**)、順にクリックします**追加**。</span><span class="sxs-lookup"><span data-stu-id="5a168-139">In the **Add New Item** dialog box, give your script a meaningful name (for example, **AddRoleMemberships.sql**), and then click **Add**.</span></span>

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. <span data-ttu-id="5a168-140">*AddRoleMemberships.sql*ファイルに、TRANSACT-SQL ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="5a168-140">In the *AddRoleMemberships.sql* file, add Transact-SQL statements that:</span></span>

    1. <span data-ttu-id="5a168-141">SQL Server ログイン、データベースにアクセスするためのデータベース ユーザーを作成します。</span><span class="sxs-lookup"><span data-stu-id="5a168-141">Create a database user for the SQL Server login that will access your database.</span></span>
    2. <span data-ttu-id="5a168-142">任意の必要なデータベース ロールにデータベース ユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="5a168-142">Add the database user to any required database roles.</span></span>
7. <span data-ttu-id="5a168-143">このファイルのようになります。</span><span class="sxs-lookup"><span data-stu-id="5a168-143">The file should resemble this:</span></span>

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. <span data-ttu-id="5a168-144">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="5a168-144">Save the file.</span></span>

## <a name="executing-the-script-on-the-target-database"></a><span data-ttu-id="5a168-145">対象のデータベースでスクリプトの実行</span><span class="sxs-lookup"><span data-stu-id="5a168-145">Executing the Script on the Target Database</span></span>

<span data-ttu-id="5a168-146">理想的には、データベース プロジェクトを展開するときに、配置後スクリプトの一部として必要な TRANSACT-SQL スクリプトを実行するとします。</span><span class="sxs-lookup"><span data-stu-id="5a168-146">Ideally, you'd run any required Transact-SQL scripts as part of a post-deployment script when you deploy your database project.</span></span> <span data-ttu-id="5a168-147">ただし、配置後スクリプトでは、ソリューション構成またはビルド プロパティに基づく条件付きロジックを実行することはありません。</span><span class="sxs-lookup"><span data-stu-id="5a168-147">However, post-deployment scripts don't allow you to execute logic conditionally based on solution configurations or build properties.</span></span> <span data-ttu-id="5a168-148">MSBuild プロジェクト ファイルから直接作成して、SQL スクリプトを実行する方法が、**ターゲット**sqlcmd.exe コマンドを実行する要素。</span><span class="sxs-lookup"><span data-stu-id="5a168-148">The alternative is to run your SQL scripts directly from the MSBuild project file, by creating a **Target** element that executes a sqlcmd.exe command.</span></span> <span data-ttu-id="5a168-149">このコマンドを使用して、対象のデータベースでスクリプトを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="5a168-149">You can use this command to run your script on the target database:</span></span>


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> <span data-ttu-id="5a168-150">Sqlcmd コマンド ライン オプションの詳細については、次を参照してください。 [sqlcmd ユーティリティ](https://msdn.microsoft.com/library/ms162773.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="5a168-150">For more information on sqlcmd command-line options, see [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).</span></span>


<span data-ttu-id="5a168-151">このコマンドを埋め込むには、MSBuild ターゲットで、前に、スクリプトを実行するどのような条件を考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a168-151">Before you embed this command in an MSBuild target, you need to consider under what conditions you want the script to run:</span></span>

- <span data-ttu-id="5a168-152">そのロールのメンバーシップを変更する前に、ターゲット データベースが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a168-152">The target database must exist before you change its role memberships.</span></span> <span data-ttu-id="5a168-153">そのため、このスクリプトを実行する必要があります*後*データベース デプロイします。</span><span class="sxs-lookup"><span data-stu-id="5a168-153">As such, you need to run this script *after* the database deployment.</span></span>
- <span data-ttu-id="5a168-154">スクリプトは、テスト環境にのみ実行されるように条件を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a168-154">You need to include a condition so that the script is only executed for test environments.</span></span>
- <span data-ttu-id="5a168-155">"What if"展開を実行しているかどうかは&#x2014;つまり、配置スクリプトの生成が実際に実行しているかどうかは&#x2014;SQL スクリプトを実行することはできません。</span><span class="sxs-lookup"><span data-stu-id="5a168-155">If you're running a "what if" deployment&#x2014;in other words, if you're generating deployment scripts but not actually running them&#x2014;you shouldn't run the SQL script.</span></span>

<span data-ttu-id="5a168-156">説明されている分割プロジェクト ファイルの方法を使用しているかどうかは[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、Contact Manager サンプルのソリューションで示したように、このような SQL スクリプトのビルド手順を分割することができます。</span><span class="sxs-lookup"><span data-stu-id="5a168-156">If you're using the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), as demonstrated by the Contact Manager sample solution, you can split the build instructions for your SQL script like this:</span></span>

- <span data-ttu-id="5a168-157">必須のアクセス許可をデプロイするかどうかを決定するプロパティと共に、環境固有のプロパティは、環境固有のプロジェクト ファイルに移動する必要があります (たとえば、 *Env Dev.proj*)。</span><span class="sxs-lookup"><span data-stu-id="5a168-157">Any required environment-specific properties, together with the property that determines whether to deploy permissions, should go in the environment-specific project file (for example, *Env-Dev.proj*).</span></span>
- <span data-ttu-id="5a168-158">MSBuild ターゲット自体とその展開先の環境間変更されない任意のプロパティは、ユニバーサル プロジェクト ファイルに移動する必要があります (たとえば、 *Publish.proj*)。</span><span class="sxs-lookup"><span data-stu-id="5a168-158">The MSBuild target itself, together with any properties that will not change between destination environments, should go in the universal project file (for example, *Publish.proj*).</span></span>

<span data-ttu-id="5a168-159">環境固有のプロジェクト ファイルでは、データベース サーバー名、ターゲット データベース名、およびユーザー ロールのメンバーシップをデプロイするかどうかを指定するブール型プロパティを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a168-159">In the environment-specific project file, you need to define the database server name, the target database name, and a Boolean property that lets the user specify whether to deploy role memberships.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


<span data-ttu-id="5a168-160">ユニバーサル プロジェクト ファイルでは、sqlcmd 実行可能ファイルの場所とを実行する SQL スクリプトの場所を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a168-160">In the universal project file, you need to provide the location of the sqlcmd executable and the location of the SQL script you want to run.</span></span> <span data-ttu-id="5a168-161">これらのプロパティは、移行先の環境に関係なく同じになります。</span><span class="sxs-lookup"><span data-stu-id="5a168-161">These properties will remain the same regardless of the destination environment.</span></span> <span data-ttu-id="5a168-162">Sqlcmd コマンドを実行する MSBuild ターゲットを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a168-162">You also need to create an MSBuild target to execute the sqlcmd command.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


<span data-ttu-id="5a168-163">その他のターゲットに役立つことが考えられます sqlcmd 実行可能ファイルの場所は、静的なプロパティとして追加することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5a168-163">Notice that you add the location of the sqlcmd executable as a static property, as this could be useful to other targets.</span></span> <span data-ttu-id="5a168-164">これに対し、として定義する SQL スクリプトの場所と sqlcmd コマンドの構文、ターゲット内の動的プロパティに属していない場合は必要なターゲットが実行される前にします。</span><span class="sxs-lookup"><span data-stu-id="5a168-164">In contrast, you define the location of your SQL script and the syntax of the sqlcmd command as dynamic properties within the target, as they will not be required before the target is executed.</span></span> <span data-ttu-id="5a168-165">ここで、 **DeployTestDBPermissions**ターゲットは、これらの条件が満たされた場合にのみ実行されます。</span><span class="sxs-lookup"><span data-stu-id="5a168-165">In this case, the **DeployTestDBPermissions** target will only be executed if these conditions are met:</span></span>

- <span data-ttu-id="5a168-166">**DeployTestDBRoleMemberships**プロパティに設定されて**true**します。</span><span class="sxs-lookup"><span data-stu-id="5a168-166">The **DeployTestDBRoleMemberships** property is set to **true**.</span></span>
- <span data-ttu-id="5a168-167">ユーザーが指定されていない、 **WhatIf = true**フラグ。</span><span class="sxs-lookup"><span data-stu-id="5a168-167">The user hasn't specified a **WhatIf=true** flag.</span></span>

<span data-ttu-id="5a168-168">最後に、忘れずに、ターゲットを起動します。</span><span class="sxs-lookup"><span data-stu-id="5a168-168">Finally, don't forget to invoke the target.</span></span> <span data-ttu-id="5a168-169">*Publish.proj*ファイル、これを行う既定の依存関係の一覧にターゲットを追加して**FullPublish**ターゲット。</span><span class="sxs-lookup"><span data-stu-id="5a168-169">In the *Publish.proj* file, you can do this by adding the target to the dependency list for the default **FullPublish** target.</span></span> <span data-ttu-id="5a168-170">いることを確認する必要がある、 **DeployTestDBPermissions**までターゲットは実行されません、 **PublishDbPackages**ターゲットが実行されています。</span><span class="sxs-lookup"><span data-stu-id="5a168-170">You need to ensure that the **DeployTestDBPermissions** target is not executed until the **PublishDbPackages** target has been executed.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a><span data-ttu-id="5a168-171">まとめ</span><span class="sxs-lookup"><span data-stu-id="5a168-171">Conclusion</span></span>

<span data-ttu-id="5a168-172">このトピックでは、1 つを追加する方法データベース ユーザーとロール メンバーシップ配置後アクションとしてデータベース プロジェクトをデプロイするときについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5a168-172">This topic described one way in which you can add database users and role memberships as a post-deployment action when you deploy a database project.</span></span> <span data-ttu-id="5a168-173">これは、機能は、定期的にテスト環境でデータベースを再作成するが、通常はできるだけ避けてステージングまたは運用環境にデータベースを展開するときに通常は便利です。</span><span class="sxs-lookup"><span data-stu-id="5a168-173">This is typically useful when you regularly re-create a database in a test environment, but it should usually be avoided when you deploy databases to staging or production environments.</span></span> <span data-ttu-id="5a168-174">そのため、データベース ユーザーおよびロールのメンバーシップが作成されるようのみこれを行うには適切なタイミングは、必要な条件付きロジックを使用することを確認してください。</span><span class="sxs-lookup"><span data-stu-id="5a168-174">As such, you should ensure that you use the necessary conditional logic so that database users and role memberships are only created when it's appropriate to do so.</span></span>

## <a name="further-reading"></a><span data-ttu-id="5a168-175">関連項目</span><span class="sxs-lookup"><span data-stu-id="5a168-175">Further Reading</span></span>

<span data-ttu-id="5a168-176">VSDBCMD を使用してデータベース プロジェクトを配置する詳細については、次を参照してください。[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)します。</span><span class="sxs-lookup"><span data-stu-id="5a168-176">For more information on using VSDBCMD to deploy database projects, see [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="5a168-177">別のターゲット環境のデータベースの配置をカスタマイズする方法のガイダンスについては、次を参照してください。[複数の環境のデータベースの配置をカスタマイズする](customizing-database-deployments-for-multiple-environments.md)します。</span><span class="sxs-lookup"><span data-stu-id="5a168-177">For guidance on customizing database deployments for different target environments, see [Customizing Database Deployments for Multiple Environments](customizing-database-deployments-for-multiple-environments.md).</span></span> <span data-ttu-id="5a168-178">カスタム MSBuild プロジェクト ファイルを使用して、展開プロセスを制御する詳細については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。</span><span class="sxs-lookup"><span data-stu-id="5a168-178">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="5a168-179">Sqlcmd コマンド ライン オプションの詳細については、次を参照してください。 [sqlcmd ユーティリティ](https://msdn.microsoft.com/library/ms162773.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="5a168-179">For more information on sqlcmd command-line options, see [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5a168-180">[前へ](customizing-database-deployments-for-multiple-environments.md)
> [次へ](deploying-membership-databases-to-enterprise-environments.md)</span><span class="sxs-lookup"><span data-stu-id="5a168-180">[Previous](customizing-database-deployments-for-multiple-environments.md)
[Next](deploying-membership-databases-to-enterprise-environments.md)</span></span>
