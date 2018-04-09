---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Web パッケージの展開のパラメーターの構成 |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、インターネット インフォメーション サービス (IIS) web アプリケーション名、接続文字列、およびサービス エンドポイントと同様に、パラメーターの値を設定する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7be08f1a1fb7232911a44cf64e2e784dbb95ff48
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="configuring-parameters-for-web-package-deployment"></a><span data-ttu-id="7736d-103">Web パッケージの展開のパラメーターの構成</span><span class="sxs-lookup"><span data-stu-id="7736d-103">Configuring Parameters for Web Package Deployment</span></span>
====================
<span data-ttu-id="7736d-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="7736d-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="7736d-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="7736d-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="7736d-106">このトピックでは、リモートの IIS web サーバーに web パッケージを展開するときは、インターネット インフォメーション サービス (IIS) web アプリケーション名、接続文字列、およびサービス エンドポイントと同様に、パラメーターの値を設定する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="7736d-106">This topic describes how to set parameter values, like Internet Information Services (IIS) web application names, connection strings, and service endpoints, when you deploy a web package to a remote IIS web server.</span></span>


<span data-ttu-id="7736d-107">Web アプリケーション プロジェクト、ビルドおよびパッケージ化プロセスを作成する場合に、次の 3 つのキー ファイルが生成されます。</span><span class="sxs-lookup"><span data-stu-id="7736d-107">When you build a web application project, the build and packaging process generates three key files:</span></span>

- <span data-ttu-id="7736d-108">A *[プロジェクト名] .zip*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-108">A *[project name].zip* file.</span></span> <span data-ttu-id="7736d-109">これは、web アプリケーション プロジェクトの web 配置パッケージです。</span><span class="sxs-lookup"><span data-stu-id="7736d-109">This is the web deployment package for your web application project.</span></span> <span data-ttu-id="7736d-110">このパッケージには、すべてのアセンブリ、ファイル、データベース スクリプト、およびリモートの IIS web サーバーに web アプリケーションを再作成に必要なリソースが含まれています。</span><span class="sxs-lookup"><span data-stu-id="7736d-110">This package contains all the assemblies, files, database scripts, and resources required to recreate your web application on a remote IIS web server.</span></span>
- <span data-ttu-id="7736d-111">A *[プロジェクト名].deploy.cmd*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-111">A *[project name].deploy.cmd* file.</span></span> <span data-ttu-id="7736d-112">これには、リモートの IIS web サーバーに web 配置パッケージを発行する Web Deploy (MSDeploy.exe) のパラメーター化コマンドのセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="7736d-112">This contains a set of parameterized Web Deploy (MSDeploy.exe) commands that publish your web deployment package to a remote IIS web server.</span></span>
- <span data-ttu-id="7736d-113">A *[プロジェクト名]。SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-113">A *[project name].SetParameters.xml* file.</span></span> <span data-ttu-id="7736d-114">これは、MSDeploy.exe コマンドにパラメーター値のセットを提供します。</span><span class="sxs-lookup"><span data-stu-id="7736d-114">This provides a set of parameter values to the MSDeploy.exe command.</span></span> <span data-ttu-id="7736d-115">このファイル内の値を更新し、Web Deploy にパラメーターとして渡す、コマンド ライン web パッケージを展開するときにできます。</span><span class="sxs-lookup"><span data-stu-id="7736d-115">You can update the values in this file and pass it to Web Deploy as a command-line parameter when you deploy your web package.</span></span>

> [!NOTE]
> <span data-ttu-id="7736d-116">ビルドとパッケージ化処理の詳細については、次を参照してください。[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)です。</span><span class="sxs-lookup"><span data-stu-id="7736d-116">For more information on the build and packaging process, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>


<span data-ttu-id="7736d-117">*SetParameters.xml*ファイルは、web アプリケーション プロジェクト ファイルと、プロジェクト内のすべての構成ファイルから動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="7736d-117">The *SetParameters.xml* file is dynamically generated from your web application project file and any configuration files within your project.</span></span> <span data-ttu-id="7736d-118">ビルドおよび Web 発行パイプライン (WPP)、プロジェクトをパッケージ化する場合が多くを移行先の IIS web アプリケーションと同様に、デプロイ環境と任意のデータベース接続文字列の間で変更する可能性のある変数の自動的に検出します。</span><span class="sxs-lookup"><span data-stu-id="7736d-118">When you build and package your project, the Web Publishing Pipeline (WPP) will automatically detect lots of the variables that are likely to change between deployment environments, like the destination IIS web application and any database connection strings.</span></span> <span data-ttu-id="7736d-119">これらの値が自動的に web 配置パッケージのパラメーター化されに追加された、 *SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-119">These values are automatically parameterized in the web deployment package and added to the *SetParameters.xml* file.</span></span> <span data-ttu-id="7736d-120">たとえばへの接続文字列を追加する場合、 *web.config*ファイル、web アプリケーション プロジェクトでビルド プロセスがこの変更を検出しへのエントリを追加、 *SetParameters.xml*ファイルそれに従っています。</span><span class="sxs-lookup"><span data-stu-id="7736d-120">For example, if you add a connection string to the *web.config* file in your web application project, the build process will detect this change and will add an entry to the *SetParameters.xml* file accordingly.</span></span>

<span data-ttu-id="7736d-121">多くの場合では、この自動パラメーター化を十分になります。</span><span class="sxs-lookup"><span data-stu-id="7736d-121">In a lot of cases, this automatic parameterization will be sufficient.</span></span> <span data-ttu-id="7736d-122">ただし、ユーザーがアプリケーションの設定またはサービス エンドポイントの Url と同様に、デプロイ環境間での他の設定を変更する必要があるへの展開パッケージでこれらの値をパラメーター化を対応するエントリを追加する、 WPPを通知*SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-122">However, if your users need to vary other settings between deployment environments, like application settings or service endpoint URLs, you need to tell the WPP to parameterize these values in the deployment package and add corresponding entries to the *SetParameters.xml* file.</span></span> <span data-ttu-id="7736d-123">以降のセクションでは、これを行う方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="7736d-123">The sections that follow explain how to do this.</span></span>

### <a name="automatic-parameterization"></a><span data-ttu-id="7736d-124">自動パラメーター化</span><span class="sxs-lookup"><span data-stu-id="7736d-124">Automatic Parameterization</span></span>

<span data-ttu-id="7736d-125">ビルドし、web アプリケーションをパッケージ化すると、WPP は自動的には次の作業をパラメーターします。</span><span class="sxs-lookup"><span data-stu-id="7736d-125">When you build and package a web application, the WPP will automatically parameterize these things:</span></span>

- <span data-ttu-id="7736d-126">移行先の IIS の web アプリケーションのパスと名前。</span><span class="sxs-lookup"><span data-stu-id="7736d-126">The destination IIS web application path and name.</span></span>
- <span data-ttu-id="7736d-127">任意の接続文字列、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-127">Any connection strings in your *web.config* file.</span></span>
- <span data-ttu-id="7736d-128">すべてのデータベースに追加する接続文字列、**パッケージ化/発行 SQL**プロジェクトのプロパティ ページ タブ。</span><span class="sxs-lookup"><span data-stu-id="7736d-128">Connection strings for any databases you add to the **Package/Publish SQL** tab in the project property pages.</span></span>

<span data-ttu-id="7736d-129">たとえば、ビルドおよびパッケージ化する場合は、[連絡先のマネージャー](the-contact-manager-solution.md) WPP 何らかの方法でパラメーター化プロセスに触れることがなくサンプル ソリューションではこれが生成*ContactManager.Mvc.SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-129">For example, if you were to build and package the [Contact Manager](the-contact-manager-solution.md) sample solution without touching the parameterization process in any way, the WPP would generate this *ContactManager.Mvc.SetParameters.xml* file:</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


<span data-ttu-id="7736d-130">この場合、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="7736d-130">In this case:</span></span>

- <span data-ttu-id="7736d-131">**IIS Web アプリケーション名**パラメーターは、web アプリケーションを展開する IIS のパス。</span><span class="sxs-lookup"><span data-stu-id="7736d-131">The **IIS Web Application Name** parameter is the IIS path where you want to deploy the web application.</span></span> <span data-ttu-id="7736d-132">既定値はから取得、**パッケージ化/発行 Web**プロジェクトのプロパティ ページでページ。</span><span class="sxs-lookup"><span data-stu-id="7736d-132">The default value is taken from the **Package/Publish Web** page in the project property pages.</span></span>
- <span data-ttu-id="7736d-133">**Web.config ApplicationServices 接続文字列**パラメーターはから生成された、 **connectionStrings/追加**内の要素、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-133">The **ApplicationServices-Web.config Connection String** parameter was generated from a **connectionStrings/add** element in the *web.config* file.</span></span> <span data-ttu-id="7736d-134">メンバーシップ データベースに接続するアプリケーションで使用される接続文字列を表します。</span><span class="sxs-lookup"><span data-stu-id="7736d-134">It represents the connection string that the application should use to contact the membership database.</span></span> <span data-ttu-id="7736d-135">ここでは、指定した値に代入され、配置された*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-135">The value you provide here will be substituted into the deployed *web.config* file.</span></span> <span data-ttu-id="7736d-136">配置前から既定値を取得*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-136">The default value is taken from the pre-deployment *web.config* file.</span></span>

<span data-ttu-id="7736d-137">WPP には、これらのプロパティを生成する展開パッケージにもパラメーター化します。</span><span class="sxs-lookup"><span data-stu-id="7736d-137">The WPP also parameterizes these properties in the deployment package it generates.</span></span> <span data-ttu-id="7736d-138">展開パッケージをインストールするときに、これらのプロパティの値を指定できます。</span><span class="sxs-lookup"><span data-stu-id="7736d-138">You can provide values for these properties when you install the deployment package.</span></span> <span data-ttu-id="7736d-139">」の説明に従って手動で IIS マネージャーで、パッケージをインストールする[Web パッケージを手動でインストールする](manually-installing-web-packages.md)、インストール ウィザードでは、すべてのパラメーターの値を指定するように求められます。</span><span class="sxs-lookup"><span data-stu-id="7736d-139">If you install the package manually through IIS Manager, as described in [Manually Installing Web Packages](manually-installing-web-packages.md), the installation wizard prompts you to provide values for any parameters.</span></span> <span data-ttu-id="7736d-140">使用してリモート パッケージをインストールする場合、 *. deploy.cmd*ファイル、」の説明に従って[Web パッケージを展開する](deploying-web-packages.md)、次のようになります Web Deploy *SetParameters.xml*ファイルの名前をパラメーターの値を提供します。</span><span class="sxs-lookup"><span data-stu-id="7736d-140">If you install the package remotely using the *.deploy.cmd* file, as described in [Deploying Web Packages](deploying-web-packages.md), Web Deploy will look to this *SetParameters.xml* file to provide the parameter values.</span></span> <span data-ttu-id="7736d-141">内の値を編集することができます、 *SetParameters.xml*ファイルを手動で、または自動のビルドおよび配置プロセスの一環としてファイルをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="7736d-141">You can edit the values in the *SetParameters.xml* file manually, or you can customize the file as part of an automated build and deployment process.</span></span> <span data-ttu-id="7736d-142">このプロセスは、このトピックで後で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="7736d-142">This process is described in more detail later in this topic.</span></span>

### <a name="custom-parameterization"></a><span data-ttu-id="7736d-143">カスタム パラメーター化</span><span class="sxs-lookup"><span data-stu-id="7736d-143">Custom Parameterization</span></span>

<span data-ttu-id="7736d-144">複雑な展開シナリオで多くの場合にしておく、プロジェクトを展開する前に、追加のプロパティをパラメーター化します。</span><span class="sxs-lookup"><span data-stu-id="7736d-144">In more complex deployment scenarios, you'll often want to parameterize additional properties before you deploy your project.</span></span> <span data-ttu-id="7736d-145">一般に、任意のプロパティと展開先環境間で変化する設定をパラメーター化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7736d-145">Generally speaking, you should parameterize any properties and settings that will vary between destination environments.</span></span> <span data-ttu-id="7736d-146">これらを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="7736d-146">These can include:</span></span>

- <span data-ttu-id="7736d-147">サービスのエンドポイント、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-147">Service endpoints in the *web.config* file.</span></span>
- <span data-ttu-id="7736d-148">アプリケーション設定で、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-148">Application settings in the *web.config* file.</span></span>
- <span data-ttu-id="7736d-149">その他の宣言型プロパティを指定するユーザーを表示します。</span><span class="sxs-lookup"><span data-stu-id="7736d-149">Any other declarative properties that you want to prompt users to specify.</span></span>

<span data-ttu-id="7736d-150">これらのプロパティをパラメーター化する最も簡単な方法は追加する、 *parameters.xml*ファイルを web アプリケーション プロジェクトのルート フォルダー。</span><span class="sxs-lookup"><span data-stu-id="7736d-150">The easiest way to parameterize these properties is to add a *parameters.xml* file to the root folder of your web application project.</span></span> <span data-ttu-id="7736d-151">たとえば、ソリューションでは、連絡先のマネージャー、ContactManager.Mvc プロジェクトが含まれています、 *parameters.xml*ルート フォルダー内のファイルです。</span><span class="sxs-lookup"><span data-stu-id="7736d-151">For example, in the Contact Manager solution, the ContactManager.Mvc project includes a *parameters.xml* file in the root folder.</span></span>

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

<span data-ttu-id="7736d-152">このファイルを開くと、表示されます、1 つが含まれている**パラメーター**エントリです。</span><span class="sxs-lookup"><span data-stu-id="7736d-152">If you open this file, you'll see that it contains a single **parameter** entry.</span></span> <span data-ttu-id="7736d-153">エントリを探して、ContactService Windows Communication Foundation (WCF) サービスのエンドポイントの URL をパラメーター化 XML Path Language (XPath) クエリを使用して、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-153">The entry uses an XML Path Language (XPath) query to locate and parameterize the endpoint URL of the ContactService Windows Communication Foundation (WCF) service in the *web.config* file.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


<span data-ttu-id="7736d-154">展開パッケージにエンドポイントの URL をパラメーター化だけでなく、WPP も追加に対応するエントリ、 *SetParameters.xml*展開パッケージと共に生成されるファイルです。</span><span class="sxs-lookup"><span data-stu-id="7736d-154">In addition to parameterizing the endpoint URL in the deployment package, the WPP also adds a corresponding entry to the *SetParameters.xml* file that gets generated alongside the deployment package.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


<span data-ttu-id="7736d-155">展開パッケージを手動でインストールする場合は、IIS マネージャーのサービス エンドポイントのアドレスと共に自動的にパラメーター化されたプロパティを求められます。</span><span class="sxs-lookup"><span data-stu-id="7736d-155">If you install the deployment package manually, IIS Manager will prompt you for the service endpoint address alongside the properties that were parameterized automatically.</span></span> <span data-ttu-id="7736d-156">実行して、展開パッケージをインストールする場合、 *. deploy.cmd*ファイルを編集、 *SetParameters.xml*の値と共にサービス エンドポイントのアドレスの値を指定するファイル、自動的にパラメーター化されたプロパティです。</span><span class="sxs-lookup"><span data-stu-id="7736d-156">If you install the deployment package by running the *.deploy.cmd* file, you can edit the *SetParameters.xml* file to provide a value for the service endpoint address together with values for the properties that were parameterized automatically.</span></span>

<span data-ttu-id="7736d-157">作成する方法の詳細について、 *parameters.xml*ファイルを参照してください[する方法: 展開の設定時に、パッケージの構成を使用するパラメーターがインストールされている](https://msdn.microsoft.com/library/ff398068.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="7736d-157">For full details on how to create a *parameters.xml* file, see [How to: Use Parameters to Configure Deployment Settings When a Package is Installed](https://msdn.microsoft.com/library/ff398068.aspx).</span></span> <span data-ttu-id="7736d-158">という名前のプロシージャ**Web.config ファイルの設定をデプロイのパラメーターを使用する**手順に沿って説明します。</span><span class="sxs-lookup"><span data-stu-id="7736d-158">The procedure named **To use deployment parameters for Web.config file settings** provides step-by-step instructions.</span></span>

## <a name="modifying-the-setparametersxml-file"></a><span data-ttu-id="7736d-159">SetParameters.xml ファイルを変更します。</span><span class="sxs-lookup"><span data-stu-id="7736d-159">Modifying the SetParameters.xml File</span></span>

<span data-ttu-id="7736d-160">Web アプリケーションのパッケージを手動で展開する予定のかどうか&#x2014;実行するか、 *. deploy.cmd*ファイルまたはコマンドラインから MSDeploy.exe を実行して&#x2014;停止を手動で編集するには nothing を使用する必要がある、 *SetParameters.xml*展開する前にファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-160">If you plan to deploy the web application package manually&#x2014;either by running the *.deploy.cmd* file or by running MSDeploy.exe from the command line&#x2014;there's nothing to stop you manually editing the *SetParameters.xml* file prior to the deployment.</span></span> <span data-ttu-id="7736d-161">ただし、エンタープライズ規模ソリューションで、使用している場合より大きな、自動化されたビルドおよび配置プロセスの一部として web アプリケーション パッケージを配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7736d-161">However, if you're working on an enterprise-scale solution, you may need to deploy a web application package as part of a larger, automated build and deployment process.</span></span> <span data-ttu-id="7736d-162">このシナリオでは、Microsoft Build Engine (MSBuild) を変更する必要があります、 *SetParameters.xml*のためにファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-162">In this scenario, you need the Microsoft Build Engine (MSBuild) to modify the *SetParameters.xml* file for you.</span></span> <span data-ttu-id="7736d-163">MSBuild を使用してこれを行う**XmlPoke**タスク。</span><span class="sxs-lookup"><span data-stu-id="7736d-163">You can do this by using the MSBuild **XmlPoke** task.</span></span>

<span data-ttu-id="7736d-164">[連絡先のマネージャーのサンプル ソリューション](the-contact-manager-solution.md)このプロセスを示しています。</span><span class="sxs-lookup"><span data-stu-id="7736d-164">The [Contact Manager sample solution](the-contact-manager-solution.md) illustrates this process.</span></span> <span data-ttu-id="7736d-165">次のコード例は、この例に関連する詳細のみを表示する編集されました。</span><span class="sxs-lookup"><span data-stu-id="7736d-165">The code examples that follow have been edited to show only the details that are relevant to this example.</span></span>

> [!NOTE]
> <span data-ttu-id="7736d-166">プロジェクト ファイルのモデルのサンプル ソリューションと全般にカスタム プロジェクト ファイルの概要についての広範な概要については、次を参照してください。[プロジェクト ファイルを理解する](understanding-the-project-file.md)と[ビルド プロセスの理解](understanding-the-build-process.md)です。</span><span class="sxs-lookup"><span data-stu-id="7736d-166">For a broader overview of the project file model in the sample solution, and an introduction to custom project files in general, see [Understanding the Project File](understanding-the-project-file.md) and [Understanding the Build Process](understanding-the-build-process.md).</span></span>


<span data-ttu-id="7736d-167">最初に、目的のパラメーター値は、環境固有のプロジェクト ファイル内のプロパティとして定義されます (たとえば、 *Env Dev.proj*)。</span><span class="sxs-lookup"><span data-stu-id="7736d-167">First, the parameter values of interest are defined as properties in the environment-specific project file (for example, *Env-Dev.proj*).</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> <span data-ttu-id="7736d-168">サーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)です。</span><span class="sxs-lookup"><span data-stu-id="7736d-168">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="7736d-169">次に、 *Publish.proj*ファイルは、これらのプロパティをインポートします。</span><span class="sxs-lookup"><span data-stu-id="7736d-169">Next, the *Publish.proj* file imports these properties.</span></span> <span data-ttu-id="7736d-170">各*SetParameters.xml*ファイルに関連付けられている、 *. deploy.cmd*ファイル、最終的にする、プロジェクト ファイルをそれぞれ呼び出す*. deploy.cmd*ファイル、プロジェクトファイルを作成、MSBuild*項目*ごと*. deploy.cmd*ファイルし、としての目的のプロパティを定義*項目メタデータ*です。</span><span class="sxs-lookup"><span data-stu-id="7736d-170">Because each *SetParameters.xml* file is associated with a *.deploy.cmd* file, and we ultimately want the project file to invoke each *.deploy.cmd* file, the project file creates an MSBuild *item* for each *.deploy.cmd* file and defines the properties of interest as *item metadata*.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


<span data-ttu-id="7736d-171">この場合、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="7736d-171">In this case:</span></span>

- <span data-ttu-id="7736d-172">**ParametersXml**メタデータ値の位置を示す、 *SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-172">The **ParametersXml** metadata value indicates the location of the *SetParameters.xml* file.</span></span>
- <span data-ttu-id="7736d-173">**IisWebAppName**値は、web アプリケーションを展開する、IIS パス。</span><span class="sxs-lookup"><span data-stu-id="7736d-173">The **IisWebAppName** value is the IIS path to which you want to deploy the web application.</span></span>
- <span data-ttu-id="7736d-174">**MembershipDBConnectionString**値は、メンバーシップ データベースの接続文字列と**MembershipDBConnectionName**値は、**名前**属性対応するパラメーターの*SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-174">The **MembershipDBConnectionString** value is the connection string for the membership database, and the **MembershipDBConnectionName** value is the **name** attribute of the corresponding parameter in the *SetParameters.xml* file.</span></span>
- <span data-ttu-id="7736d-175">**ServiceEndpointValue**値は、移行先サーバー上の WCF サービスのエンドポイント アドレス、および**ServiceEndpointParamName**値は、対応するパラメーター内の name 属性*SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-175">The **ServiceEndpointValue** value is the endpoint address for the WCF service on the destination server, and the **ServiceEndpointParamName** value is the name attribute of the corresponding parameter in the *SetParameters.xml* file.</span></span>

<span data-ttu-id="7736d-176">最後に、 *Publish.proj*ファイル、 **PublishWebPackages**対象には、 **XmlPoke**でこれらの値を変更するタスク、 *SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-176">Finally, in the *Publish.proj* file, the **PublishWebPackages** target uses the **XmlPoke** task to modify these values in the *SetParameters.xml* file.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


<span data-ttu-id="7736d-177">各わかります**XmlPoke**タスクは、4 つの属性の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="7736d-177">You'll notice that each **XmlPoke** task specifies four attribute values:</span></span>

- <span data-ttu-id="7736d-178">**XmlInputPath**属性を変更するファイルを検索する位置をタスクに通知します。</span><span class="sxs-lookup"><span data-stu-id="7736d-178">The **XmlInputPath** attribute tells the task where to find the file you want to modify.</span></span>
- <span data-ttu-id="7736d-179">**クエリ**属性は、XPath クエリを変更する XML ノードを識別します。</span><span class="sxs-lookup"><span data-stu-id="7736d-179">The **Query** attribute is an XPath query that identifies the XML node you want to change.</span></span>
- <span data-ttu-id="7736d-180">**値**属性が選択されている XML ノードに挿入する新しい値。</span><span class="sxs-lookup"><span data-stu-id="7736d-180">The **Value** attribute is the new value you want to insert into the selected XML node.</span></span>
- <span data-ttu-id="7736d-181">**条件**属性は、条件のロックを実行する必要がありますまたはタスクが実行されません。</span><span class="sxs-lookup"><span data-stu-id="7736d-181">The **Condition** attribute is the criteria on which the task should run or not run.</span></span> <span data-ttu-id="7736d-182">このような場合は、条件によりに null または空の値を挿入しようとしていないこと、 *SetParameters.xml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-182">In these cases, the condition ensures that you don't try to insert a null or empty value into the *SetParameters.xml* file.</span></span>

## <a name="conclusion"></a><span data-ttu-id="7736d-183">まとめ</span><span class="sxs-lookup"><span data-stu-id="7736d-183">Conclusion</span></span>

<span data-ttu-id="7736d-184">このトピックには、役割が説明されている、 *SetParameters.xml*ファイルし、web アプリケーション プロジェクトをビルドするときの生成方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="7736d-184">This topic described the role of the *SetParameters.xml* file and explained how it's generated when you build a web application project.</span></span> <span data-ttu-id="7736d-185">追加の設定を追加することによってパラメーター化できる方法について説明しましたが、 *parameters.xml*ファイルをプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="7736d-185">It explained how you can parameterize additional settings by adding a *parameters.xml* file to your project.</span></span> <span data-ttu-id="7736d-186">変更する方法も説明、 *SetParameters.xml*ファイルを使用してより大規模で自動化されたビルド プロセスの一環として、 **XmlPoke**プロジェクト ファイル内のタスクです。</span><span class="sxs-lookup"><span data-stu-id="7736d-186">It also described how you can modify the *SetParameters.xml* file as part of a larger, automated build process, by using the **XmlPoke** task in your project files.</span></span>

<span data-ttu-id="7736d-187">次のトピックでは、 [Web パッケージを展開する](deploying-web-packages.md)、実行するか web パッケージを展開する方法について説明、 *. deploy.cmd* MSDeploy.exe を使用してコマンドを直接またはします。</span><span class="sxs-lookup"><span data-stu-id="7736d-187">The next topic, [Deploying Web Packages](deploying-web-packages.md), describes how you can deploy a web package either by running the *.deploy.cmd* file or by using MSDeploy.exe commands directly.</span></span> <span data-ttu-id="7736d-188">どちらの場合を指定できます、 *SetParameters.xml*展開パラメーター ファイル。</span><span class="sxs-lookup"><span data-stu-id="7736d-188">In both cases, you can specify your *SetParameters.xml* file as a deployment parameter.</span></span>

## <a name="further-reading"></a><span data-ttu-id="7736d-189">関連項目</span><span class="sxs-lookup"><span data-stu-id="7736d-189">Further Reading</span></span>

<span data-ttu-id="7736d-190">Web パッケージを作成する方法については、次を参照してください。[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)です。</span><span class="sxs-lookup"><span data-stu-id="7736d-190">For information on how to create web packages, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span> <span data-ttu-id="7736d-191">実際には、web のパッケージを配置する方法のガイダンスについては、次を参照してください。 [Web パッケージを展開する](deploying-web-packages.md)です。</span><span class="sxs-lookup"><span data-stu-id="7736d-191">For guidance on how to actually deploy a web package, see [Deploying Web Packages](deploying-web-packages.md).</span></span> <span data-ttu-id="7736d-192">作成する方法についてステップ バイ ステップ チュートリアルについては、 *parameters.xml*ファイルを参照してください[する方法: 展開の設定時に、パッケージの構成を使用するパラメーターがインストールされている](https://msdn.microsoft.com/library/ff398068.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="7736d-192">For a step-by-step walkthrough on how to create a *parameters.xml* file, see [How to: Use Parameters to Configure Deployment Settings When a Package is Installed](https://msdn.microsoft.com/library/ff398068.aspx).</span></span>

<span data-ttu-id="7736d-193">Web Deploy でパラメーターの一般的なについては、次を参照してください。[アクションで Web 展開パラメーター](https://go.microsoft.com/?linkid=9805119) (ブログの投稿)。</span><span class="sxs-lookup"><span data-stu-id="7736d-193">For more general information on parameterization in Web Deploy, see [Web Deploy Parameterization in Action](https://go.microsoft.com/?linkid=9805119) (blog post).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7736d-194">[前へ](building-and-packaging-web-application-projects.md)
> [次へ](deploying-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="7736d-194">[Previous](building-and-packaging-web-application-projects.md)
[Next](deploying-web-packages.md)</span></span>
