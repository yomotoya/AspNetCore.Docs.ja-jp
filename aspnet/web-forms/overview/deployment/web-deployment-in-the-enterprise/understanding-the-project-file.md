---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: "プロジェクト ファイルを理解する |Microsoft ドキュメント"
author: jrjlee
description: "Microsoft Build Engine (MSBuild) プロジェクト ファイルは、ビルドおよび配置プロセスの中核にあります。 このトピックは、MSBuild の概要を概念的に開始しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 09c3793e9cdddb7c42cf966f2d079245f441540c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="understanding-the-project-file"></a><span data-ttu-id="4ca66-104">プロジェクト ファイルを理解します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-104">Understanding the Project File</span></span>
====================
<span data-ttu-id="4ca66-105">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="4ca66-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="4ca66-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="4ca66-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="4ca66-107">Microsoft Build Engine (MSBuild) プロジェクト ファイルは、ビルドおよび配置プロセスの中核にあります。</span><span class="sxs-lookup"><span data-stu-id="4ca66-107">Microsoft Build Engine (MSBuild) project files lie at the heart of the build and deployment process.</span></span> <span data-ttu-id="4ca66-108">このトピックでは、MSBuild とプロジェクト ファイルの概念的な概要を開始します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-108">This topic starts with a conceptual overview of MSBuild and the project file.</span></span> <span data-ttu-id="4ca66-109">これには、プロジェクト ファイルを使用して、プロジェクト ファイルを使用して、実際のアプリケーションを展開する方法の例を使用して動作時に遭遇したが、主要なコンポーネントについて説明します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-109">It describes the key components you'll come across when you work with project files, and it works through an example of how you can use project files to deploy real-world applications.</span></span>
> 
> <span data-ttu-id="4ca66-110">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="4ca66-110">What you'll learn:</span></span>
> 
> - <span data-ttu-id="4ca66-111">どの MSBuild は MSBuild プロジェクト ファイルを使用して、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="4ca66-111">How MSBuild uses MSBuild project files to build projects.</span></span>
> - <span data-ttu-id="4ca66-112">MSBuild と統合する方法 (Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツールと同様に、展開テクノロジです。</span><span class="sxs-lookup"><span data-stu-id="4ca66-112">How MSBuild integrates with deployment technologies, like the Internet Information Services (IIS) Web Deployment Tool (Web Deploy).</span></span>
> - <span data-ttu-id="4ca66-113">プロジェクト ファイルの主要なコンポーネントを理解する方法です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-113">How to understand the key components of a project file.</span></span>
> - <span data-ttu-id="4ca66-114">方法をビルドして、複雑なアプリケーションを配置するプロジェクト ファイルを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-114">How you can use project files to build and deploy complex applications.</span></span>


## <a name="msbuild-and-the-project-file"></a><span data-ttu-id="4ca66-115">MSBuild とプロジェクト ファイル</span><span class="sxs-lookup"><span data-stu-id="4ca66-115">MSBuild and the Project File</span></span>

<span data-ttu-id="4ca66-116">作成して、Visual Studio でソリューションをビルドしたときに Visual Studio は MSBuild を使用して、ソリューション内の各プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="4ca66-116">When you create and build solutions in Visual Studio, Visual Studio uses MSBuild to build each project in your solution.</span></span> <span data-ttu-id="4ca66-117">すべての Visual Studio プロジェクトには、プロジェクト & #x 2014 の種類を反映するファイル拡張子を持つ、MSBuild プロジェクト ファイルです。 たとえば、c# プロジェクト (.csproj)、Visual Basic.NET プロジェクト (.vbproj) またはデータベース プロジェクト (.dbproj) が含まれています。</span><span class="sxs-lookup"><span data-stu-id="4ca66-117">Every Visual Studio project includes an MSBuild project file, with a file extension that reflects the type of project&#x2014;for example, a C# project (.csproj), a Visual Basic.NET project (.vbproj), or a database project (.dbproj).</span></span> <span data-ttu-id="4ca66-118">プロジェクトをビルドするために MSBuild がプロジェクトに関連付けられているプロジェクト ファイルを処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4ca66-118">In order to build a project, MSBuild must process the project file associated with the project.</span></span> <span data-ttu-id="4ca66-119">プロジェクト ファイルは、すべての情報と MSBuild は、コンテンツを含めるには、プラットフォームの要件、バージョン管理情報、web サーバーまたはデータベース サーバーの設定と同様に、プロジェクトをビルドするために必要がある命令を含む XML ドキュメントとタスクを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4ca66-119">The project file is an XML document that contains all the information and instructions that MSBuild needs in order to build your project, like the content to include, the platform requirements, versioning information, web server or database server settings, and the tasks that must be performed.</span></span>

<span data-ttu-id="4ca66-120">MSBuild プロジェクト ファイルがに基づいて、 [MSBuild XML スキーマ](https://msdn.microsoft.com/library/5dy88c2e.aspx)、その結果、ビルド プロセスは完全にオープンで透過的なとします。</span><span class="sxs-lookup"><span data-stu-id="4ca66-120">MSBuild project files are based on the [MSBuild XML schema](https://msdn.microsoft.com/library/5dy88c2e.aspx), and as a result the build process is entirely open and transparent.</span></span> <span data-ttu-id="4ca66-121">さらに、MSBuild エンジン & #x 2014 を使用するために Visual Studio をインストールする必要はありません以外の場合は、MSBuild.exe の実行可能ファイルは、.NET Framework の一部と、コマンド プロンプトから実行することができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-121">In addition, you don't need to install Visual Studio in order to use the MSBuild engine&#x2014;the MSBuild.exe executable is part of the .NET Framework, and you can run it from a command prompt.</span></span> <span data-ttu-id="4ca66-122">開発者には、プロジェクトをビルドおよび配置する方法を高度な粒度の細かい制御を強制する MSBuild XML スキーマを使用して、自分の MSBuild プロジェクト ファイル作成できます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-122">As a developer, you can craft your own MSBuild project files, using the MSBuild XML schema, to impose sophisticated and fine-grained control over how your projects are built and deployed.</span></span> <span data-ttu-id="4ca66-123">これらのカスタム プロジェクト ファイルは、Visual Studio を自動的に生成されるプロジェクト ファイルとまったく同じ方法で動作します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-123">These custom project files work in exactly the same way as the project files that Visual Studio generates automatically.</span></span>

> [!NOTE]
> <span data-ttu-id="4ca66-124">チーム ビルド サービスでは、Team Foundation Server (TFS) と MSBuild プロジェクト ファイルを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-124">You can also use MSBuild project files with the Team Build service in Team Foundation Server (TFS).</span></span> <span data-ttu-id="4ca66-125">たとえば、継続的インテグレーション (CI) シナリオでのプロジェクト ファイルを使用して、新しいコードをチェックインするときに、テスト環境に展開を自動化することができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-125">For example, you can use project files in continuous integration (CI) scenarios to automate deployment to a test environment when new code is checked in.</span></span> <span data-ttu-id="4ca66-126">詳細については、次を参照してください。 [Web 配置の自動化の Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-126">For more information, see [Configuring Team Foundation Server for Automated Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span></span>


### <a name="project-file-naming-conventions"></a><span data-ttu-id="4ca66-127">プロジェクト ファイルの名前付け規則</span><span class="sxs-lookup"><span data-stu-id="4ca66-127">Project File Naming Conventions</span></span>

<span data-ttu-id="4ca66-128">プロジェクト ファイルを作成するときに、あらゆるファイル拡張子を使用できます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-128">When you create your own project files, you can use any file extension you like.</span></span> <span data-ttu-id="4ca66-129">ただし、ソリューションを他のユーザーが理解しやすくするには、これらの共通の規則を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4ca66-129">However, to make your solutions easier for others to understand, you should use these common conventions:</span></span>

- <span data-ttu-id="4ca66-130">プロジェクトをビルドするプロジェクト ファイルを作成するときは、.proj 拡張機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-130">Use the .proj extension when you create a project file that builds projects.</span></span>
- <span data-ttu-id="4ca66-131">他のプロジェクト ファイルにインポートする再利用可能なプロジェクト ファイルを作成するときは、.targets 拡張機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-131">Use the .targets extension when you create a reusable project file to import into other project files.</span></span> <span data-ttu-id="4ca66-132">.Targets 拡張子がファイルを通常しないビルド何も自体、単に、.proj ファイルにインポートする手順が含まれます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-132">Files with a .targets extension typically don't build anything themselves, they simply contain instructions that you can import into your .proj files.</span></span>

### <a name="integration-with-deployment-technologies"></a><span data-ttu-id="4ca66-133">展開のテクノロジとの統合</span><span class="sxs-lookup"><span data-stu-id="4ca66-133">Integration with Deployment Technologies</span></span>

<span data-ttu-id="4ca66-134">ASP.NET web アプリケーションと ASP.NET MVC web アプリケーションと同様に、Visual Studio 2010 で web アプリケーション プロジェクトで作業した場合、これらのプロジェクトがパッケージ化と配置をターゲット環境に web アプリケーションの組み込みサポートを含めることがわかります。</span><span class="sxs-lookup"><span data-stu-id="4ca66-134">If you've worked with web application projects in Visual Studio 2010, like ASP.NET web applications and ASP.NET MVC web applications, you'll know that these projects include built-in support for packaging and deploying the web application to a target environment.</span></span> <span data-ttu-id="4ca66-135">**プロパティ**ページをこれらのプロジェクトに含まれている**パッケージ化/発行 Web**と**パッケージ化/発行 SQL**タブを構成するのに使用できる方法のコンポーネント、アプリケーションをパッケージ化され、展開します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-135">The **Properties** pages for these projects include **Package/Publish Web** and **Package/Publish SQL** tabs that you can use to configure how the components of your application are packaged and deployed.</span></span> <span data-ttu-id="4ca66-136">これを示しています、**パッケージ化/発行 Web**  タブ。</span><span class="sxs-lookup"><span data-stu-id="4ca66-136">This shows the **Package/Publish Web** tab:</span></span>

![](understanding-the-project-file/_static/image1.png)

<span data-ttu-id="4ca66-137">これらの機能の背後にある、基になるテクノロジは、Web 発行パイプライン (WPP) と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-137">The underlying technology behind these capabilities is known as the Web Publishing Pipeline (WPP).</span></span> <span data-ttu-id="4ca66-138">WPP は基本的に MSBuild を表示および[Web Deploy](https://go.microsoft.com/?linkid=9805122) web アプリケーションの完全なビルド、パッケージ、および展開プロセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-138">The WPP essentially brings MSBuild and [Web Deploy](https://go.microsoft.com/?linkid=9805122) together to provide a complete build, package, and deployment process for your web applications.</span></span>

<span data-ttu-id="4ca66-139">良いニュースは、web プロジェクト用のカスタム プロジェクト ファイルを作成するときに、WPP が提供するための統合ポイントを活用を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-139">The good news is that you can take advantage of the integration points that the WPP provides when you create custom project files for web projects.</span></span> <span data-ttu-id="4ca66-140">プロジェクトをビルド、web 配置パッケージを作成し、1 つのプロジェクト ファイルと MSBuild に 1 回の呼び出しで、リモート サーバーでこれらのパッケージをインストールすることができるプロジェクト ファイルで展開する手順を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-140">You can include deployment instructions in your project file, which allows you to build your projects, create web deployment packages, and install these packages on remote servers through a single project file and a single call to MSBuild.</span></span> <span data-ttu-id="4ca66-141">ビルド プロセスの一部として、その他の実行可能ファイルを呼び出すこともできます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-141">You can also call any other executables as part of your build process.</span></span> <span data-ttu-id="4ca66-142">たとえば、スキーマ ファイルからデータベースを配置する VSDBCMD.exe コマンド ライン ツールを実行できます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-142">For example, you can run the VSDBCMD.exe command-line tool to deploy a database from a schema file.</span></span> <span data-ttu-id="4ca66-143">このトピックの過程で、これらの機能をエンタープライズ展開シナリオの要件を満たすために行える方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-143">Over the course of this topic, you'll see how you can take advantage of these capabilities to meet the requirements of your enterprise deployment scenarios.</span></span>

> [!NOTE]
> <span data-ttu-id="4ca66-144">Web アプリケーションの展開プロセスのしくみの詳細については、次を参照してください。 [ASP.NET Web アプリケーション プロジェクトの展開の概要](https://msdn.microsoft.com/library/dd394698.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-144">For more information on how the web application deployment process works, see [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698.aspx).</span></span>


## <a name="the-anatomy-of-a-project-file"></a><span data-ttu-id="4ca66-145">プロジェクト ファイルの構造は、</span><span class="sxs-lookup"><span data-stu-id="4ca66-145">The Anatomy of a Project File</span></span>

<span data-ttu-id="4ca66-146">ビルド プロセスの詳細を見ると、前に、MSBuild プロジェクト ファイルの基本的な構造を理解するには数分をかけて価値です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-146">Before you look at the build process in more detail, it's worth taking a few moments to familiarize yourself with the basic structure of an MSBuild project file.</span></span> <span data-ttu-id="4ca66-147">このセクションでは、確認、編集、またはプロジェクト ファイルを作成するときに発生する一般的な要素の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-147">This section provides an overview of the more common elements that you'll encounter when you review, edit, or create a project file.</span></span> <span data-ttu-id="4ca66-148">具体的には、学習します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-148">In particular, you'll learn:</span></span>

- <span data-ttu-id="4ca66-149">使用する*プロパティ*のビルド プロセスの変数を管理します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-149">How to use *properties* to manage variables for the build process.</span></span>
- <span data-ttu-id="4ca66-150">使用する*項目*など、ビルド処理への入力を識別するためのコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="4ca66-150">How to use *items* to identify the inputs to the build process, like code files.</span></span>
- <span data-ttu-id="4ca66-151">使用する方法*ターゲット*と*タスク*指示を提供する実行、msbuild を使用して*プロパティ*と*項目*で定義されています。プロジェクト ファイルです。</span><span class="sxs-lookup"><span data-stu-id="4ca66-151">How to use *targets* and *tasks* to provide execution instructions to MSBuild, using *properties* and *items* defined elsewhere in the project file.</span></span>

<span data-ttu-id="4ca66-152">これは、MSBuild プロジェクト ファイルの主な要素間の関係を示しています。</span><span class="sxs-lookup"><span data-stu-id="4ca66-152">This shows the relationship between the key elements in an MSBuild project file:</span></span>

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a><span data-ttu-id="4ca66-153">Project 要素</span><span class="sxs-lookup"><span data-stu-id="4ca66-153">The Project Element</span></span>

<span data-ttu-id="4ca66-154">[プロジェクト](https://msdn.microsoft.com/library/bcxfsh87.aspx)要素は、すべてのプロジェクト ファイルのルート要素です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-154">The [Project](https://msdn.microsoft.com/library/bcxfsh87.aspx) element is the root element of every project file.</span></span> <span data-ttu-id="4ca66-155">プロジェクト ファイルの XML スキーマを識別するだけでなく、**プロジェクト**要素は、ビルド プロセスのエントリ ポイントを指定する属性を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-155">In addition to identifying the XML schema for the project file, the **Project** element can include attributes to specify the entry points for the build process.</span></span> <span data-ttu-id="4ca66-156">たとえば、[連絡先のマネージャーのサンプル ソリューション](the-contact-manager-solution.md)、 *Publish.proj*という名前のターゲットを呼び出すことにより、ビルドを開始するファイルを指定**FullPublish**です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-156">For example, in the [Contact Manager sample solution](the-contact-manager-solution.md), the *Publish.proj* file specifies that the build should start by calling the target named **FullPublish**.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a><span data-ttu-id="4ca66-157">プロパティと条件</span><span class="sxs-lookup"><span data-stu-id="4ca66-157">Properties and Conditions</span></span>

<span data-ttu-id="4ca66-158">通常、プロジェクト ファイルは、多数のさまざまなが正常にビルドし、プロジェクトを配置するために情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4ca66-158">A project file typically needs to provide lots of different pieces of information in order to successfully build and deploy your projects.</span></span> <span data-ttu-id="4ca66-159">これらの種類の情報には、サーバー名、接続文字列、資格情報、ビルド構成、ソースと変換先ファイルのパス、およびカスタマイズをサポートするために追加するその他の情報を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-159">These pieces of information could include server names, connection strings, credentials, build configurations, source and destination file paths, and any other information you want to include to support customization.</span></span> <span data-ttu-id="4ca66-160">内では、プロジェクト ファイルのプロパティを定義する必要があります、 [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx)要素。</span><span class="sxs-lookup"><span data-stu-id="4ca66-160">In a project file, properties must be defined within a [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) element.</span></span> <span data-ttu-id="4ca66-161">MSBuild プロパティは、キーと値のペアで構成されます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-161">MSBuild properties consist of key-value pairs.</span></span> <span data-ttu-id="4ca66-162">内で、 **PropertyGroup**要素、要素名プロパティのキーと、要素の内容がプロパティの値を定義します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-162">Within the **PropertyGroup** element, the element name defines the property key and the content of the element defines the property value.</span></span> <span data-ttu-id="4ca66-163">たとえば、という名前のプロパティを定義する**ServerName**と**ConnectionString**静的サーバー名と接続文字列を保存します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-163">For example, you could define properties named **ServerName** and **ConnectionString** to store a static server name and connection string.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


<span data-ttu-id="4ca66-164">プロパティ値を取得するには、形式が使用します **$(***PropertyName***) * * *。* 。たとえばの値を取得するため、 **ServerName**プロパティを入力します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-164">To retrieve a property value, you use the format **$(***PropertyName***)***.*For example, to retrieve the value of the **ServerName** property, you would type:</span></span>


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> <span data-ttu-id="4ca66-165">プロパティ値を使用して、このトピックで後述する方法の例とが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-165">You'll see examples of how and when to use property values later in this topic.</span></span>


<span data-ttu-id="4ca66-166">プロジェクト ファイルで静的なプロパティとして情報の埋め込みは常に、ビルド プロセスを管理するための最適なアプローチです。</span><span class="sxs-lookup"><span data-stu-id="4ca66-166">Embedding information as static properties in a project file is not always the ideal approach to managing the build process.</span></span> <span data-ttu-id="4ca66-167">多数のシナリオでは、他のソースから情報を取得するか、コマンド プロンプトからの情報を提供するユーザーを支援するします。</span><span class="sxs-lookup"><span data-stu-id="4ca66-167">In a lot of scenarios, you'll want to obtain the information from other sources or empower the user to provide the information from the command prompt.</span></span> <span data-ttu-id="4ca66-168">MSBuild では、コマンド ライン パラメーターとしてのプロパティの値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-168">MSBuild allows you to specify any property value as a command-line parameter.</span></span> <span data-ttu-id="4ca66-169">たとえば、ユーザーがの値を指定できます**ServerName**ときに実行する MSBuild.exe コマンドラインからです。</span><span class="sxs-lookup"><span data-stu-id="4ca66-169">For example, the user could provide a value for **ServerName** when he or she runs MSBuild.exe from the command line.</span></span>


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> <span data-ttu-id="4ca66-170">MSBuild.exe で使用できるスイッチと引数の詳細については、次を参照してください。 [MSBuild コマンド ライン リファレンス](https://msdn.microsoft.com/library/ms164311.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-170">For more information on the arguments and switches you can use with MSBuild.exe, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>


<span data-ttu-id="4ca66-171">環境変数と組み込みのプロジェクトのプロパティの値を取得するのにプロパティと同じ構文を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-171">You can use the same property syntax to obtain the values of environment variables and built-in project properties.</span></span> <span data-ttu-id="4ca66-172">多数のよく使用されるプロパティが定義した、およびできますして使用する、プロジェクト ファイルに関連するパラメーター名を含むです。</span><span class="sxs-lookup"><span data-stu-id="4ca66-172">Lots of commonly used properties are defined for you, and you can use them in your project files by including the relevant parameter name.</span></span> <span data-ttu-id="4ca66-173">例については、現在のプロジェクト プラットフォーム & #x 2014; を取得するなど、 **x86**または**AnyCpu**& #x 2014; 含めることができます、 **$(Platform)**プロパティ参照でプロジェクト ファイルです。</span><span class="sxs-lookup"><span data-stu-id="4ca66-173">For example, to retrieve the current project platform&#x2014;for example, **x86** or **AnyCpu**&#x2014;you can include the **$(Platform)** property reference in your project file.</span></span> <span data-ttu-id="4ca66-174">詳細については、次を参照してください。[のビルドのコマンドとプロパティのマクロ](https://msdn.microsoft.com/library/c02as0cs.aspx)、 [MSBuild プロジェクトの共通プロパティ](https://msdn.microsoft.com/library/bb629394.aspx)、および[予約済みプロパティ](https://msdn.microsoft.com/library/ms164309.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-174">For more information, see [Macros for Build Commands and Properties](https://msdn.microsoft.com/library/c02as0cs.aspx), [Common MSBuild Project Properties](https://msdn.microsoft.com/library/bb629394.aspx), and [Reserved Properties](https://msdn.microsoft.com/library/ms164309.aspx).</span></span>

<span data-ttu-id="4ca66-175">プロパティはと共によく使用*条件*です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-175">Properties are often used in conjunction with *conditions*.</span></span> <span data-ttu-id="4ca66-176">MSBuild のほとんどの要素をサポート、**条件**属性は、MSBuild 基になる要素を評価する条件を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-176">Most MSBuild elements support the **Condition** attribute, which lets you specify the criteria upon which MSBuild should evaluate the element.</span></span> <span data-ttu-id="4ca66-177">たとえば、このプロパティの定義があるとします。</span><span class="sxs-lookup"><span data-stu-id="4ca66-177">For example, consider this property definition:</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


<span data-ttu-id="4ca66-178">MSBuild では、このプロパティの定義を処理するとき、まず確認するかどうか、 **$(OutputRoot)**プロパティの値が使用可能な。</span><span class="sxs-lookup"><span data-stu-id="4ca66-178">When MSBuild processes this property definition, it first checks to see whether an **$(OutputRoot)** property value is available.</span></span> <span data-ttu-id="4ca66-179">プロパティの値は空白 & #x 2014; 場合つまり、ユーザーいない; 指定された値のこのプロパティ & #x 2014 条件の評価が**true**にプロパティの値を設定および**.\Publish\Out**です。条件の評価が場合は、ユーザーは、このプロパティの値を指定するが、 **false**静的プロパティの値が使用されていません。</span><span class="sxs-lookup"><span data-stu-id="4ca66-179">If the property value is blank&#x2014;in other words, the user hasn't provided a value for this property&#x2014;the condition evaluates to **true** and the property value is set to **..\Publish\Out**. If the user has provided a value for this property, the condition evaluates to **false** and the static property value is not used.</span></span>

<span data-ttu-id="4ca66-180">条件を指定するさまざまな方法の詳細については、次を参照してください。 [MSBuild の条件](https://msdn.microsoft.com/library/7szfhaft.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-180">For more information on the different ways in which you can specify conditions, see [MSBuild Conditions](https://msdn.microsoft.com/library/7szfhaft.aspx).</span></span>

### <a name="items-and-item-groups"></a><span data-ttu-id="4ca66-181">アイテムとアイテム グループ</span><span class="sxs-lookup"><span data-stu-id="4ca66-181">Items and Item Groups</span></span>

<span data-ttu-id="4ca66-182">プロジェクト ファイルの重要な役割の 1 つは、ビルド処理への入力を定義します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-182">One of the important roles of the project file is to define the inputs to the build process.</span></span> <span data-ttu-id="4ca66-183">通常、これらの入力ファイル & #x 2014 ですコード ファイル、構成ファイル、コマンド ファイルは、およびその他のファイルとしてコピーまたは処理する必要のある一部のビルド プロセスのです。</span><span class="sxs-lookup"><span data-stu-id="4ca66-183">Typically, these inputs are files&#x2014;code files, configuration files, command files, and any other files that you need to process or copy as part of the build process.</span></span> <span data-ttu-id="4ca66-184">MSBuild プロジェクト スキーマでこれらの入力がによって表される[項目](https://msdn.microsoft.com/library/ms164283.aspx)要素。</span><span class="sxs-lookup"><span data-stu-id="4ca66-184">In the MSBuild project schema, these inputs are represented by [Item](https://msdn.microsoft.com/library/ms164283.aspx) elements.</span></span> <span data-ttu-id="4ca66-185">内で、プロジェクト ファイルの項目を定義する必要があります、 [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx)要素。</span><span class="sxs-lookup"><span data-stu-id="4ca66-185">In a project file, items must be defined within an [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) element.</span></span> <span data-ttu-id="4ca66-186">同じように**プロパティ**要素、名前を指定できます、**項目**要素などのただしです。</span><span class="sxs-lookup"><span data-stu-id="4ca66-186">Just like **Property** elements, you can name an **Item** element however you like.</span></span> <span data-ttu-id="4ca66-187">ただし、指定する必要があります、 **Include**属性をファイルまたは項目を表すワイルドカードを指定します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-187">However, you must specify an **Include** attribute to identify the file or wildcard that the item represents.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


<span data-ttu-id="4ca66-188">倍数を指定することによって**項目**と同じ名前の要素、効果的に作成するリソースの名前付きリスト。</span><span class="sxs-lookup"><span data-stu-id="4ca66-188">By specifying multiple **Item** elements with the same name, you're effectively creating a named list of resources.</span></span> <span data-ttu-id="4ca66-189">Visual Studio によって作成されるプロジェクト ファイルの 1 つの内部情報を確認するは、この動作を確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="4ca66-189">A good way to see this in action is to take a look inside one of the project files that Visual Studio creates.</span></span> <span data-ttu-id="4ca66-190">たとえば、 *ContactManager.Mvc.csproj*サンプル ソリューション内のファイルには、それぞれに同じ名前の複数の項目グループの多くが含まれる**項目**要素。</span><span class="sxs-lookup"><span data-stu-id="4ca66-190">For example, the *ContactManager.Mvc.csproj* file in the sample solution includes a lot of item groups, each with several identically named **Item** elements.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


<span data-ttu-id="4ca66-191">この方法で、プロジェクト ファイルが、同じ方法 & #x 2014; で処理する必要のあるファイルのリストを構築するために MSBuild を指示は、**参照**一覧には、ビルドの成功をのためにする必要があるアセンブリが含まれています**。コンパイル**一覧には、コンパイルする必要があるコード ファイルが含まれています。 および**コンテンツ**一覧には、変更せずにコピーする必要があるリソースが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-191">In this way, the project file is instructing MSBuild to construct lists of files that need to be processed in the same way&#x2014;the **Reference** list includes assemblies that must be in place for a successful build, the **Compile** list includes code files that must be compiled, and the **Content** list includes resources that must be copied unaltered.</span></span> <span data-ttu-id="4ca66-192">ビルド プロセスの参照し、これらの項目を使用して、このトピックの後半に紹介します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-192">We'll look at how the build process references and uses these items later in this topic.</span></span>

<span data-ttu-id="4ca66-193">項目の要素を含めることも[ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx)子要素です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-193">Item elements can also include [ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx) child elements.</span></span> <span data-ttu-id="4ca66-194">これらはユーザー定義のキー/値ペアであり、本質的にそのアイテムに固有のプロパティを表します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-194">These are user-defined key-value pairs and essentially represent properties that are specific to that item.</span></span> <span data-ttu-id="4ca66-195">たとえば、多数の**コンパイル**項目要素をプロジェクト ファイルには、 **DependentUpon**子要素です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-195">For example, a lot of the **Compile** item elements in the project file include **DependentUpon** child elements.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> <span data-ttu-id="4ca66-196">ユーザーが作成した項目のメタデータの他のすべての項目は作成時にさまざまな一般的なメタデータに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-196">In addition to user-created item metadata, all items are assigned various common metadata on creation.</span></span> <span data-ttu-id="4ca66-197">詳細については、「[既知の項目メタデータ](https://msdn.microsoft.com/library/ms164313.aspx)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4ca66-197">For more information, see [Well-known Item Metadata](https://msdn.microsoft.com/library/ms164313.aspx).</span></span>


<span data-ttu-id="4ca66-198">作成することができます**ItemGroup**ルート レベル内の要素**プロジェクト**要素または固有の仕様内にある**ターゲット**要素。</span><span class="sxs-lookup"><span data-stu-id="4ca66-198">You can create **ItemGroup** elements within the root-level **Project** element or within specific **Target** elements.</span></span> <span data-ttu-id="4ca66-199">**ItemGroup**要素もサポート**条件**属性で、プロジェクトの構成やプラットフォームのような条件に従って、ビルド処理への入力を調整することができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-199">**ItemGroup** elements also support **Condition** attributes, which lets you tailor the inputs to the build process according to conditions like the project configuration or platform.</span></span>

### <a name="targets-and-tasks"></a><span data-ttu-id="4ca66-200">ターゲットとタスク</span><span class="sxs-lookup"><span data-stu-id="4ca66-200">Targets and Tasks</span></span>

<span data-ttu-id="4ca66-201">MSBuild スキーマで、[タスク](https://msdn.microsoft.com/library/77f2hx1s.aspx)要素は、個々 のビルド命令 (またはタスク) を表します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-201">In the MSBuild schema, a [Task](https://msdn.microsoft.com/library/77f2hx1s.aspx) element represents an individual build instruction (or task).</span></span> <span data-ttu-id="4ca66-202">MSBuild には、さまざまな定義済みのタスクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4ca66-202">MSBuild includes a multitude of predefined tasks.</span></span> <span data-ttu-id="4ca66-203">例:</span><span class="sxs-lookup"><span data-stu-id="4ca66-203">For example:</span></span>

- <span data-ttu-id="4ca66-204">**コピー**タスクは、新しい場所にファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="4ca66-204">The **Copy** task copies files to a new location.</span></span>
- <span data-ttu-id="4ca66-205">**Csc**タスクは、Visual c# コンパイラを起動します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-205">The **Csc** task invokes the Visual C# compiler.</span></span>
- <span data-ttu-id="4ca66-206">**Vbc**タスクは、Visual Basic コンパイラを起動します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-206">The **Vbc** task invokes the Visual Basic compiler.</span></span>
- <span data-ttu-id="4ca66-207">**Exec**タスクが、指定したプログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-207">The **Exec** task runs a specified program.</span></span>
- <span data-ttu-id="4ca66-208">**メッセージ**タスクは、ロガーにメッセージを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-208">The **Message** task writes a message to a logger.</span></span>

> [!NOTE]
> <span data-ttu-id="4ca66-209">すぐに使用できますが、タスクの詳細については、次を参照してください。 [MSBuild タスク リファレンス](https://msdn.microsoft.com/library/7z253716.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-209">For full details of the tasks that are available out of the box, see [MSBuild Task Reference](https://msdn.microsoft.com/library/7z253716.aspx).</span></span> <span data-ttu-id="4ca66-210">独自のカスタム タスクを作成する方法などのタスクの詳細については、次を参照してください。 [MSBuild タスク](https://msdn.microsoft.com/library/ms171466.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-210">For more information on tasks, including how to create your own custom tasks, see [MSBuild Tasks](https://msdn.microsoft.com/library/ms171466.aspx).</span></span>


<span data-ttu-id="4ca66-211">内のタスクを含めることが常に必要があります[ターゲット](https://msdn.microsoft.com/library/t50z2hka.aspx)要素。</span><span class="sxs-lookup"><span data-stu-id="4ca66-211">Tasks must always be contained within [Target](https://msdn.microsoft.com/library/t50z2hka.aspx) elements.</span></span> <span data-ttu-id="4ca66-212">A**ターゲット**要素は、順番に実行される 1 つまたは複数のタスクのセットと、プロジェクト ファイルは、複数のターゲットを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-212">A **Target** element is a set of one or more tasks that are executed sequentially, and a project file can contain multiple targets.</span></span> <span data-ttu-id="4ca66-213">タスク、または一連のタスクを実行する場合は、それらを含むターゲットを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-213">When you want to run a task, or a set of tasks, you invoke the target that contains them.</span></span> <span data-ttu-id="4ca66-214">たとえば、メッセージ ログに記録する単純なプロジェクト ファイルがあるとします。</span><span class="sxs-lookup"><span data-stu-id="4ca66-214">For example, suppose you have a simple project file that logs a message.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


<span data-ttu-id="4ca66-215">使用して、コマンドラインからターゲットを呼び出すことができます、 **/t**ターゲットを指定するスイッチです。</span><span class="sxs-lookup"><span data-stu-id="4ca66-215">You can invoke the target from the command line, by using the **/t** switch to specify the target.</span></span>


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


<span data-ttu-id="4ca66-216">また、追加することができます、 **DefaultTargets**属性を**プロジェクト**要素は、呼び出し先のターゲットを指定します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-216">Alternatively, you can add a **DefaultTargets** attribute to the **Project** element, to specify the targets that you want to invoke.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


<span data-ttu-id="4ca66-217">この場合、コマンドラインからターゲットを指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="4ca66-217">In this case, you don't need to specify the target from the command line.</span></span> <span data-ttu-id="4ca66-218">プロジェクト ファイルは、単を指定して MSBuild を呼び出す、 **FullPublish**するターゲット。</span><span class="sxs-lookup"><span data-stu-id="4ca66-218">You can simply specify the project file, and MSBuild will invoke the **FullPublish** target for you.</span></span>


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


<span data-ttu-id="4ca66-219">ターゲットとタスクの両方を含めることができます**条件**属性。</span><span class="sxs-lookup"><span data-stu-id="4ca66-219">Both targets and tasks can include **Condition** attributes.</span></span> <span data-ttu-id="4ca66-220">そのため、特定の条件が満たされた場合に、ターゲットの全体または個々 のタスクを省略することもできます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-220">As such, you can choose to omit entire targets or individual tasks if certain conditions are met.</span></span>

<span data-ttu-id="4ca66-221">一般に、便利なタスクとターゲットを作成するときに、プロパティと、プロジェクト ファイルに他の場所で定義した項目を参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4ca66-221">Generally speaking, when you create useful tasks and targets, you'll need to refer to the properties and items that you've defined elsewhere in the project file:</span></span>

- <span data-ttu-id="4ca66-222">プロパティ値を使用する入力**$(***PropertyName***)**ここで、 *PropertyName*の名前を指定、**プロパティ**要素またはの名前、パラメーター。</span><span class="sxs-lookup"><span data-stu-id="4ca66-222">To use a property value, type **$(***PropertyName***)**, where *PropertyName* is the name of the **Property** element or the name of the parameter.</span></span>
- <span data-ttu-id="4ca66-223">項目を使用する入力**@(***ItemName***)**ここで、 *ItemName*の名前を指定、**項目**要素。</span><span class="sxs-lookup"><span data-stu-id="4ca66-223">To use an item, type **@(***ItemName***)**, where *ItemName* is the name of the **Item** element.</span></span>

> [!NOTE]
> <span data-ttu-id="4ca66-224">同じ名前の複数の項目を作成する場合、リストをビルドすることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="4ca66-224">Remember that if you create multiple items with the same name, you're building a list.</span></span> <span data-ttu-id="4ca66-225">これに対し、最後のプロパティ値を指定する必要が同名 & #x 2014 で、前のプロパティを上書きと同じ名前の複数のプロパティを作成する場合は、プロパティは、単一の値のみを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-225">In contrast, if you create multiple properties with the same name, the last property value you provide will overwrite any previous properties with the same name&#x2014;a property can only contain a single value.</span></span>


<span data-ttu-id="4ca66-226">たとえば、 *Publish.proj*サンプル ソリューションのファイルを見て、 **BuildProjects**ターゲットです。</span><span class="sxs-lookup"><span data-stu-id="4ca66-226">For example, in the *Publish.proj* file in the sample solution, take a look at the **BuildProjects** target.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


<span data-ttu-id="4ca66-227">このサンプルでは、これらの要点を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-227">In this sample, you can observe these key points:</span></span>

- <span data-ttu-id="4ca66-228">場合、 **BuildingInTeamBuild**パラメーターが指定されの値を持つ**true**、このターゲット内のタスクのいずれも実行されます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-228">If the **BuildingInTeamBuild** parameter is specified and has a value of **true**, none of the tasks within this target will be executed.</span></span>
- <span data-ttu-id="4ca66-229">ターゲットには 1 つのインスタンスが含まれています、 [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx)タスク。</span><span class="sxs-lookup"><span data-stu-id="4ca66-229">The target contains a single instance of the [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) task.</span></span> <span data-ttu-id="4ca66-230">このタスクでは、MSBuild の他のプロジェクトをビルドすることができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-230">This task lets you build other MSBuild projects.</span></span>
- <span data-ttu-id="4ca66-231">**ProjectsToBuild**項目は、タスクに渡されます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-231">The **ProjectsToBuild** item is passed to the task.</span></span> <span data-ttu-id="4ca66-232">この項目はプロジェクトまたはソリューション ファイルで定義されたすべての一覧を表すことが**ProjectsToBuild**項目の項目グループ内の要素。</span><span class="sxs-lookup"><span data-stu-id="4ca66-232">This item could represent a list of project or solution files, all defined by **ProjectsToBuild** item elements within an item group.</span></span> <span data-ttu-id="4ca66-233">ここで、 **ProjectsToBuild**項目が 1 つのソリューション ファイルを参照します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-233">In this case, the **ProjectsToBuild** item refers to a single solution file.</span></span>

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- <span data-ttu-id="4ca66-234">渡されるプロパティの値、 **MSBuild**タスクに名前付きパラメーターを含める**OutputRoot**と**構成**です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-234">The property values passed to the **MSBuild** task include parameters named **OutputRoot** and **Configuration**.</span></span> <span data-ttu-id="4ca66-235">これらは、されていない場合、パラメーター値が指定される場合または静的プロパティの値に設定されます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-235">These are set to parameter values if they are provided, or static property values if they are not.</span></span>

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

<span data-ttu-id="4ca66-236">表示することも、 **MSBuild**タスクを呼び出すという名前のターゲット**ビルド**です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-236">You can also see that the **MSBuild** task invokes a target named **Build**.</span></span> <span data-ttu-id="4ca66-237">これは、広く使用されている Visual Studio プロジェクト ファイルでは、カスタムのプロジェクト ファイルで使用するようないくつかの組み込みのターゲットの 1 つ**ビルド**、**クリーン**、 **を再構築**、および**発行**です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-237">This is one of several built-in targets that are widely used in Visual Studio project files and are available to you in your custom project files, like **Build**, **Clean**, **Rebuild**, and **Publish**.</span></span> <span data-ttu-id="4ca66-238">ターゲットとタスクをビルド処理を制御しの使用に関する詳細情報を学びます、 **MSBuild**具体的には、タスクのこのトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-238">You'll learn more about using targets and tasks to control the build process, and about the **MSBuild** task in particular, later in this topic.</span></span>

> [!NOTE]
> <span data-ttu-id="4ca66-239">ターゲットの詳細については、次を参照してください。 [MSBuild ターゲット](https://msdn.microsoft.com/library/ms171462.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-239">For more information on targets, see [MSBuild Targets](https://msdn.microsoft.com/library/ms171462.aspx).</span></span>


## <a name="splitting-project-files-to-support-multiple-environments"></a><span data-ttu-id="4ca66-240">複数の環境をサポートするためにプロジェクト ファイルの分割</span><span class="sxs-lookup"><span data-stu-id="4ca66-240">Splitting Project Files to Support Multiple Environments</span></span>

<span data-ttu-id="4ca66-241">テスト サーバー、ステージングのプラットフォームでは、運用環境など、複数の環境にソリューションを展開したいとします。</span><span class="sxs-lookup"><span data-stu-id="4ca66-241">Suppose you want to be able to deploy a solution to multiple environments, like test servers, staging platforms, and production environments.</span></span> <span data-ttu-id="4ca66-242">構成は、これらの環境 & #x 2014 の間で大幅に異なる場合があります。 だけでなくサーバー名、接続文字列、および、の観点からも資格情報、セキュリティ設定、およびその他の要因の多く観点から可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4ca66-242">The configuration may vary substantially between these environments&#x2014;not just in terms of server names, connection strings, and so on, but also potentially in terms of credentials, security settings, and lots of other factors.</span></span> <span data-ttu-id="4ca66-243">これを定期的に実行する必要がある場合は、対象の環境を切り替えるたびに、プロジェクト ファイルで複数のプロパティを編集する非常に便利なことはできません。</span><span class="sxs-lookup"><span data-stu-id="4ca66-243">If you need to do this regularly, it's not really expedient to edit multiple properties in your project file every time you switch the target environment.</span></span> <span data-ttu-id="4ca66-244">最適なソリューションをビルド プロセスに提供するプロパティの値の無限のリストを必要とするでもありません。</span><span class="sxs-lookup"><span data-stu-id="4ca66-244">Nor is it an ideal solution to require an endless list of property values to be provided to the build process.</span></span>

<span data-ttu-id="4ca66-245">幸運にもこれとは別です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-245">Fortunately there is an alternative.</span></span> <span data-ttu-id="4ca66-246">MSBuild では、ビルド構成を複数のプロジェクト ファイルに分割することができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-246">MSBuild lets you split your build configuration across multiple project files.</span></span> <span data-ttu-id="4ca66-247">動作を理解するこの、サンプルのソリューションでは、2 つのカスタム プロジェクト ファイルがあることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="4ca66-247">To see how this works, in the sample solution, notice that there are two custom project files:</span></span>

- <span data-ttu-id="4ca66-248">*Publish.proj*プロパティ、項目が含まれています、およびターゲットはすべての環境に共通します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-248">*Publish.proj*, which contains properties, items, and targets that are common to all environments.</span></span>
- <span data-ttu-id="4ca66-249">*Env Dev.proj*、開発環境に固有のプロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4ca66-249">*Env-Dev.proj*, which contains properties that are specific to a developer environment.</span></span>

<span data-ttu-id="4ca66-250">今すぐことに注意して、 *Publish.proj*ファイルが含まれています、[インポート](https://msdn.microsoft.com/library/92x05xfs.aspx)、タグのすぐ下にある要素、**プロジェクト**タグ。</span><span class="sxs-lookup"><span data-stu-id="4ca66-250">Now notice that the *Publish.proj* file includes an [Import](https://msdn.microsoft.com/library/92x05xfs.aspx) element, immediately beneath the opening **Project** tag.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


<span data-ttu-id="4ca66-251">**インポート**要素を使用して、現在の MSBuild プロジェクト ファイルを別の MSBuild プロジェクト ファイルの内容をインポートします。</span><span class="sxs-lookup"><span data-stu-id="4ca66-251">The **Import** element is used to import the contents of another MSBuild project file into the current MSBuild project file.</span></span> <span data-ttu-id="4ca66-252">ここで、 **TargetEnvPropsFile**パラメーターは、インポートするプロジェクト ファイルのファイル名を提供します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-252">In this case, the **TargetEnvPropsFile** parameter provides the filename of the project file you want to import.</span></span> <span data-ttu-id="4ca66-253">MSBuild を実行するときに、このパラメーターの値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-253">You can provide a value for this parameter when you run MSBuild.</span></span>


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


<span data-ttu-id="4ca66-254">実質的に、これは、1 つのプロジェクト ファイルに 2 つのファイルの内容をマージします。</span><span class="sxs-lookup"><span data-stu-id="4ca66-254">This effectively merges the contents of the two files into a single project file.</span></span> <span data-ttu-id="4ca66-255">このアプローチを使用して、ユニバーサル ビルド構成を含む 1 つのプロジェクト ファイルと環境固有のプロパティを含む複数の補助プロジェクト ファイルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-255">Using this approach, you can create one project file containing your universal build configuration and multiple supplementary project files containing environment-specific properties.</span></span> <span data-ttu-id="4ca66-256">その結果、異なるパラメーター値を持つコマンドを実行することができます、別の環境にソリューションを展開します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-256">As a result, simply running a command with a different parameter value lets you deploy your solution to a different environment.</span></span>

![](understanding-the-project-file/_static/image3.png)

<span data-ttu-id="4ca66-257">この方法で、プロジェクト ファイルの分割に従うことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="4ca66-257">Splitting your project files in this way is a good practice to follow.</span></span> <span data-ttu-id="4ca66-258">複数のプロジェクト ファイル ユニバーサル ビルド プロパティの重複を回避しながら、1 つのコマンドを実行して、複数の環境に展開できます。</span><span class="sxs-lookup"><span data-stu-id="4ca66-258">It allows developers to deploy to multiple environments by running a single command, while avoiding the duplication of universal build properties across multiple project files.</span></span>

> [!NOTE]
> <span data-ttu-id="4ca66-259">サーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-259">For guidance on how to customize the environment-specific project files for your own server environments, see [Configuring Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


## <a name="conclusion"></a><span data-ttu-id="4ca66-260">まとめ</span><span class="sxs-lookup"><span data-stu-id="4ca66-260">Conclusion</span></span>

<span data-ttu-id="4ca66-261">このトピックでは、MSBuild プロジェクト ファイルの概要を提供し、ビルド プロセスを制御する独自のカスタム プロジェクト ファイルを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-261">This topic provided a general introduction to MSBuild project files and explained how you can create your own custom project files to control the build process.</span></span> <span data-ttu-id="4ca66-262">プロジェクト ファイルを簡単にビルドおよび複数の送信先にプロジェクトを配置するには、ユニバーサル ビルド手順と環境固有のビルド プロパティに分割の概念も導入されました。</span><span class="sxs-lookup"><span data-stu-id="4ca66-262">It also introduced the concept of splitting project files into universal build instructions and environment-specific build properties, to make it easy to build and deploy projects to multiple destinations.</span></span>

<span data-ttu-id="4ca66-263">次のトピックでは、[ビルド プロセスの理解](understanding-the-build-process.md)複雑さのレベルが現実的なソリューションの配置をここで、コントロールのビルドおよび配置するプロジェクト ファイルを使用する方法により多くの洞察を提供します。</span><span class="sxs-lookup"><span data-stu-id="4ca66-263">The next topic, [Understanding the Build Process](understanding-the-build-process.md), provides more insight into how you can use project files to control build and deployment by walking you through the deployment of a solution with a realistic level of complexity.</span></span>

## <a name="further-reading"></a><span data-ttu-id="4ca66-264">関連項目</span><span class="sxs-lookup"><span data-stu-id="4ca66-264">Further Reading</span></span>

<span data-ttu-id="4ca66-265">プロジェクト ファイルと、WPP をより詳細な概要については、次を参照してください。[内の Microsoft Build Engine: MSBuild を使用して、Team Foundation ビルド](http://amzn.com/0735645248)Sayed Ibrahim Hashimi して William Bartholomew、ISBN: 978-0-7356-4524-0 です。</span><span class="sxs-lookup"><span data-stu-id="4ca66-265">For a more in-depth introduction to project files and the WPP, see [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://amzn.com/0735645248) by Sayed Ibrahim Hashimi and William Bartholomew, ISBN: 978-0-7356-4524-0.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4ca66-266">[前へ](setting-up-the-contact-manager-solution.md)
[次へ](understanding-the-build-process.md)</span><span class="sxs-lookup"><span data-stu-id="4ca66-266">[Previous](setting-up-the-contact-manager-solution.md)
[Next](understanding-the-build-process.md)</span></span>
