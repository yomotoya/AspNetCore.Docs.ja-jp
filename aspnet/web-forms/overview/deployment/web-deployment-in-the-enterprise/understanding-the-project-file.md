---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: プロジェクト ファイルを理解する |Microsoft Docs
author: jrjlee
description: Microsoft Build Engine (MSBuild) プロジェクト ファイルは、ビルドおよび配置プロセスの中核にあります。 このトピックでは、MSBuild の概念の概要を開始しています.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 7e117459f5953be7bac53267700dfb9f69802aec
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836844"
---
<a name="understanding-the-project-file"></a><span data-ttu-id="603b4-104">プロジェクト ファイルを理解します。</span><span class="sxs-lookup"><span data-stu-id="603b4-104">Understanding the Project File</span></span>
====================
<span data-ttu-id="603b4-105">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="603b4-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="603b4-106">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="603b4-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="603b4-107">Microsoft Build Engine (MSBuild) プロジェクト ファイルは、ビルドおよび配置プロセスの中核にあります。</span><span class="sxs-lookup"><span data-stu-id="603b4-107">Microsoft Build Engine (MSBuild) project files lie at the heart of the build and deployment process.</span></span> <span data-ttu-id="603b4-108">このトピックでは、MSBuild とプロジェクト ファイルの概念の概要から始まります。</span><span class="sxs-lookup"><span data-stu-id="603b4-108">This topic starts with a conceptual overview of MSBuild and the project file.</span></span> <span data-ttu-id="603b4-109">これには、プロジェクトのファイルを使用して、プロジェクト ファイルを使用して、実際のアプリケーションをデプロイする方法の例を使用して動作時に遭遇するが、主要なコンポーネントについて説明します。</span><span class="sxs-lookup"><span data-stu-id="603b4-109">It describes the key components you'll come across when you work with project files, and it works through an example of how you can use project files to deploy real-world applications.</span></span>
> 
> <span data-ttu-id="603b4-110">学習内容。</span><span class="sxs-lookup"><span data-stu-id="603b4-110">What you'll learn:</span></span>
> 
> - <span data-ttu-id="603b4-111">MSBuild がプロジェクトをビルドするのに MSBuild プロジェクト ファイルを使用方法。</span><span class="sxs-lookup"><span data-stu-id="603b4-111">How MSBuild uses MSBuild project files to build projects.</span></span>
> - <span data-ttu-id="603b4-112">方法、MSBuild は、インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) などの展開テクノロジと統合します。</span><span class="sxs-lookup"><span data-stu-id="603b4-112">How MSBuild integrates with deployment technologies, like the Internet Information Services (IIS) Web Deployment Tool (Web Deploy).</span></span>
> - <span data-ttu-id="603b4-113">プロジェクト ファイルの主要なコンポーネントを理解する方法。</span><span class="sxs-lookup"><span data-stu-id="603b4-113">How to understand the key components of a project file.</span></span>
> - <span data-ttu-id="603b4-114">どのプロジェクト ファイルを使用して、複雑なアプリケーションをビルドおよびデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="603b4-114">How you can use project files to build and deploy complex applications.</span></span>


## <a name="msbuild-and-the-project-file"></a><span data-ttu-id="603b4-115">MSBuild とプロジェクト ファイル</span><span class="sxs-lookup"><span data-stu-id="603b4-115">MSBuild and the Project File</span></span>

<span data-ttu-id="603b4-116">作成する Visual Studio でソリューションを構築すると、Visual Studio は、ソリューション内の各プロジェクトをビルドするのに MSBuild を使用します。</span><span class="sxs-lookup"><span data-stu-id="603b4-116">When you create and build solutions in Visual Studio, Visual Studio uses MSBuild to build each project in your solution.</span></span> <span data-ttu-id="603b4-117">すべての Visual Studio プロジェクトには、プロジェクトの種類を反映するファイル拡張子を持つ、MSBuild プロジェクト ファイルが含まれています。&#x2014;c# プロジェクト (.csproj)、Visual basic.net プロジェクト (.vbproj)、またはデータベース プロジェクト (.dbproj)。</span><span class="sxs-lookup"><span data-stu-id="603b4-117">Every Visual Studio project includes an MSBuild project file, with a file extension that reflects the type of project&#x2014;for example, a C# project (.csproj), a Visual Basic.NET project (.vbproj), or a database project (.dbproj).</span></span> <span data-ttu-id="603b4-118">プロジェクトをビルドするために、MSBuild はプロジェクトに関連付けられているプロジェクト ファイルを処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="603b4-118">In order to build a project, MSBuild must process the project file associated with the project.</span></span> <span data-ttu-id="603b4-119">プロジェクト ファイルは、すべての情報と MSBuild がコンテンツを含めるには、プラットフォームの要件、バージョン管理情報、web サーバーまたはデータベース サーバーの設定など、プロジェクトをビルドするために必要な手順を含む XML ドキュメントでは、実行する必要があるタスク。</span><span class="sxs-lookup"><span data-stu-id="603b4-119">The project file is an XML document that contains all the information and instructions that MSBuild needs in order to build your project, like the content to include, the platform requirements, versioning information, web server or database server settings, and the tasks that must be performed.</span></span>

<span data-ttu-id="603b4-120">MSBuild プロジェクト ファイルがに基づいて、 [MSBuild XML スキーマ](https://msdn.microsoft.com/library/5dy88c2e.aspx)、その結果、ビルド プロセスは完全にオープンで透明とします。</span><span class="sxs-lookup"><span data-stu-id="603b4-120">MSBuild project files are based on the [MSBuild XML schema](https://msdn.microsoft.com/library/5dy88c2e.aspx), and as a result the build process is entirely open and transparent.</span></span> <span data-ttu-id="603b4-121">さらに、MSBuild エンジンを使用するには、Visual Studio をインストールする必要はありません&#x2014;MSBuild.exe の実行可能ファイルは、.NET Framework の一部と、コマンド プロンプトから行うことができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-121">In addition, you don't need to install Visual Studio in order to use the MSBuild engine&#x2014;the MSBuild.exe executable is part of the .NET Framework, and you can run it from a command prompt.</span></span> <span data-ttu-id="603b4-122">開発者には、MSBuild XML スキーマを使用して、プロジェクトをビルドおよび配置する方法を洗練されたきめ細かな制御を強制する独自 MSBuild プロジェクト ファイルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="603b4-122">As a developer, you can craft your own MSBuild project files, using the MSBuild XML schema, to impose sophisticated and fine-grained control over how your projects are built and deployed.</span></span> <span data-ttu-id="603b4-123">これらのカスタム プロジェクト ファイルは、Visual Studio によって自動的に生成されるプロジェクト ファイルとまったく同じ方法で動作します。</span><span class="sxs-lookup"><span data-stu-id="603b4-123">These custom project files work in exactly the same way as the project files that Visual Studio generates automatically.</span></span>

> [!NOTE]
> <span data-ttu-id="603b4-124">Team Build サービスでは、Team Foundation Server (TFS) と MSBuild プロジェクト ファイルを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="603b4-124">You can also use MSBuild project files with the Team Build service in Team Foundation Server (TFS).</span></span> <span data-ttu-id="603b4-125">たとえば、継続的インテグレーション (CI) のシナリオでのプロジェクト ファイルを使用して、新しいコードをチェックインするときに、テスト環境にデプロイを自動化することができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-125">For example, you can use project files in continuous integration (CI) scenarios to automate deployment to a test environment when new code is checked in.</span></span> <span data-ttu-id="603b4-126">詳細については、次を参照してください。 [Web デプロイの自動化用の Team Foundation Server の構成](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="603b4-126">For more information, see [Configuring Team Foundation Server for Automated Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span></span>


### <a name="project-file-naming-conventions"></a><span data-ttu-id="603b4-127">プロジェクト ファイルの名前付け規則</span><span class="sxs-lookup"><span data-stu-id="603b4-127">Project File Naming Conventions</span></span>

<span data-ttu-id="603b4-128">プロジェクト ファイルを作成するときに、希望する任意のファイル拡張子を使用できます。</span><span class="sxs-lookup"><span data-stu-id="603b4-128">When you create your own project files, you can use any file extension you like.</span></span> <span data-ttu-id="603b4-129">ただし、お客様のソリューションを他のユーザーが理解しやすくするには、これらの一般的な規則を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="603b4-129">However, to make your solutions easier for others to understand, you should use these common conventions:</span></span>

- <span data-ttu-id="603b4-130">プロジェクトをビルドするプロジェクト ファイルを作成すると、.proj 拡張機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="603b4-130">Use the .proj extension when you create a project file that builds projects.</span></span>
- <span data-ttu-id="603b4-131">他のプロジェクト ファイルをインポートする再利用可能なプロジェクト ファイルを作成するときに .targets 拡張子を使用します。</span><span class="sxs-lookup"><span data-stu-id="603b4-131">Use the .targets extension when you create a reusable project file to import into other project files.</span></span> <span data-ttu-id="603b4-132">.Targets 拡張子を持つファイル通常しない何でも構築自体、単に、.proj ファイルにインポートできる手順が含まれます。</span><span class="sxs-lookup"><span data-stu-id="603b4-132">Files with a .targets extension typically don't build anything themselves, they simply contain instructions that you can import into your .proj files.</span></span>

### <a name="integration-with-deployment-technologies"></a><span data-ttu-id="603b4-133">展開テクノロジとの統合</span><span class="sxs-lookup"><span data-stu-id="603b4-133">Integration with Deployment Technologies</span></span>

<span data-ttu-id="603b4-134">ASP.NET web アプリケーションおよび ASP.NET MVC web アプリケーションのように、Visual Studio 2010 で web アプリケーション プロジェクトで作業したことがある場合は、これらのプロジェクトにパッケージ化と配置ターゲット環境に web アプリケーションの組み込みサポートが含まれていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="603b4-134">If you've worked with web application projects in Visual Studio 2010, like ASP.NET web applications and ASP.NET MVC web applications, you'll know that these projects include built-in support for packaging and deploying the web application to a target environment.</span></span> <span data-ttu-id="603b4-135">**プロパティ**これらのプロジェクトに含まれているページ**パッケージ化/発行 Web**と**パッケージ化/発行 SQL**構成に使用できるタブ方法のコンポーネント、アプリケーションをパッケージ化され、デプロイします。</span><span class="sxs-lookup"><span data-stu-id="603b4-135">The **Properties** pages for these projects include **Package/Publish Web** and **Package/Publish SQL** tabs that you can use to configure how the components of your application are packaged and deployed.</span></span> <span data-ttu-id="603b4-136">これを示しています、**パッケージ化/発行 Web**  タブ。</span><span class="sxs-lookup"><span data-stu-id="603b4-136">This shows the **Package/Publish Web** tab:</span></span>

![](understanding-the-project-file/_static/image1.png)

<span data-ttu-id="603b4-137">これらの機能の背後にある基になるテクノロジは、Web 発行パイプライン (WPP) と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="603b4-137">The underlying technology behind these capabilities is known as the Web Publishing Pipeline (WPP).</span></span> <span data-ttu-id="603b4-138">基本的には、MSBuild、WPP と[Web Deploy](https://go.microsoft.com/?linkid=9805122) web アプリケーションの完全なビルド、パッケージ、およびデプロイ プロセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="603b4-138">The WPP essentially brings MSBuild and [Web Deploy](https://go.microsoft.com/?linkid=9805122) together to provide a complete build, package, and deployment process for your web applications.</span></span>

<span data-ttu-id="603b4-139">良い知らせは、web プロジェクト用のカスタム プロジェクト ファイルを作成するときに、WPP が提供する統合ポイントの利用を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-139">The good news is that you can take advantage of the integration points that the WPP provides when you create custom project files for web projects.</span></span> <span data-ttu-id="603b4-140">プロジェクトをビルドし、web デプロイ パッケージを作成、およびリモート サーバー上で 1 つのプロジェクト ファイルと MSBuild への 1 回の呼び出しを通じてこれらのパッケージをインストールすることができるプロジェクト ファイルでは、デプロイの手順を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-140">You can include deployment instructions in your project file, which allows you to build your projects, create web deployment packages, and install these packages on remote servers through a single project file and a single call to MSBuild.</span></span> <span data-ttu-id="603b4-141">実行可能ファイル、ビルド プロセスの一部として呼び出すこともできます。</span><span class="sxs-lookup"><span data-stu-id="603b4-141">You can also call any other executables as part of your build process.</span></span> <span data-ttu-id="603b4-142">たとえば、スキーマ ファイルからデータベースを配置する VSDBCMD.exe コマンド ライン ツールを実行できます。</span><span class="sxs-lookup"><span data-stu-id="603b4-142">For example, you can run the VSDBCMD.exe command-line tool to deploy a database from a schema file.</span></span> <span data-ttu-id="603b4-143">このトピックの過程で、エンタープライズ展開シナリオの要件を満たすためにこれらの機能を活用を実行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="603b4-143">Over the course of this topic, you'll see how you can take advantage of these capabilities to meet the requirements of your enterprise deployment scenarios.</span></span>

> [!NOTE]
> <span data-ttu-id="603b4-144">Web アプリケーションの展開プロセスのしくみの詳細については、次を参照してください。 [ASP.NET Web アプリケーション プロジェクトの配置の概要](https://msdn.microsoft.com/library/dd394698.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="603b4-144">For more information on how the web application deployment process works, see [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698.aspx).</span></span>


## <a name="the-anatomy-of-a-project-file"></a><span data-ttu-id="603b4-145">プロジェクト ファイルの構造</span><span class="sxs-lookup"><span data-stu-id="603b4-145">The Anatomy of a Project File</span></span>

<span data-ttu-id="603b4-146">ビルド プロセスの詳細を見ると、前に、MSBuild プロジェクト ファイルの基本的な構造を理解して、わずかな時間をかけて価値です。</span><span class="sxs-lookup"><span data-stu-id="603b4-146">Before you look at the build process in more detail, it's worth taking a few moments to familiarize yourself with the basic structure of an MSBuild project file.</span></span> <span data-ttu-id="603b4-147">このセクションでは、確認、編集、またはプロジェクト ファイルを作成するときに発生する一般的な要素の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="603b4-147">This section provides an overview of the more common elements that you'll encounter when you review, edit, or create a project file.</span></span> <span data-ttu-id="603b4-148">具体的には、学習内容。</span><span class="sxs-lookup"><span data-stu-id="603b4-148">In particular, you'll learn:</span></span>

- <span data-ttu-id="603b4-149">使用する*プロパティ*変数のビルド プロセスを管理します。</span><span class="sxs-lookup"><span data-stu-id="603b4-149">How to use *properties* to manage variables for the build process.</span></span>
- <span data-ttu-id="603b4-150">使用する*項目*などのコード ファイルのビルド プロセスへの入力を識別するためにします。</span><span class="sxs-lookup"><span data-stu-id="603b4-150">How to use *items* to identify the inputs to the build process, like code files.</span></span>
- <span data-ttu-id="603b4-151">使用する*ターゲット*と*タスク*msbuild、実行手順を提供するを使用して*プロパティ*と*項目*で定義されています。プロジェクト ファイルです。</span><span class="sxs-lookup"><span data-stu-id="603b4-151">How to use *targets* and *tasks* to provide execution instructions to MSBuild, using *properties* and *items* defined elsewhere in the project file.</span></span>

<span data-ttu-id="603b4-152">これは、MSBuild プロジェクト ファイルの主な要素間の関係を示します。</span><span class="sxs-lookup"><span data-stu-id="603b4-152">This shows the relationship between the key elements in an MSBuild project file:</span></span>

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a><span data-ttu-id="603b4-153">Project 要素</span><span class="sxs-lookup"><span data-stu-id="603b4-153">The Project Element</span></span>

<span data-ttu-id="603b4-154">[プロジェクト](https://msdn.microsoft.com/library/bcxfsh87.aspx)要素はすべてのプロジェクト ファイルのルート要素です。</span><span class="sxs-lookup"><span data-stu-id="603b4-154">The [Project](https://msdn.microsoft.com/library/bcxfsh87.aspx) element is the root element of every project file.</span></span> <span data-ttu-id="603b4-155">プロジェクト ファイルの XML スキーマを識別するだけでなく、**プロジェクト**要素は、ビルド プロセスのエントリ ポイントを指定する属性を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-155">In addition to identifying the XML schema for the project file, the **Project** element can include attributes to specify the entry points for the build process.</span></span> <span data-ttu-id="603b4-156">たとえば、 [Contact Manager サンプル ソリューション](the-contact-manager-solution.md)、 *Publish.proj*という名前のターゲットを呼び出すことによって、ビルドを開始するファイルを指定**FullPublish**します。</span><span class="sxs-lookup"><span data-stu-id="603b4-156">For example, in the [Contact Manager sample solution](the-contact-manager-solution.md), the *Publish.proj* file specifies that the build should start by calling the target named **FullPublish**.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a><span data-ttu-id="603b4-157">プロパティと条件</span><span class="sxs-lookup"><span data-stu-id="603b4-157">Properties and Conditions</span></span>

<span data-ttu-id="603b4-158">通常、プロジェクト ファイルは、多数のさまざまな正常にビルドし、プロジェクトを配置するために情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="603b4-158">A project file typically needs to provide lots of different pieces of information in order to successfully build and deploy your projects.</span></span> <span data-ttu-id="603b4-159">これらの情報には、サーバー名、接続文字列、資格情報、ビルド構成、ソースと変換先ファイルのパス、およびカスタマイズをサポートするために追加するその他の情報を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-159">These pieces of information could include server names, connection strings, credentials, build configurations, source and destination file paths, and any other information you want to include to support customization.</span></span> <span data-ttu-id="603b4-160">内で、プロジェクト ファイルのプロパティを定義する必要があります、 [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx)要素。</span><span class="sxs-lookup"><span data-stu-id="603b4-160">In a project file, properties must be defined within a [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) element.</span></span> <span data-ttu-id="603b4-161">MSBuild プロパティは、キーと値のペアで構成されます。</span><span class="sxs-lookup"><span data-stu-id="603b4-161">MSBuild properties consist of key-value pairs.</span></span> <span data-ttu-id="603b4-162">内で、 **PropertyGroup**要素、要素名は、プロパティのキーを定義し、要素のコンテンツ プロパティの値を定義します。</span><span class="sxs-lookup"><span data-stu-id="603b4-162">Within the **PropertyGroup** element, the element name defines the property key and the content of the element defines the property value.</span></span> <span data-ttu-id="603b4-163">たとえば、という名前のプロパティを定義できます**ServerName**と**ConnectionString**静的サーバー名と接続文字列を格納します。</span><span class="sxs-lookup"><span data-stu-id="603b4-163">For example, you could define properties named **ServerName** and **ConnectionString** to store a static server name and connection string.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


<span data-ttu-id="603b4-164">形式を使用するプロパティの値を取得する<strong>$(</strong><em>PropertyName</em><strong>)</strong><em>.</em>たとえばの値を取得するため、 <strong>ServerName</strong>入力プロパティ。</span><span class="sxs-lookup"><span data-stu-id="603b4-164">To retrieve a property value, you use the format <strong>$(</strong><em>PropertyName</em><strong>)</strong><em>.</em>For example, to retrieve the value of the <strong>ServerName</strong> property, you would type:</span></span>


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> <span data-ttu-id="603b4-165">方法の例と使用すると、プロパティ値を使用して、このトピックの後半を確認します。</span><span class="sxs-lookup"><span data-stu-id="603b4-165">You'll see examples of how and when to use property values later in this topic.</span></span>


<span data-ttu-id="603b4-166">プロジェクト ファイルで静的なプロパティとして情報を埋め込むは常に、ビルド プロセスを管理するための理想的なアプローチです。</span><span class="sxs-lookup"><span data-stu-id="603b4-166">Embedding information as static properties in a project file is not always the ideal approach to managing the build process.</span></span> <span data-ttu-id="603b4-167">多数のシナリオでは、他のソースから情報を取得したり、コマンド プロンプトからの情報を提供するユーザーを支援します。</span><span class="sxs-lookup"><span data-stu-id="603b4-167">In a lot of scenarios, you'll want to obtain the information from other sources or empower the user to provide the information from the command prompt.</span></span> <span data-ttu-id="603b4-168">MSBuild は、コマンド ライン パラメーターとしてのプロパティの値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-168">MSBuild allows you to specify any property value as a command-line parameter.</span></span> <span data-ttu-id="603b4-169">たとえば、ユーザーが値を指定でした**ServerName**ときに実行するとき MSBuild.exe コマンドラインから。</span><span class="sxs-lookup"><span data-stu-id="603b4-169">For example, the user could provide a value for **ServerName** when he or she runs MSBuild.exe from the command line.</span></span>


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> <span data-ttu-id="603b4-170">MSBuild.exe で使用できるスイッチと引数の詳細については、次を参照してください。 [MSBuild コマンド ライン リファレンス](https://msdn.microsoft.com/library/ms164311.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="603b4-170">For more information on the arguments and switches you can use with MSBuild.exe, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>


<span data-ttu-id="603b4-171">環境変数と組み込みのプロジェクトのプロパティの値を取得するのにプロパティのと同じ構文を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-171">You can use the same property syntax to obtain the values of environment variables and built-in project properties.</span></span> <span data-ttu-id="603b4-172">一般的に使用されるプロパティの多くが、定義されているし、する使用プロジェクト ファイルで、関連するパラメーター名を含めることで。</span><span class="sxs-lookup"><span data-stu-id="603b4-172">Lots of commonly used properties are defined for you, and you can use them in your project files by including the relevant parameter name.</span></span> <span data-ttu-id="603b4-173">たとえば、現在のプロジェクト プラットフォームを取得する&#x2014;など **x86** または **AnyCpu**&#x2014;含めることができます、 **$(Platform)** プロパティ参照でプロジェクト ファイルです。</span><span class="sxs-lookup"><span data-stu-id="603b4-173">For example, to retrieve the current project platform&#x2014;for example, **x86** or **AnyCpu**&#x2014;you can include the **$(Platform)** property reference in your project file.</span></span> <span data-ttu-id="603b4-174">詳細については、次を参照してください。[ビルドのコマンドとプロパティのマクロの](https://msdn.microsoft.com/library/c02as0cs.aspx)、 [MSBuild プロジェクトの共通プロパティ](https://msdn.microsoft.com/library/bb629394.aspx)、および[の予約済みプロパティ](https://msdn.microsoft.com/library/ms164309.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="603b4-174">For more information, see [Macros for Build Commands and Properties](https://msdn.microsoft.com/library/c02as0cs.aspx), [Common MSBuild Project Properties](https://msdn.microsoft.com/library/bb629394.aspx), and [Reserved Properties](https://msdn.microsoft.com/library/ms164309.aspx).</span></span>

<span data-ttu-id="603b4-175">プロパティと組み合わせてよく*条件*します。</span><span class="sxs-lookup"><span data-stu-id="603b4-175">Properties are often used in conjunction with *conditions*.</span></span> <span data-ttu-id="603b4-176">ほとんどの MSBuild 要素をサポートして、**条件**属性には、MSBuild が要素を評価する条件を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-176">Most MSBuild elements support the **Condition** attribute, which lets you specify the criteria upon which MSBuild should evaluate the element.</span></span> <span data-ttu-id="603b4-177">たとえば、このプロパティの定義があるとします。</span><span class="sxs-lookup"><span data-stu-id="603b4-177">For example, consider this property definition:</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


<span data-ttu-id="603b4-178">MSBuild では、このプロパティの定義を処理するとき、まず確認するかどうか、 **$(OutputRoot)** プロパティの値が使用可能な。</span><span class="sxs-lookup"><span data-stu-id="603b4-178">When MSBuild processes this property definition, it first checks to see whether an **$(OutputRoot)** property value is available.</span></span> <span data-ttu-id="603b4-179">プロパティの値が空白の場合&#x2014;つまり、ユーザーは、このプロパティの値を指定していない&#x2014;条件の評価が **true** にプロパティの値を設定および **.\Publish\Out** です。条件の評価が場合は、ユーザーには、このプロパティの値が提供された、 **false**静的プロパティの値は使用されません。</span><span class="sxs-lookup"><span data-stu-id="603b4-179">If the property value is blank&#x2014;in other words, the user hasn't provided a value for this property&#x2014;the condition evaluates to **true** and the property value is set to **..\Publish\Out**. If the user has provided a value for this property, the condition evaluates to **false** and the static property value is not used.</span></span>

<span data-ttu-id="603b4-180">条件を指定するさまざまな方法の詳細については、次を参照してください。 [MSBuild の条件](https://msdn.microsoft.com/library/7szfhaft.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="603b4-180">For more information on the different ways in which you can specify conditions, see [MSBuild Conditions](https://msdn.microsoft.com/library/7szfhaft.aspx).</span></span>

### <a name="items-and-item-groups"></a><span data-ttu-id="603b4-181">項目と項目グループ</span><span class="sxs-lookup"><span data-stu-id="603b4-181">Items and Item Groups</span></span>

<span data-ttu-id="603b4-182">プロジェクト ファイルの重要な役割の 1 つは、ビルド プロセスへの入力を定義します。</span><span class="sxs-lookup"><span data-stu-id="603b4-182">One of the important roles of the project file is to define the inputs to the build process.</span></span> <span data-ttu-id="603b4-183">通常、ファイルは、これらの入力&#x2014;ファイル、構成ファイル、コマンド ファイルは、処理またはとしてコピーする必要があるその他のファイルのコード、ビルド プロセスの一部です。</span><span class="sxs-lookup"><span data-stu-id="603b4-183">Typically, these inputs are files&#x2014;code files, configuration files, command files, and any other files that you need to process or copy as part of the build process.</span></span> <span data-ttu-id="603b4-184">MSBuild プロジェクトのスキーマでこれらの入力がによって表される[項目](https://msdn.microsoft.com/library/ms164283.aspx)要素。</span><span class="sxs-lookup"><span data-stu-id="603b4-184">In the MSBuild project schema, these inputs are represented by [Item](https://msdn.microsoft.com/library/ms164283.aspx) elements.</span></span> <span data-ttu-id="603b4-185">内でプロジェクト ファイルで項目を定義する必要があります、 [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx)要素。</span><span class="sxs-lookup"><span data-stu-id="603b4-185">In a project file, items must be defined within an [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) element.</span></span> <span data-ttu-id="603b4-186">同じように**プロパティ**名前を要素、**項目**要素自由です。</span><span class="sxs-lookup"><span data-stu-id="603b4-186">Just like **Property** elements, you can name an **Item** element however you like.</span></span> <span data-ttu-id="603b4-187">ただし、指定する必要があります、 **Include**ファイルまたは項目を表すワイルドカードを識別する属性。</span><span class="sxs-lookup"><span data-stu-id="603b4-187">However, you must specify an **Include** attribute to identify the file or wildcard that the item represents.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


<span data-ttu-id="603b4-188">複数を指定することによって**項目**と同じ名前の要素を効果的に作成するリソースの名前付きのリスト。</span><span class="sxs-lookup"><span data-stu-id="603b4-188">By specifying multiple **Item** elements with the same name, you're effectively creating a named list of resources.</span></span> <span data-ttu-id="603b4-189">この動作を確認する良い方法は、Visual Studio で作成するプロジェクト ファイルのいずれかを確認します。</span><span class="sxs-lookup"><span data-stu-id="603b4-189">A good way to see this in action is to take a look inside one of the project files that Visual Studio creates.</span></span> <span data-ttu-id="603b4-190">たとえば、 *ContactManager.Mvc.csproj*サンプル ソリューション内のファイルには、それぞれに同じ名前のいくつかの項目グループの多くが含まれる**項目**要素。</span><span class="sxs-lookup"><span data-stu-id="603b4-190">For example, the *ContactManager.Mvc.csproj* file in the sample solution includes a lot of item groups, each with several identically named **Item** elements.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


<span data-ttu-id="603b4-191">この方法で、プロジェクト ファイルのと同じ方法で処理する必要があるファイルのリストを構築するために MSBuild がよう指示&#x2014;、**参照**成功したビルドの場所にする必要があるアセンブリが一覧に含まれています、 **コンパイル**リストにはコンパイルする必要があります、コード ファイルが含まれていますと**コンテンツ**一覧にそのままコピーする必要があるリソースが含まれています。</span><span class="sxs-lookup"><span data-stu-id="603b4-191">In this way, the project file is instructing MSBuild to construct lists of files that need to be processed in the same way&#x2014;the **Reference** list includes assemblies that must be in place for a successful build, the **Compile** list includes code files that must be compiled, and the **Content** list includes resources that must be copied unaltered.</span></span> <span data-ttu-id="603b4-192">ビルド プロセスの参照し、これらの項目を使用して、このトピックの後半で紹介します。</span><span class="sxs-lookup"><span data-stu-id="603b4-192">We'll look at how the build process references and uses these items later in this topic.</span></span>

<span data-ttu-id="603b4-193">項目の要素を含めることも[ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx)子要素。</span><span class="sxs-lookup"><span data-stu-id="603b4-193">Item elements can also include [ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx) child elements.</span></span> <span data-ttu-id="603b4-194">これらはユーザー定義のキー/値ペアであり、基本的にその項目に固有のプロパティを表します。</span><span class="sxs-lookup"><span data-stu-id="603b4-194">These are user-defined key-value pairs and essentially represent properties that are specific to that item.</span></span> <span data-ttu-id="603b4-195">たとえば、多くの**コンパイル**項目要素をプロジェクト ファイルには、 **DependentUpon**子要素。</span><span class="sxs-lookup"><span data-stu-id="603b4-195">For example, a lot of the **Compile** item elements in the project file include **DependentUpon** child elements.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> <span data-ttu-id="603b4-196">ユーザーが作成した項目のメタデータだけでなく、すべての項目には作成時にさまざまな一般的なメタデータが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="603b4-196">In addition to user-created item metadata, all items are assigned various common metadata on creation.</span></span> <span data-ttu-id="603b4-197">詳細については、「[既知の項目メタデータ](https://msdn.microsoft.com/library/ms164313.aspx)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="603b4-197">For more information, see [Well-known Item Metadata](https://msdn.microsoft.com/library/ms164313.aspx).</span></span>


<span data-ttu-id="603b4-198">作成することができます**ItemGroup**ルート レベル内の要素**プロジェクト**要素、または特定の**ターゲット**要素。</span><span class="sxs-lookup"><span data-stu-id="603b4-198">You can create **ItemGroup** elements within the root-level **Project** element or within specific **Target** elements.</span></span> <span data-ttu-id="603b4-199">**ItemGroup**要素もサポート**条件**属性で、プロジェクトの構成またはプラットフォームのような条件に従って、ビルド プロセスへの入力を調整することができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-199">**ItemGroup** elements also support **Condition** attributes, which lets you tailor the inputs to the build process according to conditions like the project configuration or platform.</span></span>

### <a name="targets-and-tasks"></a><span data-ttu-id="603b4-200">ターゲットとタスク</span><span class="sxs-lookup"><span data-stu-id="603b4-200">Targets and Tasks</span></span>

<span data-ttu-id="603b4-201">MSBuild のスキーマで、[タスク](https://msdn.microsoft.com/library/77f2hx1s.aspx)要素は、個々 のビルド命令 (またはタスク) を表します。</span><span class="sxs-lookup"><span data-stu-id="603b4-201">In the MSBuild schema, a [Task](https://msdn.microsoft.com/library/77f2hx1s.aspx) element represents an individual build instruction (or task).</span></span> <span data-ttu-id="603b4-202">MSBuild には、さまざまな定義済みのタスクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="603b4-202">MSBuild includes a multitude of predefined tasks.</span></span> <span data-ttu-id="603b4-203">例えば:</span><span class="sxs-lookup"><span data-stu-id="603b4-203">For example:</span></span>

- <span data-ttu-id="603b4-204">**コピー**タスクは、新しい場所にファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="603b4-204">The **Copy** task copies files to a new location.</span></span>
- <span data-ttu-id="603b4-205">**Csc**タスクは、Visual c# コンパイラを起動します。</span><span class="sxs-lookup"><span data-stu-id="603b4-205">The **Csc** task invokes the Visual C# compiler.</span></span>
- <span data-ttu-id="603b4-206">**Vbc**タスクは、Visual Basic コンパイラを起動します。</span><span class="sxs-lookup"><span data-stu-id="603b4-206">The **Vbc** task invokes the Visual Basic compiler.</span></span>
- <span data-ttu-id="603b4-207">**Exec**タスクは、指定したプログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="603b4-207">The **Exec** task runs a specified program.</span></span>
- <span data-ttu-id="603b4-208">**メッセージ**タスクでは、ロガーにメッセージを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="603b4-208">The **Message** task writes a message to a logger.</span></span>

> [!NOTE]
> <span data-ttu-id="603b4-209">すぐに使用できるタスクの詳細については、次を参照してください。 [MSBuild タスク リファレンス](https://msdn.microsoft.com/library/7z253716.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="603b4-209">For full details of the tasks that are available out of the box, see [MSBuild Task Reference](https://msdn.microsoft.com/library/7z253716.aspx).</span></span> <span data-ttu-id="603b4-210">カスタムのタスクを作成する方法など、タスクの詳細については、次を参照してください。 [MSBuild タスク](https://msdn.microsoft.com/library/ms171466.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="603b4-210">For more information on tasks, including how to create your own custom tasks, see [MSBuild Tasks](https://msdn.microsoft.com/library/ms171466.aspx).</span></span>


<span data-ttu-id="603b4-211">内のタスクを含めることが常に必要がある[ターゲット](https://msdn.microsoft.com/library/t50z2hka.aspx)要素。</span><span class="sxs-lookup"><span data-stu-id="603b4-211">Tasks must always be contained within [Target](https://msdn.microsoft.com/library/t50z2hka.aspx) elements.</span></span> <span data-ttu-id="603b4-212">A**ターゲット**要素は、一連の順番に実行される 1 つまたは複数のタスクとプロジェクト ファイルは、複数のターゲットを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-212">A **Target** element is a set of one or more tasks that are executed sequentially, and a project file can contain multiple targets.</span></span> <span data-ttu-id="603b4-213">タスクまたは一連のタスクを実行する場合は、それらを含むターゲットを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="603b4-213">When you want to run a task, or a set of tasks, you invoke the target that contains them.</span></span> <span data-ttu-id="603b4-214">たとえば、メッセージをログに記録する単純なプロジェクト ファイルがあるとします。</span><span class="sxs-lookup"><span data-stu-id="603b4-214">For example, suppose you have a simple project file that logs a message.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


<span data-ttu-id="603b4-215">使用して、コマンドラインからターゲットを呼び出すことができます、 **/t**ターゲットを指定するスイッチ。</span><span class="sxs-lookup"><span data-stu-id="603b4-215">You can invoke the target from the command line, by using the **/t** switch to specify the target.</span></span>


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


<span data-ttu-id="603b4-216">また、追加することができます、 **DefaultTargets**属性を**プロジェクト**を起動するターゲットを指定する要素。</span><span class="sxs-lookup"><span data-stu-id="603b4-216">Alternatively, you can add a **DefaultTargets** attribute to the **Project** element, to specify the targets that you want to invoke.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


<span data-ttu-id="603b4-217">この場合は、コマンドラインからターゲットを指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="603b4-217">In this case, you don't need to specify the target from the command line.</span></span> <span data-ttu-id="603b4-218">プロジェクト ファイルを指定するだけで、MSBuild が呼び出す、 **FullPublish**するターゲット。</span><span class="sxs-lookup"><span data-stu-id="603b4-218">You can simply specify the project file, and MSBuild will invoke the **FullPublish** target for you.</span></span>


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


<span data-ttu-id="603b4-219">ターゲットとタスクの両方を含めることができます**条件**属性。</span><span class="sxs-lookup"><span data-stu-id="603b4-219">Both targets and tasks can include **Condition** attributes.</span></span> <span data-ttu-id="603b4-220">そのため、特定の条件が満たされた場合に、ターゲットの全体または個々 のタスクを省略することができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-220">As such, you can choose to omit entire targets or individual tasks if certain conditions are met.</span></span>

<span data-ttu-id="603b4-221">一般に、便利なタスクとターゲットを作成するときに、プロパティと他の場所で、プロジェクト ファイルで定義した項目を参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="603b4-221">Generally speaking, when you create useful tasks and targets, you'll need to refer to the properties and items that you've defined elsewhere in the project file:</span></span>

- <span data-ttu-id="603b4-222">プロパティ値を使用する入力<strong>$(</strong><em>PropertyName</em><strong>)</strong>ここで、 <em>PropertyName</em>の名前を指定します、 <strong>プロパティ</strong>要素またはパラメーターの名前。</span><span class="sxs-lookup"><span data-stu-id="603b4-222">To use a property value, type <strong>$(</strong><em>PropertyName</em><strong>)</strong>, where <em>PropertyName</em> is the name of the <strong>Property</strong> element or the name of the parameter.</span></span>
- <span data-ttu-id="603b4-223">項目を使用する入力<strong>@(</strong><em>ItemName</em><strong>)</strong>ここで、 <em>ItemName</em>の名前を指定します、<strong>項目</strong>要素。</span><span class="sxs-lookup"><span data-stu-id="603b4-223">To use an item, type <strong>@(</strong><em>ItemName</em><strong>)</strong>, where <em>ItemName</em> is the name of the <strong>Item</strong> element.</span></span>

> [!NOTE]
> <span data-ttu-id="603b4-224">同じ名前の複数の項目を作成する場合は、一覧を構築していることを覚えておいてください。</span><span class="sxs-lookup"><span data-stu-id="603b4-224">Remember that if you create multiple items with the same name, you're building a list.</span></span> <span data-ttu-id="603b4-225">これに対し、同じ名前の複数のプロパティを作成する場合は、最後のプロパティ値を指定するプロパティが上書きされます前と同じ名前&#x2014;プロパティがのみ 1 つの値を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-225">In contrast, if you create multiple properties with the same name, the last property value you provide will overwrite any previous properties with the same name&#x2014;a property can only contain a single value.</span></span>


<span data-ttu-id="603b4-226">たとえば、 *Publish.proj*サンプル ソリューションでファイルを見て、 **BuildProjects**ターゲット。</span><span class="sxs-lookup"><span data-stu-id="603b4-226">For example, in the *Publish.proj* file in the sample solution, take a look at the **BuildProjects** target.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


<span data-ttu-id="603b4-227">このサンプルでは、次の点を確認できます。</span><span class="sxs-lookup"><span data-stu-id="603b4-227">In this sample, you can observe these key points:</span></span>

- <span data-ttu-id="603b4-228">場合、 **BuildingInTeamBuild**パラメーターが指定されておりの値を持つ**true**、このターゲット内のタスクのいずれも実行されます。</span><span class="sxs-lookup"><span data-stu-id="603b4-228">If the **BuildingInTeamBuild** parameter is specified and has a value of **true**, none of the tasks within this target will be executed.</span></span>
- <span data-ttu-id="603b4-229">ターゲットには 1 つのインスタンスが含まれています、 [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx)タスク。</span><span class="sxs-lookup"><span data-stu-id="603b4-229">The target contains a single instance of the [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) task.</span></span> <span data-ttu-id="603b4-230">このタスクでは、他の MSBuild プロジェクトをビルドすることができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-230">This task lets you build other MSBuild projects.</span></span>
- <span data-ttu-id="603b4-231">**ProjectsToBuild**項目は、タスクに渡されます。</span><span class="sxs-lookup"><span data-stu-id="603b4-231">The **ProjectsToBuild** item is passed to the task.</span></span> <span data-ttu-id="603b4-232">この項目はプロジェクトまたはソリューション ファイル、によって定義されたすべての一覧を表すことができます**ProjectsToBuild**項目の項目グループ内の要素。</span><span class="sxs-lookup"><span data-stu-id="603b4-232">This item could represent a list of project or solution files, all defined by **ProjectsToBuild** item elements within an item group.</span></span> <span data-ttu-id="603b4-233">ここで、 **ProjectsToBuild**項目が 1 つのソリューション ファイルを参照します。</span><span class="sxs-lookup"><span data-stu-id="603b4-233">In this case, the **ProjectsToBuild** item refers to a single solution file.</span></span>

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- <span data-ttu-id="603b4-234">渡されるプロパティの値、 **MSBuild**タスクという名前のパラメーターに含める**OutputRoot**と**構成**します。</span><span class="sxs-lookup"><span data-stu-id="603b4-234">The property values passed to the **MSBuild** task include parameters named **OutputRoot** and **Configuration**.</span></span> <span data-ttu-id="603b4-235">これらは、それ以外の場合に、パラメーターの値が指定された場合または静的プロパティの値に設定されます。</span><span class="sxs-lookup"><span data-stu-id="603b4-235">These are set to parameter values if they are provided, or static property values if they are not.</span></span>

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

<span data-ttu-id="603b4-236">表示することも、 **MSBuild**タスクという名前のターゲットを呼び出す**ビルド**します。</span><span class="sxs-lookup"><span data-stu-id="603b4-236">You can also see that the **MSBuild** task invokes a target named **Build**.</span></span> <span data-ttu-id="603b4-237">これは広く使用されている Visual Studio プロジェクト ファイルでは、カスタムのプロジェクト ファイルで使用できるようないくつかの組み込みのターゲットの 1 つ**ビルド**、**クリーン**、 **を再構築**、および**発行**します。</span><span class="sxs-lookup"><span data-stu-id="603b4-237">This is one of several built-in targets that are widely used in Visual Studio project files and are available to you in your custom project files, like **Build**, **Clean**, **Rebuild**, and **Publish**.</span></span> <span data-ttu-id="603b4-238">ターゲットとし、ビルド プロセスの制御やに関するタスクの使用方法の詳細を学習、 **MSBuild**タスクの具体的には、このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="603b4-238">You'll learn more about using targets and tasks to control the build process, and about the **MSBuild** task in particular, later in this topic.</span></span>

> [!NOTE]
> <span data-ttu-id="603b4-239">ターゲットの詳細については、次を参照してください。 [MSBuild ターゲット](https://msdn.microsoft.com/library/ms171462.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="603b4-239">For more information on targets, see [MSBuild Targets](https://msdn.microsoft.com/library/ms171462.aspx).</span></span>


## <a name="splitting-project-files-to-support-multiple-environments"></a><span data-ttu-id="603b4-240">複数の環境をサポートするためのプロジェクト ファイルの分割</span><span class="sxs-lookup"><span data-stu-id="603b4-240">Splitting Project Files to Support Multiple Environments</span></span>

<span data-ttu-id="603b4-241">ソリューションをテスト サーバー、ステージング プラットフォームは、運用環境など、複数の環境に配置したいとします。</span><span class="sxs-lookup"><span data-stu-id="603b4-241">Suppose you want to be able to deploy a solution to multiple environments, like test servers, staging platforms, and production environments.</span></span> <span data-ttu-id="603b4-242">これらの環境間で、構成が大幅に異なる場合があります&#x2014;だけでなくサーバー名、接続文字列、およびの観点からも資格情報、セキュリティの設定、およびその他の要因の多くの観点から可能性があります。</span><span class="sxs-lookup"><span data-stu-id="603b4-242">The configuration may vary substantially between these environments&#x2014;not just in terms of server names, connection strings, and so on, but also potentially in terms of credentials, security settings, and lots of other factors.</span></span> <span data-ttu-id="603b4-243">これを定期的に実行する必要がある場合、ターゲット環境を切り替えるたびにプロジェクト ファイル内の複数のプロパティを編集する非常に便利ではありません。</span><span class="sxs-lookup"><span data-stu-id="603b4-243">If you need to do this regularly, it's not really expedient to edit multiple properties in your project file every time you switch the target environment.</span></span> <span data-ttu-id="603b4-244">また理想的なソリューションをビルド プロセスに提供するプロパティの値の果てしないリストを必要とすることはありません。</span><span class="sxs-lookup"><span data-stu-id="603b4-244">Nor is it an ideal solution to require an endless list of property values to be provided to the build process.</span></span>

<span data-ttu-id="603b4-245">さいわい別の方法があります。</span><span class="sxs-lookup"><span data-stu-id="603b4-245">Fortunately there is an alternative.</span></span> <span data-ttu-id="603b4-246">MSBuild では、複数のプロジェクト ファイル、ビルド構成を分割することができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-246">MSBuild lets you split your build configuration across multiple project files.</span></span> <span data-ttu-id="603b4-247">動作を確認するこの、サンプルのソリューションでは、2 つのカスタム プロジェクト ファイルがあることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="603b4-247">To see how this works, in the sample solution, notice that there are two custom project files:</span></span>

- <span data-ttu-id="603b4-248">*Publish.proj*プロパティ、項目が含まれているターゲットはすべての環境に共通します。</span><span class="sxs-lookup"><span data-stu-id="603b4-248">*Publish.proj*, which contains properties, items, and targets that are common to all environments.</span></span>
- <span data-ttu-id="603b4-249">*Env Dev.proj*開発者の環境に固有のプロパティを含むです。</span><span class="sxs-lookup"><span data-stu-id="603b4-249">*Env-Dev.proj*, which contains properties that are specific to a developer environment.</span></span>

<span data-ttu-id="603b4-250">ようになりましたことに注意して、 *Publish.proj*ファイルが含まれています、[インポート](https://msdn.microsoft.com/library/92x05xfs.aspx)開始のすぐ下にある要素、**プロジェクト**タグ。</span><span class="sxs-lookup"><span data-stu-id="603b4-250">Now notice that the *Publish.proj* file includes an [Import](https://msdn.microsoft.com/library/92x05xfs.aspx) element, immediately beneath the opening **Project** tag.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


<span data-ttu-id="603b4-251">**インポート**要素を使用して、現在の MSBuild プロジェクト ファイルを別の MSBuild プロジェクト ファイルの内容をインポートします。</span><span class="sxs-lookup"><span data-stu-id="603b4-251">The **Import** element is used to import the contents of another MSBuild project file into the current MSBuild project file.</span></span> <span data-ttu-id="603b4-252">ここで、 **TargetEnvPropsFile**パラメーターをインポートするプロジェクト ファイルのファイル名を提供します。</span><span class="sxs-lookup"><span data-stu-id="603b4-252">In this case, the **TargetEnvPropsFile** parameter provides the filename of the project file you want to import.</span></span> <span data-ttu-id="603b4-253">MSBuild を実行すると、このパラメーターの値を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-253">You can provide a value for this parameter when you run MSBuild.</span></span>


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


<span data-ttu-id="603b4-254">実質的に、これは、1 つのプロジェクト ファイルに 2 つのファイルの内容をマージします。</span><span class="sxs-lookup"><span data-stu-id="603b4-254">This effectively merges the contents of the two files into a single project file.</span></span> <span data-ttu-id="603b4-255">このアプローチを使用して、汎用のビルド構成を含む 1 つのプロジェクト ファイルと環境固有のプロパティを格納している複数の補助プロジェクト ファイルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="603b4-255">Using this approach, you can create one project file containing your universal build configuration and multiple supplementary project files containing environment-specific properties.</span></span> <span data-ttu-id="603b4-256">その結果、異なるパラメーター値を持つコマンドを実行するだけできますソリューションを別の環境に配置します。</span><span class="sxs-lookup"><span data-stu-id="603b4-256">As a result, simply running a command with a different parameter value lets you deploy your solution to a different environment.</span></span>

![](understanding-the-project-file/_static/image3.png)

<span data-ttu-id="603b4-257">この方法で、プロジェクト ファイルの分割に従うことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="603b4-257">Splitting your project files in this way is a good practice to follow.</span></span> <span data-ttu-id="603b4-258">複数のプロジェクト ファイルに汎用のビルド プロパティの重複を回避しながら 1 つのコマンドを実行して複数の環境にデプロイすることができます。</span><span class="sxs-lookup"><span data-stu-id="603b4-258">It allows developers to deploy to multiple environments by running a single command, while avoiding the duplication of universal build properties across multiple project files.</span></span>

> [!NOTE]
> <span data-ttu-id="603b4-259">独自のサーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)します。</span><span class="sxs-lookup"><span data-stu-id="603b4-259">For guidance on how to customize the environment-specific project files for your own server environments, see [Configuring Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


## <a name="conclusion"></a><span data-ttu-id="603b4-260">まとめ</span><span class="sxs-lookup"><span data-stu-id="603b4-260">Conclusion</span></span>

<span data-ttu-id="603b4-261">このトピックでは、MSBuild プロジェクト ファイルの概要を提供し、ビルド プロセスを制御する独自のカスタム プロジェクト ファイルを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="603b4-261">This topic provided a general introduction to MSBuild project files and explained how you can create your own custom project files to control the build process.</span></span> <span data-ttu-id="603b4-262">ユニバーサルのビルド手順と環境固有のビルド プロパティ、ビルドし、複数の変換先にプロジェクトをデプロイするが簡単にプロジェクト ファイルに分割の概念も導入されました。</span><span class="sxs-lookup"><span data-stu-id="603b4-262">It also introduced the concept of splitting project files into universal build instructions and environment-specific build properties, to make it easy to build and deploy projects to multiple destinations.</span></span>

<span data-ttu-id="603b4-263">次のトピックでは、[ビルド プロセスを理解する](understanding-the-build-process.md)複雑さのレベルが現実的なソリューションのデプロイを検討するコントロールのビルドおよび配置するプロジェクト ファイルを使用する方法の詳細について洞察を提供します。</span><span class="sxs-lookup"><span data-stu-id="603b4-263">The next topic, [Understanding the Build Process](understanding-the-build-process.md), provides more insight into how you can use project files to control build and deployment by walking you through the deployment of a solution with a realistic level of complexity.</span></span>

## <a name="further-reading"></a><span data-ttu-id="603b4-264">関連項目</span><span class="sxs-lookup"><span data-stu-id="603b4-264">Further Reading</span></span>

<span data-ttu-id="603b4-265">プロジェクト ファイルと、WPP をより詳細な概要については、次を参照してください。[内で、Microsoft Build Engine: Using MSBuild and Team Foundation ビルド](http://amzn.com/0735645248)Sayed Ibrahim Hashimi、William Bartholomew、ISBN: 978-0-7356-4524-0。</span><span class="sxs-lookup"><span data-stu-id="603b4-265">For a more in-depth introduction to project files and the WPP, see [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://amzn.com/0735645248) by Sayed Ibrahim Hashimi and William Bartholomew, ISBN: 978-0-7356-4524-0.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="603b4-266">[前へ](setting-up-the-contact-manager-solution.md)
> [次へ](understanding-the-build-process.md)</span><span class="sxs-lookup"><span data-stu-id="603b4-266">[Previous](setting-up-the-contact-manager-solution.md)
[Next](understanding-the-build-process.md)</span></span>
