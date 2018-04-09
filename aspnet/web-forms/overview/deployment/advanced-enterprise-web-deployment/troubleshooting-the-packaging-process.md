---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: パッケージ化プロセスのトラブルシューティング |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、M に EnablePackageProcessLoggingAndAssert プロパティを使用して、パッケージ化プロセスに関する詳細情報を収集する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 816ab77c44b52c6449a139475f2ef8546bd38071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="troubleshooting-the-packaging-process"></a><span data-ttu-id="406ce-103">パッケージ化プロセスのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="406ce-103">Troubleshooting the Packaging Process</span></span>
====================
<span data-ttu-id="406ce-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="406ce-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="406ce-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="406ce-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="406ce-106">このトピックを使用して、パッケージ化プロセスに関する詳細情報を収集する方法について説明します、 **EnablePackageProcessLoggingAndAssert** Microsoft Build Engine (MSBuild) 内のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="406ce-106">This topic describes how you can collect detailed information about the packaging process by using the **EnablePackageProcessLoggingAndAssert** property in the Microsoft Build Engine (MSBuild).</span></span>
> 
> <span data-ttu-id="406ce-107">設定すると、 **EnablePackageProcessLoggingAndAssert**プロパティを**true**MSBuild は。</span><span class="sxs-lookup"><span data-stu-id="406ce-107">When you set the **EnablePackageProcessLoggingAndAssert** property to **true**, MSBuild will:</span></span>
> 
> - <span data-ttu-id="406ce-108">ビルド ログをパッケージ化プロセスに関する追加情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="406ce-108">Add additional information about the packaging process to the build logs.</span></span>
> - <span data-ttu-id="406ce-109">パッケージ リストに重複するファイルが見つかった場合は、特定の状況では、エラーをたとえば、ログインします。</span><span class="sxs-lookup"><span data-stu-id="406ce-109">Log errors under certain conditions, for example, if duplicate files are found in the packaging list.</span></span>
> - <span data-ttu-id="406ce-110">ログ ディレクトリを作成、 *ProjectName*\_フォルダーをパッケージ化し、パッケージ化するファイルに関する情報を記録に使用します。</span><span class="sxs-lookup"><span data-stu-id="406ce-110">Create a Log directory in the *ProjectName*\_Package folder and use it to record information about the files you're packaging.</span></span>
> 
> <span data-ttu-id="406ce-111">パッケージ化プロセスが失敗している場合、または web 展開パッケージに必要なファイルが含まれていない、行うこともできますこの情報プロセスおよび pinpoint のトラブルシューティングで作業がうまくいかないです。</span><span class="sxs-lookup"><span data-stu-id="406ce-111">If the packaging process is failing, or your web deployment packages don't contain the files that you expect, you can use this information to troubleshoot the process and pinpoint where things are going wrong.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="406ce-112">**EnablePackageProcessLoggingAndAssert**プロパティは、使用して、プロジェクトをビルドする場合にのみ機能、**デバッグ**構成します。</span><span class="sxs-lookup"><span data-stu-id="406ce-112">The **EnablePackageProcessLoggingAndAssert** property only works if you build your project using the **Debug** configuration.</span></span> <span data-ttu-id="406ce-113">その他の構成では、プロパティは無視されます。</span><span class="sxs-lookup"><span data-stu-id="406ce-113">The property is ignored in other configurations.</span></span>


<span data-ttu-id="406ce-114">このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="406ce-114">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="406ce-115">説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。</span><span class="sxs-lookup"><span data-stu-id="406ce-115">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="406ce-116">ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="406ce-116">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a><span data-ttu-id="406ce-117">EnablePackageProcessLoggingAndAssert プロパティについてください。</span><span class="sxs-lookup"><span data-stu-id="406ce-117">Understanding the EnablePackageProcessLoggingAndAssert Property</span></span>

<span data-ttu-id="406ce-118">[パッケージ Web アプリケーション プロジェクトのビルドと](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)Web 発行パイプライン (WPP) が MSBuild の機能を拡張して、インターネット インフォメーション サービス (IIS) Web と統合できるようにする MSBuild のターゲットのセットを提供する方法の説明配置ツール (Web 配置) します。</span><span class="sxs-lookup"><span data-stu-id="406ce-118">[Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) described how the Web Publishing Pipeline (WPP) provides a set of MSBuild targets that extend the functionality of MSBuild and enable it to integrate with the Internet Information Services (IIS) Web Deployment Tool (Web Deploy).</span></span> <span data-ttu-id="406ce-119">Web アプリケーション プロジェクトをパッケージ化するときは、WPP ターゲットを呼び出しています。</span><span class="sxs-lookup"><span data-stu-id="406ce-119">When you package a web application project, you're invoking WPP targets.</span></span>

<span data-ttu-id="406ce-120">WPP ターゲットの多くが追加情報をログに記録する条件付きロジックを含めるときに、 **EnablePackageProcessLoggingAndAssert**プロパティに設定されている**true**です。</span><span class="sxs-lookup"><span data-stu-id="406ce-120">Lots of these WPP targets include conditional logic that logs additional information when the **EnablePackageProcessLoggingAndAssert** property is set to **true**.</span></span> <span data-ttu-id="406ce-121">たとえば、確認する場合、**パッケージ**ターゲットを確認できます、追加のログ ディレクトリを作成し、場合に、ファイルの一覧をテキスト ファイルに書き込みます**EnablePackageProcessLoggingAndAssert** と等しい**true**です。</span><span class="sxs-lookup"><span data-stu-id="406ce-121">For example, if you review the **Package** target, you can see that it creates an additional log directory and writes a list of files to a text file if **EnablePackageProcessLoggingAndAssert** is equal to **true**.</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> <span data-ttu-id="406ce-122">WPP ターゲットが定義されている、 *Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web フォルダー内のファイルです。</span><span class="sxs-lookup"><span data-stu-id="406ce-122">The WPP targets are defined in the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span> <span data-ttu-id="406ce-123">このファイルを開くし、Visual Studio 2010 または任意の XML エディター内のターゲットを確認できます。</span><span class="sxs-lookup"><span data-stu-id="406ce-123">You can open this file and review the targets in Visual Studio 2010 or any XML editor.</span></span> <span data-ttu-id="406ce-124">ファイルの内容を変更しないようにしてください。</span><span class="sxs-lookup"><span data-stu-id="406ce-124">Take care not to modify the contents of the file.</span></span>


## <a name="enabling-the-additional-logging"></a><span data-ttu-id="406ce-125">追加のログ記録を有効にします。</span><span class="sxs-lookup"><span data-stu-id="406ce-125">Enabling the Additional Logging</span></span>

<span data-ttu-id="406ce-126">値を指定することができます、 **EnablePackageProcessLoggingAndAssert**プロジェクトをビルドする方法に応じて、さまざまな方法でプロパティです。</span><span class="sxs-lookup"><span data-stu-id="406ce-126">You can supply a value for the **EnablePackageProcessLoggingAndAssert** property in various ways, depending on how you build your project.</span></span>

<span data-ttu-id="406ce-127">値を指定するには、コマンドラインからプロジェクトをビルドする場合、 **EnablePackageProcessLoggingAndAssert**コマンドライン引数のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="406ce-127">If you build your project from the command line, you can supply a value for the **EnablePackageProcessLoggingAndAssert** property as a command-line argument:</span></span>


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


<span data-ttu-id="406ce-128">プロジェクトをビルドするカスタム プロジェクト ファイルを使用している場合は、含める、 **EnablePackageProcessLoggingAndAssert**値で、**プロパティ**の属性、 **MSBuild**タスク。</span><span class="sxs-lookup"><span data-stu-id="406ce-128">If you're using a custom project file to build your projects, you can include the **EnablePackageProcessLoggingAndAssert** value in the **Properties** attribute of the **MSBuild** task:</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


<span data-ttu-id="406ce-129">Team Foundation Server (TFS) ビルド定義を使用して、プロジェクトを作成する場合は、値を指定することができます、 **EnablePackageProcessLoggingAndAssert**プロパティに、**の MSBuild 引数**行。![](troubleshooting-the-packaging-process/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="406ce-129">If you're using a Team Foundation Server (TFS) build definition to build your projects, you can supply a value for the **EnablePackageProcessLoggingAndAssert** property in the **MSBuild Arguments** row:![](troubleshooting-the-packaging-process/_static/image1.png)</span></span>

> [!NOTE]
> <span data-ttu-id="406ce-130">作成とビルド定義の構成の詳細については、次を参照してください。[展開を作成するビルド定義をサポートしている](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="406ce-130">For more information on creating and configuring build definitions, see [Creating a Build Definition That Supports Deployment](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).</span></span>


<span data-ttu-id="406ce-131">または、ビルドするたびに、パッケージを含める場合は、設定を web アプリケーション プロジェクトのプロジェクト ファイルを変更できます、 **EnablePackageProcessLoggingAndAssert**プロパティを**true**です。</span><span class="sxs-lookup"><span data-stu-id="406ce-131">Alternatively, if you want to include the package in every build, you can modify the project file for your web application project to set the **EnablePackageProcessLoggingAndAssert** property to **true**.</span></span> <span data-ttu-id="406ce-132">最初にプロパティを追加する必要があります**PropertyGroup** .csproj または .vbproj ファイル内の要素。</span><span class="sxs-lookup"><span data-stu-id="406ce-132">You should add the property to the first **PropertyGroup** element within your .csproj or .vbproj file.</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a><span data-ttu-id="406ce-133">ログ ファイルの確認</span><span class="sxs-lookup"><span data-stu-id="406ce-133">Reviewing the Log Files</span></span>

<span data-ttu-id="406ce-134">ビルドおよび web アプリケーション プロジェクトをパッケージ化する場合**EnablePackageProcessLoggingAndAssert** 'éý' **true**、MSBuild には、名前付きのログ追加フォルダーが作成され、 *ProjectName*\_パッケージ フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="406ce-134">When you build and package a web application project with **EnablePackageProcessLoggingAndAssert** set to **true**, MSBuild creates an additional folder named Log in the *ProjectName*\_Package folder.</span></span> <span data-ttu-id="406ce-135">ログ フォルダーには、さまざまなファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="406ce-135">The Log folder contains various files:</span></span>

![](troubleshooting-the-packaging-process/_static/image2.png)

<span data-ttu-id="406ce-136">表示されているファイルの一覧は、プロジェクトのビルド プロセスで処理によって異なります。</span><span class="sxs-lookup"><span data-stu-id="406ce-136">The list of files that you see will vary according to the things in your project and your build process.</span></span> <span data-ttu-id="406ce-137">ただし、これらのファイルは通常のプロセスのさまざまな段階で、パッケージング、WPP を収集するファイルの一覧を記録する使用されます。</span><span class="sxs-lookup"><span data-stu-id="406ce-137">However, these files are typically used to record the list of files that the WPP is collecting for packaging, at various stages of the process:</span></span>

- <span data-ttu-id="406ce-138">*PreExcludePipelineCollectFilesPhaseFileList.txt*ファイルを除外対象として指定されているファイルを削除する前にパッケージ化 MSBuild を収集するファイルを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="406ce-138">The *PreExcludePipelineCollectFilesPhaseFileList.txt* file lists the files that MSBuild collects for packaging before any files that are specified for exclusion are removed.</span></span>
- <span data-ttu-id="406ce-139">*AfterExcludeFilesFilesList.txt*除外対象として指定されているファイルが削除された後に変更されたファイルの一覧が含まれます。</span><span class="sxs-lookup"><span data-stu-id="406ce-139">The *AfterExcludeFilesFilesList.txt* file contains the modified file list after any files that are specified for exclusion are removed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="406ce-140">パッケージ化プロセスからのファイルとフォルダーを除外する方法の詳細については、次を参照してください。[除外ファイルおよびフォルダーの展開から](excluding-files-and-folders-from-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="406ce-140">For more information on excluding files and folders from the packaging process, see [Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span>
- <span data-ttu-id="406ce-141">*AfterTransformWebConfig.txt*ファイルには、いずれかの後のパッケージ化に収集されたファイルが一覧表示*Web.config*変換が実行されています。</span><span class="sxs-lookup"><span data-stu-id="406ce-141">The *AfterTransformWebConfig.txt* file lists the files collected for packaging after any *Web.config* transforms have been performed.</span></span> <span data-ttu-id="406ce-142">この一覧で、任意の構成固有*Web.config*と同様に、ファイルを変換*web.debug.config となります*と*Web.Release.config*のファイルの一覧から除外されますパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="406ce-142">In this list, any configuration-specific *Web.config* transform files, like *Web.Debug.config* and *Web.Release.config*, are excluded from the list of files for packaging.</span></span> <span data-ttu-id="406ce-143">1 つの変換*Web.config*代わりに含まれています。</span><span class="sxs-lookup"><span data-stu-id="406ce-143">A single transformed *Web.config* is included in their place.</span></span>
- <span data-ttu-id="406ce-144">*PostAutoParameterizationWebConfigConnectionStrings.txt*ファイル内の接続文字列の後にファイルの一覧を含む、 *Web.config*ファイルがパラメーター化されました。</span><span class="sxs-lookup"><span data-stu-id="406ce-144">The *PostAutoParameterizationWebConfigConnectionStrings.txt* file contains the list of files after the connection strings in the *Web.config* file have been parameterized.</span></span> <span data-ttu-id="406ce-145">これは、パッケージを展開するときに、対象となる環境の適切な設定で、接続文字列を置き換えることができます、プロセスです。</span><span class="sxs-lookup"><span data-stu-id="406ce-145">This is the process that lets you replace your connection strings with the right settings for your target environment when you deploy the package.</span></span>
- <span data-ttu-id="406ce-146">*Prepackage.txt*ファイルには、パッケージに含まれるファイルの完成したビルド前の一覧が含まれています。</span><span class="sxs-lookup"><span data-stu-id="406ce-146">The *Prepackage.txt* file contains the finalized pre-build list of files to be included in the package.</span></span>

> [!NOTE]
> <span data-ttu-id="406ce-147">追加のログ ファイルの名前は、通常、WPP ターゲットに対応します。</span><span class="sxs-lookup"><span data-stu-id="406ce-147">The names of the additional log files typically correspond to WPP targets.</span></span> <span data-ttu-id="406ce-148">これらのターゲットを確認するには確認するには、 *Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web フォルダー内のファイルです。</span><span class="sxs-lookup"><span data-stu-id="406ce-148">You can review these targets by examining the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span>


<span data-ttu-id="406ce-149">Web パッケージの内容が期待どおりでないこれらのファイルを確認する、便利な方法、プロセスの処理にどのような点が発生しました。 を特定することができます。</span><span class="sxs-lookup"><span data-stu-id="406ce-149">If the contents of your web package aren't what you expected, reviewing these files can be a useful way to identify at what point in the process things went wrong.</span></span>

## <a name="conclusion"></a><span data-ttu-id="406ce-150">まとめ</span><span class="sxs-lookup"><span data-stu-id="406ce-150">Conclusion</span></span>

<span data-ttu-id="406ce-151">このトピックの説明を使用する方法、 **EnablePackageProcessLoggingAndAssert**パッケージ化処理のトラブルシューティングを MSBuild でのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="406ce-151">This topic described how you can use the **EnablePackageProcessLoggingAndAssert** property in MSBuild to troubleshoot the packaging process.</span></span> <span data-ttu-id="406ce-152">ビルド プロセスにプロパティ値を入力するさまざまな方法が説明されているし、プロパティを設定するときに記録されている追加の情報が記載されている**true**です。</span><span class="sxs-lookup"><span data-stu-id="406ce-152">It explained the different ways in which you can supply the property value to the build process, and it described the additional information that is recorded when you set the property to **true**.</span></span>

## <a name="further-reading"></a><span data-ttu-id="406ce-153">関連項目</span><span class="sxs-lookup"><span data-stu-id="406ce-153">Further Reading</span></span>

<span data-ttu-id="406ce-154">展開プロセスを制御するカスタム MSBuild プロジェクト ファイルを使用する方法については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。</span><span class="sxs-lookup"><span data-stu-id="406ce-154">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="406ce-155">WPP とパッケージ化プロセスの管理方法の詳細については、次を参照してください。[パッケージ Web アプリケーション プロジェクトのビルドと](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)です。</span><span class="sxs-lookup"><span data-stu-id="406ce-155">For more information on the WPP and how it manages the packaging process, see [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span> <span data-ttu-id="406ce-156">Web 配置パッケージから特定のファイルとフォルダーを除外する方法については、次を参照してください。[除外ファイルおよびフォルダーの展開から](excluding-files-and-folders-from-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="406ce-156">For guidance on how to exclude specific files and folders from web deployment packages, see [Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="406ce-157">前へ</span><span class="sxs-lookup"><span data-stu-id="406ce-157">Previous</span></span>](running-windows-powershell-scripts-from-msbuild-project-files.md)
