---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: Web サーバーを構成する web 配置発行 (Web 配置ハンドラー) |Microsoft Docs
author: jrjlee
description: このトピックでは、web の発行および IIS Web 配置 Han を使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: 2d262aab8fd6e755330acbaffa6d18c29688666f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394217"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a><span data-ttu-id="7543b-103">Web サーバーを構成する web 配置発行 (Web 配置ハンドラー)</span><span class="sxs-lookup"><span data-stu-id="7543b-103">Configuring a Web Server for Web Deploy Publishing (Web Deploy Handler)</span></span>
====================
<span data-ttu-id="7543b-104">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="7543b-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="7543b-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="7543b-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="7543b-106">このトピックでは、web の発行および IIS Web 配置ハンドラーを使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="7543b-106">This topic describes how to configure an Internet Information Services (IIS) web server to support web publishing and deployment using the IIS Web Deploy Handler.</span></span>
> 
> <span data-ttu-id="7543b-107">Web Deploy 2.0 以降を操作するときに、アプリケーションまたは web サーバーにサイトを取得する際、3 つの主な方法があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-107">When you work with Web Deploy 2.0 or later, there are three main approaches you can use to get your applications or sites onto a web server.</span></span> <span data-ttu-id="7543b-108">次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="7543b-108">You can:</span></span>
> 
> - <span data-ttu-id="7543b-109">使用して、 *Web デプロイ エージェントのリモート サービス*します。</span><span class="sxs-lookup"><span data-stu-id="7543b-109">Use the *Web Deploy Remote Agent Service*.</span></span> <span data-ttu-id="7543b-110">このアプローチには、web サーバーの以下の構成が必要ですが、サーバーにデプロイするためにローカル サーバーの管理者の資格情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-110">This approach requires less configuration of the web server, but you need to provide the credentials of a local server administrator in order to deploy anything to the server.</span></span>
> - <span data-ttu-id="7543b-111">使用して、 *Web 配置ハンドラー*します。</span><span class="sxs-lookup"><span data-stu-id="7543b-111">Use the *Web Deploy Handler*.</span></span> <span data-ttu-id="7543b-112">このアプローチは、もっと複雑な web サーバーを設定する初期の労力が必要です。</span><span class="sxs-lookup"><span data-stu-id="7543b-112">This approach is a lot more complex and requires more initial effort to set up the web server.</span></span> <span data-ttu-id="7543b-113">ただし、このアプローチを使用する場合は、配置を実行する管理者以外のユーザーを許可するように IIS を構成できます。</span><span class="sxs-lookup"><span data-stu-id="7543b-113">However, when you use this approach, you can configure IIS to allow non-administrator users to perform the deployment.</span></span> <span data-ttu-id="7543b-114">Web 配置ハンドラーでは、IIS 7 以降のバージョンで利用できるのみです。</span><span class="sxs-lookup"><span data-stu-id="7543b-114">The Web Deploy Handler is only available in IIS version 7 or later.</span></span>
> - <span data-ttu-id="7543b-115">使用*オフライン展開*します。</span><span class="sxs-lookup"><span data-stu-id="7543b-115">Use *offline deployment*.</span></span> <span data-ttu-id="7543b-116">このアプローチには、web サーバーの最低限の構成が必要ですが、サーバー管理者は、サーバー上に web パッケージのコピーし IIS マネージャーからインポートする必要があります手動でします。</span><span class="sxs-lookup"><span data-stu-id="7543b-116">This approach requires the least configuration of the web server, but a server administrator must manually copy the web package onto the server and import it through IIS Manager.</span></span>
> 
> <span data-ttu-id="7543b-117">主な機能、利点、およびこれらのアプローチの欠点の詳細については、次を参照してください。 [Web 配置を右側のアプローチを選択](choosing-the-right-approach-to-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="7543b-117">For more information on the key features, advantages, and disadvantages of these approaches, see [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md).</span></span>


<span data-ttu-id="7543b-118">特定の IIS web サイトにコンテンツを展開する管理者以外のユーザーを許可する場合ははい。</span><span class="sxs-lookup"><span data-stu-id="7543b-118">Yes, if you want to allow non-administrator users to deploy content to specific IIS websites.</span></span> <span data-ttu-id="7543b-119">この方法は、これらの種類のシナリオでは望ましくは多くの場合です。</span><span class="sxs-lookup"><span data-stu-id="7543b-119">This approach is often desirable in these types of scenarios:</span></span>

- <span data-ttu-id="7543b-120">ステージング環境または運用環境、リモート展開をトリガーする人物またはサービス アカウントは、サーバー管理者の資格情報にアクセスする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-120">Staging or production environments, where the person or service account that triggers the remote deployment is unlikely to have access to the credentials of a server administrator.</span></span>
- <span data-ttu-id="7543b-121">リモート ユーザーが web サーバー (または他のユーザーの web サイトへのアクセス) のフル コントロールを与えることがなく、web サイトを更新できるようにするホスト環境です。</span><span class="sxs-lookup"><span data-stu-id="7543b-121">Hosted environments, where you want to give remote users the ability to update their websites without giving them full control of your web servers (or access to anyone else's websites).</span></span>

<span data-ttu-id="7543b-122">開発またはテストのシナリオまたは小規模な組織では、サーバー管理者の資格情報を使用してコンテンツの展開は、多くの場合、少ない頻度。</span><span class="sxs-lookup"><span data-stu-id="7543b-122">In development or test scenarios, or in smaller organizations, deploying content using server administrator credentials is often less contentious.</span></span> <span data-ttu-id="7543b-123">これらのシナリオで使用した展開をサポートするために web サーバーの構成、[デプロイのリモート エージェント サービスを Web](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)より直接的な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="7543b-123">In these scenarios, configuring your web servers to support deployment using the [Web Deploy Remote Agent Service](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) offers a more straightforward approach.</span></span>

## <a name="task-overview"></a><span data-ttu-id="7543b-124">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="7543b-124">Task Overview</span></span>

<span data-ttu-id="7543b-125">そのまま使用し、Web 配置ハンドラーのアプローチを使用してリモート コンピューターから web パッケージを配置する web サーバーを構成するには、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-125">To configure the web server to accept and deploy web packages from a remote computer using the Web Deploy Handler approach, you'll need to:</span></span>

- <span data-ttu-id="7543b-126">作成、または展開の実行に使用する資格情報を持つドメイン ユーザー アカウント (「管理者以外のユーザー」) を選択します。</span><span class="sxs-lookup"><span data-stu-id="7543b-126">Create, or choose, a domain user account (the "non-administrator user") whose credentials you'll use to perform deployments.</span></span>
- <span data-ttu-id="7543b-127">IIS 7.5、Web 管理サービスなど、基本的な認証モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="7543b-127">Install IIS 7.5, including the Web Management Service and the Basic Authentication module.</span></span>
- <span data-ttu-id="7543b-128">Web Deploy 2.1 以降をインストールします。</span><span class="sxs-lookup"><span data-stu-id="7543b-128">Install Web Deploy 2.1 or later.</span></span>
- <span data-ttu-id="7543b-129">リモートの接続を許可する Web 管理サービスを構成し、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="7543b-129">Configure the Web Management Service to allow remote connections, and start the service.</span></span>
- <span data-ttu-id="7543b-130">展開されたコンテンツをホストする IIS の web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="7543b-130">Create an IIS website to host the deployed content.</span></span>
- <span data-ttu-id="7543b-131">IIS マネージャーで web サイトに管理者以外のユーザーのアクセス許可を付与します。</span><span class="sxs-lookup"><span data-stu-id="7543b-131">Grant your non-administrator user permissions on your website in IIS Manager.</span></span>
- <span data-ttu-id="7543b-132">委任規則を追加し、管理者以外のユーザー アカウントを使用して web サイトのコンテンツの変更をサービスに許可する Web 管理サービスを確認します。</span><span class="sxs-lookup"><span data-stu-id="7543b-132">Ensure that the Web Management Service delegation rules permit the service to add and change website content using your non-administrator user account.</span></span>
- <span data-ttu-id="7543b-133">ポート 8172 で着信接続を許可するファイアウォールを構成します。</span><span class="sxs-lookup"><span data-stu-id="7543b-133">Configure any firewalls to allow incoming connections on port 8172.</span></span>

<span data-ttu-id="7543b-134">ContactManager サンプル ソリューションを具体的にはホストにもする必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-134">To host the ContactManager sample solution specifically, you'll also need to:</span></span>

- <span data-ttu-id="7543b-135">.NET Framework 4.0 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="7543b-135">Install the .NET Framework 4.0.</span></span>
- <span data-ttu-id="7543b-136">ASP.NET MVC 3 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="7543b-136">Install ASP.NET MVC 3.</span></span>

<span data-ttu-id="7543b-137">このトピックでは、これらの手順を実行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="7543b-137">This topic will show you how to perform each of these procedures.</span></span> <span data-ttu-id="7543b-138">タスクとチュートリアルでは、このトピックでは、Windows Server 2008 R2 を実行しているサーバーのクリーン ビルドを開始することを想定しています。</span><span class="sxs-lookup"><span data-stu-id="7543b-138">The tasks and walkthroughs in this topic assume that you're starting with a clean server build running Windows Server 2008 R2.</span></span> <span data-ttu-id="7543b-139">続行する前にいることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7543b-139">Before you continue, ensure that:</span></span>

- <span data-ttu-id="7543b-140">Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="7543b-140">Windows Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>
- <span data-ttu-id="7543b-141">サーバーは、ドメインに参加しています。</span><span class="sxs-lookup"><span data-stu-id="7543b-141">The server is domain-joined.</span></span>
- <span data-ttu-id="7543b-142">サーバーは、静的 IP アドレスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="7543b-142">The server has a static IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="7543b-143">コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューターのドメインとログオン](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7543b-143">For more information on joining computers to a domain, see [Joining Computers to the Domain and Logging On](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx).</span></span> <span data-ttu-id="7543b-144">静的 IP アドレスの構成の詳細については、次を参照してください。[静的 IP アドレス構成](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7543b-144">For more information on configuring static IP addresses, see [Configure a Static IP Address](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).</span></span>


## <a name="install-products-and-components"></a><span data-ttu-id="7543b-145">製品とコンポーネントをインストールします。</span><span class="sxs-lookup"><span data-stu-id="7543b-145">Install Products and Components</span></span>

<span data-ttu-id="7543b-146">このセクションで、web サーバーで必要な製品とコンポーネントのインストールを説明します。</span><span class="sxs-lookup"><span data-stu-id="7543b-146">This section will guide you through installing the required products and components on the web server.</span></span> <span data-ttu-id="7543b-147">開始する前に、サーバーが完全に最新であることを確認する Windows 更新プログラムを実行することをお勧めは。</span><span class="sxs-lookup"><span data-stu-id="7543b-147">Before you begin, a good practice is to run Windows Update to ensure that your server is fully up to date.</span></span>

<span data-ttu-id="7543b-148">この場合、これらをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-148">In this case, you need to install these things:</span></span>

- <span data-ttu-id="7543b-149">**IIS 7 推奨構成**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-149">**IIS 7 Recommended Configuration**.</span></span> <span data-ttu-id="7543b-150">これにより、 **Web サーバー (IIS)** web サーバー上のロールと IIS モジュールと ASP.NET アプリケーションをホストするために必要なコンポーネントのセットをインストールします。</span><span class="sxs-lookup"><span data-stu-id="7543b-150">This enables the **Web Server (IIS)** role on your web server and installs the set of IIS modules and components that you need in order to host an ASP.NET application.</span></span>
- <span data-ttu-id="7543b-151">**IIS: 管理サービス**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-151">**IIS: Management Service**.</span></span> <span data-ttu-id="7543b-152">これにより、IIS で Web 管理サービス (WMSvc) がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="7543b-152">This installs the Web Management Service (WMSvc) in IIS.</span></span> <span data-ttu-id="7543b-153">このサービスは、IIS web サイトのリモート管理を有効にし、クライアントに Web 配置ハンドラーのエンドポイントを公開します。</span><span class="sxs-lookup"><span data-stu-id="7543b-153">This service enables remote management of IIS websites and exposes the Web Deploy Handler endpoint to clients.</span></span>
- <span data-ttu-id="7543b-154">**IIS: 基本認証**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-154">**IIS: Basic Authentication**.</span></span> <span data-ttu-id="7543b-155">これは、IIS 基本認証モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="7543b-155">This installs the IIS Basic Authentication module.</span></span> <span data-ttu-id="7543b-156">これにより、Web 管理サービス (WMSvc) は、指定した資格情報を認証します。</span><span class="sxs-lookup"><span data-stu-id="7543b-156">This lets the Web Management Service (WMSvc) authenticate the credentials you provide.</span></span>
- <span data-ttu-id="7543b-157">**Web 配置ツール 2.1 以降**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-157">**Web Deployment Tool 2.1 or later**.</span></span> <span data-ttu-id="7543b-158">これにより、Web Deploy (とその基になる実行可能ファイル、MSDeploy.exe) がサーバーにインストールされます。</span><span class="sxs-lookup"><span data-stu-id="7543b-158">This installs Web Deploy (and its underlying executable, MSDeploy.exe) on your server.</span></span> <span data-ttu-id="7543b-159">このプロセスの一環としては、Web 配置ハンドラーをインストールし、Web 管理サービスを統合します。</span><span class="sxs-lookup"><span data-stu-id="7543b-159">As part of this process, it installs the Web Deploy Handler and integrates it with the Web Management Service.</span></span>
- <span data-ttu-id="7543b-160">**.NET framework 4.0**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-160">**.NET Framework 4.0**.</span></span> <span data-ttu-id="7543b-161">これは、このバージョンの .NET Framework で構築されたアプリケーションの実行に必要です。</span><span class="sxs-lookup"><span data-stu-id="7543b-161">This is required to run applications that were built on this version of the .NET Framework.</span></span>
- <span data-ttu-id="7543b-162">**ASP.NET MVC 3**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-162">**ASP.NET MVC 3**.</span></span> <span data-ttu-id="7543b-163">MVC 3 アプリケーションを実行する必要があるアセンブリがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="7543b-163">This installs the assemblies you need to run MVC 3 applications.</span></span>

> [!NOTE]
> <span data-ttu-id="7543b-164">このチュートリアルでは、さまざまなコンポーネントをインストールして構成、Web Platform Installer の使用について説明します。</span><span class="sxs-lookup"><span data-stu-id="7543b-164">This walkthrough describes the use of the Web Platform Installer to install and configure various components.</span></span> <span data-ttu-id="7543b-165">Web Platform Installer を使用する必要はありません、自動的に依存関係を検出して、常に製品の最新バージョンを取得することを確認して、インストール プロセスが簡略化します。</span><span class="sxs-lookup"><span data-stu-id="7543b-165">Although you don't have to use the Web Platform Installer, it simplifies the installation process by automatically detecting dependencies and ensuring that you always get the latest product versions.</span></span> <span data-ttu-id="7543b-166">詳細については、次を参照してください。 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)します。</span><span class="sxs-lookup"><span data-stu-id="7543b-166">For more information, see [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).</span></span>


<span data-ttu-id="7543b-167">**必要な製品とコンポーネントをインストールするには**</span><span class="sxs-lookup"><span data-stu-id="7543b-167">**To install the required products and components**</span></span>

1. <span data-ttu-id="7543b-168">ダウンロードしてインストール、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)します。</span><span class="sxs-lookup"><span data-stu-id="7543b-168">Download and install the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>
2. <span data-ttu-id="7543b-169">インストールが完了したら、Web Platform Installer は自動的に起動されます。</span><span class="sxs-lookup"><span data-stu-id="7543b-169">When installation is complete, the Web Platform Installer will launch automatically.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7543b-170">いつでも、Web Platform Installer を起動できるようになりました、**開始**メニュー。</span><span class="sxs-lookup"><span data-stu-id="7543b-170">You can now launch the Web Platform Installer at any time from the **Start** menu.</span></span> <span data-ttu-id="7543b-171">これを実行する、**開始** メニューのをクリックして**すべてのプログラム**、順にクリックします**Microsoft Web Platform Installer**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-171">To do this, on the **Start** menu, click **All Programs**, and then click **Microsoft Web Platform Installer**.</span></span>
3. <span data-ttu-id="7543b-172">上部にある、 **Web Platform Installer 3.0**ウィンドウで、をクリックして**製品**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-172">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
4. <span data-ttu-id="7543b-173">ナビゲーション ウィンドウで、ウィンドウの左側にある クリックして**フレームワーク**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-173">On the left side of the window, in the navigation pane, click **Frameworks**.</span></span>
5. <span data-ttu-id="7543b-174">**Microsoft .NET Framework 4** 、.NET Framework が既にインストールされていない場合、行をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-174">In the **Microsoft .NET Framework 4** row, if the .NET Framework is not already installed, click **Add**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7543b-175">既にインストールしている Windows Update を通じて、.NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="7543b-175">You may have already installed the .NET Framework 4.0 through Windows Update.</span></span> <span data-ttu-id="7543b-176">製品またはコンポーネントが既にインストールされている場合、Web Platform Installer 示されます置き換えることで、**追加**ボタン テキストを含む**インストール済み**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-176">If a product or component is already installed, the Web Platform Installer will indicate this by replacing the **Add** button with the text **Installed**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. <span data-ttu-id="7543b-177">**ASP.NET MVC 3 (Visual Studio 2010)** 行で、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-177">In the **ASP.NET MVC 3 (Visual Studio 2010)** row, click **Add**.</span></span>
7. <span data-ttu-id="7543b-178">ナビゲーション ウィンドウで、 **Server**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-178">In the navigation pane, click **Server**.</span></span>
8. <span data-ttu-id="7543b-179">**IIS 7 推奨構成**行で、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-179">In the **IIS 7 Recommended Configuration** row, click **Add**.</span></span>
9. <span data-ttu-id="7543b-180">**Web 配置ツール 2.1**行で、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-180">In the **Web Deployment Tool 2.1** row, click **Add**.</span></span>
10. <span data-ttu-id="7543b-181">**IIS: 基本認証**行で、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-181">In the **IIS: Basic Authentication** row, click **Add**.</span></span>
11. <span data-ttu-id="7543b-182">**IIS: 管理サービス**行で、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-182">In the **IIS: Management Service** row, click **Add**.</span></span>
12. <span data-ttu-id="7543b-183">**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7543b-183">Click **Install**.</span></span> <span data-ttu-id="7543b-184">Web Platform Installer が製品の一覧を表示&#x2014;と関連する依存関係&#x2014;をインストールし、ライセンス条項に同意するように求められます。</span><span class="sxs-lookup"><span data-stu-id="7543b-184">The Web Platform Installer will show you a list of products&#x2014;together with any associated dependencies&#x2014;to be installed and will prompt you to accept the license terms.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. <span data-ttu-id="7543b-185">ライセンス条項を確認し、条項に同意する場合にクリックします**同意**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-185">Review the license terms, and if you consent to the terms, click **I Accept**.</span></span>
14. <span data-ttu-id="7543b-186">インストールが完了したら、クリックして**完了**、閉じて、 **Web Platform Installer 3.0**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="7543b-186">When the installation is complete, click **Finish**, and then close the **Web Platform Installer 3.0** window.</span></span>

<span data-ttu-id="7543b-187">IIS をインストールする前に、.NET Framework 4.0 をインストールした場合を実行する必要があります、 [ASP.NET IIS 登録ツール](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) を IIS に ASP.NET の最新バージョンを登録します。</span><span class="sxs-lookup"><span data-stu-id="7543b-187">If you installed the .NET Framework 4.0 before you installed IIS, you'll need to run the [ASP.NET IIS Registration Tool](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) to register the latest version of ASP.NET with IIS.</span></span> <span data-ttu-id="7543b-188">これを行わないと、見つかります IIS が静的なコンテンツを提供 (HTML ファイル) など、問題なくが返されます**HTTP エラー 404.0 – 見つかりません**しようとすると、ASP.NET のコンテンツを参照します。</span><span class="sxs-lookup"><span data-stu-id="7543b-188">If you don't do this, you'll find that IIS will serve static content (like HTML files) without any problems, but it will return **HTTP Error 404.0 – Not Found** when you attempt to browse to ASP.NET content.</span></span> <span data-ttu-id="7543b-189">次の手順を使用すると、ASP.NET 4.0 が登録されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7543b-189">You can use the next procedure to ensure that ASP.NET 4.0 is registered.</span></span>

<span data-ttu-id="7543b-190">**ASP.NET 4.0 を IIS に登録するには**</span><span class="sxs-lookup"><span data-stu-id="7543b-190">**To register ASP.NET 4.0 with IIS**</span></span>

1. <span data-ttu-id="7543b-191">クリックして**開始**、し、入力**コマンド プロンプト**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-191">Click **Start**, and then type **Command Prompt**.</span></span>
2. <span data-ttu-id="7543b-192">右クリックし、検索結果で**コマンド プロンプト**、 をクリックし、**管理者として実行**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-192">In the search results, right-click **Command Prompt**, and then click **Run as administrator**.</span></span>
3. <span data-ttu-id="7543b-193">コマンド プロンプト ウィンドウに移動、 **%WINDIR%\Microsoft.NET\Framework\v4.0.30319**ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="7543b-193">In the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.</span></span>
4. <span data-ttu-id="7543b-194">このコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="7543b-194">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. <span data-ttu-id="7543b-195">任意の時点の 64 ビット web アプリケーションをホストする場合は、IIS と ASP.NET の 64 ビット バージョンを登録することも必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-195">If you plan to host 64-bit web applications at any point, you should also register the 64-bit version of ASP.NET with IIS.</span></span> <span data-ttu-id="7543b-196">コマンド プロンプト ウィンドウに移動します。、 **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="7543b-196">To do this, in the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.</span></span>
6. <span data-ttu-id="7543b-197">このコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="7543b-197">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

<span data-ttu-id="7543b-198">お勧めとして Windows Update を使用してもう一度この時点でダウンロードして、新製品とコンポーネントをインストールしたら、使用可能な更新プログラムをインストールします。</span><span class="sxs-lookup"><span data-stu-id="7543b-198">As a good practice, use Windows Update again at this point to download and install any available updates for the new products and components you've installed.</span></span>

## <a name="configure-the-web-management-service"></a><span data-ttu-id="7543b-199">Web 管理サービスを構成します。</span><span class="sxs-lookup"><span data-stu-id="7543b-199">Configure the Web Management Service</span></span>

<span data-ttu-id="7543b-200">これで必要なものすべてをインストールしたら、次の手順は、IIS で Web 管理サービスを構成するは。</span><span class="sxs-lookup"><span data-stu-id="7543b-200">Now that you've installed everything you need, the next step is to configure the Web Management Service in IIS.</span></span> <span data-ttu-id="7543b-201">大まかに言えば、これらのタスクを完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-201">At a high level, you'll need to complete these tasks:</span></span>

- <span data-ttu-id="7543b-202">サーバー レベルで基本認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="7543b-202">Enable basic authentication at the server level.</span></span>
- <span data-ttu-id="7543b-203">リモート接続を受け入れる Web 管理サービスを構成します。</span><span class="sxs-lookup"><span data-stu-id="7543b-203">Configure the Web Management Service to accept remote connections.</span></span>
- <span data-ttu-id="7543b-204">Web 管理サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="7543b-204">Start the Web Management Service.</span></span>
- <span data-ttu-id="7543b-205">必要な Web 管理サービス委任規則の確立されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7543b-205">Check that the required Web Management Service delegation rules are in place.</span></span>

<span data-ttu-id="7543b-206">**Web 管理サービスを構成するには**</span><span class="sxs-lookup"><span data-stu-id="7543b-206">**To configure the Web Management Service**</span></span>

1. <span data-ttu-id="7543b-207">**開始**メニューで、**管理ツール**、 をクリックし、**インターネット インフォメーション サービス (IIS) マネージャー**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-207">On the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
2. <span data-ttu-id="7543b-208">IIS マネージャーで、**接続**ウィンドウで、サーバー ノードをクリックします (たとえば、 **STAGEWEB1**)。</span><span class="sxs-lookup"><span data-stu-id="7543b-208">In IIS Manager, in the **Connections** pane, click the server node (for example, **STAGEWEB1**).</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. <span data-ttu-id="7543b-209">中央のウィンドウで  **IIS**、ダブルクリックして**認証**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-209">In the center pane, under **IIS**, double-click **Authentication**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image4.png)
4. <span data-ttu-id="7543b-210">右クリック**基本認証**、 をクリックし、**を有効にする**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-210">Right-click **Basic Authentication**, and then click **Enable**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. <span data-ttu-id="7543b-211">**接続**ウィンドウで、最上位レベルの設定に戻るにもう一度サーバー ノードをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7543b-211">In the **Connections** pane, click the server node again to return to the top-level settings.</span></span>
6. <span data-ttu-id="7543b-212">中央のウィンドウで **管理**、ダブルクリックして**管理サービス**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-212">In the center pane, under **Management**, double-click **Management Service**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. <span data-ttu-id="7543b-213">中央のウィンドウで次のように選択します。**リモート接続を有効にする**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-213">In the center pane, select **Enable remote connections**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7543b-214">Web 管理サービスが既に実行されている場合は、まずこれを停止する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-214">If the Web Management Service is already running, you'll need to stop it first.</span></span>
8. <span data-ttu-id="7543b-215">**アクション**ウィンドウで、をクリックして**開始**Web 管理サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="7543b-215">In the **Actions** pane, click **Start** to start the Web Management Service.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. <span data-ttu-id="7543b-216">設定を保存するメッセージが表示されたら、クリックして**はい**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-216">If you're prompted to save your settings, click **Yes**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7543b-217">自動的に開始するサービスを構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="7543b-217">You may also want to configure the service to start automatically.</span></span> <span data-ttu-id="7543b-218">これを行うには、サービス コンソールを開きを右クリックして**Web 管理サービス**、 をクリックし、**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-218">To do this, open the Services console, right-click **Web Management Service**, and then click **Properties**.</span></span> <span data-ttu-id="7543b-219">**スタートアップの種類**ドロップダウン リストで、**自動**、 をクリックし、 **OK**。</span><span class="sxs-lookup"><span data-stu-id="7543b-219">In the **Startup type** dropdown list, select **Automatic**, and then click **OK**.</span></span>
10. <span data-ttu-id="7543b-220">**接続**ウィンドウで、最上位レベルの設定に戻るにもう一度サーバー ノードをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7543b-220">In the **Connections** pane, click the server node again to return to the top-level settings.</span></span>
11. <span data-ttu-id="7543b-221">中央のウィンドウで **管理**、ダブルクリックして**管理サービスの委任**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-221">In the center pane, under **Management**, double-click **Management Service Delegation**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. <span data-ttu-id="7543b-222">中央のウィンドウにはで、一連ルールにはが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7543b-222">Verify that the center pane contains a set of rules.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    <span data-ttu-id="7543b-223">これらの規則は、さまざまな Web 配置プロバイダーを使用する承認された Web 管理サービスのユーザーを許可します。</span><span class="sxs-lookup"><span data-stu-id="7543b-223">These rules allow authorized Web Management Service users to use various Web Deploy providers.</span></span> <span data-ttu-id="7543b-224">たとえば、web アプリケーションとコンテンツを Web 配置ハンドラーを使用して IIS を展開する必要がありますすべてを使用する Web 管理サービスのユーザーを認証できるようにする委任規則、 **contentPath**と**iisApp**プロバイダー (スクリーン ショットに表示される最後のルール)。</span><span class="sxs-lookup"><span data-stu-id="7543b-224">For example, to deploy web applications and content to IIS through the Web Deploy Handler, there must be a delegation rule that allows all authenticated Web Management Service users to use the **contentPath** and **iisApp** providers (the last rule that you can see in the screenshot).</span></span>

    <span data-ttu-id="7543b-225">このトピックで説明されている順序でプロジェクトとコンポーネントをインストールした場合最新バージョンの Web 配置 Web 管理サービスをすべての必要な委任規則を追加する必要がありますは自動的にします。</span><span class="sxs-lookup"><span data-stu-id="7543b-225">If you installed products and components in the order described in this topic, the latest version of Web Deploy should automatically add all the required delegation rules to the Web Management Service.</span></span> <span data-ttu-id="7543b-226">管理サービスの委任 ページで、規則が表示されない場合は、自分で作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-226">If the Management Service Delegation page does not show any rules, you'll need to create them yourself.</span></span> <span data-ttu-id="7543b-227">これを行う方法の詳細については、次を参照してください。 [Web 配置ハンドラーを構成する](https://go.microsoft.com/?linkid=9805124)します。</span><span class="sxs-lookup"><span data-stu-id="7543b-227">For instructions on how to do this, see [Configure the Web Deployment Handler](https://go.microsoft.com/?linkid=9805124).</span></span>
13. <span data-ttu-id="7543b-228">**接続**ウィンドウで、最上位レベルの設定に戻るにもう一度サーバー ノードをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7543b-228">In the **Connections** pane, click the server node again to return to the top-level settings.</span></span>

## <a name="create-and-configure-an-iis-website"></a><span data-ttu-id="7543b-229">作成し、IIS の web サイトを構成します。</span><span class="sxs-lookup"><span data-stu-id="7543b-229">Create and Configure an IIS Website</span></span>

<span data-ttu-id="7543b-230">Web コンテンツを展開するには、サーバーに、前に作成してコンテンツをホストする IIS の web サイトを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-230">Before you can deploy web content to your server, you need to create and configure an IIS website to host the content.</span></span> <span data-ttu-id="7543b-231">Web Deploy; 既存の IIS web サイトに web パッケージを配置できるだけweb サイトを作成できません。</span><span class="sxs-lookup"><span data-stu-id="7543b-231">Web Deploy can only deploy web packages to an existing IIS website; it can't create the website for you.</span></span> <span data-ttu-id="7543b-232">また、コンテンツをリモートで展開を非管理者のアカウントを許可するほとんどの余分な構成を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-232">You also need to do a little extra configuration to allow your non-administrator account to deploy content remotely.</span></span> <span data-ttu-id="7543b-233">大まかに言えば、これらのタスクを完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-233">At a high level, you'll need to complete these tasks:</span></span>

- <span data-ttu-id="7543b-234">コンテンツをホストするファイル システム上のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="7543b-234">Create a folder on the file system to host your content.</span></span>
- <span data-ttu-id="7543b-235">コンテンツを処理するために IIS の web サイトを作成し、ローカル フォルダーに関連付けます。</span><span class="sxs-lookup"><span data-stu-id="7543b-235">Create an IIS website to serve the content, and associate it with the local folder.</span></span>
- <span data-ttu-id="7543b-236">読み取りローカル フォルダーにアプリケーション プール id へのアクセス許可を付与します。</span><span class="sxs-lookup"><span data-stu-id="7543b-236">Grant read permissions to the application pool identity on the local folder.</span></span>
- <span data-ttu-id="7543b-237">Web アプリケーションを展開するドメイン アカウントに必要な IIS アクセス許可を付与します。</span><span class="sxs-lookup"><span data-stu-id="7543b-237">Grant the necessary IIS permissions to the domain account that will deploy your web application.</span></span>

<span data-ttu-id="7543b-238">IIS の既定の web サイトにコンテンツを展開するなんらですが、この方法はテストまたはデモ シナリオ以外は推奨されません。</span><span class="sxs-lookup"><span data-stu-id="7543b-238">Although there's nothing stopping you from deploying content to the default website in IIS, this approach is not recommended for anything other than test or demonstration scenarios.</span></span> <span data-ttu-id="7543b-239">運用環境をシミュレートするには、アプリケーションの要件に固有の設定で新しい IIS web サイトを作成してください。</span><span class="sxs-lookup"><span data-stu-id="7543b-239">To simulate a production environment, you should create a new IIS website with settings that are specific to the requirements of your application.</span></span>

<span data-ttu-id="7543b-240">**IIS の web サイトを作成するには**</span><span class="sxs-lookup"><span data-stu-id="7543b-240">**To create an IIS website**</span></span>

1. <span data-ttu-id="7543b-241">ローカル ファイル システムでコンテンツを格納するフォルダーを作成します (たとえば、 **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="7543b-241">On the local file system, create a folder to store your content (for example, **C:\DemoSite**).</span></span>
2. <span data-ttu-id="7543b-242">**開始**メニューで、**管理ツール**、 をクリックし、**インターネット インフォメーション サービス (IIS) マネージャー**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-242">On the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
3. <span data-ttu-id="7543b-243">IIS マネージャーで、**接続**ウィンドウで、サーバー ノードを展開 (たとえば、 **STAGEWEB1**)。</span><span class="sxs-lookup"><span data-stu-id="7543b-243">In IIS Manager, in the **Connections** pane, expand the server node (for example, **STAGEWEB1**).</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. <span data-ttu-id="7543b-244">右クリックし、**サイト**ノード、およびクリック**Web サイトの追加**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-244">Right-click the **Sites** node, and then click **Add Web Site**.</span></span>
5. <span data-ttu-id="7543b-245">**サイト名**ボックスに、IIS web サイトの名前を入力 (たとえば、 **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="7543b-245">In the **Site name** box, type a name for the IIS website (for example, **DemoSite**).</span></span>
6. <span data-ttu-id="7543b-246">**物理パス**ボックス、入力 (またはを参照)、ローカル フォルダーへのパス (たとえば、 **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="7543b-246">In the **Physical path** box, type (or browse to) the path to your local folder (for example, **C:\DemoSite**).</span></span>
7. <span data-ttu-id="7543b-247">**ポート**ボックスに、web サイトをホストするポート番号を入力 (たとえば、 **85**)。</span><span class="sxs-lookup"><span data-stu-id="7543b-247">In the **Port** box, type the port number on which you want to host the website (for example, **85**).</span></span>

    > [!NOTE]
    > <span data-ttu-id="7543b-248">標準ポート番号は、http 80 および HTTPS の場合は 443 です。</span><span class="sxs-lookup"><span data-stu-id="7543b-248">The standard port numbers are 80 for HTTP and 443 for HTTPS.</span></span> <span data-ttu-id="7543b-249">ただし、ポート 80 では、この web サイトをホストしている場合は、サイトにアクセスするには、既定の web サイトを停止する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-249">However, if you host this website on port 80, you'll need to stop the default website before you can access your site.</span></span>
8. <span data-ttu-id="7543b-250">ままに、**ホスト名**ボックスをクリックして、web サイト用のドメイン ネーム システム (DNS) レコードを構成する場合、空白**OK**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-250">Leave the **Host name** box blank, unless you want to configure a Domain Name System (DNS) record for the website, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > <span data-ttu-id="7543b-251">運用環境では、ポート 80 で web サイトをホストし、DNS レコードを一致すると共に、ホスト ヘッダーを構成するします可能性があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-251">In a production environment, you'll likely want to host your website on port 80 and configure a host header, together with matching DNS records.</span></span> <span data-ttu-id="7543b-252">IIS 7 でホスト ヘッダーの構成の詳細については、次を参照してください。 [(IIS 7) の Web サイトのホスト ヘッダーを構成する](https://technet.microsoft.com/library/cc753195(WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7543b-252">For more information on configuring host headers in IIS 7, see [Configure a Host Header for a Web Site (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx).</span></span> <span data-ttu-id="7543b-253">Windows Server 2008 R2 の DNS サーバーの役割の詳細については、次を参照してください。 [DNS サーバーの概要](https://technet.microsoft.com/en-gb/library/cc770392.aspx)と[DNS Server](https://technet.microsoft.com/windowsserver/dd448607)します。</span><span class="sxs-lookup"><span data-stu-id="7543b-253">For more information on the DNS Server role in Windows Server 2008 R2, see [DNS Server Overview](https://technet.microsoft.com/en-gb/library/cc770392.aspx) and [DNS Server](https://technet.microsoft.com/windowsserver/dd448607).</span></span>
9. <span data-ttu-id="7543b-254">**[アクション]** ウィンドウの **[サイトの編集]** の下にある **[バインド]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7543b-254">In the **Actions** pane, under **Edit Site**, click **Bindings**.</span></span>
10. <span data-ttu-id="7543b-255">**サイト バインド**ダイアログ ボックスで、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-255">In the **Site Bindings** dialog box, click **Add**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. <span data-ttu-id="7543b-256">**サイト バインドの追加**ダイアログ ボックスで、セット、 **IP アドレス**と**ポート**既存のサイトの構成に一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="7543b-256">In the **Add Site Binding** dialog box, set the **IP address** and **Port** to match your existing site configuration.</span></span>
12. <span data-ttu-id="7543b-257">**ホスト名**ボックス、web サーバーの名前を入力 (たとえば、 **STAGEWEB1**)、順にクリックします**OK**。</span><span class="sxs-lookup"><span data-stu-id="7543b-257">In the **Host name** box, type the name of your web server (for example, **STAGEWEB1**), and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > <span data-ttu-id="7543b-258">最初のサイトのバインドでは、IP アドレスとポートを使用してローカル サイトにアクセスできます。 または`http://localhost:85`します。</span><span class="sxs-lookup"><span data-stu-id="7543b-258">The first site binding allows you to access the site locally using the IP address and port or `http://localhost:85`.</span></span> <span data-ttu-id="7543b-259">2 つ目のサイトのバインドでは、マシン名を使用して、ドメインの他のコンピューターからサイトにアクセスできます (たとえば、http://stageweb1:85)します。</span><span class="sxs-lookup"><span data-stu-id="7543b-259">The second site binding allows you to access the site from other computers on the domain using the machine name (for example, http://stageweb1:85).</span></span>
13. <span data-ttu-id="7543b-260">**サイト バインド**ダイアログ ボックスで、をクリックして**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-260">In the **Site Bindings** dialog box, click **Close**.</span></span>
14. <span data-ttu-id="7543b-261">**接続**ウィンドウで、をクリックして**アプリケーション プール**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-261">In the **Connections** pane, click **Application Pools**.</span></span>
15. <span data-ttu-id="7543b-262">**アプリケーション プール**ウィンドウは、アプリケーション プールの名前を右クリックし、クリックして**基本設定**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-262">In the **Application Pools** pane, right-click the name of your application pool, and then click **Basic Settings**.</span></span> <span data-ttu-id="7543b-263">既定では、アプリケーション プールの名前が、web サイトの名前に一致 (たとえば、 **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="7543b-263">By default, the name of your application pool will match the name of your website (for example, **DemoSite**).</span></span>
16. <span data-ttu-id="7543b-264">**.NET Framework のバージョン**一覧で、 **.NET Framework v4.0.30319**、順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-264">In the **.NET Framework version** list, select **.NET Framework v4.0.30319**, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image14.png)

    > [!NOTE]
    > <span data-ttu-id="7543b-265">サンプル ソリューションでは、.NET Framework 4.0 が必要です。</span><span class="sxs-lookup"><span data-stu-id="7543b-265">The sample solution requires .NET Framework 4.0.</span></span> <span data-ttu-id="7543b-266">これではなく Web デプロイのための要件です。</span><span class="sxs-lookup"><span data-stu-id="7543b-266">This is not a requirement for Web Deploy in general.</span></span>

<span data-ttu-id="7543b-267">Web サイト コンテンツを配信するためには、アプリケーション プール id をコンテンツを格納するローカル フォルダーのアクセス許可読み取りが必要です。</span><span class="sxs-lookup"><span data-stu-id="7543b-267">In order for your website to serve content, the application pool identity must have read permissions on the local folder that stores the content.</span></span> <span data-ttu-id="7543b-268">IIS 7.5 では、アプリケーション プールは、(アプリケーション プールが通常実行されるネットワーク サービス アカウントを使用して、IIS の以前のバージョン) とは異なり、既定で一意のアプリケーション プール id で実行します。</span><span class="sxs-lookup"><span data-stu-id="7543b-268">In IIS 7.5, application pools run with a unique application pool identity by default (in contrast to previous versions of IIS, where application pools would typically run using the Network Service account).</span></span> <span data-ttu-id="7543b-269">アプリケーション プール id の実際のユーザー アカウントではないとユーザーまたはグループのすべてのリストが表示されない&#x2014;代わりにときに、作成されます動的にアプリケーション プールを開始します。</span><span class="sxs-lookup"><span data-stu-id="7543b-269">The application pool identity is not a real user account and does not show up on any lists of users or groups&#x2014;instead, it's created dynamically when the application pool is started.</span></span> <span data-ttu-id="7543b-270">各アプリケーション プール id をローカルに追加**IIS\_IUSRS**セキュリティ グループを非表示の項目として。</span><span class="sxs-lookup"><span data-stu-id="7543b-270">Each application pool identity is added to the local **IIS\_IUSRS** security group as a hidden item.</span></span>

<span data-ttu-id="7543b-271">ファイルまたはフォルダーでのアプリケーション プール id へのアクセス許可を付与するには、2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="7543b-271">To grant permissions to an application pool identity on a file or folder, you have two options:</span></span>

- <span data-ttu-id="7543b-272">アクセス許可を割り当てる、アプリケーション プール id に直接、形式を使用して<strong>IIS AppPool\</strong ><em>[アプリケーション プール名]</em>(たとえば、 <strong>IIS AppPool\DemoSite</strong>).</span><span class="sxs-lookup"><span data-stu-id="7543b-272">Assign permissions to the application pool identity directly, using the format <strong>IIS AppPool\</strong><em>[application pool name]</em>(for example, <strong>IIS AppPool\DemoSite</strong>).</span></span>
- <span data-ttu-id="7543b-273">アクセス許可を割り当てる、 **IIS\_IUSRS**グループ。</span><span class="sxs-lookup"><span data-stu-id="7543b-273">Assign permissions to the **IIS\_IUSRS** group.</span></span>

<span data-ttu-id="7543b-274">最も一般的なアプローチは、ローカルに権限を割り当てる、 **IIS\_IUSRS**グループ、このアプローチでは、ファイル システム アクセス許可を再構成することがなくアプリケーション プールを変更できるためです。</span><span class="sxs-lookup"><span data-stu-id="7543b-274">The most common approach is to assign permissions to the local **IIS\_IUSRS** group, because this approach lets you change application pools without reconfiguring file system permissions.</span></span> <span data-ttu-id="7543b-275">次の手順では、このグループ ベースのアプローチを使用します。</span><span class="sxs-lookup"><span data-stu-id="7543b-275">The next procedure uses this group-based approach.</span></span>

> [!NOTE]
> <span data-ttu-id="7543b-276">IIS 7.5 でアプリケーション プール id の詳細については、次を参照してください。[アプリケーション プール Id](https://go.microsoft.com/?linkid=9805123)します。</span><span class="sxs-lookup"><span data-stu-id="7543b-276">For more information on application pool identities in IIS 7.5, see [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).</span></span>


<span data-ttu-id="7543b-277">**IIS の web サイトのフォルダーのアクセス許可を構成するには**</span><span class="sxs-lookup"><span data-stu-id="7543b-277">**To configure folder permissions for an IIS website**</span></span>

1. <span data-ttu-id="7543b-278">Windows エクスプ ローラーでは、ローカル フォルダーの場所を参照します。</span><span class="sxs-lookup"><span data-stu-id="7543b-278">In Windows Explorer, browse to the location of your local folder.</span></span>
2. <span data-ttu-id="7543b-279">フォルダーを右クリックし、**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-279">Right-click the folder, and then click **Properties**.</span></span>
3. <span data-ttu-id="7543b-280">**セキュリティ**] タブで [**編集**、順にクリックします**追加**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-280">On the **Security** tab, click **Edit**, and then click **Add**.</span></span>
4. <span data-ttu-id="7543b-281">クリックして**場所**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-281">Click **Locations**.</span></span> <span data-ttu-id="7543b-282">**場所** ダイアログ ボックスでは、ローカルのサーバーを選択し、順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-282">In the **Locations** dialog box, select the local server, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. <span data-ttu-id="7543b-283">**[ユーザーまたはグループ**ダイアログ ボックスに「 **IIS\_IUSRS**、] をクリックして**名前の確認**順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-283">In the **Select Users or Groups** dialog box, type **IIS\_IUSRS**, click **Check Names**, and then click **OK**.</span></span>
6. <span data-ttu-id="7543b-284"><strong>のアクセス許可</strong><em>[フォルダー名]</em>ダイアログ ボックスで、新しいグループが割り当てられている通知、<strong>読み取り&amp;実行</strong>、<strong>フォルダーの一覧内容</strong>、および<strong>読み取り</strong>既定のアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="7543b-284">In the <strong>Permissions for</strong><em>[folder name]</em> dialog box, notice that the new group has been assigned the <strong>Read &amp; execute</strong>, <strong>List folder contents</strong>, and <strong>Read</strong> permissions by default.</span></span> <span data-ttu-id="7543b-285">変更なしのままにし、クリックして<strong>OK</strong>します。</span><span class="sxs-lookup"><span data-stu-id="7543b-285">Leave this unchanged and click <strong>OK</strong>.</span></span>
7. <span data-ttu-id="7543b-286">をクリックして<strong>OK</strong>を閉じる、 <em>[フォルダー名]</em><strong>プロパティ</strong> ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="7543b-286">Click <strong>OK</strong> to close the <em>[folder name]</em><strong>Properties</strong> dialog box.</span></span>

<span data-ttu-id="7543b-287">最後のタスクとしては、管理者以外のコンテンツ展開に使用する資格情報を持つユーザーに適切なアクセス許可を与える必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-287">As a final task, you must grant the appropriate permissions to the non-administrator user whose credentials you'll use to deploy content.</span></span> <span data-ttu-id="7543b-288">このユーザーには、web サイトにコンテンツをリモートで展開するアクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="7543b-288">This user requires the permissions to deploy content remotely to your website.</span></span>

<span data-ttu-id="7543b-289">**管理者以外のドメイン ユーザーの IIS web サイトのアクセス許可を構成するには**</span><span class="sxs-lookup"><span data-stu-id="7543b-289">**To configure IIS website permissions for a non-administrator domain user**</span></span>

1. <span data-ttu-id="7543b-290">IIS マネージャーで、**接続**ウィンドウで、web サイト ノードを右クリックして (たとえば、 **DemoSite**)、 をポイント**デプロイ**、順にクリックします**Web の構成配置発行**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-290">In IIS Manager, in the **Connections** pane, right-click your website node (for example, **DemoSite**), point to **Deploy**, and then click **Configure Web Deploy Publishing**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. <span data-ttu-id="7543b-291">**Web 配置発行の構成**の右側に、ダイアログ ボックス、**公開アクセス許可を付与するユーザーを選択**一覧で、省略記号ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7543b-291">In the **Configure Web Deploy Publishing** dialog box, to the right of the **Select a user to give publishing permissions** list, click the ellipsis button.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. <span data-ttu-id="7543b-292">**を許可するユーザー**  ダイアログ ボックスをクリックして、コンテンツの展開を使用するアカウントのドメインとユーザーの名前を入力**OK**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-292">In the **Allow User** dialog box, type the domain and user name of the account you want to use to deploy content, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. <span data-ttu-id="7543b-293">**Web 配置発行の構成**ダイアログ ボックスで、をクリックして**セットアップ**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-293">In the **Configure Web Deploy Publishing** dialog box, click **Setup**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > <span data-ttu-id="7543b-294">この操作は、1 つの手順で 2 つの主要機能を実行します。</span><span class="sxs-lookup"><span data-stu-id="7543b-294">This operation performs two key functions in one step.</span></span> <span data-ttu-id="7543b-295">最初に、前のセクションでを調べ、委任規則に従って、Web 管理サービスを使用してリモートでの web サイトを変更するユーザーのアクセス許可を付与します。</span><span class="sxs-lookup"><span data-stu-id="7543b-295">First, it grants the user permission to modify the website remotely through the Web Management Service, according to the delegation rules you examined in the previous section.</span></span> <span data-ttu-id="7543b-296">次に、追加、変更、および web サイトのコンテンツにアクセス許可を設定するユーザーは、web サイトのソース フォルダーのユーザーのフル コントロールを付与します。</span><span class="sxs-lookup"><span data-stu-id="7543b-296">Second, it grants the user full control of the source folder for the website, which allows the user to add, modify, and set permissions on the website content.</span></span>
5. <span data-ttu-id="7543b-297">**Web 配置発行の構成**ダイアログ ボックスで、をクリックして**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="7543b-297">In the **Configure Web Deploy Publishing** dialog box, click **Close**.</span></span>

## <a name="configure-firewall-exceptions"></a><span data-ttu-id="7543b-298">ファイアウォールの例外を構成します。</span><span class="sxs-lookup"><span data-stu-id="7543b-298">Configure Firewall Exceptions</span></span>

<span data-ttu-id="7543b-299">既定では、IIS の Web 管理サービスは TCP ポート 8172 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="7543b-299">By default, the IIS Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="7543b-300">Web サーバーで Windows ファイアウォールが有効な場合は、新しい受信規則をポート 8172 で TCP トラフィックを許可する (すべての送信トラフィックは既定では Windows ファイアウォールの許可) を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-300">If Windows Firewall is enabled on your web server, you'll need to create a new inbound rule to allow TCP traffic on port 8172 (all outbound traffic is permitted by default in Windows Firewall).</span></span> <span data-ttu-id="7543b-301">サードパーティ製のファイアウォールを使用する場合は、トラフィックを許可するルールを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-301">If you use a third-party firewall, you'll need to create rules to allow traffic.</span></span>

| <span data-ttu-id="7543b-302">Direction</span><span class="sxs-lookup"><span data-stu-id="7543b-302">Direction</span></span> | <span data-ttu-id="7543b-303">ポートから</span><span class="sxs-lookup"><span data-stu-id="7543b-303">From Port</span></span> | <span data-ttu-id="7543b-304">ポートに</span><span class="sxs-lookup"><span data-stu-id="7543b-304">To Port</span></span> | <span data-ttu-id="7543b-305">ポートの種類</span><span class="sxs-lookup"><span data-stu-id="7543b-305">Port Type</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7543b-306">受信</span><span class="sxs-lookup"><span data-stu-id="7543b-306">Inbound</span></span> | <span data-ttu-id="7543b-307">どれでも可</span><span class="sxs-lookup"><span data-stu-id="7543b-307">Any</span></span> | <span data-ttu-id="7543b-308">8172</span><span class="sxs-lookup"><span data-stu-id="7543b-308">8172</span></span> | <span data-ttu-id="7543b-309">TCP</span><span class="sxs-lookup"><span data-stu-id="7543b-309">TCP</span></span> |
| <span data-ttu-id="7543b-310">送信</span><span class="sxs-lookup"><span data-stu-id="7543b-310">Outbound</span></span> | <span data-ttu-id="7543b-311">8172</span><span class="sxs-lookup"><span data-stu-id="7543b-311">8172</span></span> | <span data-ttu-id="7543b-312">どれでも可</span><span class="sxs-lookup"><span data-stu-id="7543b-312">Any</span></span> | <span data-ttu-id="7543b-313">TCP</span><span class="sxs-lookup"><span data-stu-id="7543b-313">TCP</span></span> |
  

<span data-ttu-id="7543b-314">Windows ファイアウォールのルールの構成の詳細については、次を参照してください。[ファイアウォール規則を構成する](https://technet.microsoft.com/library/dd448559(WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7543b-314">For more information on configuring rules in Windows Firewall, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="7543b-315">サードパーティ製のファイアウォール、製品のマニュアルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="7543b-315">For third-party firewalls, please consult your product documentation.</span></span>

## <a name="conclusion"></a><span data-ttu-id="7543b-316">まとめ</span><span class="sxs-lookup"><span data-stu-id="7543b-316">Conclusion</span></span>

<span data-ttu-id="7543b-317">Web サーバーは、Web 管理サービスを使用して、Web 配置ハンドラーへのリモート展開をそのまま使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="7543b-317">Your web server should now be ready to accept remote deployments to the Web Deploy Handler through the Web Management Service.</span></span> <span data-ttu-id="7543b-318">サーバーに web アプリケーションをデプロイする前に、次の点を確認したい場合があります。</span><span class="sxs-lookup"><span data-stu-id="7543b-318">Before you attempt to deploy a web application to the server, you may want to check these key points:</span></span>

- <span data-ttu-id="7543b-319">IIS でサーバー レベルで基本認証を有効にしますか。</span><span class="sxs-lookup"><span data-stu-id="7543b-319">Have you enabled basic authentication at the server level in IIS?</span></span>
- <span data-ttu-id="7543b-320">Web 管理サービスへのリモート接続を有効にしますか。</span><span class="sxs-lookup"><span data-stu-id="7543b-320">Have you enabled remote connections to the Web Management Service?</span></span>
- <span data-ttu-id="7543b-321">Web 管理サービスを開始するか。</span><span class="sxs-lookup"><span data-stu-id="7543b-321">Have you started the Web Management Service?</span></span>
- <span data-ttu-id="7543b-322">管理サービス委任規則場所か。</span><span class="sxs-lookup"><span data-stu-id="7543b-322">Are there management service delegation rules in place?</span></span>
- <span data-ttu-id="7543b-323">アプリケーション プール id は、web サイトのソース フォルダーに読み取りアクセスをあるでしょうか。</span><span class="sxs-lookup"><span data-stu-id="7543b-323">Does the application pool identity have read access to the source folder for your website?</span></span>
- <span data-ttu-id="7543b-324">管理者以外のユーザー アカウントはサイト レベルのアクセス許可を IIS でありますか。</span><span class="sxs-lookup"><span data-stu-id="7543b-324">Does the non-administrator user account have site-level permissions in IIS?</span></span>
- <span data-ttu-id="7543b-325">ファイアウォールは TCP ポート 8172 でサーバーへの着信接続を許可しますか。</span><span class="sxs-lookup"><span data-stu-id="7543b-325">Does your firewall allow incoming connections to the server on TCP port 8172?</span></span>

## <a name="further-reading"></a><span data-ttu-id="7543b-326">関連項目</span><span class="sxs-lookup"><span data-stu-id="7543b-326">Further Reading</span></span>

<span data-ttu-id="7543b-327">Web 配置ハンドラーに web パッケージを配置するカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを構成する方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](configuring-deployment-properties-for-a-target-environment.md)します。</span><span class="sxs-lookup"><span data-stu-id="7543b-327">For guidance on how to configure custom Microsoft Build Engine (MSBuild) project files to deploy web packages to the Web Deploy Handler, see [Configuring Deployment Properties for a Target Environment](configuring-deployment-properties-for-a-target-environment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7543b-328">[前へ](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
> [次へ](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="7543b-328">[Previous](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
[Next](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)</span></span>
