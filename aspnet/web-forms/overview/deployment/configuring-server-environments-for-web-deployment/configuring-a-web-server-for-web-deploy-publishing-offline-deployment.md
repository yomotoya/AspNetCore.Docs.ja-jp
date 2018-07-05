---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: Web サーバーを構成する web 配置発行 (オフライン展開) |Microsoft Docs
author: jrjlee
description: このトピックでは、オフライン web 発行および配置をサポートするために IIS web サーバーを構成する方法について説明します。 インターネット インフォメーション サービスを使用する場合 (しました。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: 555b4b3c79d8efc641b1c179482993371735dbe2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397578"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a><span data-ttu-id="d4d54-104">Web 配置発行 (オフライン展開) の Web サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-104">Configuring a Web Server for Web Deploy Publishing (Offline Deployment)</span></span>
====================
<span data-ttu-id="d4d54-105">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="d4d54-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="d4d54-106">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="d4d54-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="d4d54-107">このトピックでは、オフライン web 発行および配置をサポートするために IIS web サーバーを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-107">This topic describes how to configure an IIS web server to support offline web publishing and deployment.</span></span>
> 
> <span data-ttu-id="d4d54-108">インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) 2.0 以降を使用するときに、アプリケーションまたは web サーバーにサイトを取得する際、3 つの主な方法があります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-108">When you work with Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0 or later, there are three main approaches you can use to get your applications or sites onto a web server.</span></span> <span data-ttu-id="d4d54-109">次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="d4d54-109">You can:</span></span>
> 
> - <span data-ttu-id="d4d54-110">使用して、 *Web デプロイ エージェントのリモート サービス*します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-110">Use the *Web Deploy Remote Agent Service*.</span></span> <span data-ttu-id="d4d54-111">このアプローチには、web サーバーの以下の構成が必要ですが、サーバーにデプロイするためにローカル サーバーの管理者の資格情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-111">This approach requires less configuration of the web server, but you need to provide the credentials of a local server administrator in order to deploy anything to the server.</span></span>
> - <span data-ttu-id="d4d54-112">使用して、 *Web 配置ハンドラー*します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-112">Use the *Web Deploy Handler*.</span></span> <span data-ttu-id="d4d54-113">このアプローチは、もっと複雑な web サーバーを設定する初期の労力が必要です。</span><span class="sxs-lookup"><span data-stu-id="d4d54-113">This approach is a lot more complex and requires more initial effort to set up the web server.</span></span> <span data-ttu-id="d4d54-114">ただし、このアプローチを使用する場合は、配置を実行する管理者以外のユーザーを許可するように IIS を構成できます。</span><span class="sxs-lookup"><span data-stu-id="d4d54-114">However, when you use this approach, you can configure IIS to allow non-administrator users to perform the deployment.</span></span> <span data-ttu-id="d4d54-115">Web 配置ハンドラーでは、IIS 7 以降のバージョンで利用できるのみです。</span><span class="sxs-lookup"><span data-stu-id="d4d54-115">The Web Deploy Handler is only available in IIS version 7 or later.</span></span>
> - <span data-ttu-id="d4d54-116">使用*オフライン展開*します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-116">Use *offline deployment*.</span></span> <span data-ttu-id="d4d54-117">このアプローチには、web サーバーの最低限の構成が必要ですが、サーバー管理者は、サーバー上に web パッケージのコピーし IIS マネージャーからインポートする必要があります手動でします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-117">This approach requires the least configuration of the web server, but a server administrator must manually copy the web package onto the server and import it through IIS Manager.</span></span>
> 
> <span data-ttu-id="d4d54-118">主な機能、利点、およびこれらのアプローチの欠点の詳細については、次を参照してください。 [Web 配置を右側のアプローチを選択](choosing-the-right-approach-to-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-118">For more information on the key features, advantages, and disadvantages of these approaches, see [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md).</span></span>


<span data-ttu-id="d4d54-119">はい、ネットワークのインフラストラクチャまたはセキュリティの制限は、リモート展開を防ぐ。</span><span class="sxs-lookup"><span data-stu-id="d4d54-119">Yes, if your network infrastructure or security restrictions prevent remote deployment.</span></span> <span data-ttu-id="d4d54-120">これは、インターネットに接続する運用環境で web サーバーが分離の場合に最も可能性の高い&#x2014;か、物理的にまたはファイアウォールや、によってサブネット&#x2014;、サーバー インフラストラクチャの残りの部分から。</span><span class="sxs-lookup"><span data-stu-id="d4d54-120">This is most likely to be the case in Internet-facing production environments, where the web servers are isolated&#x2014;either physically or by firewalls and subnets&#x2014;from the rest of your server infrastructure.</span></span>

<span data-ttu-id="d4d54-121">当然ながら、この方法は、web アプリケーションが定期的に更新された場合に望ましいになります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-121">Obviously, this approach becomes less desirable if your web applications are updated on a regular basis.</span></span> <span data-ttu-id="d4d54-122">場合は、インフラストラクチャにより、たい場合がありますリモートの展開を有効にしてください、Web 配置ハンドラーまたは Web デプロイ リモート エージェント サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-122">If your infrastructure allows it, you may want to consider enabling remote deployment, using either the Web Deploy Handler or the Web Deploy Remote Agent Service.</span></span>

## <a name="task-overview"></a><span data-ttu-id="d4d54-123">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="d4d54-123">Task Overview</span></span>

<span data-ttu-id="d4d54-124">オフラインでのインポートと web のパッケージの展開をサポートする web サーバーを構成するには、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-124">To configure the web server to support offline import and deployment of web packages, you'll need to:</span></span>

- <span data-ttu-id="d4d54-125">IIS 7.5、および推奨構成は、IIS 7 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-125">Install IIS 7.5 and the IIS 7 recommended configuration.</span></span>
- <span data-ttu-id="d4d54-126">Web Deploy 2.1 以降をインストールします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-126">Install Web Deploy 2.1 or later.</span></span>
- <span data-ttu-id="d4d54-127">展開されたコンテンツをホストする IIS の web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-127">Create an IIS website to host the deployed content.</span></span>
- <span data-ttu-id="d4d54-128">Web 配置エージェント サービスを無効にします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-128">Disable the Web Deployment Agent Service.</span></span>

<span data-ttu-id="d4d54-129">具体的には、サンプル ソリューションをホストにもする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-129">To host the sample solution specifically, you'll also need to:</span></span>

- <span data-ttu-id="d4d54-130">.NET Framework 4.0 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-130">Install the .NET Framework 4.0.</span></span>
- <span data-ttu-id="d4d54-131">ASP.NET MVC 3 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-131">Install ASP.NET MVC 3.</span></span>

<span data-ttu-id="d4d54-132">このトピックでは、これらの手順を実行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-132">This topic will show you how to perform each of these procedures.</span></span> <span data-ttu-id="d4d54-133">タスクとチュートリアルでは、このトピックでは、Windows Server 2008 R2 を実行しているサーバーのクリーン ビルドを開始することを想定しています。</span><span class="sxs-lookup"><span data-stu-id="d4d54-133">The tasks and walkthroughs in this topic assume that you're starting with a clean server build running Windows Server 2008 R2.</span></span> <span data-ttu-id="d4d54-134">続行する前にいることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-134">Before you continue, ensure that:</span></span>

- <span data-ttu-id="d4d54-135">Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="d4d54-135">Windows Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>
- <span data-ttu-id="d4d54-136">サーバーは、ドメインに参加しています。</span><span class="sxs-lookup"><span data-stu-id="d4d54-136">The server is domain-joined.</span></span>
- <span data-ttu-id="d4d54-137">サーバーは、静的 IP アドレスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="d4d54-137">The server has a static IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="d4d54-138">コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューターのドメインとログオン](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-138">For more information on joining computers to a domain, see [Joining Computers to the Domain and Logging On](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx).</span></span> <span data-ttu-id="d4d54-139">静的 IP アドレスの構成の詳細については、次を参照してください。[静的 IP アドレス構成](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-139">For more information on configuring static IP addresses, see [Configure a Static IP Address](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).</span></span>


## <a name="install-products-and-components"></a><span data-ttu-id="d4d54-140">製品とコンポーネントをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-140">Install Products and Components</span></span>

<span data-ttu-id="d4d54-141">このセクションで、web サーバーで必要な製品とコンポーネントのインストールを説明します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-141">This section will guide you through installing the required products and components on the web server.</span></span> <span data-ttu-id="d4d54-142">開始する前に、サーバーが完全に最新であることを確認する Windows 更新プログラムを実行することをお勧めは。</span><span class="sxs-lookup"><span data-stu-id="d4d54-142">Before you begin, a good practice is to run Windows Update to ensure that your server is fully up to date.</span></span>

<span data-ttu-id="d4d54-143">この場合、これらをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-143">In this case, you need to install these things:</span></span>

- <span data-ttu-id="d4d54-144">**IIS 7 推奨構成**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-144">**IIS 7 Recommended Configuration**.</span></span> <span data-ttu-id="d4d54-145">これにより、 **Web サーバー (IIS)** web サーバー上のロールと IIS モジュールと ASP.NET アプリケーションをホストするために必要なコンポーネントのセットをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-145">This enables the **Web Server (IIS)** role on your web server and installs the set of IIS modules and components that you need in order to host an ASP.NET application.</span></span>
- <span data-ttu-id="d4d54-146">**.NET framework 4.0**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-146">**.NET Framework 4.0**.</span></span> <span data-ttu-id="d4d54-147">これは、このバージョンの .NET Framework で構築されたアプリケーションの実行に必要です。</span><span class="sxs-lookup"><span data-stu-id="d4d54-147">This is required to run applications that were built on this version of the .NET Framework.</span></span>
- <span data-ttu-id="d4d54-148">**Web 配置ツール 2.1 以降**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-148">**Web Deployment Tool 2.1 or later**.</span></span> <span data-ttu-id="d4d54-149">これにより、Web Deploy (とその基になる実行可能ファイル、MSDeploy.exe) がサーバーにインストールされます。</span><span class="sxs-lookup"><span data-stu-id="d4d54-149">This installs Web Deploy (and its underlying executable, MSDeploy.exe) on your server.</span></span> <span data-ttu-id="d4d54-150">Web Deploy を使用して、IIS 統合、web パッケージ インポートおよびエクスポートすることができます。</span><span class="sxs-lookup"><span data-stu-id="d4d54-150">Web Deploy integrates with IIS and lets you import and export web packages.</span></span>
- <span data-ttu-id="d4d54-151">**ASP.NET MVC 3**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-151">**ASP.NET MVC 3**.</span></span> <span data-ttu-id="d4d54-152">MVC 3 アプリケーションを実行する必要があるアセンブリがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="d4d54-152">This installs the assemblies you need to run MVC 3 applications.</span></span>

> [!NOTE]
> <span data-ttu-id="d4d54-153">このチュートリアルでは、さまざまなコンポーネントをインストールして構成、Web Platform Installer の使用について説明します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-153">This walkthrough describes the use of the Web Platform Installer to install and configure various components.</span></span> <span data-ttu-id="d4d54-154">Web Platform Installer を使用する必要はありません、自動的に依存関係を検出して、常に製品の最新バージョンを取得することを確認して、インストール プロセスが簡略化します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-154">Although you don't have to use the Web Platform Installer, it simplifies the installation process by automatically detecting dependencies and ensuring that you always get the latest product versions.</span></span> <span data-ttu-id="d4d54-155">詳細については、次を参照してください。 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-155">For more information, see [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).</span></span>


<span data-ttu-id="d4d54-156">**必要な製品とコンポーネントをインストールするには**</span><span class="sxs-lookup"><span data-stu-id="d4d54-156">**To install the required products and components**</span></span>

1. <span data-ttu-id="d4d54-157">ダウンロードしてインストール、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-157">Download and install the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>
2. <span data-ttu-id="d4d54-158">インストールが完了したら、Web Platform Installer は自動的に起動されます。</span><span class="sxs-lookup"><span data-stu-id="d4d54-158">When installation is complete, the Web Platform Installer will launch automatically.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4d54-159">いつでも、Web Platform Installer を起動できるようになりました、**開始**メニュー。</span><span class="sxs-lookup"><span data-stu-id="d4d54-159">You can now launch the Web Platform Installer at any time from the **Start** menu.</span></span> <span data-ttu-id="d4d54-160">これを実行する、**開始** メニューのをクリックして**すべてのプログラム**、順にクリックします**Microsoft Web Platform Installer**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-160">To do this, on the **Start** menu, click **All Programs**, and then click **Microsoft Web Platform Installer**.</span></span>
3. <span data-ttu-id="d4d54-161">上部にある、 **Web Platform Installer 3.0**ウィンドウで、をクリックして**製品**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-161">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
4. <span data-ttu-id="d4d54-162">ナビゲーション ウィンドウで、ウィンドウの左側にある クリックして**フレームワーク**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-162">On the left side of the window, in the navigation pane, click **Frameworks**.</span></span>
5. <span data-ttu-id="d4d54-163">**Microsoft .NET Framework 4** 、.NET Framework が既にインストールされていない場合、行をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-163">In the **Microsoft .NET Framework 4** row, if the .NET Framework is not already installed, click **Add**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4d54-164">既にインストールしている Windows Update を通じて、.NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="d4d54-164">You may have already installed the .NET Framework 4.0 through Windows Update.</span></span> <span data-ttu-id="d4d54-165">製品またはコンポーネントが既にインストールされている場合、Web Platform Installer 示されます置き換えることで、**追加**ボタン テキストを含む**インストール済み**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-165">If a product or component is already installed, the Web Platform Installer will indicate this by replacing the **Add** button with the text **Installed**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. <span data-ttu-id="d4d54-166">**ASP.NET MVC 3 (Visual Studio 2010)** 行で、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-166">In the **ASP.NET MVC 3 (Visual Studio 2010)** row, click **Add**.</span></span>
7. <span data-ttu-id="d4d54-167">ナビゲーション ウィンドウで、 **Server**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-167">In the navigation pane, click **Server**.</span></span>
8. <span data-ttu-id="d4d54-168">**IIS 7 推奨構成**行で、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-168">In the **IIS 7 Recommended Configuration** row, click **Add**.</span></span>
9. <span data-ttu-id="d4d54-169">**Web 配置ツール 2.1**行で、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-169">In the **Web Deployment Tool 2.1** row, click **Add**.</span></span>
10. <span data-ttu-id="d4d54-170">**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-170">Click **Install**.</span></span> <span data-ttu-id="d4d54-171">Web Platform Installer が製品の一覧を表示&#x2014;と関連する依存関係&#x2014;をインストールし、ライセンス条項に同意するように求められます。</span><span class="sxs-lookup"><span data-stu-id="d4d54-171">The Web Platform Installer will show you a list of products&#x2014;together with any associated dependencies&#x2014;to be installed and will prompt you to accept the license terms.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. <span data-ttu-id="d4d54-172">ライセンス条項を確認し、条項に同意する場合にクリックします**同意**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-172">Review the license terms, and if you consent to the terms, click **I Accept**.</span></span>
12. <span data-ttu-id="d4d54-173">インストールが完了したら、クリックして**完了**、閉じて、 **Web Platform Installer 3.0**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="d4d54-173">When the installation is complete, click **Finish**, and then close the **Web Platform Installer 3.0** window.</span></span>

<span data-ttu-id="d4d54-174">IIS をインストールする前に、.NET Framework 4.0 をインストールした場合を実行する必要があります、 [ASP.NET IIS 登録ツール](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) を IIS に ASP.NET の最新バージョンを登録します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-174">If you installed the .NET Framework 4.0 before you installed IIS, you'll need to run the [ASP.NET IIS Registration Tool](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) to register the latest version of ASP.NET with IIS.</span></span> <span data-ttu-id="d4d54-175">これを行わないと、見つかります IIS が静的なコンテンツを提供 (HTML ファイル) など、問題なくが返されます**HTTP エラー 404.0 – 見つかりません**しようとすると、ASP.NET のコンテンツを参照します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-175">If you don't do this, you'll find that IIS will serve static content (like HTML files) without any problems, but it will return **HTTP Error 404.0 – Not Found** when you attempt to browse to ASP.NET content.</span></span> <span data-ttu-id="d4d54-176">次の手順を使用すると、ASP.NET 4.0 が登録されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-176">You can use the next procedure to ensure that ASP.NET 4.0 is registered.</span></span>

<span data-ttu-id="d4d54-177">**ASP.NET 4.0 を IIS に登録するには**</span><span class="sxs-lookup"><span data-stu-id="d4d54-177">**To register ASP.NET 4.0 with IIS**</span></span>

1. <span data-ttu-id="d4d54-178">クリックして**開始**、し、入力**コマンド プロンプト**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-178">Click **Start**, and then type **Command Prompt**.</span></span>
2. <span data-ttu-id="d4d54-179">右クリックし、検索結果で**コマンド プロンプト**、 をクリックし、**管理者として実行**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-179">In the search results, right-click **Command Prompt**, and then click **Run as administrator**.</span></span>
3. <span data-ttu-id="d4d54-180">コマンド プロンプト ウィンドウに移動、 **%WINDIR%\Microsoft.NET\Framework\v4.0.30319**ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="d4d54-180">In the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.</span></span>
4. <span data-ttu-id="d4d54-181">このコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-181">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. <span data-ttu-id="d4d54-182">任意の時点の 64 ビット web アプリケーションをホストする場合は、IIS と ASP.NET の 64 ビット バージョンを登録することも必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-182">If you plan to host 64-bit web applications at any point, you should also register the 64-bit version of ASP.NET with IIS.</span></span> <span data-ttu-id="d4d54-183">コマンド プロンプト ウィンドウに移動します。、 **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="d4d54-183">To do this, in the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.</span></span>
6. <span data-ttu-id="d4d54-184">このコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-184">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

<span data-ttu-id="d4d54-185">お勧めとして Windows Update を使用してもう一度この時点でダウンロードして、新製品とコンポーネントをインストールしたら、使用可能な更新プログラムをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-185">As a good practice, use Windows Update again at this point to download and install any available updates for the new products and components you've installed.</span></span>

## <a name="configure-the-iis-website"></a><span data-ttu-id="d4d54-186">IIS web サイトを構成します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-186">Configure the IIS Website</span></span>

<span data-ttu-id="d4d54-187">Web コンテンツを展開するには、サーバーに、前に作成してコンテンツをホストする IIS の web サイトを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-187">Before you can deploy web content to your server, you need to create and configure an IIS website to host the content.</span></span> <span data-ttu-id="d4d54-188">Web Deploy; 既存の IIS web サイトに web パッケージを配置できるだけweb サイトを作成できません。</span><span class="sxs-lookup"><span data-stu-id="d4d54-188">Web Deploy can only deploy web packages to an existing IIS website; it can't create the website for you.</span></span> <span data-ttu-id="d4d54-189">大まかに言えば、これらのタスクを完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-189">At a high level, you'll need to complete these tasks:</span></span>

- <span data-ttu-id="d4d54-190">コンテンツをホストするファイル システム上のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-190">Create a folder on the file system to host your content.</span></span>
- <span data-ttu-id="d4d54-191">コンテンツを処理するために IIS の web サイトを作成し、ローカル フォルダーに関連付けます。</span><span class="sxs-lookup"><span data-stu-id="d4d54-191">Create an IIS website to serve the content, and associate it with the local folder.</span></span>
- <span data-ttu-id="d4d54-192">読み取りローカル フォルダーにアプリケーション プール id へのアクセス許可を付与します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-192">Grant read permissions to the application pool identity on the local folder.</span></span>

<span data-ttu-id="d4d54-193">IIS の既定の web サイトにコンテンツを展開するなんらですが、この方法はテストまたはデモ シナリオ以外は推奨されません。</span><span class="sxs-lookup"><span data-stu-id="d4d54-193">Although there's nothing stopping you from deploying content to the default website in IIS, this approach is not recommended for anything other than test or demonstration scenarios.</span></span> <span data-ttu-id="d4d54-194">運用環境をシミュレートするには、アプリケーションの要件に固有の設定で新しい IIS web サイトを作成してください。</span><span class="sxs-lookup"><span data-stu-id="d4d54-194">To simulate a production environment, you should create a new IIS website with settings that are specific to the requirements of your application.</span></span>

<span data-ttu-id="d4d54-195">**作成および IIS の web サイトを構成するには**</span><span class="sxs-lookup"><span data-stu-id="d4d54-195">**To create and configure an IIS website**</span></span>

1. <span data-ttu-id="d4d54-196">ローカル ファイル システムでコンテンツを格納するフォルダーを作成します (たとえば、 **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="d4d54-196">On the local file system, create a folder to store your content (for example, **C:\DemoSite**).</span></span>
2. <span data-ttu-id="d4d54-197">**開始**メニューで、**管理ツール**、 をクリックし、**インターネット インフォメーション サービス (IIS) マネージャー**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-197">On the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
3. <span data-ttu-id="d4d54-198">IIS マネージャーで、**接続**ウィンドウで、サーバー ノードを展開 (たとえば、 **PROWEB1**)。</span><span class="sxs-lookup"><span data-stu-id="d4d54-198">In IIS Manager, in the **Connections** pane, expand the server node (for example, **PROWEB1**).</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. <span data-ttu-id="d4d54-199">右クリックし、**サイト**ノード、およびクリック**Web サイトの追加**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-199">Right-click the **Sites** node, and then click **Add Web Site**.</span></span>
5. <span data-ttu-id="d4d54-200">**サイト名**ボックスに、IIS web サイトの名前を入力 (たとえば、 **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="d4d54-200">In the **Site name** box, type a name for the IIS website (for example, **DemoSite**).</span></span>
6. <span data-ttu-id="d4d54-201">**物理パス**ボックス、入力 (またはを参照)、ローカル フォルダーへのパス (たとえば、 **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="d4d54-201">In the **Physical path** box, type (or browse to) the path to your local folder (for example, **C:\DemoSite**).</span></span>
7. <span data-ttu-id="d4d54-202">**ポート**ボックスに、web サイトをホストするポート番号を入力 (たとえば、 **85**)。</span><span class="sxs-lookup"><span data-stu-id="d4d54-202">In the **Port** box, type the port number on which you want to host the website (for example, **85**).</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4d54-203">標準ポート番号は、http 80 および HTTPS の場合は 443 です。</span><span class="sxs-lookup"><span data-stu-id="d4d54-203">The standard port numbers are 80 for HTTP and 443 for HTTPS.</span></span> <span data-ttu-id="d4d54-204">ただし、ポート 80 では、この web サイトをホストしている場合は、サイトにアクセスするには、既定の web サイトを停止する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-204">However, if you host this website on port 80, you'll need to stop the default website before you can access your site.</span></span>
8. <span data-ttu-id="d4d54-205">ままに、**ホスト名**ボックスをクリックして、web サイト用のドメイン ネーム システム (DNS) レコードを構成する場合、空白**OK**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-205">Leave the **Host name** box blank, unless you want to configure a Domain Name System (DNS) record for the website, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="d4d54-206">運用環境では、ポート 80 で web サイトをホストし、DNS レコードを一致すると共に、ホスト ヘッダーを構成するします可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-206">In a production environment, you'll likely want to host your website on port 80 and configure a host header, together with matching DNS records.</span></span> <span data-ttu-id="d4d54-207">IIS 7 でホスト ヘッダーの構成の詳細については、次を参照してください。 [(IIS 7) の Web サイトのホスト ヘッダーを構成する](https://technet.microsoft.com/library/cc753195(WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-207">For more information on configuring host headers in IIS 7, see [Configure a Host Header for a Web Site (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx).</span></span> <span data-ttu-id="d4d54-208">Windows Server 2008 R2 の DNS サーバーの役割の詳細については、次を参照してください。 [DNS サーバーの概要](https://technet.microsoft.com/en-gb/library/cc770392.aspx)と[DNS Server](https://technet.microsoft.com/windowsserver/dd448607)します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-208">For more information on the DNS Server role in Windows Server 2008 R2, see [DNS Server Overview](https://technet.microsoft.com/en-gb/library/cc770392.aspx) and [DNS Server](https://technet.microsoft.com/windowsserver/dd448607).</span></span>
9. <span data-ttu-id="d4d54-209">**[アクション]** ウィンドウの **[サイトの編集]** の下にある **[バインド]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-209">In the **Actions** pane, under **Edit Site**, click **Bindings**.</span></span>
10. <span data-ttu-id="d4d54-210">**サイト バインド**ダイアログ ボックスで、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-210">In the **Site Bindings** dialog box, click **Add**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. <span data-ttu-id="d4d54-211">**サイト バインドの追加**ダイアログ ボックスで、セット、 **IP アドレス**と**ポート**既存のサイトの構成に一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-211">In the **Add Site Binding** dialog box, set the **IP address** and **Port** to match your existing site configuration.</span></span>
12. <span data-ttu-id="d4d54-212">**ホスト名**ボックス、web サーバーの名前を入力 (たとえば、 **PROWEB1**)、順にクリックします**OK**。</span><span class="sxs-lookup"><span data-stu-id="d4d54-212">In the **Host name** box, type the name of your web server (for example, **PROWEB1**), and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > <span data-ttu-id="d4d54-213">最初のサイトのバインドでは、IP アドレスとポートを使用してローカル サイトにアクセスできます。 または`http://localhost:85`します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-213">The first site binding allows you to access the site locally using the IP address and port or `http://localhost:85`.</span></span> <span data-ttu-id="d4d54-214">2 つ目のサイトのバインドでは、マシン名を使用して、ドメインの他のコンピューターからサイトにアクセスできます (たとえば、http://proweb1:85)します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-214">The second site binding allows you to access the site from other computers on the domain using the machine name (for example, http://proweb1:85).</span></span>
13. <span data-ttu-id="d4d54-215">**サイト バインド**ダイアログ ボックスで、をクリックして**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-215">In the **Site Bindings** dialog box, click **Close**.</span></span>
14. <span data-ttu-id="d4d54-216">**接続**ウィンドウで、をクリックして**アプリケーション プール**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-216">In the **Connections** pane, click **Application Pools**.</span></span>
15. <span data-ttu-id="d4d54-217">**アプリケーション プール**ウィンドウは、アプリケーション プールの名前を右クリックし、クリックして**基本設定**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-217">In the **Application Pools** pane, right-click the name of your application pool, and then click **Basic Settings**.</span></span> <span data-ttu-id="d4d54-218">既定では、アプリケーション プールの名前が、web サイトの名前に一致 (たとえば、 **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="d4d54-218">By default, the name of your application pool will match the name of your website (for example, **DemoSite**).</span></span>
16. <span data-ttu-id="d4d54-219">**.NET Framework のバージョン**一覧で、 **.NET Framework v4.0.30319**、順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-219">In the **.NET Framework version** list, select **.NET Framework v4.0.30319**, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="d4d54-220">サンプル ソリューションでは、.NET Framework 4.0 が必要です。</span><span class="sxs-lookup"><span data-stu-id="d4d54-220">The sample solution requires .NET Framework 4.0.</span></span> <span data-ttu-id="d4d54-221">これではなく Web デプロイのための要件です。</span><span class="sxs-lookup"><span data-stu-id="d4d54-221">This is not a requirement for Web Deploy in general.</span></span>

<span data-ttu-id="d4d54-222">Web サイト コンテンツを配信するためには、アプリケーション プール id をコンテンツを格納するローカル フォルダーのアクセス許可読み取りが必要です。</span><span class="sxs-lookup"><span data-stu-id="d4d54-222">In order for your website to serve content, the application pool identity must have read permissions on the local folder that stores the content.</span></span> <span data-ttu-id="d4d54-223">IIS 7.5 では、アプリケーション プールは、(アプリケーション プールが通常実行されるネットワーク サービス アカウントを使用して、IIS の以前のバージョン) とは異なり、既定で一意のアプリケーション プール id で実行します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-223">In IIS 7.5, application pools run with a unique application pool identity by default (in contrast to previous versions of IIS, where application pools would typically run using the Network Service account).</span></span> <span data-ttu-id="d4d54-224">アプリケーション プール id の実際のユーザー アカウントではないとユーザーまたはグループのすべてのリストが表示されない&#x2014;代わりにときに、作成されます動的にアプリケーション プールを開始します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-224">The application pool identity is not a real user account and does not show up on any lists of users or groups&#x2014;instead, it's created dynamically when the application pool is started.</span></span> <span data-ttu-id="d4d54-225">各アプリケーション プール id をローカルに追加**IIS\_IUSRS**セキュリティ グループを非表示の項目として。</span><span class="sxs-lookup"><span data-stu-id="d4d54-225">Each application pool identity is added to the local **IIS\_IUSRS** security group as a hidden item.</span></span>

<span data-ttu-id="d4d54-226">ファイルまたはフォルダーでのアプリケーション プール id へのアクセス許可を付与するには、2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-226">To grant permissions to an application pool identity on a file or folder, you have two options:</span></span>

- <span data-ttu-id="d4d54-227">アクセス許可を割り当てる、アプリケーション プール id に直接、形式を使用して<strong>IIS AppPool\</strong ><em>[アプリケーション プール名]</em>(たとえば、 <strong>IIS AppPool\DemoSite</strong>).</span><span class="sxs-lookup"><span data-stu-id="d4d54-227">Assign permissions to the application pool identity directly, using the format <strong>IIS AppPool\</strong><em>[application pool name]</em>(for example, <strong>IIS AppPool\DemoSite</strong>).</span></span>
- <span data-ttu-id="d4d54-228">アクセス許可を割り当てる、 **IIS\_IUSRS**グループ。</span><span class="sxs-lookup"><span data-stu-id="d4d54-228">Assign permissions to the **IIS\_IUSRS** group.</span></span>

<span data-ttu-id="d4d54-229">最も一般的なアプローチは、ローカルに権限を割り当てる、 **IIS\_IUSRS**グループ、このアプローチでは、ファイル システム アクセス許可を再構成することがなくアプリケーション プールを変更できるためです。</span><span class="sxs-lookup"><span data-stu-id="d4d54-229">The most common approach is to assign permissions to the local **IIS\_IUSRS** group, because this approach lets you change application pools without reconfiguring file system permissions.</span></span> <span data-ttu-id="d4d54-230">次の手順では、このグループ ベースのアプローチを使用します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-230">The next procedure uses this group-based approach.</span></span>

> [!NOTE]
> <span data-ttu-id="d4d54-231">IIS 7.5 でアプリケーション プール id の詳細については、次を参照してください。[アプリケーション プール Id](https://go.microsoft.com/?linkid=9805123)します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-231">For more information on application pool identities in IIS 7.5, see [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).</span></span>


<span data-ttu-id="d4d54-232">**IIS の web サイトのフォルダーのアクセス許可を構成するには**</span><span class="sxs-lookup"><span data-stu-id="d4d54-232">**To configure folder permissions for an IIS website**</span></span>

1. <span data-ttu-id="d4d54-233">Windows エクスプ ローラーでは、ローカル フォルダーの場所を参照します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-233">In Windows Explorer, browse to the location of your local folder.</span></span>
2. <span data-ttu-id="d4d54-234">フォルダーを右クリックし、**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-234">Right-click the folder, and then click **Properties**.</span></span>
3. <span data-ttu-id="d4d54-235">**セキュリティ**] タブで [**編集**、順にクリックします**追加**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-235">On the **Security** tab, click **Edit**, and then click **Add**.</span></span>
4. <span data-ttu-id="d4d54-236">クリックして**場所**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-236">Click **Locations**.</span></span> <span data-ttu-id="d4d54-237">**場所** ダイアログ ボックスでは、ローカルのサーバーを選択し、順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-237">In the **Locations** dialog box, select the local server, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. <span data-ttu-id="d4d54-238">**[ユーザーまたはグループ**ダイアログ ボックスに「 **IIS\_IUSRS**、] をクリックして**名前の確認**順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-238">In the **Select Users or Groups** dialog box, type **IIS\_IUSRS**, click **Check Names**, and then click **OK**.</span></span>
6. <span data-ttu-id="d4d54-239"><strong>のアクセス許可</strong><em>[フォルダー名]</em>ダイアログ ボックスで、新しいグループが割り当てられている通知、<strong>読み取り&amp;実行</strong>、<strong>フォルダーの一覧内容</strong>、および<strong>読み取り</strong>既定のアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-239">In the <strong>Permissions for</strong><em>[folder name]</em> dialog box, notice that the new group has been assigned the <strong>Read &amp; execute</strong>, <strong>List folder contents</strong>, and <strong>Read</strong> permissions by default.</span></span> <span data-ttu-id="d4d54-240">変更なしのままにし、クリックして<strong>OK</strong>します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-240">Leave this unchanged and click <strong>OK</strong>.</span></span>
7. <span data-ttu-id="d4d54-241">をクリックして<strong>OK</strong>を閉じる、 <em>[フォルダー名]</em><strong>プロパティ</strong> ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="d4d54-241">Click <strong>OK</strong> to close the <em>[folder name]</em><strong>Properties</strong> dialog box.</span></span>

## <a name="disable-the-remote-agent-service"></a><span data-ttu-id="d4d54-242">リモート エージェント サービスを無効にします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-242">Disable the Remote Agent Service</span></span>

<span data-ttu-id="d4d54-243">Web Deploy をインストールするときに Web 配置エージェント サービスがインストールされているし、自動的に開始します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-243">When you install Web Deploy, the Web Deployment Agent Service is installed and started automatically.</span></span> <span data-ttu-id="d4d54-244">このサービスを展開し、リモートの場所から web パッケージを公開することができます。</span><span class="sxs-lookup"><span data-stu-id="d4d54-244">This service allows you to deploy and publish web packages from a remote location.</span></span> <span data-ttu-id="d4d54-245">使用しないリモート展開機能でこのシナリオでは、ため、停止し、サービスを無効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-245">You won't be using the remote deployment capability in this scenario, so you should stop and disable the service.</span></span>

> [!NOTE]
> <span data-ttu-id="d4d54-246">インポートおよび web パッケージを手動で展開するためにリモート エージェント サービスを停止する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d4d54-246">You don't need to stop the remote agent service in order to import and deploy a web package manually.</span></span> <span data-ttu-id="d4d54-247">ただし、なりますを停止し、それを使用する予定がない場合は、サービスを無効にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-247">However, it's a good practice to stop and disable the service if you don't plan to use it.</span></span>


<span data-ttu-id="d4d54-248">停止して、さまざまなコマンド ライン ユーティリティまたは Windows PowerShell コマンドレットを使用して、複数の方法でサービスを無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="d4d54-248">You can stop and disable a service in multiple ways, using various command-line utilities or Windows PowerShell cmdlets.</span></span> <span data-ttu-id="d4d54-249">この手順では、簡単な UI ベースのアプローチについて説明します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-249">This procedure describes a straightforward UI-based approach.</span></span>

<span data-ttu-id="d4d54-250">**停止し、リモート エージェント サービスを無効にします。**</span><span class="sxs-lookup"><span data-stu-id="d4d54-250">**To stop and disable the remote agent service**</span></span>

1. <span data-ttu-id="d4d54-251">**[スタート]** メニューで、 **[管理ツール]** をポイントして、 **[サービス]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d4d54-251">On the **Start** menu, point to **Administrative Tools**, and then click **Services**.</span></span>
2. <span data-ttu-id="d4d54-252">サービス コンソールで見つけ、 **Web Deployment Agent サービス**行。</span><span class="sxs-lookup"><span data-stu-id="d4d54-252">In the Services console, locate the **Web Deployment Agent Service** row.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. <span data-ttu-id="d4d54-253">右クリック**Web Deployment Agent サービス**、 をクリックし、**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-253">Right-click **Web Deployment Agent Service**, and then click **Properties**.</span></span>
4. <span data-ttu-id="d4d54-254">**Web デプロイ エージェントのサービス プロパティ**ダイアログ ボックスで、をクリックして**停止**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-254">In the **Web Deployment Agent Service Properties** dialog box, click **Stop**.</span></span>
5. <span data-ttu-id="d4d54-255">**スタートアップの種類**一覧で、**無効になっている**、順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="d4d54-255">In the **Startup type** list, select **Disabled**, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a><span data-ttu-id="d4d54-256">まとめ</span><span class="sxs-lookup"><span data-stu-id="d4d54-256">Conclusion</span></span>

<span data-ttu-id="d4d54-257">この時点で、web サーバーがオフライン web パッケージの配置の準備完了です。</span><span class="sxs-lookup"><span data-stu-id="d4d54-257">At this point, your web server is ready for offline web package deployment.</span></span> <span data-ttu-id="d4d54-258">IIS の web サイトに web パッケージをインポートする前に、次の点を確認したい場合があります。</span><span class="sxs-lookup"><span data-stu-id="d4d54-258">Before you attempt to import web packages to an IIS website, you may want to check these key points:</span></span>

- <span data-ttu-id="d4d54-259">IIS で ASP.NET 4.0 を登録したでしょうか。</span><span class="sxs-lookup"><span data-stu-id="d4d54-259">Have you registered ASP.NET 4.0 with IIS?</span></span>
- <span data-ttu-id="d4d54-260">アプリケーション プール id は、web サイトのソース フォルダーに読み取りアクセスをあるでしょうか。</span><span class="sxs-lookup"><span data-stu-id="d4d54-260">Does the application pool identity have read access to the source folder for your website?</span></span>
- <span data-ttu-id="d4d54-261">Web 配置エージェント サービスを停止したか。</span><span class="sxs-lookup"><span data-stu-id="d4d54-261">Have you stopped the Web Deployment Agent Service?</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d4d54-262">[前へ](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
> [次へ](configuring-a-database-server-for-web-deploy-publishing.md)</span><span class="sxs-lookup"><span data-stu-id="d4d54-262">[Previous](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
[Next](configuring-a-database-server-for-web-deploy-publishing.md)</span></span>
