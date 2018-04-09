---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Web 配置のサーバー環境の構成 |Microsoft ドキュメント
author: jrjlee
description: このチュートリアルでは、1 回のクリック、または自動のサポート、web サイトを展開し、さまざまな異なるシナリオで発行するサーバー環境を設定する方法を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: ff6118be618a170ac76d66a9de24a7b5cc2d840a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="configuring-server-environments-for-web-deployment"></a><span data-ttu-id="30613-103">Web 配置のサーバー環境の構成</span><span class="sxs-lookup"><span data-stu-id="30613-103">Configuring Server Environments for Web Deployment</span></span>
====================
<span data-ttu-id="30613-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="30613-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="30613-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="30613-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="30613-106">このチュートリアルでは、サーバー環境に 1 回のクリック、または自動のサポート、web サイトを展開およびさまざまなさまざまなシナリオでの発行を設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="30613-106">This tutorial will show you how to set up server environments to support one-click, or automated, website deployment and publishing in various different scenarios.</span></span> <span data-ttu-id="30613-107">チュートリアルには、展開シナリオ ベースの概要を提供すると共に、Web Farm Framework (WFF) のサーバー ファームを設定して固有のアプローチをサポートするために web サーバーを構成するように、さまざまなタスクの完了を順番に処理するトピックが含まれています。上位レベルのエンド ツー エンドのガイダンスです。</span><span class="sxs-lookup"><span data-stu-id="30613-107">The tutorial includes topics to walk you through completing various tasks, like configuring a web server to support specific approaches to deployment and setting up a Web Farm Framework (WFF) server farm, together with scenario-based overviews that provide higher-level end-to-end guidance.</span></span>
> 
> <span data-ttu-id="30613-108">チュートリアルで説明されている Fabrikam, Inc. の展開シナリオを使用して[Enterprise Web 展開: シナリオの概要](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)例とネットワーク インフラストラクチャの参照ポイントとして。</span><span class="sxs-lookup"><span data-stu-id="30613-108">The tutorial uses the Fabrikam, Inc. deployment scenario described in [Enterprise Web Deployment: Scenario Overview](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) as a reference point for examples and network infrastructure.</span></span>
> 
> <span data-ttu-id="30613-109">これらのチュートリアルのイタリア語の翻訳を参照してください。 [ http://www.lucamorelli.it](http://www.lucamorelli.it)です。</span><span class="sxs-lookup"><span data-stu-id="30613-109">For an Italian translation of these tutorials, visit [http://www.lucamorelli.it](http://www.lucamorelli.it).</span></span>


<span data-ttu-id="30613-110">このチュートリアルには、これらのトピックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="30613-110">This tutorial includes these topics:</span></span>

- [<span data-ttu-id="30613-111">適切な Web 展開手法を選択する</span><span class="sxs-lookup"><span data-stu-id="30613-111">Choosing the Right Approach to Web Deployment</span></span>](choosing-the-right-approach-to-web-deployment.md)
- [<span data-ttu-id="30613-112">シナリオ: Web 展開のテスト環境を構成する</span><span class="sxs-lookup"><span data-stu-id="30613-112">Scenario: Configuring a Test Environment for Web Deployment</span></span>](scenario-configuring-a-test-environment-for-web-deployment.md)
- [<span data-ttu-id="30613-113">シナリオ: Web 展開のステージング環境を構成する</span><span class="sxs-lookup"><span data-stu-id="30613-113">Scenario: Configuring a Staging Environment for Web Deployment</span></span>](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [<span data-ttu-id="30613-114">シナリオ: Web 展開の運用環境を構成する</span><span class="sxs-lookup"><span data-stu-id="30613-114">Scenario: Configuring a Production Environment for Web Deployment</span></span>](scenario-configuring-a-production-environment-for-web-deployment.md)
- [<span data-ttu-id="30613-115">Web 配置発行の Web サーバーを構成する (リモート エージェント)</span><span class="sxs-lookup"><span data-stu-id="30613-115">Configuring a Web Server for Web Deploy Publishing (Remote Agent)</span></span>](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [<span data-ttu-id="30613-116">Web 配置発行の Web サーバーを構成する (Web 配置ハンドラー)</span><span class="sxs-lookup"><span data-stu-id="30613-116">Configuring a Web Server for Web Deploy Publishing (Web Deploy Handler)</span></span>](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [<span data-ttu-id="30613-117">Web 配置発行の Web サーバーを構成する (オフライン展開)</span><span class="sxs-lookup"><span data-stu-id="30613-117">Configuring a Web Server for Web Deploy Publishing (Offline Deployment)</span></span>](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [<span data-ttu-id="30613-118">Web 配置発行のデータベース サーバーを構成する</span><span class="sxs-lookup"><span data-stu-id="30613-118">Configuring a Database Server for Web Deploy Publishing</span></span>](configuring-a-database-server-for-web-deploy-publishing.md)
- [<span data-ttu-id="30613-119">Web Farm Framework でサーバー ファームを作成する</span><span class="sxs-lookup"><span data-stu-id="30613-119">Creating a Server Farm with the Web Farm Framework</span></span>](creating-a-server-farm-with-the-web-farm-framework.md)
- [<span data-ttu-id="30613-120">ターゲット環境の展開プロパティを構成する</span><span class="sxs-lookup"><span data-stu-id="30613-120">Configuring Deployment Properties for a Target Environment</span></span>](configuring-deployment-properties-for-a-target-environment.md)

<span data-ttu-id="30613-121">最初のトピックでは、 [Web 配置を右側の方法を選択する](choosing-the-right-approach-to-web-deployment.md)、(Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツールを使用して web アプリケーションを発行する際の主な方法について説明 2.0。</span><span class="sxs-lookup"><span data-stu-id="30613-121">The first topic, [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md), describes the main approaches you can use to publish web applications by using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0.</span></span> <span data-ttu-id="30613-122">また、それぞれのアプローチにマップされるシナリオを識別します。</span><span class="sxs-lookup"><span data-stu-id="30613-122">It also identifies the scenarios that map to each approach.</span></span> <span data-ttu-id="30613-123">ここは、シナリオの各トピックは、完了する必要があるタスクの概要を示し、これらのタスクを完了するために使用する必要がありますのトピックを識別します。</span><span class="sxs-lookup"><span data-stu-id="30613-123">From here, each scenario topic provides a high-level overview of the tasks you need to complete and identifies the topics you'll need to work through to help you complete these tasks.</span></span>

<span data-ttu-id="30613-124">説明されている分割プロジェクト ファイルのアプローチを使用しているかどうかは[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)をビルドし、最後のトピックでは、ソリューションの配置[ターゲット環境の配置プロパティを構成する](configuring-deployment-properties-for-a-target-environment.md)、別のインストール先の環境に配置するための環境に固有のプロジェクト ファイルを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="30613-124">If you're using the split project file approach described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md) to build and deploy your solution, the final topic, [Configuring Deployment Properties for a Target Environment](configuring-deployment-properties-for-a-target-environment.md), describes how to configure environment-specific project files for deployment to different destination environments.</span></span>

## <a name="key-technologies"></a><span data-ttu-id="30613-125">主要なテクノロジ</span><span class="sxs-lookup"><span data-stu-id="30613-125">Key Technologies</span></span>

<span data-ttu-id="30613-126">このチュートリアルは、web 配置をサポートするためにこれらの製品およびテクノロジを使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="30613-126">This tutorial focuses on how to use these products and technologies to support web deployment:</span></span>

- <span data-ttu-id="30613-127">IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="30613-127">IIS 7.5</span></span>
- <span data-ttu-id="30613-128">Web Deploy 2.x</span><span class="sxs-lookup"><span data-stu-id="30613-128">Web Deploy 2.x</span></span>
- <span data-ttu-id="30613-129">WFF 2.x</span><span class="sxs-lookup"><span data-stu-id="30613-129">WFF 2.x</span></span>
- <span data-ttu-id="30613-130">IIS Web 管理サービス (WMSvc)</span><span class="sxs-lookup"><span data-stu-id="30613-130">IIS Web Management Service (WMSvc)</span></span>

<span data-ttu-id="30613-131">このチュートリアルは、Windows Server 2008 R2、SQL Server 2008 R2、ASP.NET 4.0 では、および ASP.NET MVC 3 の使用についても触れます。</span><span class="sxs-lookup"><span data-stu-id="30613-131">The tutorial also touches on the use of Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4.0, and ASP.NET MVC 3.</span></span>

## <a name="other-tutorials-in-this-series"></a><span data-ttu-id="30613-132">このシリーズの他のチュートリアル</span><span class="sxs-lookup"><span data-stu-id="30613-132">Other Tutorials in This Series</span></span>

<span data-ttu-id="30613-133">これは、エンタープライズ規模の web 配置で 5 つのチュートリアルの一連の一部を形成します。</span><span class="sxs-lookup"><span data-stu-id="30613-133">This forms part of a series of five tutorials on enterprise-scale web deployment.</span></span> <span data-ttu-id="30613-134">系列の他のチュートリアルは、次に示します。</span><span class="sxs-lookup"><span data-stu-id="30613-134">These are the other tutorials in the series:</span></span>

- <span data-ttu-id="30613-135">[エンタープライズのシナリオで Web アプリケーションを展開する](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)です。</span><span class="sxs-lookup"><span data-stu-id="30613-135">[Deploying Web Applications in Enterprise Scenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span></span> <span data-ttu-id="30613-136">この入門的な内容は、一連のチュートリアルのコンテキストの背景を提供します。</span><span class="sxs-lookup"><span data-stu-id="30613-136">This introductory content provides the contextual background for the tutorial series.</span></span> <span data-ttu-id="30613-137">チュートリアルのシナリオについて説明し、どのタスクと、系列全体で説明したチュートリアルに適合する広範なアプリケーション ライフ サイクル管理 (ALM) プロセスを示しています。</span><span class="sxs-lookup"><span data-stu-id="30613-137">It describes the tutorial scenario, and it illustrates how the tasks and walkthroughs described throughout the series fit into a broader Application Lifecycle Management (ALM) process.</span></span>
- <span data-ttu-id="30613-138">[Web、企業内の配置](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)です。</span><span class="sxs-lookup"><span data-stu-id="30613-138">[Web Deployment in the Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).</span></span> <span data-ttu-id="30613-139">このチュートリアルでは、概念的な概要 Microsoft Build Engine (MSBuild) プロジェクト ファイルを Web 発行パイプライン、Web デプロイ、およびその他の関連するテクノロジを提供します。</span><span class="sxs-lookup"><span data-stu-id="30613-139">This tutorial provides a conceptual introduction to Microsoft Build Engine (MSBuild) project files, the Web Publishing Pipeline, Web Deploy, and other related technologies.</span></span> <span data-ttu-id="30613-140">これは、複雑な展開プロセスを管理するこれらのツールを一緒に使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="30613-140">It explains how you can use these tools together to manage complex deployment processes.</span></span>
- <span data-ttu-id="30613-141">[Web 配置を Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="30613-141">[Configuring Team Foundation Server for Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span></span> <span data-ttu-id="30613-142">このチュートリアルでは、特定のビルドの配置を手動でトリガーや継続的インテグレーション (CI) プロセスの一環として自動化された展開を含む、さまざまな展開シナリオをサポートするように、Team Foundation Server (TFS) を構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="30613-142">This tutorial describes how to configure Team Foundation Server (TFS) to support various deployment scenarios, including automated deployment as part of a continuous integration (CI) process and manually triggered deployments of specific builds.</span></span>
- <span data-ttu-id="30613-143">[企業の Web 配置を高度な](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="30613-143">[Advanced Enterprise Web Deployment](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md).</span></span> <span data-ttu-id="30613-144">このチュートリアルは複数の環境のデータベースの配置をカスタマイズする、ファイルとフォルダーの展開から除外および展開プロセス中に web アプリケーションをオフラインにするなどのさまざまな高度な展開タスクを実行する方法を説明します.</span><span class="sxs-lookup"><span data-stu-id="30613-144">This tutorial describes how to accomplish various more advanced deployment tasks, like customizing database deployments for multiple environments, excluding files and folders from deployment, and taking web applications offline during the deployment process.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="30613-145">次へ</span><span class="sxs-lookup"><span data-stu-id="30613-145">Next</span></span>](choosing-the-right-approach-to-web-deployment.md)
