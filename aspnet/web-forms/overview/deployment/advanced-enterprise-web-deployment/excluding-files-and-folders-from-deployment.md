---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: ファイルとフォルダーの展開から除外 |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、方法できますを除外するファイルとフォルダー web 展開パッケージから作成および web アプリケーション プロジェクトをパッケージ化するときについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: c435448bf057bbef9127d66ffda24a07729f2322
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="excluding-files-and-folders-from-deployment"></a><span data-ttu-id="67e43-103">ファイルとフォルダーの展開から除外</span><span class="sxs-lookup"><span data-stu-id="67e43-103">Excluding Files and Folders from Deployment</span></span>
====================
<span data-ttu-id="67e43-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="67e43-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="67e43-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="67e43-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="67e43-106">このトピックでは、方法できますを除外するファイルとフォルダー web 展開パッケージから作成および web アプリケーション プロジェクトをパッケージ化するときについて説明します。</span><span class="sxs-lookup"><span data-stu-id="67e43-106">This topic describes how you can exclude files and folders from a web deployment package when you build and package a web application project.</span></span>


<span data-ttu-id="67e43-107">このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="67e43-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="67e43-108">説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。</span><span class="sxs-lookup"><span data-stu-id="67e43-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="67e43-109">ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="67e43-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="overview"></a><span data-ttu-id="67e43-110">概要</span><span class="sxs-lookup"><span data-stu-id="67e43-110">Overview</span></span>

<span data-ttu-id="67e43-111">Visual Studio 2010 で web アプリケーション プロジェクトをビルドすると、Web 発行パイプライン (WPP) では、配置可能な web のパッケージにコンパイルされた web アプリケーションをパッケージ化してこのビルド プロセスを拡張することができます。</span><span class="sxs-lookup"><span data-stu-id="67e43-111">When you build a web application project in Visual Studio 2010, the Web Publishing Pipeline (WPP) lets you extend this build process by packaging your compiled web application into a deployable web package.</span></span> <span data-ttu-id="67e43-112">この web パッケージを展開するリモートの IIS web サーバーにインターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) を使用したり、IIS マネージャーで手動で web のパッケージをインポートできます。</span><span class="sxs-lookup"><span data-stu-id="67e43-112">You can then use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy this web package to a remote IIS web server, or import the web package manually through IIS Manager.</span></span> <span data-ttu-id="67e43-113">このパッケージで解説されて[パッケージ Web アプリケーション プロジェクトのビルドと](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)です。</span><span class="sxs-lookup"><span data-stu-id="67e43-113">This packaging process is explained in [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span>

<span data-ttu-id="67e43-114">Web の内容を取得制御する方法をパッケージ化しますか。</span><span class="sxs-lookup"><span data-stu-id="67e43-114">So how do you control what gets included in your web package?</span></span> <span data-ttu-id="67e43-115">基になるプロジェクト ファイルを使って Visual Studio で、プロジェクトの設定は、シナリオの多くの十分な制御を提供します。</span><span class="sxs-lookup"><span data-stu-id="67e43-115">The project settings in Visual Studio, through the underlying project file, provide sufficient control for a lot of scenarios.</span></span> <span data-ttu-id="67e43-116">しかし、場合によっては、特定の展開先環境は、web パッケージの内容を調整する場合があります。</span><span class="sxs-lookup"><span data-stu-id="67e43-116">However, in some cases you may want to tailor the contents of your web package to specific destination environments.</span></span> <span data-ttu-id="67e43-117">たとえば、テスト環境にアプリケーションを配置するが、ステージングまたは運用環境へのアプリケーションを展開するときに、フォルダーを除外するときに、ログ ファイル用のフォルダーを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="67e43-117">For example, you might want to include a folder for log files when you deploy your application to a test environment but exclude the folder when you deploy the application to a staging or production environment.</span></span> <span data-ttu-id="67e43-118">このトピックでは、これを行う方法を示します。</span><span class="sxs-lookup"><span data-stu-id="67e43-118">This topic will show you how to do this.</span></span>

## <a name="what-gets-included-by-default"></a><span data-ttu-id="67e43-119">既定で含まれるものを取得しますか。</span><span class="sxs-lookup"><span data-stu-id="67e43-119">What Gets Included by Default?</span></span>

<span data-ttu-id="67e43-120">Visual Studio で、web アプリケーション プロジェクトのプロパティを構成するときに、**を展開する項目**ボックスの一覧、**パッケージ化/発行 Web**ページでは、web 配置に含める対象を指定できます。パッケージです。</span><span class="sxs-lookup"><span data-stu-id="67e43-120">When you configure your web application project properties in Visual Studio, the **Items to deploy** list on the **Package/Publish Web** page lets you specify what you want to include in your web deployment package.</span></span> <span data-ttu-id="67e43-121">既定では、この設定は**このアプリケーションの実行に必要なファイルのみ**です。</span><span class="sxs-lookup"><span data-stu-id="67e43-121">By default, this is set to **Only files needed to run this application**.</span></span>

![](excluding-files-and-folders-from-deployment/_static/image1.png)

<span data-ttu-id="67e43-122">選択すると**このアプリケーションの実行に必要なファイルのみ**、WPP は web のパッケージにどのファイルを追加する必要がありますを決定しようとしています。</span><span class="sxs-lookup"><span data-stu-id="67e43-122">When you choose **Only files needed to run this application**, the WPP will try to determine which files should be added to the web package.</span></span> <span data-ttu-id="67e43-123">バインディングには、以下の項目が含まれます。</span><span class="sxs-lookup"><span data-stu-id="67e43-123">This includes:</span></span>

- <span data-ttu-id="67e43-124">すべてのビルドのプロジェクトを出力します。</span><span class="sxs-lookup"><span data-stu-id="67e43-124">All the build outputs for the project.</span></span>
- <span data-ttu-id="67e43-125">ビルド アクションが付いているすべてのファイル**コンテンツ**です。</span><span class="sxs-lookup"><span data-stu-id="67e43-125">Any files marked with a build action of **Content**.</span></span>

> [!NOTE]
> <span data-ttu-id="67e43-126">含めるファイルを決定するロジックは、このファイルに含まれます。</span><span class="sxs-lookup"><span data-stu-id="67e43-126">The logic that determines which files to include is contained in this file:</span></span>   
> <span data-ttu-id="67e43-127">*%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*</span><span class="sxs-lookup"><span data-stu-id="67e43-127">*%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*</span></span>


## <a name="excluding-specific-files-and-folders"></a><span data-ttu-id="67e43-128">特定のファイルとフォルダーを除外</span><span class="sxs-lookup"><span data-stu-id="67e43-128">Excluding Specific Files and Folders</span></span>

<span data-ttu-id="67e43-129">場合によっては、ファイルとフォルダーを展開する詳細に制御を必要があります。</span><span class="sxs-lookup"><span data-stu-id="67e43-129">In some cases, you'll want more fine-grained control over which files and folders are deployed.</span></span> <span data-ttu-id="67e43-130">個の除外するファイルは時間がわかっており、排他領域はすべての展開先環境に適用されます、単に設定することができます、**ビルド アクション**それぞれのファイルに**None**です。</span><span class="sxs-lookup"><span data-stu-id="67e43-130">If you know which files you want to exclude ahead of time, and the exclusion applies to all destination environments, you can simply set the **Build Action** of each file to **None**.</span></span>

<span data-ttu-id="67e43-131">**特定のファイルを配置から除外するには**</span><span class="sxs-lookup"><span data-stu-id="67e43-131">**To exclude specific files from deployment**</span></span>

1. <span data-ttu-id="67e43-132">**ソリューション エクスプ ローラー**  ウィンドウでは、ファイルを右クリックし、をクリックして**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="67e43-132">In the **Solution Explorer** window, right-click the file, and then click **Properties**.</span></span>
2. <span data-ttu-id="67e43-133">**プロパティ** ウィンドウで、**ビルド アクション**行で、 **None**です。</span><span class="sxs-lookup"><span data-stu-id="67e43-133">In the **Properties** window, in the **Build Action** row, select **None**.</span></span>

<span data-ttu-id="67e43-134">ただし、この方法はありません常に便利です。</span><span class="sxs-lookup"><span data-stu-id="67e43-134">However, this approach is not always convenient.</span></span> <span data-ttu-id="67e43-135">たとえば、ファイルを変更することも、フォルダーに従って、展開先の環境と Visual Studio の外部から付属しています。</span><span class="sxs-lookup"><span data-stu-id="67e43-135">For example, you may want to vary which files and folders are included according to your destination environment, and from outside Visual Studio.</span></span> <span data-ttu-id="67e43-136">たとえば、連絡先のマネージャーのサンプル ソリューションで見て ContactManager.Mvc プロジェクトの内容。</span><span class="sxs-lookup"><span data-stu-id="67e43-136">For example, in the Contact Manager sample solution, take a look at the contents of the ContactManager.Mvc project:</span></span>

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- <span data-ttu-id="67e43-137">内部のフォルダーには、開発者が作成、drop、および開発する目的でローカル データベースへの追加に使用するいくつかの SQL スクリプトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="67e43-137">The Internal folder contains some SQL scripts that the developer uses to create, drop, and populate local databases for development purposes.</span></span> <span data-ttu-id="67e43-138">このフォルダーでは nothing をステージングまたは運用環境に展開されます。</span><span class="sxs-lookup"><span data-stu-id="67e43-138">Nothing in this folder should be deployed to a staging or production environment.</span></span>
- <span data-ttu-id="67e43-139">Scripts フォルダーには、いくつかの JavaScript ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="67e43-139">The Scripts folder contains several JavaScript files.</span></span> <span data-ttu-id="67e43-140">これらのファイルの多くは、デバッグをサポートまたは Visual Studio の IntelliSense を提供する純粋な含まれます。</span><span class="sxs-lookup"><span data-stu-id="67e43-140">A lot of these files are included purely to support debugging or provide IntelliSense in Visual Studio.</span></span> <span data-ttu-id="67e43-141">これらのファイルの一部はステージング環境または実稼働環境に展開されません必要があります。</span><span class="sxs-lookup"><span data-stu-id="67e43-141">Some of these files should not be deployed to staging or production environments.</span></span> <span data-ttu-id="67e43-142">ただし、トラブルシューティングを容易にする開発者テスト環境に配置することがあります。</span><span class="sxs-lookup"><span data-stu-id="67e43-142">However, you may want to deploy them to a developer test environment to facilitate troubleshooting.</span></span>

<span data-ttu-id="67e43-143">特定のファイルとフォルダーを除外するプロジェクト ファイルを操作することが簡単な方法は。</span><span class="sxs-lookup"><span data-stu-id="67e43-143">Although you could manipulate your project files to exclude specific files and folders, there is an easier way.</span></span> <span data-ttu-id="67e43-144">WPP にはという名前の項目リストを構築して、ファイルとフォルダーを除外するためのメカニズムが含まれています**ExcludeFromPackageFolders**と**ExcludeFromPackageFiles**です。</span><span class="sxs-lookup"><span data-stu-id="67e43-144">The WPP includes a mechanism to exclude files and folders by building item lists named **ExcludeFromPackageFolders** and **ExcludeFromPackageFiles**.</span></span> <span data-ttu-id="67e43-145">このメカニズムは、これらのリストに、独自の項目を追加することによって拡張できます。</span><span class="sxs-lookup"><span data-stu-id="67e43-145">You can extend this mechanism by adding your own items to these lists.</span></span> <span data-ttu-id="67e43-146">これを行うには、これらの大まかな手順を完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="67e43-146">To do this, you need to complete these high-level steps:</span></span>

1. <span data-ttu-id="67e43-147">という名前のカスタム プロジェクト ファイルを作成する *[プロジェクト名].wpp.targets*は、プロジェクト ファイルと同じフォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="67e43-147">Create a custom project file named *[project name].wpp.targets* in the same folder as your project file.</span></span>

    > [!NOTE]
    > <span data-ttu-id="67e43-148">*. Wpp.targets*ファイルは、web アプリケーション プロジェクト ファイルと同じフォルダーに移動する必要がある&#x2014;など*ContactManager.Mvc.csproj*&#x2014;任意のカスタムと同じフォルダー内ではなくプロジェクト ファイルを使用してビルドおよび配置プロセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="67e43-148">The *.wpp.targets* file needs to go in the same folder as your web application project file&#x2014;for example, *ContactManager.Mvc.csproj*&#x2014;rather than in the same folder as any custom project files you use to control the build and deployment process.</span></span>
2. <span data-ttu-id="67e43-149">*. Wpp.targets*ファイルに追加し、 **ItemGroup**要素。</span><span class="sxs-lookup"><span data-stu-id="67e43-149">In the *.wpp.targets* file, add an **ItemGroup** element.</span></span>
3. <span data-ttu-id="67e43-150">**ItemGroup**要素を追加**ExcludeFromPackageFolders**と**ExcludeFromPackageFiles**特定のファイルおよび必要に応じてフォルダーを除外する項目。</span><span class="sxs-lookup"><span data-stu-id="67e43-150">In the **ItemGroup** element, add **ExcludeFromPackageFolders** and **ExcludeFromPackageFiles** items to exclude specific files and folders as required.</span></span>

<span data-ttu-id="67e43-151">これは、この基本的な構造 *. wpp.targets*ファイル。</span><span class="sxs-lookup"><span data-stu-id="67e43-151">This is the basic structure of this *.wpp.targets* file:</span></span>


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


<span data-ttu-id="67e43-152">各項目にという名前の項目メタデータ要素が含まれているメモ**FromTarget**です。</span><span class="sxs-lookup"><span data-stu-id="67e43-152">Note that each item includes an item metadata element named **FromTarget**.</span></span> <span data-ttu-id="67e43-153">これは、オプションの値であり、ビルド プロセスには影響しません単に特定のファイルやフォルダーを省略した理由を示すために機能している場合は、ビルド ログをレビュー誰か。</span><span class="sxs-lookup"><span data-stu-id="67e43-153">This is an optional value that doesn't affect the build process; it simply serves to indicate why particular files or folders were omitted if someone reviews the build logs.</span></span>

## <a name="excluding-files-and-folders-from-a-web-package"></a><span data-ttu-id="67e43-154">Web パッケージからファイルとフォルダーを除外</span><span class="sxs-lookup"><span data-stu-id="67e43-154">Excluding Files and Folders from a Web Package</span></span>

<span data-ttu-id="67e43-155">次の手順は、追加する方法を示します、 *. wpp.targets*ファイルを web アプリケーション プロジェクトとプロジェクトをビルドするときに、web パッケージから特定のファイルとフォルダーを除外するファイルを使用する方法です。</span><span class="sxs-lookup"><span data-stu-id="67e43-155">The next procedure shows you how to add a *.wpp.targets* file to a web application project and how to use the file to exclude specific files and folders from the web package when you build your project.</span></span>

<span data-ttu-id="67e43-156">**Web 配置パッケージからファイルとフォルダーを除外するには**</span><span class="sxs-lookup"><span data-stu-id="67e43-156">**To exclude files and folders from a web deployment package**</span></span>

1. <span data-ttu-id="67e43-157">Visual Studio 2010 でソリューションを開きます。</span><span class="sxs-lookup"><span data-stu-id="67e43-157">Open your solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="67e43-158">**ソリューション エクスプ ローラー**ウィンドウで、web アプリケーションのプロジェクト ノードを右クリックし (たとえば、 **ContactManager.Mvc**)、 をポイント**追加**をクリックして**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="67e43-158">In the **Solution Explorer** window, right-click your web application project node (for example, **ContactManager.Mvc**), point to **Add**, and then click **New Item**.</span></span>
3. <span data-ttu-id="67e43-159">**新しい項目の追加**ダイアログ ボックスで、 **XML ファイル**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="67e43-159">In the **Add New Item** dialog box, select the **XML File** template.</span></span>
4. <span data-ttu-id="67e43-160">**名前**ボックスに、入力 *[プロジェクト名] * * *.wpp.targets** (たとえば、 **ContactManager.Mvc.wpp.targets**)、をクリックして**追加**.</span><span class="sxs-lookup"><span data-stu-id="67e43-160">In the **Name** box, type *[project name]***.wpp.targets** (for example, **ContactManager.Mvc.wpp.targets**), and then click **Add**.</span></span>

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > <span data-ttu-id="67e43-161">プロジェクトのルート ノードに新しい項目を追加する場合は、プロジェクト ファイルと同じフォルダーにファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="67e43-161">If you add a new item to the root node of a project, the file is created in the same folder as the project file.</span></span> <span data-ttu-id="67e43-162">これは、Windows エクスプ ローラーでフォルダーを開くことで確認できます。</span><span class="sxs-lookup"><span data-stu-id="67e43-162">You can verify this by opening the folder in Windows Explorer.</span></span>
5. <span data-ttu-id="67e43-163">ファイルの追加、**プロジェクト**要素および**ItemGroup**要素。</span><span class="sxs-lookup"><span data-stu-id="67e43-163">In the file, add a **Project** element and an **ItemGroup** element:</span></span>

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. <span data-ttu-id="67e43-164">Web パッケージからフォルダーを除外する場合は、追加、 **ExcludeFromPackageFolders**要素を**ItemGroup**要素。</span><span class="sxs-lookup"><span data-stu-id="67e43-164">If you want to exclude folders from the web package, add an **ExcludeFromPackageFolders** element to the **ItemGroup** element:</span></span>

   1. <span data-ttu-id="67e43-165">**Include**属性を除外するフォルダーのリストをセミコロンで区切って指定します。</span><span class="sxs-lookup"><span data-stu-id="67e43-165">In the **Include** attribute, provide a semicolon-separated list of the folders you want to exclude.</span></span>
   2. <span data-ttu-id="67e43-166">**FromTarget**メタデータ要素の名のように、フォルダーが除外されている理由を示すために意味のある値を指定して、 *. wpp.targets*ファイル。</span><span class="sxs-lookup"><span data-stu-id="67e43-166">In the **FromTarget** metadata element, provide a meaningful value to indicate why the folders are being excluded, like the name of the *.wpp.targets* file.</span></span>

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. <span data-ttu-id="67e43-167">Web パッケージからファイルを除外する場合は、追加、 **ExcludeFromPackageFiles**要素を**ItemGroup**要素。</span><span class="sxs-lookup"><span data-stu-id="67e43-167">If you want to exclude files from the web package, add an **ExcludeFromPackageFiles** element to the **ItemGroup** element:</span></span>

   1. <span data-ttu-id="67e43-168">**Include**属性を除外するファイルのリストをセミコロンで区切って指定します。</span><span class="sxs-lookup"><span data-stu-id="67e43-168">In the **Include** attribute, provide a semicolon-separated list of the files you want to exclude.</span></span>
   2. <span data-ttu-id="67e43-169">**FromTarget**メタデータ要素理由ファイル除外されているなどの名前を示すために意味のある値を指定して、 *. wpp.targets*ファイル。</span><span class="sxs-lookup"><span data-stu-id="67e43-169">In the **FromTarget** metadata element, provide a meaningful value to indicate why the files are being excluded, like the name of the *.wpp.targets* file.</span></span>

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. <span data-ttu-id="67e43-170">*[プロジェクト名].wpp.targets*ファイルのこのようになります。</span><span class="sxs-lookup"><span data-stu-id="67e43-170">The *[project name].wpp.targets* file should now resemble this:</span></span>

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. <span data-ttu-id="67e43-171">保存して閉じます、 *[プロジェクト名].wpp.targets*ファイル。</span><span class="sxs-lookup"><span data-stu-id="67e43-171">Save and close the *[project name].wpp.targets* file.</span></span>

<span data-ttu-id="67e43-172">次回のビルドとパッケージを web アプリケーション プロジェクト、WPP は自動的に検出、 *. wpp.targets*ファイル。</span><span class="sxs-lookup"><span data-stu-id="67e43-172">The next time you build and package your web application project, the WPP will automatically detect the *.wpp.targets* file.</span></span> <span data-ttu-id="67e43-173">そのファイルと指定したフォルダーは、web のパッケージに含まれません。</span><span class="sxs-lookup"><span data-stu-id="67e43-173">Any files and folders you specified will not be included in the web package.</span></span>

## <a name="conclusion"></a><span data-ttu-id="67e43-174">まとめ</span><span class="sxs-lookup"><span data-stu-id="67e43-174">Conclusion</span></span>

<span data-ttu-id="67e43-175">このトピックには、カスタムを作成することで、web のパッケージをビルドするときに、特定のファイルとフォルダーを除外する方法が説明されている *. wpp.targets* web アプリケーション プロジェクト ファイルと同じフォルダー内のファイルです。</span><span class="sxs-lookup"><span data-stu-id="67e43-175">This topic described how to exclude specific files and folders when you build a web package, by creating a custom *.wpp.targets* file in the same folder as your web application project file.</span></span>

## <a name="further-reading"></a><span data-ttu-id="67e43-176">関連項目</span><span class="sxs-lookup"><span data-stu-id="67e43-176">Further Reading</span></span>

<span data-ttu-id="67e43-177">カスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して、展開プロセスを制御する方法については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。</span><span class="sxs-lookup"><span data-stu-id="67e43-177">For more information on using custom Microsoft Build Engine (MSBuild) project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="67e43-178">パッケージと展開プロセスの詳細については、次を参照してください[パッケージ Web アプリケーション プロジェクトのビルドと](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)、 [Web パッケージの展開の構成パラメーター](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)、および[。Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)です。</span><span class="sxs-lookup"><span data-stu-id="67e43-178">For more information on the packaging and deployment process, see [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Configuring Parameters for Web Package Deployment](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), and [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="67e43-179">[前へ](deploying-membership-databases-to-enterprise-environments.md)
> [次へ](taking-web-applications-offline-with-web-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="67e43-179">[Previous](deploying-membership-databases-to-enterprise-environments.md)
[Next](taking-web-applications-offline-with-web-deploy.md)</span></span>
