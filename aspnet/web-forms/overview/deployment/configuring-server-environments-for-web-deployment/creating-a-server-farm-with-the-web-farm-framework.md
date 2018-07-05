---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Web Farm Framework でサーバー ファームを作成する |Microsoft Docs
author: jrjlee
description: このトピックでは、Web Farm Framework (WFF) 2.0 を使用して作成し、サーバーのコレクションからの web サーバー ファームを構成する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: ad557306cb4d3c62932c640b598f82d692bc74cf
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388294"
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a><span data-ttu-id="869df-103">Web Farm Framework でサーバー ファームを作成します。</span><span class="sxs-lookup"><span data-stu-id="869df-103">Creating a Server Farm with the Web Farm Framework</span></span>
====================
<span data-ttu-id="869df-104">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="869df-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="869df-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="869df-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="869df-106">このトピックでは、Web Farm Framework (WFF) 2.0 を使用して作成し、サーバーのコレクションからの web サーバー ファームを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="869df-106">This topic describes how to use the Web Farm Framework (WFF) 2.0 to create and configure a web server farm from a collection of servers.</span></span>


<span data-ttu-id="869df-107">WFF では、複数の負荷分散された web サーバー、web platform 製品とコンポーネント、web アプリケーション、web サイト、および構成設定を同期できます。</span><span class="sxs-lookup"><span data-stu-id="869df-107">WFF lets you synchronize web platform products and components, web applications, websites, and configuration settings across multiple load-balanced web servers.</span></span> <span data-ttu-id="869df-108">ステージングと運用環境のように、1 つ以上の web サーバーが必要があるシナリオでこの大幅に、展開と構成プロセスを簡略化することができます。</span><span class="sxs-lookup"><span data-stu-id="869df-108">In scenarios where you need more than one web server, like staging and production environments, this can vastly simplify your deployment and configuration process.</span></span> <span data-ttu-id="869df-109">1 台のサーバーへの web アプリケーションを展開する&#x2014;、*プライマリ サーバー*&#x2014;WFF のサーバー ファームの他のすべての web サーバーでは、その web アプリケーションを自動的にレプリケートするとします。</span><span class="sxs-lookup"><span data-stu-id="869df-109">You can deploy a web application to a single server&#x2014;the *primary server*&#x2014;and WFF will automatically replicate that web application on all the other web servers in the server farm.</span></span>

## <a name="understanding-the-web-farm-framework"></a><span data-ttu-id="869df-110">Web Farm Framework を理解します。</span><span class="sxs-lookup"><span data-stu-id="869df-110">Understanding the Web Farm Framework</span></span>

<span data-ttu-id="869df-111">WFF 2.0 を使用して、プロビジョニング、管理、および web サーバーのグループにコンテンツを配置することができます。</span><span class="sxs-lookup"><span data-stu-id="869df-111">You can use WFF 2.0 to provision, manage, and deploy content to a group of web servers.</span></span> <span data-ttu-id="869df-112">WFF 展開は、3 つの主要なサーバー ロールで構成されます。</span><span class="sxs-lookup"><span data-stu-id="869df-112">A WFF deployment consists of three key server roles:</span></span>

- <span data-ttu-id="869df-113">*コント ローラー サーバー*します。</span><span class="sxs-lookup"><span data-stu-id="869df-113">The *controller server*.</span></span> <span data-ttu-id="869df-114">作成および WFF サーバー ファームを構成するには、このサーバーを使用します。</span><span class="sxs-lookup"><span data-stu-id="869df-114">You use this server to create and configure WFF server farms.</span></span> <span data-ttu-id="869df-115">コント ローラーのサーバーでは、web プラットフォームのコンポーネント、構成設定、およびアプリケーション サーバー ファームの web サーバー間での同期を管理します。</span><span class="sxs-lookup"><span data-stu-id="869df-115">The controller server manages the synchronization of web platform components, configuration settings, and applications between the web servers in a server farm.</span></span> <span data-ttu-id="869df-116">コント ローラー サーバーを WFF 2.0 をインストールして、コント ローラーのサーバーは、サーバー ファーム内のサーバーの各 WFF agent をインストールにされます。</span><span class="sxs-lookup"><span data-stu-id="869df-116">You install WFF 2.0 on the controller server, and the controller server will in turn install the WFF agent on each of the servers in a server farm.</span></span> <span data-ttu-id="869df-117">コント ローラーのサーバーは概念的には、WFF サーバー ファームに属していませんし、単一のコント ローラー サーバーは、複数のサーバー ファームを管理できます。</span><span class="sxs-lookup"><span data-stu-id="869df-117">The controller server does not conceptually belong to any WFF server farm, and a single controller server can manage multiple server farms.</span></span> <span data-ttu-id="869df-118">このシナリオでは、単一 WFF コント ローラー サーバーを使用して作成およびステージングのサーバー ファームと運用環境のサーバー ファームを管理します。</span><span class="sxs-lookup"><span data-stu-id="869df-118">In this scenario, you use a single WFF controller server to create and manage the staging server farm and the production server farm.</span></span>
- <span data-ttu-id="869df-119">*プライマリ サーバー*します。</span><span class="sxs-lookup"><span data-stu-id="869df-119">The *primary server*.</span></span> <span data-ttu-id="869df-120">各 WFF サーバー ファームには、1 つのプライマリ サーバーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="869df-120">Each WFF server farm includes a single primary server.</span></span> <span data-ttu-id="869df-121">Web プラットフォーム コンポーネントをインストールまたはプライマリ サーバーにアプリケーションを展開すると、WFF はサーバー ファームの他のすべてのサーバーへの変更を同期します。</span><span class="sxs-lookup"><span data-stu-id="869df-121">When you install web platform components or deploy applications to the primary server, the WFF synchronizes your changes to all the other servers in the server farm.</span></span>
- <span data-ttu-id="869df-122">*セカンダリ サーバー*します。</span><span class="sxs-lookup"><span data-stu-id="869df-122">The *secondary server*.</span></span> <span data-ttu-id="869df-123">各 WFF サーバー ファームには、1 つまたは複数のセカンダリ サーバーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="869df-123">Each WFF server farm includes one or more secondary servers.</span></span> <span data-ttu-id="869df-124">プライマリ サーバーに行った変更は、サーバー ファーム内のすべてのセカンダリ サーバーにレプリケートされます。</span><span class="sxs-lookup"><span data-stu-id="869df-124">Any changes you make to the primary server are replicated to every secondary server within the server farm.</span></span>

<span data-ttu-id="869df-125">これは、これらのサーバー ロールが、Fabrikam, Inc. のステージングと運用環境に関連付ける方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="869df-125">This shows how these server roles relate to the Fabrikam, Inc. staging and production environments:</span></span>

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

<span data-ttu-id="869df-126">このシナリオで、ステージング環境と運用環境は両方として構成 WFF サーバー ファーム。</span><span class="sxs-lookup"><span data-stu-id="869df-126">In this scenario, the staging environment and the production environment are both configured as WFF server farms.</span></span> <span data-ttu-id="869df-127">1 つの WFF コント ローラー サーバーは、両方のファームを管理します。</span><span class="sxs-lookup"><span data-stu-id="869df-127">A single WFF controller server manages both farms.</span></span> <span data-ttu-id="869df-128">各サーバー ファーム内では、プライマリ サーバーに変更は、すべてのセカンダリ サーバーにレプリケートされます。</span><span class="sxs-lookup"><span data-stu-id="869df-128">Within each server farm, any changes to the primary server are replicated to every secondary server.</span></span>

<span data-ttu-id="869df-129">ステージングと運用環境の構成を開始する前に、こちらの記事について理解を深める WFF 2.0 の主要な概念を確認することお勧めします。</span><span class="sxs-lookup"><span data-stu-id="869df-129">Before you start to configure your staging and production environments, we recommend that you read these articles to familiarize yourself with the key concepts of WFF 2.0:</span></span>

- [<span data-ttu-id="869df-130">Iis 7 Web Farm Framework 2.0 の概要</span><span class="sxs-lookup"><span data-stu-id="869df-130">Overview of the Web Farm Framework 2.0 for IIS 7</span></span>](https://go.microsoft.com/?linkid=9805126)
- [<span data-ttu-id="869df-131">IIS 7 では、Web Farm Framework 2.0 でのサーバー ファームの設定</span><span class="sxs-lookup"><span data-stu-id="869df-131">Setting up a Server Farm with the Web Farm Framework 2.0 for IIS 7</span></span>](https://go.microsoft.com/?linkid=9805127)
- [<span data-ttu-id="869df-132">システムおよび IIS 7 Web Farm Framework 2.0 のプラットフォームの要件</span><span class="sxs-lookup"><span data-stu-id="869df-132">System and Platform Requirements for the Web Farm Framework 2.0 for IIS 7</span></span>](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a><span data-ttu-id="869df-133">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="869df-133">Task Overview</span></span>

<span data-ttu-id="869df-134">タスクとこのトピックの「チュートリアルを完了するには、少なくとも 3 台サーバー必要があります&#x2014;WFF コント ローラーが 1 つ、1 つのプライマリ web サーバー、サーバー ファームのサーバー ファームの 1 つまたは複数セカンダリの web サーバー。</span><span class="sxs-lookup"><span data-stu-id="869df-134">To complete the tasks and walkthroughs in this topic, you'll need at least three servers&#x2014;one WFF controller, one primary web server for the server farm, and one or more secondary web servers for the server farm.</span></span> <span data-ttu-id="869df-135">いつでも WFF サーバー ファームに複数のセカンダリ サーバーを追加できます。</span><span class="sxs-lookup"><span data-stu-id="869df-135">You can add more secondary servers to a WFF server farm at any time.</span></span> <span data-ttu-id="869df-136">作成および構成する必要があります、ステージング環境または運用環境に WFF サーバー ファームの高レベル。</span><span class="sxs-lookup"><span data-stu-id="869df-136">At a high level, to create and configure a WFF server farm for your staging or production environment you'll need to:</span></span>

- <span data-ttu-id="869df-137">インターネット インフォメーション サービス (IIS) 7.5 および WFF 2.0 をインストールすることによって、コント ローラー サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="869df-137">Create a controller server by installing Internet Information Services (IIS) 7.5 and WFF 2.0.</span></span>
- <span data-ttu-id="869df-138">プライマリとセカンダリ サーバーを準備するには、一般的な管理者アカウントを作成して、ファイアウォールの例外を構成します。</span><span class="sxs-lookup"><span data-stu-id="869df-138">Prepare primary and secondary servers by creating a common administrator account and configuring firewall exceptions.</span></span>
- <span data-ttu-id="869df-139">サーバー ファームを構成するには、コント ローラーのサーバーで IIS マネージャーを使用します。</span><span class="sxs-lookup"><span data-stu-id="869df-139">Configure the server farm by using IIS Manager on the controller server.</span></span>
- <span data-ttu-id="869df-140">IIS Application Request Routing (ARR) または別の負荷分散テクノロジを使用して負荷分散を構成します。</span><span class="sxs-lookup"><span data-stu-id="869df-140">Configure load balancing using IIS Application Request Routing (ARR) or an alternative load-balancing technology.</span></span>

<span data-ttu-id="869df-141">タスクとチュートリアルでは、このトピックでは、Windows Server 2008 R2 を実行しているサーバーのクリーン ビルドを開始することを想定しています。</span><span class="sxs-lookup"><span data-stu-id="869df-141">The tasks and walkthroughs in this topic assume that you're starting with clean server builds running Windows Server 2008 R2.</span></span> <span data-ttu-id="869df-142">サーバーごとに、開始する前にいることを確認します。</span><span class="sxs-lookup"><span data-stu-id="869df-142">Before you begin, for each server, ensure that:</span></span>

- <span data-ttu-id="869df-143">Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="869df-143">Windows Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>
- <span data-ttu-id="869df-144">サーバーは、ドメインに参加しています。</span><span class="sxs-lookup"><span data-stu-id="869df-144">The server is domain-joined.</span></span>
- <span data-ttu-id="869df-145">サーバーは、静的 IP アドレスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="869df-145">The server has a static IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="869df-146">コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューターのドメインとログオン](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="869df-146">For more information on joining computers to a domain, see [Joining Computers to the Domain and Logging On](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx).</span></span> <span data-ttu-id="869df-147">静的 IP アドレスの構成の詳細については、次を参照してください。[静的 IP アドレス構成](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="869df-147">For more information on configuring static IP addresses, see [Configure a Static IP Address](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).</span></span>


## <a name="create-the-wff-controller-server"></a><span data-ttu-id="869df-148">WFF コント ローラーのサーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="869df-148">Create the WFF Controller Server</span></span>

<span data-ttu-id="869df-149">WFF コント ローラーのサーバーを作成するには、IIS 7 以降と WFF 2.0 またはそれ以降の両方をインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="869df-149">To create a WFF controller server, you'll need to install both IIS 7 or later and WFF 2.0 or later.</span></span> <span data-ttu-id="869df-150">WFF が IIS の Web 配置ツール (Web 配置) を使用して内部的には、2.x、ファーム内のサーバーを同期します。</span><span class="sxs-lookup"><span data-stu-id="869df-150">Under the covers, WFF uses the IIS Web Deployment Tool (Web Deploy) 2.x to synchronize the servers in your farm.</span></span> <span data-ttu-id="869df-151">WFF をインストールする、Web Platform Installer を使用する場合、インストーラーが自動的にダウンロードされ Web 配置をインストールできます。</span><span class="sxs-lookup"><span data-stu-id="869df-151">If you use the Web Platform Installer to install WFF, the installer will automatically download and install Web Deploy for you.</span></span>

<span data-ttu-id="869df-152">**WFF コント ローラーのサーバーを作成するには**</span><span class="sxs-lookup"><span data-stu-id="869df-152">**To create the WFF controller server**</span></span>

1. <span data-ttu-id="869df-153">ダウンロードしてインストール、 [Web Platform Installer](https://go.microsoft.com/?linkid=9739157)します。</span><span class="sxs-lookup"><span data-stu-id="869df-153">Download and install the [Web Platform Installer](https://go.microsoft.com/?linkid=9739157).</span></span>
2. <span data-ttu-id="869df-154">上部にある、 **Web Platform Installer 3.0**ウィンドウで、をクリックして**製品**します。</span><span class="sxs-lookup"><span data-stu-id="869df-154">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
3. <span data-ttu-id="869df-155">ナビゲーション ウィンドウで、ウィンドウの左側にある クリックして**Server**します。</span><span class="sxs-lookup"><span data-stu-id="869df-155">On the left side of the window, in the navigation pane, click **Server**.</span></span>
4. <span data-ttu-id="869df-156">**IIS 7 推奨構成**行で、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="869df-156">In the **IIS 7 Recommended Configuration** row, click **Add**.</span></span>
5. <span data-ttu-id="869df-157"><strong>Web Farm Framework 2</strong> 。<em>x</em>行で、をクリックして<strong>追加</strong>します。</span><span class="sxs-lookup"><span data-stu-id="869df-157">In the <strong>Web Farm Framework 2.</strong><em>x</em> row, click <strong>Add</strong>.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. <span data-ttu-id="869df-158">**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="869df-158">Click **Install**.</span></span> <span data-ttu-id="869df-159">Web Platform Installer がインストールの一覧に Web 配置ツールのさまざまな他の依存関係と共にを追加することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="869df-159">Notice that the Web Platform Installer has added the Web Deployment Tool, along with various other dependencies, to the installation list.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. <span data-ttu-id="869df-160">ライセンス条項を確認し、条項に同意する場合にクリックします**同意**します。</span><span class="sxs-lookup"><span data-stu-id="869df-160">Review the license terms, and if you consent to the terms, click **I Accept**.</span></span>
8. <span data-ttu-id="869df-161">インストールが完了したら、クリックして**完了**、閉じて、 **Web Platform Installer 3.0**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="869df-161">When the installation is complete, click **Finish**, and then close the **Web Platform Installer 3.0** window.</span></span>

## <a name="configure-the-primary-and-secondary-servers"></a><span data-ttu-id="869df-162">プライマリとセカンダリ サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="869df-162">Configure the Primary and Secondary Servers</span></span>

<span data-ttu-id="869df-163">WFF サーバー ファームを作成する前に、web サーバー ファームを構成するには、いくつかの準備タスクを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="869df-163">Before you create a WFF server farm, you should complete some preparation tasks on the web servers that will make up the farm:</span></span>

- <span data-ttu-id="869df-164">許可するファイアウォール例外を追加、**コア ネットワーク**、**リモート管理**、および**ファイルとプリンターの共有**WFF コント ローラーのサーバーと通信する機能.</span><span class="sxs-lookup"><span data-stu-id="869df-164">Add firewall exceptions to allow the **Core Networking**, **Remote Administration**, and **File and Printer Sharing** features to communicate with the WFF controller server.</span></span>
- <span data-ttu-id="869df-165">ドメイン アカウントを作成する (たとえば、 **FABRIKAM\stagingfarm**) Active Directory でし、各サーバーでローカルの administrators グループに追加します。</span><span class="sxs-lookup"><span data-stu-id="869df-165">Create a domain account (for example, **FABRIKAM\stagingfarm**) in Active Directory and add it to the local administrators group on each server.</span></span> <span data-ttu-id="869df-166">サーバー ファームを作成するときに、サーバー ファームの管理者アカウントとしてこのアカウントを使用します。</span><span class="sxs-lookup"><span data-stu-id="869df-166">You'll use this account as the server farm administrator account when you create the server farm.</span></span>

<span data-ttu-id="869df-167">Windows ファイアウォールでこれらのファイアウォール例外を構成する方法の詳細については、次を参照してください。[システムと IIS 7 Web Farm Framework 2.0 のプラットフォーム要件](https://go.microsoft.com/?linkid=9805128)します。</span><span class="sxs-lookup"><span data-stu-id="869df-167">For more information on how to configure these firewall exceptions in Windows Firewall, see [System and Platform Requirements for the Web Farm Framework 2.0 for IIS 7](https://go.microsoft.com/?linkid=9805128).</span></span> <span data-ttu-id="869df-168">その他のファイアウォール システムは、製品のマニュアルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="869df-168">For other firewall systems, consult your product documentation.</span></span>

<span data-ttu-id="869df-169">次の手順を使用して、Windows Server 2008 R2 でのローカルの administrators グループにドメイン アカウントを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="869df-169">You can use the next procedure to add a domain account to the local administrators group in Windows Server 2008 R2.</span></span> <span data-ttu-id="869df-170">サーバー ファームに追加するすべてのサーバーでこの手順を実行する必要があります&#x2014;つまり、プライマリ サーバーと各セカンダリ サーバーのローカル administrators グループに同じドメイン アカウントを追加します。</span><span class="sxs-lookup"><span data-stu-id="869df-170">You should perform this procedure on every server that you want to add to the server farm&#x2014;in other words, add the same domain account to the local administrators group on the primary server and on each secondary server.</span></span>

<span data-ttu-id="869df-171">**ローカルの administrators グループにドメイン アカウントを追加するには**</span><span class="sxs-lookup"><span data-stu-id="869df-171">**To add a domain account to the local administrators group**</span></span>

1. <span data-ttu-id="869df-172">**開始**メニューで、**管理ツール**、 をクリックし、**サーバー マネージャー**。</span><span class="sxs-lookup"><span data-stu-id="869df-172">On the **Start** menu, point to **Administrative Tools**, and then click **Server Manager**.</span></span>
2. <span data-ttu-id="869df-173">**サーバー マネージャー**ウィンドウのツリー ビュー ウィンドウで [**構成**、展開**ローカル ユーザーとグループ**、] をクリックし、**グループ**.</span><span class="sxs-lookup"><span data-stu-id="869df-173">In the **Server Manager** window, in the tree view pane, expand **Configuration**, expand **Local Users and Groups**, and then click **Groups**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. <span data-ttu-id="869df-174">**グループ**ウィンドウで、ダブルクリックして**管理者**します。</span><span class="sxs-lookup"><span data-stu-id="869df-174">In the **Groups** pane, double-click **Administrators**.</span></span>
4. <span data-ttu-id="869df-175">**Administrators のプロパティ**ダイアログ ボックスで、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="869df-175">In the **Administrators Properties** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="869df-176">**[ユーザー、コンピューター、サービス アカウント、またはグループ**、ドメイン アカウント] ダイアログ ボックス、入力 (または参照) (たとえば、 **FABRIKAM\stagingfarm**)、順にクリックします**OK**.</span><span class="sxs-lookup"><span data-stu-id="869df-176">In the **Select Users, Computers, Service Accounts, or Groups** dialog box, type (or browse) to your domain account (for example, **FABRIKAM\stagingfarm**), and then click **OK**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. <span data-ttu-id="869df-177">**Administrators のプロパティ**ダイアログ ボックスで、をクリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="869df-177">In the **Administrators Properties** dialog box, click **OK**.</span></span>

<span data-ttu-id="869df-178">サーバーは、サーバー ファームに追加する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="869df-178">Your servers are now ready to be added to a server farm.</span></span> <span data-ttu-id="869df-179">プライマリ サーバーの場合は前に、または後、サーバー ファームを作成するには、アプリケーションの要件を満たすためにサーバーを構成することができます&#x2014;どちらの場合で、WFF は、同じ製品、コンポーネント、または構成をデプロイすることで、サーバーが同期されますセカンダリ サーバーです。</span><span class="sxs-lookup"><span data-stu-id="869df-179">In the case of the primary server, you can configure the server to meet your application requirements before or after you create the server farm&#x2014;in both cases, the WFF will synchronize the servers by deploying the same products, components, or configuration to your secondary servers.</span></span> <span data-ttu-id="869df-180">このチュートリアルでは、わかりやすくする、サーバー ファームの作成が完了したら、プライマリ サーバーを構成することが想定しています。</span><span class="sxs-lookup"><span data-stu-id="869df-180">For the sake of simplicity, this tutorial assumes that you'll configure the primary server when you've finished creating the server farm.</span></span>

## <a name="create-the-wff-server-farm"></a><span data-ttu-id="869df-181">WFF サーバー ファームを作成します。</span><span class="sxs-lookup"><span data-stu-id="869df-181">Create the WFF Server Farm</span></span>

<span data-ttu-id="869df-182">この時点では、すべてのサーバーは、WFF サーバー ファームに追加できる状態には。</span><span class="sxs-lookup"><span data-stu-id="869df-182">At this point, all your servers are ready to be added to a WFF server farm:</span></span>

- <span data-ttu-id="869df-183">WFF コント ローラー サーバーにインストールしました。</span><span class="sxs-lookup"><span data-stu-id="869df-183">You've installed WFF on the controller server.</span></span>
- <span data-ttu-id="869df-184">プライマリとセカンダリの web サーバーに対するファイアウォールの例外を構成しました。</span><span class="sxs-lookup"><span data-stu-id="869df-184">You've configured firewall exceptions on your primary and secondary web servers.</span></span>
- <span data-ttu-id="869df-185">プライマリとセカンダリの web サーバーでローカル administrators グループにドメイン アカウントを追加しました。</span><span class="sxs-lookup"><span data-stu-id="869df-185">You've added a domain account to the local administrators group on your primary and secondary web servers.</span></span>

<span data-ttu-id="869df-186">次の手順では、WFF でサーバー ファームを作成します。</span><span class="sxs-lookup"><span data-stu-id="869df-186">The next step is to create the server farm in WFF.</span></span> <span data-ttu-id="869df-187">実行できるこの IIS マネージャーから、WFF コント ローラーのサーバー。</span><span class="sxs-lookup"><span data-stu-id="869df-187">You can do this from IIS Manager on the WFF controller server.</span></span>

<span data-ttu-id="869df-188">**WFF サーバー ファームを作成するには**</span><span class="sxs-lookup"><span data-stu-id="869df-188">**To create a WFF server farm**</span></span>

1. <span data-ttu-id="869df-189">WFF コント ローラーのサーバー上で、**開始**メニューで、**管理ツール**、順にクリックします**インターネット インフォメーション サービス (IIS) マネージャー**します。</span><span class="sxs-lookup"><span data-stu-id="869df-189">On the WFF controller server, on the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
2. <span data-ttu-id="869df-190">**接続**ウィンドウで、ローカル サーバーのノードを展開し、右クリックして**サーバー ファーム**、 をクリックし、**サーバー ファームの作成**です。</span><span class="sxs-lookup"><span data-stu-id="869df-190">In the **Connections** pane, expand the local server node, right-click **Server Farms**, and then click **Create Server Farm**.</span></span>
3. <span data-ttu-id="869df-191">**サーバー ファームの作成**] ダイアログ ボックスに、サーバー ファームのわかりやすい名前を入力 (たとえば、**ステージング ファーム**)、し、[**サーバー ファームをプロビジョニング**します。</span><span class="sxs-lookup"><span data-stu-id="869df-191">In the **Create Server Farm** dialog box, type a meaningful name for the server farm (for example, **Staging Farm**), and then select **Provision server farm**.</span></span>
4. <span data-ttu-id="869df-192">ユーザー名と各サーバーでローカルの administrators グループに追加したドメイン アカウントのパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="869df-192">Type the user name and password of the domain account that you added to the local administrators group on each server.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. <span data-ttu-id="869df-193">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="869df-193">Click **Next**.</span></span>
6. <span data-ttu-id="869df-194">**サーバーの追加**ページの選択のプライマリ サーバーの完全修飾ドメイン名 (FQDN) を入力**プライマリ サーバー**、 をクリックし、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="869df-194">On the **Add Servers** page, type the fully qualified domain name (FQDN) of the primary server, select **Primary Server**, and then click **Add**.</span></span>
7. <span data-ttu-id="869df-195">この時点では、WFF は、指定した資格情報を使用して、プライマリ サーバーに接続を試みます。</span><span class="sxs-lookup"><span data-stu-id="869df-195">At this point, WFF will attempt to contact the primary server using the credentials you provided.</span></span> <span data-ttu-id="869df-196">テーブルにプライマリ サーバーを追加は、接続に成功した場合、**サーバーの追加**ページ。</span><span class="sxs-lookup"><span data-stu-id="869df-196">If the connection succeeds, the primary server will be added to the table on the **Add Servers** page.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="869df-197">お気付きかもしれませんを**サーバーが負荷分散の使用可能な**が既定で選択されています。</span><span class="sxs-lookup"><span data-stu-id="869df-197">You might have noticed that **Server is available for Load Balancing** is selected by default.</span></span> <span data-ttu-id="869df-198">WFF では、IIS ARR モジュールを使用して、負荷分散を実装し、それによって、サーバー ファームで web サーバー間で要求を分散します。</span><span class="sxs-lookup"><span data-stu-id="869df-198">WFF uses the IIS ARR module to implement load balancing and thereby distribute requests across the web servers in your server farm.</span></span> <span data-ttu-id="869df-199">ほとんどのシナリオではのみをオフにする、**サーバーが負荷分散の使用可能な**サードパーティの負荷分散ソリューションを代わりに使用する場合はオプションです。</span><span class="sxs-lookup"><span data-stu-id="869df-199">In most scenarios, you'd only clear the **Server is available for Load Balancing** option if you wanted to use a third-party load balancing solution instead.</span></span>
8. <span data-ttu-id="869df-200">**サーバーの追加** ページで、最初のセカンダリ サーバーの FQDN を入力してをクリックし、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="869df-200">On the **Add Servers** page, type the FQDN of your first secondary server, and then click **Add**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. <span data-ttu-id="869df-201">クリックして、ファームで追加のセカンダリ サーバーに対して手順 7**完了**します。</span><span class="sxs-lookup"><span data-stu-id="869df-201">Repeat step 7 for any additional secondary servers in your farm, and then click **Finish**.</span></span>

<span data-ttu-id="869df-202">WFF サーバー ファームが起動し、実行します。</span><span class="sxs-lookup"><span data-stu-id="869df-202">Your WFF server farm is now up and running.</span></span> <span data-ttu-id="869df-203">Web プラットフォームの製品や、プライマリ サーバーと web アプリケーションまたはプライマリ サーバーに展開するコンテンツをインストールするコンポーネントは、すべてのセカンダリ サーバーで自動的にプロビジョニングされます。</span><span class="sxs-lookup"><span data-stu-id="869df-203">Any web platform products or components that you install on the primary server, and any web applications or content that you deploy to the primary server, will be automatically provisioned on all your secondary servers.</span></span>

<span data-ttu-id="869df-204">WFF は広範で複雑なトピックとで詳細情報を入手することができます、 [IIS 7 用の Microsoft Web Farm Framework 2.0](https://go.microsoft.com/?linkid=9805129) web サイト。</span><span class="sxs-lookup"><span data-stu-id="869df-204">WFF is a broad and complex topic, and you can learn more about it on the [Microsoft Web Farm Framework 2.0 for IIS 7](https://go.microsoft.com/?linkid=9805129) website.</span></span> <span data-ttu-id="869df-205">しばらくの間、ただしが意識する必要がある 2 つの機能領域です。</span><span class="sxs-lookup"><span data-stu-id="869df-205">For the time being, however, there are two features areas that you need to be aware of:</span></span>

- <span data-ttu-id="869df-206">*アプリケーションのプロビジョニング*はサーバー ファーム内のすべてのセカンダリ サーバー間で web アプリケーションと構成の設定のように、プライマリ サーバーからコンテンツを複製するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="869df-206">*Application provisioning* is the process that replicates content from the primary server, like web applications and configuration settings, across all the secondary servers in the server farm.</span></span> <span data-ttu-id="869df-207">たとえば、Contact Manager サンプル ソリューションをプライマリのステージング サーバーに配置する場合 WFF アプリケーションのプロビジョニング プロセスがこのソリューションをすべてのセカンダリ ステージング サーバーにデプロイするされます。</span><span class="sxs-lookup"><span data-stu-id="869df-207">For example, if you deploy the Contact Manager sample solution to your primary staging server, the WFF application provisioning process will deploy this solution to all your secondary staging servers.</span></span> <span data-ttu-id="869df-208">既定では、アプリケーションのプロビジョニング プロセスには、30 秒ごとが実行されます。</span><span class="sxs-lookup"><span data-stu-id="869df-208">By default, the application provisioning process runs every 30 seconds.</span></span>
- <span data-ttu-id="869df-209">*プラットフォームのプロビジョニング*は web platform 製品と、プライマリ サーバーからサーバー ファーム内のすべてのセカンダリ サーバーに、コンポーネントを同期するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="869df-209">*Platform provisioning* is the process that synchronizes web platform products and components from the primary server to all the secondary servers in the server farm.</span></span> <span data-ttu-id="869df-210">たとえば、プライマリ、ステージング サーバーに ASP.NET MVC 3 をインストールする場合、プラットフォームのプロビジョニング プロセスは、Web Platform Installer すべて、セカンダリ ステージング サーバー上で ASP.NET MVC 3 をインストールする使用します。</span><span class="sxs-lookup"><span data-stu-id="869df-210">For example, if you install ASP.NET MVC 3 on your primary staging server, the platform provisioning process will use the Web Platform Installer to install ASP.NET MVC 3 on all your secondary staging servers.</span></span> <span data-ttu-id="869df-211">既定では、プラットフォームのプロビジョニング プロセスには、5 分が実行されます。</span><span class="sxs-lookup"><span data-stu-id="869df-211">By default, the platform provisioning process runs every five minutes.</span></span>

<span data-ttu-id="869df-212">管理できます基本的なアプリケーションとプラットフォームのプロビジョニングの設定から IIS マネージャー、WFF コント ローラーのサーバー上。</span><span class="sxs-lookup"><span data-stu-id="869df-212">You can manage basic application and platform provisioning settings from IIS Manager on your WFF controller server.</span></span>

<span data-ttu-id="869df-213">**アプリケーションとプラットフォームのプロビジョニングの設定を詳細します。**</span><span class="sxs-lookup"><span data-stu-id="869df-213">**Explore application and platform provisioning settings**</span></span>

1. <span data-ttu-id="869df-214">IIS マネージャーで、**接続**ウィンドウで、サーバー ファームを選択します。</span><span class="sxs-lookup"><span data-stu-id="869df-214">In IIS Manager, in the **Connections** pane, select your server farm.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. <span data-ttu-id="869df-215">**サーバー ファーム**ウィンドウで、ダブルクリックして**アプリケーションのプロビジョニング**します。</span><span class="sxs-lookup"><span data-stu-id="869df-215">In the **Server Farm** pane, double-click **Application Provisioning**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. <span data-ttu-id="869df-216">ご覧のように、サーバー ファームは 30 秒ごとに、プライマリ サーバーとセカンダリ サーバーの間の web コンテンツと構成設定を同期する構成されています。</span><span class="sxs-lookup"><span data-stu-id="869df-216">As you can see, the server farm is currently configured to synchronize web content and configuration settings between the primary server and the secondary servers every 30 seconds.</span></span>
4. <span data-ttu-id="869df-217">をクリックして**戻る**、し、ダブルクリック**プラットフォームのプロビジョニング**します。</span><span class="sxs-lookup"><span data-stu-id="869df-217">Click **Back**, and then double-click **Platform Provisioning**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. <span data-ttu-id="869df-218">ご覧のように、サーバー ファームは 5 分ごとに web プラットフォーム製品とコンポーネント間、プライマリ サーバーとセカンダリ サーバーを同期する構成されています。</span><span class="sxs-lookup"><span data-stu-id="869df-218">As you can see, the server farm is currently configured to synchronize web platform products and components between the primary server and the secondary servers every five minutes.</span></span>
6. <span data-ttu-id="869df-219">クリックして**戻る**します。</span><span class="sxs-lookup"><span data-stu-id="869df-219">Click **Back**.</span></span>
7. <span data-ttu-id="869df-220">Web プラットフォーム製品をすぐに同期するサーバー ファームを強制的にで、**アクション**ウィンドウで、をクリックして**プラットフォームのプロビジョニング**します。</span><span class="sxs-lookup"><span data-stu-id="869df-220">To force the server farm to synchronize web platform products immediately, in the **Actions** pane, click **Provision Platform**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > <span data-ttu-id="869df-221">プラットフォームのプロビジョニングと、時間がかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="869df-221">Platform provisioning may take some time.</span></span> <span data-ttu-id="869df-222">インストーラー プロセスは、サーバー ファーム内のセカンダリ サーバーでは、バック グラウンドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="869df-222">The installer process runs in the background on the secondary servers in your server farm.</span></span>
8. <span data-ttu-id="869df-223">プロビジョニング プロセスが完了するための十分な時間を許可するよう、製品と、プライマリ サーバーに追加したコンポーネントが、セカンダリ サーバーでレプリケートされて今すぐいるを確認できます。</span><span class="sxs-lookup"><span data-stu-id="869df-223">Once you've allowed sufficient time for the provisioning process to complete, you can verify that the products and components that you added to the primary server have now been replicated on the secondary servers.</span></span> <span data-ttu-id="869df-224">たとえば、セカンダリ サーバーにログオンしを使用して、**サーバー マネージャー**ウィンドウを web サーバーの役割がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="869df-224">For example, you can log on to a secondary server and use the **Server Manager** window to verify that the web server role has been installed.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. <span data-ttu-id="869df-225">さまざまな web プラットフォームのコンポーネントが追加されていることを確認してインストールされているプログラムの一覧を確認することもできます。</span><span class="sxs-lookup"><span data-stu-id="869df-225">You can also check the installed programs list to verify that various web platform components have been added.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a><span data-ttu-id="869df-226">負荷分散を構成します。</span><span class="sxs-lookup"><span data-stu-id="869df-226">Configure Load Balancing</span></span>

<span data-ttu-id="869df-227">Web ファームを作成するときに、web サーバー間での HTTP 要求を分散する負荷分散の何らかの形式を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="869df-227">When you create a web farm, you need to set up some form of load balancing to distribute HTTP requests between your web servers.</span></span> <span data-ttu-id="869df-228">これは、Windows Server 2008 のネットワーク負荷分散、IIS ARR、またはサード パーティ製ソフトウェア ベースまたはハードウェア ベースの負荷が分散ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="869df-228">This could be Windows Server 2008 network load balancing, IIS ARR, or a third-party software-based or hardware-based load balancing solution.</span></span>

<span data-ttu-id="869df-229">IIS の先端と密接に統合する WFF は設計されています</span><span class="sxs-lookup"><span data-stu-id="869df-229">WFF is designed to integrate closely with IIS ARR.</span></span> <span data-ttu-id="869df-230">この統合を利用するには、WFF コント ローラー サーバーに ARR モジュールをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="869df-230">To take advantage of this integration, you need to install the ARR module on the WFF controller server.</span></span> <span data-ttu-id="869df-231">すべてのトラフィック、web、コント ローラー サーバーに通常ドメイン ネーム システム (DNS) レコードを構成することで。</span><span class="sxs-lookup"><span data-stu-id="869df-231">You then direct all your web traffic to the controller server, typically by configuring Domain Name System (DNS) records.</span></span> <span data-ttu-id="869df-232">コント ローラーのサーバーは、サーバーの可用性とさまざまなその他の条件に基づいて、ファーム内のサーバー間で着信要求を分散します。</span><span class="sxs-lookup"><span data-stu-id="869df-232">The controller server will then distribute incoming requests among the servers in your farm, based on server availability and various other criteria.</span></span>

> [!NOTE]
> <span data-ttu-id="869df-233">WFF; で、ARR を使用する必要はありません。サード パーティの負荷分散ソリューションを使用する WFF を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="869df-233">You don't have to use ARR with WFF; you can configure WFF to work with third-party load balancing solutions.</span></span> <span data-ttu-id="869df-234">詳細については、次を参照してください。 [IIS 7 Web Farm Framework 2.0 の概要](https://go.microsoft.com/?linkid=9805126)します。</span><span class="sxs-lookup"><span data-stu-id="869df-234">For more information, see [Overview of the Web Farm Framework 2.0 for IIS 7](https://go.microsoft.com/?linkid=9805126).</span></span>


<span data-ttu-id="869df-235">このチュートリアルの範囲を超えてはこれの最も複雑なトピックでは、ARR を使用して負荷分散します。</span><span class="sxs-lookup"><span data-stu-id="869df-235">Load balancing using ARR is a complex topic, most of which is beyond the scope of this tutorial.</span></span> <span data-ttu-id="869df-236">ただし、ARR モジュールをインストールし、負荷分散の概要を次の手順を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="869df-236">However, you can use the next procedure to install the ARR module and get started with load balancing.</span></span>

<span data-ttu-id="869df-237">**WFF コント ローラーのサーバーでの負荷分散を設定するには**</span><span class="sxs-lookup"><span data-stu-id="869df-237">**To set up load balancing on the WFF controller server**</span></span>

1. <span data-ttu-id="869df-238">WFF コント ローラーのサーバーでは、Web Platform Installer を起動します。</span><span class="sxs-lookup"><span data-stu-id="869df-238">On the WFF controller server, launch the Web Platform Installer.</span></span>
2. <span data-ttu-id="869df-239">上部にある、 **Web Platform Installer 3.0**ウィンドウで、をクリックして**製品**します。</span><span class="sxs-lookup"><span data-stu-id="869df-239">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
3. <span data-ttu-id="869df-240">ナビゲーション ウィンドウで、ウィンドウの左側にある クリックして**Server**します。</span><span class="sxs-lookup"><span data-stu-id="869df-240">On the left side of the window, in the navigation pane, click **Server**.</span></span>
4. <span data-ttu-id="869df-241">**アプリケーション要求ルーティング 2.5**行で、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="869df-241">In the **Application Request Routing 2.5** row, click **Add**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. <span data-ttu-id="869df-242">クリックして**インストール**、しの指示に従って、 **Web Platform のインストール**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="869df-242">Click **Install**, and then follow the instructions in the **Web Platform Installation** window.</span></span>
6. <span data-ttu-id="869df-243">インストールが完了したら、IIS マネージャーを起動し、**接続**ウィンドウで、サーバー ファームのノードをクリックします。</span><span class="sxs-lookup"><span data-stu-id="869df-243">When the installation is complete, launch IIS Manager, and in the **Connections** pane, click your server farm node.</span></span> <span data-ttu-id="869df-244">追加されたいくつかの新しいアイコンに注意してください、**サーバー ファーム**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="869df-244">Notice that several new icons have been added to the **Server Farm** pane.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. <span data-ttu-id="869df-245">**サーバー ファーム**ウィンドウで、ダブルクリックして**負荷分散**します。</span><span class="sxs-lookup"><span data-stu-id="869df-245">In the **Server Farm** pane, double-click **Load Balance**.</span></span>
8. <span data-ttu-id="869df-246">**負荷分散**アルゴリズムのバランスを取るウィンドウで、ロードを選択します (たとえば、**少なくとも現在の要求**)。</span><span class="sxs-lookup"><span data-stu-id="869df-246">In the **Load Balance** pane, select a load balance algorithm (for example, **Least current request**).</span></span>

    > [!NOTE]
    > <span data-ttu-id="869df-247">負荷分散アルゴリズムとその他の構成設定の詳細については、次を参照してください。[アプリケーション要求ルーティング モジュール](https://go.microsoft.com/?linkid=9805130)します。</span><span class="sxs-lookup"><span data-stu-id="869df-247">For more information on load balancing algorithms and other configuration settings, see [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. <span data-ttu-id="869df-248">**アクション**ウィンドウで、をクリックして**適用**します。</span><span class="sxs-lookup"><span data-stu-id="869df-248">In the **Actions** pane, click **Apply**.</span></span>

<span data-ttu-id="869df-249">基本的な負荷分散、サーバー ファーム内のサーバーが構成されました。</span><span class="sxs-lookup"><span data-stu-id="869df-249">You have now configured basic load balancing for the servers in your server farm.</span></span> <span data-ttu-id="869df-250">コント ローラー サーバーにすべての web ファーム トラフィックを転送する場合、要求は可用性に応じて、ファーム内のサーバーと選択した負荷分散アルゴリズムの間で配布されます。</span><span class="sxs-lookup"><span data-stu-id="869df-250">If you direct all your web farm traffic to the controller server, the requests will be distributed between the servers in your farm according to availability and the load balancing algorithm you selected.</span></span>

<span data-ttu-id="869df-251">ARR で負荷分散を構成する方法の詳細については、次を参照してください。[アプリケーション要求ルーティング モジュール](https://go.microsoft.com/?linkid=9805130)します。</span><span class="sxs-lookup"><span data-stu-id="869df-251">For more information on how to configure load balancing with ARR, see [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).</span></span>

## <a name="monitor-the-server-farm"></a><span data-ttu-id="869df-252">サーバー ファームを監視します。</span><span class="sxs-lookup"><span data-stu-id="869df-252">Monitor the Server Farm</span></span>

<span data-ttu-id="869df-253">コント ローラーのサーバーで IIS マネージャーで、いつでも、サーバー ファームのヘルスを監視できます。</span><span class="sxs-lookup"><span data-stu-id="869df-253">You can monitor the health of your server farm at any time through IIS Manager on the controller server.</span></span> <span data-ttu-id="869df-254">**接続**ウィンドウでは、サーバー ファームを展開して、クリックして**サーバー**します。</span><span class="sxs-lookup"><span data-stu-id="869df-254">In the **Connections** pane, expand your server farm, and then click **Servers**.</span></span> <span data-ttu-id="869df-255">中央のウィンドウは、最近のアクティビティのトレース ログと共に、ファーム内の各サーバーの概要を紹介します。</span><span class="sxs-lookup"><span data-stu-id="869df-255">The center pane will show a summary of each server in the farm together with a trace log of recent activity.</span></span>

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a><span data-ttu-id="869df-256">まとめ</span><span class="sxs-lookup"><span data-stu-id="869df-256">Conclusion</span></span>

<span data-ttu-id="869df-257">WFF サーバー ファームは稼働しているのようになります。</span><span class="sxs-lookup"><span data-stu-id="869df-257">Your WFF server farm should now be up and running.</span></span> <span data-ttu-id="869df-258">必要に応じてどの展開方法をサポートするために、プライマリ サーバーを構成する&#x2014;詳細については「関連項目」セクションを参照してください。&#x2014;し、構成は、サーバー ファーム内の各セカンダリ サーバーにレプリケートされます。</span><span class="sxs-lookup"><span data-stu-id="869df-258">You can configure the primary server to support whichever deployment approach you prefer&#x2014;see the Further Reading section for details&#x2014;and your configuration will be replicated on each secondary server in the server farm.</span></span>

## <a name="further-reading"></a><span data-ttu-id="869df-259">関連項目</span><span class="sxs-lookup"><span data-stu-id="869df-259">Further Reading</span></span>

<span data-ttu-id="869df-260">構成と使用、WFF のすべての側面の詳細については、次を参照してください。、 [IIS 7 用の Microsoft Web Farm Framework 2.0](https://go.microsoft.com/?linkid=9805129) web サイト。</span><span class="sxs-lookup"><span data-stu-id="869df-260">For more guidance on all aspects of configuring and using the WFF, see the [Microsoft Web Farm Framework 2.0 for IIS 7](https://go.microsoft.com/?linkid=9805129) website.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="869df-261">[前へ](configuring-a-database-server-for-web-deploy-publishing.md)
> [次へ](configuring-deployment-properties-for-a-target-environment.md)</span><span class="sxs-lookup"><span data-stu-id="869df-261">[Previous](configuring-a-database-server-for-web-deploy-publishing.md)
[Next](configuring-deployment-properties-for-a-target-environment.md)</span></span>
