---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: Web サーバーを構成する web 配置発行 (リモート エージェント) |Microsoft Docs
author: jrjlee
description: このトピックでは、web の発行および IIS の Web デプロイを使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: cb3191a260eb10a47f1aaf818052fcae023ff74a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392767"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a><span data-ttu-id="e2a21-103">Web 配置発行 (リモート エージェント) の Web サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-103">Configuring a Web Server for Web Deploy Publishing (Remote Agent)</span></span>
====================
<span data-ttu-id="e2a21-104">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="e2a21-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="e2a21-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="e2a21-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="e2a21-106">このトピックでは、web の発行および (Web 配置) IIS Web 配置ツールのリモート エージェント サービスを使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-106">This topic describes how to configure an Internet Information Services (IIS) web server to support web publishing and deployment using the IIS Web Deployment Tool (Web Deploy) Remote Agent Service.</span></span>
> 
> <span data-ttu-id="e2a21-107">Web Deploy 2.0 以降を操作するときに、アプリケーションまたは web サーバーにサイトを取得する際、3 つの主な方法があります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-107">When you work with Web Deploy 2.0 or later, there are three main approaches you can use to get your applications or sites onto a web server.</span></span> <span data-ttu-id="e2a21-108">次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="e2a21-108">You can:</span></span>
> 
> - <span data-ttu-id="e2a21-109">使用して、 *Web デプロイ エージェントのリモート サービス*します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-109">Use the *Web Deploy Remote Agent Service*.</span></span> <span data-ttu-id="e2a21-110">このアプローチには、web サーバーの以下の構成が必要ですが、サーバーにデプロイするためにローカル サーバーの管理者の資格情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-110">This approach requires less configuration of the web server, but you need to provide the credentials of a local server administrator in order to deploy anything to the server.</span></span>
> - <span data-ttu-id="e2a21-111">使用して、 *Web 配置ハンドラー*します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-111">Use the *Web Deploy Handler*.</span></span> <span data-ttu-id="e2a21-112">このアプローチは、もっと複雑な web サーバーを設定する初期の労力が必要です。</span><span class="sxs-lookup"><span data-stu-id="e2a21-112">This approach is a lot more complex and requires more initial effort to set up the web server.</span></span> <span data-ttu-id="e2a21-113">ただし、このアプローチを使用する場合は、配置を実行する管理者以外のユーザーを許可するように IIS を構成できます。</span><span class="sxs-lookup"><span data-stu-id="e2a21-113">However, when you use this approach, you can configure IIS to allow non-administrator users to perform the deployment.</span></span> <span data-ttu-id="e2a21-114">Web 配置ハンドラーでは、IIS 7 以降のバージョンで利用できるのみです。</span><span class="sxs-lookup"><span data-stu-id="e2a21-114">The Web Deploy Handler is only available in IIS version 7 or later.</span></span>
> - <span data-ttu-id="e2a21-115">使用*オフライン展開*します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-115">Use *offline deployment*.</span></span> <span data-ttu-id="e2a21-116">このアプローチには、web サーバーの最低限の構成が必要ですが、サーバー管理者は、サーバー上に web パッケージのコピーし IIS マネージャーからインポートする必要があります手動でします。</span><span class="sxs-lookup"><span data-stu-id="e2a21-116">This approach requires the least configuration of the web server, but a server administrator must manually copy the web package onto the server and import it through IIS Manager.</span></span>
> 
> <span data-ttu-id="e2a21-117">主な機能、利点、およびこれらのアプローチの欠点の詳細については、次を参照してください。 [Web 配置を右側のアプローチを選択](choosing-the-right-approach-to-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-117">For more information on the key features, advantages, and disadvantages of these approaches, see [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md).</span></span>


## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a><span data-ttu-id="e2a21-118">Web では、リモート エージェントの適切なアプローチを展開するのでしょうか。</span><span class="sxs-lookup"><span data-stu-id="e2a21-118">Is the Web Deploy Remote Agent the Right Approach for You?</span></span>

<span data-ttu-id="e2a21-119">はい、コンテンツを展開したユーザーは、移行先サーバーの管理者の資格情報を指定できます。</span><span class="sxs-lookup"><span data-stu-id="e2a21-119">Yes, if the user who will deploy the content can supply the credentials of an administrator on the destination server.</span></span> <span data-ttu-id="e2a21-120">この方法は、これらの種類のシナリオでは望ましくは多くの場合です。</span><span class="sxs-lookup"><span data-stu-id="e2a21-120">This approach is often desirable in these types of scenarios:</span></span>

- <span data-ttu-id="e2a21-121">移行先の web サーバーとデータベース サーバーに対するフル コントロールを持つ開発者の開発またはテスト環境</span><span class="sxs-lookup"><span data-stu-id="e2a21-121">Development or test environments, where the developer has full control over the destination web server and database server.</span></span>
- <span data-ttu-id="e2a21-122">小規模な組織で 1 人のユーザーまたはユーザーの小規模なグループが、全体のアプリケーション ライフ サイクルを制御します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-122">Smaller organizations in which a single user or a small group of users has control over the entire application lifecycle.</span></span>

<span data-ttu-id="e2a21-123">多くの大規模な組織とステージング環境または運用環境に特にで多くの場合は現実的では web サーバーで管理者権限をユーザーに付与します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-123">In lots of larger organizations, and particularly for staging or production environments, it's often not realistic to give users administrator rights on web servers.</span></span> <span data-ttu-id="e2a21-124">ホストされた web サーバーの場合でない場合に特に起こりやすくなります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-124">In the case of hosted web servers, this is especially unlikely to be the case.</span></span> <span data-ttu-id="e2a21-125">さらに、ビルド サーバーからのデプロイを自動化することを計画している、ない場合、展開プロセスに管理者の資格情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-125">In addition, if you're planning to automate deployment from a build server, you may not want to use administrator credentials for the deployment process.</span></span> <span data-ttu-id="e2a21-126">これらのシナリオで使用した展開をサポートするために web サーバーの構成、 [Web 配置ハンドラー](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)より満足できる選択肢を提供することがあります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-126">In these scenarios, configuring your web servers to support deployment using the [Web Deploy Handler](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md) may provide a more satisfactory choice.</span></span>

## <a name="task-overview"></a><span data-ttu-id="e2a21-127">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="e2a21-127">Task Overview</span></span>

<span data-ttu-id="e2a21-128">このトピックでは、インターネット インフォメーション サービス (IIS) 7.5 web サーバーをそのまま使用し、Web デプロイのリモート エージェントのアプローチを使用してリモート コンピューターから web パッケージの配置を構成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-128">This topic describes how to configure an Internet Information Services (IIS) 7.5 web server to accept and deploy web packages from a remote computer using the Web Deploy Remote Agent approach.</span></span> <span data-ttu-id="e2a21-129">必要があります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-129">You'll need to:</span></span>

- <span data-ttu-id="e2a21-130">IIS 7.5、および推奨構成は、IIS 7 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e2a21-130">Install IIS 7.5 and the IIS 7 recommended configuration.</span></span>
- <span data-ttu-id="e2a21-131">Web Deploy 2.1 以降をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e2a21-131">Install Web Deploy 2.1 or later.</span></span>
- <span data-ttu-id="e2a21-132">展開されたコンテンツをホストする IIS の web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-132">Create an IIS website to host the deployed content.</span></span>
- <span data-ttu-id="e2a21-133">Web 配置エージェント サービスが実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-133">Ensure that the Web Deployment Agent Service is running.</span></span>

<span data-ttu-id="e2a21-134">具体的には、サンプル ソリューションをホストにもする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-134">To host the sample solution specifically, you'll also need to:</span></span>

- <span data-ttu-id="e2a21-135">.NET Framework 4.0 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e2a21-135">Install the .NET Framework 4.0.</span></span>
- <span data-ttu-id="e2a21-136">ASP.NET MVC 3 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e2a21-136">Install ASP.NET MVC 3.</span></span>

<span data-ttu-id="e2a21-137">このトピックでは、これらの手順を実行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-137">This topic will show you how to perform each of these procedures.</span></span> <span data-ttu-id="e2a21-138">タスクとチュートリアルでは、このトピックでは、Windows Server 2008 R2 を実行しているサーバーのクリーン ビルドを開始することを想定しています。</span><span class="sxs-lookup"><span data-stu-id="e2a21-138">The tasks and walkthroughs in this topic assume that you're starting with a clean server build running Windows Server 2008 R2.</span></span> <span data-ttu-id="e2a21-139">続行する前にいることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-139">Before you continue, ensure that:</span></span>

- <span data-ttu-id="e2a21-140">Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="e2a21-140">Windows Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>
- <span data-ttu-id="e2a21-141">サーバーは、ドメインに参加しています。</span><span class="sxs-lookup"><span data-stu-id="e2a21-141">The server is domain-joined.</span></span>
- <span data-ttu-id="e2a21-142">サーバーは、静的 IP アドレスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="e2a21-142">The server has a static IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="e2a21-143">コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューターのドメインとログオン](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-143">For more information on joining computers to a domain, see [Joining Computers to the Domain and Logging On](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx).</span></span> <span data-ttu-id="e2a21-144">静的 IP アドレスの構成の詳細については、次を参照してください。[静的 IP アドレス構成](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-144">For more information on configuring static IP addresses, see [Configure a Static IP Address](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).</span></span> <span data-ttu-id="e2a21-145">リモート エージェント サービスは、IIS 6 で以降はサポートされするドメインに参加させる必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e2a21-145">The Remote Agent service is supported by IIS 6 onwards and does not require you to be joined to a domain.</span></span> <span data-ttu-id="e2a21-146">ただし、このチュートリアルの手順では開発され、IIS 7.5 でテストし、他のバージョンの手順が異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-146">However, the steps in this tutorial were developed and tested on IIS 7.5 and procedures for other versions may vary.</span></span>


## <a name="install-products-and-components"></a><span data-ttu-id="e2a21-147">製品とコンポーネントをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e2a21-147">Install Products and Components</span></span>

<span data-ttu-id="e2a21-148">このセクションで、web サーバーで必要な製品とコンポーネントのインストールを説明します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-148">This section will guide you through installing the required products and components on the web server.</span></span> <span data-ttu-id="e2a21-149">開始する前に、サーバーが完全に最新であることを確認する Windows 更新プログラムを実行することをお勧めは。</span><span class="sxs-lookup"><span data-stu-id="e2a21-149">Before you begin, a good practice is to run Windows Update to ensure that your server is fully up to date.</span></span>

<span data-ttu-id="e2a21-150">この場合、これらをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-150">In this case, you need to install these things:</span></span>

- <span data-ttu-id="e2a21-151">**IIS 7 推奨構成**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-151">**IIS 7 Recommended Configuration**.</span></span> <span data-ttu-id="e2a21-152">これにより、 **Web サーバー (IIS)** web サーバー上のロールと IIS モジュールと ASP.NET アプリケーションをホストするために必要なコンポーネントのセットをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e2a21-152">This enables the **Web Server (IIS)** role on your web server and installs the set of IIS modules and components that you need in order to host an ASP.NET application.</span></span>
- <span data-ttu-id="e2a21-153">**.NET framework 4.0**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-153">**.NET Framework 4.0**.</span></span> <span data-ttu-id="e2a21-154">これは、このバージョンの .NET Framework で構築されたアプリケーションの実行に必要です。</span><span class="sxs-lookup"><span data-stu-id="e2a21-154">This is required to run applications that were built on this version of the .NET Framework.</span></span>
- <span data-ttu-id="e2a21-155">**Web 配置ツール 2.1 以降**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-155">**Web Deployment Tool 2.1 or later**.</span></span> <span data-ttu-id="e2a21-156">これにより、Web Deploy (とその基になる実行可能ファイル、MSDeploy.exe) がサーバーにインストールされます。</span><span class="sxs-lookup"><span data-stu-id="e2a21-156">This installs Web Deploy (and its underlying executable, MSDeploy.exe) on your server.</span></span> <span data-ttu-id="e2a21-157">このプロセスの一環として、インストールされ、Web Deployment Agent サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-157">As part of this process, it installs and starts the Web Deployment Agent Service.</span></span> <span data-ttu-id="e2a21-158">このサービスでは、リモート コンピューターから web パッケージを配置することができます。</span><span class="sxs-lookup"><span data-stu-id="e2a21-158">This service lets you deploy web packages from a remote computer.</span></span>
- <span data-ttu-id="e2a21-159">**ASP.NET MVC 3**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-159">**ASP.NET MVC 3**.</span></span> <span data-ttu-id="e2a21-160">MVC 3 アプリケーションを実行する必要があるアセンブリがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="e2a21-160">This installs the assemblies you need to run MVC 3 applications.</span></span>

> [!NOTE]
> <span data-ttu-id="e2a21-161">このチュートリアルでは、Web Platform Installer をインストールに必要なコンポーネントの構成の使用について説明します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-161">This walkthrough describes the use of the Web Platform Installer to install and configure the required components.</span></span> <span data-ttu-id="e2a21-162">Web Platform Installer を使用する必要はありません、自動的に依存関係を検出して、常に製品の最新バージョンを取得することを確認して、インストール プロセスが簡略化します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-162">Although you don't have to use the Web Platform Installer, it simplifies the installation process by automatically detecting dependencies and ensuring that you always get the latest product versions.</span></span> <span data-ttu-id="e2a21-163">詳細については、次を参照してください。 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-163">For more information, see [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).</span></span>


<span data-ttu-id="e2a21-164">**必要な製品とコンポーネントをインストールするには**</span><span class="sxs-lookup"><span data-stu-id="e2a21-164">**To install the required products and components**</span></span>

1. <span data-ttu-id="e2a21-165">ダウンロードしてインストール、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-165">Download and install the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>
2. <span data-ttu-id="e2a21-166">インストールが完了したら、Web Platform Installer は自動的に起動されます。</span><span class="sxs-lookup"><span data-stu-id="e2a21-166">When installation is complete, the Web Platform Installer will launch automatically.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2a21-167">いつでも、Web Platform Installer を起動できるようになりました、**開始**メニュー。</span><span class="sxs-lookup"><span data-stu-id="e2a21-167">You can now launch the Web Platform Installer at any time from the **Start** menu.</span></span> <span data-ttu-id="e2a21-168">これを実行する、**開始** メニューのをクリックして**すべてのプログラム**、順にクリックします**Microsoft Web Platform Installer**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-168">To do this, on the **Start** menu, click **All Programs**, and then click **Microsoft Web Platform Installer**.</span></span>
3. <span data-ttu-id="e2a21-169">上部にある、 **Web Platform Installer 3.0**ウィンドウで、をクリックして**製品**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-169">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
4. <span data-ttu-id="e2a21-170">ナビゲーション ウィンドウで、ウィンドウの左側にある クリックして**フレームワーク**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-170">On the left side of the window, in the navigation pane, click **Frameworks**.</span></span>
5. <span data-ttu-id="e2a21-171">**Microsoft .NET Framework 4** 、.NET Framework が既にインストールされていない場合、行をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-171">In the **Microsoft .NET Framework 4** row, if the .NET Framework is not already installed, click **Add**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2a21-172">既にインストールしている Windows Update を通じて、.NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="e2a21-172">You may have already installed the .NET Framework 4.0 through Windows Update.</span></span> <span data-ttu-id="e2a21-173">製品またはコンポーネントが既にインストールされている場合、Web Platform Installer 示されます置き換えることで、**追加**ボタン テキストを含む**インストール済み**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-173">If a product or component is already installed, the Web Platform Installer will indicate this by replacing the **Add** button with the text **Installed**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. <span data-ttu-id="e2a21-174">**ASP.NET MVC 3 (Visual Studio 2010)** 行で、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-174">In the **ASP.NET MVC 3 (Visual Studio 2010)** row, click **Add**.</span></span>
7. <span data-ttu-id="e2a21-175">ナビゲーション ウィンドウで、 **Server**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-175">In the navigation pane, click **Server**.</span></span>
8. <span data-ttu-id="e2a21-176">**IIS 7 推奨構成**行で、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-176">In the **IIS 7 Recommended Configuration** row, click **Add**.</span></span>
9. <span data-ttu-id="e2a21-177">**Web 配置ツール 2.1**行で、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-177">In the **Web Deployment Tool 2.1** row, click **Add**.</span></span>
10. <span data-ttu-id="e2a21-178">**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e2a21-178">Click **Install**.</span></span> <span data-ttu-id="e2a21-179">Web Platform Installer が製品の一覧を表示&#x2014;と関連する依存関係&#x2014;をインストールし、ライセンス条項に同意するように求められます。</span><span class="sxs-lookup"><span data-stu-id="e2a21-179">The Web Platform Installer will show you a list of products&#x2014;together with any associated dependencies&#x2014;to be installed and will prompt you to accept the license terms.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. <span data-ttu-id="e2a21-180">ライセンス条項を確認し、条項に同意する場合にクリックします**同意**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-180">Review the license terms, and if you consent to the terms, click **I Accept**.</span></span>
12. <span data-ttu-id="e2a21-181">インストールが完了したら、クリックして**完了**、閉じて、 **Web Platform Installer 3.0**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="e2a21-181">When the installation is complete, click **Finish**, and then close the **Web Platform Installer 3.0** window.</span></span>

<span data-ttu-id="e2a21-182">IIS をインストールする前に、.NET Framework 4.0 をインストールした場合を実行する必要があります、 [ASP.NET IIS 登録ツール](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) を IIS に ASP.NET の最新バージョンを登録します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-182">If you installed the .NET Framework 4.0 before you installed IIS, you'll need to run the [ASP.NET IIS Registration Tool](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) to register the latest version of ASP.NET with IIS.</span></span> <span data-ttu-id="e2a21-183">これを行わないと、見つかります IIS が静的なコンテンツを提供 (HTML ファイル) など、問題なくが返されます**HTTP エラー 404.0 – 見つかりません**しようとすると、ASP.NET のコンテンツを参照します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-183">If you don't do this, you'll find that IIS will serve static content (like HTML files) without any problems, but it will return **HTTP Error 404.0 – Not Found** when you attempt to browse to ASP.NET content.</span></span> <span data-ttu-id="e2a21-184">この手順を使用すると、ASP.NET 4.0 が登録されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-184">You can use this procedure to ensure that ASP.NET 4.0 is registered.</span></span>

<span data-ttu-id="e2a21-185">**ASP.NET 4.0 を IIS に登録するには**</span><span class="sxs-lookup"><span data-stu-id="e2a21-185">**To register ASP.NET 4.0 with IIS**</span></span>

1. <span data-ttu-id="e2a21-186">クリックして**開始**、し、入力**コマンド プロンプト**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-186">Click **Start**, and then type **Command Prompt**.</span></span>
2. <span data-ttu-id="e2a21-187">右クリックし、検索結果で**コマンド プロンプト**、 をクリックし、**管理者として実行**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-187">In the search results, right-click **Command Prompt**, and then click **Run as administrator**.</span></span>
3. <span data-ttu-id="e2a21-188">コマンド プロンプト ウィンドウに移動、 **%WINDIR%\Microsoft.NET\Framework\v4.0.30319**ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="e2a21-188">In the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.</span></span>
4. <span data-ttu-id="e2a21-189">このコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-189">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. <span data-ttu-id="e2a21-190">任意の時点の 64 ビット web アプリケーションをホストする場合は、IIS と ASP.NET の 64 ビット バージョンを登録することも必要があります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-190">If you plan to host 64-bit web applications at any point, you should also register the 64-bit version of ASP.NET with IIS.</span></span> <span data-ttu-id="e2a21-191">コマンド プロンプト ウィンドウに移動します。、 **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="e2a21-191">To do this, in the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.</span></span>
6. <span data-ttu-id="e2a21-192">このコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-192">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

<span data-ttu-id="e2a21-193">お勧めとして Windows Update を使用してもう一度この時点でダウンロードして、新製品とコンポーネントをインストールしたら、使用可能な更新プログラムをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e2a21-193">As a good practice, use Windows Update again at this point to download and install any available updates for the new products and components you've installed.</span></span>

## <a name="configure-the-iis-website"></a><span data-ttu-id="e2a21-194">IIS web サイトを構成します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-194">Configure the IIS Website</span></span>

<span data-ttu-id="e2a21-195">Web コンテンツを展開するには、サーバーに、前に作成してコンテンツをホストする IIS の web サイトを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-195">Before you can deploy web content to your server, you need to create and configure an IIS website to host the content.</span></span> <span data-ttu-id="e2a21-196">Web Deploy; 既存の IIS web サイトに web パッケージを配置できるだけweb サイトを作成できません。</span><span class="sxs-lookup"><span data-stu-id="e2a21-196">Web Deploy can only deploy web packages to an existing IIS website; it can't create the website for you.</span></span> <span data-ttu-id="e2a21-197">大まかに言えば、これらのタスクを完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-197">At a high level, you'll need to complete these tasks:</span></span>

- <span data-ttu-id="e2a21-198">コンテンツをホストするファイル システム上のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-198">Create a folder on the file system to host your content.</span></span>
- <span data-ttu-id="e2a21-199">コンテンツを処理するために IIS の web サイトを作成し、ローカル フォルダーに関連付けます。</span><span class="sxs-lookup"><span data-stu-id="e2a21-199">Create an IIS website to serve the content, and associate it with the local folder.</span></span>
- <span data-ttu-id="e2a21-200">読み取りローカル フォルダーにアプリケーション プール id へのアクセス許可を付与します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-200">Grant read permissions to the application pool identity on the local folder.</span></span>

<span data-ttu-id="e2a21-201">IIS の既定の web サイトにコンテンツを展開するなんらですが、この方法はテストまたはデモ シナリオ以外は推奨されません。</span><span class="sxs-lookup"><span data-stu-id="e2a21-201">Although there's nothing stopping you from deploying content to the default website in IIS, this approach is not recommended for anything other than test or demonstration scenarios.</span></span> <span data-ttu-id="e2a21-202">運用環境をシミュレートするには、アプリケーションの要件に固有の設定で新しい IIS web サイトを作成してください。</span><span class="sxs-lookup"><span data-stu-id="e2a21-202">To simulate a production environment, you should create a new IIS website with settings that are specific to the requirements of your application.</span></span>

<span data-ttu-id="e2a21-203">**作成および IIS の web サイトを構成するには**</span><span class="sxs-lookup"><span data-stu-id="e2a21-203">**To create and configure an IIS website**</span></span>

1. <span data-ttu-id="e2a21-204">ローカル ファイル システムでコンテンツを格納するフォルダーを作成します (たとえば、 **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="e2a21-204">On the local file system, create a folder to store your content (for example, **C:\DemoSite**).</span></span>
2. <span data-ttu-id="e2a21-205">**開始**メニューで、**管理ツール**、 をクリックし、**インターネット インフォメーション サービス (IIS) マネージャー**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-205">On the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
3. <span data-ttu-id="e2a21-206">IIS マネージャーで、**接続**ウィンドウで、サーバー ノードを展開 (たとえば、 **TESTWEB1**)。</span><span class="sxs-lookup"><span data-stu-id="e2a21-206">In IIS Manager, in the **Connections** pane, expand the server node (for example, **TESTWEB1**).</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. <span data-ttu-id="e2a21-207">右クリックし、**サイト**ノード、およびクリック**Web サイトの追加**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-207">Right-click the **Sites** node, and then click **Add Web Site**.</span></span>
5. <span data-ttu-id="e2a21-208">**サイト名**ボックスに、IIS web サイトの名前を入力 (たとえば、 **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="e2a21-208">In the **Site name** box, type a name for the IIS website (for example, **DemoSite**).</span></span>
6. <span data-ttu-id="e2a21-209">**物理パス**ボックス、入力 (またはを参照)、ローカル フォルダーへのパス (たとえば、 **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="e2a21-209">In the **Physical path** box, type (or browse to) the path to your local folder (for example, **C:\DemoSite**).</span></span>
7. <span data-ttu-id="e2a21-210">**ポート**ボックスに、web サイトをホストするポート番号を入力 (たとえば、 **85**)。</span><span class="sxs-lookup"><span data-stu-id="e2a21-210">In the **Port** box, type the port number on which you want to host the website (for example, **85**).</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2a21-211">標準ポート番号は、http 80 および HTTPS の場合は 443 です。</span><span class="sxs-lookup"><span data-stu-id="e2a21-211">The standard port numbers are 80 for HTTP and 443 for HTTPS.</span></span> <span data-ttu-id="e2a21-212">ただし、ポート 80 では、この web サイトをホストしている場合は、サイトにアクセスするには、既定の web サイトを停止する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-212">However, if you host this website on port 80, you'll need to stop the default website before you can access your site.</span></span>
8. <span data-ttu-id="e2a21-213">ままに、**ホスト名**ボックスをクリックして、web サイト用のドメイン ネーム システム (DNS) レコードを構成する場合、空白**OK**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-213">Leave the **Host name** box blank, unless you want to configure a Domain Name System (DNS) record for the website, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="e2a21-214">運用環境では、ポート 80 で web サイトをホストし、DNS レコードを一致すると共に、ホスト ヘッダーを構成するします可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-214">In a production environment, you'll likely want to host your website on port 80 and configure a host header, together with matching DNS records.</span></span> <span data-ttu-id="e2a21-215">IIS 7 でホスト ヘッダーの構成の詳細については、次を参照してください。 [(IIS 7) の Web サイトのホスト ヘッダーを構成する](https://technet.microsoft.com/library/cc753195(WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-215">For more information on configuring host headers in IIS 7, see [Configure a Host Header for a Web Site (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx).</span></span> <span data-ttu-id="e2a21-216">Windows Server 2008 R2 の DNS サーバーの役割の詳細については、次を参照してください。 [DNS サーバーの概要](https://technet.microsoft.com/en-gb/library/cc770392.aspx)と[DNS Server](https://technet.microsoft.com/windowsserver/dd448607)します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-216">For more information on the DNS Server role in Windows Server 2008 R2, see [DNS Server Overview](https://technet.microsoft.com/en-gb/library/cc770392.aspx) and [DNS Server](https://technet.microsoft.com/windowsserver/dd448607).</span></span>
9. <span data-ttu-id="e2a21-217">**[アクション]** ウィンドウの **[サイトの編集]** の下にある **[バインド]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e2a21-217">In the **Actions** pane, under **Edit Site**, click **Bindings**.</span></span>
10. <span data-ttu-id="e2a21-218">**サイト バインド**ダイアログ ボックスで、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-218">In the **Site Bindings** dialog box, click **Add**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. <span data-ttu-id="e2a21-219">**サイト バインドの追加**ダイアログ ボックスで、セット、 **IP アドレス**と**ポート**既存のサイトの構成に一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="e2a21-219">In the **Add Site Binding** dialog box, set the **IP address** and **Port** to match your existing site configuration.</span></span>
12. <span data-ttu-id="e2a21-220">**ホスト名**ボックス、web サーバーの名前を入力 (たとえば、 **TESTWEB1**)、順にクリックします**OK**。</span><span class="sxs-lookup"><span data-stu-id="e2a21-220">In the **Host name** box, type the name of your web server (for example, **TESTWEB1**), and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > <span data-ttu-id="e2a21-221">最初のサイトのバインドでは、IP アドレスとポートを使用してローカル サイトにアクセスできます。 または`http://localhost:85`します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-221">The first site binding allows you to access the site locally using the IP address and port or `http://localhost:85`.</span></span> <span data-ttu-id="e2a21-222">2 つ目のサイトのバインドでは、マシン名を使用して、ドメインの他のコンピューターからサイトにアクセスできます (たとえば、http://testweb1:85)します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-222">The second site binding allows you to access the site from other computers on the domain using the machine name (for example, http://testweb1:85).</span></span>
13. <span data-ttu-id="e2a21-223">**サイト バインド**ダイアログ ボックスで、をクリックして**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-223">In the **Site Bindings** dialog box, click **Close**.</span></span>
14. <span data-ttu-id="e2a21-224">**接続**ウィンドウで、をクリックして**アプリケーション プール**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-224">In the **Connections** pane, click **Application Pools**.</span></span>
15. <span data-ttu-id="e2a21-225">**アプリケーション プール**ウィンドウは、アプリケーション プールの名前を右クリックし、クリックして**基本設定**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-225">In the **Application Pools** pane, right-click the name of your application pool, and then click **Basic Settings**.</span></span> <span data-ttu-id="e2a21-226">既定では、アプリケーション プールの名前が、web サイトの名前に一致 (たとえば、 **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="e2a21-226">By default, the name of your application pool will match the name of your website (for example, **DemoSite**).</span></span>
16. <span data-ttu-id="e2a21-227">**.NET Framework のバージョン**一覧で、 **.NET Framework v4.0.30319**、順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-227">In the **.NET Framework version** list, select **.NET Framework v4.0.30319**, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="e2a21-228">サンプル ソリューションでは、.NET Framework 4.0 が必要です。</span><span class="sxs-lookup"><span data-stu-id="e2a21-228">The sample solution requires .NET Framework 4.0.</span></span> <span data-ttu-id="e2a21-229">これではなく Web デプロイのための要件です。</span><span class="sxs-lookup"><span data-stu-id="e2a21-229">This is not a requirement for Web Deploy in general.</span></span>

<span data-ttu-id="e2a21-230">Web サイト コンテンツを配信するためには、アプリケーション プール id をコンテンツを格納するローカル フォルダーのアクセス許可読み取りが必要です。</span><span class="sxs-lookup"><span data-stu-id="e2a21-230">In order for your website to serve content, the application pool identity must have read permissions on the local folder that stores the content.</span></span> <span data-ttu-id="e2a21-231">IIS 7.5 では、アプリケーション プールは、(アプリケーション プールが通常実行されるネットワーク サービス アカウントを使用して、IIS の以前のバージョン) とは異なり、既定で一意のアプリケーション プール id で実行します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-231">In IIS 7.5, application pools run with a unique application pool identity by default (in contrast to previous versions of IIS, where application pools would typically run using the Network Service account).</span></span> <span data-ttu-id="e2a21-232">アプリケーション プール id の実際のユーザー アカウントではないとユーザーまたはグループのすべてのリストが表示されない&#x2014;代わりにときに、作成されます動的にアプリケーション プールを開始します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-232">The application pool identity is not a real user account and does not show up on any lists of users or groups&#x2014;instead, it's created dynamically when the application pool is started.</span></span> <span data-ttu-id="e2a21-233">各アプリケーション プール id をローカルに追加**IIS\_IUSRS**セキュリティ グループを非表示の項目として。</span><span class="sxs-lookup"><span data-stu-id="e2a21-233">Each application pool identity is added to the local **IIS\_IUSRS** security group as a hidden item.</span></span>

<span data-ttu-id="e2a21-234">ファイルまたはフォルダーでのアプリケーション プール id へのアクセス許可を付与するには、2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-234">To grant permissions to an application pool identity on a file or folder, you have two options:</span></span>

- <span data-ttu-id="e2a21-235">アクセス許可を割り当てる、アプリケーション プール id に直接、形式を使用して<strong>IIS AppPool\</strong ><em>[アプリケーション プール名]</em>(たとえば、 <strong>IIS AppPool\DemoSite</strong>).</span><span class="sxs-lookup"><span data-stu-id="e2a21-235">Assign permissions to the application pool identity directly, using the format <strong>IIS AppPool\</strong><em>[application pool name]</em>(for example, <strong>IIS AppPool\DemoSite</strong>).</span></span>
- <span data-ttu-id="e2a21-236">アクセス許可を割り当てる、 **IIS\_IUSRS**グループ。</span><span class="sxs-lookup"><span data-stu-id="e2a21-236">Assign permissions to the **IIS\_IUSRS** group.</span></span>

<span data-ttu-id="e2a21-237">最も一般的なアプローチは、ローカルに権限を割り当てる、 **IIS\_IUSRS**のため、このアプローチでは、ファイル システム アクセス許可を再構成することがなくアプリケーション プールを変更できます。 グループ化します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-237">The most common approach is to assign permissions to the local **IIS\_IUSRS** group because this approach lets you change application pools without reconfiguring file system permissions.</span></span> <span data-ttu-id="e2a21-238">次の手順では、このグループ ベースのアプローチを使用します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-238">The next procedure uses this group-based approach.</span></span>

> [!NOTE]
> <span data-ttu-id="e2a21-239">IIS 7.5 でアプリケーション プール id の詳細については、次を参照してください。[アプリケーション プール Id](https://go.microsoft.com/?linkid=9805123)します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-239">For more information on application pool identities in IIS 7.5, see [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).</span></span>


<span data-ttu-id="e2a21-240">**IIS の web サイトのフォルダーのアクセス許可を構成するには**</span><span class="sxs-lookup"><span data-stu-id="e2a21-240">**To configure folder permissions for an IIS website**</span></span>

1. <span data-ttu-id="e2a21-241">Windows エクスプ ローラーでは、ローカル フォルダーの場所を参照します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-241">In Windows Explorer, browse to the location of your local folder.</span></span>
2. <span data-ttu-id="e2a21-242">フォルダーを右クリックし、**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-242">Right-click the folder, and then click **Properties**.</span></span>
3. <span data-ttu-id="e2a21-243">**セキュリティ**] タブで [**編集**、順にクリックします**追加**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-243">On the **Security** tab, click **Edit**, and then click **Add**.</span></span>
4. <span data-ttu-id="e2a21-244">クリックして**場所**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-244">Click **Locations**.</span></span> <span data-ttu-id="e2a21-245">**場所** ダイアログ ボックスでは、ローカルのサーバーを選択し、順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-245">In the **Locations** dialog box, select the local server, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. <span data-ttu-id="e2a21-246">**[ユーザーまたはグループ**ダイアログ ボックスに「 **IIS\_IUSRS**、] をクリックして**名前の確認**順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-246">In the **Select Users or Groups** dialog box, type **IIS\_IUSRS**, click **Check Names**, and then click **OK**.</span></span>
6. <span data-ttu-id="e2a21-247"><strong>のアクセス許可</strong><em>[フォルダー名]</em>ダイアログ ボックスで、新しいグループが割り当てられている通知、<strong>読み取り&amp;実行</strong>、<strong>フォルダーの一覧内容</strong>、および<strong>読み取り</strong>既定のアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-247">In the <strong>Permissions for</strong><em>[folder name]</em>dialog box, notice that the new group has been assigned the <strong>Read &amp; execute</strong>, <strong>List folder contents</strong>, and <strong>Read</strong> permissions by default.</span></span> <span data-ttu-id="e2a21-248">変更なしのままにし、クリックして<strong>OK</strong>します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-248">Leave this unchanged and click <strong>OK</strong>.</span></span>
7. <span data-ttu-id="e2a21-249">をクリックして<strong>OK</strong>を閉じる、 <em>[フォルダー名]</em><strong>プロパティ</strong> ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="e2a21-249">Click <strong>OK</strong> to close the <em>[folder name]</em><strong>Properties</strong> dialog box.</span></span>

<span data-ttu-id="e2a21-250">最後のタスクとして、サーバーにすべての web パッケージをデプロイする前に行う必要があります、Web Deployment Agent サービスが実行されていること。</span><span class="sxs-lookup"><span data-stu-id="e2a21-250">As a final task before you attempt to deploy any web packages to your server, you should ensure that the Web Deployment Agent Service is running.</span></span> <span data-ttu-id="e2a21-251">リモート コンピューターからパッケージを展開するときに、Web Deployment Agent サービスは抽出およびパッケージの内容をインストールする責任を負います。</span><span class="sxs-lookup"><span data-stu-id="e2a21-251">When you deploy a package from a remote computer, the Web Deployment Agent Service is responsible for extracting and installing the contents of the package.</span></span> <span data-ttu-id="e2a21-252">サービスは、Web 配置ツールをインストールするときに、既定で開始され、ネットワーク サービスの id で実行されます。</span><span class="sxs-lookup"><span data-stu-id="e2a21-252">The service is started by default when you install the Web Deployment Tool and runs under the Network Service identity.</span></span>

<span data-ttu-id="e2a21-253">確認できます、複数の異なる方法で、サービスが実行されているかどうかさまざまなコマンド ライン ユーティリティまたは Windows PowerShell コマンドレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-253">You can check whether a service is running in multiple different ways, using various command-line utilities or Windows PowerShell cmdlets.</span></span> <span data-ttu-id="e2a21-254">この手順では、簡単な UI ベースのアプローチについて説明します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-254">This procedure describes a straightforward UI-based approach.</span></span>

<span data-ttu-id="e2a21-255">**Web 配置エージェント サービスが実行されていることを確認するには**</span><span class="sxs-lookup"><span data-stu-id="e2a21-255">**To check that the Web Deployment Agent Service is running**</span></span>

1. <span data-ttu-id="e2a21-256">**[スタート]** メニューで、 **[管理ツール]** をポイントして、 **[サービス]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e2a21-256">On the **Start** menu, point to **Administrative Tools**, and then click **Services**.</span></span>
2. <span data-ttu-id="e2a21-257">検索、 **Web Deployment Agent サービス**行、および、いることを確認、**状態**に設定されている**開始**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-257">Locate the **Web Deployment Agent Service** row, and verify that the **Status** is set to **Started**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. <span data-ttu-id="e2a21-258">サービスがまだ開始されていない場合はクリックして**開始**します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-258">If the service is not already started, click **Start**.</span></span>

## <a name="configure-firewall-exceptions"></a><span data-ttu-id="e2a21-259">ファイアウォールの例外を構成します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-259">Configure Firewall Exceptions</span></span>

<span data-ttu-id="e2a21-260">既定では、リモート エージェント サービスは この URL で TCP ポート 80 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="e2a21-260">By default, the Remote Agent Service listens on TCP port 80, at this URL:</span></span>

<http://servername.com/MSDEPLOYAGENTSERVICE>

<span data-ttu-id="e2a21-261">ほとんどの場合、web サーバーは、通常ポート 80 で HTTP 要求をリッスンするため、リモート エージェント サービスの追加のファイアウォール規則を構成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e2a21-261">In most cases, you won't need to configure any additional firewall rules for the Remote Agent Service because web servers typically listen for HTTP requests on port 80.</span></span> <span data-ttu-id="e2a21-262">非標準ポートでリッスンするように、インストールをカスタマイズする場合は、必要に応じてファイアウォールの例外を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-262">If you customized your installation to listen on a nonstandard port, you'll need to configure firewall exceptions as required.</span></span>

## <a name="conclusion"></a><span data-ttu-id="e2a21-263">まとめ</span><span class="sxs-lookup"><span data-stu-id="e2a21-263">Conclusion</span></span>

<span data-ttu-id="e2a21-264">この時点で、web サーバーがそのまま使用してリモート コンピューターから web パッケージをインストールする準備ができてです。</span><span class="sxs-lookup"><span data-stu-id="e2a21-264">At this point, your web server is ready to accept and install web packages from a remote computer.</span></span> <span data-ttu-id="e2a21-265">サーバーに web アプリケーションをデプロイする前に、次の点を確認したい場合があります。</span><span class="sxs-lookup"><span data-stu-id="e2a21-265">Before you attempt to deploy a web application to the server, you may want to check these key points:</span></span>

- <span data-ttu-id="e2a21-266">IIS で ASP.NET 4.0 を登録したでしょうか。</span><span class="sxs-lookup"><span data-stu-id="e2a21-266">Have you registered ASP.NET 4.0 with IIS?</span></span>
- <span data-ttu-id="e2a21-267">アプリケーション プール id は、web サイトのソース フォルダーに読み取りアクセスをあるでしょうか。</span><span class="sxs-lookup"><span data-stu-id="e2a21-267">Does the application pool identity have read access to the source folder for your website?</span></span>
- <span data-ttu-id="e2a21-268">Web 配置エージェント サービスが実行されているか。</span><span class="sxs-lookup"><span data-stu-id="e2a21-268">Is the Web Deployment Agent Service running?</span></span>

## <a name="further-reading"></a><span data-ttu-id="e2a21-269">関連項目</span><span class="sxs-lookup"><span data-stu-id="e2a21-269">Further Reading</span></span>

<span data-ttu-id="e2a21-270">リモート エージェント サービスに web パッケージを配置するカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを構成する方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](configuring-deployment-properties-for-a-target-environment.md)します。</span><span class="sxs-lookup"><span data-stu-id="e2a21-270">For guidance on how to configure custom Microsoft Build Engine (MSBuild) project files to deploy web packages to the Remote Agent Service, see [Configuring Deployment Properties for a Target Environment](configuring-deployment-properties-for-a-target-environment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e2a21-271">[前へ](scenario-configuring-a-production-environment-for-web-deployment.md)
> [次へ](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)</span><span class="sxs-lookup"><span data-stu-id="e2a21-271">[Previous](scenario-configuring-a-production-environment-for-web-deployment.md)
[Next](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)</span></span>
