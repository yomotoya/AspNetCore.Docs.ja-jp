---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Web パッケージ展開のパラメーターの構成 |Microsoft Docs
author: jrjlee
description: このトピックでは、インターネット インフォメーション サービス (IIS) web アプリケーションの名前、接続文字列、およびサービスのエンドポイントなどのパラメーター値を設定する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: e6db7a8351e01bbbc14eb2b993248ee7d5a84f7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386550"
---
<a name="configuring-parameters-for-web-package-deployment"></a><span data-ttu-id="4593c-103">Web パッケージ展開のパラメーターの構成</span><span class="sxs-lookup"><span data-stu-id="4593c-103">Configuring Parameters for Web Package Deployment</span></span>
====================
<span data-ttu-id="4593c-104">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="4593c-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="4593c-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="4593c-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="4593c-106">このトピックでは、リモートの IIS web サーバーに web パッケージを展開するときに、インターネット インフォメーション サービス (IIS) web アプリケーションの名前、接続文字列、およびサービスのエンドポイントなどのパラメーター値を設定する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4593c-106">This topic describes how to set parameter values, like Internet Information Services (IIS) web application names, connection strings, and service endpoints, when you deploy a web package to a remote IIS web server.</span></span>


<span data-ttu-id="4593c-107">Web アプリケーション プロジェクト、ビルドおよびパッケージ化プロセスを作成する場合に、3 つのキー ファイルが生成されます。</span><span class="sxs-lookup"><span data-stu-id="4593c-107">When you build a web application project, the build and packaging process generates three key files:</span></span>

- <span data-ttu-id="4593c-108">A *[プロジェクト名] .zip*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-108">A *[project name].zip* file.</span></span> <span data-ttu-id="4593c-109">これは、web アプリケーション プロジェクトの web 配置パッケージです。</span><span class="sxs-lookup"><span data-stu-id="4593c-109">This is the web deployment package for your web application project.</span></span> <span data-ttu-id="4593c-110">このパッケージには、すべてのアセンブリ、ファイル、データベースのスクリプト、およびリモートの IIS web サーバーに web アプリケーションを再作成に必要なリソースが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4593c-110">This package contains all the assemblies, files, database scripts, and resources required to recreate your web application on a remote IIS web server.</span></span>
- <span data-ttu-id="4593c-111">A *[プロジェクト名].deploy.cmd*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-111">A *[project name].deploy.cmd* file.</span></span> <span data-ttu-id="4593c-112">これには、一連リモートの IIS web サーバーに web 配置パッケージを公開する Web Deploy (MSDeploy.exe) のパラメーター化されたコマンドにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4593c-112">This contains a set of parameterized Web Deploy (MSDeploy.exe) commands that publish your web deployment package to a remote IIS web server.</span></span>
- <span data-ttu-id="4593c-113">A *[プロジェクト名]。SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-113">A *[project name].SetParameters.xml* file.</span></span> <span data-ttu-id="4593c-114">これは、MSDeploy.exe コマンドにパラメーター値のセットを提供します。</span><span class="sxs-lookup"><span data-stu-id="4593c-114">This provides a set of parameter values to the MSDeploy.exe command.</span></span> <span data-ttu-id="4593c-115">このファイル内の値を更新し、Web Deploy をパラメーターとして渡す、コマンド ライン web パッケージを展開するときにできます。</span><span class="sxs-lookup"><span data-stu-id="4593c-115">You can update the values in this file and pass it to Web Deploy as a command-line parameter when you deploy your web package.</span></span>

> [!NOTE]
> <span data-ttu-id="4593c-116">ビルドとパッケージ化プロセスの詳細については、次を参照してください。[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)します。</span><span class="sxs-lookup"><span data-stu-id="4593c-116">For more information on the build and packaging process, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>


<span data-ttu-id="4593c-117">*SetParameters.xml*ファイルは、web アプリケーションのプロジェクト ファイルと、プロジェクト内のすべての構成ファイルから動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="4593c-117">The *SetParameters.xml* file is dynamically generated from your web application project file and any configuration files within your project.</span></span> <span data-ttu-id="4593c-118">ビルドおよび Web 発行パイプライン (WPP)、プロジェクトをパッケージ化する場合、変換先の IIS web アプリケーションのように、デプロイ環境とデータベース接続文字列の間を変更する可能性のある変数の多くが自動的に検出します。</span><span class="sxs-lookup"><span data-stu-id="4593c-118">When you build and package your project, the Web Publishing Pipeline (WPP) will automatically detect lots of the variables that are likely to change between deployment environments, like the destination IIS web application and any database connection strings.</span></span> <span data-ttu-id="4593c-119">これらの値が自動的に web デプロイ パッケージでパラメーター化されたに追加された、 *SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-119">These values are automatically parameterized in the web deployment package and added to the *SetParameters.xml* file.</span></span> <span data-ttu-id="4593c-120">接続文字列を追加する場合など、 *web.config*ファイル、web アプリケーション プロジェクトでビルド プロセスは、この変更を検出およびエントリを追加、 *SetParameters.xml*ファイルそれに応じて。</span><span class="sxs-lookup"><span data-stu-id="4593c-120">For example, if you add a connection string to the *web.config* file in your web application project, the build process will detect this change and will add an entry to the *SetParameters.xml* file accordingly.</span></span>

<span data-ttu-id="4593c-121">多くの場合、この自動パラメーター化は、十分になります。</span><span class="sxs-lookup"><span data-stu-id="4593c-121">In a lot of cases, this automatic parameterization will be sufficient.</span></span> <span data-ttu-id="4593c-122">ただし、ユーザーがアプリケーションやサービスのエンドポイント Url のように、展開環境の間には、その他の設定を変更する必要がある、展開パッケージでこれらの値をパラメーター化し、を対応するエントリを追加するWPPを指示する*SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-122">However, if your users need to vary other settings between deployment environments, like application settings or service endpoint URLs, you need to tell the WPP to parameterize these values in the deployment package and add corresponding entries to the *SetParameters.xml* file.</span></span> <span data-ttu-id="4593c-123">次のセクションでは、これを行う方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4593c-123">The sections that follow explain how to do this.</span></span>

### <a name="automatic-parameterization"></a><span data-ttu-id="4593c-124">自動パラメーター化</span><span class="sxs-lookup"><span data-stu-id="4593c-124">Automatic Parameterization</span></span>

<span data-ttu-id="4593c-125">ビルドし、web アプリケーションをパッケージ化すると、WPP をこれらの操作は自動的にパラメーター化します。</span><span class="sxs-lookup"><span data-stu-id="4593c-125">When you build and package a web application, the WPP will automatically parameterize these things:</span></span>

- <span data-ttu-id="4593c-126">変換先の IIS の web アプリケーションのパスと名前。</span><span class="sxs-lookup"><span data-stu-id="4593c-126">The destination IIS web application path and name.</span></span>
- <span data-ttu-id="4593c-127">すべての接続文字列、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-127">Any connection strings in your *web.config* file.</span></span>
- <span data-ttu-id="4593c-128">追加するすべてのデータベースの接続文字列、**パッケージ化/発行 SQL**プロジェクトのプロパティ ページ タブ。</span><span class="sxs-lookup"><span data-stu-id="4593c-128">Connection strings for any databases you add to the **Package/Publish SQL** tab in the project property pages.</span></span>

<span data-ttu-id="4593c-129">たとえば、ビルドおよびパッケージ化した場合、 [Contact Manager](the-contact-manager-solution.md) WPP、何らかの方法でパラメーター化のプロセスに触れることがなく、サンプル ソリューションでは、これが生成*ContactManager.Mvc.SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-129">For example, if you were to build and package the [Contact Manager](the-contact-manager-solution.md) sample solution without touching the parameterization process in any way, the WPP would generate this *ContactManager.Mvc.SetParameters.xml* file:</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


<span data-ttu-id="4593c-130">この場合、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="4593c-130">In this case:</span></span>

- <span data-ttu-id="4593c-131">**IIS Web アプリケーション名**パラメーターは、web アプリケーションを展開する IIS のパス。</span><span class="sxs-lookup"><span data-stu-id="4593c-131">The **IIS Web Application Name** parameter is the IIS path where you want to deploy the web application.</span></span> <span data-ttu-id="4593c-132">既定値を取得、**パッケージ化/発行 Web**プロジェクトのプロパティ ページでページ。</span><span class="sxs-lookup"><span data-stu-id="4593c-132">The default value is taken from the **Package/Publish Web** page in the project property pages.</span></span>
- <span data-ttu-id="4593c-133">**ApplicationServices-web.config Connection String**から生成されたパラメーターを**connectionStrings/追加**内の要素、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-133">The **ApplicationServices-Web.config Connection String** parameter was generated from a **connectionStrings/add** element in the *web.config* file.</span></span> <span data-ttu-id="4593c-134">これは、メンバーシップ データベースに接続するため、アプリケーションが使用する接続文字列を表します。</span><span class="sxs-lookup"><span data-stu-id="4593c-134">It represents the connection string that the application should use to contact the membership database.</span></span> <span data-ttu-id="4593c-135">ここでされますを指定した値に代入され、デプロイされている*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-135">The value you provide here will be substituted into the deployed *web.config* file.</span></span> <span data-ttu-id="4593c-136">配置前から既定値を取得*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-136">The default value is taken from the pre-deployment *web.config* file.</span></span>

<span data-ttu-id="4593c-137">WPP には、これらのプロパティを生成する展開パッケージにもパラメーター化します。</span><span class="sxs-lookup"><span data-stu-id="4593c-137">The WPP also parameterizes these properties in the deployment package it generates.</span></span> <span data-ttu-id="4593c-138">展開パッケージをインストールするときに、これらのプロパティの値を指定できます。</span><span class="sxs-lookup"><span data-stu-id="4593c-138">You can provide values for these properties when you install the deployment package.</span></span> <span data-ttu-id="4593c-139">」の説明に従って手動で IIS マネージャーから、パッケージをインストールするかどうか[Web パッケージを手動でインストール](manually-installing-web-packages.md)、インストール ウィザードでは、すべてのパラメーターの値を指定するように求められます。</span><span class="sxs-lookup"><span data-stu-id="4593c-139">If you install the package manually through IIS Manager, as described in [Manually Installing Web Packages](manually-installing-web-packages.md), the installation wizard prompts you to provide values for any parameters.</span></span> <span data-ttu-id="4593c-140">使用してリモート パッケージをインストールする場合、 *. deploy.cmd* 」の説明に従って、ファイル[Web パッケージを展開する](deploying-web-packages.md)、次のようになります Web Deploy *SetParameters.xml*ファイルをパラメーター値を指定します。</span><span class="sxs-lookup"><span data-stu-id="4593c-140">If you install the package remotely using the *.deploy.cmd* file, as described in [Deploying Web Packages](deploying-web-packages.md), Web Deploy will look to this *SetParameters.xml* file to provide the parameter values.</span></span> <span data-ttu-id="4593c-141">内の値を編集することができます、 *SetParameters.xml*ファイルを手動で、または自動のビルドと展開プロセスの一環としてファイルをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="4593c-141">You can edit the values in the *SetParameters.xml* file manually, or you can customize the file as part of an automated build and deployment process.</span></span> <span data-ttu-id="4593c-142">このプロセスは、このトピックで後で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="4593c-142">This process is described in more detail later in this topic.</span></span>

### <a name="custom-parameterization"></a><span data-ttu-id="4593c-143">カスタムのパラメーター化</span><span class="sxs-lookup"><span data-stu-id="4593c-143">Custom Parameterization</span></span>

<span data-ttu-id="4593c-144">複雑な展開シナリオで多くの場合、プロジェクトを展開する前に、追加のプロパティをパラメーター化するします。</span><span class="sxs-lookup"><span data-stu-id="4593c-144">In more complex deployment scenarios, you'll often want to parameterize additional properties before you deploy your project.</span></span> <span data-ttu-id="4593c-145">一般に、任意のプロパティと変換先の環境間で変化する設定がパラメーター化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4593c-145">Generally speaking, you should parameterize any properties and settings that will vary between destination environments.</span></span> <span data-ttu-id="4593c-146">これらを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="4593c-146">These can include:</span></span>

- <span data-ttu-id="4593c-147">サービスのエンドポイント、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-147">Service endpoints in the *web.config* file.</span></span>
- <span data-ttu-id="4593c-148">アプリケーションの設定、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-148">Application settings in the *web.config* file.</span></span>
- <span data-ttu-id="4593c-149">その他の宣言型プロパティには、指定するユーザーが求められます。</span><span class="sxs-lookup"><span data-stu-id="4593c-149">Any other declarative properties that you want to prompt users to specify.</span></span>

<span data-ttu-id="4593c-150">これらのプロパティをパラメーター化する最も簡単な方法が追加するには、 *'parameters.xml'* ファイルを web アプリケーション プロジェクトのルート フォルダー。</span><span class="sxs-lookup"><span data-stu-id="4593c-150">The easiest way to parameterize these properties is to add a *parameters.xml* file to the root folder of your web application project.</span></span> <span data-ttu-id="4593c-151">たとえば、連絡先マネージャー ソリューション、ContactManager.Mvc プロジェクトが含まれています、 *'parameters.xml'* ルート フォルダー内のファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-151">For example, in the Contact Manager solution, the ContactManager.Mvc project includes a *parameters.xml* file in the root folder.</span></span>

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

<span data-ttu-id="4593c-152">このファイルを開く場合、1 つが含まれている表示されます**パラメーター**エントリ。</span><span class="sxs-lookup"><span data-stu-id="4593c-152">If you open this file, you'll see that it contains a single **parameter** entry.</span></span> <span data-ttu-id="4593c-153">エントリを見つけてで ContactService Windows Communication Foundation (WCF) サービスのエンドポイント URL をパラメーター化、XML Path Language (XPath) クエリを使用して、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-153">The entry uses an XML Path Language (XPath) query to locate and parameterize the endpoint URL of the ContactService Windows Communication Foundation (WCF) service in the *web.config* file.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


<span data-ttu-id="4593c-154">展開パッケージにエンドポイントの URL をパラメーター化だけでなく、WPP も追加に対応するエントリ、 *SetParameters.xml*展開パッケージに生成されるファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-154">In addition to parameterizing the endpoint URL in the deployment package, the WPP also adds a corresponding entry to the *SetParameters.xml* file that gets generated alongside the deployment package.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


<span data-ttu-id="4593c-155">展開パッケージを手動でインストールする場合は、IIS マネージャー、サービス エンドポイントのアドレスを自動的にパラメーター化されたプロパティの横を求められます。</span><span class="sxs-lookup"><span data-stu-id="4593c-155">If you install the deployment package manually, IIS Manager will prompt you for the service endpoint address alongside the properties that were parameterized automatically.</span></span> <span data-ttu-id="4593c-156">実行して、展開パッケージをインストールする場合、 *. deploy.cmd*ファイルを編集できます、 *SetParameters.xml*の値と共に、サービスのエンドポイント アドレスの値を指定するファイル、自動的にパラメーター化されたプロパティです。</span><span class="sxs-lookup"><span data-stu-id="4593c-156">If you install the deployment package by running the *.deploy.cmd* file, you can edit the *SetParameters.xml* file to provide a value for the service endpoint address together with values for the properties that were parameterized automatically.</span></span>

<span data-ttu-id="4593c-157">作成する方法の詳細について、 *'parameters.xml'* ファイルを参照してください[方法: デプロイの設定時に、パッケージの構成を使用してパラメーターがインストールされている](https://msdn.microsoft.com/library/ff398068.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4593c-157">For full details on how to create a *parameters.xml* file, see [How to: Use Parameters to Configure Deployment Settings When a Package is Installed](https://msdn.microsoft.com/library/ff398068.aspx).</span></span> <span data-ttu-id="4593c-158">という名前のプロシージャ**Web.config ファイルの設定のデプロイ パラメーターを使用する**手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="4593c-158">The procedure named **To use deployment parameters for Web.config file settings** provides step-by-step instructions.</span></span>

## <a name="modifying-the-setparametersxml-file"></a><span data-ttu-id="4593c-159">SetParameters.xml ファイルの変更</span><span class="sxs-lookup"><span data-stu-id="4593c-159">Modifying the SetParameters.xml File</span></span>

<span data-ttu-id="4593c-160">Web アプリケーションのパッケージを手動で展開する予定のかどうか&#x2014;実行するか、 *. deploy.cmd*ファイルまたはコマンドラインから MSDeploy.exe を実行して&#x2014;停止を手動で編集するには何もない、 *SetParameters.xml*展開する前にファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-160">If you plan to deploy the web application package manually&#x2014;either by running the *.deploy.cmd* file or by running MSDeploy.exe from the command line&#x2014;there's nothing to stop you manually editing the *SetParameters.xml* file prior to the deployment.</span></span> <span data-ttu-id="4593c-161">ただし、エンタープライズ規模のソリューションで作業する場合は、大規模で自動化されたビルドと展開プロセスの一部として web アプリケーション パッケージを配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4593c-161">However, if you're working on an enterprise-scale solution, you may need to deploy a web application package as part of a larger, automated build and deployment process.</span></span> <span data-ttu-id="4593c-162">このシナリオで Microsoft Build Engine (MSBuild) を変更する必要があります、 *SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-162">In this scenario, you need the Microsoft Build Engine (MSBuild) to modify the *SetParameters.xml* file for you.</span></span> <span data-ttu-id="4593c-163">MSBuild を使用してこれを行う**XmlPoke**タスク。</span><span class="sxs-lookup"><span data-stu-id="4593c-163">You can do this by using the MSBuild **XmlPoke** task.</span></span>

<span data-ttu-id="4593c-164">[Contact Manager サンプル ソリューション](the-contact-manager-solution.md)このプロセスを示しています。</span><span class="sxs-lookup"><span data-stu-id="4593c-164">The [Contact Manager sample solution](the-contact-manager-solution.md) illustrates this process.</span></span> <span data-ttu-id="4593c-165">後のコード例は、この例に関連する詳細のみを表示する編集されています。</span><span class="sxs-lookup"><span data-stu-id="4593c-165">The code examples that follow have been edited to show only the details that are relevant to this example.</span></span>

> [!NOTE]
> <span data-ttu-id="4593c-166">サンプル ソリューションでは、および一般的なカスタムのプロジェクト ファイルの概要で、プロジェクト ファイルのモデルのより広範な概要については、次を参照してください。[プロジェクト ファイルを理解する](understanding-the-project-file.md)と[ビルド プロセスを理解する](understanding-the-build-process.md)します。</span><span class="sxs-lookup"><span data-stu-id="4593c-166">For a broader overview of the project file model in the sample solution, and an introduction to custom project files in general, see [Understanding the Project File](understanding-the-project-file.md) and [Understanding the Build Process](understanding-the-build-process.md).</span></span>


<span data-ttu-id="4593c-167">最初に、関心のあるパラメーターの値は、環境固有のプロジェクト ファイル内のプロパティとして定義されます (たとえば、 *Env Dev.proj*)。</span><span class="sxs-lookup"><span data-stu-id="4593c-167">First, the parameter values of interest are defined as properties in the environment-specific project file (for example, *Env-Dev.proj*).</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> <span data-ttu-id="4593c-168">独自のサーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)します。</span><span class="sxs-lookup"><span data-stu-id="4593c-168">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="4593c-169">次に、 *Publish.proj*ファイルは、これらのプロパティをインポートします。</span><span class="sxs-lookup"><span data-stu-id="4593c-169">Next, the *Publish.proj* file imports these properties.</span></span> <span data-ttu-id="4593c-170">ため、各*SetParameters.xml*ファイルが関連付けられている、 *. deploy.cmd*をそれぞれ呼び出すプロジェクト ファイルが最終的にするファイル、および *. deploy.cmd*ファイル、プロジェクトファイルには、MSBuild が作成されます*項目*各 *. deploy.cmd*として関心のあるプロパティを定義ファイルを開き*項目メタデータ*します。</span><span class="sxs-lookup"><span data-stu-id="4593c-170">Because each *SetParameters.xml* file is associated with a *.deploy.cmd* file, and we ultimately want the project file to invoke each *.deploy.cmd* file, the project file creates an MSBuild *item* for each *.deploy.cmd* file and defines the properties of interest as *item metadata*.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


<span data-ttu-id="4593c-171">この場合、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="4593c-171">In this case:</span></span>

- <span data-ttu-id="4593c-172">**ParametersXml**メタデータ値の位置を示す、 *SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-172">The **ParametersXml** metadata value indicates the location of the *SetParameters.xml* file.</span></span>
- <span data-ttu-id="4593c-173">**IisWebAppName**値は、web アプリケーションを配置する IIS パス。</span><span class="sxs-lookup"><span data-stu-id="4593c-173">The **IisWebAppName** value is the IIS path to which you want to deploy the web application.</span></span>
- <span data-ttu-id="4593c-174">**MembershipDBConnectionString**値は、メンバーシップ データベースの接続文字列と**MembershipDBConnectionName**値は、**名前**属性内の対応するパラメーターの*SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-174">The **MembershipDBConnectionString** value is the connection string for the membership database, and the **MembershipDBConnectionName** value is the **name** attribute of the corresponding parameter in the *SetParameters.xml* file.</span></span>
- <span data-ttu-id="4593c-175">**ServiceEndpointValue**値は、移行先サーバー上の WCF サービスのエンドポイント アドレスと**ServiceEndpointParamName**値に対応するパラメーターの name 属性は、*SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-175">The **ServiceEndpointValue** value is the endpoint address for the WCF service on the destination server, and the **ServiceEndpointParamName** value is the name attribute of the corresponding parameter in the *SetParameters.xml* file.</span></span>

<span data-ttu-id="4593c-176">最後に、 *Publish.proj*ファイル、 **PublishWebPackages**対象には、 **XmlPoke**でこれらの値を変更するタスク、 *SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-176">Finally, in the *Publish.proj* file, the **PublishWebPackages** target uses the **XmlPoke** task to modify these values in the *SetParameters.xml* file.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


<span data-ttu-id="4593c-177">各わかります**XmlPoke**タスクでは、4 つの属性の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="4593c-177">You'll notice that each **XmlPoke** task specifies four attribute values:</span></span>

- <span data-ttu-id="4593c-178">**XmlInputPath**属性を変更するファイルの検索場所をタスクに指示します。</span><span class="sxs-lookup"><span data-stu-id="4593c-178">The **XmlInputPath** attribute tells the task where to find the file you want to modify.</span></span>
- <span data-ttu-id="4593c-179">**クエリ**属性が変更する XML ノードを識別する XPath クエリ。</span><span class="sxs-lookup"><span data-stu-id="4593c-179">The **Query** attribute is an XPath query that identifies the XML node you want to change.</span></span>
- <span data-ttu-id="4593c-180">**値**属性が選択されている XML ノードに挿入する新しい値。</span><span class="sxs-lookup"><span data-stu-id="4593c-180">The **Value** attribute is the new value you want to insert into the selected XML node.</span></span>
- <span data-ttu-id="4593c-181">**条件**属性は、条件のロックを実行する必要がありますまたはタスクが実行されません。</span><span class="sxs-lookup"><span data-stu-id="4593c-181">The **Condition** attribute is the criteria on which the task should run or not run.</span></span> <span data-ttu-id="4593c-182">Null または空の値を挿入しようとしていないことが条件では、このような場合は、により、 *SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-182">In these cases, the condition ensures that you don't try to insert a null or empty value into the *SetParameters.xml* file.</span></span>

## <a name="conclusion"></a><span data-ttu-id="4593c-183">まとめ</span><span class="sxs-lookup"><span data-stu-id="4593c-183">Conclusion</span></span>

<span data-ttu-id="4593c-184">このトピックには、役割が説明されている、 *SetParameters.xml*ファイルし、web アプリケーション プロジェクトをビルドするときの生成方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4593c-184">This topic described the role of the *SetParameters.xml* file and explained how it's generated when you build a web application project.</span></span> <span data-ttu-id="4593c-185">追加することで追加の設定をパラメーター化する方法について説明しました、 *'parameters.xml'* ファイルをプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="4593c-185">It explained how you can parameterize additional settings by adding a *parameters.xml* file to your project.</span></span> <span data-ttu-id="4593c-186">変更する方法を説明し、 *SetParameters.xml*ファイルを使用して、拡大、自動化されたビルド プロセスの一部として、 **XmlPoke**プロジェクト ファイルで作業します。</span><span class="sxs-lookup"><span data-stu-id="4593c-186">It also described how you can modify the *SetParameters.xml* file as part of a larger, automated build process, by using the **XmlPoke** task in your project files.</span></span>

<span data-ttu-id="4593c-187">次のトピックでは、 [Web パッケージを展開する](deploying-web-packages.md)、いずれかを実行して web パッケージを展開する方法について説明、 *. deploy.cmd*ファイルまたは直接 MSDeploy.exe を使用してコマンドします。</span><span class="sxs-lookup"><span data-stu-id="4593c-187">The next topic, [Deploying Web Packages](deploying-web-packages.md), describes how you can deploy a web package either by running the *.deploy.cmd* file or by using MSDeploy.exe commands directly.</span></span> <span data-ttu-id="4593c-188">どちらの場合も、指定することができます、 *SetParameters.xml*デプロイ パラメーターとしてファイル。</span><span class="sxs-lookup"><span data-stu-id="4593c-188">In both cases, you can specify your *SetParameters.xml* file as a deployment parameter.</span></span>

## <a name="further-reading"></a><span data-ttu-id="4593c-189">関連項目</span><span class="sxs-lookup"><span data-stu-id="4593c-189">Further Reading</span></span>

<span data-ttu-id="4593c-190">Web パッケージを作成する方法については、次を参照してください。[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)します。</span><span class="sxs-lookup"><span data-stu-id="4593c-190">For information on how to create web packages, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span> <span data-ttu-id="4593c-191">実際に web パッケージをデプロイする方法のガイダンスについては、次を参照してください。 [Web パッケージを展開する](deploying-web-packages.md)します。</span><span class="sxs-lookup"><span data-stu-id="4593c-191">For guidance on how to actually deploy a web package, see [Deploying Web Packages](deploying-web-packages.md).</span></span> <span data-ttu-id="4593c-192">作成する方法についてステップ バイ ステップ チュートリアルについては、 *'parameters.xml'* ファイルを参照してください[方法: デプロイの設定時に、パッケージの構成を使用してパラメーターがインストールされている](https://msdn.microsoft.com/library/ff398068.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4593c-192">For a step-by-step walkthrough on how to create a *parameters.xml* file, see [How to: Use Parameters to Configure Deployment Settings When a Package is Installed](https://msdn.microsoft.com/library/ff398068.aspx).</span></span>

<span data-ttu-id="4593c-193">Web デプロイのパラメーター化の一般的なについては、次を参照してください。[アクションで Web デプロイ Parameterization](https://go.microsoft.com/?linkid=9805119) (ブログの投稿)。</span><span class="sxs-lookup"><span data-stu-id="4593c-193">For more general information on parameterization in Web Deploy, see [Web Deploy Parameterization in Action](https://go.microsoft.com/?linkid=9805119) (blog post).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4593c-194">[前へ](building-and-packaging-web-application-projects.md)
> [次へ](deploying-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="4593c-194">[Previous](building-and-packaging-web-application-projects.md)
[Next](deploying-web-packages.md)</span></span>
