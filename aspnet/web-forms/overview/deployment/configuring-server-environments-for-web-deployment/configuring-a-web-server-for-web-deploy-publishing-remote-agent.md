---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: "Web サーバーを構成する web デプロイの発行 (リモート エージェント) |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、web の発行および IIS Web 配置を使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: 61e357198ffa4e93d35b7fa4619270da630547c6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a><span data-ttu-id="15fd1-103">Web デプロイの発行 (リモート エージェント) 用の Web サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="15fd1-103">Configuring a Web Server for Web Deploy Publishing (Remote Agent)</span></span>
====================
<span data-ttu-id="15fd1-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="15fd1-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="15fd1-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="15fd1-106">このトピックでは、web の発行および IIS Web 配置ツール (Web 配置) のリモート エージェント サービスを使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-106">This topic describes how to configure an Internet Information Services (IIS) web server to support web publishing and deployment using the IIS Web Deployment Tool (Web Deploy) Remote Agent Service.</span></span>
> 
> <span data-ttu-id="15fd1-107">Web Deploy 2.0 以降を使用するときは、3 つの主な方法がアプリケーションまたは web サーバー上にサイトを取得するを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="15fd1-107">When you work with Web Deploy 2.0 or later, there are three main approaches you can use to get your applications or sites onto a web server.</span></span> <span data-ttu-id="15fd1-108">次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="15fd1-108">You can:</span></span>
> 
> - <span data-ttu-id="15fd1-109">使用して、 *Web デプロイ エージェントのリモート サービス*です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-109">Use the *Web Deploy Remote Agent Service*.</span></span> <span data-ttu-id="15fd1-110">このアプローチには、web サーバーの以下の構成が必要ですが、何もサーバーに配置するためにローカル サーバーの管理者の資格情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-110">This approach requires less configuration of the web server, but you need to provide the credentials of a local server administrator in order to deploy anything to the server.</span></span>
> - <span data-ttu-id="15fd1-111">使用して、 *Web Deploy ハンドラー*です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-111">Use the *Web Deploy Handler*.</span></span> <span data-ttu-id="15fd1-112">このアプローチはもっと複雑であり、web サーバーを設定する初期多くの労力が必要です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-112">This approach is a lot more complex and requires more initial effort to set up the web server.</span></span> <span data-ttu-id="15fd1-113">ただし、このアプローチを使用する場合は、配置を実行する管理者以外のユーザーを許可するように IIS を構成できます。</span><span class="sxs-lookup"><span data-stu-id="15fd1-113">However, when you use this approach, you can configure IIS to allow non-administrator users to perform the deployment.</span></span> <span data-ttu-id="15fd1-114">Web 展開ハンドラーは IIS 7 以降のバージョンにできるだけです。</span><span class="sxs-lookup"><span data-stu-id="15fd1-114">The Web Deploy Handler is only available in IIS version 7 or later.</span></span>
> - <span data-ttu-id="15fd1-115">使用して*オフライン展開*です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-115">Use *offline deployment*.</span></span> <span data-ttu-id="15fd1-116">このアプローチには、web サーバーの最低限の構成が必要ですが、サーバー管理者は、する必要があります手動でサーバーに web パッケージをコピーして IIS マネージャーからインポートします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-116">This approach requires the least configuration of the web server, but a server administrator must manually copy the web package onto the server and import it through IIS Manager.</span></span>
> 
> <span data-ttu-id="15fd1-117">主な機能、利点、およびこれらのアプローチの欠点の詳細については、次を参照してください。 [Web 配置を右側の方法を選択する](choosing-the-right-approach-to-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-117">For more information on the key features, advantages, and disadvantages of these approaches, see [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md).</span></span>


## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a><span data-ttu-id="15fd1-118">Web では、リモート エージェントの適切なアプローチを配置するのですか。</span><span class="sxs-lookup"><span data-stu-id="15fd1-118">Is the Web Deploy Remote Agent the Right Approach for You?</span></span>

<span data-ttu-id="15fd1-119">はい (コンテンツを展開したユーザーは、移行先サーバーの管理者の資格情報を提供できる場合)。</span><span class="sxs-lookup"><span data-stu-id="15fd1-119">Yes, if the user who will deploy the content can supply the credentials of an administrator on the destination server.</span></span> <span data-ttu-id="15fd1-120">この方法は、これらの種類のシナリオでは望ましく、多くの場合。</span><span class="sxs-lookup"><span data-stu-id="15fd1-120">This approach is often desirable in these types of scenarios:</span></span>

- <span data-ttu-id="15fd1-121">ターゲット web サーバーおよびデータベース サーバーに対するフル コントロールを持つ開発者の開発またはテスト環境</span><span class="sxs-lookup"><span data-stu-id="15fd1-121">Development or test environments, where the developer has full control over the destination web server and database server.</span></span>
- <span data-ttu-id="15fd1-122">小規模な組織が 1 人のユーザーまたはユーザーの小さなグループに、全体のアプリケーション ライフ サイクルを制御します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-122">Smaller organizations in which a single user or a small group of users has control over the entire application lifecycle.</span></span>

<span data-ttu-id="15fd1-123">多くの大規模な組織とステージング環境または実稼働環境に特に、web サーバー上の管理者権限をユーザーに与える実際は、ほとんどの場合です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-123">In lots of larger organizations, and particularly for staging or production environments, it's often not realistic to give users administrator rights on web servers.</span></span> <span data-ttu-id="15fd1-124">ホストされる web サーバーの場合これは特に、大文字である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-124">In the case of hosted web servers, this is especially unlikely to be the case.</span></span> <span data-ttu-id="15fd1-125">さらに、ビルド サーバーから展開を自動化する場合は、可能性がありますいないする展開プロセスに管理者の資格情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-125">In addition, if you're planning to automate deployment from a build server, you may not want to use administrator credentials for the deployment process.</span></span> <span data-ttu-id="15fd1-126">これらのシナリオで使用した展開をサポートするために web サーバーの構成、 [Web 展開ハンドラー](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)さらに快適な選択肢を提供する場合があります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-126">In these scenarios, configuring your web servers to support deployment using the [Web Deploy Handler](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md) may provide a more satisfactory choice.</span></span>

## <a name="task-overview"></a><span data-ttu-id="15fd1-127">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="15fd1-127">Task Overview</span></span>

<span data-ttu-id="15fd1-128">このトピックでは、そのまま使用し、Web デプロイのリモート エージェントのアプローチを使用してリモート コンピューターから web パッケージを配置するインターネット インフォメーション サービス (IIS) 7.5 web サーバーを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-128">This topic describes how to configure an Internet Information Services (IIS) 7.5 web server to accept and deploy web packages from a remote computer using the Web Deploy Remote Agent approach.</span></span> <span data-ttu-id="15fd1-129">必要があります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-129">You'll need to:</span></span>

- <span data-ttu-id="15fd1-130">IIS 7.5、および IIS 7 の推奨構成をインストールします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-130">Install IIS 7.5 and the IIS 7 recommended configuration.</span></span>
- <span data-ttu-id="15fd1-131">Web Deploy 2.1 以降をインストールします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-131">Install Web Deploy 2.1 or later.</span></span>
- <span data-ttu-id="15fd1-132">展開されたコンテンツをホストする IIS の web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-132">Create an IIS website to host the deployed content.</span></span>
- <span data-ttu-id="15fd1-133">Web Deployment Agent サービスが実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-133">Ensure that the Web Deployment Agent Service is running.</span></span>

<span data-ttu-id="15fd1-134">サンプル ソリューションを具体的にはホストにもする必要があります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-134">To host the sample solution specifically, you'll also need to:</span></span>

- <span data-ttu-id="15fd1-135">.NET Framework 4.0 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-135">Install the .NET Framework 4.0.</span></span>
- <span data-ttu-id="15fd1-136">ASP.NET MVC 3 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-136">Install ASP.NET MVC 3.</span></span>

<span data-ttu-id="15fd1-137">このトピックでは、各手順を実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-137">This topic will show you how to perform each of these procedures.</span></span> <span data-ttu-id="15fd1-138">タスクとここでチュートリアルは、Windows Server 2008 R2 を実行するクリーン サーバー ビルドを開始していることを想定しています。</span><span class="sxs-lookup"><span data-stu-id="15fd1-138">The tasks and walkthroughs in this topic assume that you're starting with a clean server build running Windows Server 2008 R2.</span></span> <span data-ttu-id="15fd1-139">続行する前に以下のことを確認します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-139">Before you continue, ensure that:</span></span>

- <span data-ttu-id="15fd1-140">Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="15fd1-140">Windows Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>
- <span data-ttu-id="15fd1-141">サーバーがドメインに参加します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-141">The server is domain-joined.</span></span>
- <span data-ttu-id="15fd1-142">サーバーは、静的 IP アドレスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="15fd1-142">The server has a static IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="15fd1-143">コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューター、ドメインとログオン](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-143">For more information on joining computers to a domain, see [Joining Computers to the Domain and Logging On](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx).</span></span> <span data-ttu-id="15fd1-144">静的 IP アドレスを構成する方法については、次を参照してください。[静的 IP アドレスを構成](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-144">For more information on configuring static IP addresses, see [Configure a Static IP Address](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx).</span></span> <span data-ttu-id="15fd1-145">リモート エージェント サービスは、以降は IIS 6 でサポートされてし、するドメインに参加させる必要はありません。</span><span class="sxs-lookup"><span data-stu-id="15fd1-145">The Remote Agent service is supported by IIS 6 onwards and does not require you to be joined to a domain.</span></span> <span data-ttu-id="15fd1-146">ただし、このチュートリアルの手順が開発され、IIS 7.5 でテストされた、他のバージョンのプロシージャが異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-146">However, the steps in this tutorial were developed and tested on IIS 7.5 and procedures for other versions may vary.</span></span>


## <a name="install-products-and-components"></a><span data-ttu-id="15fd1-147">製品とコンポーネントをインストールします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-147">Install Products and Components</span></span>

<span data-ttu-id="15fd1-148">このセクションでは、web サーバーに必要な製品とコンポーネントをインストールを説明します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-148">This section will guide you through installing the required products and components on the web server.</span></span> <span data-ttu-id="15fd1-149">開始する前に、お勧め、サーバーが最新であることを確認する Windows Update を実行します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-149">Before you begin, a good practice is to run Windows Update to ensure that your server is fully up to date.</span></span>

<span data-ttu-id="15fd1-150">この場合、これらをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-150">In this case, you need to install these things:</span></span>

- <span data-ttu-id="15fd1-151">**IIS 7 の推奨構成**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-151">**IIS 7 Recommended Configuration**.</span></span> <span data-ttu-id="15fd1-152">これにより、 **Web サーバー (IIS)**ロール、web サーバー上の IIS モジュールおよび ASP.NET アプリケーションをホストするために必要なコンポーネントのセットをインストールします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-152">This enables the **Web Server (IIS)** role on your web server and installs the set of IIS modules and components that you need in order to host an ASP.NET application.</span></span>
- <span data-ttu-id="15fd1-153">**.NET framework 4.0**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-153">**.NET Framework 4.0**.</span></span> <span data-ttu-id="15fd1-154">これは、このバージョンの .NET Framework で構築されたアプリケーションの実行に必要です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-154">This is required to run applications that were built on this version of the .NET Framework.</span></span>
- <span data-ttu-id="15fd1-155">**Web 配置ツール 2.1 以降**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-155">**Web Deployment Tool 2.1 or later**.</span></span> <span data-ttu-id="15fd1-156">これにより、Web Deploy (とその基になる実行可能ファイル、MSDeploy.exe) がサーバーにインストールされます。</span><span class="sxs-lookup"><span data-stu-id="15fd1-156">This installs Web Deploy (and its underlying executable, MSDeploy.exe) on your server.</span></span> <span data-ttu-id="15fd1-157">このプロセスの一環としてがインストールされ、Web Deployment Agent サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-157">As part of this process, it installs and starts the Web Deployment Agent Service.</span></span> <span data-ttu-id="15fd1-158">このサービスでは、リモート コンピューターから web パッケージを展開できます。</span><span class="sxs-lookup"><span data-stu-id="15fd1-158">This service lets you deploy web packages from a remote computer.</span></span>
- <span data-ttu-id="15fd1-159">**ASP.NET MVC 3**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-159">**ASP.NET MVC 3**.</span></span> <span data-ttu-id="15fd1-160">これは、MVC 3 アプリケーションを実行する必要があるアセンブリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-160">This installs the assemblies you need to run MVC 3 applications.</span></span>

> [!NOTE]
> <span data-ttu-id="15fd1-161">このチュートリアルでは、Web Platform Installer をインストールして、必要なコンポーネントの構成の使用について説明します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-161">This walkthrough describes the use of the Web Platform Installer to install and configure the required components.</span></span> <span data-ttu-id="15fd1-162">Web Platform Installer を使用できますが、自動的に依存関係を検出して、常に製品の最新バージョンを取得することを確認して、インストール プロセスが簡略化します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-162">Although you don't have to use the Web Platform Installer, it simplifies the installation process by automatically detecting dependencies and ensuring that you always get the latest product versions.</span></span> <span data-ttu-id="15fd1-163">詳細については、次を参照してください。 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-163">For more information, see [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).</span></span>


<span data-ttu-id="15fd1-164">**必要な製品とコンポーネントをインストールするには**</span><span class="sxs-lookup"><span data-stu-id="15fd1-164">**To install the required products and components**</span></span>

1. <span data-ttu-id="15fd1-165">ダウンロードしてインストール、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-165">Download and install the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>
2. <span data-ttu-id="15fd1-166">インストールが完了したら、Web Platform Installer が自動的に起動します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-166">When installation is complete, the Web Platform Installer will launch automatically.</span></span>

    > [!NOTE]
    > <span data-ttu-id="15fd1-167">いつでも、Web Platform Installer を起動できるようになりました、**開始**メニュー。</span><span class="sxs-lookup"><span data-stu-id="15fd1-167">You can now launch the Web Platform Installer at any time from the **Start** menu.</span></span> <span data-ttu-id="15fd1-168">[、**開始**] メニューのをクリックして**すべてのプログラム**、順にクリック**Microsoft Web Platform Installer**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-168">To do this, on the **Start** menu, click **All Programs**, and then click **Microsoft Web Platform Installer**.</span></span>
3. <span data-ttu-id="15fd1-169">上部にある、 **Web Platform Installer 3.0**ウィンドウで、をクリックして**製品**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-169">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
4. <span data-ttu-id="15fd1-170">ナビゲーション ウィンドウで、ウィンドウの左側にあるをクリックして**フレームワーク**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-170">On the left side of the window, in the navigation pane, click **Frameworks**.</span></span>
5. <span data-ttu-id="15fd1-171">**Microsoft .NET Framework 4** 、.NET Framework がインストールされていない場合、行をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-171">In the **Microsoft .NET Framework 4** row, if the .NET Framework is not already installed, click **Add**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="15fd1-172">既にインストールしている Windows Update から .NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="15fd1-172">You may have already installed the .NET Framework 4.0 through Windows Update.</span></span> <span data-ttu-id="15fd1-173">製品またはコンポーネントがインストール済みの場合、Web Platform Installer は、これに置き換えることで、**追加**ボタン テキストを**インストール**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-173">If a product or component is already installed, the Web Platform Installer will indicate this by replacing the **Add** button with the text **Installed**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. <span data-ttu-id="15fd1-174">**ASP.NET MVC 3 (Visual Studio 2010)**行で、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-174">In the **ASP.NET MVC 3 (Visual Studio 2010)** row, click **Add**.</span></span>
7. <span data-ttu-id="15fd1-175">ナビゲーション ウィンドウで **サーバー**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-175">In the navigation pane, click **Server**.</span></span>
8. <span data-ttu-id="15fd1-176">**IIS 7 の推奨構成**行で、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-176">In the **IIS 7 Recommended Configuration** row, click **Add**.</span></span>
9. <span data-ttu-id="15fd1-177">**Web 配置ツール 2.1**行で、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-177">In the **Web Deployment Tool 2.1** row, click **Add**.</span></span>
10. <span data-ttu-id="15fd1-178">**[インストール]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-178">Click **Install**.</span></span> <span data-ttu-id="15fd1-179">Web Platform Installer をインストールするには、関連する依存関係 & #x 2014; と共に; 製品 & #x 2014 の一覧が表示され、ライセンス条項に同意するように求められます。</span><span class="sxs-lookup"><span data-stu-id="15fd1-179">The Web Platform Installer will show you a list of products&#x2014;together with any associated dependencies&#x2014;to be installed and will prompt you to accept the license terms.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. <span data-ttu-id="15fd1-180">ライセンス条項を確認し、条項に同意した場合にをクリックして**同意**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-180">Review the license terms, and if you consent to the terms, click **I Accept**.</span></span>
12. <span data-ttu-id="15fd1-181">インストールが完了したらをクリックして**完了**、し、閉じます、 **Web Platform Installer 3.0**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="15fd1-181">When the installation is complete, click **Finish**, and then close the **Web Platform Installer 3.0** window.</span></span>

<span data-ttu-id="15fd1-182">IIS をインストールする前に、.NET Framework 4.0 をインストールした場合を実行する必要があります、 [ASP.NET IIS 登録ツール](https://msdn.microsoft.com/en-us/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) を IIS と ASP.NET の最新バージョンを登録します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-182">If you installed the .NET Framework 4.0 before you installed IIS, you'll need to run the [ASP.NET IIS Registration Tool](https://msdn.microsoft.com/en-us/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) to register the latest version of ASP.NET with IIS.</span></span> <span data-ttu-id="15fd1-183">これを行わないと、ことがわかります (HTML ファイル) などの静的なコンテンツを IIS が提供する、問題なくが返されます**HTTP エラー 404.0-Not Found** ASP.NET のコンテンツを参照しようとします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-183">If you don't do this, you'll find that IIS will serve static content (like HTML files) without any problems, but it will return **HTTP Error 404.0 – Not Found** when you attempt to browse to ASP.NET content.</span></span> <span data-ttu-id="15fd1-184">この手順を使用すると、ASP.NET 4.0 が登録されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="15fd1-184">You can use this procedure to ensure that ASP.NET 4.0 is registered.</span></span>

<span data-ttu-id="15fd1-185">**ASP.NET 4.0 を IIS に登録するには**</span><span class="sxs-lookup"><span data-stu-id="15fd1-185">**To register ASP.NET 4.0 with IIS**</span></span>

1. <span data-ttu-id="15fd1-186">をクリックして**開始**、し、入力**コマンド プロンプト**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-186">Click **Start**, and then type **Command Prompt**.</span></span>
2. <span data-ttu-id="15fd1-187">検索結果を右クリックして**コマンド プロンプト**、クリックして**管理者として実行**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-187">In the search results, right-click **Command Prompt**, and then click **Run as administrator**.</span></span>
3. <span data-ttu-id="15fd1-188">コマンド プロンプト ウィンドウに移動、 **%WINDIR%\Microsoft.NET\Framework\v4.0.30319**ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="15fd1-188">In the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.</span></span>
4. <span data-ttu-id="15fd1-189">このコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-189">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. <span data-ttu-id="15fd1-190">任意の時点での 64 ビットの web アプリケーションをホストする場合は、IIS と 64 ビット バージョンの ASP.NET を登録することも必要があります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-190">If you plan to host 64-bit web applications at any point, you should also register the 64-bit version of ASP.NET with IIS.</span></span> <span data-ttu-id="15fd1-191">コマンド プロンプト ウィンドウで、これに移動、 **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="15fd1-191">To do this, in the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.</span></span>
6. <span data-ttu-id="15fd1-192">このコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-192">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

<span data-ttu-id="15fd1-193">をお勧め Windows Update を使用してもう一度この時点でダウンロードして、新しい製品とコンポーネントがインストールされている使用可能な更新プログラムをインストールします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-193">As a good practice, use Windows Update again at this point to download and install any available updates for the new products and components you've installed.</span></span>

## <a name="configure-the-iis-website"></a><span data-ttu-id="15fd1-194">IIS web サイトを構成します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-194">Configure the IIS Website</span></span>

<span data-ttu-id="15fd1-195">Web コンテンツを展開するには、サーバーに、前に作成し、コンテンツをホストする IIS の web サイトを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-195">Before you can deploy web content to your server, you need to create and configure an IIS website to host the content.</span></span> <span data-ttu-id="15fd1-196">Web Deploy にしかデプロイできません web パッケージ既存の IIS web サイトです。web サイトを作成できません。</span><span class="sxs-lookup"><span data-stu-id="15fd1-196">Web Deploy can only deploy web packages to an existing IIS website; it can't create the website for you.</span></span> <span data-ttu-id="15fd1-197">大まかに言えば、これらのタスクを完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-197">At a high level, you'll need to complete these tasks:</span></span>

- <span data-ttu-id="15fd1-198">コンテンツをホストするファイル システムにフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-198">Create a folder on the file system to host your content.</span></span>
- <span data-ttu-id="15fd1-199">コンテンツを提供する IIS の web サイトを作成し、ローカル フォルダーに関連付けます。</span><span class="sxs-lookup"><span data-stu-id="15fd1-199">Create an IIS website to serve the content, and associate it with the local folder.</span></span>
- <span data-ttu-id="15fd1-200">読み取り、アプリケーション プール id に、ローカル フォルダーに対する権限を付与します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-200">Grant read permissions to the application pool identity on the local folder.</span></span>

<span data-ttu-id="15fd1-201">阻止する IIS の既定の web サイトにコンテンツを展開するからですが、この方法はテストまたはデモのシナリオ以外の何かの推奨されません。</span><span class="sxs-lookup"><span data-stu-id="15fd1-201">Although there's nothing stopping you from deploying content to the default website in IIS, this approach is not recommended for anything other than test or demonstration scenarios.</span></span> <span data-ttu-id="15fd1-202">実稼働環境をシミュレートするには、アプリケーションの要件に固有の設定で新しい IIS web サイトを作成してください。</span><span class="sxs-lookup"><span data-stu-id="15fd1-202">To simulate a production environment, you should create a new IIS website with settings that are specific to the requirements of your application.</span></span>

<span data-ttu-id="15fd1-203">**作成し、IIS の web サイトを構成します。**</span><span class="sxs-lookup"><span data-stu-id="15fd1-203">**To create and configure an IIS website**</span></span>

1. <span data-ttu-id="15fd1-204">ローカル ファイル システムにコンテンツを保存するフォルダーを作成します (たとえば、 **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="15fd1-204">On the local file system, create a folder to store your content (for example, **C:\DemoSite**).</span></span>
2. <span data-ttu-id="15fd1-205">**開始** メニューのをポイント**管理ツール**、クリックして**インターネット インフォメーション サービス (IIS) マネージャー**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-205">On the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
3. <span data-ttu-id="15fd1-206">IIS マネージャーで、**接続** ウィンドウで、サーバー ノードを展開 (たとえば、 **TESTWEB1**)。</span><span class="sxs-lookup"><span data-stu-id="15fd1-206">In IIS Manager, in the **Connections** pane, expand the server node (for example, **TESTWEB1**).</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. <span data-ttu-id="15fd1-207">右クリックし、**サイト**ノードをクリックして**Web サイトの追加**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-207">Right-click the **Sites** node, and then click **Add Web Site**.</span></span>
5. <span data-ttu-id="15fd1-208">**サイト名**ボックスで、IIS の web サイトの名前を入力 (たとえば、 **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="15fd1-208">In the **Site name** box, type a name for the IIS website (for example, **DemoSite**).</span></span>
6. <span data-ttu-id="15fd1-209">**物理パス**ボックスに入力 (またはを参照)、ローカル フォルダーへのパス (たとえば、 **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="15fd1-209">In the **Physical path** box, type (or browse to) the path to your local folder (for example, **C:\DemoSite**).</span></span>
7. <span data-ttu-id="15fd1-210">**ポート**ボックスに、web サイトをホストするポート番号を入力 (たとえば、 **85**)。</span><span class="sxs-lookup"><span data-stu-id="15fd1-210">In the **Port** box, type the port number on which you want to host the website (for example, **85**).</span></span>

    > [!NOTE]
    > <span data-ttu-id="15fd1-211">標準のポート番号は http が 80 および HTTPS の場合は 443 です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-211">The standard port numbers are 80 for HTTP and 443 for HTTPS.</span></span> <span data-ttu-id="15fd1-212">ただし、ポート 80 では、この web サイトをホストしている場合は、サイトにアクセスするには、既定の web サイトを停止する必要があります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-212">However, if you host this website on port 80, you'll need to stop the default website before you can access your site.</span></span>
8. <span data-ttu-id="15fd1-213">ままにして、**ホスト名**ボックスをクリックして、web サイトのドメイン ネーム システム (DNS) レコードを構成するのでない限り、空白**OK**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-213">Leave the **Host name** box blank, unless you want to configure a Domain Name System (DNS) record for the website, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="15fd1-214">実稼働環境にすることができますをポート 80 で web サイトをホストし、DNS レコードを一致すると共に、ホスト ヘッダーを構成します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-214">In a production environment, you'll likely want to host your website on port 80 and configure a host header, together with matching DNS records.</span></span> <span data-ttu-id="15fd1-215">IIS 7 でのホスト ヘッダーを構成する方法については、次を参照してください。 [(IIS 7) の Web サイトのホスト ヘッダーを構成する](https://technet.microsoft.com/en-us/library/cc753195(WS.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-215">For more information on configuring host headers in IIS 7, see [Configure a Host Header for a Web Site (IIS 7)](https://technet.microsoft.com/en-us/library/cc753195(WS.10).aspx).</span></span> <span data-ttu-id="15fd1-216">Windows Server 2008 R2 の DNS サーバーの役割の詳細については、次を参照してください。 [DNS サーバーの概要](https://technet.microsoft.com/en-gb/library/cc770392.aspx)と[DNS Server](https://technet.microsoft.com/en-us/windowsserver/dd448607)です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-216">For more information on the DNS Server role in Windows Server 2008 R2, see [DNS Server Overview](https://technet.microsoft.com/en-gb/library/cc770392.aspx) and [DNS Server](https://technet.microsoft.com/en-us/windowsserver/dd448607).</span></span>
9. <span data-ttu-id="15fd1-217">**[アクション]** ウィンドウの **[サイトの編集]** の下にある **[バインド]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-217">In the **Actions** pane, under **Edit Site**, click **Bindings**.</span></span>
10. <span data-ttu-id="15fd1-218">**サイト バインド**ダイアログ ボックスで、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-218">In the **Site Bindings** dialog box, click **Add**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. <span data-ttu-id="15fd1-219">**サイト バインドの追加**ダイアログ ボックスで、設定、 **IP アドレス**と**ポート**既存サイトの構成に一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-219">In the **Add Site Binding** dialog box, set the **IP address** and **Port** to match your existing site configuration.</span></span>
12. <span data-ttu-id="15fd1-220">**ホスト名**ボックス、web サーバーの名前を入力 (たとえば、 **TESTWEB1**)、をクリックし、 **[ok]**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-220">In the **Host name** box, type the name of your web server (for example, **TESTWEB1**), and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > <span data-ttu-id="15fd1-221">最初のサイトのバインドでは、IP アドレスとポートを使用してローカル サイトにアクセスできます。 または`http://localhost:85`です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-221">The first site binding allows you to access the site locally using the IP address and port or `http://localhost:85`.</span></span> <span data-ttu-id="15fd1-222">2 つ目のサイト バインドでは、コンピューター名 (たとえば、http://testweb1:85) を使用して、ドメインの他のコンピューターからサイトにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="15fd1-222">The second site binding allows you to access the site from other computers on the domain using the machine name (for example, http://testweb1:85).</span></span>
13. <span data-ttu-id="15fd1-223">**サイト バインド**ダイアログ ボックスで、をクリックして**閉じる**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-223">In the **Site Bindings** dialog box, click **Close**.</span></span>
14. <span data-ttu-id="15fd1-224">**接続** ウィンドウで、をクリックして**アプリケーション プール**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-224">In the **Connections** pane, click **Application Pools**.</span></span>
15. <span data-ttu-id="15fd1-225">**アプリケーション プール** ウィンドウでは、アプリケーション プールの名前を右クリックし、をクリックして**基本設定**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-225">In the **Application Pools** pane, right-click the name of your application pool, and then click **Basic Settings**.</span></span> <span data-ttu-id="15fd1-226">既定では、アプリケーション プールの名前が、web サイトの名前に一致 (たとえば、 **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="15fd1-226">By default, the name of your application pool will match the name of your website (for example, **DemoSite**).</span></span>
16. <span data-ttu-id="15fd1-227">**.NET Framework のバージョン**一覧で、 **.NET Framework v4.0.30319**、順にクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-227">In the **.NET Framework version** list, select **.NET Framework v4.0.30319**, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="15fd1-228">サンプル ソリューションには、.NET Framework 4.0 が必要です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-228">The sample solution requires .NET Framework 4.0.</span></span> <span data-ttu-id="15fd1-229">これは、要件ではありません、Web Deploy の一般にします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-229">This is not a requirement for Web Deploy in general.</span></span>

<span data-ttu-id="15fd1-230">Web サイト コンテンツを提供するためには、アプリケーション プール id 読み取り権限が必要、コンテンツを格納するローカル フォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-230">In order for your website to serve content, the application pool identity must have read permissions on the local folder that stores the content.</span></span> <span data-ttu-id="15fd1-231">IIS 7.5、アプリケーション プールは、(ここでアプリケーション プールは通常の実行、Network Service アカウントを使用して、IIS の以前のバージョン) とは異なり、既定では、一意のアプリケーション プール id で実行します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-231">In IIS 7.5, application pools run with a unique application pool identity by default (in contrast to previous versions of IIS, where application pools would typically run using the Network Service account).</span></span> <span data-ttu-id="15fd1-232">アプリケーション プール id が実際のユーザー アカウントではないと、ユーザーまたはグループ & #x 2014 のすべてのリストに表示されないです。 代わりに、それが動的に作成、アプリケーション プールが開始されたときにします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-232">The application pool identity is not a real user account and does not show up on any lists of users or groups&#x2014;instead, it's created dynamically when the application pool is started.</span></span> <span data-ttu-id="15fd1-233">各アプリケーション プール id がローカルに追加**IIS\_IUSRS**セキュリティ グループを非表示のアイテムとして。</span><span class="sxs-lookup"><span data-stu-id="15fd1-233">Each application pool identity is added to the local **IIS\_IUSRS** security group as a hidden item.</span></span>

<span data-ttu-id="15fd1-234">ファイルまたはフォルダーでのアプリケーション プール id へのアクセス許可を付与するには、2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-234">To grant permissions to an application pool identity on a file or folder, you have two options:</span></span>

- <span data-ttu-id="15fd1-235">アクセス許可を割り当てるアプリケーション プール id に直接、形式を使用して**IIS AppPool\***[アプリケーション プール名] * (たとえば、 **IIS AppPool\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="15fd1-235">Assign permissions to the application pool identity directly, using the format **IIS AppPool\***[application pool name]*(for example, **IIS AppPool\DemoSite**).</span></span>
- <span data-ttu-id="15fd1-236">アクセス許可を割り当てる、 **IIS\_IUSRS**グループ。</span><span class="sxs-lookup"><span data-stu-id="15fd1-236">Assign permissions to the **IIS\_IUSRS** group.</span></span>

<span data-ttu-id="15fd1-237">最も一般的な方法は、ローカルに権限を割り当てる、 **IIS\_IUSRS**このアプローチでは、ファイル システム アクセス許可を再構成することがなくアプリケーション プールを変更することができますので、グループ化します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-237">The most common approach is to assign permissions to the local **IIS\_IUSRS** group because this approach lets you change application pools without reconfiguring file system permissions.</span></span> <span data-ttu-id="15fd1-238">次の手順では、このグループ ベースのアプローチを使用します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-238">The next procedure uses this group-based approach.</span></span>

> [!NOTE]
> <span data-ttu-id="15fd1-239">IIS 7.5 におけるアプリケーション プール id の詳細については、次を参照してください。[アプリケーション プール Id](https://go.microsoft.com/?linkid=9805123)です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-239">For more information on application pool identities in IIS 7.5, see [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).</span></span>


<span data-ttu-id="15fd1-240">**IIS の web サイトのフォルダーのアクセス許可を構成するには**</span><span class="sxs-lookup"><span data-stu-id="15fd1-240">**To configure folder permissions for an IIS website**</span></span>

1. <span data-ttu-id="15fd1-241">Windows エクスプ ローラーで、ローカル フォルダーの場所を参照します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-241">In Windows Explorer, browse to the location of your local folder.</span></span>
2. <span data-ttu-id="15fd1-242">フォルダーを右クリックし、をクリックして**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-242">Right-click the folder, and then click **Properties**.</span></span>
3. <span data-ttu-id="15fd1-243">**セキュリティ** タブで、をクリックして**編集**、クリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-243">On the **Security** tab, click **Edit**, and then click **Add**.</span></span>
4. <span data-ttu-id="15fd1-244">をクリックして**場所**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-244">Click **Locations**.</span></span> <span data-ttu-id="15fd1-245">**場所**ダイアログ ボックスでは、ローカル サーバーを選択し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-245">In the **Locations** dialog box, select the local server, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. <span data-ttu-id="15fd1-246">**ユーザーまたはグループ** ダイアログ ボックスで、「 **IIS\_IUSRS**、 をクリックして**名前の確認**順にクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-246">In the **Select Users or Groups** dialog box, type **IIS\_IUSRS**, click **Check Names**, and then click **OK**.</span></span>
6. <span data-ttu-id="15fd1-247">**のアクセス許可***[フォルダー名]* ダイアログ ボックスで、新しいグループが割り当てられている、**読み取り&amp;実行**、**フォルダーの一覧内容**、および**読み取り**既定のアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-247">In the **Permissions for***[folder name]*dialog box, notice that the new group has been assigned the **Read &amp; execute**, **List folder contents**, and **Read** permissions by default.</span></span> <span data-ttu-id="15fd1-248">これを変更せずのままにし、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-248">Leave this unchanged and click **OK**.</span></span>
7. <span data-ttu-id="15fd1-249">をクリックして**OK**を閉じる、 *[フォルダー名]***プロパティ** ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="15fd1-249">Click **OK** to close the *[folder name]***Properties** dialog box.</span></span>

<span data-ttu-id="15fd1-250">最後のタスクとして、サーバーにすべての web パッケージを展開する前に行う必要があります Web Deployment Agent サービスが実行されていること。</span><span class="sxs-lookup"><span data-stu-id="15fd1-250">As a final task before you attempt to deploy any web packages to your server, you should ensure that the Web Deployment Agent Service is running.</span></span> <span data-ttu-id="15fd1-251">リモート コンピューターからパッケージを展開するときに Web Deployment Agent サービスが、抽出およびパッケージの内容をインストールします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-251">When you deploy a package from a remote computer, the Web Deployment Agent Service is responsible for extracting and installing the contents of the package.</span></span> <span data-ttu-id="15fd1-252">サービスは、Web 配置ツールをインストールするときに、既定で開始され、Network Service の id で実行されます。</span><span class="sxs-lookup"><span data-stu-id="15fd1-252">The service is started by default when you install the Web Deployment Tool and runs under the Network Service identity.</span></span>

<span data-ttu-id="15fd1-253">確認できます、複数のさまざまな方法でサービスが実行されているかどうかさまざまなコマンド ライン ユーティリティまたは Windows PowerShell コマンドレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-253">You can check whether a service is running in multiple different ways, using various command-line utilities or Windows PowerShell cmdlets.</span></span> <span data-ttu-id="15fd1-254">この手順では、UI ベースの簡単な方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-254">This procedure describes a straightforward UI-based approach.</span></span>

<span data-ttu-id="15fd1-255">**Web Deployment Agent サービスが実行されていることを確認するには**</span><span class="sxs-lookup"><span data-stu-id="15fd1-255">**To check that the Web Deployment Agent Service is running**</span></span>

1. <span data-ttu-id="15fd1-256">**[スタート]** メニューで、 **[管理ツール]**をポイントして、 **[サービス]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-256">On the **Start** menu, point to **Administrative Tools**, and then click **Services**.</span></span>
2. <span data-ttu-id="15fd1-257">検索、 **Web Deployment Agent サービス**行をしていることを確認、**ステータス**に設定されている**Started**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-257">Locate the **Web Deployment Agent Service** row, and verify that the **Status** is set to **Started**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. <span data-ttu-id="15fd1-258">サービスが開始されていない場合はクリックして**開始**です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-258">If the service is not already started, click **Start**.</span></span>

## <a name="configure-firewall-exceptions"></a><span data-ttu-id="15fd1-259">ファイアウォールの例外を構成します。</span><span class="sxs-lookup"><span data-stu-id="15fd1-259">Configure Firewall Exceptions</span></span>

<span data-ttu-id="15fd1-260">既定では、リモート エージェント サービスはこの URL で、TCP ポート 80 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="15fd1-260">By default, the Remote Agent Service listens on TCP port 80, at this URL:</span></span>

<span data-ttu-id="15fd1-261">http://[*サーバー名*]/MSDEPLOYAGENTSERVICE</span><span class="sxs-lookup"><span data-stu-id="15fd1-261">http://[*server name*]/MSDEPLOYAGENTSERVICE</span></span>

<span data-ttu-id="15fd1-262">ほとんどの場合、web サーバーが通常リッスン ポート 80 で HTTP 要求のために、リモート エージェント サービスの追加のファイアウォール ルールを構成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="15fd1-262">In most cases, you won't need to configure any additional firewall rules for the Remote Agent Service because web servers typically listen for HTTP requests on port 80.</span></span> <span data-ttu-id="15fd1-263">非標準ポートでリッスンするように、インストールをカスタマイズする場合は、必要に応じてファイアウォールの例外を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-263">If you customized your installation to listen on a nonstandard port, you'll need to configure firewall exceptions as required.</span></span>

## <a name="conclusion"></a><span data-ttu-id="15fd1-264">まとめ</span><span class="sxs-lookup"><span data-stu-id="15fd1-264">Conclusion</span></span>

<span data-ttu-id="15fd1-265">この時点では、web サーバーをそのまま使用して、リモート コンピューターから web パッケージをインストールする準備ができてです。</span><span class="sxs-lookup"><span data-stu-id="15fd1-265">At this point, your web server is ready to accept and install web packages from a remote computer.</span></span> <span data-ttu-id="15fd1-266">Web アプリケーション サーバーを展開する前に、これらの要点をチェックすることがあります。</span><span class="sxs-lookup"><span data-stu-id="15fd1-266">Before you attempt to deploy a web application to the server, you may want to check these key points:</span></span>

- <span data-ttu-id="15fd1-267">IIS で ASP.NET 4.0 を登録したしますか。</span><span class="sxs-lookup"><span data-stu-id="15fd1-267">Have you registered ASP.NET 4.0 with IIS?</span></span>
- <span data-ttu-id="15fd1-268">アプリケーション プール id は、web サイトのソース フォルダーに読み取りアクセスがしますか。</span><span class="sxs-lookup"><span data-stu-id="15fd1-268">Does the application pool identity have read access to the source folder for your website?</span></span>
- <span data-ttu-id="15fd1-269">Web Deployment Agent サービスが実行されているか。</span><span class="sxs-lookup"><span data-stu-id="15fd1-269">Is the Web Deployment Agent Service running?</span></span>

## <a name="further-reading"></a><span data-ttu-id="15fd1-270">関連項目</span><span class="sxs-lookup"><span data-stu-id="15fd1-270">Further Reading</span></span>

<span data-ttu-id="15fd1-271">リモート エージェント サービスに web パッケージを配置するカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを構成する方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](configuring-deployment-properties-for-a-target-environment.md)です。</span><span class="sxs-lookup"><span data-stu-id="15fd1-271">For guidance on how to configure custom Microsoft Build Engine (MSBuild) project files to deploy web packages to the Remote Agent Service, see [Configuring Deployment Properties for a Target Environment](configuring-deployment-properties-for-a-target-environment.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="15fd1-272">[前へ](scenario-configuring-a-production-environment-for-web-deployment.md)
[次へ](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)</span><span class="sxs-lookup"><span data-stu-id="15fd1-272">[Previous](scenario-configuring-a-production-environment-for-web-deployment.md)
[Next](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)</span></span>
