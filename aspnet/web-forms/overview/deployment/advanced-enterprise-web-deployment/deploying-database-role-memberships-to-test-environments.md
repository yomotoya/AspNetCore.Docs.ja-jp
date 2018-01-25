---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: "データベース ロール メンバーシップをテスト環境に展開する |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、テスト環境にソリューションの配置の一部として、データベース ロールにユーザー アカウントを追加する方法について説明します。 含むソリューションを展開するときにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 226c28622f76e866fba1fc33cf9b9b7a01e5295b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="deploying-database-role-memberships-to-test-environments"></a><span data-ttu-id="c61f3-104">データベース ロール メンバーシップをテスト環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-104">Deploying Database Role Memberships to Test Environments</span></span>
====================
<span data-ttu-id="c61f3-105">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="c61f3-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="c61f3-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="c61f3-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="c61f3-107">このトピックでは、テスト環境にソリューションの配置の一部として、データベース ロールにユーザー アカウントを追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-107">This topic describes how to add user accounts to database roles as part of a solution deployment to a test environment.</span></span>
> 
> <span data-ttu-id="c61f3-108">ステージングまたは運用環境にデータベース プロジェクトを含むソリューションを展開するときに通常しない開発者はデータベース ロールにユーザー アカウントの追加を自動化します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-108">When you deploy a solution containing a database project to a staging or production environment, you typically don't want the developer to automate the addition of user accounts to database roles.</span></span> <span data-ttu-id="c61f3-109">ほとんどの場合、どのデータベース ロールに追加する必要があるユーザー アカウントを認識せず、開発者といつでもこれらの要件を変更する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c61f3-109">In most cases, the developer won't know which user accounts need to be added to which database roles, and these requirements could change at any time.</span></span> <span data-ttu-id="c61f3-110">ただし、開発用にデータベース プロジェクトを含むソリューションを配置または、環境をテストするときに、この状況は、通常ではなく別。</span><span class="sxs-lookup"><span data-stu-id="c61f3-110">However, when you deploy a solution containing a database project to a development or test environment, the situation is usually rather different:</span></span>
> 
> - <span data-ttu-id="c61f3-111">開発者は、通常再展開を定期的にソリューション 1 日に数回多くの場合。</span><span class="sxs-lookup"><span data-stu-id="c61f3-111">The developer typically re-deploys the solution on a regular basis, often several times a day.</span></span>
> - <span data-ttu-id="c61f3-112">通常、データベースがすべての配置では、データベース ユーザーの作成し、すべての展開後に、ロールに追加する必要があることを意味で再作成されます。</span><span class="sxs-lookup"><span data-stu-id="c61f3-112">The database is typically re-created on every deployment, which means that database users must be created and added to roles after every deployment.</span></span>
> - <span data-ttu-id="c61f3-113">開発者は、ターゲット開発またはテスト環境を完全に制御を通常はします。</span><span class="sxs-lookup"><span data-stu-id="c61f3-113">The developer typically has full control over the target development or test environment.</span></span>
> 
> <span data-ttu-id="c61f3-114">このシナリオでは、データベース ユーザーを自動的に作成し、展開プロセスの一部としてデータベース ロール メンバーシップを割り当てると役に立つは多くの場合です。</span><span class="sxs-lookup"><span data-stu-id="c61f3-114">In this scenario, it's often beneficial to automatically create database users and assign database role memberships as part of the deployment process.</span></span>
> 
> <span data-ttu-id="c61f3-115">キー係数この操作する必要がある、条件付きターゲット環境に基づきます。</span><span class="sxs-lookup"><span data-stu-id="c61f3-115">The key factor is that this operation needs to be conditional based on the target environment.</span></span> <span data-ttu-id="c61f3-116">を、ステージングまたは運用環境に配置する場合は、操作をスキップします。</span><span class="sxs-lookup"><span data-stu-id="c61f3-116">If you're deploying to a staging or a production environment, you want to skip the operation.</span></span> <span data-ttu-id="c61f3-117">開発者に配置する環境をテストするか、展開するロールのメンバーシップをそれ以上の介入なし。</span><span class="sxs-lookup"><span data-stu-id="c61f3-117">If you're deploying to a developer or test environment, you want to deploy role memberships without further intervention.</span></span> <span data-ttu-id="c61f3-118">このトピックでは、この課題に対処する 1 つのアプローチについて説明します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-118">This topic describes one approach you can use to address this challenge.</span></span>


<span data-ttu-id="c61f3-119">このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。サンプル ソリューション & #x 2014; このチュートリアルのシリーズを使用して、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; を ASP.NET MVC 3 アプリケーションを Windows のなどの複雑性のレベルが現実的な web アプリケーションを表すCommunication Foundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="c61f3-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="c61f3-120">説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスで 2 つのプロジェクト ファイル & #x 2014; 1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。</span><span class="sxs-lookup"><span data-stu-id="c61f3-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="c61f3-121">ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="c61f3-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="c61f3-122">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="c61f3-122">Task Overview</span></span>

<span data-ttu-id="c61f3-123">このトピックを前提とします。</span><span class="sxs-lookup"><span data-stu-id="c61f3-123">This topic assumes that:</span></span>

- <span data-ttu-id="c61f3-124">」の説明に従って、ソリューションの配置をプロジェクト ファイルの分割アプローチを使用する[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)です。</span><span class="sxs-lookup"><span data-stu-id="c61f3-124">You use the split project file approach to solution deployment, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span>
- <span data-ttu-id="c61f3-125">」の説明に従って、データベース プロジェクトを配置するプロジェクト ファイルから VSDBCMD を呼び出す[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。</span><span class="sxs-lookup"><span data-stu-id="c61f3-125">You call VSDBCMD from the project file to deploy your database project, as described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

<span data-ttu-id="c61f3-126">データベース ユーザーを作成し、データベース プロジェクトをテスト環境に配置するときに、ロールのメンバーシップを割り当てる、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c61f3-126">To create database users and assign role memberships when you deploy a database project to a test environment, you'll need to:</span></span>

- <span data-ttu-id="c61f3-127">必要なデータベースの変更は、Transact の構造化照会言語 (TRANSACT-SQL) スクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-127">Create a Transact Structured Query Language (Transact-SQL) script that makes the necessary database changes.</span></span>
- <span data-ttu-id="c61f3-128">SQL スクリプトを実行するには、sqlcmd.exe ユーティリティを使用して Microsoft Build Engine (MSBuild) ターゲットを作成します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-128">Create a Microsoft Build Engine (MSBuild) target that uses the sqlcmd.exe utility to run the SQL script.</span></span>
- <span data-ttu-id="c61f3-129">テスト環境にソリューションを配置するときに、ターゲットを呼び出すように、プロジェクト ファイルを構成します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-129">Configure your project files to invoke the target when you're deploying your solution to a test environment.</span></span>

<span data-ttu-id="c61f3-130">このトピックでは、各手順を実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-130">This topic will show you how to perform each of these procedures.</span></span>

## <a name="scripting-the-database-role-memberships"></a><span data-ttu-id="c61f3-131">データベース ロールのメンバーシップをスクリプト作成</span><span class="sxs-lookup"><span data-stu-id="c61f3-131">Scripting the Database Role Memberships</span></span>

<span data-ttu-id="c61f3-132">任意の場所を選択して、多くのさまざまな方法は、TRANSACT-SQL スクリプトを作成できます。</span><span class="sxs-lookup"><span data-stu-id="c61f3-132">You can create a Transact-SQL script in a lot of different ways, and in any location you choose.</span></span> <span data-ttu-id="c61f3-133">最も簡単な方法では、Visual Studio 2010 で、ソリューション内でスクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-133">The easiest approach is to create the script within your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="c61f3-134">**SQL スクリプトを作成するには**</span><span class="sxs-lookup"><span data-stu-id="c61f3-134">**To create a SQL script**</span></span>

1. <span data-ttu-id="c61f3-135">**ソリューション エクスプ ローラー**ウィンドウで、データベース プロジェクト ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-135">In the **Solution Explorer** window, expand your database project node.</span></span>
2. <span data-ttu-id="c61f3-136">右クリックし、**スクリプト**フォルダーを指す**追加**、順にクリック**新しいフォルダー**です。</span><span class="sxs-lookup"><span data-stu-id="c61f3-136">Right-click the **Scripts** folder, point to **Add**, and then click **New Folder**.</span></span>
3. <span data-ttu-id="c61f3-137">型**テスト**としてフォルダーの名前とし、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-137">Type **Test** as the folder name, and then press Enter.</span></span>
4. <span data-ttu-id="c61f3-138">右クリックし、**テスト**フォルダーを指す**追加**、順にクリック**スクリプト**です。</span><span class="sxs-lookup"><span data-stu-id="c61f3-138">Right-click the **Test** folder, point to **Add**, and then click **Script**.</span></span>
5. <span data-ttu-id="c61f3-139">**新しい項目の追加** ダイアログ ボックスで、スクリプトにわかりやすい名前を付けます (たとえば、 **AddRoleMemberships.sql**)、をクリックし、**追加**です。</span><span class="sxs-lookup"><span data-stu-id="c61f3-139">In the **Add New Item** dialog box, give your script a meaningful name (for example, **AddRoleMemberships.sql**), and then click **Add**.</span></span>

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. <span data-ttu-id="c61f3-140">*AddRoleMemberships.sql*ファイル、TRANSACT-SQL ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-140">In the *AddRoleMemberships.sql* file, add Transact-SQL statements that:</span></span>

    1. <span data-ttu-id="c61f3-141">データベースにアクセスする SQL Server ログインのデータベース ユーザーを作成します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-141">Create a database user for the SQL Server login that will access your database.</span></span>
    2. <span data-ttu-id="c61f3-142">データベース ユーザーを必要なデータベース ロールに追加します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-142">Add the database user to any required database roles.</span></span>
7. <span data-ttu-id="c61f3-143">ファイルは、このようになります。</span><span class="sxs-lookup"><span data-stu-id="c61f3-143">The file should resemble this:</span></span>

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. <span data-ttu-id="c61f3-144">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-144">Save the file.</span></span>

## <a name="executing-the-script-on-the-target-database"></a><span data-ttu-id="c61f3-145">対象のデータベースでのスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-145">Executing the Script on the Target Database</span></span>

<span data-ttu-id="c61f3-146">理想的には、データベース プロジェクトを展開するときに、配置後スクリプトの一部として、必要な TRANSACT-SQL スクリプトを実行するとします。</span><span class="sxs-lookup"><span data-stu-id="c61f3-146">Ideally, you'd run any required Transact-SQL scripts as part of a post-deployment script when you deploy your database project.</span></span> <span data-ttu-id="c61f3-147">ただし、配置後スクリプトは、ソリューションの構成またはビルド プロパティに基づく条件付きロジックを実行することはできません。</span><span class="sxs-lookup"><span data-stu-id="c61f3-147">However, post-deployment scripts don't allow you to execute logic conditionally based on solution configurations or build properties.</span></span> <span data-ttu-id="c61f3-148">代替手段が MSBuild プロジェクト ファイルから直接作成することで、SQL スクリプトを実行するには、**ターゲット**sqlcmd.exe コマンドを実行する要素。</span><span class="sxs-lookup"><span data-stu-id="c61f3-148">The alternative is to run your SQL scripts directly from the MSBuild project file, by creating a **Target** element that executes a sqlcmd.exe command.</span></span> <span data-ttu-id="c61f3-149">このコマンドを使用すると、ターゲット データベースで、スクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-149">You can use this command to run your script on the target database:</span></span>


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> <span data-ttu-id="c61f3-150">Sqlcmd コマンド ライン オプションの詳細については、次を参照してください。 [sqlcmd ユーティリティ](https://msdn.microsoft.com/library/ms162773.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="c61f3-150">For more information on sqlcmd command-line options, see [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).</span></span>


<span data-ttu-id="c61f3-151">MSBuild ターゲットでこのコマンドを埋め込むと、前に、スクリプトを実行するどのような条件を考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c61f3-151">Before you embed this command in an MSBuild target, you need to consider under what conditions you want the script to run:</span></span>

- <span data-ttu-id="c61f3-152">そのロールのメンバーシップを変更する前に、ターゲット データベースが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c61f3-152">The target database must exist before you change its role memberships.</span></span> <span data-ttu-id="c61f3-153">そのため、このスクリプトを実行する必要があります*後*データベース デプロイします。</span><span class="sxs-lookup"><span data-stu-id="c61f3-153">As such, you need to run this script *after* the database deployment.</span></span>
- <span data-ttu-id="c61f3-154">スクリプトは、テスト環境でのみ実行されるように条件を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="c61f3-154">You need to include a condition so that the script is only executed for test environments.</span></span>
- <span data-ttu-id="c61f3-155">"What-if"展開 & #x 2014; を実行している場合つまり、配置スクリプトを生成するが、実際にその & #x 2014; を実行している場合するべきではありません SQL スクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-155">If you're running a "what if" deployment&#x2014;in other words, if you're generating deployment scripts but not actually running them&#x2014;you shouldn't run the SQL script.</span></span>

<span data-ttu-id="c61f3-156">説明されている分割プロジェクト ファイルのアプローチを使用しているかどうかは[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)連絡先のマネージャーのサンプル ソリューションで示したように、次のように、SQL スクリプトのビルド手順を分割することができます。</span><span class="sxs-lookup"><span data-stu-id="c61f3-156">If you're using the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), as demonstrated by the Contact Manager sample solution, you can split the build instructions for your SQL script like this:</span></span>

- <span data-ttu-id="c61f3-157">必要なアクセス許可を展開するかどうかを決定するプロパティと共に、環境に固有のプロパティが環境に固有のプロジェクト ファイルに移動する必要があります (たとえば、 *Env Dev.proj*)。</span><span class="sxs-lookup"><span data-stu-id="c61f3-157">Any required environment-specific properties, together with the property that determines whether to deploy permissions, should go in the environment-specific project file (for example, *Env-Dev.proj*).</span></span>
- <span data-ttu-id="c61f3-158">その展開先環境間で変化しない任意のプロパティ自体には、MSBuild ターゲット ユニバーサル プロジェクト ファイル内で移動する必要があります (たとえば、 *Publish.proj*)。</span><span class="sxs-lookup"><span data-stu-id="c61f3-158">The MSBuild target itself, together with any properties that will not change between destination environments, should go in the universal project file (for example, *Publish.proj*).</span></span>

<span data-ttu-id="c61f3-159">環境に固有のプロジェクト ファイルでは、データベース サーバー名、ターゲット データベース名およびロールのメンバーシップを展開するかどうかを指定するブール型プロパティを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c61f3-159">In the environment-specific project file, you need to define the database server name, the target database name, and a Boolean property that lets the user specify whether to deploy role memberships.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


<span data-ttu-id="c61f3-160">ユニバーサル プロジェクト ファイルでは、sqlcmd の実行可能ファイルの場所とを実行する SQL スクリプトの場所を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c61f3-160">In the universal project file, you need to provide the location of the sqlcmd executable and the location of the SQL script you want to run.</span></span> <span data-ttu-id="c61f3-161">これらのプロパティは、展開先の環境に関係なく同じになります。</span><span class="sxs-lookup"><span data-stu-id="c61f3-161">These properties will remain the same regardless of the destination environment.</span></span> <span data-ttu-id="c61f3-162">また、sqlcmd コマンドを実行する MSBuild ターゲットを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c61f3-162">You also need to create an MSBuild target to execute the sqlcmd command.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


<span data-ttu-id="c61f3-163">他のターゲットに役立つ可能性があります、sqlcmd の実行可能ファイルの場所は、静的なプロパティとして追加することを確認します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-163">Notice that you add the location of the sqlcmd executable as a static property, as this could be useful to other targets.</span></span> <span data-ttu-id="c61f3-164">これに対し、定義する、SQL スクリプトの場所と sqlcmd コマンドの構文、ターゲット内の動的なプロパティとして可能性が必要なターゲットが実行される前に、します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-164">In contrast, you define the location of your SQL script and the syntax of the sqlcmd command as dynamic properties within the target, as they will not be required before the target is executed.</span></span> <span data-ttu-id="c61f3-165">ここで、 **DeployTestDBPermissions**ターゲットは、これらの条件が満たされた場合にのみ実行されます。</span><span class="sxs-lookup"><span data-stu-id="c61f3-165">In this case, the **DeployTestDBPermissions** target will only be executed if these conditions are met:</span></span>

- <span data-ttu-id="c61f3-166">**DeployTestDBRoleMemberships**プロパティに設定されている**true**です。</span><span class="sxs-lookup"><span data-stu-id="c61f3-166">The **DeployTestDBRoleMemberships** property is set to **true**.</span></span>
- <span data-ttu-id="c61f3-167">ユーザーが指定されていない、 **WhatIf = true**フラグ。</span><span class="sxs-lookup"><span data-stu-id="c61f3-167">The user hasn't specified a **WhatIf=true** flag.</span></span>

<span data-ttu-id="c61f3-168">最後に、忘れずにターゲットを起動します。</span><span class="sxs-lookup"><span data-stu-id="c61f3-168">Finally, don't forget to invoke the target.</span></span> <span data-ttu-id="c61f3-169">*Publish.proj*ファイル、これを行うターゲットを既定の依存関係の一覧に追加することによって**FullPublish**ターゲットです。</span><span class="sxs-lookup"><span data-stu-id="c61f3-169">In the *Publish.proj* file, you can do this by adding the target to the dependency list for the default **FullPublish** target.</span></span> <span data-ttu-id="c61f3-170">いることを確認する必要があります、 **DeployTestDBPermissions**までターゲットは実行されません、 **PublishDbPackages**ターゲットが実行されました。</span><span class="sxs-lookup"><span data-stu-id="c61f3-170">You need to ensure that the **DeployTestDBPermissions** target is not executed until the **PublishDbPackages** target has been executed.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a><span data-ttu-id="c61f3-171">まとめ</span><span class="sxs-lookup"><span data-stu-id="c61f3-171">Conclusion</span></span>

<span data-ttu-id="c61f3-172">このトピックでは、1 つを追加する方法データベース ユーザーとロールのメンバーシップ配置後アクションとして、データベース プロジェクトを展開するときに説明されています。</span><span class="sxs-lookup"><span data-stu-id="c61f3-172">This topic described one way in which you can add database users and role memberships as a post-deployment action when you deploy a database project.</span></span> <span data-ttu-id="c61f3-173">これは、機能は、定期的にテスト環境でデータベースを再作成するが、通常できるだけ避けてステージングまたは運用環境にデータベースを展開するときに一般的に便利です。</span><span class="sxs-lookup"><span data-stu-id="c61f3-173">This is typically useful when you regularly re-create a database in a test environment, but it should usually be avoided when you deploy databases to staging or production environments.</span></span> <span data-ttu-id="c61f3-174">そのため、データベース ユーザーとロールのメンバーシップが作成されるようにのみこれを行うことをお勧めときに、必要な条件付きロジックを使用することを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c61f3-174">As such, you should ensure that you use the necessary conditional logic so that database users and role memberships are only created when it's appropriate to do so.</span></span>

## <a name="further-reading"></a><span data-ttu-id="c61f3-175">関連項目</span><span class="sxs-lookup"><span data-stu-id="c61f3-175">Further Reading</span></span>

<span data-ttu-id="c61f3-176">VSDBCMD を使用してデータベース プロジェクトを配置する方法の詳細については、次を参照してください。[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)です。</span><span class="sxs-lookup"><span data-stu-id="c61f3-176">For more information on using VSDBCMD to deploy database projects, see [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="c61f3-177">別のターゲット環境のデータベースの配置をカスタマイズする方法のガイダンスについては、次を参照してください。[複数の環境のデータベースの配置をカスタマイズする](customizing-database-deployments-for-multiple-environments.md)です。</span><span class="sxs-lookup"><span data-stu-id="c61f3-177">For guidance on customizing database deployments for different target environments, see [Customizing Database Deployments for Multiple Environments](customizing-database-deployments-for-multiple-environments.md).</span></span> <span data-ttu-id="c61f3-178">展開プロセスを制御するカスタム MSBuild プロジェクト ファイルを使用する方法については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。</span><span class="sxs-lookup"><span data-stu-id="c61f3-178">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="c61f3-179">Sqlcmd コマンド ライン オプションの詳細については、次を参照してください。 [sqlcmd ユーティリティ](https://msdn.microsoft.com/library/ms162773.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="c61f3-179">For more information on sqlcmd command-line options, see [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c61f3-180">[前へ](customizing-database-deployments-for-multiple-environments.md)
[次へ](deploying-membership-databases-to-enterprise-environments.md)</span><span class="sxs-lookup"><span data-stu-id="c61f3-180">[Previous](customizing-database-deployments-for-multiple-environments.md)
[Next](deploying-membership-databases-to-enterprise-environments.md)</span></span>
