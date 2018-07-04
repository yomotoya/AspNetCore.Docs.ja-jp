---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: 展開からのファイルとフォルダーの除外 |Microsoft Docs
author: jrjlee
description: このトピックでは、方法できますから除外するファイルとフォルダー web 配置パッケージをビルドして、web アプリケーション プロジェクトをパッケージ化について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: c50352d423f41f84677dbf048e74088214340f3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382583"
---
<a name="excluding-files-and-folders-from-deployment"></a><span data-ttu-id="00191-103">展開からのファイルとフォルダーの除外</span><span class="sxs-lookup"><span data-stu-id="00191-103">Excluding Files and Folders from Deployment</span></span>
====================
<span data-ttu-id="00191-104">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="00191-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="00191-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="00191-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="00191-106">このトピックでは、方法できますから除外するファイルとフォルダー web 配置パッケージをビルドして、web アプリケーション プロジェクトをパッケージ化について説明します。</span><span class="sxs-lookup"><span data-stu-id="00191-106">This topic describes how you can exclude files and folders from a web deployment package when you build and package a web application project.</span></span>


<span data-ttu-id="00191-107">このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="00191-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="00191-108">これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。</span><span class="sxs-lookup"><span data-stu-id="00191-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="00191-109">ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="00191-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="overview"></a><span data-ttu-id="00191-110">概要</span><span class="sxs-lookup"><span data-stu-id="00191-110">Overview</span></span>

<span data-ttu-id="00191-111">Visual Studio 2010 で web アプリケーション プロジェクトをビルドするときに Web 発行パイプライン (WPP) では、配置可能な web のパッケージにコンパイルされた web アプリケーションをパッケージ化して、このビルド プロセスを拡張することができます。</span><span class="sxs-lookup"><span data-stu-id="00191-111">When you build a web application project in Visual Studio 2010, the Web Publishing Pipeline (WPP) lets you extend this build process by packaging your compiled web application into a deployable web package.</span></span> <span data-ttu-id="00191-112">この web パッケージを展開するリモートの IIS web サーバーにインターネット インフォメーション サービス (IIS) の Web 配置ツール (Web 配置) を使用したり、IIS マネージャーで手動で web パッケージをインポートできます。</span><span class="sxs-lookup"><span data-stu-id="00191-112">You can then use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy this web package to a remote IIS web server, or import the web package manually through IIS Manager.</span></span> <span data-ttu-id="00191-113">このパッケージ化プロセスが説明した[のビルドとパッケージ化 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)します。</span><span class="sxs-lookup"><span data-stu-id="00191-113">This packaging process is explained in [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span>

<span data-ttu-id="00191-114">Web の内容を取得制御する方法をパッケージ化しますか。</span><span class="sxs-lookup"><span data-stu-id="00191-114">So how do you control what gets included in your web package?</span></span> <span data-ttu-id="00191-115">基になるプロジェクト ファイルを使って Visual Studio でプロジェクトの設定は、多くのシナリオのための十分な制御を提供します。</span><span class="sxs-lookup"><span data-stu-id="00191-115">The project settings in Visual Studio, through the underlying project file, provide sufficient control for a lot of scenarios.</span></span> <span data-ttu-id="00191-116">しかし、場合によっては、特定の送信先の環境に web パッケージの内容を調整する場合があります。</span><span class="sxs-lookup"><span data-stu-id="00191-116">However, in some cases you may want to tailor the contents of your web package to specific destination environments.</span></span> <span data-ttu-id="00191-117">たとえば、テスト環境にアプリケーションのデプロイがステージング環境または運用環境にアプリケーションをデプロイするときに、フォルダーを除外する場合にログ ファイル用のフォルダーを追加する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="00191-117">For example, you might want to include a folder for log files when you deploy your application to a test environment but exclude the folder when you deploy the application to a staging or production environment.</span></span> <span data-ttu-id="00191-118">このトピックでは、これを行う方法を示します。</span><span class="sxs-lookup"><span data-stu-id="00191-118">This topic will show you how to do this.</span></span>

## <a name="what-gets-included-by-default"></a><span data-ttu-id="00191-119">既定で内容を取得しますか。</span><span class="sxs-lookup"><span data-stu-id="00191-119">What Gets Included by Default?</span></span>

<span data-ttu-id="00191-120">Visual Studio で web アプリケーション プロジェクトのプロパティを構成するときに、**配置する項目**ボックスの一覧、**パッケージ化/発行 Web**ページでは、web 配置に含める対象を指定できます。パッケージです。</span><span class="sxs-lookup"><span data-stu-id="00191-120">When you configure your web application project properties in Visual Studio, the **Items to deploy** list on the **Package/Publish Web** page lets you specify what you want to include in your web deployment package.</span></span> <span data-ttu-id="00191-121">既定では、この設定は**このアプリケーションの実行に必要なファイルのみ**します。</span><span class="sxs-lookup"><span data-stu-id="00191-121">By default, this is set to **Only files needed to run this application**.</span></span>

![](excluding-files-and-folders-from-deployment/_static/image1.png)

<span data-ttu-id="00191-122">選択すると**このアプリケーションの実行に必要なファイルのみ**、WPP は web のパッケージにファイルを追加する必要がありますを決定しようとしています。</span><span class="sxs-lookup"><span data-stu-id="00191-122">When you choose **Only files needed to run this application**, the WPP will try to determine which files should be added to the web package.</span></span> <span data-ttu-id="00191-123">バインディングには、以下の項目が含まれます。</span><span class="sxs-lookup"><span data-stu-id="00191-123">This includes:</span></span>

- <span data-ttu-id="00191-124">プロジェクトのすべてのビルドを出力します。</span><span class="sxs-lookup"><span data-stu-id="00191-124">All the build outputs for the project.</span></span>
- <span data-ttu-id="00191-125">すべてのファイルがビルド アクションでマークされた**コンテンツ**します。</span><span class="sxs-lookup"><span data-stu-id="00191-125">Any files marked with a build action of **Content**.</span></span>

> [!NOTE]
> <span data-ttu-id="00191-126">このファイルに含めるファイルを決定するロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="00191-126">The logic that determines which files to include is contained in this file:</span></span>   
> <span data-ttu-id="00191-127">*%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*</span><span class="sxs-lookup"><span data-stu-id="00191-127">*%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*</span></span>


## <a name="excluding-specific-files-and-folders"></a><span data-ttu-id="00191-128">特定のファイルとフォルダーを除外</span><span class="sxs-lookup"><span data-stu-id="00191-128">Excluding Specific Files and Folders</span></span>

<span data-ttu-id="00191-129">場合によっては、細かい制御ファイルとフォルダーを展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="00191-129">In some cases, you'll want more fine-grained control over which files and folders are deployed.</span></span> <span data-ttu-id="00191-130">も先に除外するファイルは、次の時間、さんと除外は、すべての展開先環境に適用されます、単に設定することができる場合、**ビルド アクション**それぞれのファイルに**None**します。</span><span class="sxs-lookup"><span data-stu-id="00191-130">If you know which files you want to exclude ahead of time, and the exclusion applies to all destination environments, you can simply set the **Build Action** of each file to **None**.</span></span>

<span data-ttu-id="00191-131">**特定のファイルを配置から除外するには**</span><span class="sxs-lookup"><span data-stu-id="00191-131">**To exclude specific files from deployment**</span></span>

1. <span data-ttu-id="00191-132">**ソリューション エクスプ ローラー**ウィンドウでは、ファイルを右クリックし、順にクリックします**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="00191-132">In the **Solution Explorer** window, right-click the file, and then click **Properties**.</span></span>
2. <span data-ttu-id="00191-133">**プロパティ**ウィンドウで、**ビルド アクション**行で、 **None**します。</span><span class="sxs-lookup"><span data-stu-id="00191-133">In the **Properties** window, in the **Build Action** row, select **None**.</span></span>

<span data-ttu-id="00191-134">ただし、このアプローチは常に便利です。</span><span class="sxs-lookup"><span data-stu-id="00191-134">However, this approach is not always convenient.</span></span> <span data-ttu-id="00191-135">たとえばをどのファイルを変更することがあり、フォルダーは、移行先環境に従って、Visual Studio の外部から含まれています。</span><span class="sxs-lookup"><span data-stu-id="00191-135">For example, you may want to vary which files and folders are included according to your destination environment, and from outside Visual Studio.</span></span> <span data-ttu-id="00191-136">たとえば、連絡先マネージャーのサンプル ソリューションでは、ContactManager.Mvc プロジェクトの内容を見てをみましょう。</span><span class="sxs-lookup"><span data-stu-id="00191-136">For example, in the Contact Manager sample solution, take a look at the contents of the ContactManager.Mvc project:</span></span>

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- <span data-ttu-id="00191-137">内部のフォルダーには、開発者は作成、ドロップ、および開発用のローカルのデータベースの設定を使用していくつかの SQL スクリプトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="00191-137">The Internal folder contains some SQL scripts that the developer uses to create, drop, and populate local databases for development purposes.</span></span> <span data-ttu-id="00191-138">このフォルダーではありませんは、ステージング環境または運用環境に展開してください。</span><span class="sxs-lookup"><span data-stu-id="00191-138">Nothing in this folder should be deployed to a staging or production environment.</span></span>
- <span data-ttu-id="00191-139">Scripts フォルダーには、いくつかの JavaScript ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="00191-139">The Scripts folder contains several JavaScript files.</span></span> <span data-ttu-id="00191-140">これらのファイルの多くは、デバッグをサポートまたは Visual Studio の IntelliSense を提供する純粋に含まれています。</span><span class="sxs-lookup"><span data-stu-id="00191-140">A lot of these files are included purely to support debugging or provide IntelliSense in Visual Studio.</span></span> <span data-ttu-id="00191-141">これらのファイルの一部はステージング環境または運用環境に展開されませんする必要があります。</span><span class="sxs-lookup"><span data-stu-id="00191-141">Some of these files should not be deployed to staging or production environments.</span></span> <span data-ttu-id="00191-142">ただし、トラブルシューティングを容易に開発テスト環境に展開したい場合があります。</span><span class="sxs-lookup"><span data-stu-id="00191-142">However, you may want to deploy them to a developer test environment to facilitate troubleshooting.</span></span>

<span data-ttu-id="00191-143">特定のファイルとフォルダーを除外するプロジェクト ファイルを操作することが簡単な方法は。</span><span class="sxs-lookup"><span data-stu-id="00191-143">Although you could manipulate your project files to exclude specific files and folders, there is an easier way.</span></span> <span data-ttu-id="00191-144">WPP にはという名前のアイテムの一覧を作成することにより、ファイルやフォルダーを除外するためのメカニズムが含まれています**ExcludeFromPackageFolders**と**ExcludeFromPackageFiles**します。</span><span class="sxs-lookup"><span data-stu-id="00191-144">The WPP includes a mechanism to exclude files and folders by building item lists named **ExcludeFromPackageFolders** and **ExcludeFromPackageFiles**.</span></span> <span data-ttu-id="00191-145">このメカニズムを拡張するには、これらのリストに、独自の項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="00191-145">You can extend this mechanism by adding your own items to these lists.</span></span> <span data-ttu-id="00191-146">これを行うには、これらの大まかな手順を完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="00191-146">To do this, you need to complete these high-level steps:</span></span>

1. <span data-ttu-id="00191-147">という名前のカスタム プロジェクト ファイルを作成する *[プロジェクト名].wpp.targets*プロジェクト ファイルと同じフォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="00191-147">Create a custom project file named *[project name].wpp.targets* in the same folder as your project file.</span></span>

    > [!NOTE]
    > <span data-ttu-id="00191-148">*. Wpp.targets*ファイルは、web アプリケーション プロジェクト ファイルと同じフォルダーに移動する必要がある&#x2014;など*ContactManager.Mvc.csproj*&#x2014;任意のカスタムと同じフォルダー内ではなくプロジェクト ファイルを使用して、ビルドおよび展開プロセスを制御します。</span><span class="sxs-lookup"><span data-stu-id="00191-148">The *.wpp.targets* file needs to go in the same folder as your web application project file&#x2014;for example, *ContactManager.Mvc.csproj*&#x2014;rather than in the same folder as any custom project files you use to control the build and deployment process.</span></span>
2. <span data-ttu-id="00191-149">*. Wpp.targets*ファイルを追加、 **ItemGroup**要素。</span><span class="sxs-lookup"><span data-stu-id="00191-149">In the *.wpp.targets* file, add an **ItemGroup** element.</span></span>
3. <span data-ttu-id="00191-150">**ItemGroup**要素を追加**ExcludeFromPackageFolders**と**ExcludeFromPackageFiles**特定のファイルと必要なフォルダーを除外する項目。</span><span class="sxs-lookup"><span data-stu-id="00191-150">In the **ItemGroup** element, add **ExcludeFromPackageFolders** and **ExcludeFromPackageFiles** items to exclude specific files and folders as required.</span></span>

<span data-ttu-id="00191-151">これは、この基本的な構造 *. wpp.targets*ファイル。</span><span class="sxs-lookup"><span data-stu-id="00191-151">This is the basic structure of this *.wpp.targets* file:</span></span>


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


<span data-ttu-id="00191-152">各項目がという名前の項目メタデータ要素が含まれることに注意してください**FromTarget**します。</span><span class="sxs-lookup"><span data-stu-id="00191-152">Note that each item includes an item metadata element named **FromTarget**.</span></span> <span data-ttu-id="00191-153">これは、任意の値であり、ビルド プロセスには影響しません特定のファイルまたはフォルダーが省略された理由を示すためにだけ機能する場合、ユーザーが、ビルド ログをレビューします。</span><span class="sxs-lookup"><span data-stu-id="00191-153">This is an optional value that doesn't affect the build process; it simply serves to indicate why particular files or folders were omitted if someone reviews the build logs.</span></span>

## <a name="excluding-files-and-folders-from-a-web-package"></a><span data-ttu-id="00191-154">Web パッケージからのファイルとフォルダーの除外</span><span class="sxs-lookup"><span data-stu-id="00191-154">Excluding Files and Folders from a Web Package</span></span>

<span data-ttu-id="00191-155">次の手順は、追加する方法を示します、 *. wpp.targets*ファイル、web アプリケーション プロジェクトとプロジェクトをビルドするときに、web パッケージから特定のファイルとフォルダーを除外するファイルを使用する方法。</span><span class="sxs-lookup"><span data-stu-id="00191-155">The next procedure shows you how to add a *.wpp.targets* file to a web application project and how to use the file to exclude specific files and folders from the web package when you build your project.</span></span>

<span data-ttu-id="00191-156">**Web 配置パッケージからファイルとフォルダーを除外するには**</span><span class="sxs-lookup"><span data-stu-id="00191-156">**To exclude files and folders from a web deployment package**</span></span>

1. <span data-ttu-id="00191-157">Visual Studio 2010 でソリューションを開きます。</span><span class="sxs-lookup"><span data-stu-id="00191-157">Open your solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="00191-158">**ソリューション エクスプ ローラー**ウィンドウで、web アプリケーションのプロジェクト ノードを右クリックして (たとえば、 **ContactManager.Mvc**)、 をポイント**追加**をクリックして**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="00191-158">In the **Solution Explorer** window, right-click your web application project node (for example, **ContactManager.Mvc**), point to **Add**, and then click **New Item**.</span></span>
3. <span data-ttu-id="00191-159">**新しい項目の追加**ダイアログ ボックスで、 **XML ファイル**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="00191-159">In the **Add New Item** dialog box, select the **XML File** template.</span></span>
4. <span data-ttu-id="00191-160">**名前**ボックスに「 *[プロジェクト名] * * *.wpp.targets** (たとえば、 **ContactManager.Mvc.wpp.targets**) 順にクリックします**追加**.</span><span class="sxs-lookup"><span data-stu-id="00191-160">In the **Name** box, type *[project name]***.wpp.targets** (for example, **ContactManager.Mvc.wpp.targets**), and then click **Add**.</span></span>

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > <span data-ttu-id="00191-161">プロジェクトのルート ノードに新しい項目を追加する場合は、プロジェクト ファイルと同じフォルダーにファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="00191-161">If you add a new item to the root node of a project, the file is created in the same folder as the project file.</span></span> <span data-ttu-id="00191-162">これは、Windows エクスプ ローラーでフォルダーを開くことで確認できます。</span><span class="sxs-lookup"><span data-stu-id="00191-162">You can verify this by opening the folder in Windows Explorer.</span></span>
5. <span data-ttu-id="00191-163">ファイルで、追加、**プロジェクト**要素と**ItemGroup**要素。</span><span class="sxs-lookup"><span data-stu-id="00191-163">In the file, add a **Project** element and an **ItemGroup** element:</span></span>

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. <span data-ttu-id="00191-164">Web パッケージからフォルダーを除外する場合は、追加、 **ExcludeFromPackageFolders**要素を**ItemGroup**要素。</span><span class="sxs-lookup"><span data-stu-id="00191-164">If you want to exclude folders from the web package, add an **ExcludeFromPackageFolders** element to the **ItemGroup** element:</span></span>

   1. <span data-ttu-id="00191-165">**Include**属性で、除外するフォルダーのセミコロン区切りの一覧を指定します。</span><span class="sxs-lookup"><span data-stu-id="00191-165">In the **Include** attribute, provide a semicolon-separated list of the folders you want to exclude.</span></span>
   2. <span data-ttu-id="00191-166">**FromTarget**メタデータ要素名などののフォルダーが除外されている理由を示す意味のある値を指定、 *. wpp.targets*ファイル。</span><span class="sxs-lookup"><span data-stu-id="00191-166">In the **FromTarget** metadata element, provide a meaningful value to indicate why the folders are being excluded, like the name of the *.wpp.targets* file.</span></span>

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. <span data-ttu-id="00191-167">Web パッケージからファイルを除外する場合は、追加、 **ExcludeFromPackageFiles**要素を**ItemGroup**要素。</span><span class="sxs-lookup"><span data-stu-id="00191-167">If you want to exclude files from the web package, add an **ExcludeFromPackageFiles** element to the **ItemGroup** element:</span></span>

   1. <span data-ttu-id="00191-168">**Include**属性で、除外するファイルのセミコロン区切りの一覧を指定します。</span><span class="sxs-lookup"><span data-stu-id="00191-168">In the **Include** attribute, provide a semicolon-separated list of the files you want to exclude.</span></span>
   2. <span data-ttu-id="00191-169">**FromTarget**メタデータ要素を意味のある理由のファイル除外されているなどの名前を示す値を指定、 *. wpp.targets*ファイル。</span><span class="sxs-lookup"><span data-stu-id="00191-169">In the **FromTarget** metadata element, provide a meaningful value to indicate why the files are being excluded, like the name of the *.wpp.targets* file.</span></span>

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. <span data-ttu-id="00191-170">*[プロジェクト名].wpp.targets*ファイルのこのようになります。</span><span class="sxs-lookup"><span data-stu-id="00191-170">The *[project name].wpp.targets* file should now resemble this:</span></span>

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. <span data-ttu-id="00191-171">保存して閉じます、 *[プロジェクト名].wpp.targets*ファイル。</span><span class="sxs-lookup"><span data-stu-id="00191-171">Save and close the *[project name].wpp.targets* file.</span></span>

<span data-ttu-id="00191-172">次回ビルドとパッケージ、web アプリケーション プロジェクト、WPP を自動的に検出、 *. wpp.targets*ファイル。</span><span class="sxs-lookup"><span data-stu-id="00191-172">The next time you build and package your web application project, the WPP will automatically detect the *.wpp.targets* file.</span></span> <span data-ttu-id="00191-173">そのファイルと指定したフォルダーは、web パッケージに含まれません。</span><span class="sxs-lookup"><span data-stu-id="00191-173">Any files and folders you specified will not be included in the web package.</span></span>

## <a name="conclusion"></a><span data-ttu-id="00191-174">まとめ</span><span class="sxs-lookup"><span data-stu-id="00191-174">Conclusion</span></span>

<span data-ttu-id="00191-175">このトピックでは、カスタムを作成して、web パッケージをビルドするときに、特定のファイルとフォルダーを除外する方法を説明しました。 *。 wpp.targets* web アプリケーション プロジェクト ファイルと同じフォルダー内のファイル。</span><span class="sxs-lookup"><span data-stu-id="00191-175">This topic described how to exclude specific files and folders when you build a web package, by creating a custom *.wpp.targets* file in the same folder as your web application project file.</span></span>

## <a name="further-reading"></a><span data-ttu-id="00191-176">関連項目</span><span class="sxs-lookup"><span data-stu-id="00191-176">Further Reading</span></span>

<span data-ttu-id="00191-177">カスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して、展開プロセスを制御する詳細については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。</span><span class="sxs-lookup"><span data-stu-id="00191-177">For more information on using custom Microsoft Build Engine (MSBuild) project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="00191-178">パッケージ化と展開プロセスの詳細については、次を参照してください[のビルドとパッケージ化 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)、 [Web パッケージ展開の構成パラメーター](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)、および[。Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)します。</span><span class="sxs-lookup"><span data-stu-id="00191-178">For more information on the packaging and deployment process, see [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Configuring Parameters for Web Package Deployment](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), and [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="00191-179">[前へ](deploying-membership-databases-to-enterprise-environments.md)
> [次へ](taking-web-applications-offline-with-web-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="00191-179">[Previous](deploying-membership-databases-to-enterprise-environments.md)
[Next](taking-web-applications-offline-with-web-deploy.md)</span></span>
