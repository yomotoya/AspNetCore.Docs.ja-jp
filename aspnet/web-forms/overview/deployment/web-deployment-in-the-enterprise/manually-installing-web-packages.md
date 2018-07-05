---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Web パッケージを手動でインストールする |Microsoft Docs
author: jrjlee
description: このトピックでは、インターネット インフォメーション サービス (IIS) に web 配置パッケージを手動でインポートする方法について説明します。 トピックの構築と Web アプリケーションをパッケージ化しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: d2b2e4852d01f62feef40f8b15252737327ec4ec
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386628"
---
<a name="manually-installing-web-packages"></a><span data-ttu-id="e0a2f-104">Web パッケージを手動でインストールします。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-104">Manually Installing Web Packages</span></span>
====================
<span data-ttu-id="e0a2f-105">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="e0a2f-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="e0a2f-106">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="e0a2f-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="e0a2f-107">このトピックでは、インターネット インフォメーション サービス (IIS) に web 配置パッケージを手動でインポートする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-107">This topic describes how to manually import a web deployment package into Internet Information Services (IIS).</span></span>
> 
> <span data-ttu-id="e0a2f-108">トピック[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)説明されているパッケージ化する、IIS Web 配置ツール (Web 配置)、Microsoft Build Engine (MSBuild) と、Web 発行パイプライン (WPP) と組み合わせて使用する方法、1 つの zip ファイルに web アプリケーション プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-108">The topic [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md) described how the IIS Web Deployment Tool (Web Deploy), in conjunction with the Microsoft Build Engine (MSBuild) and the Web Publishing Pipeline (WPP), lets you package your web application projects into a single zip file.</span></span> <span data-ttu-id="e0a2f-109">このファイルには、web 配置パッケージ (または、展開パッケージだけです) と呼ばれる一般的には、IIS が web サーバーに web アプリケーションを再作成するために必要なすべてのコンテンツと構成情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-109">This file, commonly known as a web deployment package (or simply a deployment package), contains all the content and configuration information that IIS needs in order to re-create your web application on a web server.</span></span>
> 
> <span data-ttu-id="e0a2f-110">Web 配置パッケージを作成したら、さまざまな方法で、IIS サーバーに発行することができます。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-110">Once you've created a web deployment package, you can publish it to an IIS server in various ways.</span></span> <span data-ttu-id="e0a2f-111">多数のシナリオでは、MSBuild、WPP、および Web デプロイを作成し、自動またはシングル ステップのビルドと展開プロセスの一部として web パッケージをリモートでインストールの間の統合ポイントを活用するためにします。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-111">In a lot of scenarios, you'll want to take advantage of the integration points between MSBuild, the WPP, and Web Deploy to create and install web packages remotely as part of an automated or single-step build and deployment process.</span></span> <span data-ttu-id="e0a2f-112">このプロセスについては、「 [Web パッケージを展開する](deploying-web-packages.md)します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-112">This process is described in [Deploying Web Packages](deploying-web-packages.md).</span></span> <span data-ttu-id="e0a2f-113">ただし、このことは限りません。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-113">However, this isn't always possible.</span></span> <span data-ttu-id="e0a2f-114">インターネットに接続する実稼働環境に web アプリケーションをデプロイするとします。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-114">Suppose you want to deploy a web application to an Internet-facing production environment.</span></span> <span data-ttu-id="e0a2f-115">セキュリティ上の理由から、このような運用環境では、境界ネットワーク (DMZ、非武装地帯、およびスクリーン サブネットとも呼ばれます) で、ビルド サーバーから分離したサブネット上のファイアウォールの内側にある可能性が非常に最もです。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-115">For security reasons, such a production environment is at the very least likely to be behind a firewall on a subnet that is separate from the build server, in a perimeter network (also known as DMZ, demilitarized zone, and screened subnet).</span></span> <span data-ttu-id="e0a2f-116">多くの場合で運用環境は、別のドメイン、または物理的に分離されたネットワーク上になります。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-116">In lots of cases, the production environment will be on a separate domain or on a physically isolated network.</span></span>
> 
> <span data-ttu-id="e0a2f-117">これらのシナリオで、ポート、移行先サーバー上に web パッケージと IIS に手動でインポートする唯一の選択肢があります。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-117">In these scenarios, your only option may be to port the web package onto the destination server and manually import it into IIS.</span></span> <span data-ttu-id="e0a2f-118">Web アプリケーションを公開するための非常に効果的な手法ではまだが、このアプローチは、自動化された展開のため、&#x2014;単に、web サーバーに 1 つの zip ファイルをコピーし、ウィザードを使用してインポートする手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-118">Although this approach precludes automated deployment, it's still a highly effective technique for publishing a web application&#x2014;you simply copy a single zip file to your web server and use a wizard to guide you through the import process.</span></span>


<span data-ttu-id="e0a2f-119">このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="e0a2f-120">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="e0a2f-120">Task Overview</span></span>

<span data-ttu-id="e0a2f-121">IIS に web 配置パッケージをインポートするこれらの高度なタスクを完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-121">You'll need to complete these high-level tasks to import a web deployment package into IIS:</span></span>

- <span data-ttu-id="e0a2f-122">MSBuild コマンドライン、チームがビルドまたは Visual Studio 2010 を使用して web 配置パッケージを作成します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-122">Create a web deployment package using the MSBuild command line, Team Build, or Visual Studio 2010.</span></span>
- <span data-ttu-id="e0a2f-123">Web パッケージを移行先の web サーバーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-123">Copy the web package to the destination web server.</span></span>
- <span data-ttu-id="e0a2f-124">IIS マネージャーでアプリケーション パッケージのインポート ウィザードを使用して web パッケージをインストールし、接続文字列とサービスのエンドポイントなどの変数の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-124">Use the Import Application Package Wizard in IIS Manager to install the web package and provide values for variables like connection strings and service endpoints.</span></span>

<span data-ttu-id="e0a2f-125">このトピックでは、これらの手順を実行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-125">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="e0a2f-126">タスクとチュートリアルでは、このトピックでは、web パッケージ、Web デプロイ、および、WPP の背後にある概念を理解している既にことを想定しています。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-126">The tasks and walkthroughs in this topic assume that you're already familiar with the concepts behind web packages, Web Deploy, and the WPP.</span></span> <span data-ttu-id="e0a2f-127">詳細については、次を参照してください。[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-127">For more information, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e0a2f-128">このトピックでと組み合わせて使用最適[Web 配置発行 (オフライン展開) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)、必要なコンポーネントをインストールし、パッケージのインポート用の IIS の web サイトを準備する方法を説明しています。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-128">This topic is best used in conjunction with [Configure a Web Server for Web Deploy Publishing (Offline Deployment)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), which explains how to install the required components and prepare an IIS website for package import.</span></span>


## <a name="create-a-web-deployment-package"></a><span data-ttu-id="e0a2f-129">Web 配置パッケージを作成します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-129">Create a Web Deployment Package</span></span>

<span data-ttu-id="e0a2f-130">最初のタスクでは、デプロイする web アプリケーション プロジェクトの web 配置パッケージを作成します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-130">The first task is to create a web deployment package for the web application project you want to deploy.</span></span> <span data-ttu-id="e0a2f-131">さまざまな方法では、web パッケージを作成できます。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-131">You can create web packages in a variety of ways.</span></span>

<span data-ttu-id="e0a2f-132">**アプローチ 1: Visual Studio でビルド プロセスの一環としてパッケージを作成します。**</span><span class="sxs-lookup"><span data-stu-id="e0a2f-132">**Approach 1: Create a package as part of the build process with Visual Studio**</span></span>

<span data-ttu-id="e0a2f-133">ビルドするたびに web 配置パッケージを作成する web アプリケーション プロジェクトを構成することができます、**パッケージ化/発行 Web**プロジェクトのプロパティ ページのタブ。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-133">You can configure your web application project to create a web deployment package after every build through the **Package/Publish Web** tab on the project property pages.</span></span> <span data-ttu-id="e0a2f-134">このプロセスについては、「[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-134">This process is described in [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>

<span data-ttu-id="e0a2f-135">**方法 2: MSBuild でビルド プロセスの一環としてパッケージを作成します。**</span><span class="sxs-lookup"><span data-stu-id="e0a2f-135">**Approach 2: Create a package as part of the build process with MSBuild**</span></span>

<span data-ttu-id="e0a2f-136">MSBuild を使用して直接、カスタム MSBuild プロジェクト ファイルまたはコマンドラインから web アプリケーション プロジェクトをビルドする場合、ビルド プロセスの一部としての web 配置パッケージを作成を含めることによって、 **DeployOnBuild = true**と**DeployTarget パッケージ =** コマンド内のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-136">If you build your web application project by using MSBuild directly, either through a custom MSBuild project file or from the command line, you can create a web deployment package as part of the build process by including the **DeployOnBuild=true** and **DeployTarget=Package** properties in your command.</span></span> <span data-ttu-id="e0a2f-137">このプロセスについては、「[ビルド プロセスを理解する](understanding-the-build-process.md)します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-137">This process is described in [Understanding the Build Process](understanding-the-build-process.md).</span></span>

<span data-ttu-id="e0a2f-138">**方法 3: Visual Studio で、必要に応じてパッケージを作成します。**</span><span class="sxs-lookup"><span data-stu-id="e0a2f-138">**Approach 3: Create a package on demand in Visual Studio**</span></span>

<span data-ttu-id="e0a2f-139">Visual Studio 2010 でいつでも、web アプリケーション プロジェクトの web 配置パッケージを作成できます。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-139">You can create a web deployment package for a web application project at any time in Visual Studio 2010.</span></span> <span data-ttu-id="e0a2f-140">これを実行する、**ソリューション エクスプ ローラー**ウィンドウでは、web アプリケーション プロジェクトを右クリックし、順にクリックします**展開パッケージのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-140">To do this, in the **Solution Explorer** window, right-click your web application project, and then click **Build Deployment Package**.</span></span>

![](manually-installing-web-packages/_static/image1.png)

<span data-ttu-id="e0a2f-141">**方法 4: コマンドラインからの要求時にパッケージを作成します。**</span><span class="sxs-lookup"><span data-stu-id="e0a2f-141">**Approach 4: Create a package on demand from the command line**</span></span>

<span data-ttu-id="e0a2f-142">コマンドラインから web 配置パッケージを作成するには呼び出すことによって、**パッケージ**MSBuild を使用して、web アプリケーション プロジェクトのターゲット。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-142">You can create a web deployment package from the command line by invoking the **Package** target on your web application project using MSBuild.</span></span> <span data-ttu-id="e0a2f-143">このコマンドのようになります。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-143">The command should resemble this:</span></span>


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


<span data-ttu-id="e0a2f-144">どちらのアプローチを使用して、最終的な結果は同じです。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-144">Whichever approach you use, the end result is the same.</span></span> <span data-ttu-id="e0a2f-145">WPP は、web アプリケーション プロジェクトの出力フォルダー内のさまざまな関連リソースと共にの zip ファイルとして web 配置パッケージを作成します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-145">The WPP creates a web deployment package as a zip file, together with various supporting resources, in the output folder for your web application project.</span></span>

![](manually-installing-web-packages/_static/image2.png)

<span data-ttu-id="e0a2f-146">Web パッケージを手動でインポートすることを計画している、ときに、zip ファイルのみが必要です。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-146">When you're planning to import the web package manually, you require only the zip file.</span></span> <span data-ttu-id="e0a2f-147">ターゲット web サーバーにこのファイルをコピーし、インポート プロセスを開始することができます。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-147">Copy this file to your target web server and you can begin the import process.</span></span>

## <a name="import-a-web-package-into-iis"></a><span data-ttu-id="e0a2f-148">IIS に Web パッケージをインポートします。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-148">Import a Web Package into IIS</span></span>

<span data-ttu-id="e0a2f-149">次の手順を使用して、ローカル ファイル システムから IIS の web サイトに web 配置パッケージをインポートすることができます。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-149">You can use the next procedure to import a web deployment package from the local file system into an IIS website.</span></span> <span data-ttu-id="e0a2f-150">この手順を実行する前にあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-150">Before you perform this procedure, ensure that you have:</span></span>

- <span data-ttu-id="e0a2f-151">Web サーバーに web デプロイ パッケージをコピーします。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-151">Copied the web deployment package to the web server.</span></span>
- <span data-ttu-id="e0a2f-152">アプリケーションをホストする IIS web サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-152">Configured an IIS web server to host your application.</span></span>

<span data-ttu-id="e0a2f-153">Web 展開パッケージをサポートするために、IIS web サーバーの構成の詳細については、次を参照してください。 [Web 配置発行 (オフライン展開) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-153">For more information on configuring an IIS web server to support web deployment packages, see [Configure a Web Server for Web Deploy Publishing (Offline Deployment)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).</span></span>

<span data-ttu-id="e0a2f-154">**IIS マネージャーを使用して web 配置パッケージをインポートするには**</span><span class="sxs-lookup"><span data-stu-id="e0a2f-154">**To import a web deployment package using IIS Manager**</span></span>

1. <span data-ttu-id="e0a2f-155">IIS マネージャーで、**接続**ウィンドウで、IIS web サイトを右クリックし、 をポイント**デプロイ**、 をクリックし、**アプリケーションのインポート**します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-155">In IIS Manager, in the **Connections** pane, right-click your IIS website, point to **Deploy**, and then click **Import Application**.</span></span>

    ![](manually-installing-web-packages/_static/image3.png)
2. <span data-ttu-id="e0a2f-156">アプリケーション パッケージのインポート ウィザードでの**パッケージを選択して** ページで、web デプロイ パッケージの場所を参照してクリックして**次へ**します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-156">In the Import Application Package Wizard, on the **Select the Package** page, browse to the location of your web deployment package, and then click **Next**.</span></span>
3. <span data-ttu-id="e0a2f-157">**パッケージの内容を選択**ページを必要としない、クリックしてコンテンツをオフ**次**。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-157">On the **Select the Contents of the Package** page, clear any content that you don't require, and then click **Next**.</span></span>

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="e0a2f-158">多くの場合に、web 配置パッケージに付属するすべてのものをインポートすることがありますしません。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-158">In a lot of cases, you may not want to import everything that comes with a web deployment package.</span></span> <span data-ttu-id="e0a2f-159">たとえば、関連付けられているデータベースを置き換えるへの Web 配置を許可しない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-159">For example, you may not want to allow Web Deploy to replace the associated database.</span></span>  
    > <span data-ttu-id="e0a2f-160">**アクセス許可を付与**エントリは、アプリケーション プール id が web サイトのコンテンツを格納する物理フォルダーにアクセスできるようにする移行先のファイル システムのアクセス許可を設定します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-160">The **Grant permissions** entries set permissions on the destination file system to ensure that the application pool identity can access the physical folder that stores the website content.</span></span> <span data-ttu-id="e0a2f-161">さらに、匿名認証のユーザーを Multipurpose Internet Mail Extensions (MIME) の種類のファイルを提供するアプリケーションに、フォルダーへのアクセス許可は付与読み取られます。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-161">In addition, the anonymous authentication user is granted read permission to the folder to let the application serve Multipurpose Internet Mail Extensions (MIME) type files.</span></span> <span data-ttu-id="e0a2f-162">場合は、これらのエントリを削除し、アクセス許可を手動で構成します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-162">If you prefer, you can remove these entries and configure permissions manually.</span></span>
4. <span data-ttu-id="e0a2f-163">**アプリケーション パッケージ情報の入力** ページで、要求された情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-163">On the **Enter Application Package Information** page, provide the requested information.</span></span>

    ![](manually-installing-web-packages/_static/image5.png)
5. <span data-ttu-id="e0a2f-164">Web パッケージを作成するときに、WPP は、アプリケーションの構成ファイルを分析し、接続文字列とサービスのエンドポイントなど、任意の変数を検出します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-164">When you create a web package, the WPP analyzes the configuration file for your application and detects any variables, like connection strings and service endpoints.</span></span> <span data-ttu-id="e0a2f-165">この場合、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-165">In this case:</span></span>

    1. <span data-ttu-id="e0a2f-166">**アプリケーション パス**は、アプリケーションをインストールする IIS のパスです。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-166">**Application Path** is the IIS path where you want to install your application.</span></span> <span data-ttu-id="e0a2f-167">この設定は、WPP を作成するすべての展開パッケージに共通です。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-167">This setting is common to all deployment packages that the WPP creates.</span></span>
    2. <span data-ttu-id="e0a2f-168">**サービスのエンドポイント アドレスを ContactService**は、アプリケーションがデプロイされた WCF サービスとの通信に使用するアドレスです。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-168">**ContactService Service Endpoint Address** is the address that the application should use to communicate with the deployed WCF service.</span></span> <span data-ttu-id="e0a2f-169">この設定は、内のエントリには、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-169">This setting corresponds to an entry in the *web.config* file.</span></span>
    3. <span data-ttu-id="e0a2f-170">最初の**接続文字列**設定は、Web Deploy が (この場合、ASP.NET メンバーシップ データベース内) のアプリケーションに関連付けられたデータベースの配置に使用する接続文字列。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-170">The first **Connection String** setting is the connection string that Web Deploy should use to deploy the database associated with the application (in this case an ASP.NET membership database).</span></span> <span data-ttu-id="e0a2f-171">この設定の設定に対応、**パッケージ化/発行 SQL** Visual Studio でのタブ。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-171">This setting corresponds to the setting on the **Package/Publish SQL** tab in Visual Studio.</span></span>
    4. <span data-ttu-id="e0a2f-172">2 番目の**接続文字列**設定は、アプリケーションは実際に稼働している場合、データベースとの通信に使用される接続文字列。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-172">The second **Connection String** setting is the connection string that your application will actually use to communicate with the database when it's up and running.</span></span> <span data-ttu-id="e0a2f-173">これで接続文字列のエントリに対応して、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-173">This corresponds to a connection string entry in the *web.config* file.</span></span>

        > [!NOTE]
        > <span data-ttu-id="e0a2f-174">これらのパラメーター元の場所の詳細については、次を参照してください。 [Web パッケージ展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-174">For more information on where these parameters come from, see [Configuring Parameters for Web Package Deployment](configuring-parameters-for-web-package-deployment.md).</span></span>
6. <span data-ttu-id="e0a2f-175">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-175">Click **Next**.</span></span>
7. <span data-ttu-id="e0a2f-176">初めてこの web サイトにアプリケーションをデプロイした表示しない場合をインストールする前にすべての既存のコンテンツを削除するかどうかを指定する求め。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-176">If this is not the first time you've deployed the application to this website, you'll be prompted to specify whether you want to delete all existing content prior to installation.</span></span> <span data-ttu-id="e0a2f-177">要件に合わせて、適切なオプションを選択し、クリックして**次**します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-177">Choose the option that's appropriate for your requirements, and then click **Next**.</span></span>

    ![](manually-installing-web-packages/_static/image6.png)
8. <span data-ttu-id="e0a2f-178">IIS は、パッケージのインストールが完了したら、クリックして**完了**します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-178">When IIS has finished installing the package, click **Finish**.</span></span>

    ![](manually-installing-web-packages/_static/image7.png)

<span data-ttu-id="e0a2f-179">この時点では、IIS に web アプリケーションを正常に公開しました。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-179">At this point, you've successfully published your web application to IIS.</span></span>

## <a name="conclusion"></a><span data-ttu-id="e0a2f-180">まとめ</span><span class="sxs-lookup"><span data-stu-id="e0a2f-180">Conclusion</span></span>

<span data-ttu-id="e0a2f-181">このトピックでは、IIS マネージャーを使用して IIS の web サイトに web 配置パッケージをインポートする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-181">This topic described how to import a web deployment package into an IIS website using IIS Manager.</span></span> <span data-ttu-id="e0a2f-182">Web アプリケーションの発行には、このアプローチは、セキュリティやインフラストラクチャの制約が不可能または望ましくない、リモート展開を行うときに適しています。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-182">This approach to web application publishing is appropriate when security or infrastructure constraints make remote deployment impossible or undesirable.</span></span>

## <a name="further-reading"></a><span data-ttu-id="e0a2f-183">関連項目</span><span class="sxs-lookup"><span data-stu-id="e0a2f-183">Further Reading</span></span>

<span data-ttu-id="e0a2f-184">Web パッケージを手動でインポートをサポートする IIS web サーバーを構成する方法のガイダンスについては、次を参照してください。 [Web 配置発行 (オフライン展開) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-184">For guidance on how to configure an IIS web server to support manually importing a web package, see [Configure a Web Server for Web Deploy Publishing (Offline Deployment)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).</span></span> <span data-ttu-id="e0a2f-185">Web パッケージの展開の一般的なガイダンスについては、次を参照してください。[チュートリアル: Web 配置パッケージ (4 の第 1 部) を使用した Web アプリケーション プロジェクトを展開](https://msdn.microsoft.com/library/dd483479.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="e0a2f-185">For more general guidance on deploying web packages, see [Walkthrough: Deploying a Web Application Project Using a Web Deployment Package (Part 1 of 4)](https://msdn.microsoft.com/library/dd483479.aspx).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e0a2f-186">前へ</span><span class="sxs-lookup"><span data-stu-id="e0a2f-186">Previous</span></span>](creating-and-running-a-deployment-command-file.md)
