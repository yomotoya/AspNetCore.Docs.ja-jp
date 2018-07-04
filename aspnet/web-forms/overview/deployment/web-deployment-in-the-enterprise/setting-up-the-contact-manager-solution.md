---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: 連絡先マネージャー ソリューションのセットアップ |Microsoft Docs
author: jrjlee
description: このトピックでは、ダウンロードしてローカル開発用ワークステーションで実行する連絡先マネージャー ソリューションを構成する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: a7675a793909ec4d95164ee47a3a43f73600c5bc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366690"
---
<a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="9c0b7-103">連絡先マネージャー ソリューションの設定</span><span class="sxs-lookup"><span data-stu-id="9c0b7-103">Setting Up the Contact Manager Solution</span></span>
====================
<span data-ttu-id="9c0b7-104">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="9c0b7-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="9c0b7-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="9c0b7-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="9c0b7-106">このトピックでは、ダウンロードしてローカル開発用ワークステーションで実行する連絡先マネージャー ソリューションを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>


## <a name="system-requirements"></a><span data-ttu-id="9c0b7-107">システム要件</span><span class="sxs-lookup"><span data-stu-id="9c0b7-107">System Requirements</span></span>

<span data-ttu-id="9c0b7-108">連絡先マネージャー ソリューションをローカルで実行して、このチュートリアルで説明されているその他のタスクを実行するには、開発者のワークステーションにこのソフトウェアをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="9c0b7-109">Visual Studio 2010 Service Pack 1、Premium または Ultimate Edition</span><span class="sxs-lookup"><span data-stu-id="9c0b7-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="9c0b7-110">インターネット インフォメーション サービス (IIS) 7.5 Express します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="9c0b7-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="9c0b7-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="9c0b7-112">IIS Web 配置ツール (Web 配置) 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="9c0b7-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="9c0b7-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="9c0b7-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="9c0b7-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="9c0b7-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="9c0b7-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="9c0b7-115">.NET Framework 4</span></span>
- <span data-ttu-id="9c0b7-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="9c0b7-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="9c0b7-117">Visual Studio 2010 を除くには、ダウンロードしてこれらの製品とコンポーネントを経由のすべての最新バージョンをインストールすることができます、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="9c0b7-118">ダウンロードして、ソリューションを展開します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-118">Download and Extract the Solution</span></span>

<span data-ttu-id="9c0b7-119">連絡先のマネージャーのサンプル アプリケーションをダウンロードするには、MSDN コード ギャラリーから[ここ](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0)します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="9c0b7-120">構成し、ソリューションの実行</span><span class="sxs-lookup"><span data-stu-id="9c0b7-120">Configure and Run the Solution</span></span>

<span data-ttu-id="9c0b7-121">で構成して、ローカル コンピューターに、連絡先マネージャー ソリューションを実行するには、これらの大まかな手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="9c0b7-122">既に 1 つがあるないの場合は、メンバーシップとロール管理機能を有効になっているローカル ASP.NET アプリケーション サービス データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="9c0b7-123">内の接続文字列を編集、 *web.config*ファイルをローカルの SQL Server Express インスタンスをポイントします。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="9c0b7-124">Visual Studio 2010 からソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="9c0b7-125">このセクションの残りの部分では、これらの各タスクを完了する方法に関するガイダンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="9c0b7-126">**アプリケーション サービス データベースを作成するには**</span><span class="sxs-lookup"><span data-stu-id="9c0b7-126">**To create the application services database**</span></span>

1. <span data-ttu-id="9c0b7-127">Visual Studio 2010 コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="9c0b7-128">これを実行する、**開始**メニューで、**すべてのプログラム**、 をクリックして**Microsoft Visual Studio 2010**、 をクリックして**Visual Studio Tools**、し、クリックして**Visual Studio コマンド プロンプト (2010)** します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="9c0b7-129">コマンド プロンプトで、このコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="9c0b7-130">使用して、 **– C**スイッチを使用するデータベース サーバーの接続文字列を指定します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="9c0b7-131">使用して、 **– A**アプリケーション サービス データベースに追加する機能を指定するスイッチ。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="9c0b7-132">この場合、 **m**メンバーシップ プロバイダーのサポートを追加することを示しますと**r**ロール マネージャーのサポートを追加することを示します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="9c0b7-133">使用して、 **– d**スイッチをアプリケーション サービス データベースの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="9c0b7-134">このスイッチを省略した場合、ユーティリティの既定の名前を持つデータベースが作成されます**aspnetdb**します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="9c0b7-135">データベースが正常に作成されて、コマンド プロンプトに確認メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="9c0b7-136">詳細については、aspnet\_regsql ユーティリティを参照してください[ASP.NET SQL Server の登録ツール (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>


<span data-ttu-id="9c0b7-137">次の手順では、連絡先マネージャー ソリューション内の接続文字列が SQL Server Express のローカル インスタンスをポイントするかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="9c0b7-138">**接続文字列を更新するには**</span><span class="sxs-lookup"><span data-stu-id="9c0b7-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="9c0b7-139">Visual Studio 2010 では、連絡先マネージャー ソリューションを開きます。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="9c0b7-140">**ソリューション エクスプ ローラー**ウィンドウで、展開、 **ContactManager.Mvc**プロジェクト、し、ダブルクリック、 **Web.config**ノード。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9c0b7-141">ContactManager.Mvc プロジェクトには、2 つが含まれます*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="9c0b7-142">プロジェクト レベルのファイルを編集する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="9c0b7-143">**ConnectionStrings**要素、という名前の接続文字列を確認します。 **ApplicationServices** 、ローカルの ASP.NET アプリケーション サービス データベースを指します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="9c0b7-144">**ソリューション エクスプ ローラー**ウィンドウで、展開、 **ContactManager.Service**プロジェクト、し、ダブルクリック、 **Web.config**ノード。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="9c0b7-145">**ConnectionStrings**という接続文字列内の要素**ContactManagerContext**、いることを確認、**データ ソース**プロパティが SQL のローカル インスタンスに設定サーバーの高速です。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="9c0b7-146">接続文字列内の他に何も変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="9c0b7-147">開いているすべてのファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-147">Save all open files.</span></span>

<span data-ttu-id="9c0b7-148">ローカル コンピューターに、連絡先マネージャー ソリューションを実行する準備がなります。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="9c0b7-149">ASP.NET は、アプリケーション サービス データベースを作成しなくてもこの手順を実行する場合は、データベースが初めてのユーザーを作成しようとしたに作成されます。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="9c0b7-150">ただし、手動でデータベースの作成に制御、はるかに多くをサポートするアプリケーション サービスの機能セット。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>


<span data-ttu-id="9c0b7-151">**連絡先マネージャー ソリューションを実行するには**</span><span class="sxs-lookup"><span data-stu-id="9c0b7-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="9c0b7-152">Visual Studio 2010 で f5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="9c0b7-153">Internet Explorer が起動し、連絡先マネージャー ASP.NET MVC 3 アプリケーションの URL を要求します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="9c0b7-154">既定では、アプリケーションが表示されます、**すべての連絡先**ページ。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="9c0b7-155">いくつかの連絡先を追加し、アプリケーションが期待どおりに動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="9c0b7-156">参照する`http://localhost:50114/Account/Register`(別のポートでアプリケーションをホストしている場合は、URL を調整する)。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="9c0b7-157">ユーザー名、電子メール アドレス、およびパスワードを追加し、アカウントを正常に登録することであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="9c0b7-158">参照する`http://localhost:50114/Account/LogOn`(別のポートでアプリケーションをホストしている場合は、URL を調整する)。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="9c0b7-159">作成したアカウントを使用してログオンいただけることを確認します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="9c0b7-160">デバッグを停止する Internet Explorer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9c0b7-161">まとめ</span><span class="sxs-lookup"><span data-stu-id="9c0b7-161">Conclusion</span></span>

<span data-ttu-id="9c0b7-162">この時点では、ローカル コンピューター上で実行する連絡先マネージャー ソリューションを完全に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="9c0b7-163">このチュートリアルでは、他のトピックに従って操作すると、参照としてソリューションを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="9c0b7-164">次のトピックでは、[プロジェクト ファイルを理解する](understanding-the-project-file.md)、連絡先マネージャー ソリューション内では、カスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して、展開プロセスを制御する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="9c0b7-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9c0b7-165">[前へ](the-contact-manager-solution.md)
> [次へ](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="9c0b7-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
