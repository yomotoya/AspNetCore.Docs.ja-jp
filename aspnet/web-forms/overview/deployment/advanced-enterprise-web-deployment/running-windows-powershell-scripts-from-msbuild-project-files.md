---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: MSBuild プロジェクト ファイルからの Windows PowerShell スクリプトの実行 |Microsoft Docs
author: jrjlee
description: このトピックでは、ビルドおよび配置プロセスの一部として Windows PowerShell スクリプトを実行する方法について説明します。 スクリプトをローカルで実行することができます (つまり、b の..。
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: faedcee480b6c50dc560055206fedbe7af4d5f67
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803150"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="ed3fd-104">MSBuild プロジェクト ファイルからの Windows PowerShell スクリプトの実行</span><span class="sxs-lookup"><span data-stu-id="ed3fd-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="ed3fd-105">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="ed3fd-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="ed3fd-106">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="ed3fd-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="ed3fd-107">このトピックでは、ビルドおよび配置プロセスの一部として Windows PowerShell スクリプトを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="ed3fd-108">(つまり、ビルド サーバー) でローカル スクリプトを実行したり、送信先の web サーバーまたはデータベース サーバーなどのリモートでできます。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="ed3fd-109">配置後の Windows PowerShell スクリプトを実行するかの理由が います。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="ed3fd-110">たとえば、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="ed3fd-111">カスタム イベント ソースをレジストリに追加します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="ed3fd-112">ファイル システム ディレクトリのアップロードを生成します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="ed3fd-113">ビルド ディレクトリをクリーンアップします。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-113">Clean up build directories.</span></span>
> - <span data-ttu-id="ed3fd-114">カスタム ログ ファイルにエントリを記述します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="ed3fd-115">新しくプロビジョニングされた web アプリケーションにユーザーの招待メールを送信します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="ed3fd-116">適切なアクセス許可を持つユーザー アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="ed3fd-117">SQL Server インスタンス間のレプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="ed3fd-118">このトピックでは、Microsoft Build Engine (MSBuild) プロジェクト ファイルでカスタム ターゲットからローカルとリモートの両方に、Windows PowerShell スクリプトを実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="ed3fd-119">このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="ed3fd-120">これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="ed3fd-121">ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="ed3fd-122">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="ed3fd-122">Task Overview</span></span>

<span data-ttu-id="ed3fd-123">を、自動またはシングル ステップの展開プロセスの一部として Windows PowerShell スクリプトを実行するには、これらの高度なタスクを完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="ed3fd-124">ソリューションをソース管理には、Windows PowerShell スクリプトを追加します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="ed3fd-125">Windows PowerShell スクリプトを起動するコマンドを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="ed3fd-126">コマンドで、予約済みの XML 文字をエスケープします。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="ed3fd-127">カスタム MSBuild プロジェクト ファイルでターゲットを作成し、使用、 **Exec**コマンドを実行するタスク。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="ed3fd-128">このトピックでは、これらの手順を実行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="ed3fd-129">タスクとチュートリアルでは、このトピックでは、MSBuild ターゲットおよびプロパティ、慣れている既にことと、カスタム MSBuild プロジェクト ファイルを使用してビルドおよび配置プロセスを促進する方法を理解することを想定しています。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="ed3fd-130">詳細については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="ed3fd-131">作成して、Windows PowerShell スクリプトの追加</span><span class="sxs-lookup"><span data-stu-id="ed3fd-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="ed3fd-132">という名前のサンプル Windows PowerShell スクリプトを使用して、このトピックのタスクで**LogDeploy.ps1** MSBuild からスクリプトを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="ed3fd-133">**LogDeploy.ps1**スクリプトには、ログ ファイルを単一行のエントリを書き込む単純な関数が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]


<span data-ttu-id="ed3fd-134">**LogDeploy.ps1**スクリプトは 2 つのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="ed3fd-135">最初のパラメーターは、エントリを追加するログ ファイルに完全なパスを表すし、2 番目のパラメーターは、ログ ファイルに記録する、配置先を表します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="ed3fd-136">スクリプトを実行するときに、この形式でログ ファイルに行が追加されます。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="ed3fd-137">させる、 **LogDeploy.ps1** MSBuild を使用可能なスクリプト、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="ed3fd-138">スクリプトをソース コントロールに追加します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-138">Add the script to source control.</span></span>
- <span data-ttu-id="ed3fd-139">Visual Studio 2010 でのソリューションに対して、スクリプトを追加します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="ed3fd-140">ビルド サーバー上またはリモート コンピューターでスクリプトを実行するかどうかに関係なく、ソリューションのコンテンツでスクリプトを展開する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="ed3fd-141">1 つのオプションでは、ソリューション フォルダーにスクリプトを追加します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="ed3fd-142">連絡先のマネージャーの例で、展開プロセスの一部として、Windows PowerShell スクリプトを使用するため、理にかなって発行ソリューション フォルダーにスクリプトを追加します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="ed3fd-143">ソリューション フォルダーの内容は、ソース マテリアルとしてビルド サーバーにコピーされます。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="ed3fd-144">ただし、プロジェクト出力の一部を形成しません。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="ed3fd-145">ビルド サーバーで Windows PowerShell スクリプトの実行</span><span class="sxs-lookup"><span data-stu-id="ed3fd-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="ed3fd-146">一部のシナリオで、プロジェクトをビルドしているコンピューターで Windows PowerShell スクリプトを実行する場合があります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="ed3fd-147">たとえば、ビルド フォルダーをクリーンアップまたはカスタムのログ ファイルにエントリを書き込むに Windows PowerShell スクリプトを使用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="ed3fd-148">構文の観点から MSBuild プロジェクト ファイルから Windows PowerShell スクリプトを実行しているが通常コマンド プロンプトから、Windows PowerShell スクリプトを実行すると同じ。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="ed3fd-149">実行可能ファイル、powershell.exe を起動し、使用する必要がある、 **– コマンド**スイッチを実行する Windows PowerShell のコマンドを提供します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="ed3fd-150">(Windows PowerShell v2 を使用することも、 **– ファイル**切り替えます)。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="ed3fd-151">コマンドは、この形式を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="ed3fd-152">例えば:</span><span class="sxs-lookup"><span data-stu-id="ed3fd-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="ed3fd-153">スクリプトへのパスにスペースが含まれている場合は、ファイルのパスを単一引用符の前にアンパサンドで囲む必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="ed3fd-154">コマンドを囲むために既に使用しているため、二重引用符を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="ed3fd-155">MSBuild からこのコマンドを呼び出す場合は、いくつか追加の考慮事項にもあります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="ed3fd-156">最初に、含める必要がある、 **– NonInteractive**スクリプトをサイレント モードで実行されるようにするフラグ。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="ed3fd-157">次に、含める必要がある、 **– ExecutionPolicy**フラグを適切な引数の値。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="ed3fd-158">これには、Windows PowerShell はスクリプトに適用され、スクリプトの実行を防ぐことがあります既定の実行ポリシーをオーバーライドすることができます、実行ポリシーを指定します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="ed3fd-159">これらの引数の値から選択できます。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="ed3fd-160">値**Unrestricted** Windows PowerShell スクリプトを署名するかどうかに関係なく、スクリプトを実行できるようになります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="ed3fd-161">値**RemoteSigned** Windows PowerShell をローカル コンピューター上に作成された符号なしのスクリプトを実行できるようになります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="ed3fd-162">ただし、別の場所で作成されたスクリプトを署名する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="ed3fd-163">(実際には、していない場合に、ビルド サーバーで Windows PowerShell スクリプトをローカルで作成した)。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="ed3fd-164">値**AllSigned**により、署名済みスクリプトのみを実行する Windows PowerShell。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="ed3fd-165">既定の実行ポリシーは**Restricted**、され、Windows PowerShell は任意のスクリプト ファイルを実行できなきます。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="ed3fd-166">最後に、Windows PowerShell コマンドで発生する予約済み XML 文字をエスケープする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="ed3fd-167">単一引用符を置き換える **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="ed3fd-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="ed3fd-168">二重引用符で置き換える **&amp;quot;**</span><span class="sxs-lookup"><span data-stu-id="ed3fd-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="ed3fd-169">アンパサンドを置き換える **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="ed3fd-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="ed3fd-170">これらの変更を行ったときに、コマンドがこのようになります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="ed3fd-171">カスタム MSBuild プロジェクト ファイル内には、新しいターゲットを作成して使用することができます、 **Exec**このコマンドを実行するタスク。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="ed3fd-172">この例に注意してください。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-172">In this example, note that:</span></span>

- <span data-ttu-id="ed3fd-173">パラメーター値と、Windows PowerShell 実行可能ファイルの場所など、任意の変数は、MSBuild プロパティとして宣言されます。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="ed3fd-174">コマンドラインからこれらの値を上書きするユーザーを有効にするのには、条件が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="ed3fd-175">**MSDeployComputerName**どこかプロジェクト ファイルでプロパティを宣言します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="ed3fd-176">ビルド プロセスの一部としてこのターゲットを実行するときに Windows PowerShell がコマンドを実行し、指定したファイルにログ エントリを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="ed3fd-177">リモート コンピューターで Windows PowerShell スクリプトの実行</span><span class="sxs-lookup"><span data-stu-id="ed3fd-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="ed3fd-178">Windows PowerShell がリモート コンピューターでスクリプトを実行できる[Windows リモート管理](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx)(WinRM)。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="ed3fd-179">これを行うには、使用する必要があります、 [Invoke-command](https://technet.microsoft.com/library/dd347578.aspx)コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="ed3fd-180">これにより、スクリプトをリモート コンピューターにコピーすることがなく 1 つまたは複数のリモート コンピューターに対してスクリプトを実行できます。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="ed3fd-181">スクリプトの実行に使用したローカル コンピューターには、すべての結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="ed3fd-182">使用する前に、 **Invoke-command**リモート コンピューターでスクリプトを Windows PowerShell を実行するコマンドレット、リモートのメッセージを受け入れるように WinRM リスナーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="ed3fd-183">コマンドを実行してこれを行う**winrm quickconfig**リモート コンピューター。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="ed3fd-184">詳細については、次を参照してください。[インストールと構成の Windows リモート管理](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="ed3fd-185">実行するこの構文を使用する、Windows PowerShell ウィンドウから、 **LogDeploy.ps1**リモート コンピューター上のスクリプト。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="ed3fd-186">使用するさまざまな他の方法がある**Invoke-command**スクリプトを実行するが、この方法が最も簡単なパラメーター値を指定して、パスにスペースを管理する必要がある場合。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="ed3fd-187">これをコマンド プロンプトから実行するときに実行可能ファイル、Windows PowerShell を起動して使用する必要があります、 **– コマンド**の説明を指定するパラメーター。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="ed3fd-188">前に、いくつか追加のスイッチを行い、MSBuild からコマンドを実行すると、予約済み XML 文字をエスケープする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="ed3fd-189">使用できると同様に、最後に、 **Exec**コマンドを実行するカスタム MSBuild ターゲット内のタスク。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="ed3fd-190">Windows PowerShell がで指定したコンピューターでスクリプトを実行、ビルド プロセスの一部としてこのターゲットを実行するときに、 **– computername**引数。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="ed3fd-191">まとめ</span><span class="sxs-lookup"><span data-stu-id="ed3fd-191">Conclusion</span></span>

<span data-ttu-id="ed3fd-192">このトピックでは、MSBuild プロジェクト ファイルから Windows PowerShell スクリプトを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="ed3fd-193">このアプローチを使用すると、またはシングル ステップの自動ビルドと展開プロセスの一環としてローカルまたはリモート コンピューター上の Windows PowerShell スクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="ed3fd-194">関連項目</span><span class="sxs-lookup"><span data-stu-id="ed3fd-194">Further Reading</span></span>

<span data-ttu-id="ed3fd-195">Windows PowerShell スクリプトに署名して、実行ポリシーの管理に関するガイダンスについては、次を参照してください。 [Windows PowerShell スクリプトの実行](https://technet.microsoft.com/library/ee176949.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="ed3fd-196">リモート コンピューターから Windows PowerShell コマンドを実行する方法の詳細については、次を参照してください。[リモート コマンドを実行している](https://technet.microsoft.com/library/dd819505.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="ed3fd-197">カスタム MSBuild プロジェクト ファイルを使用して、展開プロセスを制御する詳細については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。</span><span class="sxs-lookup"><span data-stu-id="ed3fd-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ed3fd-198">[前へ](taking-web-applications-offline-with-web-deploy.md)
> [次へ](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="ed3fd-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
