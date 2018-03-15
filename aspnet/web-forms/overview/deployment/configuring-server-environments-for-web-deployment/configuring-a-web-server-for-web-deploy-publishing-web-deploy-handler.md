---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: "デプロイの発行設定 Web 用 Web サーバーの構成 (Web Deploy ハンドラー) |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、web の発行および IIS Web 配置ハングルを使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: 81848c683fb9ddaa8942f030a520847a3c89fde0
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a><span data-ttu-id="d2f1a-103">デプロイの発行設定 Web 用 Web サーバーの構成 (Web Deploy ハンドラー)</span><span class="sxs-lookup"><span data-stu-id="d2f1a-103">Configuring a Web Server for Web Deploy Publishing (Web Deploy Handler)</span></span>
====================
<span data-ttu-id="d2f1a-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="d2f1a-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="d2f1a-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="d2f1a-106">このトピックでは、web の発行と、IIS Web 配置ハンドラーを使用して展開をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-106">This topic describes how to configure an Internet Information Services (IIS) web server to support web publishing and deployment using the IIS Web Deploy Handler.</span></span>
> 
> <span data-ttu-id="d2f1a-107">Web Deploy 2.0 以降を使用するときは、3 つの主な方法がアプリケーションまたは web サーバー上にサイトを取得するを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-107">When you work with Web Deploy 2.0 or later, there are three main approaches you can use to get your applications or sites onto a web server.</span></span> <span data-ttu-id="d2f1a-108">次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-108">You can:</span></span>
> 
> - <span data-ttu-id="d2f1a-109">使用して、 *Web デプロイ エージェントのリモート サービス*です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-109">Use the *Web Deploy Remote Agent Service*.</span></span> <span data-ttu-id="d2f1a-110">このアプローチには、web サーバーの以下の構成が必要ですが、何もサーバーに配置するためにローカル サーバーの管理者の資格情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-110">This approach requires less configuration of the web server, but you need to provide the credentials of a local server administrator in order to deploy anything to the server.</span></span>
> - <span data-ttu-id="d2f1a-111">使用して、 *Web Deploy ハンドラー*です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-111">Use the *Web Deploy Handler*.</span></span> <span data-ttu-id="d2f1a-112">このアプローチはもっと複雑であり、web サーバーを設定する初期多くの労力が必要です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-112">This approach is a lot more complex and requires more initial effort to set up the web server.</span></span> <span data-ttu-id="d2f1a-113">ただし、このアプローチを使用する場合は、配置を実行する管理者以外のユーザーを許可するように IIS を構成できます。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-113">However, when you use this approach, you can configure IIS to allow non-administrator users to perform the deployment.</span></span> <span data-ttu-id="d2f1a-114">Web 展開ハンドラーは IIS 7 以降のバージョンにできるだけです。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-114">The Web Deploy Handler is only available in IIS version 7 or later.</span></span>
> - <span data-ttu-id="d2f1a-115">使用して*オフライン展開*です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-115">Use *offline deployment*.</span></span> <span data-ttu-id="d2f1a-116">このアプローチには、web サーバーの最低限の構成が必要ですが、サーバー管理者は、する必要があります手動でサーバーに web パッケージをコピーして IIS マネージャーからインポートします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-116">This approach requires the least configuration of the web server, but a server administrator must manually copy the web package onto the server and import it through IIS Manager.</span></span>
> 
> <span data-ttu-id="d2f1a-117">主な機能、利点、およびこれらのアプローチの欠点の詳細については、次を参照してください。 [Web 配置を右側の方法を選択する](choosing-the-right-approach-to-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-117">For more information on the key features, advantages, and disadvantages of these approaches, see [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md).</span></span>


<span data-ttu-id="d2f1a-118">特定の IIS web サイトにコンテンツを展開する管理者以外のユーザーを許可する場合ははい。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-118">Yes, if you want to allow non-administrator users to deploy content to specific IIS websites.</span></span> <span data-ttu-id="d2f1a-119">この方法は、これらの種類のシナリオでは望ましく、多くの場合。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-119">This approach is often desirable in these types of scenarios:</span></span>

- <span data-ttu-id="d2f1a-120">ステージング環境または運用環境、リモート展開をトリガーする人物またはサービス アカウントは、サーバー管理者の資格情報にアクセスする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-120">Staging or production environments, where the person or service account that triggers the remote deployment is unlikely to have access to the credentials of a server administrator.</span></span>
- <span data-ttu-id="d2f1a-121">ホスト環境は、リモート ユーザーが web サーバー (または他のユーザーの web サイトへのアクセス) のフル コントロールを与えることがなく、web サイトを更新できるようにします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-121">Hosted environments, where you want to give remote users the ability to update their websites without giving them full control of your web servers (or access to anyone else's websites).</span></span>

<span data-ttu-id="d2f1a-122">開発またはテストのシナリオまたは小規模な組織では、サーバー管理者の資格情報を使用して展開するコンテンツが多くの場合、小さい悪化します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-122">In development or test scenarios, or in smaller organizations, deploying content using server administrator credentials is often less contentious.</span></span> <span data-ttu-id="d2f1a-123">これらのシナリオで使用した展開をサポートするために web サーバーの構成、 [Web リモート エージェント サービスの展開](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)さらにわかりやすいアプローチを提供します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-123">In these scenarios, configuring your web servers to support deployment using the [Web Deploy Remote Agent Service](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) offers a more straightforward approach.</span></span>

## <a name="task-overview"></a><span data-ttu-id="d2f1a-124">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="d2f1a-124">Task Overview</span></span>

<span data-ttu-id="d2f1a-125">Web 展開ハンドラー アプローチを使用してリモート コンピューターから web パッケージを配置して受け入れ、web サーバーを構成するのには、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-125">To configure the web server to accept and deploy web packages from a remote computer using the Web Deploy Handler approach, you'll need to:</span></span>

- <span data-ttu-id="d2f1a-126">作成、またはドメイン ユーザー アカウント (「管理者以外のユーザー」) の展開を実行に使用する資格情報を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-126">Create, or choose, a domain user account (the "non-administrator user") whose credentials you'll use to perform deployments.</span></span>
- <span data-ttu-id="d2f1a-127">IIS 7.5、Web 管理サービスなど、基本的な認証モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-127">Install IIS 7.5, including the Web Management Service and the Basic Authentication module.</span></span>
- <span data-ttu-id="d2f1a-128">Web Deploy 2.1 以降をインストールします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-128">Install Web Deploy 2.1 or later.</span></span>
- <span data-ttu-id="d2f1a-129">リモート接続を許可する Web 管理サービスを構成し、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-129">Configure the Web Management Service to allow remote connections, and start the service.</span></span>
- <span data-ttu-id="d2f1a-130">展開されたコンテンツをホストする IIS の web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-130">Create an IIS website to host the deployed content.</span></span>
- <span data-ttu-id="d2f1a-131">IIS マネージャーで web サイトの管理者以外のユーザー アクセス許可を付与します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-131">Grant your non-administrator user permissions on your website in IIS Manager.</span></span>
- <span data-ttu-id="d2f1a-132">委任規則を追加し、管理者以外のユーザー アカウントを使用して web サイトのコンテンツの変更をサービスに許可する Web 管理サービスを確認してください。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-132">Ensure that the Web Management Service delegation rules permit the service to add and change website content using your non-administrator user account.</span></span>
- <span data-ttu-id="d2f1a-133">ポート 8172 で着信接続を許可するファイアウォールを構成します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-133">Configure any firewalls to allow incoming connections on port 8172.</span></span>

<span data-ttu-id="d2f1a-134">ContactManager サンプル ソリューションを具体的にはホストにもする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-134">To host the ContactManager sample solution specifically, you'll also need to:</span></span>

- <span data-ttu-id="d2f1a-135">.NET Framework 4.0 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-135">Install the .NET Framework 4.0.</span></span>
- <span data-ttu-id="d2f1a-136">ASP.NET MVC 3 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-136">Install ASP.NET MVC 3.</span></span>

<span data-ttu-id="d2f1a-137">このトピックでは、各手順を実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-137">This topic will show you how to perform each of these procedures.</span></span> <span data-ttu-id="d2f1a-138">タスクとここでチュートリアルは、Windows Server 2008 R2 を実行するクリーン サーバー ビルドを開始していることを想定しています。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-138">The tasks and walkthroughs in this topic assume that you're starting with a clean server build running Windows Server 2008 R2.</span></span> <span data-ttu-id="d2f1a-139">続行する前に以下のことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-139">Before you continue, ensure that:</span></span>

- <span data-ttu-id="d2f1a-140">Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-140">Windows Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>
- <span data-ttu-id="d2f1a-141">サーバーがドメインに参加します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-141">The server is domain-joined.</span></span>
- <span data-ttu-id="d2f1a-142">サーバーは、静的 IP アドレスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-142">The server has a static IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="d2f1a-143">コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューター、ドメインとログオン](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-143">For more information on joining computers to a domain, see [Joining Computers to the Domain and Logging On](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx).</span></span> <span data-ttu-id="d2f1a-144">静的 IP アドレスを構成する方法については、次を参照してください。[静的 IP アドレスを構成](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-144">For more information on configuring static IP addresses, see [Configure a Static IP Address](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).</span></span>


## <a name="install-products-and-components"></a><span data-ttu-id="d2f1a-145">製品とコンポーネントをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-145">Install Products and Components</span></span>

<span data-ttu-id="d2f1a-146">このセクションでは、web サーバーに必要な製品とコンポーネントをインストールを説明します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-146">This section will guide you through installing the required products and components on the web server.</span></span> <span data-ttu-id="d2f1a-147">開始する前に、お勧め、サーバーが最新であることを確認する Windows Update を実行します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-147">Before you begin, a good practice is to run Windows Update to ensure that your server is fully up to date.</span></span>

<span data-ttu-id="d2f1a-148">この場合、これらをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-148">In this case, you need to install these things:</span></span>

- <span data-ttu-id="d2f1a-149">**IIS 7 の推奨構成**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-149">**IIS 7 Recommended Configuration**.</span></span> <span data-ttu-id="d2f1a-150">これにより、 **Web サーバー (IIS)**ロール、web サーバー上の IIS モジュールおよび ASP.NET アプリケーションをホストするために必要なコンポーネントのセットをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-150">This enables the **Web Server (IIS)** role on your web server and installs the set of IIS modules and components that you need in order to host an ASP.NET application.</span></span>
- <span data-ttu-id="d2f1a-151">**IIS: 管理サービス**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-151">**IIS: Management Service**.</span></span> <span data-ttu-id="d2f1a-152">これにより、IIS で Web 管理サービス (WMSvc) がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-152">This installs the Web Management Service (WMSvc) in IIS.</span></span> <span data-ttu-id="d2f1a-153">このサービスは、IIS web サイトのリモート管理を有効にし、クライアントに Web 配置ハンドラー エンドポイントを公開します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-153">This service enables remote management of IIS websites and exposes the Web Deploy Handler endpoint to clients.</span></span>
- <span data-ttu-id="d2f1a-154">**IIS: 基本認証**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-154">**IIS: Basic Authentication**.</span></span> <span data-ttu-id="d2f1a-155">これには、基本認証の IIS モジュールがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-155">This installs the IIS Basic Authentication module.</span></span> <span data-ttu-id="d2f1a-156">これにより、Web 管理サービス (WMSvc) は、指定した資格情報を認証します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-156">This lets the Web Management Service (WMSvc) authenticate the credentials you provide.</span></span>
- <span data-ttu-id="d2f1a-157">**Web 配置ツール 2.1 以降**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-157">**Web Deployment Tool 2.1 or later**.</span></span> <span data-ttu-id="d2f1a-158">これにより、Web Deploy (とその基になる実行可能ファイル、MSDeploy.exe) がサーバーにインストールされます。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-158">This installs Web Deploy (and its underlying executable, MSDeploy.exe) on your server.</span></span> <span data-ttu-id="d2f1a-159">このプロセスの一環として、Web 配置のハンドラーをインストールし、Web 管理サービスと統合します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-159">As part of this process, it installs the Web Deploy Handler and integrates it with the Web Management Service.</span></span>
- <span data-ttu-id="d2f1a-160">**.NET Framework 4.0**.</span><span class="sxs-lookup"><span data-stu-id="d2f1a-160">**.NET Framework 4.0**.</span></span> <span data-ttu-id="d2f1a-161">これは、このバージョンの .NET Framework で構築されたアプリケーションの実行に必要です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-161">This is required to run applications that were built on this version of the .NET Framework.</span></span>
- <span data-ttu-id="d2f1a-162">**ASP.NET MVC 3**.</span><span class="sxs-lookup"><span data-stu-id="d2f1a-162">**ASP.NET MVC 3**.</span></span> <span data-ttu-id="d2f1a-163">これは、MVC 3 アプリケーションを実行する必要があるアセンブリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-163">This installs the assemblies you need to run MVC 3 applications.</span></span>

> [!NOTE]
> <span data-ttu-id="d2f1a-164">このチュートリアルでは、Web Platform Installer をインストールして構成するさまざまなコンポーネントの使用について説明します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-164">This walkthrough describes the use of the Web Platform Installer to install and configure various components.</span></span> <span data-ttu-id="d2f1a-165">Web Platform Installer を使用できますが、自動的に依存関係を検出して、常に製品の最新バージョンを取得することを確認して、インストール プロセスが簡略化します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-165">Although you don't have to use the Web Platform Installer, it simplifies the installation process by automatically detecting dependencies and ensuring that you always get the latest product versions.</span></span> <span data-ttu-id="d2f1a-166">詳細については、次を参照してください。 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-166">For more information, see [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).</span></span>


<span data-ttu-id="d2f1a-167">**必要な製品とコンポーネントをインストールするには**</span><span class="sxs-lookup"><span data-stu-id="d2f1a-167">**To install the required products and components**</span></span>

1. <span data-ttu-id="d2f1a-168">ダウンロードしてインストール、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-168">Download and install the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>
2. <span data-ttu-id="d2f1a-169">インストールが完了したら、Web Platform Installer が自動的に起動します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-169">When installation is complete, the Web Platform Installer will launch automatically.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d2f1a-170">いつでも、Web Platform Installer を起動できるようになりました、**開始**メニュー。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-170">You can now launch the Web Platform Installer at any time from the **Start** menu.</span></span> <span data-ttu-id="d2f1a-171">[、**開始**] メニューのをクリックして**すべてのプログラム**、順にクリック**Microsoft Web Platform Installer**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-171">To do this, on the **Start** menu, click **All Programs**, and then click **Microsoft Web Platform Installer**.</span></span>
3. <span data-ttu-id="d2f1a-172">上部にある、 **Web Platform Installer 3.0**ウィンドウで、をクリックして**製品**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-172">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
4. <span data-ttu-id="d2f1a-173">ナビゲーション ウィンドウで、ウィンドウの左側にあるをクリックして**フレームワーク**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-173">On the left side of the window, in the navigation pane, click **Frameworks**.</span></span>
5. <span data-ttu-id="d2f1a-174">**Microsoft .NET Framework 4** 、.NET Framework がインストールされていない場合、行をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-174">In the **Microsoft .NET Framework 4** row, if the .NET Framework is not already installed, click **Add**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d2f1a-175">既にインストールしている Windows Update から .NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-175">You may have already installed the .NET Framework 4.0 through Windows Update.</span></span> <span data-ttu-id="d2f1a-176">製品またはコンポーネントがインストール済みの場合、Web Platform Installer は、これに置き換えることで、**追加**ボタン テキストを**インストール**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-176">If a product or component is already installed, the Web Platform Installer will indicate this by replacing the **Add** button with the text **Installed**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. <span data-ttu-id="d2f1a-177">**ASP.NET MVC 3 (Visual Studio 2010)**行で、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-177">In the **ASP.NET MVC 3 (Visual Studio 2010)** row, click **Add**.</span></span>
7. <span data-ttu-id="d2f1a-178">ナビゲーション ウィンドウで **サーバー**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-178">In the navigation pane, click **Server**.</span></span>
8. <span data-ttu-id="d2f1a-179">**IIS 7 の推奨構成**行で、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-179">In the **IIS 7 Recommended Configuration** row, click **Add**.</span></span>
9. <span data-ttu-id="d2f1a-180">**Web 配置ツール 2.1**行で、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-180">In the **Web Deployment Tool 2.1** row, click **Add**.</span></span>
10. <span data-ttu-id="d2f1a-181">**IIS: 基本認証**行で、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-181">In the **IIS: Basic Authentication** row, click **Add**.</span></span>
11. <span data-ttu-id="d2f1a-182">**IIS: 管理サービス**行で、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-182">In the **IIS: Management Service** row, click **Add**.</span></span>
12. <span data-ttu-id="d2f1a-183">**[インストール]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-183">Click **Install**.</span></span> <span data-ttu-id="d2f1a-184">Web Platform Installer をインストールするには、関連する依存関係 & #x 2014; と共に; 製品 & #x 2014 の一覧が表示され、ライセンス条項に同意するように求められます。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-184">The Web Platform Installer will show you a list of products&#x2014;together with any associated dependencies&#x2014;to be installed and will prompt you to accept the license terms.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. <span data-ttu-id="d2f1a-185">ライセンス条項を確認し、条項に同意した場合にをクリックして**同意**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-185">Review the license terms, and if you consent to the terms, click **I Accept**.</span></span>
14. <span data-ttu-id="d2f1a-186">インストールが完了したらをクリックして**完了**、し、閉じます、 **Web Platform Installer 3.0**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-186">When the installation is complete, click **Finish**, and then close the **Web Platform Installer 3.0** window.</span></span>

<span data-ttu-id="d2f1a-187">IIS をインストールする前に、.NET Framework 4.0 をインストールした場合を実行する必要があります、 [ASP.NET IIS 登録ツール](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) を IIS と ASP.NET の最新バージョンを登録します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-187">If you installed the .NET Framework 4.0 before you installed IIS, you'll need to run the [ASP.NET IIS Registration Tool](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) to register the latest version of ASP.NET with IIS.</span></span> <span data-ttu-id="d2f1a-188">これを行わないと、ことがわかります (HTML ファイル) などの静的なコンテンツを IIS が提供する、問題なくが返されます**HTTP エラー 404.0-Not Found** ASP.NET のコンテンツを参照しようとします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-188">If you don't do this, you'll find that IIS will serve static content (like HTML files) without any problems, but it will return **HTTP Error 404.0 – Not Found** when you attempt to browse to ASP.NET content.</span></span> <span data-ttu-id="d2f1a-189">次の手順を使用すると、ASP.NET 4.0 が登録されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-189">You can use the next procedure to ensure that ASP.NET 4.0 is registered.</span></span>

<span data-ttu-id="d2f1a-190">**ASP.NET 4.0 を IIS に登録するには**</span><span class="sxs-lookup"><span data-stu-id="d2f1a-190">**To register ASP.NET 4.0 with IIS**</span></span>

1. <span data-ttu-id="d2f1a-191">をクリックして**開始**、し、入力**コマンド プロンプト**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-191">Click **Start**, and then type **Command Prompt**.</span></span>
2. <span data-ttu-id="d2f1a-192">検索結果を右クリックして**コマンド プロンプト**、クリックして**管理者として実行**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-192">In the search results, right-click **Command Prompt**, and then click **Run as administrator**.</span></span>
3. <span data-ttu-id="d2f1a-193">コマンド プロンプト ウィンドウに移動、 **%WINDIR%\Microsoft.NET\Framework\v4.0.30319**ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-193">In the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.</span></span>
4. <span data-ttu-id="d2f1a-194">このコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-194">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. <span data-ttu-id="d2f1a-195">任意の時点での 64 ビットの web アプリケーションをホストする場合は、IIS と 64 ビット バージョンの ASP.NET を登録することも必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-195">If you plan to host 64-bit web applications at any point, you should also register the 64-bit version of ASP.NET with IIS.</span></span> <span data-ttu-id="d2f1a-196">コマンド プロンプト ウィンドウで、これに移動、 **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-196">To do this, in the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.</span></span>
6. <span data-ttu-id="d2f1a-197">このコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-197">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

<span data-ttu-id="d2f1a-198">をお勧め Windows Update を使用してもう一度この時点でダウンロードして、新しい製品とコンポーネントがインストールされている使用可能な更新プログラムをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-198">As a good practice, use Windows Update again at this point to download and install any available updates for the new products and components you've installed.</span></span>

## <a name="configure-the-web-management-service"></a><span data-ttu-id="d2f1a-199">Web 管理サービスを構成します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-199">Configure the Web Management Service</span></span>

<span data-ttu-id="d2f1a-200">必要なものをインストールしたら、次の手順は、IIS で Web 管理サービスを構成するは。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-200">Now that you've installed everything you need, the next step is to configure the Web Management Service in IIS.</span></span> <span data-ttu-id="d2f1a-201">大まかに言えば、これらのタスクを完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-201">At a high level, you'll need to complete these tasks:</span></span>

- <span data-ttu-id="d2f1a-202">サーバー レベルで基本認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-202">Enable basic authentication at the server level.</span></span>
- <span data-ttu-id="d2f1a-203">リモート接続を許可する Web 管理サービスを構成します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-203">Configure the Web Management Service to accept remote connections.</span></span>
- <span data-ttu-id="d2f1a-204">Web 管理サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-204">Start the Web Management Service.</span></span>
- <span data-ttu-id="d2f1a-205">必要な Web 管理サービス委任規則が満たされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-205">Check that the required Web Management Service delegation rules are in place.</span></span>

<span data-ttu-id="d2f1a-206">**Web 管理サービスを構成するには**</span><span class="sxs-lookup"><span data-stu-id="d2f1a-206">**To configure the Web Management Service**</span></span>

1. <span data-ttu-id="d2f1a-207">**開始** メニューのをポイント**管理ツール**、クリックして**インターネット インフォメーション サービス (IIS) マネージャー**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-207">On the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
2. <span data-ttu-id="d2f1a-208">IIS マネージャーで、**接続** ウィンドウで、サーバー ノードをクリックして (たとえば、 **STAGEWEB1**)。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-208">In IIS Manager, in the **Connections** pane, click the server node (for example, **STAGEWEB1**).</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. <span data-ttu-id="d2f1a-209">中央のウィンドウで  **IIS**をダブルクリックして**認証**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-209">In the center pane, under **IIS**, double-click **Authentication**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image4.png)
4. <span data-ttu-id="d2f1a-210">右クリック**基本認証**、クリックして**を有効にする**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-210">Right-click **Basic Authentication**, and then click **Enable**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. <span data-ttu-id="d2f1a-211">**接続** ウィンドウで、最上位レベルの設定に戻るには、もう一度サーバー ノードをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-211">In the **Connections** pane, click the server node again to return to the top-level settings.</span></span>
6. <span data-ttu-id="d2f1a-212">中央のウィンドウで **管理**をダブルクリックして**管理サービス**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-212">In the center pane, under **Management**, double-click **Management Service**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. <span data-ttu-id="d2f1a-213">中央のウィンドウで次のように選択します。**リモート接続を有効にする**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-213">In the center pane, select **Enable remote connections**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d2f1a-214">Web 管理サービスが既に実行されている場合は、最初に停止する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-214">If the Web Management Service is already running, you'll need to stop it first.</span></span>
8. <span data-ttu-id="d2f1a-215">**アクション** ウィンドウで、をクリックして**開始**Web 管理サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-215">In the **Actions** pane, click **Start** to start the Web Management Service.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. <span data-ttu-id="d2f1a-216">設定を保存するメッセージが表示されたら、クリックして**はい**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-216">If you're prompted to save your settings, click **Yes**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d2f1a-217">自動的に開始するサービスを構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-217">You may also want to configure the service to start automatically.</span></span> <span data-ttu-id="d2f1a-218">これを行うには、サービス コンソールを開きを右クリックして**Web 管理サービス**、クリックして**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-218">To do this, open the Services console, right-click **Web Management Service**, and then click **Properties**.</span></span> <span data-ttu-id="d2f1a-219">**スタートアップの種類**ドロップダウン リストで、**自動**、順にクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-219">In the **Startup type** dropdown list, select **Automatic**, and then click **OK**.</span></span>
10. <span data-ttu-id="d2f1a-220">**接続** ウィンドウで、最上位レベルの設定に戻るには、もう一度サーバー ノードをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-220">In the **Connections** pane, click the server node again to return to the top-level settings.</span></span>
11. <span data-ttu-id="d2f1a-221">中央のウィンドウで **管理**をダブルクリックして**管理サービス委任**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-221">In the center pane, under **Management**, double-click **Management Service Delegation**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. <span data-ttu-id="d2f1a-222">中央のペインに、一連ルールにはが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-222">Verify that the center pane contains a set of rules.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    <span data-ttu-id="d2f1a-223">これらの規則は、さまざまな Web Deploy プロバイダーを使用する承認された Web 管理サービスのユーザーを許可します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-223">These rules allow authorized Web Management Service users to use various Web Deploy providers.</span></span> <span data-ttu-id="d2f1a-224">たとえば、web アプリケーションとコンテンツを Web 配置のハンドラーを使用して IIS を展開する必要がありますすべて使用する Web 管理サービスのユーザーを認証できるようにする委任規則、 **contentPath**と**iisApp**プロバイダー (、前回のルールのスクリーン ショットに表示される)。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-224">For example, to deploy web applications and content to IIS through the Web Deploy Handler, there must be a delegation rule that allows all authenticated Web Management Service users to use the **contentPath** and **iisApp** providers (the last rule that you can see in the screenshot).</span></span>

    <span data-ttu-id="d2f1a-225">このトピックで説明した順序での製品とコンポーネントをインストールした場合、最新バージョンの Web Deploy 必要があります自動的に追加すべての必要な委任規則 Web 管理サービス。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-225">If you installed products and components in the order described in this topic, the latest version of Web Deploy should automatically add all the required delegation rules to the Web Management Service.</span></span> <span data-ttu-id="d2f1a-226">管理サービスの委任のページで、規則が表示されない場合は、自分で作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-226">If the Management Service Delegation page does not show any rules, you'll need to create them yourself.</span></span> <span data-ttu-id="d2f1a-227">これを行う方法については、次を参照してください。 [Web の展開のハンドラーを構成する](https://go.microsoft.com/?linkid=9805124)です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-227">For instructions on how to do this, see [Configure the Web Deployment Handler](https://go.microsoft.com/?linkid=9805124).</span></span>
13. <span data-ttu-id="d2f1a-228">**接続** ウィンドウで、最上位レベルの設定に戻るには、もう一度サーバー ノードをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-228">In the **Connections** pane, click the server node again to return to the top-level settings.</span></span>

## <a name="create-and-configure-an-iis-website"></a><span data-ttu-id="d2f1a-229">作成し、IIS の web サイトを構成します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-229">Create and Configure an IIS Website</span></span>

<span data-ttu-id="d2f1a-230">Web コンテンツを展開するには、サーバーに、前に作成し、コンテンツをホストする IIS の web サイトを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-230">Before you can deploy web content to your server, you need to create and configure an IIS website to host the content.</span></span> <span data-ttu-id="d2f1a-231">Web Deploy にしかデプロイできません web パッケージ既存の IIS web サイトです。web サイトを作成できません。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-231">Web Deploy can only deploy web packages to an existing IIS website; it can't create the website for you.</span></span> <span data-ttu-id="d2f1a-232">また、非管理者アカウントは、コンテンツをリモートで展開を許可するほとんどの余分な構成を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-232">You also need to do a little extra configuration to allow your non-administrator account to deploy content remotely.</span></span> <span data-ttu-id="d2f1a-233">大まかに言えば、これらのタスクを完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-233">At a high level, you'll need to complete these tasks:</span></span>

- <span data-ttu-id="d2f1a-234">コンテンツをホストするファイル システムにフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-234">Create a folder on the file system to host your content.</span></span>
- <span data-ttu-id="d2f1a-235">コンテンツを提供する IIS の web サイトを作成し、ローカル フォルダーに関連付けます。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-235">Create an IIS website to serve the content, and associate it with the local folder.</span></span>
- <span data-ttu-id="d2f1a-236">読み取り、アプリケーション プール id に、ローカル フォルダーに対する権限を付与します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-236">Grant read permissions to the application pool identity on the local folder.</span></span>
- <span data-ttu-id="d2f1a-237">Web アプリケーションを展開するドメイン アカウントに必要な IIS のアクセス許可を付与します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-237">Grant the necessary IIS permissions to the domain account that will deploy your web application.</span></span>

<span data-ttu-id="d2f1a-238">阻止する IIS の既定の web サイトにコンテンツを展開するからですが、この方法はテストまたはデモのシナリオ以外の何かの推奨されません。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-238">Although there's nothing stopping you from deploying content to the default website in IIS, this approach is not recommended for anything other than test or demonstration scenarios.</span></span> <span data-ttu-id="d2f1a-239">実稼働環境をシミュレートするには、アプリケーションの要件に固有の設定で新しい IIS web サイトを作成してください。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-239">To simulate a production environment, you should create a new IIS website with settings that are specific to the requirements of your application.</span></span>

<span data-ttu-id="d2f1a-240">**IIS の web サイトを作成するには**</span><span class="sxs-lookup"><span data-stu-id="d2f1a-240">**To create an IIS website**</span></span>

1. <span data-ttu-id="d2f1a-241">ローカル ファイル システムにコンテンツを保存するフォルダーを作成します (たとえば、 **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-241">On the local file system, create a folder to store your content (for example, **C:\DemoSite**).</span></span>
2. <span data-ttu-id="d2f1a-242">**開始** メニューのをポイント**管理ツール**、クリックして**インターネット インフォメーション サービス (IIS) マネージャー**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-242">On the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
3. <span data-ttu-id="d2f1a-243">IIS マネージャーで、**接続** ウィンドウで、サーバー ノードを展開 (たとえば、 **STAGEWEB1**)。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-243">In IIS Manager, in the **Connections** pane, expand the server node (for example, **STAGEWEB1**).</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. <span data-ttu-id="d2f1a-244">右クリックし、**サイト**ノードをクリックして**Web サイトの追加**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-244">Right-click the **Sites** node, and then click **Add Web Site**.</span></span>
5. <span data-ttu-id="d2f1a-245">**サイト名**ボックスで、IIS の web サイトの名前を入力 (たとえば、 **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-245">In the **Site name** box, type a name for the IIS website (for example, **DemoSite**).</span></span>
6. <span data-ttu-id="d2f1a-246">**物理パス**ボックスに入力 (またはを参照)、ローカル フォルダーへのパス (たとえば、 **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-246">In the **Physical path** box, type (or browse to) the path to your local folder (for example, **C:\DemoSite**).</span></span>
7. <span data-ttu-id="d2f1a-247">**ポート**ボックスに、web サイトをホストするポート番号を入力 (たとえば、 **85**)。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-247">In the **Port** box, type the port number on which you want to host the website (for example, **85**).</span></span>

    > [!NOTE]
    > <span data-ttu-id="d2f1a-248">標準のポート番号は http が 80 および HTTPS の場合は 443 です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-248">The standard port numbers are 80 for HTTP and 443 for HTTPS.</span></span> <span data-ttu-id="d2f1a-249">ただし、ポート 80 では、この web サイトをホストしている場合は、サイトにアクセスするには、既定の web サイトを停止する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-249">However, if you host this website on port 80, you'll need to stop the default website before you can access your site.</span></span>
8. <span data-ttu-id="d2f1a-250">ままにして、**ホスト名**ボックスをクリックして、web サイトのドメイン ネーム システム (DNS) レコードを構成するのでない限り、空白**OK**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-250">Leave the **Host name** box blank, unless you want to configure a Domain Name System (DNS) record for the website, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > <span data-ttu-id="d2f1a-251">実稼働環境にすることができますをポート 80 で web サイトをホストし、DNS レコードを一致すると共に、ホスト ヘッダーを構成します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-251">In a production environment, you'll likely want to host your website on port 80 and configure a host header, together with matching DNS records.</span></span> <span data-ttu-id="d2f1a-252">IIS 7 でのホスト ヘッダーを構成する方法については、次を参照してください。 [(IIS 7) の Web サイトのホスト ヘッダーを構成する](https://technet.microsoft.com/library/cc753195(WS.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-252">For more information on configuring host headers in IIS 7, see [Configure a Host Header for a Web Site (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx).</span></span> <span data-ttu-id="d2f1a-253">Windows Server 2008 R2 の DNS サーバーの役割の詳細については、次を参照してください。 [DNS サーバーの概要](https://technet.microsoft.com/en-gb/library/cc770392.aspx)と[DNS Server](https://technet.microsoft.com/windowsserver/dd448607)です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-253">For more information on the DNS Server role in Windows Server 2008 R2, see [DNS Server Overview](https://technet.microsoft.com/en-gb/library/cc770392.aspx) and [DNS Server](https://technet.microsoft.com/windowsserver/dd448607).</span></span>
9. <span data-ttu-id="d2f1a-254">**[アクション]** ウィンドウの **[サイトの編集]** の下にある **[バインド]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-254">In the **Actions** pane, under **Edit Site**, click **Bindings**.</span></span>
10. <span data-ttu-id="d2f1a-255">**サイト バインド**ダイアログ ボックスで、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-255">In the **Site Bindings** dialog box, click **Add**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. <span data-ttu-id="d2f1a-256">**サイト バインドの追加**ダイアログ ボックスで、設定、 **IP アドレス**と**ポート**既存サイトの構成に一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-256">In the **Add Site Binding** dialog box, set the **IP address** and **Port** to match your existing site configuration.</span></span>
12. <span data-ttu-id="d2f1a-257">**ホスト名**ボックス、web サーバーの名前を入力 (たとえば、 **STAGEWEB1**)、をクリックし、 **[ok]**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-257">In the **Host name** box, type the name of your web server (for example, **STAGEWEB1**), and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > <span data-ttu-id="d2f1a-258">最初のサイトのバインドでは、IP アドレスとポートを使用してローカル サイトにアクセスできます。 または`http://localhost:85`です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-258">The first site binding allows you to access the site locally using the IP address and port or `http://localhost:85`.</span></span> <span data-ttu-id="d2f1a-259">2 つ目のサイト バインドでは、コンピューター名 (たとえば、http://stageweb1:85) を使用して、ドメインの他のコンピューターからサイトにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-259">The second site binding allows you to access the site from other computers on the domain using the machine name (for example, http://stageweb1:85).</span></span>
13. <span data-ttu-id="d2f1a-260">**サイト バインド**ダイアログ ボックスで、をクリックして**閉じる**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-260">In the **Site Bindings** dialog box, click **Close**.</span></span>
14. <span data-ttu-id="d2f1a-261">**接続** ウィンドウで、をクリックして**アプリケーション プール**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-261">In the **Connections** pane, click **Application Pools**.</span></span>
15. <span data-ttu-id="d2f1a-262">**アプリケーション プール** ウィンドウでは、アプリケーション プールの名前を右クリックし、をクリックして**基本設定**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-262">In the **Application Pools** pane, right-click the name of your application pool, and then click **Basic Settings**.</span></span> <span data-ttu-id="d2f1a-263">既定では、アプリケーション プールの名前が、web サイトの名前に一致 (たとえば、 **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-263">By default, the name of your application pool will match the name of your website (for example, **DemoSite**).</span></span>
16. <span data-ttu-id="d2f1a-264">**.NET Framework のバージョン**一覧で、 **.NET Framework v4.0.30319**、順にクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-264">In the **.NET Framework version** list, select **.NET Framework v4.0.30319**, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image14.png)

    > [!NOTE]
    > <span data-ttu-id="d2f1a-265">サンプル ソリューションには、.NET Framework 4.0 が必要です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-265">The sample solution requires .NET Framework 4.0.</span></span> <span data-ttu-id="d2f1a-266">これは、要件ではありません、Web Deploy の一般にします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-266">This is not a requirement for Web Deploy in general.</span></span>

<span data-ttu-id="d2f1a-267">Web サイト コンテンツを提供するためには、アプリケーション プール id 読み取り権限が必要、コンテンツを格納するローカル フォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-267">In order for your website to serve content, the application pool identity must have read permissions on the local folder that stores the content.</span></span> <span data-ttu-id="d2f1a-268">IIS 7.5、アプリケーション プールは、(ここでアプリケーション プールは通常の実行、Network Service アカウントを使用して、IIS の以前のバージョン) とは異なり、既定では、一意のアプリケーション プール id で実行します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-268">In IIS 7.5, application pools run with a unique application pool identity by default (in contrast to previous versions of IIS, where application pools would typically run using the Network Service account).</span></span> <span data-ttu-id="d2f1a-269">アプリケーション プール id が実際のユーザー アカウントではないと、ユーザーまたはグループ & #x 2014 のすべてのリストに表示されないです。 代わりに、それが動的に作成、アプリケーション プールが開始されたときにします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-269">The application pool identity is not a real user account and does not show up on any lists of users or groups&#x2014;instead, it's created dynamically when the application pool is started.</span></span> <span data-ttu-id="d2f1a-270">各アプリケーション プール id がローカルに追加**IIS\_IUSRS**セキュリティ グループを非表示のアイテムとして。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-270">Each application pool identity is added to the local **IIS\_IUSRS** security group as a hidden item.</span></span>

<span data-ttu-id="d2f1a-271">ファイルまたはフォルダーでのアプリケーション プール id へのアクセス許可を付与するには、2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-271">To grant permissions to an application pool identity on a file or folder, you have two options:</span></span>

- <span data-ttu-id="d2f1a-272">アクセス許可を割り当てるアプリケーション プール id に直接、形式を使用して**IIS AppPool\***[アプリケーション プール名] * (たとえば、 **IIS AppPool\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-272">Assign permissions to the application pool identity directly, using the format **IIS AppPool\***[application pool name]*(for example, **IIS AppPool\DemoSite**).</span></span>
- <span data-ttu-id="d2f1a-273">アクセス許可を割り当てる、 **IIS\_IUSRS**グループ。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-273">Assign permissions to the **IIS\_IUSRS** group.</span></span>

<span data-ttu-id="d2f1a-274">最も一般的な方法は、ローカルに権限を割り当てる、 **IIS\_IUSRS**このアプローチでは、ファイル システム アクセス許可を再構成することがなくアプリケーション プールを変更することができますので、グループ化します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-274">The most common approach is to assign permissions to the local **IIS\_IUSRS** group, because this approach lets you change application pools without reconfiguring file system permissions.</span></span> <span data-ttu-id="d2f1a-275">次の手順では、このグループ ベースのアプローチを使用します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-275">The next procedure uses this group-based approach.</span></span>

> [!NOTE]
> <span data-ttu-id="d2f1a-276">IIS 7.5 におけるアプリケーション プール id の詳細については、次を参照してください。[アプリケーション プール Id](https://go.microsoft.com/?linkid=9805123)です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-276">For more information on application pool identities in IIS 7.5, see [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).</span></span>


<span data-ttu-id="d2f1a-277">**IIS の web サイトのフォルダーのアクセス許可を構成するには**</span><span class="sxs-lookup"><span data-stu-id="d2f1a-277">**To configure folder permissions for an IIS website**</span></span>

1. <span data-ttu-id="d2f1a-278">Windows エクスプ ローラーで、ローカル フォルダーの場所を参照します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-278">In Windows Explorer, browse to the location of your local folder.</span></span>
2. <span data-ttu-id="d2f1a-279">フォルダーを右クリックし、をクリックして**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-279">Right-click the folder, and then click **Properties**.</span></span>
3. <span data-ttu-id="d2f1a-280">**セキュリティ** タブで、をクリックして**編集**、クリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-280">On the **Security** tab, click **Edit**, and then click **Add**.</span></span>
4. <span data-ttu-id="d2f1a-281">をクリックして**場所**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-281">Click **Locations**.</span></span> <span data-ttu-id="d2f1a-282">**場所**ダイアログ ボックスでは、ローカル サーバーを選択し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-282">In the **Locations** dialog box, select the local server, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. <span data-ttu-id="d2f1a-283">**ユーザーまたはグループ** ダイアログ ボックスで、「 **IIS\_IUSRS**、 をクリックして**名前の確認**順にクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-283">In the **Select Users or Groups** dialog box, type **IIS\_IUSRS**, click **Check Names**, and then click **OK**.</span></span>
6. <span data-ttu-id="d2f1a-284">**のアクセス許可 * * * [フォルダー名]*  ダイアログ ボックスで、新しいグループが割り当てられている、**読み取り&amp;実行**、**フォルダー内容の一覧**、および**読み取り**既定のアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-284">In the **Permissions for***[folder name]* dialog box, notice that the new group has been assigned the **Read &amp; execute**, **List folder contents**, and **Read** permissions by default.</span></span> <span data-ttu-id="d2f1a-285">これを変更せずのままにし、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-285">Leave this unchanged and click **OK**.</span></span>
7. <span data-ttu-id="d2f1a-286">をクリックして**OK**を閉じる、 *[フォルダー名] * * * プロパティ** ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-286">Click **OK** to close the *[folder name]***Properties** dialog box.</span></span>

<span data-ttu-id="d2f1a-287">最後のタスクとして、管理者以外の資格情報にユーザーのコンテンツを展開するときに使用する適切なアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-287">As a final task, you must grant the appropriate permissions to the non-administrator user whose credentials you'll use to deploy content.</span></span> <span data-ttu-id="d2f1a-288">このユーザーには、web サイトにコンテンツをリモートで展開する権限が必要です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-288">This user requires the permissions to deploy content remotely to your website.</span></span>

<span data-ttu-id="d2f1a-289">**管理者以外のドメイン ユーザーの IIS web サイトのアクセス許可を構成するには**</span><span class="sxs-lookup"><span data-stu-id="d2f1a-289">**To configure IIS website permissions for a non-administrator domain user**</span></span>

1. <span data-ttu-id="d2f1a-290">IIS マネージャーで、**接続** ウィンドウで、web サイト ノードを右クリックして (たとえば、 **DemoSite**)、 をポイント**展開**、クリックして**Web の構成デプロイの発行**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-290">In IIS Manager, in the **Connections** pane, right-click your website node (for example, **DemoSite**), point to **Deploy**, and then click **Configure Web Deploy Publishing**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. <span data-ttu-id="d2f1a-291">**Web 配置をパブリッシング**の右側に、ダイアログ ボックス、**発行アクセス許可を付与するユーザーを選択**一覧で、省略記号ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-291">In the **Configure Web Deploy Publishing** dialog box, to the right of the **Select a user to give publishing permissions** list, click the ellipsis button.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. <span data-ttu-id="d2f1a-292">**を許可するユーザー**  ダイアログ ボックスを使用して、コンテンツを展開して、をクリックするアカウントのドメインとユーザーの名前を入力**OK**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-292">In the **Allow User** dialog box, type the domain and user name of the account you want to use to deploy content, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. <span data-ttu-id="d2f1a-293">**Web 配置をパブリッシング**ダイアログ ボックスで、をクリックして**セットアップ**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-293">In the **Configure Web Deploy Publishing** dialog box, click **Setup**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > <span data-ttu-id="d2f1a-294">この操作は、1 つの手順で 2 つのキー機能を実行します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-294">This operation performs two key functions in one step.</span></span> <span data-ttu-id="d2f1a-295">最初に、前のセクションで確認する委任の規則に従って、Web 管理サービスを使用してリモートで web サイトを変更するユーザーの権限を付与します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-295">First, it grants the user permission to modify the website remotely through the Web Management Service, according to the delegation rules you examined in the previous section.</span></span> <span data-ttu-id="d2f1a-296">次に、追加、変更、および web サイトのコンテンツに対する権限を設定することができますが、web サイト用のソース フォルダーのユーザーのフル コントロールを付与します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-296">Second, it grants the user full control of the source folder for the website, which allows the user to add, modify, and set permissions on the website content.</span></span>
5. <span data-ttu-id="d2f1a-297">**Web 配置をパブリッシング**ダイアログ ボックスで、をクリックして**閉じる**です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-297">In the **Configure Web Deploy Publishing** dialog box, click **Close**.</span></span>

## <a name="configure-firewall-exceptions"></a><span data-ttu-id="d2f1a-298">ファイアウォールの例外を構成します。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-298">Configure Firewall Exceptions</span></span>

<span data-ttu-id="d2f1a-299">既定では、IIS Web 管理サービスは TCP ポート 8172 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-299">By default, the IIS Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="d2f1a-300">Web サーバーで Windows ファイアウォールが有効な場合は、ルールを作成する新しい受信ポート 8172 で TCP トラフィックを許可する (すべての送信トラフィックは既定では、Windows ファイアウォールで許可されている) 必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-300">If Windows Firewall is enabled on your web server, you'll need to create a new inbound rule to allow TCP traffic on port 8172 (all outbound traffic is permitted by default in Windows Firewall).</span></span> <span data-ttu-id="d2f1a-301">サードパーティ製のファイアウォールを使用する場合は、トラフィックを許可する規則を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-301">If you use a third-party firewall, you'll need to create rules to allow traffic.</span></span>

| <span data-ttu-id="d2f1a-302">Direction</span><span class="sxs-lookup"><span data-stu-id="d2f1a-302">Direction</span></span> | <span data-ttu-id="d2f1a-303">ポートから</span><span class="sxs-lookup"><span data-stu-id="d2f1a-303">From Port</span></span> | <span data-ttu-id="d2f1a-304">ポートに</span><span class="sxs-lookup"><span data-stu-id="d2f1a-304">To Port</span></span> | <span data-ttu-id="d2f1a-305">ポートの種類</span><span class="sxs-lookup"><span data-stu-id="d2f1a-305">Port Type</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="d2f1a-306">受信</span><span class="sxs-lookup"><span data-stu-id="d2f1a-306">Inbound</span></span> | <span data-ttu-id="d2f1a-307">どれでも可</span><span class="sxs-lookup"><span data-stu-id="d2f1a-307">Any</span></span> | <span data-ttu-id="d2f1a-308">8172</span><span class="sxs-lookup"><span data-stu-id="d2f1a-308">8172</span></span> | <span data-ttu-id="d2f1a-309">TCP</span><span class="sxs-lookup"><span data-stu-id="d2f1a-309">TCP</span></span> |
| <span data-ttu-id="d2f1a-310">送信</span><span class="sxs-lookup"><span data-stu-id="d2f1a-310">Outbound</span></span> | <span data-ttu-id="d2f1a-311">8172</span><span class="sxs-lookup"><span data-stu-id="d2f1a-311">8172</span></span> | <span data-ttu-id="d2f1a-312">どれでも可</span><span class="sxs-lookup"><span data-stu-id="d2f1a-312">Any</span></span> | <span data-ttu-id="d2f1a-313">TCP</span><span class="sxs-lookup"><span data-stu-id="d2f1a-313">TCP</span></span> |
  

<span data-ttu-id="d2f1a-314">Windows ファイアウォールのルールの構成の詳細については、次を参照してください。[ファイアウォール規則を構成する](https://technet.microsoft.com/library/dd448559(WS.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-314">For more information on configuring rules in Windows Firewall, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="d2f1a-315">サードパーティ製のファイアウォール製品のマニュアルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-315">For third-party firewalls, please consult your product documentation.</span></span>

## <a name="conclusion"></a><span data-ttu-id="d2f1a-316">まとめ</span><span class="sxs-lookup"><span data-stu-id="d2f1a-316">Conclusion</span></span>

<span data-ttu-id="d2f1a-317">Web サーバーは、Web 管理サービスを使用して、Web 配置ハンドラーへのリモート展開を受け入れる準備ができてができます。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-317">Your web server should now be ready to accept remote deployments to the Web Deploy Handler through the Web Management Service.</span></span> <span data-ttu-id="d2f1a-318">Web アプリケーション サーバーを展開する前に、これらの要点をチェックすることがあります。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-318">Before you attempt to deploy a web application to the server, you may want to check these key points:</span></span>

- <span data-ttu-id="d2f1a-319">IIS でサーバー レベルで基本認証を有効にしますか。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-319">Have you enabled basic authentication at the server level in IIS?</span></span>
- <span data-ttu-id="d2f1a-320">Web 管理サービスへのリモート接続を有効にしますか。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-320">Have you enabled remote connections to the Web Management Service?</span></span>
- <span data-ttu-id="d2f1a-321">Web 管理サービスを開始しましたか。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-321">Have you started the Web Management Service?</span></span>
- <span data-ttu-id="d2f1a-322">ルールは、ある管理サービス委任インプレース?</span><span class="sxs-lookup"><span data-stu-id="d2f1a-322">Are there management service delegation rules in place?</span></span>
- <span data-ttu-id="d2f1a-323">アプリケーション プール id は、web サイトのソース フォルダーに読み取りアクセスがしますか。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-323">Does the application pool identity have read access to the source folder for your website?</span></span>
- <span data-ttu-id="d2f1a-324">管理者以外のユーザー アカウントはサイト レベルのアクセス許可を IIS でですか。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-324">Does the non-administrator user account have site-level permissions in IIS?</span></span>
- <span data-ttu-id="d2f1a-325">ファイアウォールは TCP ポート 8172 でサーバーへの着信接続を許可しますか。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-325">Does your firewall allow incoming connections to the server on TCP port 8172?</span></span>

## <a name="further-reading"></a><span data-ttu-id="d2f1a-326">関連項目</span><span class="sxs-lookup"><span data-stu-id="d2f1a-326">Further Reading</span></span>

<span data-ttu-id="d2f1a-327">Web 展開ハンドラーに web パッケージを配置するカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを構成する方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](configuring-deployment-properties-for-a-target-environment.md)です。</span><span class="sxs-lookup"><span data-stu-id="d2f1a-327">For guidance on how to configure custom Microsoft Build Engine (MSBuild) project files to deploy web packages to the Web Deploy Handler, see [Configuring Deployment Properties for a Target Environment](configuring-deployment-properties-for-a-target-environment.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d2f1a-328">[前へ](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
[次へ](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="d2f1a-328">[Previous](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
[Next](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)</span></span>
